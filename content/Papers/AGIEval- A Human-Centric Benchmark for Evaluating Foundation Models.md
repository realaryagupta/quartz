# **AGIEval: A Human-Centric Benchmark for Evaluating Foundation Models**

## **Complete Detailed Analysis**

---

## **ABSTRACT - Detailed Breakdown**

### **Core Problem Statement**

The abstract opens by establishing the fundamental challenge: _"Evaluating the general abilities of foundation models to tackle human-level tasks is a vital aspect of their development and application in the pursuit of Artificial General Intelligence (AGI)."_

**What this means:**

- **Foundation models**: Large pre-trained models (like GPT-4) that can be adapted to many tasks
- **Human-level tasks**: Activities that require human cognition, reasoning, and knowledge
- **AGI (Artificial General Intelligence)**: AI systems with broad, flexible intelligence comparable to humans across diverse domains

**Key criticism of existing approaches:** _"Traditional benchmarks, which rely on artificial datasets, may not accurately represent human-level capabilities."_

**Why this matters:**

- Artificial datasets are constructed specifically for machines
- They may not reflect real-world complexity
- They don't measure capabilities humans actually need
- Models might overfit to artificial patterns

---

### **The Proposed Solution: AGIEval**

The authors introduce **AGIEval**, a benchmark based on:

1. **College entrance exams** (SAT, Gaokao)
2. **Law school admission tests** (LSAT)
3. **Math competitions** (AMC, AIME)
4. **Lawyer qualification tests** (bar exams)

**Why these sources are superior:**

- **Officially recognized standards**: These exams define what society considers necessary human capabilities
- **High stakes**: Millions of people take them annually (12M for Gaokao, 1.7M for SAT)
- **Real-world relevance**: Success on these exams determines educational and career opportunities
- **Established difficulty**: Percentile scores show human performance distributions

---

### **Key Findings**

**1. GPT-4's Impressive Performance:**

- **95% accuracy on SAT Math** (approaching top 1% of human test-takers)
- **92.5% accuracy on Gaokao English**
- **Exceeds average human performance** on SAT, LSAT, and math competitions

**Statistical context:**

- Average human SAT Math score: ~66th percentile → 66% accuracy
- GPT-4's 95% would place it above 99% of human test-takers
- This represents a **major milestone** in AI capabilities

**2. Identified Weaknesses:** Despite strong overall performance, GPT-4 struggles with:

- **Complex reasoning tasks** (e.g., LSAT analytical reasoning, physics problems)
- **Domain-specific knowledge** (law, chemistry)

---

### **Four-Dimensional Capability Analysis**

The authors analyze model performance across:

**1. Understanding**: Can the model comprehend semantic meaning? **2. Knowledge**: Can it recall relevant facts and formulas? **3. Reasoning**: Can it perform logical deduction and multi-step thinking? **4. Calculation**: Can it execute mathematical operations accurately?

**Significance**: This granular analysis reveals _why_ models succeed or fail, not just _that_ they do.

---

### **Broader Impact**

The abstract concludes: _"By concentrating on tasks pertinent to human cognition and decision-making, our benchmark delivers a more meaningful and robust evaluation of foundation models' performance in real-world scenarios."_

**Why this matters for AI development:**

- Provides actionable insights for improving models
- Establishes realistic expectations for deployment
- Identifies gaps between current AI and human-level AGI
- Creates accountability through standardized, reproducible evaluation

---

## **SECTION 1: INTRODUCTION - Detailed Breakdown**

### **1.1: Motivation and Context**

**Opening premise:** _"Large foundation models, such as the latest large language models (LLMs) ChatGPT and GPT-4, have exhibited remarkable versatility and adaptability, with plethora of applications spanning various domains as a decision-making assistant."_

**Examples of deployment:**

- Daily tasks (email writing, scheduling)
- Specialized fields:
    - **Law**: Contract analysis, legal research
    - **Medicine**: Diagnostic assistance, literature review
    - **Finance**: Market analysis, risk assessment

**The AGI trajectory:** _"With these advancements, AI systems are inching closer to achieving Artificial General Intelligence (AGI)."_

**What AGI means:**

- Not narrow AI (specialized for one task)
- Broad intelligence across domains
- Ability to transfer knowledge between contexts
- Human-like flexibility in problem-solving

---

### **1.2: The Assessment Challenge**

**Core requirement:** _"It is essential to effectively assess their general abilities in handling human-centric tasks, identify potential shortcomings, and ensure that they can handle complex, human-centric tasks effectively."_

**Three key evaluation goals:**

1. **Assessment**: Measure current capabilities accurately
2. **Shortcoming identification**: Find specific weaknesses
3. **Reliability verification**: Ensure trustworthy performance

**Why reasoning evaluation is critical:** _"Evaluating their reasoning abilities is also crucial to ensure their reliability and trustworthiness across diverse settings."_

**Real-world implications:**

- Medical AI must reason correctly about symptoms
- Legal AI must apply precedents logically
- Financial AI must deduce market implications
- Errors in reasoning can cause harm in high-stakes domains

---

### **1.3: Problems with Traditional Benchmarks**

**Primary critique:** _"Traditional benchmarks for evaluating foundation models often fall short in providing an accurate assessment of their general abilities in handling human-level tasks."_

**Specific limitations identified:**

**1. Use of artificial datasets:**

- Created by researchers specifically for AI evaluation
- May not reflect real-world task distributions
- Often focus on narrow, well-defined problems
- Can be "gamed" through pattern matching

**2. Lack of human-cognitive alignment:** _"These benchmarks often focus on tasks that do not truly represent the complexities and nuances of real-world human cognition and decision-making."_

**Example contrast:**

- **Artificial benchmark**: "Classify sentiment of this sentence: 'The movie was okay.'"
- **Human-centric task**: "Should this patient undergo surgery based on their symptoms, medical history, and risk factors?"

**3. Limited real-world applicability:** _"Leading to a skewed evaluation of models' capabilities and limiting their ability to provide meaningful insights into the models' real-world applicability."_

**Consequence**: Models might score well on benchmarks but fail in deployment.

---

### **1.4: The AGIEval Solution**

**Design philosophy:** _"We introduce a human-centric benchmark AGIEval specifically designed to evaluate the general abilities of foundation models in tasks pertinent to human cognition and problem-solving."_

**Data sources - specifics:**

**1. Chinese College Entrance Exam (Gaokao):**

- **Scale**: 12 million participants annually
- **Subjects**: History, Geography, Biology, Chemistry, Physics, Math, English, Chinese
- **Purpose**: Determines university admission in China
- **Significance**: One of the world's most competitive standardized exams

**2. American SAT:**

- **Scale**: 1.7 million participants annually
- **Sections**: Math, English (Reading & Writing)
- **Purpose**: College admissions in the United States
- **Percentile data available**: Allows comparison with human performance distributions

**3. Law School Admission Test (LSAT):**

- **Participants**: ~170,000 annually
- **Sections**:
    - Logical Reasoning: Evaluate arguments
    - Analytical Reasoning: Logic games
    - Reading Comprehension: Dense legal passages
- **Purpose**: Law school admissions
- **Difficulty**: Tests abstract reasoning under time pressure

**4. Math Competitions:**

- **AMC (American Mathematics Competitions)**: ~300,000 participants
- **AIME (American Invitational Mathematics Examination)**: ~3,000 participants (top AMC scorers)
- **Characteristics**: Creative problem-solving, non-standard questions

**5. Lawyer Qualification Test:**

- **Scale**: 820,000 participants annually (Chinese bar exam)
- **Content**: Constitutional law, contract law, criminal law, professional ethics
- **Purpose**: Legal practice authorization

**6. Civil Service Examinations:**

- **Scale**: 2 million participants annually
- **Content**: General knowledge, logical reasoning, domain expertise
- **Purpose**: Government employment

---

### **1.5: Why These Exams Matter**

**Official recognition:** _"These exams establish officially recognized standards for assessing human-level capabilities."_

**What makes a good benchmark:**

1. **Widely accepted**: Society agrees these tests measure important capabilities
2. **High standards**: Difficulty is calibrated to human performance
3. **Diverse participation**: Millions of people from varied backgrounds
4. **Consequential**: Results determine life outcomes (education, careers)
5. **Stable over time**: Question quality is maintained through professional development

---

### **1.6: Bilingual Design**

**Innovation:** _"The benchmark covers bilingual tasks in both Chinese and English, allowing for a more comprehensive evaluation of the models' capabilities."_

**Why bilingualism matters:**

**1. Language-specific reasoning:**

- Some concepts are easier to express in certain languages
- Logical structures vary across languages
- Cultural context affects question interpretation

**2. Model generalization:**

- Tests if models truly understand concepts or just English patterns
- Reveals language-specific biases or limitations
- More realistic for global deployment

**3. Broader evaluation scope:**

- English: ~20% of global population
- Chinese: ~18% of global population
- Combined: Covers ~38% of humanity

---

### **1.7: Scope and Focus**

**Important limitation noted:** _"Since state-of-the-art foundation models, such as Text-Davinci-003, ChatGPT, and GPT-4, only have publicly available APIs for language-only tasks, we release the language-only version of AGIEval."_

**What this means:**

- Original exams include images, diagrams, charts
- This version focuses on text-only questions
- Future versions may include multimodal evaluation
- Current scope: 20 tasks across diverse subjects

---

### **1.8: Experimental Scope**

**Models evaluated:**

1. **GPT-4** (closed-source, latest)
2. **ChatGPT** (closed-source, conversational)
3. **Text-Davinci-003** (closed-source, GPT-3.5 series)
4. **Vicuna-13B** (open-source, LLaMA-based)

**Evaluation settings:**

1. **Zero-shot**: No examples, just the question
2. **Few-shot**: 5 examples provided before the question
3. **Chain-of-Thought (CoT)**: Model generates reasoning steps before answering
4. **Combinations**: Zero-shot CoT, Few-shot CoT

---

### **1.9: Major Findings Preview**

**1. GPT-4 outperforms average humans on:**

- LSAT (law school admission)
- SAT (college admission)
- Math competitions

**Context**: This is **historically significant**—first time AI exceeds average human performance on such comprehensive standardized exams.

**2. Gap between GPT-4 and top human performance:**

- Average human: 67% (50th percentile)
- GPT-4: ~58-61% (depending on setting)
- Top human: 91% (99th percentile)

**Interpretation**: Models are competent but not yet at expert level.

**3. Specific weaknesses identified:**

- **Complex reasoning**: LSAT-AR (analytical reasoning), physics
- **Domain knowledge**: Law, chemistry

---

### **1.10: Qualitative Analysis Preview**

**Four-dimensional framework:**

**Understanding dimension:**

- Can models comprehend question semantics?
- Do they parse complex sentence structures correctly?
- Result: Generally strong

**Knowledge dimension:**

- Can models recall relevant facts and formulas?
- Result: Strong for math/logic, weak for specialized domains (law, chemistry)

**Reasoning dimension:**

- Can models perform multi-step logical deduction?
- Result: **Weakest dimension**—struggles with complex, multi-hop reasoning

**Calculation dimension:**

- Can models execute mathematical operations?
- Result: Variable—good for standard math, struggles with variable substitution in chemistry/biology

---

### **1.11: Research Goals**

**Three-pronged objective:**

**1. Innovation driver:** _"We aim to drive innovation in developing more effective and reliable AI assistants that advance towards Artificial General Intelligence (AGI)."_

**2. Limitation identification:** _"By identifying areas for improvement and understanding their limitations, we can enhance the models' performance."_

**3. Mechanistic understanding:** _"Gain a deeper understanding of their underlying mechanisms."_

**Ultimate vision:** _"More powerful and dependable AI systems that better serve the needs of users across a wide range of applications."_

---

### **1.12: Summary Statement**

**Closing synthesis:** _"Our research underscores the importance of evaluating foundation models in the context of human-level tasks and provides a solid benchmark for such evaluations."_

**Call to action:** _"We hope our findings inspire further innovation and progress in the development of large foundation models, ultimately leading to more reliable and effective AI systems."_

**Open science commitment:**

- Data released: https://github.com/ruixiangcui/AGIEval
- Model outputs released for all settings
- Enables reproducibility and follow-up research

---

## **SECTION 2: BACKGROUND AND RELATED WORK**

### **2.1: Large Foundation Models**

**Historical context:** _"In recent years, large foundation models, like Large Language Models (LLMs) (e.g., GPT-3, GPT-4, OPT and FLAN-T5) have successfully demonstrated unprecedented performance in a wide range of natural language tasks."_

**Key models mentioned:**

**GPT-3 (Brown et al., 2020):**

- 175 billion parameters
- First to show strong few-shot learning
- Demonstrated emergent capabilities at scale

**GPT-4 (OpenAI, 2023):**

- Size undisclosed (likely >1 trillion parameters)
- Multimodal (accepts images)
- Significant improvements in reasoning

**OPT (Zhang et al., 2022a):**

- Open-source alternative to GPT-3
- Up to 175B parameters
- Meta/Facebook's contribution to open AI

**FLAN-T5 (Chung et al., 2022):**

- Instruction-tuned version of T5
- Focuses on following natural language instructions
- Demonstrates importance of instruction tuning

---

**Success factors identified:**

**1. Deep learning advances:**

- Transformer architecture (Vaswani et al., 2017)
- Attention mechanisms
- Parallelizable training

**2. Architectural improvements:**

- Layer normalization
- Residual connections
- Efficient attention variants

**3. Massive training data:**

- Internet-scale text corpora
- Diverse domains and styles
- Trillions of tokens

---

**Current state:** _"The most recent cutting-edge language models, such as GPT-4 and ChatGPT, have continued to demonstrate substantial adaptability to a diverse array of tasks and domains and have served as a daily decision-making assistant for human beings."_

**Real-world deployment examples:**

- Customer service chatbots
- Code generation (GitHub Copilot)
- Educational tutoring
- Content creation
- Research assistance

---

**Critical concerns raised:** _"Despite their impressive performance on various benchmarks, concerns have been raised about the reasoning abilities, trustfulness and real-world applicability of these models (Marcus & Davis, 2019)."_

**Specific concerns:**

**1. Reasoning limitations:**

- May use shallow pattern matching instead of deep reasoning
- Struggle with counterfactual thinking
- Can generate plausible-sounding but incorrect logic

**2. Trustfulness issues:**

- Hallucination (generating false information confidently)
- Inconsistency across similar queries
- Lack of uncertainty quantification

**3. Real-world applicability gaps:**

- Performance degradation on out-of-distribution data
- Difficulty with tasks requiring world knowledge updates
- Context window limitations

---

**The resulting need:** _"Therefore, there is still a need for more comprehensive benchmarks that assess the general reasoning abilities of large foundation models in human-centric tasks."_

---

### **2.2: Existing Benchmarks**

**Philosophy:** _"Constructing benchmarks is a reliable way to establish evaluation standards and monitor model performance."_

---

#### **2.2.1: Single-Task Benchmarks**

**Early benchmarks focused on specific capabilities:**

**SQuAD (Rajpurkar et al., 2016):**

- **Task**: Reading comprehension and answer extraction
- **Format**: Given a passage and question, extract the answer span
- **Example**:
    - Passage: "The Eiffel Tower was completed in 1889."
    - Question: "When was the Eiffel Tower completed?"
    - Answer: "1889"
- **Significance**: Established extractive QA as a benchmark task
- **Limitation**: Tests only information retrieval, not reasoning

**SNLI (Bowman et al., 2015):**

- **Task**: Natural Language Inference
- **Format**: Given premise and hypothesis, determine relationship (entailment/contradiction/neutral)
- **Example**:
    - Premise: "A man is playing guitar."
    - Hypothesis: "A person is making music."
    - Label: Entailment
- **Significance**: Tests semantic understanding
- **Limitation**: Adversarial examples show shallow pattern matching

**Other single-task benchmarks:**

- **Fact extraction**: FEVER (Thorne et al., 2018)
- **Named entity recognition**: HIPE (Ehrmann et al., 2022)

---

#### **2.2.2: Multi-Task Benchmarks**

**BERT era motivation:** _"The emergence of general language models (LMs) like BERT (Devlin et al., 2019) has made it increasingly essential to develop more comprehensive benchmarks to assess the general capabilities of these LMs."_

