## Overview and Context

This paper, published at TACL (Transactions of the Association for Computational Linguistics) and presented at ACL 2024, investigates a fundamental and practically important question: when you give a language model a long input containing many documents, does it actually _use_ all that information equally well? The answer, the authors discover, is a resounding no — and the failure follows a very specific, reproducible pattern described as a **U-shaped performance curve**.

---

## 1. Abstract — Line by Line

> "While recent language models have the ability to take long contexts as input, relatively little is known about how well they _use_ longer context."

The distinction between _can take_ and _actually uses_ is the paper's central insight. Just because a model has a 100K-token context window does not mean it pays equal attention to all 100K tokens. The paper separates the question of _capacity_ from _capability_.

> "We analyze the performance of language models on two tasks that require identifying relevant information in their input contexts: multi-document question answering and key-value retrieval."

Two carefully chosen tasks:

- **Multi-document QA**: Realistic, semantically rich — mirrors real-world retrieval-augmented generation (RAG) applications
- **Key-value retrieval**: Synthetic, semantically minimal — a controlled baseline that strips away language complexity

Using both together lets the authors distinguish between failures caused by semantic complexity and failures caused by positional bias in the model's attention mechanism.

> "We find that performance can degrade significantly when changing the position of relevant information, indicating that current language models do not robustly make use of information in long input contexts."

This is the core empirical finding. The **position** of the answer document — not the document itself — determines whether the model finds it.

> "In particular, we observe that performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts, even for explicitly long-context models."

Two named biases emerge:

- **Primacy bias**: Models prefer information at the _beginning_ of the context
- **Recency bias**: Models prefer information at the _end_ of the context
- **The middle is a dead zone**: Performance collapses for information placed in the middle

The phrase "even for explicitly long-context models" is critical — models marketed as handling 16K or 100K tokens still suffer from this problem.

---

## 1. Introduction — Line by Line

### Paragraph 1: Setting the Stage

> "Language models have become an important and flexible building block in a variety of user-facing language technologies, including conversational interfaces, search and summarization, and collaborative writing."

The authors motivate why this matters practically. LLMs are not just research tools — they power products used by millions of people daily.

> "These models perform downstream tasks primarily via prompting: all relevant task specification and data to process is formatted as a textual input context, and the model returns a generated text completion."

**Prompting** is the dominant paradigm: instead of fine-tuning a model for each task, you write a prompt that includes both the instructions and all relevant data. The model then "completes" the prompt with the answer. This means everything — task description, documents, question — must fit into a single input string.

> "These input contexts can contain thousands of tokens, especially when language models are used to process long documents (e.g., legal or scientific documents, conversation histories, etc.) or when language models are augmented with external information."

**Token**: The unit of text that LLMs process. A token is roughly 0.75 words in English. A 4K token context window fits roughly 3,000 words, or about 6 pages. Legal documents, scientific papers, or long conversation histories can far exceed this.

The authors cite **retrieval-augmented generation (RAG)** as a key use case — this is the architecture where a search system first retrieves relevant documents from a database, and those documents are then fed into an LLM's context alongside the user's question. The LLM is expected to read all retrieved documents and find the relevant information.

---

### Paragraph 2: The Technical Challenge

> "Existing language models are generally implemented with Transformers (Vaswani et al., 2017), which require memory and compute that increases quadratically in sequence length."

**Transformers** are the neural network architecture underlying virtually all modern LLMs (GPT, Claude, Llama, etc.). The key operation is **self-attention**, where every token in the sequence attends to (computes relationships with) every other token.

**Why quadratic scaling is a problem**: If sequence length is $n$, then the attention computation requires $O(n^2)$ operations and memory. Doubling the sequence length quadruples the compute needed. This is why early models had small context windows.

**Mathematically**, for a sequence of $n$ tokens with embedding dimension $d$, the attention scores matrix $A$ has shape $n \times n$, with each entry $A_{ij}$ computed as:

$$A_{ij} = \frac{\mathbf{q}_i \cdot \mathbf{k}_j}{\sqrt{d_k}}$$

where $\mathbf{q}_i$ is the query vector for token $i$, $\mathbf{k}_j$ is the key vector for token $j$, and $d_k$ is the key dimension (used for scaling to stabilize gradients). The full attention output for token $i$ is:

$$\text{output}_i = \sum_j \text{softmax}(A_{ij}) \cdot \mathbf{v}_j$$

Computing this for all $n$ tokens requires $O(n^2)$ dot products.

> "As a result, Transformer language models were often trained with relatively small context windows (between 512–2048 tokens). Recent improvements in hardware (e.g., faster GPUs with more memory) and algorithms (Dai et al., 2019; Dao et al., 2022; Poli et al., ...) have resulted in language models with larger context windows."

Referenced algorithms:

