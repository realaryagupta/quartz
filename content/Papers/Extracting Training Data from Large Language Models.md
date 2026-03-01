## Abstract — Line by Line

**"It has become common to publish large (billion parameter) language models that have been trained on private datasets."**

This sets the context: modern AI systems like GPT-2, GPT-3, and others are trained on massive text corpora — often scraped from the internet, proprietary sources, emails, etc. These models have hundreds of millions to hundreds of billions of _parameters_ (the numerical weights learned during training).

**"This paper demonstrates that in such settings, an adversary can perform a training data extraction attack to recover individual training examples by querying the language model."**

The core claim: by simply _asking_ (querying) a language model to generate text, a malicious actor can reconstruct verbatim text that was in the original training data. This is alarming because:

- The model was not designed to replay training data
- The training data may be private/sensitive
- No special access (like model weights) is needed — just a public API

**"We demonstrate our attack on GPT-2..."**

GPT-2 is OpenAI's language model released in 2019. The authors chose it specifically because both the model _and_ its training data source are public, so the researchers could verify their results without causing real harm.

**"...able to extract hundreds of verbatim text sequences from the model's training data."**

Not paraphrases or summaries — _exact, character-for-character copies_ of training text.

**"These extracted examples include (public) personally identifiable information (names, phone numbers, and email addresses), IRC conversations, code, and 128-bit UUIDs."**

- **PII**: Personally Identifiable Information — data that can identify a specific person
- **IRC conversations**: Internet Relay Chat logs (a form of online messaging)
- **128-bit UUIDs**: Universally Unique Identifiers — essentially random 32-hex-character strings like `1e4bd2a8-e8c8-4a62-adcd-40a936480059`. The fact that these are _random_ makes memorization especially striking — there's no way the model "figured out" a UUID; it must have stored it.

**"Our attack is possible even though each of the above sequences are included in just one document in the training data."**

This is the most alarming finding. Common intuition says: "the model only saw this once, so it can't have learned it." The paper shatters this. Even single-occurrence text can be memorized.

**"Worryingly, we find that larger models are more vulnerable than smaller models."**

This is counterintuitive to some. Bigger models are _better_ at everything — including memorizing. This has direct implications for current AI development, where models keep growing.

---

## Section 1: Introduction — Paragraph by Paragraph

### Paragraph 1: What are Language Models?

> "Language models (LMs)—statistical models which assign a probability to a sequence of words—are fundamental to many natural language processing tasks."

A **language model** learns the statistical structure of text. Given a sequence of words, it assigns a probability to that sequence. For example, P("The cat sat on the mat") > P("The mat sat on the cat") because the first is more grammatically natural.

> "Modern neural-network-based LMs use very large model architectures (e.g., 175 billion parameters [7]) and train on massive datasets (e.g., nearly a terabyte of English text [55])."

- **175 billion parameters** refers to GPT-3
- **A terabyte of text** is roughly 500 billion words — more text than a human could read in thousands of lifetimes
- **Parameters**: These are the numerical weights in the neural network, adjusted during training to minimize prediction error

> "This scaling increases the ability of LMs to generate fluent natural language, and also allows them to be applied to a plethora of other tasks, even without updating their parameters."

The phrase "without updating their parameters" refers to **zero-shot** or **few-shot** learning — where GPT-3, for example, can perform translation, summarization, or coding simply by being prompted, without any task-specific training.

### Paragraph 2: The Privacy Problem

> "...machine learning models are notorious for exposing information about their (potentially private) training data—both in general and in the specific case of language models."

This introduces the field of **training data privacy**. When a model trains on data, some of that data leaves "traces" in the model's weights.

> "...adversaries can apply membership inference attacks to predict whether or not any particular example was in the training data."

**Membership inference attack**: Given a model and a specific text sample, can you determine whether that sample was used during training? This is weaker than _extraction_ — you're only answering yes/no, not reconstructing the text.

### Paragraph 3: Overfitting and the False Sense of Security

> "Such privacy leakage is typically associated with overfitting—when a model's training error is significantly lower than its test error—because overfitting often indicates that a model has memorized examples from its training set."

**Overfitting** defined: A model that has memorized training examples performs much better on training data than on new, unseen data. The gap between training loss and test (validation) loss is the **generalization gap**.

> "Indeed, overfitting is a sufficient condition for privacy leakage and many attacks work by exploiting overfitting."

