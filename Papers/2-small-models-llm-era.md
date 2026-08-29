# Research Summary: What is the Role of Small Models in the LLM Era: A Survey

## TL;DR

This survey examines the evolving role of **Small Models (SMs)** in an AI landscape dominated by Large Language Models (LLMs). It categorizes the relationship between small and large models into **collaboration** and **competition/complementarity**. Small models can improve LLM efficiency through techniques such as speculative decoding and model routing, while also offering advantages in low-latency, resource-constrained, and specialized applications. An empirical evaluation using the **Massive Text Embedding Benchmark (MTEB)** shows diminishing performance returns as model size increases for some NLP tasks. The paper argues that SMs are strategic components of future AI ecosystems rather than merely cheaper alternatives to LLMs.

## Paper Information

| Field | Details |
|---|---|
| **Title** | *What is the Role of Small Models in the LLM Era: A Survey* |
| **Authors** | Lihu Chen and Gaël Varoquaux |
| **Year** | 2026 |
| **Research area** | Natural Language Processing, Model Efficiency, Machine Learning |
| **Primary focus** | Role and applications of Small Models in the LLM era |
| **Empirical benchmark** | Massive Text Embedding Benchmark (MTEB) |

## One-Paragraph Summary

This paper presents a comprehensive survey of the evolving role of Small Models (SMs) in a landscape dominated by Large Language Models (LLMs). The authors define SMs relatively based on parameter count and categorize their relationship with LLMs into two primary paradigms: **Collaboration** and **Competition/Complementarity**. Collaboration involves bidirectional assistance, where LLMs enhance SMs through knowledge distillation and synthetic data generation, while SMs assist LLMs through speculative decoding and model routing to improve inference efficiency. In the competition/complementarity paradigm, SMs are preferred in computation-constrained edge devices, low-latency applications, and specialized domains such as healthcare and law where interpretability is important. An empirical evaluation using the **Massive Text Embedding Benchmark (MTEB)** demonstrates diminishing returns in performance as model size increases for certain tasks. The authors conclude that SMs are strategic, cost-effective components of future AI ecosystems and agentic systems.

## Why This Paper Matters

The rapid scaling of LLMs has produced increasingly capable models, but also brings substantial computational costs, energy consumption, and hardware requirements. These constraints can make large models impractical for:

- Academic researchers with limited computational resources.
- Small businesses.
- Edge devices.
- Applications requiring low latency.
- Systems requiring localized processing.

The paper provides a systematic framework for understanding why smaller models remain important despite the capabilities of modern LLMs.

Rather than treating SMs simply as weaker versions of LLMs, the survey highlights their distinct advantages in:

- **Efficiency**
- **Low-latency inference**
- **Specialized domains**
- **On-device deployment**
- **Interpretability**
- **Cost-sensitive applications**
- **LLM acceleration and routing**

## Research Problem

### Problem Addressed

The paper addresses the growing computational and economic costs associated with increasingly large LLMs and the resulting need to understand the role of smaller models.

It investigates how SMs can:

- Complement large models.
- Collaborate with large models.
- Compete with large models in specialized settings.
- Reduce inference costs.
- Enable deployment on resource-constrained hardware.

### Why This Problem Is Important

LLMs can be excessive for relatively simple tasks, while their deployment can introduce significant financial, computational, and environmental costs.

Small models can provide more practical solutions when applications require:

- Real-time responses.
- Low computational overhead.
- Localized processing.
- Resource-constrained deployment.
- Greater interpretability.

### Gap in Previous Research

Previous surveys have often focused on:

- Scaling LLMs.
- Model compression.
- Individual efficiency techniques.

The authors identify a lack of systematic analysis covering the **synergistic relationship between small and large models**, as well as the unique strengths that SMs can provide.

## Main Contributions

### 1. Systematic Framework

The paper provides a structured categorization of SM roles, distinguishing between:

- **Collaboration with LLMs**
- **Competition/complementarity with LLMs**

### 2. Relative Definition of Small Models

Instead of defining SMs using a fixed parameter threshold, the paper defines them **relative to a reference model**.

This makes the definition more adaptable as model scales continue to evolve.

### 3. Empirical Benchmarking

The authors use the **Massive Text Embedding Benchmark (MTEB)** to analyze how performance changes with model size across multiple NLP tasks.

### 4. Future-Oriented Roadmap

The survey identifies open challenges and future directions, including:

- Small models in agentic systems.
- Open-world model collaboration.
- Efficient model ecosystems.

## Methodology

The paper combines a **systematic literature review** with an empirical evaluation.

### Survey Scope

The authors review research covering:

- Model compression.
- Knowledge distillation.
- Speculative decoding.
- Model routing.
- Domain-specific model applications.

### Architectures

The survey covers a broad range of architectures, including:

- Encoder-only Transformer models.
- Decoder-only Transformer models.
- Other neural architectures.
- Statistical architectures.

## Empirical Evaluation

### Benchmark

The empirical analysis uses the **Massive Text Embedding Benchmark (MTEB)**.

### Tasks

The evaluation covers four task categories:

- Text similarity.
- Text classification.
- Information retrieval.
- Text clustering.

Average performance is reported over five datasets for each task.

### Model Download Analysis

The authors also analyze open-source NLP models on HuggingFace, categorizing models into size groups:

- `[200M, 500M, 1B, 6B]`

This is used to study model download trends.

## Key Results

### Diminishing Returns

MTEB results show **diminishing returns in performance as model size increases** for tasks such as:

