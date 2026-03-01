
This is a 2019 KDD paper from Google describing the engineering and machine learning system behind Gmail's Smart Compose feature. Let me walk through every section with full technical depth.

---

## 1. Abstract & High-Level Framing

The paper opens by describing Smart Compose as a **real-time suggestion system** embedded inside Gmail's composition interface. The key phrase is _"reducing repetitive typing"_ — the system is not trying to write emails for you, but to complete phrases you're already writing, based on context.

The central claim is that this system:

1. Uses a **large-scale neural language model** at its core
2. Required novel **serving infrastructure** to meet real-time latency constraints
3. Saves users **over one billion characters of typing per week**

This last figure is important: it grounds the system in real-world impact and justifies the enormous engineering investment.

---

## 2. Introduction & Motivation

### Background: Smart Reply vs. Smart Compose

The paper positions Smart Compose against its predecessor, **Smart Reply** (Kannan et al., 2016). Smart Reply automatically generates short response suggestions (like "Sounds good!" or "Thanks!") to incoming emails. It uses a **sequence-to-sequence model** — an encoder reads the incoming message, a decoder outputs a fixed reply from a curated whitelist.

Smart Compose is fundamentally different:

- Smart Reply fires **once per incoming email** (single inference event)
- Smart Compose fires **on nearly every keystroke** while composing
- Smart Reply's outputs are constrained to a **small whitelist** of pre-approved phrases
- Smart Compose outputs are **open-ended** and context-sensitive — the model must generate novel continuations dynamically

### The Five Core Challenges

The introduction enumerates five challenges, each of which maps to a later section of the paper. Understanding these upfront is key to understanding every design decision made later.

---

#### Challenge 1: Latency

> _"The system requires the 90th percentile latency to be under 60ms."_

This is extremely tight. The footnote cites Nielsen (1993), a classic HCI text, which established that **100ms is the threshold at which a system feels instantaneous to a human**. Since network overhead, application logic, and other factors consume some of that budget, the model inference itself must complete well under 100ms.

The **90th percentile** (P90) framing is important: this isn't average latency. This means that 90% of all requests must complete within 60ms. The remaining 10% can be slower — but targeting the P90 ensures the system feels responsive to the vast majority of users under varying load conditions.

The challenge is compounded by the nature of neural language models. Unlike traditional n-gram models (which are essentially lookup tables in finite-state automata), neural language models are **stateful** — they carry hidden states through recurrent layers or attention matrices that must be computed sequentially. This makes them hard to parallelize across the _time dimension_ of a single inference call.

---

#### Challenge 2: Scale

> _"Gmail is used by more than 1.5 billion diverse users."_

This is a serving challenge: the system must handle enormous query volume while maintaining quality. But it's also a modeling challenge — a model trained on data from 1.5 billion users must generalize across vastly different writing styles, topics, and vocabularies. A model too small won't capture this diversity; a model too large won't fit within latency and cost budgets.

---

#### Challenge 3: Personalization

Each user has a unique writing style — recurring phrases, preferred greetings, domain-specific vocabulary. A single global model, no matter how large, cannot optimally capture every individual's idiosyncrasies. The system needs a mechanism to **adapt to individual users** without requiring a separate large neural model per person (which would be computationally and storage-wise infeasible at 1.5 billion users).

---

#### Challenge 4: Fairness and Privacy

Two distinct sub-problems:

**Privacy**: The model is trained on billions of private emails. There's a risk of **memorization** — a neural model trained on sensitive text might reproduce it verbatim in suggestions, leaking one user's private content to another. The paper cites Carlini et al. (2018), which demonstrated that neural networks _can_ memorize and reproduce training data under certain conditions.

**Fairness/Bias**: Language models learn statistical associations from human-generated text, which contains human biases. The paper gives a concrete example: typing "I am meeting an investor next week" triggered the suggestion "Did you want to meet **him**" while "I am meeting a nurse next week" triggered "Did you want to meet **her**." This is a **gender-occupation bias** embedded in the training data's statistical correlations.

Notably, the development team built this system without anyone being able to **look at the underlying training data** — a strong privacy constraint that itself shapes the engineering process.

---

#### Challenge 5: Metrics Design

Online A/B testing (showing the model to real users and measuring behavior) is the gold standard for evaluation, but it's:

- **Expensive**: requires diverting real user traffic
- **Slow**: meaningful results take days or weeks
- **Risky**: a bad model hurts real user experience

The team therefore needed **offline metrics** that could be computed quickly on held-out data and that correlate strongly with online metrics. Designing such metrics — and verifying that correlation — is itself a research contribution of the paper.