- **Transformer-XL (Dai et al., 2019)**: Adds recurrence to extend context beyond fixed windows
- **FlashAttention (Dao et al., 2022)**: A hardware-aware exact attention algorithm that dramatically reduces memory by clever IO management between GPU HBM (high bandwidth memory) and SRAM, enabling longer sequences
- **Hyena (Poli et al., 2023)**: Replaces attention with convolutional operations that scale sub-quadratically

> "...it remains unclear how these extended-context language models make use of their input contexts when performing downstream tasks."

This is the research gap. The field focused on _increasing_ context window size, but didn't rigorously test _whether models actually use that context effectively_. This paper fills that gap.

---

### Paragraph 3: The Experimental Approach

> "We empirically investigate this question via controlled experiments with a variety of state-of-the-art open (MPT-30B-Instruct, LongChat-13B (16K)) and closed (OpenAI's GPT-3.5-Turbo and Anthropic's Claude-1.3) language models."

**Open models**: Weights are publicly available, can be run locally **Closed models**: Available only via API, weights proprietary

The mix is important — it tests whether the U-shaped performance curve is a property of specific models or a general phenomenon across the field.

> "In particular, our experiments make controlled changes to the input context size and the position of the relevant information within the input context and study their effects on language model performance."

**Controlled experiment design**: The authors isolate two independent variables:

1. **Context size** (10, 20, or 30 documents; 75, 140, or 300 key-value pairs)
2. **Position of relevant information** (1st, 5th, 10th, ... last position)

Everything else (the question, the answer document, the distractor documents) stays constant. This isolation is what makes the findings rigorous — you can attribute performance changes specifically to position.

> "If language models can robustly use information within long input contexts, then their performance should be minimally affected by the position of the relevant information in the input context."

This is the **null hypothesis**: if models work as advertised, position shouldn't matter. The paper falsifies this null hypothesis.

---

### Paragraph 4: Multi-Document QA Preview

> "We first experiment with multi-document question answering, which requires models to reason over provided documents to find relevant information and use it to answer a given question; this task mimics the retrieval-augmented generation setup underlying many commercial generative search and question answering applications (e.g., Bing Chat)."

**RAG (Retrieval-Augmented Generation)**: The dominant architecture for LLM-based search and QA systems. A retriever (e.g., Contriever, BM25) fetches relevant documents from a database; a reader (the LLM) reads those documents and generates an answer. Bing Chat (now Copilot) and similar systems use this architecture.

> "We find that changing the position of relevant information in the input context can substantially affect model performance... Furthermore, we observe a distinctive U-shaped performance curve (Figure 1)."

**Figure 1 explained in detail**: The x-axis shows the position of the answer document (1st through 20th), the y-axis shows accuracy. The curve forms a "U" shape:

- Position 1 (beginning): ~75% accuracy for GPT-3.5-Turbo
- Positions 5–15 (middle): drops to ~55–60% accuracy
- Position 20 (end): rises back to ~65% accuracy
- The dashed horizontal line shows **closed-book performance** (56.1%) — the model answering without any documents, using only its training knowledge

The remarkable finding: when the answer is in the middle of the context, GPT-3.5-Turbo performs _worse than if you gave it no documents at all_. The retrieved documents actively hurt performance when placed in the middle.

> "...language model performance is highest when relevant information occurs at the very beginning (primacy bias) or end of its input context (recency bias)."

**Primacy bias**: Tendency to pay more attention to the beginning of the sequence. In human psychology, this is the ability to retain the first items in a list (related to long-term memory encoding). In LLMs, the mechanism is different — it likely stems from training data structure.

**Recency bias**: Tendency to pay more attention to the most recently seen tokens. In decoder-only Transformers, this may be a direct consequence of the **causal attention mask** — each token can only attend to previous tokens, and the most recent tokens have been processed last, potentially making them most salient.

> "For example, when relevant information is placed in the middle of its input context, GPT-3.5-Turbo's performance on the multi-document question task is lower than its performance when predicting without any documents (i.e., the closed-book setting; 56.1%)."

This is one of the paper's most striking specific findings. Middle-context performance (as low as ~53%) is worse than closed-book performance (56.1%). The model is actively distracted and confused by the presence of irrelevant documents when the relevant one is buried in the middle.

> "Furthermore, we find that models often have identical performance to their extended-context counterparts, indicating that extended-context models are not necessarily better at using their input context."

For example, GPT-3.5-Turbo (4K context) and GPT-3.5-Turbo (16K context) have nearly _superimposed_ performance curves when tested on the same inputs. Extending the context window doesn't improve the model's ability to use that context — it just means the model can _accept_ more input while still struggling equally with middle-context information.

---

### Paragraph 5: Key-Value Retrieval Preview

> "Given that language models struggle to retrieve and use relevant information in the multi-document question answering task, to what extent can language models even retrieve from their input contexts? We study this question with a synthetic key-value retrieval task."

The logic here is careful: multi-document QA requires two steps — (1) finding the relevant document, and (2) using it to reason about the answer. The U-shaped curve could theoretically be caused by difficulty in either step. The key-value task isolates step (1) — pure retrieval of an exact match — removing semantic reasoning from the equation.

