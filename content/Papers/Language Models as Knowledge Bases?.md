## Paper Overview

This 2019 paper from Facebook AI Research and University College London asks a deceptively simple question: instead of building elaborate, hand-crafted knowledge bases to store facts about the world, could we just _ask_ a pretrained language model? The paper introduces the **LAMA probe** to systematically test this idea.

---

## Abstract — Line by Line

> _"Recent progress in pretraining language models on large textual corpora led to a surge of improvements for downstream NLP tasks."_

**Background context:** By 2019, the dominant paradigm in NLP had shifted toward **pretraining** — training a large neural network on massive amounts of raw text (like all of Wikipedia), and then reusing those learned weights for specific tasks (called **downstream tasks**, e.g., question answering, sentiment analysis). This was a huge leap from training models from scratch for each task.

> _"Whilst learning linguistic knowledge, these models may also be storing relational knowledge present in the training data, and may be able to answer queries structured as 'fill-in-the-blank' cloze statements."_

Two key concepts here:

- **Linguistic knowledge** = grammar, syntax, word meanings, sentence structure. It was already well-established that pretrained models learn this.
- **Relational knowledge** = factual relationships between entities in the world, e.g., "Paris is the capital of France," or "Einstein developed the theory of relativity." This is the _new_ claim being tested.
- **Cloze statement** = a sentence with a blank that must be filled in. Originates from educational psychology (the "cloze procedure" by Taylor, 1953). Example: _"Dante was born in ___."_ The model must predict the missing word.

The hypothesis is that because these models have seen so much text, they may have implicitly memorized facts, not just grammar.

> _"Language models have many advantages over structured knowledge bases: they require no schema engineering, allow practitioners to query about an open class of relations, are easy to extend to more data, and require no human supervision to train."_

This is the motivating argument. Let's unpack each advantage:

- **No schema engineering:** A traditional knowledge base (KB) like Wikidata or Freebase requires someone to predefine the _structure_ — what types of entities exist, what relations are allowed between them, what format they take. This is laborious.
- **Open class of relations:** A structured KB can only answer queries about relations that were designed into it. A language model, in principle, can answer questions about any relation that appeared in text.
- **Easy to extend:** Adding more knowledge to a KB requires careful curation. Adding more training data to a language model is comparatively straightforward.
- **No human supervision:** KBs require human annotators to label facts. Language models train on raw, unannotated text.

> _"We find that (i) without fine-tuning, BERT contains relational knowledge competitive with traditional NLP methods that have some access to oracle knowledge, (ii) BERT also does remarkably well on open-domain question answering against a supervised baseline, and (iii) certain types of factual knowledge are learned much more readily than others by standard language model pretraining approaches."_

Three headline findings, previewed here and elaborated throughout the paper:

- **Finding (i):** BERT, used completely off-the-shelf with no task-specific training, rivals systems that were given _cheating-level_ help (oracle entity linking — explained later).
- **Finding (ii):** BERT approaches the performance of DrQA, a dedicated, supervised QA system.
- **Finding (iii):** Not all facts are equally learnable. Simple one-to-one facts (e.g., "capital of") are easier than complex many-to-many facts (e.g., "diplomatic relations with").

---

## Section 1: Introduction — Paragraph by Paragraph

> _"Recently, pretrained high-capacity language models such as ELMo (Peters et al., 2018a) and BERT (Devlin et al., 2018a) have become increasingly important in NLP."_

**ELMo** = Embeddings from Language Models. A model from AllenNLP that produces _contextualized_ word embeddings using bidirectional LSTMs. "Contextualized" means the same word gets a different representation depending on the sentence it's in (e.g., "bank" in "river bank" vs. "bank account").

**BERT** = Bidirectional Encoder Representations from Transformers. Google's 2018 model, which uses the **Transformer** architecture (rather than LSTMs) and trains with a **masked language modeling** objective (predict randomly hidden words). BERT was a massive leap forward and became the dominant pretrained model.

> _"They are optimised to either predict the next word in a sequence or some masked word anywhere in a given sequence (e.g. 'Dante was born in [Mask] in the year 1265.')."_

This identifies the two main training objectives:

