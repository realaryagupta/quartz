## 1. Introduction to the Exegesis

This report constitutes a comprehensive, line-by-line technical analysis of the Introduction section of the survey paper **"Toward Efficient Agents: A Survey of Memory, Tool learning, and Planning"** (arXiv:2601.14192, January 2026). This document is designed not merely as a summary, but as an exhaustive expansion of the primary text, unpacking every theoretical construct, historical allusion, and mathematical implication embedded within the authors' prose.

The analysis is grounded in the premise that the transition from static Large Language Models (LLMs) to autonomous Agentic Systems represents a fundamental discontinuity in artificial intelligence development—a shift as significant as the move from symbolic AI to connectionism. By dissecting the Introduction, we uncover the structural inefficiencies inherent in this new paradigm and evaluating the proposed taxonomies for mitigation.

The report follows the exact narrative arc of the source text, expanding each sentence into a detailed technical discussion supported by background literature, algorithmic derivations, and comparative analysis.

---

## 2. The Historical and Architectural Paradigm Shift

### 2.1 Verbatim Text Analysis

> _"The landscape of Artificial Intelligence has undergone a paradigm shift, evolving from the era of Convolutional Neural Networks (CNNs) and Recurrent Neural Networks (RNNs) to the advent of Large Language Models (LLMs), and the emergence of LLM-based Agents currently."_

### 2.2 Theoretical Deconstruction of Architectural Evolution

The authors open by situating the current research within a macro-historical context. To understand the specific efficiency constraints of "LLM-based Agents," one must first rigorously understand the architectures they succeeded and the computational legacy they inherited.

#### 2.2.1 The Era of Perceptual and Temporal Constraints (CNNs & RNNs)

The explicit mention of **Convolutional Neural Networks (CNNs)** and **Recurrent Neural Networks (RNNs)** serves to contrast "perception" with "reasoning."

- **Convolutional Neural Networks (CNNs):**
    
    - **Mechanism:** CNNs operate on the mathematical principle of **spatial invariance**. A convolutional kernel $K$ slides over an input matrix $I$ (e.g., an image), computing the dot product at each position. This architecture relies on the assumption that features (edges, textures) are local and translation-invariant.
        
    - **Limitation in Agency:** While CNNs revolutionized computer vision (perception), they are structurally incapable of _reasoning_ or _sequential planning_. They function as static mappers $f: \mathcal{X} \rightarrow \mathcal{Y}$. They possess no internal state memory that persists across independent inferences, making them unsuitable for the temporal continuity required by agents.
        
    - **Relevance to Efficiency:** CNNs are highly efficient due to weight sharing (the same kernel is used across the entire input). The shift to Transformers (LLMs) abandoned this efficiency for the expressivity of global attention, a trade-off that underpins the current efficiency crisis discussed later in the paper.
        
- **Recurrent Neural Networks (RNNs):**
    
    - **Mechanism:** RNNs introduced the dimension of time. An RNN processes a sequence $x_1, x_2,..., x_t$ by maintaining a hidden state vector $h_t$ which acts as a compressed representation of the history. The update rule is typically $h_t = \sigma(W_h h_{t-1} + W_x x_t + b)$.
        
    - **The Vanishing Gradient Problem:** As the sequence length $t$ increases, the gradients utilized for training (backpropagation through time) tend to vanish or explode, making it mathematically impossible for standard RNNs to learn dependencies over long horizons (e.g., >100 tokens). Even advanced variants like **Long Short-Term Memory (LSTM)** networks , which utilize gating mechanisms (input, forget, and output gates) to preserve gradient flow, struggle with the scale of context required for complex reasoning.
        
    - **The Agentic Gap:** RNNs process data sequentially ($O(N)$ latency). They cannot "look back" at past information without iterating through all intermediate steps. This strictly limits their ability to perform the complex, non-linear information retrieval required by modern agents.
        

#### 2.2.2 The Advent of Large Language Models (LLMs)

The "advent" refers to the introduction of the **Transformer** architecture (Vaswani et al., 2017), which replaced recurrence with **Self-Attention**.

- **The Attention Mechanism:**
    
    The core mathematical innovation that enabled LLMs is the scaled dot-product attention:
    
    $$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
    
    - **$Q$ (Query):** What the model is currently focusing on.
        
    - **$K$ (Key):** The index of information present in the context.
        
    - **$V$ (Value):** The actual information content.
        
    
    This mechanism allows every token in a sequence to "attend" to every other token directly, regardless of distance. This global context awareness is the prerequisite for **reasoning**—the ability to correlate a premise defined at token 1 with a conclusion derived at token 10,000.
    