**BERT's impact:**

- First major pre-trained model for NLP
- Bidirectional context understanding
- Transfer learning paradigm: pre-train on large corpus, fine-tune on specific tasks

---

**GLUE (Wang et al., 2018):** **Full name**: General Language Understanding Evaluation

**Tasks included:**

1. **CoLA**: Grammatical acceptability
2. **SST-2**: Sentiment analysis
3. **MRPC**: Paraphrase detection
4. **QQP**: Question similarity
5. **STS-B**: Semantic textual similarity
6. **MNLI**: Multi-genre natural language inference
7. **QNLI**: Question-answering NLI
8. **RTE**: Recognizing textual entailment
9. **WNLI**: Winograd schema challenge

**Format**: Aggregate score across all tasks

**Impact**: _"GLUE series benchmarks have significantly influenced language model development, encouraging researchers to enhance their models' generalization capabilities."_

**Problem**: Models quickly saturated GLUE (>90% accuracy), indicating ceiling effect.

---

**SuperGLUE (Wang et al., 2019):** **Motivation**: GLUE became too easy

**Harder tasks:**

1. **BoolQ**: Boolean yes/no questions
2. **CB**: Commitment Bank (nuanced entailment)
3. **COPA**: Causal reasoning
4. **MultiRC**: Multi-sentence reading comprehension
5. **ReCoRD**: Reading comprehension with commonsense
6. **RTE**: More difficult version
7. **WiC**: Word-in-context disambiguation
8. **WSC**: Winograd Schema Challenge (pronoun resolution)

**Significance**: Raised the bar, but models continue to improve rapidly.

---

**Other multi-task benchmarks:**

**LAMBADA (Paperno et al., 2016):**

- **Task**: Predict the last word of a passage
- **Unique feature**: Requires understanding long-range context
- **Example**: Reading several paragraphs to predict a final word that requires narrative understanding

**SentEval (Conneau & Kiela, 2018):**

- **Focus**: Evaluating sentence representations
- **Tasks**: 17 diverse tasks testing sentence embeddings
- **Use case**: Comparing different encoding methods

**DecaNLP (McCann et al., 2018):**

- **Philosophy**: Frame 10 NLP tasks as question answering
- **Goal**: Unified framework for multi-task learning

---

#### **2.2.3: Critique of Existing Benchmarks**

**Fundamental limitation:** _"Despite their broad applicability, these benchmarks mainly consist of artificially curated datasets designed to evaluate specific machine skills, rather than real-world problems aimed at assessing human behaviors."_

**Specific problems:**

**1. Artificial curation:**

- Researchers create datasets specifically for AI
- May not reflect real-world task distributions
- Often optimized for easy annotation and evaluation

**2. Focus on simple understanding:** _"Consequently, these benchmarks primarily focus on simpler textual understanding rather than complex reasoning abilities aligned with real-world applicability."_

**Examples of simplicity:**

- Extracting information explicitly stated in text
- Simple paraphrase detection
- Surface-level sentiment analysis

**What's missing:**

- Multi-step reasoning
- Integration of world knowledge
- Creative problem-solving
- Domain-specific expertise

---

#### **2.2.4: MMLU - A Step Forward**

**MMLU (Hendrycks et al., 2020):** **Full name**: Massive Multitask Language Understanding

**Key innovation:** _"MMLU addresses this issue by collecting questions from online sources covering a diverse set of subjects (e.g., history, humanities) that humans learn, pushing towards human-centric evaluation."_

**Subjects covered (57 total):**

- STEM: Math, physics, computer science, engineering
- Humanities: History, philosophy, law
- Social sciences: Psychology, sociology, economics
- Other: Medicine, business, professional skills

**Format**: Multiple-choice questions, typically 4 options

---

**Differences between MMLU and AGIEval:**

**1. Data source transparency:**

- **MMLU**: _"Data source is not explicitly mentioned, and it is unclear whether the tasks come from similarly professional and high-quality sources."_
- **AGIEval**: Uses official, standardized exams with known provenance

**Why this matters:**

- Official exams have professional question development
- Standardization ensures consistent difficulty
- Quality control through pilot testing
- Established validity and reliability

**2. Bilingual evaluation:**

- **MMLU**: English only
- **AGIEval**: English + Chinese

**Benefits of bilingualism:**

- Tests language-independent reasoning
- Evaluates cross-cultural knowledge
- More representative of global capabilities

---

### **2.3: Evaluation of Recent Large Foundation Models**

**Broader evaluation landscape:** _"The rapid development of foundation models has significantly propelled research efforts in evaluating their performance and behavior."_

**Importance statement:** _"As these models continue to grow in size and complexity, it becomes increasingly important to assess their abilities in understanding textual data and performing complex reasoning and problem-solving tasks that humans typically excel at."_

---

#### **2.3.1: Bias and Safety Evaluation**

**ToxiGen (Hartvigsen et al., 2022):**

- **Focus**: Measuring toxic language generation
- **Method**: Adversarial prompts designed to elicit biased responses
- **Importance**: Safety in deployment

**BOLD (Dhamala et al., 2021):**

- **Focus**: Bias in Open-ended Language Generation
- **Method**: Measuring demographic biases in generated text
- **Dimensions**: Race, gender, religion, political ideology

---

#### **2.3.2: Holistic Evaluation**

**Liang et al. (2022):**

- **Scale**: Dozens of language models evaluated
- **Latest model**: InstructGPT (precursor to ChatGPT)
- **Tasks**: Traditional NLP benchmarks like OpenBook QA, text classification
- **Focus**: Comprehensive behavior analysis

**Limitations noted:**

1. **Outdated models**: Doesn't cover ChatGPT or GPT-4
2. **Artificial datasets**: Uses researcher-created benchmarks, not human exams

---

#### **2.3.3: Human-Centric Scenario Evaluation**

**Emerging trend:** _"Recognizing this limitation, recent reports have emphasized the importance of evaluations in human-centric scenarios."_

**Choi et al. (2023):**

- **Study**: ChatGPT Goes to Law School
- **Method**: Essay questions from law school exams
- **Finding**: ChatGPT achieves C+ average

**Bubeck et al. (2023):**

- **Title**: Sparks of Artificial General Intelligence: Early Experiments with GPT-4
- **Focus**: Exploring emergent capabilities of GPT-4
- **Finding**: GPT-4 shows qualitative improvements in reasoning

**OpenAI (2023) - Official GPT-4 Technical Report:**

- **Content**: Performance on human exams (LSAT, SAT, etc.)
- **Finding**: GPT-4 scores in 90th percentile on many standardized tests

---

#### **2.3.4: Gaps in Existing Evaluation**

**Critical problems identified:** _"However, the relevant benchmarks in these reports and the corresponding model outputs are not publicly available, and the evaluation metric is also not transparent."_

**Specific issues:**

**1. Lack of public data:**

- Cannot reproduce results
- Cannot verify claims
- Cannot build on findings

**2. Opaque methodology:**

- Evaluation metrics not specified
- Prompting strategies unclear
- Selection criteria for questions unknown

**3. Limited comparability:**

- Different studies use different subsets of questions
- Inconsistent reporting of results
- Hard to compare across models

---

**AGIEval's response:** _"To bridge this gap, we propose AGIEval, which releases collected questions from high-standard, official human exams and supports a standardized, automatic evaluation metric."_

**Key contributions:**

1. **Public dataset**: 8,062 questions available on GitHub
2. **Standardized metrics**: Accuracy for multiple-choice, EM/F1 for fill-in-blank
3. **Released outputs**: All model responses for ChatGPT, GPT-4, etc.
4. **Reproducibility**: Clear experimental setup and prompts

**Community benefit:** _"Furthermore, we conduct comprehensive experiments with the most recent LLMs including ChatGPT and GPT-4 and release the model outputs to foster analysis and research within the entire community."_

---

## **SECTION 3: HUMAN-CENTRIC BENCHMARK CONSTRUCTION**

### **3.1: Design Principles**

#### **3.1.1: Emphasis on Human-Level Cognitive Tasks**

**Primary objective:** _"In designing the human-centric benchmark, our primary objective is to center on tasks that closely align with human cognition and problem-solving, ultimately assessing the general abilities of foundation models in a more meaningful and comprehensive manner."_

**Implementation strategy:** _"To achieve this, we incorporate a diverse range of official, public, and high-standard admission and qualification exams that cater to general human test-takers."_

**Types of exams included:**

1. **College admission tests**: SAT, Gaokao
2. **Law school admission**: LSAT
3. **Math exams**: AMC, AIME
4. **Professional qualification**: Lawyer bar exam
5. **Civil service exams**: Government employment tests

---

**Why these exams represent human cognition:**

**Scale of participation:** _"These exams...are taken by millions of individuals seeking entry into higher education or new career paths."_

**Specific numbers:**

- Gaokao: 12 million annually
- SAT: 1.7 million annually
- LSAT: 170,000 annually
- Lawyer qualification: 820,000 annually
- Civil service: 2 million annually
- AMC: 300,000 annually
- AIME: 3,000 annually (top AMC performers)

**Societal validation:** _"By adhering to these officially recognized standards for evaluating human-level capabilities, our benchmark design ensures that the assessment of model performance is directly relevant to human decision-making and cognitive abilities."_

**What "officially recognized" means:**

- Government-sanctioned or professionally administered
- Established through decades of psychometric research
- Validated against real-world outcomes (college success, professional competence)
- Standardized administration and scoring procedures

---

#### **3.1.2: Relevance to Real-World Scenarios**

**Guiding principle:** _"The second design principle guiding the construction of our human-centric benchmark is the emphasis on tasks that hold significant implications for real-world situations."_

**Selection criteria:** _"By selecting tasks derived from high-standard admission and qualification exams, we ensure that the assessment reflects the complexity and practicality of challenges that individuals commonly encounter in various fields and contexts."_

---

**Real-world relevance dimensions:**

**1. Complexity:**

- Multi-step problem-solving
- Integration of multiple knowledge domains
- Time pressure management
- Ambiguity handling

**2. Practicality:**

- Tasks mirror professional requirements
- Skills transfer to actual work
- Predictive of real-world performance

**3. Authenticity:**

- Questions from actual exams, not simulations
- Reflect genuine challenges people face
- Have real consequences for test-takers

---

**Dual benefit:** _"This approach allows us to not only gauge the models' performance in relation to human cognitive abilities but also better understand their applicability and effectiveness in real-life scenarios."_

**Two evaluation dimensions:**

1. **Descriptive**: How do models compare to humans?
2. **Predictive**: Will models succeed in real deployment?

**Long-term goal:** _"Consequently, our benchmark design contributes to the development of AI systems that are more reliable, practical, and well-suited for addressing a wide range of real-world problems."_

---

### **3.2: Exam Selection - Detailed Breakdown**

**Selection philosophy:** _"In line with the design principles outlined above, we have selected a diverse range of standardized, high-quality exams that emphasize human-level reasoning and real-world relevance."_

**Scale emphasis:** _"Some exams are participated by millions of human test-takers annually. For example, 12 millions of students participate in Gaokao every year."_

**Data transparency:** _"Statistics of annual human participants are reported in Tab. 1."_

---

#### **3.2.1: General College Entrance Exams**

**Purpose and scope:** _"These exams, include Graduate Record Examinations (GRE), Scholastic Assessment Test (SAT) and China College Entrance Exam (Gaokao), are designed to assess the general aptitude and subject-specific knowledge of students seeking entry into higher education institutions."_

**Cognitive skills tested:** _"College admission tests encompass various subjects and require critical thinking, problem-solving, and analytical skills, making them ideal for evaluating the performance of large language models in relation to human cognition."_

---

**Specific exams and subjects:**

**Chinese Gaokao:** _"We collect exams corresponding to 8 subjects from Chinese Gaokao:"_

1. **History (GK-history)**:
    
    - Historical events and their causes
    - Chronological reasoning
    - Source analysis
2. **Math (GK-Math-QA, GK-Math-Cloze)**:
    
    - Multiple-choice math problems
    - Fill-in-the-blank calculations
    - Applied mathematics
3. **English (GK-En)**:
    
    - Reading comprehension
    - Grammar and usage
    - Vocabulary in context
4. **Chinese (GK-Ch)**:
    
    - Classical Chinese literature
    - Modern Chinese comprehension
    - Writing analysis
5. **Geography (GK-geography)**:
    
    - Physical geography
    - Human geography
    - Spatial reasoning
6. **Biology (GK-biology)**:
    
    - Cellular biology
    - Genetics
    - Ecology
7. **Chemistry (GK-chemistry)**:
    
    - Chemical equations
    - Reaction mechanisms
    - Stoichiometry
8. **Physics (GK-physics)**:
    
    - Mechanics
    - Electromagnetism
    - Thermodynamics

---

**American SAT:** _"We select mathematical questions from GRE, select English and math subjects from SAT to construct the benchmark."_

**SAT-Math:**

- **Content**: Algebra, geometry, trigonometry, data analysis
- **Format**: Multiple-choice and grid-in questions
- **Skills**: Problem-solving, mathematical reasoning

**SAT-English:**

- **Content**: Reading comprehension passages
- **Passage types**: Literature, history/social studies, science
- **Skills**: Evidence-based reading, rhetorical analysis

---

#### **3.2.2: Law School Admission Test (LSAT)**

**Purpose:** _"Law school admission tests, such as the LSAT, are intended to measure the reasoning and analytical skills of prospective law students."_

**Section breakdown:** _"These tests include sections on logical reasoning, reading comprehension, and analytical reasoning, which challenge the test-takers' ability to analyze complex information and draw accurate conclusions."_

---

**Three LSAT sections in AGIEval:**

**LSAT-AR (Analytical Reasoning / "Logic Games"):**

- **Format**: Set of conditions/rules, then questions about scenarios
- **Example structure**:
    - "Seven people (A-G) must be assigned to three teams..."
    - Rules: "If A is on team 1, then B cannot be on team 2..."
    - Questions: "Which arrangement is possible?"
- **Skills**: Diagramming, constraint satisfaction, deductive reasoning

**LSAT-LR (Logical Reasoning):**

- **Format**: Short argument followed by question
- **Question types**:
    - Strengthen/weaken the argument
    - Identify assumptions
    - Find logical flaws
    - Parallel reasoning
- **Skills**: Argument analysis, logical structure identification

**LSAT-RC (Reading Comprehension):**

- **Format**: Dense passages (law, science, humanities) with questions
- **Passage length**: 400-500 words
- **Question types**: Main idea, inference, author's purpose, detail questions
- **Skills**: Complex text comprehension, synthesis

---

**Value for evaluation:** _"Incorporating these tasks in our benchmark enables us to assess language models' capabilities in legal reasoning and analysis."_

**Why legal reasoning matters:**

- Tests abstract thinking
- Requires precise logical deduction
- Involves complex conditional statements
- Mirrors professional analytical work

---

#### **3.2.3: Lawyer Qualification Test**

**Professional certification context:** _"Lawyer qualification tests, such as the bar exam, assess the legal knowledge, analytical skills, and ethical understanding of individuals pursuing a career in law."_

**Content scope:** _"These exams cover a broad range of legal topics, including constitutional law, contract law, criminal law, and property law, and require candidates to demonstrate their ability to apply legal principles and reason effectively."_

---

**Two JEC-QA tasks:**

**JEC-QA-KD (Knowledge-Driven):**

- **Focus**: Legal knowledge recall
- **Question type**: "According to the law, which statement is correct?"
- **Skills**: Memorization of legal principles, statute interpretation

**JEC-QA-CA (Case Analysis):**

- **Focus**: Applying law to scenarios
- **Question type**: "Given this situation, what legal outcome follows?"
- **Skills**: Case reasoning, legal principle application

---

**Source:** _"Specifically, we select questions from previous Chinese lawyer qualification tests to build the benchmark."_

**Why bar exams matter:** _"By incorporating lawyer qualification tests in our benchmark, we can evaluate language models' performance in the context of professional legal expertise and ethical judgment."_

**Real-world application:**

- Legal AI assistants
- Contract review systems
- Case law analysis tools

---

#### **3.2.4: Graduate Management Admission Test (GMAT)**

**Purpose:** _"The GMAT is a standardized exam designed to assess the analytical, quantitative, verbal, and integrated reasoning skills of prospective graduate business school students."_