_Sufficient_ means: if a model overfits, it _will_ leak. But it is not _necessary_ — as this paper shows, models can leak even without overfitting.

> "The association between overfitting and memorization has—erroneously—led many to assume that state-of-the-art LMs will not leak information about their training data."

This is the paper's central argument setup: the prior assumption was wrong.

> "Because these models are often trained on massive de-duplicated datasets only for a single epoch, they exhibit little to no overfitting."

- **De-duplicated**: Removing repeated documents so each document is seen only once
- **Single epoch**: One pass through all the training data. Compare to typical neural networks that train for dozens or hundreds of epochs
- Training for just one epoch on diverse data means the model rarely sees the same example twice, which typically prevents memorization

> "Accordingly, the prevailing wisdom has been that 'the degree of copying with respect to any given work is likely to be, at most, de minimis' and that models do not significantly memorize any particular training example."

**De minimis** (Latin): "too small to be significant." This was the legal and technical consensus — that LLMs couldn't meaningfully reproduce training data. This paper proves that consensus wrong.

---

## Section 2: Background

### Section 2.1: Language Modeling

The paper formalizes what a language model is:

> "one popular choice is a 'next-step prediction' objective"

The model learns to predict the next token given all previous tokens. This is called **autoregressive** modeling.

**The probability model:**

$$\Pr(x_1, x_2, \ldots, x_n)$$

This is the probability of an entire sequence of tokens $x_1$ through $x_n$. A token is roughly a word or word-piece (e.g., "unbelievable" might be split into "un", "believ", "able").

**The chain rule decomposition:**

$$\Pr(x_1, x_2, \ldots, x_n) = \prod_{i=1}^{n} \Pr(x_i \mid x_1, \ldots, x_{i-1})$$

This says: the probability of the whole sequence equals the product of the probability of each token given all previous tokens. This is the **chain rule of probability** — mathematically exact, not an approximation.

- $\prod$ means "product" (multiply all terms)
- $x_i$ is the $i$-th token
- $x_1, \ldots, x_{i-1}$ is the context (all prior tokens)

**Example**: P("The cat sat") = P("The") × P("cat" | "The") × P("sat" | "The cat")

**The neural network:**

$$f_\theta(x_i \mid x_1, \ldots, x_{i-1})$$

- $f$ is the neural network function
- $\theta$ (theta) represents all the parameters (weights) of the network
- This outputs the probability distribution over possible next tokens

**The training loss:**

$$L(\theta) = -\log \prod_{i=1}^{n} f_\theta(x_i \mid x_1, \ldots, x_{i-1})$$

This is the **negative log-likelihood** loss. To minimize this loss, the model must maximize the probability it assigns to the actual training tokens. Key points:

- $\log$ converts the product to a sum (easier to compute): $-\log \prod = -\sum \log$
- The negative sign means minimizing loss = maximizing probability
- The "optimal" solution to this objective would be to _memorize_ every training example — giving probability 1 to the exact training sequence

**Text generation:**

$$\hat{x}_{i+1} \sim f_\theta(x_{i+1} \mid x_1, \ldots, x_i)$$

The `~` means "sampled from." The model samples the next token from the probability distribution, feeds it back in, and repeats. This is called **autoregressive generation**.

**Top-n sampling**: Set all but the top-n most likely tokens to probability zero, renormalize, then sample. This prevents the model from ever generating very unlikely (often nonsensical) tokens.

**GPT-2 specifics:**

- Uses **word-pieces** with a **Byte Pair Encoder (BPE)** — a vocabulary construction method that splits rare words into subword units
- GPT-2 XL: 1.5 billion parameters
- GPT-2 Medium: 345 million parameters
- GPT-2 Small: 117 million parameters
- Trained on 40GB of text from Reddit-linked webpages
- Trained for ~12 epochs (unusual — most papers say 1 epoch, but this is from personal communication with authors)
- Training loss ≈ 10% lower than test loss — minimal overfitting

### Section 2.2: Training Data Privacy

**Three types of attacks, in increasing severity:**

1. **Membership inference attack**: "Was this specific document in the training data?" — yes/no answer only
2. **Model inversion attack**: Reconstruct a _representative_ (fuzzy/average) example from a class. Example: recover a blurry "average face" that the face-recognition model learned
3. **Training data extraction attack**: Reconstruct _verbatim_ training examples — exact text, exact numbers, exact strings