- **From Modeling to Reasoning:**
    
    The authors distinguish LLMs from their predecessors by noting they are not just for "static text generation." The "paradigm shift" is the emergent property of LLMs to perform **In-Context Learning**. By providing examples in the prompt (context), the model adapts its behavior without weight updates. This is the foundational capability that allows an LLM to function as the "brain" of an agent.
    

#### 2.2.3 The Emergence of LLM-based Agents

The phrase "LLM-based Agents currently" defines the scope of the survey. An **Agent** is defined systemically:

$$\text{Agent} = \text{LLM} + \text{Memory} + \text{Tools} + \text{Planning}$$

This modular definition is critical. The LLM provides the cognitive processing (reasoning), but the "Agency"—the ability to act autonomously over time—emerges from the interaction of the LLM with these other components.

- **Reference Context:** The paper cites ****. Based on the bibliographic analysis , these refer to:
    
    - ** Toolformer (Schick et al., 2023):** A seminal paper demonstrating that LLMs can self-supervise the use of external tools (calculators, wikis), bridging the gap between static knowledge and dynamic action.
        
    - ** Generative Agents (Park et al., 2023):** This demonstrated agents simulating believable human behavior in a sandbox environment, heavily relying on _memory streams_ and _reflection_, establishing the importance of memory architecture.
        
    - ** WebShop / WebArena:** These references likely point to benchmarks that operationalize agency—moving from answering questions (QA) to navigating e-commerce websites (Action).
        

### 2.3 Functional Distinction: Processing vs. Interaction

> _"Unlike their predecessors, which primarily focused on perception or static text generation, agentic systems do not merely process information; they actively interact with external environments to execute complex, multi-step workflows across diverse domains, such as autonomous software engineering and accelerated scientific discovery ."_

#### 2.3.1 Active Interaction and the POMDP Formulation

The authors draw a sharp distinction between "processing" (passive) and "interacting" (active). This shift formally moves the problem space from **Supervised Learning** (mapping $X \rightarrow Y$) to **Reinforcement Learning** or **Control Theory**, specifically modeled as a **Partially Observable Markov Decision Process (POMDP)**.

Although defined in Section 2.1 of the paper , the Introduction relies on the reader understanding this shift.

- **State ($S$):** The true state of the environment (e.g., the hidden bugs in a code repository).
    
- **Observation ($O$):** What the agent sees (e.g., a compiler error message).
    
- **Action ($A$):** The agent's intervention (e.g., editing a file).
    
- **Transition ($P$):** How the world changes based on the action ($S_{t+1} \sim P(S_t, A_t)$).
    

Standard LLMs (e.g., ChatGPT via web interface) often act as **Oracles**: they provide an answer based on internal weights. Agents act as **Controllers**: they emit actions to change the state of the world to maximize a reward function (e.g., passing unit tests).

#### 2.3.2 Diverse Domains and Their Computational Demands

The text highlights two specific domains to exemplify the high stakes of efficiency:

**A. Autonomous Software Engineering**

- **Context:** This refers to agents like **SWE-Agent**, **Devin**, or **CodeAgents**.
    
- **Workflow:** An agent must 1) Clone a repo, 2) Index the file structure (Memory), 3) Reproduce a bug (Planning), 4) Write a test case (Tool), 5) Modify code (Action), 6) Run the test (Observation).
    
- **Efficiency Implication:** A single task (e.g., "Fix Issue #402") might require reading 100 files and running the compiler 50 times. The context window fills with verbose code and error logs. If the agent is inefficient (e.g., re-reading the whole repo for every small edit), the cost becomes prohibitive.
    

**B. Accelerated Scientific Discovery**

- **Context:** This refers to systems like **ChemCrow** or the **AI Scientist**.
    
- **Workflow:** These agents perform literature reviews, formulate hypotheses, and potentially drive robotic lab equipment to synthesize molecules.
    
- **Efficiency Implication:** Here, "latency" is not just milliseconds; it includes the time of real-world experiments. However, the _cognitive_ efficiency involves managing massive datasets (e.g., protein structures) and ensuring that the agent does not hallucinate dangerous chemical combinations. The "Search Space" for molecules is vast ($10^{60}$), requiring highly efficient **Planning** and **Pruning** strategies.
    