**Section structure:** _"The GMAT consists of sections such as Analytical Writing Assessment, Integrated Reasoning, Quantitative Reasoning, and Verbal Reasoning, which evaluate the test-takers' abilities to think critically, analyze data, and communicate effectively."_

---

**AGIEval inclusion:** From the AQuA-RAT dataset (Algebraic Question Answering with Rationales):

- **Content**: GMAT and GRE quantitative reasoning questions
- **Focus**: Algebraic word problems
- **Format**: Multiple-choice with worked solutions

**Example question type:** "A train travels 120 km in 2 hours. If it increases its speed by 20 km/h, how long will it take to travel 180 km?"

**Skills tested:**

- Quantitative reasoning
- Data analysis
- Problem-solving under time pressure

---

**Business context value:** _"Incorporating GMAT tasks in our benchmark enables us to assess the language models' performance in a business context and their potential to assist in decision-making and problem-solving in management scenarios."_

---

#### **3.2.5: High School Math Competitions**

**Competition description:** _"High school math competitions, such as the American Mathematics Competitions (AMC) and the American Invitational Mathematics Examination (AIME), challenge students' mathematical abilities, creativity, and problem-solving skills."_

**Mathematical breadth:** _"These contests cover a wide range of mathematical topics, including number theory, algebra, geometry, and combinatorics, and often feature unconventional problems that require inventive approaches to solve."_

---

**What makes competition math unique:**

**1. Creative problem-solving:**

- No standard algorithms
- Requires insight and "aha moments"
- Multiple solution paths possible

**2. Mathematical maturity:**

- Beyond rote computation
- Requires understanding of deep principles
- Tests mathematical intuition

---

**Competition structure:**

**AMC (American Mathematics Competitions):**

- **Levels**: AMC 8 (middle school), AMC 10, AMC 12
- **Format**: 25 multiple-choice questions, 75 minutes
- **Difficulty**: Ranges from moderate to very challenging
- **Participants**: ~300,000 annually

**AIME (American Invitational Mathematics Examination):**

- **Qualification**: Top AMC scorers only (~3,000 students)
- **Format**: 15 questions, answers are integers 0-999
- **Difficulty**: Very challenging
- **Time**: 3 hours

---

**Dataset source:** _"For high school math competitions, we employ data from the MATH dataset (Hendrycks et al.), comprising questions from AMC and AIME."_

**Sampling:** _"We down-sample...MATH to 1,000 instances each."_

---

**Evaluation value:** _"By incorporating tasks from high school math competitions into our benchmark, we can further evaluate the language models' aptitude for tackling complex and creative mathematical problems, as well as their ability to generate novel solutions."_

**Why competition math matters for AI:**

- Tests true mathematical reasoning, not pattern matching
- Requires creative thinking
- Goes beyond memorized formulas
- Proxy for general problem-solving ability

---

#### **3.2.6: Chinese Civil Service Examination**

**Purpose and scope:** _"The Chinese Civil Service Examination is a standardized test administered in China to assess the aptitude and skills of individuals seeking entry into the civil service."_

**Competencies evaluated:** _"These exams evaluate a range of competencies, such as general knowledge, reasoning abilities, language skills, and subject-specific expertise related to the roles and responsibilities of various civil service positions in China."_

---

**LogiQA dataset:** _"For Chinese civil service examinations, we repurpose data from LogiQA (Liu et al., 2021), a dataset built on various types of logical reasoning questions collected from the National Civil Servants Examination of China."_

**Bilingual nature:** _"It is worth noting that LogiQA consists of bilingual questions (English and Chinese), where the English version is a translated version of the original Chinese version."_

**Two versions in AGIEval:**

- **LogiQA-en**: English translation
- **LogiQA-ch**: Original Chinese

---

**Question types in LogiQA:**

1. **Deductive reasoning**: Given premises, what must be true?
2. **Inductive reasoning**: What conclusion best fits the evidence?
3. **Abductive reasoning**: What best explains the observations?
4. **Assumption identification**: What must be assumed for an argument to work?
5. **Logical flaw identification**: What's wrong with this reasoning?

---

**Evaluation significance:** _"Including tasks from national civil service exams in our benchmark allows us to gauge the language models' performance in public administration contexts and their potential to contribute to policy-making, decision-making, and public service delivery processes."_

**Real-world applications:**

- Policy analysis systems
- Administrative decision support
- Government service chatbots
- Document processing and analysis

---

**Summary statement:** _"By including these tasks in our human-centric benchmark, we create a comprehensive assessment that effectively measures the performance of foundation models in various real-world, human-level reasoning scenarios."_

---

### **3.3: Construction of Human-Centric Benchmark**

#### **3.3.1: Data Collection - Detailed Process**

**Source diversity:** _"As previously mentioned, our human-centric benchmark comprises questions from a diverse range of official and high-quality exams, originally designed for human test-takers."_

**Complete list of sources:**

- General college admission: GRE, Gaokao, SAT
- Major-specific entrance: LSAT, GMAT
- Math competitions: AMC, AIME
- Professional qualification: Chinese lawyer bar exam
- Civil service: Chinese National Civil Servants Examination

---

**Handling subjective questions:** _"Since evaluating model performance on subjective questions is challenging without human expert scoring, we believe such questions are unsuitable for inclusion in this benchmark for consistent assessment."_

**Rationale:**

- Subjective evaluation introduces variability
- Requires expensive expert annotation
- Hard to ensure consistency across evaluators
- Difficult to scale

**Solution:** _"To ensure a robust and standardized evaluation metric, we have removed all subjective questions, retaining only objective ones, such as multiple-choice and fill-in-the-blank questions."_

**Types retained:**

1. **Multiple-choice**: Select A, B, C, or D
2. **Fill-in-the-blank**: Provide exact numeric or short text answer

---

**Data collection by source:**

**Gaokao and SAT:** _"We gather Gaokao and SAT questions from publicly available online sources, along with their corresponding solutions or explanations."_

**Sources include:**

- Official practice tests
- Released previous exams
- Educational websites
- Official test preparation materials

---

**LSAT:** _"For the LSAT, we utilize data from Wang et al. (2022) and Zhong et al. (2022), which encompasses three tasks (logical reasoning, reading comprehension, and analytical reasoning) from the LSAT administered between 1991 and 2016."_

**Why this range:**

- Sufficient historical data
- Questions remain relevant
- Diverse question styles across years
- Released publicly by LSAC (Law School Admission Council)

---

**LogiQA:** _"For Chinese civil service examinations, we repurpose data from LogiQA (Liu et al., 2021)."_

**Bilingual provision:** _"It is worth noting that LogiQA consists of bilingual questions (English and Chinese), where the English version is a translated version of the original Chinese version."_

**Translation quality**: Professional translation maintaining logical structure and meaning.

---

**MATH dataset:** _"For high school math competitions, we employ data from the MATH dataset (Hendrycks et al.), comprising questions from AMC and AIME."_

**Dataset characteristics:**

- 12,500 problems originally
- Organized by difficulty level
- Includes step-by-step solutions
- Covers competition mathematics

---

**AQuA-RAT:** _"Furthermore, we incorporate GRE and GMAT questions from AQaA-RAT (Ling et al., 2017), which emphasizes algebraic word problems."_

**Dataset features:**

- 100,000 algebraic word problems
- Includes natural language rationales (explanations)
- Crowdsourced solutions
- Focus on quantitative reasoning

---

**JEC-QA:** _"In the case of the Chinese Civil Service Examination, we reuse instances from JEC-QA (Zhong et al., 2020), a large-scale dataset derived from the National Judicial Examination of China."_

**Sampling decision:** _"We down-sample the two types of JEC-QA and MATH to 1,000 instances each."_

**Rationale for downsampling:**

- Balance across different exam types
- Computational constraints for evaluation
- Sufficient statistical power
- Manageable for qualitative analysis

---

**Final dataset statistics:** _"As a result, we construct a benchmark consisting of 8,062 questions for evaluation."_

**Distribution shown in Table 1:**

|Exam Category|Tasks|Total Questions|
|---|---|---|
|Gaokao|9 subjects|2,280|
|SAT|2 subjects|426|
|JEC-QA|2 types|2,000|
|LSAT|3 sections|1,000|
|LogiQA|2 languages|1,302|
|GMAT/GRE|1 (AQuA)|254|
|AMC/AIME|1 (MATH)|1,000|
|**Total**|**20 tasks**|**8,062**|

---

**Bilingual design:** _"Detailed data statistics are presented in Table 1. It is worth noting that our benchmark is bilingual, encompassing both English and Chinese tests."_

**Language distribution:**

- **English-only tasks**: SAT, LSAT, MATH, AQuA-RAT, LogiQA-en
- **Chinese-only tasks**: Gaokao (all subjects), JEC-QA, LogiQA-ch
- **Approximate split**: ~60% Chinese, ~40% English

**Design benefit:** _"This design enables the evaluation of a broader scope of model capabilities, reflecting their performance and adaptability across different languages."_

---

#### **3.3.2: Evaluation Metrics**

**Question format distribution:** _"The benchmark questions consist of objective formats: multiple-choice and fill-in-the-blank questions."_

---

**Multiple-choice questions:** _"For multiple-choice questions, we adopt standard classification accuracy as the evaluation metric."_

**Calculation:**

```
Accuracy = (Number of correct answers) / (Total number of questions)
```

**Properties:**

- Simple and interpretable
- Standard in educational assessment
- Allows direct comparison with human performance
- No partial credit

**Percentage of dataset**: ~85% multiple-choice

---

**Fill-in-the-blank questions:** _"For fill-in-the-blank questions, we employ Exact Match (EM) and F1 metrics."_

**Exact Match (EM):**

- **Definition**: Answer must match ground truth exactly
- **Calculation**: Binary (0 or 1) for each question
- **Use case**: When precise answers are required (e.g., numerical values)

**F1 Score:**

- **Definition**: Harmonic mean of precision and recall at token level
- **Calculation**:
    - Precision = (tokens in both prediction and ground truth) / (tokens in prediction)
    - Recall = (tokens in both prediction and ground truth) / (tokens in ground truth)
    - F1 = 2 × (Precision × Recall) / (Precision + Recall)
- **Use case**: When partial credit makes sense (e.g., short text answers)

**Example:**

- Ground truth: "photosynthesis"
- Prediction: "photosynthesis process"
- EM: 0 (not exact match)
- F1: 0.67 (partial credit for shared token)

**Percentage of dataset**: ~15% fill-in-blank (mostly in Gaokao Math-Cloze)

---

#### **3.3.3: Human Performance**

**Importance of human baselines:** _"It is worth noting that we also report the average and top performances of human test-takers as human-level boundaries for each task."_

**Two performance levels:**

**1. Average Human Performance (50th percentile):**

- Represents typical test-taker
- Median score across all participants
- Baseline for "human-level" capability

**2. Top Human Performance (99th percentile for most, 90th for some):**

- Represents high achievers
- Goal for AI systems aspiring to expert-level performance
- Upper bound for comparison

---

**Data sources for human performance:**

**From original publications:** _"The human performance results for MATH, LogiQA, and JEC-QA were gathered from their respective publications."_

**These datasets include:**

- MATH: Human solver accuracy from original paper
- LogiQA: Annotator agreement and performance
- JEC-QA: Professional performance data

---

**Estimated from score distributions:** _"While for AQuA-RAT (GMAT, GRE), LSAT, SAT, and Gaokao, we estimated the human performances by scaling the scores of the average (50%) and top (1%) test takers against the full scores to 100."_

**Estimation methodology:**

**Example for SAT Math:**

1. Full SAT Math score range: 200-800
2. Average (50th percentile) score: ~528
3. Top (99th percentile) score: ~760
4. Scaling to 100:
    - Average: (528-200)/(800-200) × 100 ≈ 55%
    - Top: (760-200)/(800-200) × 100 ≈ 93%

**Similar process for:**

- LSAT: Using official LSAC percentile tables
- Gaokao: Using published provincial cutoff scores
- GRE/GMAT: Using ETS and GMAC score reports

---

**Important disclaimer:** _"It is important to note that while our datasets and human accuracies provide a useful approximation of human test-taker ability, they do not fully represent the range of skills and knowledge that such individuals may possess."_

**Limitations to consider:**

**1. Test-specific skills:**

- Humans may have test-taking strategies not reflected in scores
- Time management skills
- Guessing strategies

**2. Contextual factors:**

- Test anxiety affects human performance
- Models don't experience stress
- Humans may underperform due to external factors

**3. Sample bias:**

- Test-takers are self-selected population
- May not represent general population
- Varies by exam (e.g., AIME only top math students)

**4. Incomplete representation:**

- Tests measure specific subset of intelligence
- Don't capture all human cognitive capabilities
- Practical skills vs. test performance may differ

---

### **Table 1 Analysis: Detailed Statistics**

**Table 1 breakdown** (complete data from paper):

|Exam|# Participants|Language|Task|Subject|# Instances|Avg. Tokens|
|---|---|---|---|---|---|---|
|**Gaokao**|12M|Chinese|GK-geography|Geography|199|144|
||||GK-biology|Biology|210|141|
||||GK-history|History|243|116|
||||GK-chemistry|Chemistry|207|113|
||||GK-physics|Physics|200|124|
||||GK-En|English|306|356|
||||GK-Ch|Chinese|246|935|
||||GK-Math-QA|Math|351|68|
||||GK-Math-Cloze|Math|118|60|
|**SAT**|1.7M|English|SAT-En|English|206|656|
||||SAT-Math|Math|220|54|
|**Lawyer Qual.**|820K|Chinese|JEC-QA-KD|Law|1000|146|
||||JEC-QA-CA|Law|1000|213|
|**LSAT**|170K|English|LSAT-AR|Law-Analytics|230|154|
||||LSAT-LR|Law-Logic|510|178|
||||LSAT-RC|Law-Reading|260|581|
|**Civil Service**|2M|English|LogiQA-en|Logic|651|144|
|||Chinese|LogiQA-ch|Logic|651|242|
|**GRE/GMAT**|340K/150K|English|AQuA-RAT|Math|254|77|
|**AMC/AIME**|300K/3K|English|MATH|Math|1000|40|

---

**Key insights from Table 1:**

**1. Participation scale:**

- Gaokao is largest (12M), showing huge social importance in China
- Even "small" exams like LSAT have 170K participants annually
- Total annual participants across all exams: ~17 million people

**2. Token length variations:**

- **Shortest**: MATH (40 tokens average) - concise competition problems
- **Longest**: GK-Ch (935 tokens average) - Chinese literature passages
- **Reading-heavy**: SAT-En (656), LSAT-RC (581) - long passages

**Implications for models:**

- Need strong reading comprehension for long-context tasks
- Must handle both brief and extended reasoning
- Context window must accommodate longest questions

**3. Language distribution:**

- Chinese-only: 11 tasks (5,676 questions)
- English-only: 7 tasks (2,081 questions)
- Bilingual (both versions): 2 tasks (1,302 questions)

**4. Subject diversity:**

- **STEM**: Math (4 tasks), Physics, Chemistry, Biology, Geography
- **Humanities**: History, Chinese literature, English
- **Professional**: Law (5 tasks), Logic (2 tasks)

**5. Question volume balance:**

- Largest single task: GK-Math-QA (351 questions)
- Smallest single task: GK-geography (199 questions)
- Most tasks: 200-300 questions (sweet spot for evaluation)

---

## **SECTION 4: EVALUATION OF FOUNDATION MODELS**

### **4.1: Foundation Models Selected for Evaluation**

**Introduction:** _"In this section, we evaluate the performance of various state-of-the-art language models on our benchmark dataset. The selected models represent a diverse range of capabilities and are widely recognized in the field."_

---

#### **4.1.1: GPT-4**

**Model description:** _"The fourth-generation model in the GPT series, GPT-4 is a state-of-the-art, large-scale generative pre-trained transformer model with enhanced performance and a broader knowledge base compared to its predecessors."_

**Performance characterization:** _"While it does not surpass human capabilities in many real-world scenarios, it demonstrates human-level performance on many scenarios."_

**What this means:**

- Not superhuman across the board
- Achieves human parity on specific tasks
- Represents current frontier of AI capability