---

## 3. Finding the Right Model (Section 3)

### 3.1 Data

#### Training Corpus

> _"After the pre-processing steps, there are about 8 billion English messages in our data set."_

8 billion emails is an extraordinary corpus. The 80/20 train/test split gives ~6.4 billion training examples and ~1.6 billion test examples.

#### Contextual Features Used as Inputs

The model doesn't just see the text the user is currently typing. It also receives:

1. **Previous email body** — if this is a reply, the original message the user received
2. **Email subject line**
3. **Date and time** — used as discrete (categorical) features, not continuous values
    - These enable time-aware suggestions: "Good morning" in the morning, "Happy Thanksgiving" in November
4. **Locale** — the user's language/regional setting (e.g., en-US vs. en-GB)
    - This captures spelling variants: "behavior" (American) vs. "behaviour" (British)

#### Pre-processing Pipeline

The paper describes five pre-processing steps, each with a clear rationale:

**Language detection**: Emails outside the target language are discarded. For the English model, this ensures the model trains only on English text and isn't confused by code-switching or multilingual emails.

**Segmentation/Tokenization**: The raw email text is split into sentences, and sentences are split into words and punctuation marks. This defines the fundamental unit of the model's vocabulary.

**Normalization**: Rare words and personal entities — names, URLs, email addresses, phone numbers — are replaced with **special tokens** (e.g., `<NAME>`, `<URL>`). This serves two purposes:

- **Privacy**: Personal names and contact info don't appear in the model's vocabulary and can't be surfaced to other users
- **Generalization**: Rare entities that appear in training data won't be memorized; the model learns structural patterns instead

**Quotation removal**: Quoted text from replied-to emails (the ">" prefixed lines in an email thread) is removed. This prevents the model from being confused about what the user is actively composing versus what was previously written.

**Salutation/close removal**: Opening lines like "Hi John," and closing lines like "Best regards, Mary" are removed. The paper doesn't explain this explicitly, but the likely reason is that these phrases follow such rigid templates that training on them would skew the model toward always suggesting salutations, and the personalization module handles them better anyway.

#### Vocabulary

> _"Our vocabulary contains the most frequent 50k English words."_

A vocabulary of 50,000 words covers the vast majority of common English usage. The **word-level** tokenization (as opposed to character-level or subword-level) means each token is a complete word. Out-of-vocabulary (OOV) words are handled by a special `<UNK>` token. The multilingual model (Section 6) uses a different approach — **WordPiece** subword tokenization — precisely because rare words in non-English languages would be OOV much more frequently.

---

### 3.2 Model Architecture

The fundamental task is stated precisely:

> _"Predict a sequence of tokens of variable length, conditioned on the prefix token sequence typed by a user and additional contextual information."_

This is **conditional language modeling**: given some prefix (what the user has typed so far) plus context (subject, previous email, date, locale), predict the most likely continuation.

The training objective is:

> _"Maximize the log probability of producing the correct target sequence given the input for all data samples in the training corpus."_

Formally, if we denote the target sequence as tokens $w_1, w_2, \ldots, w_T$, and the context as $C$, the objective is:

$$\mathcal{L} = \sum_{t=1}^{T} \log P(w_t \mid w_1, \ldots, w_{t-1}, C)$$

This is standard **maximum likelihood estimation** (MLE) over a language model. Each term $P(w_t \mid w_1, \ldots, w_{t-1}, C)$ is the model's predicted probability of the next word given all prior words and the context.

Three architectural approaches are compared:

---

#### Architecture A: Language Model A (LM-A)

**Structure**: A standard language model (LSTM or Transformer) that operates over the current email body prefix. Context is encoded separately and injected at every time step.

**Context encoding**: For each context field (subject, previous email), the model computes the **average of all token embeddings** in that field. So if the subject is "Meeting on Thursday," the subject encoding is:

$$\mathbf{c}_{subject} = \frac{1}{N} \sum_{i=1}^{N} \mathbf{e}_i$$

where $\mathbf{e}_i$ is the embedding vector for the $i$-th token in the subject and $N$ is the number of tokens. This averaged embedding is then **concatenated** with the current token's embedding at each time step before being fed to the language model.

For the LSTM models: token embedding size is 256, and by concatenating the averaged subject embedding and averaged previous email embedding (each 256-dimensional), the input to the LSTM becomes **768-dimensional**.

**Strengths**: Simple, efficient. The context is encoded once (not re-encoded at every step), keeping inference fast.