---

## 3. The Efficiency Bottleneck: Structural Exacerbation

### 3.1 Verbatim Text Analysis

> _"However, this shift toward autonomous action has introduced a critical bottleneck: efficiency. While the deployment of LLMs is already resource-intensive, this challenge is significantly exacerbated in agentic systems."_

### 3.2 The Economics of Inference

To understand "resource-intensive," we must quantify the baseline cost of LLMs.

- **Parameter Count:** A 70B parameter model requires ~140GB of VRAM (at FP16 precision) just to load.
    
- **Memory Bandwidth Bound:** Inference is typically memory-bandwidth bound, not compute-bound. Generating one token requires reading the entire model from memory.
    
- **Quadratic Attention:** The cost of processing the prompt scales quadratically with sequence length $L$ ($O(L^2)$) due to the attention matrix calculation.
    

### 3.3 The Multiplier Effect of Agency

> _"Unlike a standard LLM that typically operates in a linear, single-turn query-response format, an agent consumes exponentially more resources due to its recursive nature."_

This is the central thesis of the efficiency problem. The authors use the term "recursive nature" to describe the agentic loop.

#### 3.3.1 Linear vs. Recursive Execution

- **Linear (Standard LLM):** User Prompt $\rightarrow$ Processing $\rightarrow$ Response.
    
    - Cost $\approx f(\text{Prompt Length} + \text{Response Length})$.
        
- **Recursive (Agent):**
    
    $$\text{Context}_{t+1} = \text{Context}_t + \text{Thought}_t + \text{Action}_t + \text{Observation}_t$$
    
    The agent calls the LLM iteratively. At step $t$, the input includes the _entire history_ of steps $0$ to $t-1$.
    

#### 3.3.2 The Mathematics of Context Accumulation

Let $C_0$ be the initial context (user instruction).

Let $k$ be the average tokens generated per step (thought + action).

Let $o$ be the average tokens returned by the tool (observation).

Let $N$ be the number of steps to solve the task.

The total token volume processed ($V_{total}$) over a trajectory is:

$$V_{total} = \sum_{t=0}^{N-1} \text{InputContext}_t$$

Since $\text{InputContext}_t = C_0 + t \times (k + o)$, the summation becomes:

$$V_{total} = \sum_{t=0}^{N-1} [C_0 + t(k+o)]$$

$$V_{total} = N \cdot C_0 + (k+o) \frac{N(N-1)}{2}$$

**Implication:**

The total cost scales **quadratically** with the number of steps $N$ ($O(N^2)$).

- If an agent takes 10 steps ($N=10$), it doesn't pay for 10 inferences; it pays for the accumulated history re-processed 10 times.
    
- **Exacerbation:** In domains like software engineering, $o$ (observation size) can be massive (e.g., a 5,000 line log file). This causes the $(k+o)$ term to explode, leading to **Context Window Saturation** (running out of tokens) and massive latency.
    

### 3.4 Operational Components of the Bottleneck

> _"To automate intricate real-world tasks , agents must perform extensive memory management, iterative tool usage, and complex planning over multiple steps."_

This sentence explicitly categorizes the sources of inefficiency, setting up the taxonomy for the rest of the paper.

|**Component**|**Nature of Inefficiency**|**Technical Bottleneck**|
|---|---|---|
|**Memory Management**|Storing and retrieving history.|** Retrieval Latency:** Vector DB lookups take time.<br><br>  <br><br>**Context Fragmentation:** Relevant info is buried in noise ("Lost in the Middle").|
|**Iterative Tool Usage**|Interacting with APIs.|**Network Latency:** Waiting for HTTP requests.<br><br>  <br><br>**Parsing Overhead:** Converting JSON tool outputs into tokens.|
|**Complex Planning**|Reasoning chains (CoT).|**Token Generation Cost:** "Thinking" generates tokens that are discarded after the final answer.<br><br>  <br><br>**Search Space:** Exploring multiple plan branches (Tree of Thoughts) multiplies inference calls.|

---

## 4. The Agentic Workflow: An Anatomical Decomposition

### 4.1 Verbatim Text Analysis

> _"To understand the urgency of agent efficiency, one must examine the typical agentic workflow. Upon receiving a user instruction, an agent engages in a recursive loop that heavily uses the following key components: memory, planning, and tool learning to observe output and provide the final solution."_

### 4.2 The Workflow Diagram and Analysis