---

**Training methodology:** _"GPT-4 has been iteratively aligned using insights from adversarial testing and ChatGPT, resulting in remarkable improvements in factuality."_

**Iterative alignment explained:**

**1. Adversarial testing:**

- Researchers try to make GPT-4 generate false information
- Identify failure modes
- Retrain to fix those specific failures
- Iterate multiple times

**2. Insights from ChatGPT:**

- ChatGPT served as a testbed
- User interactions revealed common failure patterns
- Feedback loops from RLHF (Reinforcement Learning from Human Feedback)
- Applied lessons to GPT-4

**3. Factuality improvements:**

- Reduced hallucination rate
- Better citation of sources
- More accurate knowledge recall
- Appropriate uncertainty expression ("I don't know" when appropriate)

---

**Capabilities (from OpenAI technical report):**

- Multimodal (accepts images, though AGIEval uses text-only)
- Longer context window (32K tokens in some variants)
- Better instruction following
- Improved reasoning on complex tasks

---

#### **4.1.2: ChatGPT**

**Model description:** _"A conversational AI model developed by OpenAI, ChatGPT is designed to engage in interactive and dynamic conversations."_

**Training process:** _"It has been trained on a vast instruction dataset and further tuned by reinforcement learning with human feedbacks (RLHF), enabling it to provide contextually relevant and coherent responses aligned with human expectation."_

---

**RLHF process breakdown:**

**Step 1: Supervised Fine-Tuning (SFT)**

- Human AI trainers write example conversations
- Model trained to mimic these high-quality responses
- Covers diverse topics and conversation styles

**Step 2: Reward Model Training**

- Trainers rank multiple model outputs for same prompt
- Train reward model to predict human preferences
- Captures nuanced quality judgments

**Step 3: Proximal Policy Optimization (PPO)**

- Model generates responses
- Reward model scores them
- Policy updated to maximize expected reward
- Balances helpfulness, harmlessness, honesty

---

**ChatGPT characteristics:**

- Conversational style (friendly, helpful tone)
- Refuses inappropriate requests
- Admits mistakes and limitations
- Asks clarifying questions
- Based on GPT-3.5 architecture

---

**Distinction from GPT-4:**

- Earlier model (released November 2022)
- Less capable on complex reasoning
- Faster inference (smaller model)
- Still highly capable on many tasks

---

#### **4.1.3: Text-Davinci-003**

**Model position:** _"GPT-3.5 is an intermediate version between GPT-3 and GPT-4, offering improved performance over GPT-3 through further instruction tuning."_

**Bridge characterization:** _"It serves as a bridge between the two models, allowing for comparative analysis."_

---

**GPT-3.5 family:**

- **text-davinci-001**: Early instruction-tuned GPT-3
- **text-davinci-002**: Improved instruction following
- **text-davinci-003**: Final GPT-3.5 series model (best of family)
- **ChatGPT**: Conversational variant of GPT-3.5

---

**Selection rationale:** _"Specifically, we select the variant Text-Davinci-003 from GPT-3.5 series for evaluation."_

**Why this variant:**

- Most capable GPT-3.5 model
- Publicly available via API
- Well-documented performance
- Allows comparison with both GPT-3 and ChatGPT

---

**Text-Davinci-003 characteristics:**

- Instruction-tuned but not conversational
- Better at following complex prompts
- More "direct" responses (less chatty than ChatGPT)
- Uses Completion API (not Chat API)

---

#### **4.1.4: Vicuna-13B**

**Open-source representative:** _"It is an open-source LLM, trained on user-shared conversations from ShareGPT by fine-tuning LLaMA."_

**Performance claim:** _"It achieves over 90% of the quality of OpenAI's ChatGPT."_

**Benchmark performance:** _"As of May 2023, Vicuna-13B ranks among the top models on the Open LLM Leaderboard (Beeching et al., 2023)."_

---

**Training methodology:**

**Base model: LLaMA-13B**

- Meta's open-source foundation model
- 13 billion parameters
- Trained on publicly available data
- Strong baseline performance

**Fine-tuning data: ShareGPT**

- User-shared ChatGPT conversations
- ~70K conversations
- Diverse topics and styles
- Curated for quality

**Fine-tuning process:**

- Supervised fine-tuning on conversations
- Optimized for helpfulness
- Attempts to replicate ChatGPT behavior

---

**Significance for AGIEval:** _"Vicuna, despite excelling on OpenLLM leaderboard and its claimed comparable ability with ChatGPT, falls short on AGIEval, highlighting the valuable challenges our benchmark presents to open-source models."_

**Performance gap:**

- Vicuna-13B average: ~27% on AGIEval
- ChatGPT average: ~43-45% on AGIEval
- Gap: ~15-18 percentage points

**Implications:**

- Open LLM Leaderboard may not capture human-centric reasoning
- Significant gap between open-source and proprietary models
- AGIEval provides complementary evaluation

---

#### **4.1.5: Additional Models (Mentioned)**

**Note on expanded evaluation:** _"A recent technical report (SenseTime, 2023) further evaluates foundation models (GLM130B, LLaMa-65B, and InternLM-104B) on AGIEval."_

**These models:**

- **GLM130B**: Chinese-focused model, 130B parameters
- **LLaMa-65B**: Larger version of Meta's LLaMA
- **InternLM-104B**: SenseTime's proprietary model

**Why not in main paper:**

- Published after initial AGIEval release
- Focus on most widely-used models
- Demonstrates benchmark adoption by community

---

### **4.2: Experimental Setup**

**Introduction:** _"In this section, we describe the experimental setup used to evaluate the performance of large language models, including GPT-4, ChatGPT and Text-Davinci-003, on our human-centric reasoning tasks benchmark."_

**Evaluation dimensions:** _"To gauge the adaptability of the language models, we conducted two types of evaluations: zero-shot and few-shot. We further implement a 'Chain-of-Thought' reasoning evaluation for both zero-shot and few-shot learning."_

---

**Transparency commitment:** _"To foster analyses about models' behavior and foster relevant research, we release the model outputs under all the settings."_

**What's released:**

- All model predictions
- Intermediate reasoning steps (for CoT)
- Prompts used for each task
- Exact parameter settings

**Research enablement:**

- Error analysis possible
- Failure mode identification
- Prompt engineering insights
- Benchmarking new models

---

#### **4.2.1: Zero-shot and Few-shot Evaluation**

**Zero-shot setting:** _"In the zero-shot setting, models were directly evaluated on the questions without being provided any prior examples of the specific tasks."_

**What this tests:** _"This scenario tests the models' innate ability to reason and solve problems without explicit training."_

---

**Zero-shot characteristics:**

**1. No task-specific examples:**

- Model sees question for first time
- No demonstration of expected format
- Relies on pre-training knowledge only

**2. Instruction-based:**

- Relies on natural language task description
- E.g., "Answer the following question by selecting A, B, C, or D"
- Tests instruction-following capability

**3. Purest test of generalization:**

- No opportunity to learn from examples
- True "out-of-the-box" performance
- Most similar to human taking unfamiliar test

---

**Few-shot setting:** _"In the few-shot setting, models were given a small number of examples (e.g., 5) from the same task before being evaluated on the test samples."_

**What this tests:** _"This evaluation setup tests the models' ability to quickly adapt and learn from limited examples, simulating real-world scenarios where direct supervision may be scarce."_

---

**Few-shot characteristics:**

**1. Small number of examples (K=5):**

- Not enough for traditional machine learning
- Tests rapid adaptation ("meta-learning")
- Simulates human-like quick learning

**2. In-context learning:**

- Examples provided in the prompt itself
- No gradient updates (model weights unchanged)
- Learning happens through attention mechanism

**3. Format clarification:**

- Shows expected answer format
- Demonstrates reasoning style
- Reduces ambiguity

---

**Comparison between settings:**

|Aspect|Zero-shot|Few-shot|
|---|---|---|
|Examples provided|0|5|
|Model updates|None|None|
|Context length|Shorter|Longer|
|Human analogy|Taking unfamiliar test|Quick study before test|
|Difficulty|Harder|Easier (usually)|

---

#### **4.2.2: Chain-of-Thought (CoT) Reasoning**

**Background citation:** _"Chain-of-Thought (CoT) prompting (Wei et al., 2022) enables large language models to break down a complex question to a series of decomposed reasoning steps."_

**Core idea:**

- Instead of directly answering, model first generates reasoning
- Mimics human "showing your work"
- Enables step-by-step problem solving

---

**Implementation:** _"To further probe the models' reasoning capabilities, we implemented a 'Chain-of-Thought' reasoning evaluation. This method involves asking the model to first generate an explanation for the given question, and subsequently, answer the question based on its self-generated explanation."_

---

**Two-step process (Zero-shot CoT):**

**Step 1: Rationale Generation** _"In the first step, the model is prompted with '[question] Let's think step by step:' to generate a detailed explanation of its understanding of the problem and the reasoning process required to solve it (Zhang et al., 2022b)."_

**Example prompt:**

```
Question: If John has 3 apples and Mary has 2 more than John, how many does Mary have?
Let's think step by step:
```

**Model generates:**

```
First, we need to find how many apples John has: 3 apples.
Then, Mary has 2 more than John: 3 + 2 = 5 apples.
So Mary has 5 apples.
```

---

**Step 2: Answer Generation** _"In the second step, the model is prompted with the generated explanation '[question] Explanation is: [explanation]. The answer is:', and provide an answer to the question, taking into account its self-generated explanation from the first step."_

**Example prompt:**

```
Question: If John has 3 apples and Mary has 2 more than John, how many does Mary have?
Explanation is: First, we need to find how many apples John has: 3 apples. Then, Mary has 2 more than John: 3 + 2 = 5 apples. So Mary has 5 apples.
The answer is:
```

**Model generates:**

```
5
```

---

**Purpose of two-step process:** _"This process aimed to test the model's ability to use its own reasoning to derive a coherent and accurate solution, simulating the way humans often rely on their understanding and internal reasoning processes to solve problems."_

**Benefits:**

1. **Decomposition**: Breaks complex problems into steps
2. **Self-correction**: Model can catch errors in its reasoning
3. **Transparency**: Explanations reveal model's thought process
4. **Accuracy boost**: Often improves performance

---

**Few-shot CoT variation:** _"Specially, as shown in Fig. 2, in the few-shot CoT setting, the explanation and answer are generated in the same step."_

**Difference from zero-shot CoT:**

- Examples include both reasoning and answers
- Single forward pass instead of two
- Model learns CoT format from examples

**Example few-shot CoT prompt:**

```
Here are the answers for the questions in exams.
Q1: If 2x + 3 = 7, what is x?
Explanation: We need to isolate x. First, subtract 3 from both sides: 2x = 4. Then divide by 2: x = 2.
The answer is 2.

Q2: If 3x - 5 = 10, what is x?
Explanation: Add 5 to both sides: 3x = 15. Then divide by 3: x = 5.
The answer is 5.

[... 3 more examples ...]

Q: If 4x + 1 = 9, what is x?
```

---

#### **4.2.3: Figure 2 Analysis - Prompting Examples**

**Figure 2 shows four prompting strategies:**

---

**1. Zero-shot Prompting (top left):**

```
Input: [Question]
Among A to D, the answer is:
Output: <Answer>
```

**Characteristics:**

- No examples
- Direct question-to-answer
- Minimal prompting
- Relies on model's pre-training

---

**2. Few-shot Prompting (bottom left):**

```
Input: Here are the answers for the questions in exams.
Q1: [Question 1] The answer is [Answer 1]
Q2: [Question 2] The answer is [Answer 2]
...
Qn: [Question n] The answer is [Answer n]
[Question n+1]

Output: The answer is: <Answer>
```

**Characteristics:**

- Shows answer format
- Demonstrates expected response style
- No reasoning shown
- Quick pattern learning

---

**3. Zero-shot CoT Prompting (top right):**

**Step 1: Rationale Generation**

```
Input: [Question]
Let's think step by step.
Output: <Explanation>
```

**Step 2: Answer Generation**

```
Input: [Question]
Let's think step by step. [Explanation]
Among A to D, the answer is:
Output: <Answer>
```

**Characteristics:**

- Two-step process
- Self-generated reasoning
- Magic phrase: "Let's think step by step"
- No example reasoning shown

---

**4. Few-shot CoT Prompting (bottom right):**

```
Input: Here are the answers for the questions in exams.
Q1: [Question 1] Explanation is: [Explanation]. The answer is [Answer 1]
Q2: [Question 2] Explanation is: [Explanation]. The answer is [Answer 2]
...
Qn: [Question n] Explanation is: [Explanation]. The answer is [Answer n]
[Question n+1]

Output: Explanation is <Explanation>. The answer is <Answer>
```

**Characteristics:**

- Shows example reasoning steps
- Demonstrates desired explanation format
- Single-step generation
- Combines benefits of few-shot and CoT

---

**Visual representation in Figure 2:**

- Dashed boxes separate steps in zero-shot CoT
- Solid boxes for single-step methods
- Orange text highlights where explanation goes
- Clear distinction between input and output

---

#### **4.2.4: Evaluation Metrics**

**Dual approach:** _"To assess the performance of the language models on our benchmark, we used both quantitative and qualitative evaluation metrics."_

---

**Quantitative metrics:** _"Quantitative metrics included task-specific accuracy for multi-choice questions and use Exact Match (EM) for fill-in-blank questions."_

**Multiple-choice accuracy:**

- **Formula**: correct / total
- **Range**: 0% to 100%
- **Interpretation**: Higher is better
- **Advantages**: Simple, interpretable, comparable to human scores

**Exact Match (EM):**

- **Application**: Fill-in-blank questions (math cloze)
- **Scoring**: 1 if exact match, 0 otherwise
- **Strictness**: No partial credit
- **Use case**: When precision is essential (numerical answers)

---

**Qualitative evaluation:** _"For a more in-depth analysis, we also performed qualitative evaluations, which involved human evaluators assessing the models' responses in terms of semantic understanding capability, knowledge utilization, and reasoning quality."_

**Human evaluation process:**

1. Sample 100 incorrect answers per task
2. Expert annotators (Ph.D. students, researchers) examine outputs
3. Score on four dimensions: Understanding, Knowledge, Reasoning, Calculation
4. Identify common error patterns
5. Provide detailed failure analysis

---

**Comprehensive understanding:** _"This combination of evaluation metrics allowed us to obtain a comprehensive understanding of the models' strengths and limitations in human-centric reasoning tasks."_

**Why both are needed:**

- **Quantitative**: Objective comparison, progress tracking
- **Qualitative**: Understanding _why_ models fail, guiding improvements

---

#### **4.2.5: Implementation Details**

**API platform:** _"All experiments were conducted using the respective language models' API provided by Azure OpenAI Service."_

**Why Azure:**

- Stable API endpoints
- Consistent model versions
- Enterprise-grade reliability
- Clear documentation

---

**Two API types:** _"The Azure OpenAI services offer two types of APIs: completion and chat completion."_

**Completion API:**

- **Input**: Text prompt
- **Output**: Continuation of text
- **Use case**: Text generation tasks
- **Models using this**: Text-Davinci-003

**Chat Completion API:**

- **Input**: Conversation history (messages)
- **Output**: Next assistant message
- **Use case**: Conversational interactions
- **Models using this**: ChatGPT, GPT-4

---

**API assignment by model:** _"For Text-Davinci-003, we use the completion API, and for ChatGPT and GPT-4, we use the chat completion API."_

**Constraint noted:** _"Notably, only the chat completion API is available for GPT-4 at present."_

**Implication**: Few-shot experiments with GPT-4 required special handling (see below).

---

**Generation parameters:**

**Temperature:** _"We use a temperature of zero to generate output using greedy search."_

- **Temperature = 0**: Deterministic output (always selects most likely token)
- **Greedy search**: No sampling, pure argmax
- **Benefit**: Reproducible results

**Maximum tokens:** _"Set the maximum number of tokens for generation to 2048."_

- Allows for long explanations in CoT
- Accommodates verbose reasoning
- Matches context window of some models

**Other parameters:** _"Additionally, we set the frequency penalty to zero and top p to 1, which are the default values for these APIs."_