> "In this task, models are given a collection of JSON-formatted key-value pairs and must return the value associated with a specific key."

Keys and values are **128-bit UUIDs** (Universally Unique Identifiers) — random hexadecimal strings like `9f4a92b9-5f69-4725-ba1e-403f08dea695`. This is deliberate: UUIDs have no semantic meaning, no linguistic patterns the model could exploit. The model literally must find the exact match in the input.

> "Although some models perform the synthetic key-value retrieval task perfectly, other models struggle to simply retrieve matching tokens that occur in the middle of their input context and continue to exhibit a U-shaped performance curve."

Claude-1.3 achieves near-perfect performance on key-value retrieval across all context lengths. But GPT-3.5-Turbo and MPT-30B-Instruct struggle severely when the relevant key-value pair is in the middle. This tells us the problem is a real positional bias in model attention, not just difficulty with semantic reasoning.

---

### Paragraph 6: Analysis of Why — Preview

> "To better understand why language models struggle to robustly access and use information in their input contexts, we study the role of model architecture (decoder-only vs. encoder-decoder), query-aware contextualization, and instruction fine-tuning."

Three potential causal factors investigated:

1. **Model architecture**: Decoder-only (GPT-style, causal attention, can only attend to past tokens) vs. encoder-decoder (T5-style, bidirectional encoder that can attend to all tokens simultaneously)
    
2. **Query-aware contextualization**: In standard setup, the question comes _after_ all the documents. In a decoder-only model, this means the model processes all documents _without knowing what question it's trying to answer_. What if we put the question both before and after the documents?
    
3. **Instruction fine-tuning**: Models are first pre-trained on raw text, then fine-tuned on instruction-response pairs. The fine-tuning data might systematically put instructions at the beginning, reinforcing primacy bias.
    

Key findings preview:

- **Encoder-decoder models** are more robust to position changes — _but only within their training-time sequence length_. Beyond that, they also show a U-shaped curve.
- **Query-aware contextualization** dramatically helps key-value retrieval (near-perfect performance) but barely helps multi-document QA.
- **Both base and instruction-tuned models** show the U-shaped curve, meaning instruction fine-tuning is not the root cause.

---

## 2. Multi-Document Question Answering — Detailed Breakdown

### 2.1 Experimental Setup

**Task formulation:**

Let $k$ = total number of documents in the input context.

The input consists of:

- A question $q$
- $k$ documents: ${d_1, d_2, ..., d_k}$
- Exactly one document $d^*$ contains the answer
- $k - 1$ **distractor documents** that do not contain the answer

The model must find $d^*$ and use it to generate the correct answer.

**Dataset: NaturalQuestions-Open (Lee et al., 2019; Kwiatkowski et al., 2019)**

NaturalQuestions is a benchmark of real user queries issued to Google Search, paired with human-annotated answers from Wikipedia. The authors use 2,655 queries where the annotated long answer is a paragraph (excluding list or table answers, which would complicate evaluation).

**Documents**: Wikipedia passages chunked to at most 100 tokens each.

**Obtaining the answer document**: The Wikipedia paragraph annotated as the answer in NaturalQuestions.

**Obtaining distractor documents**: Use **Contriever** (Izacard et al., 2021), a dense retrieval system fine-tuned on **MS-MARCO** (a large-scale QA dataset), to retrieve the $k-1$ Wikipedia chunks most relevant to the query that do _not_ contain any annotated answers.

Why use relevant distractors rather than random documents? Because this simulates the real RAG scenario where a retriever returns plausible-but-wrong documents. The distractors are presented in **decreasing order of relevance** (most relevant first), mimicking a typical search result ranking.

**Controlling position**: To test the effect of position, the authors simply reorder the documents, placing $d^*$ at different positions (1st, 5th, 10th, etc.) while keeping the distractor documents in place. Crucially, this does not change the question or the desired answer — only where in the long input the answer is hidden.

**Controlling context length**: Add or remove distractor documents. 10 documents ≈ 2K tokens, 20 documents ≈ 4K tokens, 30 documents ≈ 6K tokens.

**Metric: Accuracy**

Following prior work (Kandpal et al., 2022; Mallen et al., 2023), accuracy is defined as:

> Does any of the correct answers (as taken from NaturalQuestions annotations) appear in the model's predicted output?

This is a **substring match** metric — lenient enough to allow paraphrasing that still contains the key answer string.

**Baseline settings:**

- **Closed-book**: Model receives no documents. Must answer from parametric memory (knowledge learned during pre-training). Represents what the model "already knows."
- **Oracle**: Model receives only the single document that contains the answer. Represents upper bound — what the model could achieve if it had perfect retrieval.

---

### 2.2 Models

**Open Models:**

**MPT-30B-Instruct** (MosaicML):