The text describes a cyclic process:

**Input $\rightarrow$ Memory $\rightarrow$ Planning $\rightarrow$ Tool Learning $\rightarrow$ Observation $\rightarrow$ (Loop)**

This workflow is a reification of the cognitive architectures studied in cognitive science (e.g., OODA Loop) applied to LLMs.

#### 4.2.1 Phase 1: Input & Memory (Contextualization)

- **Process:** The User Instruction ($I$) acts as the query. The system queries its **Memory Module** to retrieve relevant context ($M$).
    
- **Efficiency Challenge:** The naive approach is to dump all history into the prompt. An efficient agent uses **Selective Retrieval** (discussed in Section 3 of the paper), fetching only the top-$k$ relevant memory chunks.
    
- **Technical Term:** **RAG (Retrieval-Augmented Generation)**. The agent augments its parametric memory (weights) with non-parametric memory (external store).
    

#### 4.2.2 Phase 2: Planning (Reasoning)

- **Process:** The agent processes $(I + M)$ to formulate a **Plan**. This involves breaking the goal into sub-tasks (Decomposition).
    
- **Mechanism:** This is often implemented via **Chain-of-Thought (CoT)** prompting, where the model generates intermediate reasoning steps ("Let's think step by step").
    
- **Efficiency Challenge:** CoT increases token count linearly. **Efficient Planning** (Section 5) involves strategies like **Plan Caching** (reusing old plans) or **Fast/Slow Thinking** (using a smaller model for simple plans).
    

#### 4.2.3 Phase 3: Tool Learning (Execution)

- **Process:** The Plan dictates an **Action**. If the action requires external data, the agent invokes a **Tool** (API).
    
- **Terminology Note:** The paper uses "Tool Learning" to encompass both _learning_ how to use tools (e.g., via fine-tuning like Toolformer) and the _act_ of using them (inference).
    
- **Efficiency Challenge:** **Hallucinated Calls:** The model calls a tool that doesn't exist. **Parameter Errors:** The model formats the JSON incorrectly. Both result in wasted round-trips.
    

#### 4.2.4 Phase 4: Observation (Feedback)

- **Process:** The environment executes the tool and returns an **Observation** (e.g., search results, code execution output).
    
- **Efficiency Challenge:** **Verbosity.** A simple `ls -la` command might return 100 lines of text. If fed directly back into the LLM, it consumes the context window. Efficient agents use **Observation Compression** or **Summarization** before feeding it back.
    

### 4.3 The Compounding Accumulation Loop

> _"In each iteration n, the system must first retrieve relevant context from memory, reason over the current state to formulate a plan, execute a specific tool-incorporated action, and process the resulting observation. This cycle creates a compounding accumulation of tokens, where the output of step n becomes the input cost of step $n+1$, resulting in high inference costs and slow response times."_

This paragraph reiterates the quadratic cost scaling derived in Section 3.3.2 but emphasizes the **operational consequence**: "high inference costs and slow response times."

**The Consequence: Sustainability and Accessibility**

> _"raising profound concerns regarding the long-term sustainability and equitable accessibility of these increasingly capable systems."_

- **Sustainability:** Energy consumption. Training a model is a one-time cost; running an inefficient agent loop billions of times creates a massive carbon footprint.
    
- **Accessibility:** If an agent costs $5.00 per query to run (due to thousands of tokens and multiple steps), it remains a luxury tool for enterprise, inaccessible to the general public or researchers in low-resource settings. This establishes the _ethical_ motivation for the survey.
    

---

## 5. Defining the "Efficient Agent"

### 5.1 Verbatim Definition and Analysis

> _"Efficient agent is not a smaller model, but as an agentic system optimized to maximize task success rates while minimizing resource consumption, including token usage, inference latency, and computational cost across memory, tool usage, and planning modules."_

This is the most critical conceptual contribution of the Introduction. It redefines efficiency from a model-centric view to a system-centric view.

#### 5.1.1 The Distinction: Model vs. System Efficiency

- **Efficient LLM (Model-Centric):** Focuses on **Quantization** (int8 inference), **Pruning** (removing weights), **Distillation** (Teacher $\rightarrow$ Student), and **Flash Attention** (kernel optimization). The goal is to make the `generate()` function faster.
    
- **Efficient Agent (System-Centric):** Focuses on the **Control Loop**. Even if the LLM is instantaneous, an agent that takes 100 bad steps is inefficient. An efficient agent might use a _larger, slower_ model if that model is smart enough to solve the task in 1 step instead of 10.
    