- **Frequency penalty = 0**: No repetition penalty
- **Top p = 1**: No nucleus sampling (all tokens considered)
- Combined with temp=0: Pure deterministic generation

---

**API differences and implications:**

**Completion API behavior:**

- Straightforward prompt-to-completion
- Easy to control formatting
- Predictable behavior
- Works well for few-shot

**Chat Completion API behavior:** _"The Chat Completion API exhibits distinct properties in comparison to the Completion API."_

**Zero-shot advantage:** _"In a zero-shot context, the Chat Completion API has the potential to autonomously generate reasoning steps, eliminating the necessity for prompt engineering and potentially enhancing performance."_

**Explanation**: Chat models are trained to be helpful by default, may naturally include reasoning.

---

**Few-shot challenges:** _"For few-shot scenarios, it is imperative to adapt the few-shot examples into conversational history, as recommended in the Azure guidelines."_

**Conversion process:** _"The inquiry is transformed into a user input, while the AI's response is composed of a chain-of-thought explanation and answer."_

**Example conversion:**

```
# Original few-shot example:
Q: What is 2+2?
A: 4

# Converted to chat format:
{
  "role": "user",
  "content": "Q: What is 2+2?"
},
{
  "role": "assistant",
  "content": "A: 4"
}
```

---

**Problem encountered:** _"However, we have observed that the models, particularly ChatGPT, encounter difficulties in adhering to the pattern using the Chat Completion API."_

**What happens:**

- Format inconsistency
- Models add extra commentary
- Answer structure varies
- Harder to extract final answer

---

**Solution for ChatGPT:** _"Consequently, we employ the Completion API to conduct few-shot experiments with ChatGPT, which is analogous to Text-Davinci-003, in order to gain a deeper understanding of the disparities between Text-Davinci-003 and ChatGPT."_

**Rationale:**

- Ensures fair comparison
- Controls for API differences
- Isolates model capability differences

---

**Future work noted:** _"If a completion API for GPT-4 become accessible in the future, we will revise and update the few-shot outcomes accordingly."_

**Current situation**: GPT-4 few-shot uses Chat API (only option available).

---

**Few-shot Examples Construction:**

**For datasets with training sets:** _"For AQuA-RAT, LogiQA and LSAT, we randomly sample five examples of medium sentence length of the test set from the provided training set."_

**Selection criteria:**

- **Quantity**: 5 examples (standard few-shot count)
- **Selection method**: Random sampling
- **Length constraint**: Medium sentence length (avoids very short/long outliers)
- **Source**: Training set (no test set contamination)

---

**For datasets without separate train/test:** _"Similarly, for Gaokao and SAT, we randomly select five examples of medium sentence length from the dataset that was initially collected and exclude them from the test set."_

**Process:**

1. Collect full dataset
2. Sample 5 examples randomly
3. Set aside as few-shot demonstrations
4. Use remaining as test set
5. Ensures no overlap

---

**For JEC-QA special case:** _"For JEC-QA, given that the test set is not publicly available, we take the first 1,000 examples from the training set as the test set and again sample five examples of medium sentence length from the rest."_

**Rationale:**

- Original test set private
- Create new test set from training data
- Standard practice when test unavailable

---

**For MATH dataset:** _"For MATH, we use the same instances as in the appendices of Lewkowycz et al. (2022)."_

**Reference**: Paper on solving quantitative reasoning with language models **Benefit**: Enables direct comparison with prior work

---

**Chain-of-Thought Demonstrations:**

**For datasets with existing rationales:** _"To generate explanations for few-shot CoT experiments, for AQuA-RAT and MATH, we use the existing rationales from these datasets."_

**AQuA-RAT rationales:**

- Crowdsourced step-by-step solutions
- Natural language explanations
- Human-written reasoning chains

**MATH rationales:**

- Competition-style solutions
- Formal mathematical reasoning
- Step-by-step derivations

---

**For datasets needing annotations:** _"For Gaokao and SAT, we collected expert annotations."_

**Process:**

- Hire domain experts (teachers, tutors)
- Generate step-by-step solutions
- Quality control review
- Ensure clarity and correctness

---

**For datasets using model-generated CoT:** _"For LogiQA, JEC-QA and LSAT, we use ChatGPT to generate explanations given the questions and the answers."_

**Procedure:**

1. Provide ChatGPT with question + correct answer
2. Prompt: "Explain how to arrive at this answer step by step"
3. Review generated explanations for quality
4. Use as few-shot demonstrations

**Rationale:**

- Cost-effective for large datasets
- ChatGPT generates reasonable explanations
- Quality sufficient for few-shot examples

---

**Transparency:** _"We release all CoT demonstrations in the Github repository."_

**Includes:**

- All few-shot examples
- All CoT explanations (human and model-generated)
- Exact prompts used
- Enables reproduction and scrutiny

---

### **4.3: Main Results**

**Overview:** _"The results of zero-shot learning and zero-shot CoT (Chain-of-Thought) are reported in Tab. 2. The results of few-shot learning and few-shot CoT are reported in Tab. 3."_

**Human performance baseline:** _"We also report average and top human performance on each task."_

**Findings preview:** _"From the results, we highlight the following findings."_

---

#### **Table 2 Analysis: Zero-shot and Zero-shot CoT Results**

**Table structure:**

- Rows: 20 tasks
- Columns: Human performance (Avg., Top), then 6 model columns (3 models × 2 settings)
- All values are accuracy percentages

**Key observations from the data:**

---

**(1) Superior Performance of GPT-4:**

**Quantitative evidence:** _"Our results indicate that, on average, GPT-4 significantly outperforms its counterparts (ChatGPT and Text-Davinci-003) across all four evaluation settings."_

**Zero-shot averages:**

- Text-Davinci-003: 38.1%
- ChatGPT: 42.9%
- GPT-4: 56.4%

**Zero-shot CoT averages:**

- Text-Davinci-003: 37.4%
- ChatGPT: 43.2%
- GPT-4: 58.4%

**Improvement over ChatGPT:**

- Zero-shot: +13.5 percentage points
- Zero-shot CoT: +15.2 percentage points
- Relative improvement: ~31-35%

---

**Standout achievements:** _"Impressively, GPT-4 achieves 93.8% accuracy on Gaokao-English and 95% accuracy on SAT-MATH, demonstrating its superior general capabilities in handling human-centric tasks."_

**SAT-Math breakdown:**

- Human average: 66%
- Human top: 94%
- GPT-4 (zero-shot): 64.6%
- GPT-4 (zero-shot CoT): **95.0%**

**Interpretation**: With CoT, GPT-4 reaches top 1% human performance on SAT Math.

**Gaokao-English breakdown:**

- Human average: 69%
- Human top: 91%
- GPT-4 (zero-shot): 91.9%
- GPT-4 (zero-shot CoT): 92.5%

**Interpretation**: GPT-4 exceeds average humans and approaches top performance on Chinese college entrance English exam.

---

**(2) Comparison between ChatGPT and Text-Davinci-003:**

**Knowledge-intensive tasks:** _"Our analysis shows that ChatGPT significantly outperforms Text-Davinci-003 in tasks that require a high degree of external knowledge, such as those involving geography, biology, chemistry, physics, and mathematics."_

**Evidence from Table 2:**

|Task|Text-Davinci-003|ChatGPT|Gap|
|---|---|---|---|
|GK-geography|53.3%|59.8%|+6.5|
|GK-biology|40.5%|52.9%|+12.4|
|GK-chemistry|27.1%|38.7%|+11.6|
|GK-physics|22.0%|33.0%|+11.0|
|GK-Math-QA|28.2%|36.5%|+8.3|

**Interpretation:** _"This suggests that ChatGPT has a stronger knowledge base and is better equipped to handle tasks that necessitate a deep understanding of specific domains."_

**Possible explanations:**

- More recent training data
- Better RLHF alignment on factual questions
- More diverse training corpus
- Enhanced fact retention mechanisms

---

**Understanding-focused tasks:** _"On the other hand, ChatGPT slightly outperforms Text-Davinci-003 or achieves comparable results in tasks that require pure understanding and do not rely heavily on external knowledge, such as English and LSAT tasks, across all evaluation settings."_

**Evidence from Table 2:**

|Task|Text-Davinci-003|ChatGPT|Gap|
|---|---|---|---|
|SAT-English|74.8%|81.1%|+6.3|
|LSAT-LR|47.5%|52.6%|+5.1|
|LSAT-RC|64.7%|65.4%|+0.7|

**Interpretation:** _"This observation implies that both models are capable of handling tasks centered on language comprehension and logical reasoning without the need for specialized domain knowledge."_

---

**(3) Challenge of Complex Tasks:**

**Overall observation:** _"Despite the overall good performance of the models, we observe that all the LLMs struggle with complex reasoning tasks, such as MATH, LSAT-AR, GK-physics, and GK-Math, across all evaluation settings."_

---

**MATH (competition mathematics):**

- Human average: 40%
- Human top: 90%
- GPT-4 (best): 47.7%
- **Gap to top human**: 42.3 points

**LSAT-AR (analytical reasoning/logic games):**

- Human average: 56%
- Human top: 91%
- GPT-4 (best): 35.2%
- **Gap to average human**: 20.8 points (GPT-4 below average!)

**GK-physics:**

- Human average: 71%
- Human top: 94%
- GPT-4 (best): 45.5%
- **Gap to average human**: 25.5 points

**GK-Math (Gaokao):**

- GK-Math-QA: GPT-4 gets 50.7% (human avg 73%)
- GK-Math-Cloze: GPT-4 gets 16.1% (human avg 73%)

---

**Analysis:** _"This highlights the limitations of these models in handling tasks that require advanced reasoning and problem-solving skills."_

**Why these tasks are hard:**

**MATH challenges:**

- Non-standard problems
- Requires creative insights
- Multiple step reasoning
- No memorized solutions

**LSAT-AR challenges:**

- Complex constraint satisfaction
- Requires diagramming
- Multi-step deduction
- Abstract logical structures

**Physics challenges:**

- Requires deep conceptual understanding
- Variable substitution and formula manipulation
- Integrated knowledge of multiple concepts
- Real-world application of principles

---

**Future direction:** _"The observed difficulties in tackling complex reasoning problems present an opportunity for future research and development, aiming to enhance the models' general reasoning capabilities."_

---

**(4) Few-shot Learning vs. Zero-shot Learning:**

**Surprising finding:** _"In our experiments, we observe that few-shot learning generally leads to only a limited improvement in performance compared to zero-shot learning."_

**Quantitative evidence from Table 3:**

**Average improvements (few-shot over zero-shot):**

- Text-Davinci-003: 41.2% vs 38.1% = **+3.1 points**
- ChatGPT: 44.4% vs 42.9% = **+1.5 points**
- GPT-4: 59.2% vs 56.4% = **+2.8 points**

**With CoT:**

- Text-Davinci-003: 40.4% vs 37.4% = **+3.0 points**
- ChatGPT: 45.0% vs 43.2% = **+1.8 points**
- GPT-4: 61.3% vs 58.4% = **+2.9 points**

---

**Interpretation:** _"This finding suggests that the zero-shot capabilities of current large language models (LLMs) are approaching their few-shot learning abilities."_

**Historical context:** _"This marks a significant advancement compared to the original GPT-3 (Brown et al., 2020) model, where few-shot performance was considerably better than zero-shot."_

**GPT-3 (from original paper):**

- Zero-shot: Often poor (<50% on many tasks)
- Few-shot: Significant improvements (+15-30 points common)
- Few-shot essential for good performance

**Modern models (GPT-3.5+):**

- Zero-shot: Already quite capable
- Few-shot: Marginal improvements
- Zero-shot often sufficient

---

**Explanation:** _"One plausible explanation for this development is the enhanced human-alignment and instruction tuning in current LLMs."_

**Instruction tuning impact:**

- Models trained on diverse task instructions
- Better at understanding task intent from description
- Less need for examples to clarify expectations
- More robust to task variations

**Human alignment impact:** _"These improvements enable the models to better understand the meaning and context of tasks in advance, thus allowing them to perform well even in zero-shot settings."_

---

**Significance:** _"This progress in LLMs' zero-shot capabilities highlights the effectiveness of recent advancements in instruction tuning of LLMs."_

**Practical implications:**

- More user-friendly (no example crafting needed)
- Better out-of-box performance
- Easier deployment
- Lower prompt engineering burden

---

**Vicuna performance note:** _"As shown in Fig. 4 [should be Table 4], Vicuna, despite excelling on OpenLLM leaderboard and its claimed comparable ability with ChatGPT, falls short on AGIEval, highlighting the valuable challenges our benchmark presents to open-source models."_

**Vicuna scores (from Table 4):**

- Zero-shot average: ~27% (vs ChatGPT ~43%)
- Zero-shot CoT average: ~28% (vs ChatGPT ~43%)
- **Gap**: ~15-16 points below ChatGPT

**Implications:**

- Open LLM Leaderboard may test different skills
- AGIEval provides complementary human-centric evaluation
- Significant capability gap remains between open and closed models
- Room for improvement in open-source models

---

### **4.4: Analyses of Chain-of-Thought Prompting (CoT)**

**Overall assessment:** _"As reported in Tab. 2 and Tab. 3, the Chain-of-Thought (CoT) prompting technique demonstrates its potential by improving average zero-shot and few-shot performance."_

**Average improvements with CoT:**

- Zero-shot: 37.4-43.2% → 37.4-58.4% (variable by model)
- Few-shot: 40.4-45.0% → 40.4-61.3% (variable by model)

---

**Caveat:** _"However, the performance gains from CoT are not consistently observed across all tasks."_

**Finding summary:** _"Our analysis of CoT leads to the following findings:"_

---

#### **Finding (1): Performance Variability**

**Positive cases:** _"CoT substantially enhances performance in English mathematical exams, including MATH, AQuA-RAT, and SAT-Math."_

**Evidence:**

**MATH improvements (GPT-4):**

- Zero-shot: 35.7%
- Zero-shot CoT: 47.7%
- **Gain**: +12.0 points

**AQuA-RAT improvements (GPT-4):**

- Zero-shot: 40.6%
- Zero-shot CoT: 73.2%
- **Gain**: +32.6 points (massive improvement!)

**SAT-Math improvements (GPT-4):**

- Zero-shot: 64.6%
- Zero-shot CoT: 95.0%
- **Gain**: +30.4 points (reaches top human level!)

---

**Negative cases:** _"However, it leads to performance degradation in several other tasks, which may be a consequence of the model generating misleading reasoning processes."_

**Evidence of degradation:**

**GK-Math-Cloze (GPT-4):**

- Zero-shot: 16.1%
- Zero-shot CoT: 15.3%
- **Loss**: -0.8 points

**GK-Cn (Chinese) (GPT-4):**

- Zero-shot: 53.3%
- Zero-shot CoT: 44.7%
- **Loss**: -8.6 points

**SAT-English without passages (GPT-4):**

- Zero-shot: 51.0%
- Zero-shot CoT: 25.2%
- **Loss**: -25.8 points (dramatic decline!)

---

**Analysis of variability:** _"We also observed that the effects of CoT vary across different tasks, indicating that its impact on model performance is not uniform."_

**When CoT helps:**

- Multi-step mathematical reasoning
- Problems requiring explicit calculation
- Complex logical deduction
- Algorithmic problem-solving

**When CoT hurts:**

- Simple factual recall
- Direct text comprehension
- Pattern recognition
- Questions with misleading reasoning paths

---

**Future work needed:** _"It is crucial to further investigate the factors contributing to these performance variations and identify ways to optimize CoT for a broader range of tasks."_

---

#### **Finding (2): Backbone Dependency**

**Core observation:** _"The effectiveness of CoT is influenced by the underlying model."_

**GPT-4 advantage:** _"For example, GPT-4 is better equipped to generate illustrative reasoning processes, which subsequently result in enhanced performance when employing CoT."_

**Evidence:**

**Average CoT benefit by model (zero-shot to zero-shot CoT):**

- Text-Davinci-003: 38.1% → 37.4% (**-0.7 points**)
- ChatGPT: 42.9% → 43.2% (**+0.3 points**)
- GPT-4: 56.4% → 58.4% (**+2.0 points**)

**With few-shot:**