- 30 billion parameters
- Maximum context: 8,192 tokens
- Pre-trained on 1 trillion tokens with 2,048-token sequences
- Additional **sequence length adaptation pre-training**: 50 billion tokens with 8,192-token sequences — a technique to teach the model to handle longer sequences by gradually exposing it to them
- Uses **ALiBi (Attention with Linear Biases)** for positional encoding (Press et al., 2022)

**ALiBi explained**: Traditional positional embeddings (like sinusoidal or learned positional embeddings) add a positional signal to each token's embedding. ALiBi instead modifies the attention scores directly by subtracting a linear bias that grows with distance:

$$A_{ij}^{\text{ALiBi}} = A_{ij} - m \cdot |i - j|$$

where $m$ is a per-head slope. This penalizes attention to distant tokens linearly, encouraging locality. A key advantage: ALiBi can extrapolate to sequences longer than those seen during training, since the bias is computed on-the-fly for any distance.

**LongChat-13B (16K)** (Li et al., 2023):

- 13 billion parameters (based on **LLaMA-13B** by Touvron et al., 2023a)
- Original LLaMA context window: 2,048 tokens
- Extended to 16,384 tokens using **condensed rotary positional embeddings (RoPE)**

**RoPE (Rotary Positional Embeddings)** explained: Rather than adding positional information to token embeddings, RoPE encodes position by _rotating_ the query and key vectors in the attention computation. For position $m$ and dimension $d$, the rotation applies a matrix that depends on $m$. To extend context, LongChat "condenses" the positional indices — effectively mapping positions in a 16K sequence to the 2K range the model was trained on, then fine-tunes on 16K sequences.

**Closed Models:**

**GPT-3.5-Turbo**: OpenAI's model, maximum context 4K tokens **GPT-3.5-Turbo (16K)**: Extended version, maximum context 16K tokens

**Claude-1.3**: Anthropic's model, maximum context 8K tokens **Claude-1.3 (100K)**: Extended version, maximum context 100K tokens

All models use **greedy decoding** — at each generation step, the model picks the single most probable next token. This ensures deterministic, reproducible outputs and removes sampling randomness as a confound.

---

### 2.3 Results and Discussion

**Table 1: Closed-Book and Oracle Accuracy**

|Model|Closed-Book|Oracle|
|---|---|---|
|LongChat-13B (16K)|35.0%|83.4%|
|MPT-30B-Instruct|31.5%|81.9%|
|GPT-3.5-Turbo|56.1%|88.3%|
|GPT-3.5-Turbo (16K)|56.0%|88.6%|
|Claude-1.3|48.3%|76.1%|
|Claude-1.3 (100K)|48.2%|76.4%|

Key observations from Table 1:

- **Closed-book vs. oracle gap**: The gap measures how much the model benefits from having the answer document. GPT-3.5-Turbo improves by 32 percentage points; LongChat-13B improves by 48 percentage points. This shows all models can use the answer document when it's the only document provided.
- **Extended-context models nearly identical to base models**: GPT-3.5-Turbo vs. GPT-3.5-Turbo (16K): 56.1% vs. 56.0% (closed-book), 88.3% vs. 88.6% (oracle). The extended context window brings essentially no benefit in the oracle setting.

**Figure 5: The Main Result**

Three subplots, one each for 10, 20, and 30 total documents. In all cases:

- Peak performance occurs at position 1 (beginning) and the last position
- Performance drops sharply as the relevant document moves toward the middle
- The effect worsens with more documents (deeper U-shape with 30 docs vs. 10 docs)

**Specific numbers from Tables 5, 6, 7 (Appendix G):**

For GPT-3.5-Turbo with 20 total documents:

- Position 1 (index 0): **75.8%**
- Position 5 (index 4): **57.2%**
- Position 10 (index 9): **53.8%** ← lowest, worse than closed-book (56.1%)
- Position 15 (index 14): **55.4%**
- Position 20 (index 19): **63.2%**

The drop from 75.8% to 53.8% represents a 22 percentage point decline solely due to the position of the answer document. The document is identical; only its placement changes.

**Claude-1.3 with 20 total documents:**

- Position 1: **59.9%**
- Position 5: **55.9%**
- Position 10: **56.8%**
- Position 15: **57.2%**
- Position 20: **60.1%**

Claude shows a much _flatter_ U-shaped curve (about 4% variation vs. 22% for GPT-3.5-Turbo). This is notable — Claude-1.3 is more robust to positional variation, though the U-shape pattern still exists.

**Extended-context models are not better at using context:**

For GPT-3.5-Turbo vs. GPT-3.5-Turbo (16K) at 20 documents:

- The performance curves are nearly superimposed throughout
- Index 9 performance: 53.8% vs. 54.1%

This is a striking negative result. Doubling or quadrupling the context window does not change how the model _attends_ to information within that window. The positional bias is intrinsic to how the model was trained, not just its technical capacity.

---

## 3. Key-Value Retrieval Task — Detailed Breakdown

### 3.1 Experimental Setup

