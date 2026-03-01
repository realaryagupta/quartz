**Summary:** This session explores a shift from pure function approximation (Deep Learning) to "Active Inference" and object-centered architectures, arguing that AGI requires models grounded in macroscopic physics and causal relationships to achieve human-like systems engineering capabilities and robustness.

---

## The Bayesian Brain & Empirical Inquiry

The speaker argues that the brain operates fundamentally as a Bayesian inference engine, encapsulating the scientific method through iterative hypothesis testing and generative modeling.

### Core Concepts

- **Normative Empirical Inquiry:** Bayesian inference provides a mathematical framework for how science _should_ work: gathering data, comparing it to priors, and updating internal models.
    
- **Generative Models:** Unlike discriminative models (which map input $\rightarrow$ class), the brain builds generative models of the world conditioned on hypotheses. It asks, "What underlying process caused this sensory data?".
    
- **Cue Combination:**
    
    - **Definition:** The brain integrates multiple sensory inputs (e.g., vision + audio) weighted by their reliability.
        
    - **Optimality:** Behavioral experiments show humans are "relatively optimal" in combining cues trial-by-trial, effectively managing uncertainty.
        
    - **Information Filtering:** 90% of the brain's job is deciding what to _ignore_. Processing power is focused on relevant affordances rather than raw throughput.
        

### Technical Insight: The "Context" Overhead

- **Latency vs. Fidelity:** While we filter out most data, we maintain low-level statistics (priors) via constant input to allow for flexible task switching. This parallels **self-supervised learning** in LLMs—maintaining a broad probability distribution over the domain just in case specific context is needed later.
    

---

## Causal Modeling: Micro vs. Macro

A critical distinction is made between "ground truth" physics (micro) and the actionable world of agents (macro).

### Instrumental Causality

- **Affordances over Truth:** We prefer causal models not because they are "true" at the quantum level, but because they align with our **affordances** (actions we can take).
    
- **Macroscopic Variables:**
    
    - In physics, we track _momentum_ not because we observe it, but because it renders the system **Markovian** (current state completely predicts the next state), simplifying computation.
        
    - **Downward Causation:** A valid macroscopic object (e.g., a "table") is justified if its macro-behavior predicts the system's evolution without needing to simulate every atom.
        

> **System Design Note:** This aligns with **Domain-Driven Design (DDD)**. You model the "Aggregate Root" at the level of business logic consistency, not at the database row level. Over-modeling micro-interactions leads to brittle systems.

---

## The Engineering Turn in AI

The explosion of AI is attributed less to specific architectures (like Transformers) and more to the commoditization of the development loop.

### Autograd: The Unsung Hero

- **Shift to Engineering:** **Autograd** (automatic differentiation) transformed AI from a mathematical derivation problem into an engineering problem. It allowed rapid experimentation with architectures without manually deriving backward passes.
    
- **Transformer Critique:** The success of Transformers is attributed partly to **scaling laws** rather than inherent architectural magic.
    
    - _Evidence:_ **Mamba** (a State Space Model/RNN variant) achieves similar performance to Transformers when scaled, suggesting scaling is the dominant factor.
        
- **The Function Approximation Trap:** Current AI focuses on function approximation (fitting curves to data). The speaker argues this lacks the structure required for reasoning, planning, and systems engineering.
    

---

## Object-Centered Architectures & Active Inference

To move beyond "stochastic parrots," the speaker proposes architectures that mirror the sparse, object-oriented nature of reality.

### The "Lots of Little Models" Approach

Instead of one monolithic model (like GPT-4) trying to learn everything implicitly, use a distributed system of specialized models.

- **Sparsity & Compositionality:** The world is composed of objects (books, cats, forklifts) with specific interaction rules (forces).
    
- **Efficiency:** A "warehouse agent" only needs to load the subset of models relevant to a warehouse. It doesn't need to know about underwater basket weaving unless it encounters a basket underwater.
    
- **Generalization via Composition:** Just as systems engineers combine a wing and an engine to make a plane, an AI should combine learned object models to solve novel problems. Standard DL struggles here because it interpolates rather than composes.
    

### Scaling Bayesian Inference

Historically, Bayesian methods failed to scale. New techniques are bridging the gap:

1. **Normalizing Flows:** Mapping complex probability distributions to simple ones (like Gaussians) to make inference tractable.
    
2. **Natural Gradient Descent (Amari):** Adjusting updates based on the geometry of the probability space (Fisher Information Matrix) rather than just Euclidean parameter space.
    

> **Practical Application:** In **Robotics**, training on "Lots of Little Models" prevents the **Sim-to-Real gap**. If a robot learns the _physics_ of interaction (forces) rather than just mimicking trajectories (Behavior Cloning), it can adapt to friction changes or new object weights.

---

## The "Cat in the Warehouse" Problem (OOD Detection)

A key test for robust AI is handling Out-Of-Distribution (OOD) events.

### The Scenario

A warehouse robot trained on boxes and forklifts suddenly encounters a cat.

- **Standard DL:** Likely hallucinates a box or drives over the cat because it forces the input into a known class.
    
- **Active Inference/Bayesian:**
    
    1. **Surprisal Signal:** The system detects high "surprisal" (prediction error)—the sensory data does not fit any loaded model (box, forklift).
        
    2. **Model Selection:** The agent pauses and queries a cloud repository ("phone a friend") containing a vast bank of models.
        
    3. **Hypothesis Testing:** It downloads candidate models (Cat? Dog? Raccoon?), observes the entity's motion to validate the "Cat" model, and instantiates it into the local environment.
        

---

## Alignment via Belief Exchange

The speaker rejects "Reward Shaping" (RLHF) as the primary alignment mechanism, calling it "wishing on a malevolent genie."

### The Alignment Protocol

- **Ambiguity of Action:** Observing an action is insufficient because Action is a conflation of **Belief + Value**. You cannot mathematically separate them just by watching.
    
- **Dialectic Alignment:** Humans align by exchanging beliefs ("Why did you do that?"). By synchronizing **World Models** (beliefs), any remaining discrepancy in action can be isolated to a difference in **Values** (Reward Function).
    
- **Safety Implication:** We should aim for AI that can explain its _belief structure_, allowing us to correct its causal model of the world, rather than just punishing bad outputs.
    

---

## Key Takeaways

- **Autograd > Architectures:** The AI boom is driven by the ability to iterate as engineers (thanks to Autograd) rather than the specific superiority of Transformers over RNNs/SSMs.
    
- **Object-Centricity is Key to AGI:** To achieve systems engineering capability (inventing new things), AI must model the world as composed of interacting objects with causal physics, not just token correlations.
    
- **Bayesian Scaling is Solved:** Techniques like Normalizing Flows and Natural Gradient Descent (BONG) now allow Bayesian inference to scale to deep learning levels.
    
- **Handle OOD via Surprisal:** Robust systems must track "surprisal" to detect when their current model set is failing, triggering a search for new models (The Cat in the Warehouse).
    
- **Alignment Requires Language:** You cannot align an agent solely through reward signals; you must synchronize beliefs through communication to isolate value differences.

----

### Papers:

- Zoubin Ghahramani (Google DeepMind)
- Mamba: Linear-Time Sequence Modeling
- xLSTM: Extended Long Short-Term Memory 
- 3D Gaussian Splatting 
- Lenia: Biology of Artificial Life 
- Karl Friston (UCL)
- The Genomic Bottleneck 
- Growing Neural Cellular Automata
- DreamCoder
- Person