- Text-Davinci-003: 41.2% → 40.4% (**-0.8 points**)
- ChatGPT: 44.4% → 45.0% (**+0.6 points**)
- GPT-4: 59.2% → 61.3% (**+2.1 points**)

---

**Interpretation:**

- **Text-Davinci-003**: CoT often hurts performance (generates poor reasoning)
- **ChatGPT**: Minimal impact (slightly positive)
- **GPT-4**: Consistent benefits (better reasoning generation)

---

**Why GPT-4 benefits more:**

1. **Better reasoning quality**: Generates more accurate step-by-step logic
2. **Fewer errors**: Less likely to introduce mistakes in reasoning
3. **Self-correction**: Can catch and fix errors in generated reasoning
4. **Robustness**: Reasoning helps even when slight errors present

---

**Significance:** _"This observation highlights the importance of considering the interplay between CoT and the backbone model, as their compatibility can significantly impact problem-solving capabilities."_

**Practical implication**: CoT is not a universal solution—requires sufficiently capable base model.

---

#### **Finding (3): Language Sensitivity**

**Core observation:** _"The impact of CoT varies across different languages."_

---

**LogiQA example:** _"In the case of the LogiQA exam, the English version is translated from the original Chinese version."_

**GPT-4 performance:**

**LogiQA-English:**

- Zero-shot: 49.3%
- Zero-shot CoT: 57.8%
- **Change**: +8.5 points (**improvement**)

**LogiQA-Chinese:**

- Zero-shot: 58.8%
- Zero-shot CoT: 57.5%
- **Change**: -1.3 points (**degradation**)

---

**ChatGPT performance:**

**LogiQA-English:**

- Zero-shot: 35.0%
- Zero-shot CoT: 39.9%
- **Change**: +4.9 points (**improvement**)

**LogiQA-Chinese:**

- Zero-shot: 41.0%
- Zero-shot CoT: 38.9%
- **Change**: -2.1 points (**degradation**)

---

**Pattern observed:** _"Both ChatGPT and GPT-4 demonstrate performance improvements with CoT in LogiQA-English but suffer from performance degradation in LogiQA-Chinese."_

---

**Math exams pattern:** _"A similar pattern is observed in mathematical tests, where CoT boosts performance in English math exams (MATH, AQuA) but leads to performance declines in the Chinese math exam in Gaokao."_

**Evidence:**

**MATH (English) - GPT-4:**

- Zero-shot: 35.7%
- Zero-shot CoT: 47.7%
- **Change**: +12.0 points

**AQuA-RAT (English) - GPT-4:**

- Zero-shot: 40.6%
- Zero-shot CoT: 73.2%
- **Change**: +32.6 points

**GK-Math-QA (Chinese) - GPT-4:**

- Zero-shot: 47.0%
- Zero-shot CoT: 50.7%
- **Change**: +3.7 points (smaller gain)

**GK-Math-Cloze (Chinese) - GPT-4:**

- Zero-shot: 16.1%
- Zero-shot CoT: 15.3%
- **Change**: -0.8 points (degradation)

---

**Interpretation:** _"These findings suggest that CoT's effectiveness is sensitive to language variations, emphasizing the need to further generalize and optimize CoT for different linguistic contexts."_

**Possible explanations:**

**1. Training data imbalance:**

- More English reasoning examples in training
- CoT examples predominantly in English
- Less practice with Chinese reasoning chains

**2. Language-specific reasoning styles:**

- English: More explicit, step-by-step
- Chinese: More implicit, holistic
- CoT format matches English style better

**3. Translation artifacts:**

- Chinese→English translation may create patterns CoT exploits
- Original Chinese doesn't have these patterns
- Models confuse translation patterns with logical structure

---

**Future direction:** _"By tailoring CoT to better accommodate diverse languages, we can ensure more consistent and reliable problem-solving capabilities across a wider range of tasks and language settings."_

**Recommendations:**

- Develop language-specific CoT prompting strategies
- Include more multilingual reasoning in training
- Study language-specific reasoning patterns
- Create bilingual CoT datasets

---

**Summary statement:** _"These observations indicate that the effectiveness of CoT is relevant to task, model capability, and involved language. These factors should be carefully considered when employing CoT for specific tasks or developing future language models."_

**Three-way interaction:**

1. **Task type**: Math benefits, some tasks don't
2. **Model capability**: Better models benefit more
3. **Language**: English benefits more than Chinese

---

### **4.5: Qualitative Analyses of Model Capabilities**

**Motivation:** _"To gain a deeper understanding of the models' alignment with human capabilities, we perform a qualitative analysis by sampling 100 errorly answered instances for each task."_

**Why qualitative analysis:**

- Understand failure modes
- Identify systematic errors
- Provide actionable insights
- Complement quantitative metrics

---

**Methodology:** _"Specifically, we examine the outputs from ChatGPT under the zero-shot CoT setting, where the model is required to generate both explanations and the corresponding answers."_

**Why ChatGPT:**

- Good representative of current models
- Generates explicit reasoning
- Widely deployed

**Why zero-shot CoT:**

- Shows reasoning process
- Reveals model's thinking
- Easier to diagnose errors

**Why incorrect answers:**

- Success cases less informative
- Failures reveal limitations
- Guide improvement efforts

---

**Annotator selection:** _"We enlist human annotators with expert knowledge, such as Ph.D. students and professional researchers, to evaluate the model outputs (i.e., explanations and answers) along the following four dimensions."_

**Annotator qualifications:**

- Domain expertise in relevant subjects
- Familiarity with standardized testing
- Training in qualitative analysis
- Quality control procedures

---

#### **Four Dimensions of Evaluation**

**1. Understanding:** _"Evaluating whether the model can accurately comprehend the semantic meaning of the context and questions."_

**What's assessed:**

- Can model parse complex sentences?
- Does it identify key information?
- Does it understand question intent?
- Can it handle ambiguity?

---

**2. Knowledge:** _"Assessing whether the model can accurately recall relevant external knowledge or formulas for problem-solving."_

**What's assessed:**

- Does model know relevant facts?
- Can it recall appropriate formulas?
- Does it have domain-specific knowledge?
- Is knowledge up-to-date?

---

**3. Reasoning:** _"Determining whether the model can perform correct reasoning steps or formulate accurate reasoning ideas to solve problems."_

**What's assessed:**

- Does reasoning follow logically?
- Are inference steps valid?
- Can model do multi-step deduction?
- Does it avoid logical fallacies?

---

**4. Calculation:** _"Assessing whether the model can make correct mathematical calculations of the given formulas in the contexts of math, biology, chemistry and physics."_

**What's assessed:**

- Arithmetic accuracy
- Formula application
- Variable substitution
- Numerical precision

---

**Scoring:** _"An instance is scored with 1 if the model performs correctly on the corresponding skill and 0 otherwise."_

**Binary scoring rationale:**

- Clear, objective evaluation
- Easy to aggregate
- No ambiguity in partial credit
- Enables statistical analysis

---

**Task-specific exclusions:** _"It is worth noting that some tasks, such as LSAT and certain English tasks, primarily emphasize understanding and reasoning without requiring extensive external knowledge or calculation. These tasks are excluded from the corresponding skill analyses to maintain focus on the relevant capabilities."_

**Exclusions by dimension:**

- **Knowledge**: LSAT-RC, SAT-En excluded (pure comprehension)
- **Calculation**: All non-STEM tasks excluded
- **Understanding/Reasoning**: Included for all tasks

---

**Visualization:** _"The average scores on tasks for the four dimensions of capabilities are shown in Fig. 3."_

**Figure 3 description:**

- Four separate bar charts (one per dimension)
- Each chart shows all relevant tasks
- Bar height = proportion of correct capability (0 to 1)
- Easy to see which tasks challenge which capabilities

---

#### **4.5.1: Overall Trend of Model Capabilities**

**Understanding dimension analysis:**

**General performance:** _"The model generally performs well in the understanding dimension. For most tasks, it can accurately interpret the meaning of questions, demonstrating its ability to comprehend context effectively."_

**From Figure 3(a):**

- Most tasks: 0.6-1.0 score
- Strong performance on: MATH, SAT-En, GK-En, LogiQA
- Weaker performance on: GK-Ch (Chinese), GK-chemistry

**Interpretation:**

- Models have strong NLU capabilities
- Can parse complex questions
- Effective at identifying key information
- Understanding is not the primary bottleneck

---

**Knowledge dimension analysis:**

**Strengths:** _"In the knowledge dimension, the model demonstrates proficiency in identifying correct knowledge or formulas for mathematical and logical tasks."_

**From Figure 3(b):**

- Strong on: MATH (~0.95), SAT-Math (~0.95), AQuA (~1.0)
- Strong on: GK-Math tasks (~0.9-1.0)

---

**Weaknesses:** _"However, it encounters difficulties in recalling specific domain knowledge, such as law, biology, and physics."_

**From Figure 3(b):**

- Weak on: GK-physics (~0.3)
- Weak on: GK-chemistry (~0.35)
- Weak on: GK-biology (~0.3)
- Weak on: JEC-QA (law) (~0.4)

---

**Implications:** _"This observation emphasizes the significance of integrating more domain-specific knowledge into the model, potentially through the utilization of specialized domain-specific knowledge bases or knowledge-enhanced pre-training techniques."_

**Possible solutions:**

- Domain-specific fine-tuning
- Knowledge graph integration
- Retrieval-augmented generation
- Specialized training on textbooks/journals

---

**Reasoning dimension analysis:**

**Overall assessment:** _"Among the four dimensions, the model's reasoning capability appears to be relatively underdeveloped."_

**Critical finding:** _"For tasks necessitating complex, multi-step reasoning (e.g., LSAT-AR, LogiQA, and GK-Physics), the model struggles to accurately execute multi-step reasoning processes."_

**From Figure 3(c):**

- Weakest on: LSAT-AR (~0.2) - logic games requiring complex constraint satisfaction
- Weak on: GK-Math-Cloze (~0.2)
- Weak on: JEC-QA-CA (~0.3)
- Moderate on: Most other tasks (0.4-0.7)

---

**Significance:** _"This underlines the importance of future research concentrating on augmenting the model's reasoning capabilities, potentially through the exploration of innovative prompting methods or training strategies that encourage complex reasoning and problem-solving skills."_

**Why reasoning is hard:**

- Requires holding multiple facts in working memory
- Multi-step logical chains prone to error propagation
- Abstract reasoning beyond pattern matching
- Counterfactual and hypothetical thinking

---

**Calculation dimension analysis:**

**General assessment:** _"The model's calculation ability is weaker than their understanding capacity and displays variability across different subjects."_

**Performance by subject:** _"They perform better in math exams, but face challenges in chemistry and biology exams, which often require frequent variable substitution involving chemical elements."_

**From Figure 3(d):**

- Strong on: SAT-Math (~0.65), AQuA (~0.65), MATH (~0.65)
- Moderate on: GK-Math tasks (~0.3-0.6)
- Weak on: GK-chemistry (~0.25)
- Weak on: GK-biology (~0.2)

---

**Explanation of difficulty:** _"This suggests that enhancing the calculation and combinatorial abstraction and calculation ability of the model, particularly in subject areas with specialized notations or customized symbol substitutions, is a crucial challenge for further improvement."_

**Examples of hard calculations:**

- Chemistry: Balancing equations with multiple elements
- Biology: Genetic cross calculations with multiple alleles
- Physics: Vector calculations with components
- Multi-step numerical problems

---

**Summary:** _"These insights into the model's performance across the four dimensions provide valuable information to guide future research and development efforts aimed at enhancing large language models' capabilities in addressing human-centric tasks."_

**Capability ranking (strongest to weakest):**

1. **Understanding**: Generally strong (0.6-1.0)
2. **Knowledge**: Strong for math/logic, weak for domain-specific (0.3-1.0)
3. **Calculation**: Moderate, varies by domain (0.2-0.7)
4. **Reasoning**: Weakest overall (0.2-0.7)

---

#### **4.5.2: Strengths**

**Introduction:** _"By closely examining the models' output explanations and analyzing their behavior patterns, we identify several strengths that highlight the capabilities of these models in handling various aspects of problem-solving."_

---

**Strength 1: Good Understanding**

**Description:** _"The models excel in accurately comprehending the semantic meaning of context and questions. They effectively discern nuances, interpret complex questions, and parse intricate sentences, showcasing their strong natural language understanding skills."_

**Specific capabilities:**

- **Nuance detection**: Distinguishing between similar but different meanings
- **Complex syntax**: Parsing long sentences with multiple clauses
- **Anaphora resolution**: Understanding pronouns and references
- **Implicit information**: Inferring unstated but implied information

**Foundation for problem-solving:** _"This capability enables them to grasp the core concepts of a problem and lays the foundation for subsequent reasoning and problem-solving steps."_

---

**Strength 2: Proficiency in Simple Reasoning and Deduction**

**Description:** _"The models are adept at handling tasks that require simple reasoning and deduction. They can draw straightforward conclusions, identify logical connections, and perform basic inference, which is crucial for addressing a wide variety of problems."_

**Examples given:**

**Example 1: Logical equivalence** _"For example, the model can comprehend 'could be true except' is equals to 'cannot be true'."_

**What this shows:**

- Understanding logical negation
- Recognizing equivalent phrasings
- Abstract logical reasoning

---

**Example 2: LSAT-AR deduction** _"Also, taking a question in the LSAT-AR task as an example, it requires the model to place 8 books to a bookcase with three shelves following conditions: 'each shelf should have at least 2 books and more books should be placed on the bottom shelf than on the top shelf.'"_

**Model's successful reasoning:** _"The model successfully deduced that 'there are at least 3 books on the bottom shelf and at most 2 books on the top shelf.'"_

**Deduction steps:**

1. Total books: 8
2. Three shelves, each ≥2 books minimum: 6 books allocated
3. Remaining: 2 books
4. Bottom > Top constraint
5. To maximize difference: Bottom gets extras, Top stays minimal
6. Conclusion: Top=2 (min), Bottom≥3

**What this shows:**

- Constraint satisfaction reasoning
- Arithmetic reasoning
- Inequality handling
- Logical deduction from multiple premises

---

**Significance:** _"Their ability to effectively perform simple reasoning tasks is an essential component of their overall problem-solving skillset."_

---

**Strength 3: Grasping General Reasoning Process**

**Description:** _"The models demonstrate an ability to understand and generate the general idea of reasoning processes. They can identify the main components of a problem, recognize the structure of a solution, and outline a high-level reasoning strategy."_

**What this means:**

- **Problem decomposition**: Breaking problems into subproblems
- **Solution structure**: Identifying steps needed
- **Strategy formulation**: Planning approach before executing
- **Meta-reasoning**: Thinking about thinking

**Practical value:** _"This capability allows them to generate meaningful explanations and provides a starting point for more detailed reasoning and problem-solving tasks."_

---

**Overall assessment of strengths:** _"These strengths indicate that the models have made significant progress in aligning with human problem-solving capabilities. However, there is still room for improvement, especially in complex reasoning tasks and domain-specific knowledge, as discussed in the subsequent section on weaknesses."_

---

#### **4.5.3: Weaknesses**

**Introduction:** _"Despite the significant strengths displayed by the models, there are certain limitations that need to be addressed to improve their overall performance. We outline these weaknesses based on the analysis of the models' output explanations:"_

---

**UNDERSTANDING DIMENSION WEAKNESSES:**

---

**Weakness 1: Difficulty with Variable Substitution**

**Description:** _"The models struggle to understand questions that require variable substitution, often failing to recognize the need for this operation and how it should be applied to solve the problem."_

**Impact:** _"This limitation can hinder their ability to tackle a wide range of mathematical and logical tasks."_

**Specific example:** _"For instance, the model frequently struggles to answer chemistry questions that involve substituting a variable in a chemical equation with a chemical element and analyzing its properties."_

**Example scenario:**

- Given: General equation Na₂CO₃ + 2HCl → 2NaCl + H₂O + CO₂
- Question: "What happens when you substitute carbonate with sulfate?"
- Model fails to: Recognize need to replace CO₃²⁻ with SO₄²⁻
- Model fails to: Adjust equation accordingly
- Model fails to: Reason about changed properties

**Why this is hard:**

- Requires abstract symbol manipulation
- Need to maintain equation balance
- Must reason about chemical properties
- Combines syntax (equation structure) and semantics (chemical meaning)