**Task formulation:**

Input: A serialized JSON object with $k$ key-value pairs, where:

- Each key is a randomly-generated **128-bit UUID** (e.g., `9f4a92b9-5f69-4725-ba1e-403f08dea695`)
- Each value is a different randomly-generated 128-bit UUID
- All keys and values are unique

The model is given a specific key and must return its associated value.

**Example (Figure 6):**

```json
{
  "2a8d601d-1d69-4e64-9f90-8ad825a74195": "bb3ba2a5-7de8-434b-a86e-a88bb9fa7289",
  "a54e2eed-e625-4570-9f74-3624e77d6684": "d1ff29be-4e2a-4208-a182-0cea716be3d4",
  "9f4a92b9-5f69-4725-ba1e-403f08dea695": "703a7ce5-f17f-4e6d-b895-5836ba5ec71c",
  ...
}
Key: "9f4a92b9-5f69-4725-ba1e-403f08dea695"
Corresponding value: 703a7ce5-f17f-4e6d-b895-5836ba5ec71c
```

**Why UUIDs?** UUIDs are intentionally chosen to eliminate semantic cues. A UUID cannot be guessed, paraphrased, or inferred — the model must perform exact token matching. Any failure is therefore purely due to the model failing to retrieve the right position in context, not semantic confusion.

**Why this is important**: It separates _retrieval_ from _reasoning_. Multi-document QA requires finding the right document AND reasoning over it. Key-value retrieval requires only finding the right pair — there's no reasoning step. If models still show the U-shaped curve on this task, the problem is definitively about positional access in the attention mechanism.

**Context sizes tested**: 75 key-value pairs (~4K tokens), 140 pairs (~8K tokens), 300 pairs (~16K tokens). These token counts are much larger than the multi-document QA settings because each UUID pair takes approximately 50–55 tokens.

**Evaluation**: 500 examples per setting. Same accuracy metric (does the correct value appear in the output?).

---

### 3.2 Results and Discussion

**Figure 7: Key-Value Retrieval Results**

Three striking findings:

**1. Claude succeeds where others fail:** Claude-1.3 and Claude-1.3 (100K) achieve near-perfect accuracy (~100%) on all three context lengths at all positions. This is the most striking inter-model difference in the paper — Claude has solved this particular task regardless of position.

**2. GPT-3.5-Turbo and MPT-30B-Instruct show severe U-shaped curves:** At 300 key-value pairs (~16K tokens):

- Best-case performance (beginning or end): ~85–90%
- Worst-case performance (middle): ~45–50%

Nearly a 40 percentage point swing just from changing position! And remember — the model is only being asked to copy a UUID from the input; there is no reasoning required.

**3. LongChat-13B exhibits unusual behavior:** When the relevant key-value pair is at the _start_ of the input context, LongChat-13B sometimes **generates code** to retrieve the key rather than directly outputting the value. This is a behavioral quirk — the model has apparently learned from coding data that JSON lookups are done programmatically, and confuses this with the task of directly extracting a value.

**The gap between key-value and multi-document QA results:** The fact that Claude does perfectly on key-value retrieval but still shows _some_ U-shaped curve on multi-document QA suggests that the two tasks probe different failure modes. Key-value retrieval is a pure memory access task; multi-document QA also requires reasoning, and the reasoning itself may be disrupted by the presence of many distracting documents regardless of position.

---

## 4. Why Are Models Not Robust? — Detailed Breakdown

### 4.1 Effect of Model Architecture

**Decoder-only models** (all models evaluated so far):

- Process tokens sequentially from left to right
- At each timestep $t$, token $t$ can only attend to tokens $1, ..., t$ (causal/autoregressive masking)
- The attention mask enforces: $A_{ij} = -\infty$ for $j > i$ before softmax

**Encoder-decoder models** (T5-style, Vaswani et al., 2017):

- **Encoder**: Processes the entire input bidirectionally — each token attends to _all_ other tokens simultaneously
- **Decoder**: Generates output tokens autoregressively, attending to both the encoder output and previously generated decoder tokens

**The hypothesis**: Because encoder-decoder models can process the input bidirectionally, they might better estimate the relative importance of different input segments. A document near the beginning can "see" documents near the end via attention, potentially enabling better comparison.

**Models tested:**

**Flan-T5-XXL** (Raffel et al., 2020; Chung et al., 2022):

- T5 architecture (encoder-decoder), "XXL" = extra-extra-large parameter count
- Instruction fine-tuned ("Flan" = "Finetuned Language Models" using instruction tuning)
- Training-time maximum sequence length: **512 tokens** (encoder and decoder)

**Flan-UL2** (Tay et al., 2023):

- "Unifying Language Learning" paradigm
- Initially trained on 512-token sequences
- Then pre-trained on extra 100K steps with 1,024-token sequences
- Final instruction fine-tuning on sequences with **2,048 tokens in encoder, 512 in decoder**
- Uses **relative positional embeddings** (can in principle generalize beyond training-time lengths)