#### 5.1.2 The Optimization Objective: The Pareto Frontier

The definition implies a **Multi-Objective Optimization**:

$$\text{Maximize } \mathcal{R} \text{ (Success Rate)}$$

$$\text{Minimize } \mathcal{C} \text{ (Cost)}$$

where $\mathcal{C}$ is a vector: .

This invokes the concept of the **Pareto Frontier**, discussed in the "Preliminaries" (Section 2.2 of the paper) but introduced conceptually here. An agent is Pareto-efficient if no other agent configuration can achieve the same Success Rate for a lower Cost.

#### 5.1.3 The Cost Equation (From Preliminaries)

To fully appreciate the Introduction's definition, we must look at the mathematical formulation provided in Section 2.2 of the paper :

$$Cost_{agent} \approx \alpha N_{tok} + I_{tool} \cdot Cost_{tool} + I_{mem} \cdot Cost_{mem} + \Pi_{retry} \cdot Cost_{retry}$$

- $\alpha N_{tok}$: The cost of the LLM generation itself.
    
- $I_{tool}$: Indicator variable (1 if tools are used). $Cost_{tool}$ includes the API fee and latency.
    
- $I_{mem}$: Indicator variable for memory access. $Cost_{mem}$ includes retrieval latency.
    
- $\Pi_{retry}$: The number of retries triggered by failure.
    

The "Efficient Agent" minimizes this sum while maintaining the success rate. The survey categorizes methods that attack specific terms in this equation:

- **Efficient Memory** attacks $I_{mem}$ and $Cost_{mem}$.
    
- **Efficient Tool Learning** attacks $I_{tool}$ and $Cost_{tool}$.
    
- **Efficient Planning** attacks $\Pi_{retry}$ and $N_{tok}$ (by reducing step count).
    

---

## 6. Strategic Taxonomy: The Three Pillars of Efficiency

### 6.1 Verbatim Text Analysis

> _"Our survey aims to systematize the numerous efforts in this emerging field... we categorize existing works into three strategic directions..."_

### 6.2 Direction 1: Efficient Memory

> _"1) Efficient Memory: Techniques for compressing historical context, managing memory storage, and optimizing context retrieval."_

#### 6.2.1 Technical Deep Dive

- **Compression:** Instead of storing raw text strings ("The user asked for X"), efficient agents store **semantic embeddings** or **summaries**.
    
    - _Example:_ **MemGPT** uses a hierarchical structure where the "Main Context" is fixed, and older memories are paged out to storage, summarized, or recalled only when triggered.
        
- **Storage Management:** Moving from simple Lists (Python `list`) to structured databases (Vector DBs like Pinecone/Milvus, or Graph DBs).
    
    - _Graph Memory:_ Papers like **GraphReader** or **AriGraph** structure memory as a Knowledge Graph (Entities and Relations), enabling multi-hop reasoning that linear text cannot support.
        
- **Retrieval Optimization:** Standard RAG retrieves based on cosine similarity. Efficient agents use **Hybrid Search** (Keyword + Semantic) or **Active Retrieval** (the agent decides _when_ to search, rather than searching every turn).
    

### 6.3 Direction 2: Efficient Tool Learning

> _"2) Efficient Tool Learning: Strategies to minimize the number of tool calls and reduce the latency of external interactions."_

#### 6.3.1 Technical Deep Dive

- **Tool Selection (retrieval):** When an agent has 10,000 tools available, it cannot put all descriptions in the prompt. It must retrieve the top-5 relevant tools.
    
    - _Reference:_ **ToolKenGPT** or **AnyTool**.
        
- **Tool Calling (Execution):**
    
    - **Batching/Parallelism:** If the plan requires `Search(A)` and `Search(B)`, efficient agents execute them in parallel (using `async` calls) rather than sequentially.
        
    - _Reference:_ **LLMCompiler** compiles the plan into a DAG (Directed Acyclic Graph) of tasks to maximize parallelism.
        
- **Latency Reduction:** Using smaller, specialized models (fine-tuned) just for tool calling, rather than using a massive generalist model.
    
    - _Reference:_ **Toolformer** or **Gorilla**.
        

### 6.4 Direction 3: Efficient Planning

> _"3) Efficient Planning: Strategies to reduce the number of executing steps and API calls required to solve a problem."_

#### 6.4.1 Technical Deep Dive