---

**Weakness 2: Challenges with Complex Math Concepts and Symbols**

**Description:** _"The models find it difficult to comprehend complex mathematical concepts and interpret the meaning of symbols, particularly when multiple symbols are involved."_

**Impact:** _"This weakness limits their ability to effectively address advanced mathematical problems."_

**Examples:**

- Multiple integrals: ∫∫∫ f(x,y,z) dV
- Complex notation: ∑ᵢⱼₖ aᵢⱼₖ xᵢ yⱼ zₖ
- Abstract algebra symbols: ⊕, ⊗, ≅
- Specialized notation: ∇²φ = ρ/ε₀

**Specific difficulties:**

- Operator precedence
- Subscript/superscript interpretation
- Domain-specific notation conventions
- Nested mathematical expressions

---

**Weakness 3: Confusion with Similar Concepts**

**Description:** _"The models can easily be confused by similar concepts or terms, sometimes leading to incorrect or misleading reasoning."_

**Example given:** _"For example, in the physics exam, the model is confused by the difference between vertical speed and horizontal speed of moving object."_

**Specific scenario:**

- Projectile motion problem
- Model conflates vertical velocity component with total velocity
- Or confuses horizontal displacement with total distance
- Leads to incorrect application of kinematic equations

**Other similar confusions observed:**

- Velocity vs. acceleration
- Mass vs. weight
- Heat vs. temperature
- Correlation vs. causation
- Necessary vs. sufficient conditions

**Why this happens:**

- Surface-level pattern matching
- Insufficient conceptual grounding
- Training data ambiguity
- Lack of deep understanding of distinctions

**Implication:** _"This issue underscores the need for better disambiguation and concept understanding techniques in future model iterations."_

---

**Weakness 4: Difficulty in Handling Long Contexts**

**Description:** _"The models are prone to being disrupted by long contexts, leading to a decline in their comprehension and reasoning abilities."_

**Specific problems:**

- Losing track of early information
- Forgetting constraints mentioned at beginning
- Attention diffusion across long text
- Decreased accuracy with position in context

**Examples:**

- Long LSAT reading passages (500+ words)
- Complex Gaokao Chinese passages (900+ tokens)
- Multi-paragraph physics problems with multiple parts

**Technical explanation:**

- Attention mechanism limitations
- Context window constraints (2048 tokens typical)
- Recency bias (recent information weighted more)
- Positional encoding limitations

**Future direction:** _"Improving the models' capacity to maintain focus and process extensive information is essential for enhancing their performance in real-world scenarios."_

**Possible solutions:**

- Hierarchical processing
- Memory mechanisms
- Segmented attention
- Retrieval-augmented architecture

---

**KNOWLEDGE DIMENSION WEAKNESSES:**

---

**Weakness 5: Insufficiency in Commonsense and Domain-Specific Knowledge**

**General description:** _"The models occasionally demonstrate a lack of commonsense or domain-specific knowledge, which hinders their ability to generate plausible explanations and provide accurate answers."_

---

**Commonsense knowledge failure example:**

**Scenario given:** _"For instance, given the conditions 'if Julio and Kevin both lead morning sessions, we know that Kevin and Rebecca must lead sessions that meet on the same day,' the model incorrectly deduces that 'Therefore, Rebecca must also lead a morning session.'"_

**Error analysis:**

- **Given premise**: Julio and Kevin lead morning sessions → Kevin and Rebecca same day
- **Model's inference**: Rebecca must also lead morning session
- **Correct reasoning**: Rebecca could lead afternoon session on the same day
- **Failure**: Model doesn't understand "same day" ≠ "same time of day"

**Commonsense knowledge needed:**

- Days contain both mornings and afternoons
- "Same day" allows different times
- Morning and afternoon are distinct temporal categories

---

**Domain-specific knowledge failures:** _"Additionally, the model generally performs poorly on tasks requiring specific domain knowledge, such as law and chemistry."_

**Law domain:**

- Lacks knowledge of legal precedents
- Doesn't understand legal principles deeply
- Misapplies legal concepts
- Poor on constitutional law, contracts, torts

**Chemistry domain:**

- Incomplete periodic table knowledge
- Errors in chemical reactions
- Misunderstands bonding principles
- Struggles with organic chemistry nomenclature

**Biology domain:**

- Genetic inheritance calculations
- Cellular processes
- Anatomical/physiological relationships
- Ecological concepts

---

**Implications:** _"This limitation underscores the importance of incorporating diverse knowledge sources into the training data and exploring techniques that can more effectively integrate and access this information within the models."_

_"Moreover, it emphasizes the necessity to broaden the models' exposure to a wider array of subjects and fields, ensuring a more comprehensive understanding of various domains."_

---

**Weakness 6: Difficulty Identifying Correct Formulas**

**Description:** _"The models occasionally struggle to recall and apply the appropriate formulas necessary to solve particular problems, especially in tasks that demand specialized knowledge or expertise."_

**Examples:**

- Physics: Using wrong kinematic equation for scenario
- Chemistry: Applying incorrect stoichiometry formula
- Statistics: Misidentifying which probability distribution applies
- Calculus: Choosing wrong integration technique

**Why this is hard:**

- Requires recognizing problem type from description
- Mapping situation to formula
- Understanding formula applicability conditions
- Remembering specialized formulas

**Impact:** _"This shortcoming suggests that there is potential for improvement in the models' knowledge retrieval mechanisms and their ability to recognize the relevance of specific formulas to a given problem."_

**Future directions:** _"Developing strategies to enhance the models' proficiency in identifying and applying correct formulas will be essential for improving their performance in tasks requiring a deep understanding of domain-specific concepts and techniques."_

**Possible solutions:**

- Formula databases with retrieval
- Problem-type classification
- Worked example training
- Explicit formula reasoning

---

**Summary for knowledge weaknesses:** _"Addressing these weaknesses in knowledge will contribute to the development of more robust and versatile large language models, better equipped to tackle a broader range of human-centric tasks and exhibit a more comprehensive understanding of various domains."_

---

**REASONING DIMENSION WEAKNESSES:**

---

**Weakness 7: Challenges in Strict Logical Deduction**

**Overview:** _"The models frequently encounter difficulties when attempting to perform strict logical deduction accurately."_

**Common errors:** _"Common issues include ignoring premise conditions, misconstruing sufficient and necessary conditions, or making errors in logical chaining."_

_"These types of errors are commonly observed in manual analyses."_

---

**Example 1: Ignoring premise conditions**

**Scenario:** _"For instance, given a condition, 'If Myers is on the team, neither Ortega nor Paine can be', and a solution, 'Ortega, Paine, Thomson, and Zayre are on the team', the model incorrectly states that this solution is wrong because 'Paine and Ortega are on the team', neglecting to first satisfy the premise condition 'If Myers is on the team'."_

**Error analysis:**

- **Logical structure**: IF Myers THEN NOT (Ortega OR Paine)
- **Proposed solution**: Ortega + Paine + Thomson + Zayre (no Myers!)
- **Model's error**: Rejects solution because Ortega and Paine present
- **Correct reasoning**: Contrapositive: If (Ortega OR Paine) THEN NOT Myers
- **Conclusion**: Solution is actually valid (Myers not on team)

**What the model failed to do:**

- Check premise condition first
- Apply contrapositive correctly
- Understand conditional logic structure

---

**Example 2: Confusing sufficient and necessary conditions**

**Scenario:** _"Furthermore, the model demonstrates a misunderstanding of the difference between sufficient and necessary conditions in its explanation of another question and states: 'If Kayne is assigned to an ambassadorship, then so is Jaramillo. This constraint is essentially the same as the given constraint that if Jaramillo is assigned to one of the ambassadorships, then so is Kayne'."_

**Error analysis:**

- **Given**: Kayne → Jaramillo (if Kayne, then Jaramillo)
- **Model claims equivalent to**: Jaramillo → Kayne
- **Actual logical relationship**: These are NOT equivalent
- **Correct relationship**: Contrapositive of A→B is ¬B→¬A, not B→A

**Truth table demonstration:**

|Kayne|Jaramillo|Kayne→Jaramillo|Jaramillo→Kayne|Equivalent?|
|---|---|---|---|---|
|Yes|Yes|True|True||
|Yes|No|False|True|No|
|No|Yes|True|False|No|
|No|No|True|True||

**What the model failed to understand:**

- Difference between sufficient and necessary conditions
- Asymmetry of implication
- Correct form of contrapositive

---

**Improvement needed:** _"To address these limitations, it is essential to improve the models' abilities to recognize and apply logical rules and refine their understanding of logical structures."_

---

**Weakness 8: Difficulty with Counterfactual Reasoning**

**Description:** _"The models consistently struggle with counterfactual reasoning tasks. They have difficulty generating alternative scenarios, evaluating hypothetical outcomes, or exploring potential consequences based on varying assumptions."_

---

**Example from LSAT:** _"For instance, the models frequently make incorrect judgments for counterfactual questions in the LSAT-AR task: 'Which one of the following, if substituted for the constraint that [Constraint A], would have the same effect in determining the assignment?'"_

**What this question requires:**

1. Understand current constraint A
2. Identify its logical effect on possible assignments
3. For each alternative constraint:
    - Imagine world with that constraint instead
    - Determine which assignments it allows
    - Compare to original constraint's allowed assignments
4. Find constraint with identical logical consequences

**Why this is hard:**

- **Hypothetical thinking**: Reasoning about non-actual situations
- **Systematic exploration**: Checking all possibilities
- **Equivalence testing**: Determining if effects match exactly
- **Mental simulation**: Holding multiple scenarios in mind

---

**Significance:** _"Enhancing the models' capabilities in handling counterfactual reasoning tasks is vital for developing a more comprehensive problem-solving skillset."_

**Real-world importance:**

- Legal reasoning (what if law was different?)
- Scientific hypothesis testing
- Strategic planning (scenario analysis)
- Causal reasoning

---

**Weakness 9: Struggles in Multi-hop Complex Reasoning**

**Description:** _"The models have difficulty accurately executing multi-hop complex reasoning tasks, often displaying inconsistent logic, omitting inference steps, or producing flawed reasoning chains."_

**What is multi-hop reasoning:**

- Requires connecting multiple pieces of information
- Each step builds on previous steps
- Error in early step propagates forward
- Requires maintaining coherent logical thread

**Example structure:**

1. Given: Facts A, B, C
2. From A and B, derive D
3. From D and C, derive E
4. From E, derive F (final answer)

**Model failures:**

- **Omitting steps**: Jumps from A,B directly to E
- **Inconsistent logic**: D contradicts earlier derivation
- **Lost thread**: Forgets D when reasoning about E
- **Error propagation**: Wrong D leads to wrong E and F

**Specific challenge:** _"To address a broader range of complex problems, it is crucial to improve the models' abilities to systematically navigate and process multi-step reasoning tasks."_

---

**Weakness 10: Establishing Incorrect Conclusions and Contradictory Reasoning**

**Description:** _"The models occasionally set an incorrect conclusion first and then generate contradictory reasoning based on that faulty foundation."_

**Process of failure:**

1. Model generates answer (possibly by pattern matching)
2. Then generates explanation
3. Explanation attempts to justify answer
4. But answer is wrong
5. Explanation contains contradictions or errors

**Why this happens:**

- Answer generation and explanation generation separate
- Confirmation bias in explanation generation
- Backward reasoning from wrong answer
- Lack of verification step

**Implication:** _"This behavior emphasizes the need for improved reasoning verification and error correction techniques in the models' problem-solving processes."_

**Possible solutions:**

- Verify consistency between reasoning and answer
- Check reasoning before committing to answer
- Self-critique mechanisms
- Adversarial verification

---

**Weakness 11: Concealed Substitution of Concepts**

**Description:** _"The models sometimes covertly substitute one concept with another similar one, leading to inaccurate or misleading reasoning."_

**Example given:** _"For example, in a biology exam, the model replaces the concept of 'isotopically labeled amino acids' with 'isotopically labeled tRNA (a tool for transporting amino acids)', resulting in erroneous reasoning."_

**Error analysis:**

- **Original concept**: Isotopically labeled amino acids
- **Substituted concept**: Isotopically labeled tRNA
- **Relationship**: tRNA transports amino acids, but they're distinct molecules
- **Why wrong**: tRNA and amino acids behave differently in experiments
- **Consequence**: Wrong predictions about experimental results

**Other examples:**

- Substituting "correlation" with "causation"
- Confusing "hypothesis" with "theory"
- Mixing up "precision" and "accuracy"

**Underlying issue:** _"This issue underscores the importance of better concept disambiguation and reasoning coherence in future model iterations."_

---

**Weakness 12: Difficulty in Identifying Solutions**

**Description:** _"The models occasionally struggle to discover feasible solutions for specific problems, possibly due to limitations in their knowledge, reasoning capabilities, or problem-solving strategies."_

**When this occurs:**

- Constraint satisfaction problems
- Finding counterexamples
- Constructive proofs
- Optimization problems

**Example scenarios:**

- LSAT-AR: Finding valid book arrangement satisfying all constraints
- Math: Constructing example of geometric shape with specific properties
- Chemistry: Designing reaction pathway to synthesize molecule

**What's needed:** _"Addressing this shortcoming involves refining the models' ability to explore, evaluate, and select appropriate solutions based on the given problem context."_

**Approaches:**

- Systematic search strategies
- Heuristic methods
- Backtracking when constraints violated
- Creative solution generation

---

**Weakness 13: Vulnerability to Contextual Disturbance**

**Description:** _"The reasoning ability of large language models is often easily disrupted by changes in the surrounding context."_

**Specific problem:** _"When the context is modified, the models may produce different deductions for the same condition, suggesting that the robustness of their reasoning ability is not yet sufficient."_

**Examples:**

- Adding irrelevant information changes answer
- Reordering premises leads to different conclusions
- Paraphrasing question produces inconsistent responses
- Different example problems in few-shot prompt affect logic

**Why this is problematic:**

- Indicates shallow understanding
- Reasoning should be stable across presentations
- Real-world scenarios have varying contexts
- Undermines reliability and trust

**Significance:** _"This observation emphasizes the need to develop models that can maintain consistent reasoning performance, even in the presence of varying contextual information, ensuring more reliable and stable problem-solving capabilities."_

---

**Summary for reasoning weaknesses:** _"By addressing these reasoning weaknesses, future large language models can be developed with more robust problem-solving capabilities, enabling them to effectively tackle a broader range of human-centric tasks and exhibit more sophisticated reasoning skills that align closely with human cognition."_

---

**CALCULATION DIMENSION WEAKNESSES:**

---

**Weakness 14: Calculation Errors and Variable Substitution Difficulties**

**Overview:** _"The model is prone to making calculation errors, particularly when dealing with complex variable substitutions."_

---

**Root causes:** _"This may be attributed to the inherent limitations of the model's computation process in handling mathematical operations, as well as its difficulty in parsing intricate relationships between variables."_

**Specific limitations:**

**1. Arithmetic errors:**

- Addition/subtraction mistakes
- Multiplication/division errors
- Rounding errors
- Order of operations mistakes

**2. Variable substitution failures:**

- Forgetting to substitute all instances
- Partial substitution
- Incorrect value replacement
- Symbol confusion

**3. Algebraic manipulation errors:**

- Invalid equation transformations
- Sign errors when moving terms
- Distributive property mistakes
- Factoring errors

---

**Example scenario (chemistry):**

- Given: Ideal gas law PV = nRT
- Substitute: n = 2 mol, R = 8.314 J/(mol·K), T = 300K, V = 0.025 m³
- Calculate: P = nRT/V = (2)(8.314)(300)/(0.025)
- Model errors:
    - Forgets to substitute one variable
    - Or makes arithmetic mistake: 2×8.314×300 = 4988.4 (correct)
    - Then 4988.4/0.025 = 199,536 Pa
    - Model might calculate wrong intermediate or final value

---

**Impact:** _"Consequently, the model may struggle to maintain accuracy and precision when attempting to solve problems involving advanced algebraic manipulations or multi-step calculations."_

---

**Future directions:** _"To address this issue, future iterations of the model should focus on enhancing its mathematical reasoning capabilities and improving its ability to recognize and apply relevant mathematical rules."_