**Key finding (Figure 8):**

When Flan-UL2 is evaluated on sequences **within** its 2,048-token training-time context window:

- Absolute difference between best- and worst-case performance: only **1.9%**
- Nearly flat performance curve — the model is robust to positional changes

When Flan-UL2 is evaluated on sequences **longer than** 2,048 tokens:

- U-shaped performance curve appears
- Performance degrades when relevant information is in the middle

This is a crucial finding: **robustness to positional changes is not an intrinsic property of encoder-decoder architecture — it only holds within the training sequence length**. When extrapolating beyond what the model was trained on, even encoder-decoder models fail.

**Why might bidirectional attention help within training length?**

The authors hypothesize: "encoder-decoder models may make better use of their context windows because their bidirectional encoder allows processing each document in the context of future documents, potentially improving relative importance estimation between documents."

In other words, when a decoder-only model reads Document 3, it hasn't yet seen Documents 4–20. It cannot know that Documents 4–20 are irrelevant to the question (which hasn't been seen yet either). An encoder-decoder model reads all documents simultaneously and can therefore weight them relative to each other and to the question from the start.

---

### 4.2 Effect of Query-Aware Contextualization

**The problem with standard setup:**

In the default multi-document QA and key-value retrieval prompts, the structure is:

```
[Task instruction]
[Document 1]
[Document 2]
...
[Document k]
[Question]
```

For a decoder-only model, when it processes Document 1, it has not yet seen the question. It cannot contextualize Document 1 relative to the question. This is similar to reading a textbook without knowing what exam you're studying for.

**Query-aware contextualization**: Place the query both _before_ and _after_ the documents:

```
[Task instruction]
[Question] ← query placed at beginning
[Document 1]
...
[Document k]
[Question] ← query repeated at end
```

With the query at the beginning, when the model processes Document 1, it has already seen the question and can (in principle) evaluate Document 1's relevance to that question.

**Results on key-value retrieval:**

The effect is dramatic — **all models achieve near-perfect performance** on key-value retrieval with query-aware contextualization across all context sizes. For example, GPT-3.5-Turbo (16K) with query-aware contextualization achieves **perfect performance** with 300 key-value pairs (worst-case without it: 45.6%).

This makes intuitive sense: when the model knows the specific UUID it's looking for before reading the JSON, it can efficiently scan for an exact match.

**Results on multi-document QA (Figure 9):**

The effect is minimal and mixed. Query-aware contextualization:

- Slightly improves performance when relevant information is at the very beginning
- Slightly _decreases_ performance in other settings

The U-shaped curve remains essentially unchanged. Why the discrepancy with key-value retrieval?

The difference is that key-value retrieval is a pure lookup task — knowing the key beforehand enables exact string matching. Multi-document QA requires semantic reasoning: the model must not just find a document containing certain words, but understand that the document is relevant to the question's intent, extract the answer, and compose a response. These semantic reasoning steps are harder to make query-aware.

---

### 4.3 Effect of Instruction Fine-Tuning

**Instruction fine-tuning (IFT)** is the process of taking a pre-trained base language model and further training it on a dataset of (instruction, response) pairs. This is what makes GPT-3.5-Turbo and similar models follow instructions reliably, rather than just completing text.

**The hypothesis**: IFT datasets typically place task instructions at the _beginning_ of the input context. This might train the model to pay more attention to the beginning, contributing to primacy bias.

**Comparison: MPT-30B (base) vs. MPT-30B-Instruct (after IFT)**

**Figure 10 results:**

Surprisingly, both models show a U-shaped performance curve. MPT-30B-Instruct has higher absolute accuracy throughout (instruction fine-tuning improves overall task following), but the shape of the curve is similar.

Specific observations:

- **Base model best-vs-worst gap**: ~10 percentage points
- **Instruct model best-vs-worst gap**: ~4 percentage points

So instruction fine-tuning _slightly_ reduces the severity of the positional bias, but does not eliminate it. The U-shaped curve is **not caused by instruction fine-tuning** — it exists in base models too.

**Connection to prior work:**

Khandelwal et al. (2018) and Press et al. (2021) found that non-instruction-fine-tuned language models are recency-biased (prefer tokens near the end). This recency bias was observed in next-word prediction tasks. The current paper finds that instruction-formatted data introduces _both_ primacy and recency bias (U-shape) — the instruction format teaches models to also attend to the beginning (where instructions typically appear).

**Llama-2 experiments (Appendix E):**

Testing Llama-2 at 7B, 13B, and 70B parameters with and without fine-tuning reveals a **model size threshold** for the U-shaped curve:

- **7B models**: Only recency bias (no primacy) — regardless of fine-tuning
- **13B and 70B models**: U-shaped curve (both primacy and recency bias)