The paper focuses on #3, which is most dangerous. Prior work had only shown extraction attacks on small models under artificial conditions (e.g., deliberately inserting "canary" strings many times).

**Differential Privacy (DP)**: A mathematical framework guaranteeing that the model's output is nearly indistinguishable whether or not any single record was included in training. Implemented via **DP-SGD** (Differentially Private Stochastic Gradient Descent). Trade-off: strong privacy guarantees but often reduces model accuracy, especially on rare/unusual data points.

---

## Section 3: Threat Model and Definitions

### Section 3.1: Defining Memorization

The paper needs a _rigorous_ definition to make claims testable. Informal notions of "memorization" won't do.

**Model Knowledge Extraction (Definition 1):**

> A string $s$ is _extractable_ from an LM $f_\theta$ if there exists a prefix $c$ such that: $$s \leftarrow \arg\max_{s': |s'|=N} f_\theta(s' \mid c)$$

Breaking this down:

- $s$: the string we want to extract (length $N$ tokens)
- $c$: a prefix (context/prompt) fed to the model
- $f_\theta(s' \mid c)$: the probability the model assigns to sequence $s'$ given prefix $c$
- $\arg\max$: "find the $s'$ that maximizes..." — i.e., the most likely continuation
- The model "knows" $s$ if, when given the right prompt $c$, it will generate $s$ as its most likely output

**Important caveat**: Computing exact $\arg\max$ over all possible length-$N$ sequences is computationally intractable (there are $|V|^N$ possibilities where $|V|$ is vocabulary size). So in practice, approximate methods like greedy sampling are used.

**k-Eidetic Memorization (Definition 2):**

> A string $s$ is _k-eidetic memorized_ (for $k \geq 1$) by an LM $f_\theta$ if:
> 
> 1. $s$ is extractable from $f_\theta$ (Definition 1), AND
> 2. $s$ appears in at most $k$ training examples: $|{x \in X : s \subseteq x}| \leq k$

- **Eidetic** comes from "eidetic memory" (photographic memory) — recalling something after seeing it very few times
- $X$: the full training dataset
- $x \in X$: a single training document
- $s \subseteq x$: string $s$ appears somewhere within document $x$
- The count $|{...}|$ counts _distinct documents_ containing $s$, not total occurrences
- **Lower $k$ = stronger memorization** (the string appeared in fewer documents)
- $k=1$ is the strongest case: the string appears in only one training document

**Why this definition matters**: It separates "interesting" memorization from trivial memorization. Memorizing "the" is not interesting (it's in every document, so $k$ is enormous). Memorizing someone's phone number that appears in one webpage is interesting ($k=1$).

### Section 3.2: Threat Model

**Adversary's capabilities**: Black-box access only. The attacker can:

- Submit input text to the model
- Receive output text or token probabilities
- Call $f_\theta(x_1, \ldots, x_n)$ to get sequence probability

The attacker _cannot_:

- Inspect model weights $\theta$
- Access hidden states, attention vectors
- See training data directly

This is realistic — GPT-3 is available only through an API.

**Adversary's goal**: Extract memorized training data, with emphasis on low-$k$ (rare) examples. The attack is _untargeted_ — they're not trying to extract a specific person's data, just whatever the model has memorized.

### Section 3.3 & 3.4: Ethical Considerations

The authors minimize harm by targeting GPT-2 (public model, public training data). They still partially redact PII they find. They contacted the individual whose information appears in Figure 1. Key ethical tension: publishing the attack enables malicious use, but hiding it prevents the research community from developing defenses.

---

## Section 4: Initial Attack

### Step 1 — Generate Text (Section 4.1)

The model is initialized with a start-of-sentence token and generates 256 tokens using **top-n sampling** with $n=40$ (keeping only the top 40 most likely next tokens at each step). 200,000 samples are generated.

The intuition: if the model assigns high probability to a sequence, maybe that sequence was in the training data.

### Step 2 — Membership Inference via Perplexity (Section 4.2)

**Perplexity formula:**

$$P = \exp\left(-\frac{1}{n} \sum_{i=1}^{n} \log f_\theta(x_i \mid x_1, \ldots, x_{i-1})\right)$$

Breaking this down:

- $n$: length of the sequence
- $f_\theta(x_i \mid x_1, \ldots, x_{i-1})$: the model's probability for the $i$-th token given previous tokens
- $\log$: natural logarithm
- $-\frac{1}{n}\sum \log(\cdot)$: the average negative log-probability — this is the **cross-entropy** per token
- $\exp(\cdot)$: exponentiating converts back from log-space

**Intuition**: Low perplexity = the model is "not surprised" by the sequence = assigns high probability = likely memorized. High perplexity = the model finds this sequence unlikely.

**Results of initial attack**: Found memorized content (MIT license, website terms), but only high-$k$ examples (seen thousands of times). Two problems:

1. **Low diversity**: Many duplicate samples (hundreds of copies of the same memorized Vaughn Live user guidelines)
2. **High false positives**: Repetitive text like "I love you. I love you. I love you." gets low perplexity but isn't training data

---

## Section 5: Improved Attack

### Improved Generation (Section 5.1)

**Problem**: Top-n sampling from an empty context always starts similarly, producing the same popular prefixes.

**Fix 1 — Decaying Temperature (Section 5.1.1)**

Temperature modifies the softmax output. Normally:

$$\text{softmax}(z)_i = \frac{\exp(z_i)}{\sum_j \exp(z_j)}$$

With temperature $t$:

$$\text{softmax}(z/t)_i = \frac{\exp(z_i/t)}{\sum_j \exp(z_j/t)}$$

- $z$: the "logit" vector — raw output scores of the neural network before conversion to probabilities
- $t > 1$: "flattens" the distribution — all tokens become more equally likely → more diverse, less predictable output
- $t < 1$: "sharpens" the distribution — the most likely token becomes even more dominant → less diverse

**Strategy**: Start at $t=10$ (very diverse/random), decay to $t=1$ (normal) over the first 20 tokens. This lets the model "explore" many different starting points, then commit to high-confidence continuations. The idea is that memorized content will "snap into focus" once the model gets close to a memorized prefix.

**Fix 2 — Internet Conditioning (Section 5.1.2)**

Seed the model with 5–10 tokens scraped from the open web (Common Crawl, not Reddit, to minimize overlap with GPT-2's training data). This ensures diverse, realistic prefixes similar to actual training data.

### Improved Membership Inference (Section 5.2)

**Core insight**: Instead of just looking at perplexity under GPT-2, compare GPT-2's perplexity to some _other_ measure of text likelihood. If GPT-2 assigns a sequence much higher probability than expected, it's likely memorized.

**Six metrics used:**

1. **Perplexity**: Raw GPT-2 XL perplexity (baseline, already described)
    
2. **Small/Medium comparison**: Ratio of log-perplexities: $$\text{Score} = \frac{\log P_{\text{XL}}}{\log P_{\text{Small}}}$$
    
    - Intuition: XL has more memorization capacity than Small. If XL assigns much lower perplexity than Small, the sequence may be specifically memorized by XL
    - Filters out text that's just generally common (both models would find it likely)
3. **zlib entropy comparison**: $$\text{Score} = \frac{\log P_{\text{GPT-2}}}{\text{zlib entropy}}$$
    
    - **zlib**: A standard lossless compression algorithm. Its compression ratio reflects the statistical redundancy/predictability of text
    - If GPT-2 assigns much lower perplexity than zlib would suggest (i.e., the sequence isn't structurally repetitive but GPT-2 still knows it), it's likely specifically memorized
    - Filters out trivially repetitive text ("I love you I love you...") which zlib would also compress well
4. **Lowercase comparison**: $$\text{Score} = \frac{P_{\text{original}}}{P_{\text{lowercased}}}$$
    
    - If a sequence has unusual capitalization (e.g., "CamelCase code identifiers" or "ALL-CAPS HEADLINES"), the model will find the original much more likely than a lowercased version
    - Signals content where exact casing was memorized
5. **Sliding window perplexity**: $$\text{Score} = \min_{\text{window } w} P(w)$$
    
    - Takes the minimum perplexity over all 50-token windows in the sequence
    - Handles cases where only _part_ of a 256-token sample is memorized — even if surrounded by high-perplexity non-memorized text

---

## Section 6: Evaluation

### Methodology

**3 generation strategies × 6 metrics = 18 configurations**

For each configuration: generate 200,000 samples, rank by the metric, select 100 from the top-1000 (with a bias toward higher-ranked samples) → **1,800 total candidate samples**

**De-duplication via trigram overlap:**

Define the trigram-multiset $\text{tri}(s)$ = multiset of all word-level 3-grams in $s$.

Sample $s_1$ is a duplicate of $s_2$ if: $$|\text{tri}(s_1) \cap \text{tri}(s_2)| \geq |\text{tri}(s_1)| / 2$$

This says: if more than half of $s_1$'s trigrams also appear in $s_2$, they're duplicates. The intersection uses multiset semantics (counts matter).

**Verification**: Authors searched Google for found strings AND worked with OpenAI to query the actual GPT-2 training dataset using fuzzy 3-gram matching.

### Results

**604 out of 1,800 candidates (33.5%) were verified as memorized.** Best single configuration: 67% precision (Internet sampling + zlib metric).

**Table 1 breakdown of 604 samples** — top categories:

- US/international news: 109
- Log files: 79
- Licenses/copyright: 54
- Named individuals (non-news): **46** ← PII concern
- Contact info: **32** ← PII concern
- High entropy (UUIDs, base64): 35

---

## Section 7: Model Size and Insertion Frequency

This section uses **naturally occurring canaries** — URLs in a specific format that appear varying numbers of times in one training document.

**Experiment**: URLs of the form `/r/[6-char-ID]/[article_title]` from a pastebin.com document. Each URL appears a different number of times in that document (due to Reddit comment threading where quoted emails repeat).

**Two attack difficulty levels**:

1. **Hard**: Provide the common URL prefix, sample 10,000 completions, check if any match verbatim
2. **Easy**: Also provide the first 6 characters of the specific URL, then use beam search

**Key results (Table 4)**:

- GPT-2 XL memorizes all URLs appearing ≥33 times (hard mode)
- GPT-2 Medium memorizes roughly half
- GPT-2 Small memorizes none (hard mode), only the one appearing 359 times (easy mode)

**Two lessons**:

1. Larger models memorize dramatically more: XL memorizes 18× more than Small in one experiment
2. Even 33 repetitions within _a single document_ is enough for memorization in the largest model

---

## Section 8: Mitigations

Four proposed strategies, each with limitations:

**1. Differential Privacy (DP-SGD)**

- Adds calibrated noise to gradients during training
- Provides mathematically provable upper bounds on privacy leakage
- Problems: requires knowing which records to protect (hard for web data), degrades model quality especially on rare data, increases training time

**2. Data Curation**

- Filter PII from training data
- Better de-duplication (not just document-level — also within-document repetitions)
- Careful data sourcing (pastebin.com alone was the 12th most common domain in GPT-2's training set)
- Problem: imperfect — some sensitive data always slips through

**3. Limiting Downstream Impact**

- Fine-tuning on clean task-specific data may cause "catastrophic forgetting" of pre-training memorization
- Downstream systems could filter outputs using membership inference
- Problem: fine-tuning may introduce its own memorization

**4. Auditing**

- Use extraction attacks to _test_ models before deployment
- Complements formal DP guarantees with empirical measurement

---

## Section 9: Lessons (Key Takeaways)

1. **Extraction attacks are practical**, not just theoretical — work at scale with black-box access
    
2. **Memorization ≠ overfitting** — models with negligible train-test gap still memorize. The _average_ loss being similar doesn't mean individual examples aren't memorized
    
3. **Larger models memorize more** — scaling laws apply to memorization too, which is a serious concern as models grow to hundreds of billions of parameters
    
4. **Context-dependence**: The right prompt dramatically increases extraction. With the prompt "pi is 3.14159", GPT-2 correctly generates 799 digits of π. With "e begins 2.7182818, pi begins 3.14159", it generates 824. This means the paper's 604 examples are a _vast undercount_ — many more sequences are memorized but not found due to suboptimal prompting
    
5. **The 0.1% figure is a loose lower bound**: Of 600,000 generated samples, at least 604 (0.1%) contained memorized text — but only 1,800 were manually inspected
    

---

## Summary of the Full Technical Pipeline

```
[GPT-2 model]
     ↓
Generate 200,000 samples (256 tokens each)
  using one of: top-n / temperature / internet-conditioned
     ↓
Rank samples using one of 6 metrics:
  perplexity / small-ratio / medium-ratio / 
  zlib-ratio / lowercase-ratio / window
     ↓
De-duplicate using trigram overlap
     ↓
Select top-100 from top-1000 (per configuration)
     ↓
Manual inspection + Internet search + OpenAI confirmation
     ↓
604 verified memorized training examples
```

This pipeline is simple, requires no model internals, and works against any language model accessible via API — which is what makes it practically dangerous.