**Possible approaches:** _"This could involve incorporating specialized modules or mechanisms specifically designed to handle complex calculations, variable substitutions, and numerical problem-solving tasks."_

**Specific solutions:**

1. **Symbolic computation integration**: Use computer algebra systems
2. **Step verification**: Check each calculation step
3. **Tool use**: Allow calculator/computation API calls
4. **Specialized training**: More focus on calculation accuracy
5. **Verification prompts**: Ask model to double-check calculations

---

**Broader vision:** *"By refining the model's ability to accurately process and solve intricate mathematical problems, we can expand its applicability across a broader range of disciplines and domains, ensuring a more comprehensive and robust problem-solving skillset."**

---

## **SECTION 5: DISCUSSION ABOUT FUTURE DIRECTIONS**

**Introduction:** _"In light of the findings and limitations identified in our analysis, we point out several potential future directions for the development of large foundation models. These directions aim to address the weaknesses observed and further improve the models' capabilities in various human-centric tasks."_

---

### **5.1: Inclusion of External Knowledge and Formulas**

**Core proposal:** _"Enriching the models with external knowledge sources, like formulas and domain-specific knowledge can help enhance their performance in mathematical and knowledge-intensive tasks."_

---

**Domain-specific adaptation:** _"Specifically, developing models that can effectively handle domain-specific tasks, such as those in law, biology, or physics, requires the integration of specialized knowledge bases and expertise into the model, and enables the model to adapt to different verticals more effectively."_

**What "verticals" means:**

- Industry-specific domains
- Professional specializations
- Academic disciplines

---

**Implementation approaches:** _"This could involve integrating structured knowledge repositories, mathematical and scientific concepts into the models with pre-training or knowledge-enhanced prompting methods, allowing them to access and apply relevant information more efficiently."_

**Specific techniques:**

**1. Knowledge graph integration:**

- Link model to structured knowledge bases
- Examples: Wikidata, domain-specific ontologies
- Enable fact lookup during inference

**2. Formula databases:**

- Repository of mathematical/scientific formulas
- Indexed by domain and application
- Retrieved when relevant problems detected

**3. Knowledge-enhanced pre-training:**

- Train on curated domain-specific corpora
- Scientific papers, textbooks, professional publications
- Emphasize technical accuracy

**4. Retrieval-augmented generation:**

- Model retrieves relevant documents
- Incorporates retrieved knowledge into response
- Combines parametric and non-parametric knowledge

---

**Benefits:**

- Better recall of specialized facts
- More accurate technical information
- Reduced hallucination on domain-specific topics
- Ability to stay current (update knowledge bases)

---

### **5.2: Strict Complex Logical Reasoning**

**Core proposal:** _"Improving the models' capacity for strict complex logical reasoning is crucial for their performance in a wide range of human-centric tasks."_

---

**Approach 1: Specialized training data** _"This could involve the creation of new datasets that emphasize complex reasoning."_

**Dataset characteristics:**

- Multi-step logical problems
- Formal logic exercises
- Constraint satisfaction problems
- Proof-based mathematics
- Legal reasoning cases

---

**Approach 2: External reasoning tools** _"As well as incorporating APIs and external symbolic compilers that can execute strict logical or mathematical deduction, and use the execution results to further facilitate logical analysis and reasoning verification."_

**Tool integration examples:**

**1. Theorem provers:**

- Automated reasoning systems (e.g., Coq, Isabelle)
- Verify logical proofs
- Check consistency of reasoning

**2. SAT/SMT solvers:**

- Boolean satisfiability solvers
- Find solutions satisfying logical constraints
- Verify feasibility of proposed solutions

**3. Computer algebra systems:**

- Symbolic mathematics (e.g., Mathematica, Maple)
- Exact symbolic calculations
- Formula manipulation and verification

**4. Constraint solvers:**

- Solve constraint satisfaction problems
- Verify proposed solutions
- Generate valid examples

---

**Workflow:**

1. Model generates reasoning steps
2. Translates steps to formal logic
3. External tool verifies correctness
4. Tool provides feedback
5. Model adjusts reasoning if needed
6. Produces final verified answer

**Benefits:**

- Guarantees logical validity
- Catches reasoning errors
- Enables complex deductive reasoning
- Builds trust through verification

---

### **5.3: Multi-lingual Reasoning Capabilities Generalization**

**Motivation:** _"As mentioned in Sec. 4.4, the reasoning capabilities of models are variant across different language, where the reasoning ability is relatively better for rich-resourced language like English."_

**Observed problem:**

- CoT helps English tasks more than Chinese
- Performance gaps between languages
- Language-specific reasoning patterns

---

**Importance:** _"Enhancing the models' multi-lingual reasoning capabilities is essential for their applicability in a diverse range of real-world scenarios."_

**Why this matters:**

- Global deployment requires multilingual support
- Most of world's population doesn't speak English
- Different languages encode knowledge differently
- Cultural contexts affect reasoning

---

**Future direction:** _"Therefore, future directions can put more focus on enhancing multilingual generalization of reasoning capability of foundation models."_

**Specific approaches:**

**1. Multilingual reasoning datasets:**

- Translate existing reasoning benchmarks
- Create native reasoning tasks in multiple languages
- Ensure cultural relevance

**2. Cross-lingual transfer learning:**

- Train reasoning in one language
- Transfer to others
- Identify language-independent patterns

**3. Multilingual CoT training:**

- Include reasoning chains in multiple languages
- Diverse reasoning styles
- Language-specific prompting strategies

**4. Balanced training:**

- Don't over-represent English
- Include reasoning in low-resource languages
- Multilingual mixing strategies

---

### **5.4: Multi-modal Evaluation**

**Core proposal:** _"Expanding the evaluation framework to include multi-modal tasks can provide a more comprehensive assessment of the models' capabilities."_

---

**What multi-modal means:** _"This could involve incorporating visual, auditory, or interactive tasks that require the models to process and reason with multiple types of input simultaneously and generates multi-modal outputs for comprehensive real-world applications."_

**Input modalities:**

- **Visual**: Images, diagrams, charts, graphs
- **Auditory**: Speech, audio signals
- **Text**: Written language (already included)
- **Structured data**: Tables, databases
- **Interactive**: Dynamic environments, games

**Output modalities:**

- **Text**: Natural language explanations
- **Visual**: Generated images, diagrams
- **Code**: Executable programs
- **Structured**: JSON, tables, formal representations

---

**Why multi-modal matters for human-centric evaluation:**

**1. Real exams include visuals:**

- Geometry diagrams
- Chemical structures
- Data visualizations
- Reading passage images
- Maps and charts

**2. Complete human cognition:**

- Humans integrate multiple senses
- Visual reasoning essential
- Spatial understanding required
- Comprehensive evaluation needs all modalities

**3. Real-world applicability:**

- Professional work uses multiple modalities
- Doctors read X-rays and lab results
- Engineers work with technical drawings
- Scientists analyze experimental data

---

**AGIEval future:** _"In the future work, we will focus on the multi-modal version of AGIEval."_

**Planned additions:**

- Original exam questions with images
- Diagram-based reasoning tasks
- Visual math problems
- Scientific figures and graphs
- Data visualization interpretation

---

### **5.5: Better Automatic Evaluation Metrics for Human-centric Tasks**

**Core problem:** _"Developing more robust and meaningful automatic evaluation metrics is crucial for the objective assessment of large language models' performance."_

---

**Current limitations:**

- Accuracy doesn't capture reasoning quality
- Exact match too strict for some tasks
- Hard to evaluate open-ended responses
- Process vs. outcome evaluation

---

**Future research focus:** _"Future research should focus on devising metrics that can accurately capture the models' understanding, knowledge, and reasoning abilities, while taking into account the nuances and complexities of real-world tasks."_

**Desired properties:**

**1. Process-aware metrics:**

- Evaluate reasoning steps, not just final answer
- Reward correct intermediate steps
- Penalize flawed reasoning even with correct answer

**2. Partial credit:**

- Recognize partially correct solutions
- Credit for right approach with calculation error
- Nuanced scoring beyond binary

**3. Explanation quality:**

- Measure coherence of generated reasoning
- Evaluate logical validity
- Assess completeness

**4. Robustness measures:**

- Consistency across paraphrases
- Stability under perturbations
- Variance across prompting

**5. Human-aligned scoring:**

- Correlates with expert judgments
- Captures what matters to humans
- Reflects real-world assessment criteria

---

**Potential approaches:**

**1. Learned metrics:**

- Train models to score explanations
- Based on human judgments
- Can capture nuanced quality

**2. Formal verification:**

- Check logical validity automatically
- Verify reasoning chains
- Detect contradictions

**3. Multi-dimensional scoring:**

- Separate scores for understanding, knowledge, reasoning, calculation
- Weighted combination
- Diagnostic feedback

---

### **5.6: Robustness of Reasoning Capability**

**Core challenge:** _"Improving the robustness of the models' reasoning capabilities is essential for ensuring their consistency and reliability across various contexts."_

---

**The problem:**

- Models sensitive to irrelevant context
- Inconsistent across paraphrases
- Different answers to equivalent questions
- Vulnerable to prompt variations

---

**Desired outcome:** _"This can be achieved by exploring techniques that enhance the models' ability to maintain consistent reasoning performance, even when faced with changes in the surrounding context or variations in the input data."_

---

**Specific robustness goals:**

**1. Paraphrase invariance:**

- Same answer for rephrased questions
- Recognize logical equivalence
- Stable core reasoning

**2. Context robustness:**

- Irrelevant information doesn't confuse
- Focus on pertinent facts
- Filter distractors

**3. Prompt stability:**

- Less sensitive to exact prompt wording
- Consistent across prompting styles
- Robust to minor changes

**4. Order invariance:**

- Premise order shouldn't affect logic
- Consistent regardless of presentation
- Focus on logical structure

---

**Approaches to improve robustness:**

**1. Adversarial training:**

- Train on perturbed examples
- Paraphrases, reorderings, distractors
- Learn invariance to irrelevant changes

**2. Consistency regularization:**

- Penalize inconsistent predictions
- Encourage stable representations
- Reward invariance

**3. Reasoning verification:**

- Check reasoning with formal tools
- Verify consistency
- Catch contradictions

**4. Ensemble methods:**

- Multiple reasoning paths
- Aggregate predictions
- Increase reliability

---

### **Conclusion of Future Directions**

**Summary statement:** _"By addressing these future directions, foundation models can be further developed and refined to exhibit more advanced capabilities that align closely with human cognition, ultimately enabling them to tackle a broader range of complex, human-centric tasks with greater accuracy and reliability."_

**The vision:**

1. **Knowledge integration**: Better domain expertise
2. **Logical reasoning**: Verified, robust deduction
3. **Multilingual**: Truly global capabilities
4. **Multi-modal**: Complete sensory integration
5. **Better metrics**: Accurate quality assessment
6. **Robustness**: Reliable, consistent performance

**Path to AGI:**

- Current models show promise
- Significant gaps remain
- Systematic improvement possible
- Human-centric evaluation essential

---

## **SECTION 6: CONCLUSION**

**Opening summary:** _"In this paper, we introduce AGIEval, a novel benchmark specifically designed to assess the general capabilities of large foundation models with respect to human-level cognition."_

---

### **6.1: Benchmark Composition**

**Data sources:** _"The benchmark comprises high-quality official admission tests, qualification exams, and advanced competitions tailored for human participants, including law school admission tests and college entrance examinations."_

**Why these sources:** _"These assessments establish officially recognized standards for gauging human capabilities, making them well-suited for evaluating foundation models in the context of human-centric tasks."_

---

**Bilingual design:** _"Additionally, AGIEval incorporates bilingual tasks in both Chinese and English, offering a more comprehensive assessment of model behavior."_

**Scope:**

- 20 tasks across diverse subjects
- 8,062 questions total
- Both English and Chinese
- Multiple evaluation settings

---

### **6.2: Models Evaluated**

**Evaluated models:** _"We have carried out an extensive evaluation of three cutting-edge large foundation models: Text-Davinci-003, ChatGPT, and GPT-4, using AGIEval."_

**Evaluation settings:**

- Zero-shot
- Few-shot (K=5)
- Zero-shot Chain-of-Thought
- Few-shot Chain-of-Thought

---

### **6.3: Major Achievements**

**Headline results:** _"Remarkably, GPT-4 surpasses average human performance on LSAT, SAT, and math competition, attaining a 95% accuracy rate on the SAT Math test and a 92.5% accuracy on the Gaokao English test, demonstrating the impressive performance of contemporary foundation models."_

**Significance:**

- First AI to exceed average humans on comprehensive standardized exams
- Approaches top human performance on some tasks
- Milestone toward AGI
- Validates scaling hypothesis

**Performance summary:**

- **GPT-4**: 58-61% average (varies by setting)
- **ChatGPT**: 43-45% average
- **Text-Davinci-003**: 37-40% average
- **Human average**: 67%
- **Top humans**: 91%

---

### **6.4: Identified Limitations**

**Despite achievements:** _"Despite their significant achievements, our in-depth manual analyses also reveal the limitations of these large language models in terms of understanding, knowledge utilization, reasoning and calculation."_

**Specific weaknesses:**

1. **Understanding**: Variable substitution, long contexts
2. **Knowledge**: Domain-specific gaps (law, chemistry, physics)
3. **Reasoning**: Complex deduction, multi-hop reasoning, counterfactuals
4. **Calculation**: Errors in complex calculations, especially with variable substitution

---

### **6.5: Future Research Directions**

**Guided by findings:** _"Guided by these findings, we explore potential future research avenues in this domain."_

**Key directions identified:**

1. External knowledge integration
2. Verified logical reasoning
3. Multilingual capabilities
4. Multi-modal evaluation
5. Better evaluation metrics
6. Robustness improvements

---

### **6.6: Research Philosophy**

**Core motivation:** _"By assessing these foundation models on human-centric tasks and probing their capabilities more deeply, we strive to foster the development of models that are more closely aligned with human cognition."_

**Alignment goals:**

- Match human reasoning patterns
- Handle real-world complexity
- Demonstrate reliable performance
- Enable trustworthy deployment

---

**Ultimate objective:** _"Ultimately, this will enable them to tackle a broader range of intricate, human-centric tasks with increased accuracy and reliability."_

**Vision:**

- AI systems as cognitive partners
- Augment human capabilities
- Handle professional-level tasks
- Progress toward AGI

---

### **6.7: Contribution to Field**

**Scientific contribution:**

- **Benchmark**: Public, reproducible, comprehensive
- **Evaluation**: Transparent methodology, released outputs
- **Analysis**: Detailed capability assessment
- **Insights**: Actionable improvement directions

**Community impact:**

- Enables progress tracking
- Guides research priorities
- Provides diagnostic tools
- Facilitates open science

---

### **6.8: Open Science Commitment**

**Data and code release:**

- Full dataset: 8,062 questions
- All model outputs across settings
- Exact prompts used
- Evaluation scripts
- GitHub repository: https://github.com/ruixiangcui/AGIEval

**Reproducibility:**

- Clear experimental procedures
- Specified hyperparameters
- Transparent metrics
- Documented limitations

---

### **6.9: Broader Impact**

**For AI development:**

- Realistic performance assessment
- Identifies critical weaknesses
- Guides resource allocation
- Informs safety considerations

**For society:**

- Understand AI capabilities honestly
- Set appropriate expectations
- Make informed deployment decisions
- Ensure responsible AI development

---

### **Final Statement**

AGIEval represents a significant step toward evaluating AI systems on truly human-relevant tasks. By grounding evaluation in high-stakes standardized exams that millions of people take annually, the benchmark provides meaningful comparison points and reveals both the impressive progress and remaining limitations of current foundation models.

The finding that GPT-4 approaches or exceeds average human performance on some standardized tests marks a historic milestone, while the identified gaps—particularly in complex reasoning, domain-specific knowledge, and calculation accuracy—chart a clear path for future research.

Through comprehensive evaluation across four dimensions (understanding, knowledge, reasoning, calculation) and multiple settings (zero-shot, few-shot, with and without Chain-of-Thought), AGIEval provides the community with actionable insights to drive progress toward more capable, reliable, and human-aligned AI systems.

---

**END OF COMPLETE DETAILED ANALYSIS**