This suggests primacy bias is an emergent property of larger models. The authors hypothesize that smaller models seen in prior work (typically <1B parameters) didn't show primacy bias because they were too small. Larger models may have learned to use longer-range context from pre-training data — for instance, StackOverflow Q&A posts, where the question appears at the beginning and the answer at the end, teaching the model to connect beginning and end.

---

## 5. Is More Context Always Better? Case Study with Open-Domain QA

### Setup

This section moves from controlled experiments to a realistic setting: **open-domain question answering** with NaturalQuestions-Open.

**Retriever-reader setup:**

- **Retriever**: Contriever fine-tuned on MS-MARCO retrieves top-$k$ documents from Wikipedia for each question
- **Reader**: LLM reads those $k$ documents and generates an answer
- Variable: $k$ (number of retrieved documents) ranges from 5 to 50

**Key difference from §2:** In the controlled setting, the context _always_ contained exactly one document with the answer. In the realistic open-domain setting, some questions may have answers in _none_ of the top-$k$ documents, and some may have answers in _multiple_ documents. This tests how performance scales with retrieval in practice.

**Two metrics evaluated:**

1. **Retriever recall@k**: Fraction of queries for which at least one of the top-$k$ retrieved documents contains an annotated answer
2. **Reader accuracy**: Fraction of queries where the LLM correctly answers, given the top-$k$ documents

---

### Results (Figure 11)

**Retriever recall** increases steadily as $k$ increases (from ~60% at $k$=5 to ~85% at $k$=50). More documents → higher chance that one contains the answer.

**Reader accuracy** saturates quickly:

- Performance increases rapidly from $k$=5 to $k$=20
- Beyond $k$=20, additional documents provide almost no benefit
- Specifically: going from $k$=20 to $k$=50 only improves GPT-3.5-Turbo by ~1.5% and Claude-1.3 by ~1%

**The gap between retriever recall and reader accuracy:**

At $k$=50, retriever recall might be ~85% but reader accuracy is only ~65%. This 20-point gap represents queries where the answer is in the retrieved documents but the model still fails to find and use it.

**Why does reader accuracy saturate before retriever recall?**

Because of the findings from §2 — when the answer document is buried in the middle of a long context, models often fail to use it. As $k$ increases, the answer document (when present) is increasingly likely to be pushed toward the middle (since high-relevance documents rank first and tend to cluster near the beginning). The marginal benefit of adding more documents is offset by the increasing difficulty of finding the answer in a longer context.

**Practical implication:**

The authors suggest two promising directions:

1. **Reranking**: Push the most relevant documents to the beginning or end of the context (the positions models attend to best)
2. **Ranked list truncation** (Arampatzis et al., 2009): Instead of always using top-50 documents, adaptively decide when to stop (e.g., once recall saturates, additional documents hurt more than help)

---

## 6. Related Work — Contextual Overview

### 6.1 Long-Context Language Models

The authors situate their work against several lines of research aimed at extending transformer context length:

- **Transformer-XL (Dai et al., 2019)**: Recurrence mechanism that caches previous hidden states
- **Longformer (Beltagy et al., 2020)**: Sparse attention — each token attends to a local window plus a few global tokens, reducing $O(n^2)$ to $O(n)$
- **BigBird (Zaheer et al., 2020)**: Combines random attention, local window attention, and global token attention
- **Linformer (Wang et al., 2020)**: Low-rank approximation of the attention matrix
- **Random Feature Attention (Peng et al., 2021)**: Approximates attention using random feature maps
- **FlashAttention (Dao et al., 2022)**: Exact attention with IO-aware CUDA kernel — same result as standard attention but 2–4× faster and much less memory
- **RWKV (Peng, 2023)**: Replaces attention with an RNN-like mechanism with linear complexity
- **S4 (Gu et al., 2022)**: State space models for long sequences
- **Hyena (Poli et al., 2023)**: Convolutional attention-free architecture

**The key critique**: Most prior long-context work evaluates using **perplexity on a diverse web corpus** as a proxy for long-context ability. Perplexity measures how well the model predicts held-out text. This paper argues perplexity is an inadequate proxy — a model can have low perplexity on long sequences while still failing to access specific facts buried in the middle. "Precise knowledge access on long contexts may be an added challenge" beyond what perplexity measures.

---

### 6.2 How Do Language Models Use Context?

Prior work the authors build on and distinguish from:

- **Khandelwal et al. (2018)**: Found LSTM language models make "increasingly coarse use of longer-term context" — they mainly use nearby tokens. This established recency bias in pre-Transformer models.
- **Sankar et al. (2019)**: Similar findings in dialogue models.
- **Daniluk et al. (2017)**: Attentive LSTMs mainly use recent history.
- **O'Connor and Andreas (2021)**: Many "information-destroying" operations (shuffling words, replacing words with random tokens) had minimal effect on Transformer LM predictions, suggesting models don't use all their context as expected.
- **Krishna et al. (2022)**: Long-context generation in modest-sized Transformers degenerates because models fail to properly condition on long context.
- **Sun et al. (2021)**: Longer contexts improve prediction of only a few tokens — consistent with information-theoretic arguments by Sharan et al. (2018) showing bounded mutual information implies marginal average benefits from longer context.
- **Qin et al. (2023)**: Efficient Transformers on long-context NLP tasks are recency-biased and don't effectively use long-range context.