- **Next-word prediction** (used by GPT, ELMo's forward direction): Given words 1 through _t-1_, predict word _t_. This is a **unidirectional** or **autoregressive** objective.
- **Masked word prediction** (used by BERT): Randomly mask some tokens and train the model to reconstruct them using both left and right context. This is a **bidirectional** objective.

The masked objective is what makes BERT naturally suited to the cloze task — it was literally trained to fill in blanks.

> _"The parameters of these models appear to store vast amounts of linguistic knowledge (Peters et al., 2018b; Goldberg, 2019; Tenney et al., 2019) useful for downstream tasks."_

**Parameters** = the learned weights (numbers) inside the neural network. When we say knowledge is "stored in parameters," we mean the model's behavior (what it predicts) reflects that knowledge, even though it's distributed across millions of numerical weights — not stored symbolically like a database entry.

Referenced prior work:

- **Peters et al. (2018b):** Showed that different layers of ELMo capture different types of linguistic knowledge (lower layers = syntax, higher layers = semantics).
- **Goldberg (2019):** Showed BERT captures English syntactic phenomena well.
- **Tenney et al. (2019):** Showed contextual representations encode sentence structure.

> _"This knowledge is usually accessed either by conditioning on latent context representations produced by the original model or by using the original model weights to initialize a task-specific model which is then further fine-tuned."_

Two standard ways to use pretrained models:

1. **Feature extraction / conditioning:** Run the pretrained model to get intermediate representations (vectors), then feed those into a separate, smaller task-specific model. The pretrained model's weights are _frozen_ (not updated).
2. **Fine-tuning:** Start with the pretrained model's weights and continue training on labeled task-specific data. All weights (or most of them) are updated. This generally performs better.

> _"In contrast, knowledge bases are effective solutions for accessing annotated gold-standard relational data by enabling queries such as (Dante, born-in, X)."_

**Knowledge base (KB):** A structured repository of facts, typically stored as _triples_: (subject, relation, object). Example: (Dante, born-in, Florence). You can query a KB by fixing some elements and asking for others.

**Gold-standard:** Data that has been manually verified by humans. The "gold" metaphor implies it's the highest-quality ground truth.

The query notation **(Dante, born-in, X)** means: "What is the object X such that Dante born-in X?" — i.e., "Where was Dante born?"

> _"However, in practice we often need to extract relational data from text or other modalities to populate these knowledge bases. This requires complex NLP pipelines involving entity extraction, coreference resolution, entity linking and relation extraction (Surdeanu and Ji, 2014)—components that often need supervised data and fixed schemas."_

Building a KB from scratch is hard. The pipeline involves:

- **Entity extraction (Named Entity Recognition, NER):** Identifying mentions of entities in text (people, places, organizations, etc.). E.g., recognizing "Dante" as a person.
- **Coreference resolution:** Determining that "he," "the poet," and "Dante" in a paragraph all refer to the same entity.
- **Entity linking:** Connecting a text mention (e.g., "Dante") to the correct entry in a knowledge base (e.g., Wikidata entry Q1067).
- **Relation extraction:** Determining the relationship between two identified entities in a sentence. E.g., from "Dante was born in Florence," extract (Dante, born-in, Florence).

Each step can introduce errors, and those errors **propagate and accumulate** through the pipeline — a mistake in entity extraction breaks everything downstream.

> _"Instead, we could attempt to query neural language models for relational data by asking them to fill in masked tokens in sequences like 'Dante was born in [Mask]', as illustrated in Figure 1."_

This is the paper's core proposal. Rather than running a complex pipeline, just ask the language model directly. **Figure 1** (described in the paper) shows the contrast:

- **Top path (KB):** Query → Symbolic lookup → Answer
- **Bottom path (LM):** Cloze sentence → Neural prediction → Answer

---

## Section 2: Background — Technical Details

### 2.1 Unidirectional Language Models

> _"Given an input sequence of tokens w = [w₁, w₂, ..., wₙ], unidirectional language models commonly assign a probability p(w) to the sequence by factorizing it as follows:_

$$p(w) = \prod_t p(w_t \mid w_{t-1}, \ldots, w_1) \tag{1}$$

**Breaking down this equation:**

- **w** = the entire sequence of words/tokens. A **token** is roughly a word or subword unit (e.g., "running" might be one token, or it might be split into "run" + "##ning" in BERT's tokenizer).
- **p(w)** = the probability the model assigns to the entire sequence. A good language model assigns high probability to fluent, natural language.
- **∏ₜ** = product over all time steps t (i.e., multiply probabilities together). This is the **chain rule of probability** applied to sequences.
- **p(wₜ | wₜ₋₁, ..., w₁)** = the conditional probability of the t-th word given all previous words. This is how the model "reads left-to-right" — at each step, it only sees past context.

This factorization is exact (no approximation) — the joint probability of any sequence can always be written this way by the chain rule. The model's job is to estimate each conditional probability accurately.

> _"A common way to estimate this probability is using neural language models... with:_

$$p(w_t \mid w_{t-1}, \ldots, w_1) = \text{softmax}(W h_t + b) \tag{2}$$

**Breaking down this equation:**

- **hₜ ∈ ℝᵏ** = the **hidden state** vector at position t. This is a k-dimensional vector (k is the hidden size, e.g., 768 for BERT-base) that the neural network computes to represent the context up to position t. It encodes everything the model "knows" about the sequence so far.
- **W ∈ ℝ|V| × k** = a learned **weight matrix** that maps the k-dimensional hidden state to scores for each word in the vocabulary. |V| is the vocabulary size (e.g., 30,000 for BERT).
- **b** = a **bias vector** (a learned offset).
- **Wh_t + b** = a linear transformation producing a |V|-dimensional vector of unnormalized scores (called **logits**) — one score per vocabulary word.
- **softmax(·)** = converts logits into a valid probability distribution. For a vector z, softmax(z)ᵢ = exp(zᵢ) / Σⱼ exp(zⱼ). All outputs are positive and sum to 1.

So equation (2) says: compute a score for every possible next word, then normalize those scores into probabilities.

The key question — "How do you compute **hₜ**?" — is what distinguishes different neural LM architectures:

- **MLP (Bengio et al., 2003):** Simple feedforward network over a fixed window of past words.
- **Convolutional layers (Dauphin et al., 2017):** Uses convolutions to process sequences — good at capturing local patterns.
- **RNN/LSTM (Zaremba et al., 2014; Merity et al., 2016):** Processes words sequentially, maintaining a hidden state that (theoretically) can capture long-range dependencies.
- **Self-attention / Transformer (Radford et al., 2018; Dai et al., 2019; Radford et al., 2019):** Allows every position to directly attend to every other position, capturing long-range dependencies more effectively.

**fairseq-fconv** (Dauphin et al., 2017): Uses **gated convolutional networks** instead of RNNs. The "gating" mechanism (similar to LSTMs) helps control information flow. Trained on **WikiText-103** (103 million words from Wikipedia). Has 324M parameters. The paper uses the pretrained version from the fairseq library.

**Transformer-XL** (Dai et al., 2019): A Transformer-based language model that extends the standard Transformer to handle longer sequences. Standard Transformers process fixed-length segments; Transformer-XL caches previous segment outputs and uses **relative positional encoding** (rather than absolute positions) to allow the model to remember context across segment boundaries. Achieves a **perplexity** of 18.3 on WikiText-103. (Perplexity is a measure of LM quality — lower is better. Roughly, perplexity of 18.3 means the model is as uncertain as if it had to choose uniformly among ~18 equally likely next words.)

### 2.2 Bidirectional "Language Models"

> _"However, in many downstream applications we mostly care about having access to contextual representations of words..."_

For many tasks, we want a representation of a word that captures its full context — both what comes before _and_ after. A unidirectional LM only sees past context, which is a limitation.

> _"Formally, given an input sequence w = [w₁, w₂, ..., wₙ] and a position 1 ≤ i ≤ N, we want to estimate:_

$$p(w_i) = p(w_i \mid w_1, \ldots, w_{i-1}, w_{i+1}, \ldots, w_N)$$

**What this says:** The probability of word _wᵢ_ conditioned on _all other words_ in the sequence — both to the left and to the right. This is a fundamentally different (and more powerful) objective than predicting only from the left.

**Note:** The paper puts "Language Models" in quotes for bidirectional models because, technically, this is not a proper language model in the traditional sense — you can't use it to generate sequences left-to-right. It's better described as a **contextual representation model** (as Tenney et al. suggest), but the authors keep the term "language model" for simplicity.

**ELMo** (Peters et al., 2018a): Runs a **forward LSTM** (left to right) and a **backward LSTM** (right to left) over the input:

- Forward direction produces: **→hᵢ** (hidden state at position i from left context)
- Backward direction produces: **←hᵢ** (hidden state at position i from right context)

The training objective combines the forward and backward **log-likelihoods**:

- Forward: maximize Σᵢ log p(wᵢ | w₁,...,wᵢ₋₁)
- Backward: maximize Σᵢ log p(wᵢ | wᵢ₊₁,...,wₙ)

ELMo uses **multiple LSTM layers** — lower layers capture syntax, higher layers capture semantics. The final word representation is a learned weighted combination of all layers.

Two versions are considered:

- **ELMo original (Eb):** Trained on the Google Billion Word dataset (800M words). 93.6M parameters.
- **ELMo 5.5B (E5B):** Same architecture but trained on English Wikipedia + WMT news data (5.5 billion words).

**BERT** (Devlin et al., 2018a): Uses a **Transformer encoder** (not a recurrent network) with a **Masked Language Model (MLM)** objective:

1. Randomly mask ~15% of input tokens with a special [MASK] token
2. Train the model to predict the original tokens at masked positions using the full bidirectional context

Additionally, BERT uses a **Next Sentence Prediction (NSP)** auxiliary objective — given two sentences, predict whether the second follows the first in the original text. This helps with tasks requiring understanding of inter-sentence relationships.

Two versions:

- **BERT-base (Bb):** 110M parameters, 12 Transformer layers
- **BERT-large (Bl):** 340M parameters, 24 Transformer layers

Both trained on English Wikipedia + BookCorpus (3.3B words total).

---

## Section 3: Related Work

This section positions the paper relative to prior work. Key points:

**What prior work studied:** Linguistic properties of pretrained models (syntax, grammar, word similarity, sentence structure transfer). These studies probe _how well_ models learn language.

**What this paper adds:** A new dimension — _factual and commonsense knowledge_. Do these models store world facts, not just language patterns?

Key related work mentioned:

- **Baroni et al. (2014):** Showed neural word representations (e.g., word2vec) outperform count-based methods for lexical semantics.
- **Marvin and Linzen (2018):** Showed LSTMs don't learn syntax well by testing grammaticality judgments (assigning higher probability to grammatical vs. ungrammatical sentences).
- **McCoy et al. (2019):** Showed BERT can rely on fallible syntactic heuristics rather than true language understanding for NLI (Natural Language Inference) tasks.
- **Peters et al. (2018b):** Lower ELMo layers → local syntax; higher layers → long-range semantics.
- **Goldberg (2019):** BERT captures English syntax remarkably well.
- **Radford et al. (2019) — GPT-2:** A large unidirectional Transformer. Achieved F1=55 on CoQA zero-shot and 4.1% on Natural Questions. The paper excludes it because: (a) the large model was not publicly released, and (b) the small public version performs poorly (< 1% on Natural Questions). GPT-1 is also excluded due to limited, lower-cased vocabulary.

---

## Section 4: The LAMA Probe

**LAMA** = **LA**nguage **M**odel **A**nalysis. A systematic benchmark for testing factual/commonsense knowledge in pretrained LMs.

### Core Methodology

A language model is said to **know** a fact (subject, relation, object) if it can correctly predict the masked object in a cloze sentence expressing that fact.

**Example:**

- Fact: (Dante, born-in, Florence)
- Cloze sentence: "Dante was born in ___."
- The model knows this fact if it ranks "Florence" highest in its output distribution.

### 4.1 Knowledge Sources

#### 4.1.1 Google-RE

- ~60K facts manually extracted from Wikipedia
- 5 relations total, but only **3 are used** (the other 2 involve multi-token objects, which are excluded for technical reasons explained in §4.5):
    - **place of birth** — template: "[S] was born in [O]"
    - **date of birth** — template: "[S] was born on [O]"
    - **place of death** — template: "[S] died in [O]"
- Each fact is **manually aligned** to a Wikipedia sentence that expresses it. This alignment is used by the supervised baseline (RE) — the baseline _knows_ which sentence to look at.
- 5,527 facts total across 3 relations.

#### 4.1.2 T-REx

- Derived from the **T-REx dataset** (Elsahar et al., 2018), which aligns Wikidata triples to Wikipedia sentences.
- **Wikidata** is a large, collaboratively maintained knowledge base with millions of entities and relations.
- **41 Wikidata relations** are used, with up to 1,000 facts per relation (subsampled).
- Templates manually defined per relation (Table 3 shows examples like "P19: [S] was born in [O]").
- Alignment to Wikipedia is **automatic** (not manual like Google-RE), so it can be noisy. But Elsahar et al. report 97.8% alignment accuracy.
- 34,039 facts total — much larger than Google-RE.
- Relations categorized by **cardinality**:
    - **1-to-1 (1-1):** Each subject has exactly one object (e.g., "capital of" — a country has one capital). 937 facts, 2 relations.
    - **N-to-1 (N-1):** Multiple subjects can share an object (e.g., "nationality" — many people share the same nationality). 20,006 facts, 23 relations.
    - **N-to-M (N-M):** Both sides are many-to-many (e.g., "diplomatic relations with" — a country maintains relations with many others). 13,096 facts, 16 relations.

#### 4.1.3 ConceptNet

- **ConceptNet** (Speer and Havasi, 2012): A multilingual commonsense knowledge graph built on top of **Open Mind Common Sense (OMCS)** — a crowdsourced collection of commonsense facts.
- Covers relationships like: "ravens CapableOf fly," "birds HasA feathers," "time HasProperty finite."
- Unlike Google-RE and T-REx, ConceptNet facts are **not aligned to Wikipedia** — so the supervised RE baseline cannot be applied here.
- 16 relations, single-token objects only, 11,458 facts.
- Templates come from the OMCS sentences themselves: find the sentence containing both subject and object, then mask the object.

#### 4.1.4 SQuAD

- **SQuAD** (Stanford Question Answering Dataset, Rajpurkar et al., 2016): A popular QA benchmark where questions are answered from Wikipedia passages.
- The authors manually select **305 context-insensitive questions** (questions that can be answered without the passage — i.e., they test general knowledge, not reading comprehension of a specific passage).
- Questions are manually rewritten as cloze sentences:
    - "Who developed the theory of relativity?" → "The theory of relativity was developed by ___."
- Only single-token answers are kept.
- This tests **open-domain QA** — no passage is provided to the language model.

### 4.2 Models

The six models tested (with abbreviations used in tables):

|Abbreviation|Model|Architecture|Parameters|Training Data|Size|
|---|---|---|---|---|---|
|Fs|fairseq-fconv|ConvNet|324M|WikiText-103|103M words|
|Txl|Transformer-XL (large)|Transformer|257M|WikiText-103|103M words|
|Eb|ELMo original|BiLSTM|93.6M|Google Billion Word|800M words|
|E5B|ELMo 5.5B|BiLSTM|93.6M|Wikipedia + WMT|5.5B words|
|Bb|BERT-base|Transformer|110M|Wikipedia + BookCorpus|3.3B words|
|Bl|BERT-large|Transformer|340M|Wikipedia + BookCorpus|3.3B words|

**How each model generates predictions:**

For position t (the masked token):

- **Unidirectional models (Fs, Txl):** Use the network output **hₜ₋₁** — the hidden state _just before_ position t, representing the left context — to produce the softmax distribution.
    
- **ELMo:** Uses **→hₜ₋₁** (forward direction, just before position t) and **←hₜ₊₁** (backward direction, just after position t). Averages the softmax probabilities from both directions, following the original ELMo training objective.
    
- **BERT:** Literally masks the token at position t with [MASK], then feeds the output vector **hₜ** corresponding to that masked position into the softmax layer. This is exactly what BERT was trained to do.
    

**Unified vocabulary:** All models have different vocabularies (ELMo: ~800K tokens, BERT: ~30K tokens). To compare fairly, the authors compute the **intersection** of all vocabularies — ~21K case-sensitive tokens. All models are evaluated by ranking only tokens in this joint vocabulary. This is critical because a model with a large vocabulary has more "competition" (harder to rank the correct token first), making comparisons unfair otherwise.

### 4.3 Baselines

**Freq (Frequency baseline):** For a given (subject, relation) pair, this baseline ranks candidate objects by how often they appear as objects for that relation in the _test data_. This is purely a prior — it ignores the subject entirely. It measures the upper bound for a model that always predicts the most common answer for a relation (e.g., "United States" for nationality, regardless of the person).

**RE (Relation Extraction):** Uses the pretrained RE model of Sorokin and Gurevych (2017), which:

- Was trained on Wikipedia annotated with Wikidata relations
- Uses an **LSTM encoder** with an **attention mechanism** to extract relation triples from sentences
- Is given the _aligned sentence_ (the sentence known to express the fact) — a significant advantage

Two variants:

- **REₙ (naïve entity linking):** Uses **exact string matching** to link entity mentions in the extracted triples to the query subject. If the text says "Dante Alighieri" but the query uses "Dante," they might not match.
- **REₒ (oracle entity linking):** Uses an **oracle** — if the RE model extracts _any_ triple of the correct relation type from the sentence, the extracted object is linked to the correct answer, regardless of whether the subject matches. This is a form of "cheating" — it's given maximum help. Formally: if RE extracts any (s', r, o') from sentence x, then s' is linked to s and o' is linked to o.

**DrQA** (Chen et al., 2017): A two-stage open-domain QA system:

1. **Retrieval:** Uses TF-IDF information retrieval to find relevant Wikipedia articles for the question.
2. **Reading comprehension:** A neural reading comprehension model extracts the answer from the top-k retrieved articles.

DrQA is **supervised** — trained on labeled QA data. It also has access to an information retrieval system. It's constrained to single-token answers for fair comparison.

### 4.4 Metrics

**Mean Precision@k (P@k):** For a given fact, P@k = 1 if the correct object token is ranked among the top k predictions, 0 otherwise. The mean is taken over all facts.

- **P@1** = Is the top prediction correct? (Strictest metric)
- **P@10** = Is the correct answer anywhere in the top 10?

For **N-to-M relations** where a subject can have multiple valid objects: following standard KB completion practice (Bordes et al., 2013), all other valid objects that appear in the training set are **removed from the candidate set** when evaluating. This avoids penalizing the model for predicting another valid answer.

### 4.5 Design Considerations

**Manually defined templates:** The authors acknowledge that different phrasings of the same query can lead to different (sometimes better or worse) results. They argue this means they're measuring a **lower bound** — if the model knew the fact, a better template might reveal it. They draw an analogy to KBs: a KB can only be queried by its exact relation ID; if you use the wrong ID, you get 0 accuracy regardless of whether the fact is stored.

**Single token only:** Multi-token prediction requires additional decisions (beam size, length normalization, n-gram penalties, etc.) that would confound the results. Also, well-calibrated multi-token generation from bidirectional models (like BERT) is itself an open research problem.

**Object slots only:** They only predict the _object_ in a triple (not the subject or relation). Querying subject slots can be done using reverse relations (e.g., both "capital-of" and "is-capital-of" as separate relations). Relation slots are skipped because: (a) relations span multiple tokens, and (b) the same relation can be expressed many ways, making it unclear what the correct answer is.

---

## Section 5: Results

### Table 2 — Main Results (P@1)

The table shows **mean precision at 1** — what fraction of the time is the model's top prediction correct?

**Google-RE results:**

|Model|birth-place|birth-date|death-place|Total|
|---|---|---|---|---|
|Freq|4.6|1.9|6.8|4.4|
|REₙ|3.5|0.0|0.1|1.2|
|REₒ|13.8|1.9|7.2|7.6|
|Fs|4.4|0.3|3.0|2.6|
|Txl|2.7|1.1|0.9|1.6|
|Eb|5.5|0.1|0.3|2.0|
|E5B|7.5|0.1|1.3|3.0|
|**Bb**|**14.9**|**1.5**|**13.1**|**9.8**|
|**Bl**|**16.1**|**1.4**|**14.0**|**10.5**|

Key observations:

- BERT-large (10.5) beats even the oracle RE baseline (7.6) by **2.9 points**.
- This is remarkable: REₒ was given a sentence known to contain the fact AND an oracle that bypasses entity linking errors. BERT had no such help.
- Birth-date is hard for everyone — dates are highly diverse (many possible values) and BERT gets 1.4%.

**T-REx results (broken down by relation type):**

|Type|#Facts|BERT-large (Bl)|REₒ|
|---|---|---|---|
|1-1|937|**74.5**|10.0|
|N-1|20,006|**34.2**|33.8|
|N-M|13,096|24.3|**36.7**|
|Total|34,039|**32.3**|33.8|

- For **1-to-1 relations**, BERT is dramatically better (74.5 vs. 10.0). This makes sense — if a fact is unambiguous and appears frequently in text, BERT is very good at memorizing it.
- For **N-to-M relations**, BERT underperforms REₒ (24.3 vs. 36.7). When there are many valid answers, the masked language modeling objective may not reliably single out which specific object the query is asking about.
- Overall, BERT-large is competitive with the oracle RE baseline (32.3 vs. 33.8).

**ConceptNet (P@1):**

- BERT-large: 19.2 (best)
- BERT-base: 15.6
- Others: 3.6–6.2

BERT generalizes from factual to commonsense knowledge.

**SQuAD (P@1):**

- DrQA (supervised, with IR): **37.5**
- BERT-large (unsupervised, no IR): 17.4
- But at P@10: DrQA = 63.5, BERT-large = 57.1 (gap is only 6.4 points!)

### Figure 2: P@k Curves for T-REx

Shows how P@k grows as k increases (on a log scale for k). Key insight: even if BERT doesn't rank the correct answer #1, it usually ranks it in the top 10 (~60% of the time) or top 100 (~80% of the time). This means BERT is "in the right ballpark" even when not exactly correct — useful for downstream models that can work with the top-k candidates.

### Figure 3: Pearson Correlation Analysis

For BERT-large on T-REx, the paper computes **Pearson correlation coefficients** between P@1 and various metrics. Pearson correlation r ranges from -1 (perfect negative correlation) to +1 (perfect positive correlation), with 0 meaning no linear relationship.

Variables analyzed:

- **SM (Subject Mentions):** How often the subject is mentioned in BERT's training corpus → **r ≈ -0.005** (nearly zero — not correlated with performance)
- **OM (Object Mentions):** How often the object is mentioned in the training corpus → **r ≈ 0.2** (moderate positive correlation — more common objects are predicted more accurately)
- **LPFP (Log Probability First Prediction):** The log probability BERT assigns to its top prediction → **r ≈ 0.42** (strong positive correlation — when BERT is confident, it's often right)
- **SOCS (Subject-Object Cosine Similarity):** Cosine similarity between the spaCy word vectors of the subject and object → **r ≈ 0.31** (moderate positive — semantically related subject-object pairs are easier)
- **ST (Subject Tokens):** Number of tokens in the subject → **r ≈ 0.12** (small positive)
- **SWP (Subject Word Pieces):** Number of BERT WordPiece tokens in the subject → **r ≈ 0.035** (near zero)

**WordPiece tokenization:** BERT doesn't tokenize by whitespace. It uses a subword tokenization scheme where rare words are split into smaller pieces (e.g., "tokenization" → "token" + "##ization"). This is what SWP measures.

**Key takeaway from Figure 3:** Object frequency in training data (OM) and model confidence (LPFP) are the strongest predictors of success. Subject frequency (SM) doesn't matter much — the model doesn't succeed just because the subject is common; it succeeds because the _object_ is common and the model is confident.

### Figure 4: Rank Distribution Across Query Phrasings

For 100 random facts per relation, 10 different Wikipedia sentences expressing each fact are tested. The rank of the correct answer is recorded for each phrasing.

Results show:

- **BERT-base, BERT-large, ELMo 5.5B:** Lowest variability — rank is consistently low (close to the top). They are **robust to different phrasings** of the same query.
- **ELMo original:** Surprisingly, not far from BERT despite not having seen Wikipedia during training.
- **fairseq-fconv, Transformer-XL:** High variability — performance depends heavily on the specific phrasing used. This suggests these models are more sensitive to surface form.

### Table 3: Qualitative Examples

Shows BERT-large's top-5 predictions with log probabilities for various queries. Notable examples:

**T-REx:**

- "Adolphe Adam died in ___." → **Paris** [-0.5] ✓ (very confident and correct)
- "English bulldog is a subclass of ___." → dogs [-0.3] (predicted "dogs" instead of "dog") — correct semantic type, wrong surface form
- "The official language of Mauritius is ___." → **English** [-0.6] ✓
- "iPod Touch is produced by ___." → **Apple** [-1.6] ✓
- "Bailey Peninsula is located in ___." → **Antarctica** [-1.4] ✓
- "JDK is developed by ___." → IBM [-2.0] (predicted IBM instead of Oracle — wrong, but plausible)
- "Pope Clement VII has the position of ___." → cardinal [-2.4], Pope [-2.5] (predicted "cardinal" first, then "Pope" — correct answer ranked 2nd)

**ConceptNet:**

- "Ravens can ___." → fly [-1.5] ✓
- "Birds have ___." → wings [-1.8] (predicted "wings" instead of "feathers" — both are reasonable)
- "Sometimes virus causes ___." → disease [-1.2], then infection [-2.6] ✓ (infection is the gold answer, ranked 3rd)

**A key observation from these examples:** Even when BERT is wrong, it predicts something of the _correct semantic type_ (a city when asked about a city, a company when asked about a company, a sport position when asked about a sport position). This suggests the model has learned a form of type-awareness.

---

## Section 6: Discussion and Conclusion

> _"We found that it is non-trivial to extract a knowledge base from text that performs on par to directly using pretrained BERT-large."_

This is the paper's central empirical conclusion. The supervised RE pipeline — even with its generous oracle entity linking — struggles to match BERT.

> _"...so we added Wikitext-103 as additional data to the relation extraction system and observed no significant change in performance. This suggests that while relation extraction performance might be difficult to improve with more data, language models trained on ever growing corpora might become a viable alternative to traditional knowledge bases."_

An important ablation experiment: they gave the RE system _more data_ and it didn't help. But language models _do_ improve with more data (ELMo 5.5B outperforms ELMo original). This suggests a fundamental architectural advantage for LMs over pipeline-based RE.

> _"In addition to testing future pretrained language models using the LAMA probe, we are interested in quantifying the variance of recalling factual knowledge with respect to varying natural language templates. Moreover, assessing multi-token answers remains an open challenge for our evaluation setup."_

Two open problems identified for future work:

1. **Template sensitivity** — the paper acknowledges this is a real limitation and a lower bound. Future work should systematically explore prompt/template design (this became a major research direction, eventually leading to the field of **prompt engineering**).
2. **Multi-token answers** — the single-token constraint is a significant limitation. Many facts have multi-word answers (e.g., "New York City," "United Kingdom"). Extending to multi-token generation is left for future work.

---

## Broader Significance and Impact

This paper was historically significant for several reasons:

1. **It coined the framing of "language models as knowledge bases"** — a research agenda that exploded after this paper. It directly inspired work on prompt tuning, knowledge probing, and eventually retrieval-augmented generation.
    
2. **It introduced the LAMA benchmark** — which became a standard evaluation tool for probing factual knowledge in LMs. Subsequent work (e.g., "How Much Knowledge Can You Pack Into the Parameters of a Language Model?" by Roberts et al., 2020) directly builds on it.
    
3. **It presaged prompt engineering** — the insight that the _phrasing_ (template) of a query matters enormously for what a model "knows" became central to GPT-3 and all subsequent large language model work. The paper's "lower bound" argument directly motivates later work on **autoprompt** and **soft prompts**.
    
4. **It raised important caveats** that are still discussed today: BERT might be retrieving answers from memorized training text rather than "understanding" them. The correlation between object frequency and performance supports this concern. The question of whether LMs "know" facts or merely "recall" them remains philosophically and empirically contested.