- Text similarity.
- Text classification.

This suggests that increasing model size does not always produce proportional improvements.

### Information Retrieval Efficiency

Lightweight encoder-based models remain widely used for **Information Retrieval (IR)** because they provide important speed advantages when computing similarity between queries and large document collections.

### Small Model Usage Trends

Analysis of HuggingFace download data shows high download volumes for models in the smaller size groups, indicating substantial practical demand despite the broader focus on LLMs.

### Agentic Systems

Industry research cited by the paper predicts that by **2028**, approximately **68% of customer-service interactions** will be processed by agentic systems.

The survey argues that SMs are particularly suitable for specialized and repetitive tasks within such systems.

## Key Findings

1. **Relative scaling matters.** A model's importance should be evaluated relative to the task and hardware constraints rather than by absolute parameter count alone.
2. **Small and large models can work together.** Collaboration is bidirectional: LLMs can improve SMs, while SMs can accelerate or route work for LLMs.
3. **Specialization can beat scale.** SMs fine-tuned on specialized datasets, such as biomedical or legal data, can outperform general-purpose LLMs on relevant tasks.
4. **Interpretability is an advantage.** Compact models can be easier to audit and interpret, making them attractive in sensitive domains.
5. **SMs have robustness challenges.** Smaller models are generally more susceptible to distribution shifts and catastrophic forgetting.
6. **Efficiency is a first-class capability.** SMs provide advantages in latency, computational cost, and deployment flexibility.
7. **Model ecosystems are increasingly important.** The future can involve multiple models of different sizes working together rather than relying exclusively on a single large model.

## Important Concepts

### Knowledge Distillation

A process in which a large **teacher model** transfers knowledge to a smaller **student model**, often by training the student to reproduce the teacher's output probabilities.

### Speculative Decoding

A technique for accelerating LLM inference in which a small model quickly generates draft tokens that are then verified or corrected by a larger model in parallel.

### Model Routing

An architecture in which a lightweight classifier determines whether a query can be handled by a cheaper small model or should be sent to a more expensive large model.

### Data Pruning

The strategic removal of redundant or low-quality data from pre-training datasets to improve the efficiency of training smaller models.

### Agentic Systems

AI frameworks in which models act as agents that perform specific, often repetitive, multi-step tasks autonomously.

## Takeaways

- **Efficiency is strategic.** SMs are not merely fallback options for limited budgets; they can be essential to high-speed, scalable, and sustainable AI systems.
- **Specialization can matter more than size.** A well-tuned SM can outperform a general-purpose LLM on specialized tasks.
- **Collaboration is key.** Future AI systems can use models of different sizes to balance performance and cost.
- **Some capabilities remain scale-dependent.** Certain reasoning capabilities emerge at larger scales and may be difficult for SMs to reproduce through distillation alone.
- **Edge deployment favors SMs.** Smaller models are important for on-device AI because of their efficiency and suitability for localized processing.

## Limitations

### Explicitly Mentioned by the Authors

- **Capability gap:** A persistent performance gap remains between SMs and LLMs, particularly for complex knowledge reasoning tasks.
- **Robustness:** SMs tend to be less robust to perturbations and distribution shifts during real-world deployment.
- **Evaluation bias:** Small models used as judges can be sensitive to answer ordering and may exhibit significant biases.
- **Knowledge boundary estimation:** Current routing approaches have difficulty generalizing to open-world settings where model capabilities are not predefined.

### Inferred Limitations

- **Model selection:** The empirical evaluation focuses primarily on text embedding models through MTEB and may not fully represent generative SMs in reasoning or creative tasks.
- **Architecture evolution:** Although the relative definition of SMs is adaptable, some techniques discussed, such as speculative decoding, are strongly centered on the current Transformer paradigm and may require significant adaptation for future architectures.

## Critical Evaluation

### Strengths

- Provides a timely and clear taxonomy of the relationship between LLMs and SMs.
- The **relative definition of small models** is a strong conceptual contribution because it remains applicable as model scales change.
- Combines a broad literature survey with quantitative MTEB results.
- Highlights both competitive and collaborative roles for small models.
- Connects model efficiency with practical deployment considerations.
- Uses open-source models and well-documented benchmarks, supporting reproducibility.

### Weaknesses

- The discussion of robustness issues could provide more depth on mitigation strategies beyond knowledge distillation.
- The empirical evaluation is valuable but relatively narrow in scope.
- MTEB primarily evaluates text embedding models and therefore does not capture every capability relevant to generative small models.

### Experimental Quality

**Solid.** MTEB is a standard benchmark for evaluating text embeddings and provides a quantitative foundation for the survey's discussion of model-size scaling.

### Reproducibility

The empirical analysis uses well-documented benchmarks and open-source models, making the reported analysis largely reproducible.

### Support for Conclusions

The conclusions are supported by both the extensive literature review and the MTEB performance trends showing diminishing returns for larger models on specific tasks.

## Bottom Line

The paper argues that **small models remain strategically important in the LLM era**. Their value comes not only from lower parameter counts but from their efficiency, specialization, low latency, interpretability, and ability to collaborate with larger models.

The survey presents a future in which AI systems are not necessarily built around one massive model. Instead, **ecosystems of models with different sizes and capabilities** can work together, using small models for specialized or efficient tasks and large models where their additional capabilities are necessary.

## Further Reading

The source material provides one reference:

1. L. Chen and G. Varoquaux, *"What is the Role of Small Models in the LLM Era: A Survey,"*.