**What distinguishes this paper**: It studies _instruction-tuned_ models on _downstream tasks_ (QA, retrieval) rather than language modeling perplexity. It also studies _fine-grained position effects_ (not just beginning vs. random, but every position from 1st to 30th), and covers a wider range of model types and sizes.

---

### 6.3 The Serial-Position Effect

This section connects the LLM finding to **human cognitive psychology**:

**Serial-position effect (Ebbinghaus, 1913; Murdock Jr, 1962)**: In free recall experiments, when humans are asked to memorize and recall a list of items, they tend to best remember:

- **First items** (primacy effect): attributed to long-term memory encoding — first items get rehearsed more
- **Last items** (recency effect): attributed to short-term/working memory — last items are still in active memory at recall time
- **Middle items**: Worst recall

The shape of human recall performance as a function of list position is a U-curve — exactly what this paper observes in LLMs.

The authors note: "Observing a serial-position-like effect in language models is perhaps surprising, since the self-attention mechanisms underlying Transformer language models is technically equally capable of retrieving any token from their contexts."

This is a profound observation: unlike human memory, which has biological constraints causing the serial-position effect, **Transformers' self-attention can theoretically attend equally to any position**. $A_{ij}$ is computed for all pairs $(i, j)$ — there is no architectural reason why position 10 should be harder to access than position 1. The fact that LLMs show this effect anyway suggests it's an artifact of the _training data and process_, not the architecture's fundamental capability.

---

## 7. Conclusion — Summary

The paper establishes several things with strong empirical support:

1. **The U-shaped performance curve is real and consistent**: Across 6 models, 2 tasks, multiple context sizes, and various ablations, models consistently perform best when relevant information is at the beginning or end of the input, worst when it's in the middle.
    
2. **Extended-context models are not better at using context**: Simply extending the context window doesn't fix the underlying positional bias.
    
3. **Encoder-decoder models are more robust — but only within training length**: Bidirectional attention helps within trained context lengths; beyond that, even encoder-decoder models degrade.
    
4. **Query-aware contextualization helps retrieval tasks but not reasoning tasks**: Putting the query before the documents helps exact-match retrieval but not semantic QA.
    
5. **The U-shape exists in both base and instruction-tuned models**: It's not caused by instruction fine-tuning but is somewhat mitigated by it.
    
6. **Model size matters**: Smaller models (<13B parameters) show only recency bias; larger models show both primacy and recency bias.
    
7. **More retrieved documents ≠ better performance**: Reader accuracy saturates before retriever recall, indicating models waste additional retrieved documents.
    

**New evaluation protocol proposed**: To claim a model can robustly use long contexts, it is necessary to show minimal performance difference between best- and worst-case positions of relevant information — not just demonstrate that the model can process long sequences.

---

## Key Terminology Glossary

|Term|Definition|
|---|---|
|**Token**|Basic processing unit of an LLM; roughly 0.75 words in English|
|**Context window**|Maximum number of tokens an LLM can process at once|
|**Primacy bias**|Tendency to better use information at the beginning of the context|
|**Recency bias**|Tendency to better use information at the end of the context|
|**U-shaped performance curve**|Performance is highest at beginning and end positions, lowest in the middle|
|**Retrieval-augmented generation (RAG)**|Architecture where a retriever fetches documents and an LLM reads them|
|**Decoder-only model**|Transformer where each token attends only to previous tokens (GPT-style)|
|**Encoder-decoder model**|Transformer with bidirectional encoder and autoregressive decoder (T5-style)|
|**Instruction fine-tuning (IFT)**|Additional training on instruction-response pairs after pre-training|
|**Greedy decoding**|Generating text by always selecting the highest-probability next token|
|**Distractor documents**|Documents that do not contain the answer, used to increase context length|
|**Closed-book performance**|Accuracy without any documents; relies on parametric (training) memory|
|**Oracle performance**|Accuracy when only the answer document is provided; upper bound|
|**Query-aware contextualization**|Placing the question before and after the documents in the prompt|
|**ALiBi**|Attention with Linear Biases; positional encoding that penalizes distant attention|
|**RoPE**|Rotary Positional Embeddings; encodes position via rotation of query/key vectors|
|**Perplexity**|Standard LM evaluation metric: exponential of cross-entropy loss on held-out text|
|**UUID**|Universally Unique Identifier; 128-bit random hexadecimal string|
|**Serial-position effect**|Human memory phenomenon: best recall for first and last items in a list|
|**Contriever**|Dense retrieval model trained with contrastive learning, fine-tuned on MS-MARCO|
|**NaturalQuestions-Open**|Benchmark of real Google queries with Wikipedia-annotated answers|