- **Step Reduction:** Moving from **ReAct** (Reason+Act loop) to **Reflexion**. Reflexion allows the agent to "verbally reinforce" itself after a failure, storing a "lesson learned" so it doesn't repeat the mistake in the next episode. This reduces the _total_ steps over the agent's lifetime.
    
- **Search Mechanisms:**
    
    - **Tree of Thoughts (ToT):** Exploring multiple reasoning branches. While this _increases_ compute per step, it increases the success rate, potentially reducing the need for costly retries.
        
    - __A_ Search:_* Papers like **ToolChain*** use heuristic search algorithms to find the most efficient path through the tool space.
        
- **Fast/Slow Thinking:** **SwiftSage** mimics the human cognitive dual-process theory. A small "Fast" model handles routine actions (low cost), while a large "Slow" model is invoked only when the fast model struggles (high efficiency).
    

---

## 7. Structure of the Survey

### 7.1 Verbatim Text Analysis

> _"The remainder of this survey is organized as follows: Section 2 introduces the preliminaries... Sections 3 through 5 explore component-level efficiency... Subsequently, Section 6 addresses the quantification of efficiency. The survey concludes with a discussion on open challenges and future research directions."_

### 7.2 Implications of the Structure

- **Quantification (Section 6):** The authors emphasize that "Efficiency" is useless without metrics. Section 6 reviews benchmarks like **CostBench** and **TPS-Bench** (Tokens Per Second / Transactions Per Second). This highlights a shift in the community from purely accuracy-based leaderboards (like HuggingFace Open LLM Leaderboard) to **Cost-Aware Leaderboards**.
    
- **Challenges (Section 7):** The inclusion of "future directions" suggests the field is nascent. The analysis of open challenges likely covers **Unified Evaluation Protocols** (how to compare agents with different architectures?) and **Hardware-Software Co-design** (chips designed specifically for agentic loops).
    

---

## 8. Integrated Bibliographic Analysis

To fully satisfy the "missing information" requirement, we map the citation numbers mentioned in the Introduction to the specific papers identified in the Research Snippets.

|**Citations (Intro)**|**Category**|**Identified Papers & Context**|
|---|---|---|
|****|**Agent Emergence**|** Toolformer:** Self-supervised tool learning.<br><br>  <br><br>** Generative Agents:** Simulating human behavior.<br><br>  <br><br>** WebArena:** Benchmark for web agents.<br><br>  <br><br>**** Likely refers to the **GPT-3** or **InstructGPT** paper initiating the LLM era.|
|****|**Software Engineering**|** SWE-Bench:** Evaluating agents on GitHub issues.<br><br>  <br><br>**** Likely **Devin** or **CodeAgents** technical reports.|
|****|**Scientific Discovery**|** ChemCrow:** Agents for chemistry.<br><br>  <br><br>** AI Scientist:** Automated research paper generation.|
|****|**Real-World Tasks**|** GAIA:** A benchmark for General AI Assistants.<br><br>  <br><br>**** Likely **AutoGPT** or **BabyAGI** (early autonomous agent frameworks).|
|****|**Efficient LLMs**|These are surveys on _Model Compression_ (Quantization, Pruning), contrasted against the _System Efficiency_ of this paper.|

**Bibliographic Insight:** The Introduction strategically cites benchmarks (, ) alongside methods (, ). This signals that the efficiency crisis is empirical—observed in benchmarks—not just theoretical. The benchmarks proved that while agents _can_ solve tasks, they do so at a cost that prevents scaling.

---

## 9. Conclusion of the Exegesis

The Introduction of "Toward Efficient Agents" functions as a manifesto for a new sub-field of AI research: **Agentic Systems Engineering**. It successfully argues that the bottleneck for future AI is not the intelligence of the model (which is "already resource-intensive"), but the **operational efficiency of the recursive control loop**.

By defining efficiency as a Pareto optimization problem spanning Memory, Tools, and Planning, the authors provide a structured framework for dissecting the chaotic landscape of agent research. The transition from "Is it smart?" to "Is it sustainable?" marks the maturation of Agentic AI from research prototypes to production systems.

The subsequent sections of the survey (Memory, Tool Learning, Planning) are not random categories but are derived directly from the components of the recursive cost equation ($Cost_{agent}$) established in the Preliminaries. This logical coherence makes the survey a foundational text for researchers aiming to deploy agents in resource-constrained environments.

---