**Weaknesses**: Averaging all token embeddings is a crude approximation. It loses word order and inter-word relationships in the context. A subject like "Don't cancel the meeting" and "Cancel the meeting" would produce similar average embeddings even though their meanings differ.

---

#### Architecture B: Language Model B (LM-B)

**Structure**: The subject, previous email body, and current email body are all concatenated into a **single flat sequence**, separated by special delimiter tokens (e.g., `[SEP]`). This entire concatenated sequence is fed into a single language model.

**Strengths**: No separate context encoder needed. The model can learn complex relationships between the subject, the previous email, and the current prefix through its attention or recurrence mechanisms. Simpler architecture.

**Weaknesses**: Much longer sequences. If the subject is 10 tokens, the previous email is 200 tokens, and the current prefix is 50 tokens, the total sequence is 260 tokens instead of 50. This increases computational cost quadratically for Transformers (because self-attention is $O(n^2)$ in sequence length) and linearly for LSTMs.

---

#### Architecture C: Sequence-to-Sequence (Seq2Seq)

**Structure**: A classic encoder-decoder architecture (like those used in neural machine translation). The **encoder** reads the concatenated subject and previous email body. The **decoder** generates the current email body, attending to the encoder's outputs at each step via an **attention mechanism**.

**The attention mechanism** is the key innovation here. Rather than compressing context into a single fixed vector (like LM-A's average embedding), attention allows the decoder, at each generation step, to **selectively focus** on different parts of the encoded context. At decoding step $t$, the attention weight $\alpha_{ti}$ represents how much the decoder should "look at" encoder position $i$:

$$\alpha_{ti} = \frac{\exp(score(\mathbf{h}_t^{dec}, \mathbf{h}_i^{enc}))}{\sum_j \exp(score(\mathbf{h}_t^{dec}, \mathbf{h}_j^{enc}))}$$

The context vector used in generation is then:

$$\mathbf{c}_t = \sum_i \alpha_{ti} \mathbf{h}_i^{enc}$$

**Strengths**: Theoretically superior context utilization. The model can dynamically focus on the most relevant parts of the subject or previous email at each generation step.

**Weaknesses**: More parameters, longer training time, and — crucially — **slower inference**, because the decoder must attend over the entire encoded context at each step.

---

### 3.3 Triggering (Beam Search at Inference)

#### Beam Search Algorithm

At inference time, the model must generate a suggestion given the user's current prefix. A naive approach would be **greedy decoding** — always pick the single most likely next token. This is fast but produces suboptimal suggestions (a locally optimal choice at step $t$ might preclude a globally better sequence).

**Beam search** keeps track of $m$ best candidate sequences simultaneously:

1. Initialize the heap with a single empty candidate
2. At each step:
    - For each of the $m$ candidates in the heap, compute the probability distribution over the vocabulary (50,000 words)
    - Select the top $k$ most probable next tokens for each candidate
    - Add these $k \times m$ extensions to the heap
    - Prune the heap back to $m$ candidates (keeping only the highest-scoring ones)
3. A candidate is **complete** when it generates a sentence-ending punctuation mark, an `<EOS>` (end-of-sequence) token, or reaches a predefined maximum length
4. Beam search ends when no new complete candidates are added

#### Confidence Score

Not every prefix should trigger a suggestion. If the model is uncertain, showing a bad suggestion is worse than showing nothing — it erodes user trust.

The paper uses a **length-normalized log conditional probability** as the confidence score:

$$\text{score}(w_1, \ldots, w_T) = \frac{1}{T} \sum_{t=1}^{T} \log P(w_t \mid w_1, \ldots, w_{t-1}, C)$$

Breaking this down:

- $\log P(w_t \mid \ldots)$ is the log-probability the model assigns to the $t$-th token. Higher means more confident.
- Summing these gives the total log-probability of the sequence.
- Dividing by $T$ (length normalization) prevents the model from systematically preferring short suggestions (which naturally have higher raw log-probabilities since there are fewer terms to multiply).

A **triggering threshold** is set based on a target **coverage rate** — the fraction of keystrokes for which the system shows a suggestion. This is a hyperparameter tuned to balance suggestion quality and frequency.

---

### 3.4 Evaluation Metrics

#### Metric 1: Log Perplexity

The paper gives the formula:

$$\text{Log Perplexity}(x) = -\sum_x p(x) \log p(x)$$

Let me unpack this carefully.

**Perplexity** is the standard metric for language model evaluation. Intuitively, it measures how "surprised" the model is by the test data. A lower perplexity means the model assigns higher probability to the ground truth tokens — i.e., it's a better predictor.

The formula $-\sum_x p(x) \log p(x)$ is actually the definition of **entropy** of the distribution $p$, measured in nats (if $\log$ is natural log) or bits (if $\log$ is base 2). In the language modeling context, $x$ ranges over possible next tokens, $p(x)$ is the model's predicted probability for token $x$, and the ground truth is one specific $x^*$.

In practice, for a test set of tokens $w_1, \ldots, w_N$, log perplexity is computed as:

$$\text{Log Perplexity} = -\frac{1}{N} \sum_{i=1}^{N} \log P(w_i \mid w_1, \ldots, w_{i-1}, C)$$

This is the average negative log-likelihood per token. Traditional perplexity (without the "log") would be $e^{\text{Log Perplexity}}$, but the paper works directly in log space, which is numerically more stable.

**Important limitation**: Perplexity measures the model's fit to the full distribution over next tokens. But Smart Compose only _shows_ a suggestion when confidence exceeds a threshold. A model might have better perplexity by being more confident on common tokens, even if it's worse at exactly the suggestions that get shown. This motivates the second metric.

---

#### Metric 2: ExactMatch@N

> _"For a predicted phrase that is N words long, the percentage of predicted phrases that exactly matches the first N words in the ground truth text."_

**ExactMatch@N** asks: if the model predicts a phrase of length $N$, does that phrase exactly match the first $N$ words the user actually typed?

For example, if the user's ground truth continuation is "I will be there at noon" and the model suggests "I will be there," ExactMatch@4 would be 1 (a match), but if the model suggests "I will arrive there," ExactMatch@4 would be 0.

Key design choices in this metric:

- Results are reported for $N \in {1, 2, 3, 5}$ and as a weighted average over all lengths up to 15
- To ensure **fair comparison across models**, each model's triggering confidence threshold is adjusted so that all models have the same **coverage** (same fraction of keystrokes trigger a suggestion). This prevents a model from "cheating" by only making suggestions when it's very confident (high ExactMatch but low coverage)

**Why this metric matters**: ExactMatch directly measures whether the suggestion the user sees is actually correct — it's a proxy for whether the user would press Tab to accept the suggestion. It's much more aligned with the actual product goal than perplexity.

---

### 3.5 Experiments

#### Training Infrastructure

All experiments use **synchronous distributed training** on 32 NVIDIA P100 GPUs. Synchronous training means all GPUs compute gradients on different mini-batches simultaneously, then aggregate and average the gradients before updating the model weights. This is more stable than asynchronous training but requires all workers to complete before proceeding.

#### LSTM Implementation Details

- **LSTM layers**: Long Short-Term Memory (Hochreiter & Schmidhuber, 1997). Each LSTM unit maintains a **cell state** $c_t$ and a **hidden state** $h_t$, controlled by three gates (input, forget, output) that regulate information flow through time. This allows LSTMs to capture long-range dependencies in text better than vanilla RNNs.
- **Residual connections** (He et al., 2015): In deeper LSTM stacks (4 or 6 layers), the output of layer $l$ is added directly to the input of layer $l+2$ (a "skip connection"). This prevents gradient vanishing during backpropagation through many layers.
- **Adam optimizer** (Kingma & Ba, 2014): An adaptive learning rate optimizer that maintains per-parameter moving averages of gradients and their squares, effectively giving each parameter its own learning rate. More robust than vanilla SGD for language model training.
- **Learning rate schedule**: Follows the RNMT+ schedule (Chen et al., 2018) — learning rate warms up from zero, then decays.
- **Uniform label smoothing** (Szegedy et al., 2015): Instead of training the model to assign probability 1.0 to the correct token, the target is softened to $1 - \epsilon$ for the correct token and $\epsilon / (V-1)$ for all other tokens (where $V$ is vocabulary size and $\epsilon$ is a small constant like 0.1). This prevents overconfident predictions and improves generalization.
- **Adaptive gradient clipping**: If the log of the gradient norm exceeds 4 standard deviations of its moving average, the entire training step is discarded. This prevents rare, catastrophically large gradients from destabilizing training.

#### Transformer Implementation Details

The Transformer architecture (Vaswani et al., "Attention is All You Need," 2017) replaces recurrence with **self-attention**: every token attends to every other token in the sequence simultaneously. This captures long-range dependencies more effectively than LSTMs but is quadratically expensive in sequence length.

Parameters:

- 6 layers, 8 attention heads
- Embedding size set to $\frac{1}{3} d_{model}$, so the actual model dimension is $d_{model}$
- Feed-forward inner layer dimension $d_{ff}$ varies

#### Results from Table 1 (LM-A Approach)

|Model|# Params|Train Time (h)|Test Log Perplexity|
|---|---|---|---|
|LSTM-2-1024 (no context)|77.7M|72|3.39|
|LSTM-2-1024|79.8M|74|**3.26**|
|LSTM-4-1024|96.6M|80|3.19|
|LSTM-6-1024|113.3M|120|3.18|
|LSTM-2-2048|171.9M|138|3.13|
|Transformer-768-2048|84.3M|202|3.08|
|Transformer-768-4096|103.2M|212|3.03|
|Transformer-768-8192|141.0M|366|2.96|
|Transformer-1536-8192|310.2M|387|**2.90**|

Key observations from this table:

1. Adding context (subject + previous email) reduces perplexity from **3.39 → 3.26** (a gain of 0.13) with essentially no extra parameters (79.8M vs. 77.7M). This validates the contextual features.
2. Deeper LSTMs help marginally: going from 2 to 4 layers reduces perplexity by 0.07, and adding a 6th layer provides almost no additional gain (3.19 → 3.18).
3. Wider LSTMs (2048 hidden units vs. 1024) help more than adding depth.
4. Transformers consistently outperform LSTMs at similar parameter counts: an 84M-parameter Transformer achieves 3.08 vs. 3.26 for an 80M LSTM — a difference of **0.18**.
5. Transformers are significantly slower to train: 202 hours vs. 74 hours for comparable-sized models.

#### Results from Table 2 (ExactMatch, LM-A)

|Model|@1|@2|@3|@5|Overall|
|---|---|---|---|---|---|
|LSTM-2-1024|84.63%|51.88%|40.25%|31.82%|66.99%|
|LSTM-2-2048|84.89%|54.35%|43.79%|36.18%|68.31%|
|Transformer-768-2048|83.93%|51.97%|43.37%|41.10%|66.94%|
|Transformer-1536-8192|85.33%|53.14%|47.87%|43.92%|67.73%|

The gap between Transformer and LSTM in ExactMatch is **much smaller** than in perplexity. The paper explains: "the improvement in the log perplexity metric is mostly in places where the model is relatively low in confidence and Smart Compose is unlikely to be triggered." In other words, Transformers are better at assigning probabilities to rare or uncertain tokens, but for the high-confidence suggestions that actually get shown to users, LSTMs are nearly as good.

At longer suggestion lengths (@5 words), Transformers have a clear advantage (41.10% vs 31.82% for the base models), which makes sense — attention allows the model to better track long-range context across the full suggested phrase.

#### Results from Tables 3 and 4 (LM-B and Seq2Seq)

**LM-B** achieves comparable quality to LM-A at similar model sizes. The simpler architecture (flat concatenation) doesn't hurt quality, though it results in longer sequences and higher computational cost.

**Seq2Seq** models outperform language models of similar size — the LSTM Seq2Seq (179.7M params, perplexity 3.09) beats LSTM-4-1024 (96.6M params, perplexity 3.19) with more parameters but similar training time. The encoder-decoder with attention genuinely provides better context modeling. The Transformer Big seq2seq model achieves 2.96 perplexity, matching the large Transformer LM, suggesting diminishing returns from more sophisticated architectures at these scales.

---

## 4. Production System (Section 4)

### 4.1 Life of a Smart Compose Request

The system is implemented as a **streaming RPC (Remote Procedure Call) server**. A higher-level Gmail service determines eligibility (is this user opted in? is this an eligible composition context?) and initiates a streaming session.

Each request involves two phases:

#### Context Encoding

The embedding lookups for subject, previous email, date/time, locale — computed **once per streaming session**, not once per keystroke. These embeddings are concatenated and held in memory.

#### Prefix Encoding Steps

The user's typed prefix is fed token by token through the language model. Each token requires one forward pass through all LSTM layers.

**Crucial optimization**: Users have a **sticky assignment** to a server — the same user always routes to the same physical machine during a session. This means the hidden state from the previous request (the last character the user typed) is **cached** on that server. When the user types a new character, the system only needs to compute the LSTM forward pass for newly typed tokens since the last request, not the entire prefix from the start. This dramatically reduces the per-keystroke computation cost.

#### Beam Search Steps

Once the prefix is encoded, beam search generates candidate continuations. If the user's prefix ends **mid-word** (e.g., they typed "Th"), the first beam search step is constrained to only vocabulary words that begin with the characters already typed. This allows **partial word completion** even though the model operates at word-level granularity.

**Offensive token filtering**: During beam search, tokens flagged as offensive or undesirable for product reasons are dropped from consideration. This is a simple but effective safety mechanism operating at the token level.

---

### 4.2 Balancing Quality and Inference Latency (Table 5)

This table is one of the most practically important in the paper. It shows the **relative latency** of beam search for four models, measured on CPU:

|Model|Per-step|Suggestions 1-5|6-10|11-15|Overall|
|---|---|---|---|---|---|
|LSTM-2-1024|**1x**|3.91x|8.51x|12.53x|9.51x|
|LSTM-2-2048|2.29x|8.78x|19.60x|29.13x|20.87x|
|Transformer-768-2048|2.22x|10.78x|17.58x|28.27x|19.71x|
|Transformer-1536-8192|5.42x|25.51x|40.87x|69.84x|37.96x|

The **per-step latency** baseline is LSTM-2-1024. All other numbers are multiples of that.

The critical insight is about **Transformers vs. LSTMs for generation**:

For LSTMs, each generation step has roughly **constant cost** — you feed in one token, get one hidden state out, regardless of how many tokens have been generated so far.

For Transformers, each generation step must **attend over all previous decoding steps**. The self-attention mechanism computes: $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where $Q$, $K$, $V$ are query, key, and value matrices derived from all previous tokens. As the sequence grows by one token, the key and value matrices grow by one row, making each successive step **progressively more expensive**.

This explains why the latency gap between Transformers and LSTMs grows with suggestion length: at 11-15 token suggestions, the Transformer-768-2048 is **17.58x slower** than LSTM-2-1024, vs. only 2.22x slower per step. The growing latency makes long Transformer suggestions increasingly impractical.

**Production decision**: Despite Transformer's quality advantage in perplexity, the team chose **LM-A with LSTM** for production, arguing that:

1. The latency constraint cannot be relaxed
2. The quality gap in the ExactMatch metric (which actually matters for the product) is much smaller than in perplexity
3. The Transformer's perplexity advantage comes primarily on low-confidence predictions that never get shown to users anyway

---

### 4.3 Accelerating Inference (TPU Deployment, Table 7)

|Platform|Relative Latency|Relative Throughput|
|---|---|---|
|CPU|1x|1x|
|TPUv1|0.16x|13.3x|
|TPUv2|**0.10x**|**93.3x**|

**TPU (Tensor Processing Unit)** is Google's custom ASIC designed specifically for matrix multiplication operations that dominate neural network inference. The paper describes two generations:

**TPUv1** (Jouppi et al., 2017): The first generation, which accelerates matrix multiplications and activation functions but required some operations like Softmax to remain on the CPU host. This introduces overhead from data transfer between CPU and TPU.

**TPUv2 (Cloud TPU)** (Google, 2019): Hardware support for a broader set of TensorFlow operations. Combined with the **XLA just-in-time compiler** (Accelerated Linear Algebra), which fuses operations and optimizes computation graphs for the TPU's hardware architecture, almost all computation can run on the TPU.

Result: TPUv2 achieves a **10x latency reduction** and **93x throughput increase** over CPU. The throughput gain is larger than the latency gain because the team uses **request batching** — combining the prefix encoding and beam search computations across multiple simultaneous user requests into a single TPU call, dramatically improving hardware utilization.

---

### 4.4 Training Infrastructure

Production models are trained on **Cloud TPU "quarter-pod"** setups (64 chips). Using the same hardware (TPU) for both training and serving avoids **training/serving skew** — a common problem where models trained with floating-point precision behave differently when served with integer quantization. Since both use the same TPU environment, this consistency problem is eliminated.

---

## 5. Personalization (Section 5)

### 5.1 The Personalization Architecture

The core problem: a global neural language model cannot capture individual writing styles. But training a separate neural LM per user is infeasible (1.5 billion models, each requiring significant storage and compute).

**Solution**: A two-model interpolation system. For each user, a small, simple **personal n-gram language model** is trained. At inference time, its predictions are combined with the global neural LM's predictions:

$$P_{\text{final}} = \alpha P_{\text{personal}} + (1 - \alpha) P_{\text{global}}$$

Where:

- $P_{\text{personal}}$: The per-user n-gram model's probability distribution over next tokens
- $P_{\text{global}}$: The global LSTM language model's probability distribution
- $\alpha \in [0, 1]$: The interpolation weight (a single constant, not context-dependent)

This linear interpolation is computed **at each beam search step**, meaning the suggestion at every token position is influenced by the blend.

#### Why an N-gram model for the personal component?

The paper cites several practical advantages of **n-gram language models with Katz backoff** (Katz, 1987):

1. **Small data requirement**: A user might have only thousands of sent emails, not billions. N-gram models work reasonably well with sparse data; deep neural networks require far more data to learn meaningful patterns.
    
2. **Fast to train**: Rebuilding a personal model periodically (as users write more emails) requires re-training. N-gram counting is computationally trivial compared to gradient-based neural network training.
    
3. **Compact storage**: The model is stored in **Weighted Finite Automata (WFA)** format — a compressed graph representation where paths through the graph represent n-gram sequences and edge weights represent probabilities. This is much more storage-efficient than a neural network's weight matrices.
    

**Katz backoff** (Katz, 1987) is a smoothing technique for n-gram models: if a particular n-gram (e.g., a 4-gram) was never seen in training data, "back off" to a lower-order n-gram (e.g., a 3-gram) with a discount factor. This ensures the model assigns non-zero probability to all possible sequences, even unseen ones.

#### Interpolation Weight

$\alpha$ is a constant (not context-dependent, which would require an additional model). Its value is:

1. First estimated from offline experiments (Figure 4)
2. Then verified through online A/B testing

**Figure 4** shows ExactMatch vs. $\alpha$ values from 0 to 1, evaluated over 11 rounds on real user data. The curve peaks at $\alpha \approx 0.4$ and degrades on both sides:

- $\alpha = 0$: Pure global model — misses personal style
- $\alpha = 1$: Pure personal n-gram — lacks the neural model's generalization power, poor at longer-range context

The optimal 0.4 blend captures personal vocabulary and phrases while relying heavily on the global neural model for fluency and generalization.

#### Real-World Gains

After launching personalization, the team observed:

- **~6% relative gain in Click-Through Rate (CTR)**: Users accept personalized suggestions more often
- **~10% relative gain in ExactMatch**: Personalized suggestions more precisely match what users would actually write

**Table 8** illustrates the value concretely:

|Model|Suggestion|
|---|---|
|Global-only|Will this work for sm + _all group?_|
|Personal-only|Will this work for sm + _artcompose._|
|Blended|Will this work for sm + _artcompose?_|

The global model suggests "all group" (generic, grammatically plausible). The personal model suggests "artcompose" (a specific term this user frequently uses) but with wrong punctuation. The blend gets both the personal term and the correct ending punctuation.

---

## 6. Multilingual Model (Section 6)

### Motivation

Extending to Spanish, French, Italian, and Portuguese brings two immediate problems:

1. **Vocabulary explosion**: A word-level multilingual model needs to accommodate all words across all languages. Spanish, French, Italian, and Portuguese share some vocabulary (cognates) but each has thousands of unique words, plus conjugations, gendered forms, etc. A joint vocabulary might need hundreds of thousands of entries, making the output softmax layer prohibitively large and slow.
    
2. **Rare word coverage**: Less common words in any given language would be OOV much more often in a multilingual setting than in dedicated monolingual models.
    

### WordPiece Tokenization (Schuster & Nakajima, 2012)

**WordPiece** is a subword tokenization algorithm that splits words into smaller units:

- Common words stay intact: "the" → ["the"]
- Rare words get split: "unbelievable" → ["un", "##believe", "##able"]
- Very rare words reduce to characters

With a shared 50k WordPiece vocabulary across all five languages (English, Spanish, French, Italian, Portuguese), common subword units (like common suffixes, prefixes, and stems) are shared, while language-specific rare words are represented by combinations of common pieces. This achieves good coverage without requiring a massive language-specific vocabulary.

The approach is inspired by Johnson et al. (2017), Google's multilingual neural machine translation system, which used a single model to translate between many language pairs by prepending a language identifier token.

### Training Data

|Language|Messages|
|---|---|
|Spanish|~4 billion|
|French|~2 billion|
|Portuguese|~2 billion|
|Italian|~1 billion|

All split 80/20 for training and testing.

### Results (Table 9)

For all four languages, the pattern is:

1. **Monolingual word model** → lowest perplexity and best ExactMatch
2. **Monolingual WordPiece model** → slightly worse than word model (WordPiece introduces approximate token boundaries)
3. **Multilingual WordPiece model** → comparable to monolingual WordPiece for most languages

The exception is **Portuguese**, where the multilingual model actually _outperforms_ both monolingual models. The paper's explanation: Portuguese is "lower-resource" (only 1B messages vs. 4B for Spanish), so it benefits from **cross-lingual transfer** — the multilingual model can leverage Spanish, French, and Italian training data to improve Portuguese, since these Romance languages share substantial vocabulary and grammatical structure.

**Spanish** shows the largest quality gap against the multilingual model (it's the highest-resource language, so it has the most to lose from sharing capacity with other languages).

**Production decision**: Despite monolingual models being technically superior for Spanish, French, and Italian, the team deployed the **multilingual WordPiece model** for all four languages. The trade-off is:

- Slightly lower per-language quality
- One model to train, maintain, serve, and update vs. four separate models
- Simpler serving infrastructure

Language-specific triggering thresholds are set to achieve similar coverage across all four languages and comparable coverage to the English model.

---

## 7. Fairness and Privacy (Section 7)

### Gender-Occupation Bias

> _"When composing 'I am meeting an investor next week,' the model suggested 'Did you want to meet him,' whereas for 'I am meeting a nurse next week,' it suggested 'Did you want to meet her.'"_

This is a textbook example of **stereotypical gender-occupation bias** embedded in training data statistics. Historically, investors are more often men and nurses are more often women, and this correlation appears in billions of emails, so the model learned it. The bias is real and directly harmful — it reinforces stereotypes and could make users uncomfortable.

**Mitigation (v1)**: The team removed any suggestions containing gender pronouns. This is a blunt instrument — it prevents both biased and unbiased pronoun suggestions. The paper acknowledges they "continue to explore algorithmic approaches to handle bias" — suggesting debiasing at the training or post-processing level.

### Memorization Risk

Neural language models trained on sensitive private data can **memorize** specific training examples. Carlini et al. (2018) demonstrated that it's possible to construct specific prompts that cause a language model to regurgitate memorized text — credit card numbers, passwords, private messages.

**Mitigation**: The paper states they performed "extensive testing to make sure only common phrases used by multiple users are memorized." The key principle: if a phrase appears in millions of users' emails, it's not private to any one user. If a phrase appears in only a few users' emails, it's potentially private and should not be reproduced.

The team also worked without direct access to training data, adding a procedural privacy safeguard. The design of data pipelines (normalization of personal entities, quotation removal, etc.) further reduces the surface area for memorization of private content.

---

## 8. Future Work (Section 8)

The paper identifies three promising future directions:

**1. Local attention for Transformers**: Rather than attending over _all_ previous decoding steps (which scales with sequence length), apply attention only over a fixed window of the most recent steps. This would maintain much of the Transformer's quality advantage while capping inference cost at a constant (window size × layer count), making Transformers potentially viable for production serving.

**2. Language model pre-training** (inspired by Dai & Le, 2015 and Devlin et al.'s BERT, 2018): Train a large model on broad, general text corpora first, then fine-tune on email data. Pre-training can produce rich contextual representations that benefit downstream tasks. In 2019, this was emerging as the dominant paradigm in NLP (BERT had just been published); the Smart Compose team anticipated integrating it.

**3. Variational Autoencoders (VAEs)** for language modeling: VAEs (Kingma & Welling, 2014) learn a **latent space** of hidden variables capturing high-level properties of text — topic, sentiment, writing style — not explicitly modeled by standard LMs. Bowman et al. (2016) extended VAEs to RNNs. Applying this to Smart Compose could produce suggestions that better match a user's implicit stylistic preferences and make suggestions more diverse (different suggestions capturing different possible continuations rather than minor variations of the same prediction).

---

## 9. Summary of Key Design Decisions

|Decision|Choice Made|Reason|
|---|---|---|
|Core architecture|LSTM LM-A|Latency vs. quality trade-off; Transformers too slow for production|
|Context encoding|Averaged token embeddings|Simple, fast, single forward pass|
|Beam search|Yes, with confidence threshold|Better than greedy; triggering prevents bad suggestions|
|Hardware|Cloud TPU + XLA|10x latency, 93x throughput over CPU|
|Personalization|Global LSTM + personal n-gram blended|Lightweight per-user model, avoids full neural per-user model|
|Multilingual|Single WordPiece model|Operational simplicity over marginal quality loss|
|Gender bias|Remove pronoun suggestions|Blunt but deployable fix while algorithmic solutions developed|
|Privacy|Entity normalization + memorization testing|Defense in depth|

---

This paper is a masterclass in the gap between research ML and production ML: the best-performing model (large Transformer) was not deployed, and the winning solution (LSTM + personalization + TPUs + careful engineering) prioritized latency, cost, and fairness alongside raw accuracy. Every design choice was governed by the hard constraint that this system had to work, in real-time, for 1.5 billion people.