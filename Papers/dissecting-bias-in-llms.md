# Research Summary: Dissecting Bias in LLMs: A Mechanistic Interpretability Perspective

## TL;DR

This paper investigates how demographic and gender biases are encoded inside GPT-2 Small, GPT-2 Large, and Llama-2 using **Edge Attribution Patching (EAP)**. The authors find that bias is concentrated in specific layers and edges rather than being uniformly distributed. Ablating the top 40% of identified edges reduces bias metrics by more than 90% in GPT-2 Small and Llama-2, while GPT-2 Large requires 60%. However, the identified bias circuits are unstable under grammatical and lexical changes and can overlap with circuits needed for general language tasks. The proposed inference-time debiasing method can therefore reduce bias without parameter updates, but may degrade model utility.

## Paper Information

| Field | Details |
|---|---|
| **Title** | *Dissecting Bias in LLMs: A Mechanistic Interpretability Perspective* |
| **Authors** | Bhavik Chandna (University of California San Diego), Zubair Bashir (Indian Institute of Technology, Kharagpur), Procheta Sen (University of Liverpool) |
| **Year** | 2025 |
| **Research area** | Mechanistic Interpretability (MI) of Large Language Models (LLMs), focusing on demographic and gender biases |
| **Models** | GPT-2 Small, GPT-2 Large, Llama-2 |
| **Primary method** | Edge Attribution Patching (EAP) |
| **Secondary method** | Sparse Autoencoders (SAE) |

## One-Paragraph Summary

The research investigates the internal computational mechanisms of Large Language Models (LLMs) to determine how demographic and gender biases are encoded within their neural architectures. Using **Edge Attribution Patching (EAP)**, a causal intervention framework, the authors identify specific computational edges responsible for biased outputs. Bias is found to be localized within particular layers and a relatively small subset of edges: ablating the top 40% of edges in GPT-2 Small and Llama-2 reduces bias metrics by more than 90%, while GPT-2 Large requires 60%. At the same time, these bias-related circuits are unstable under grammatical variations, lexical perturbations, and fine-tuning. They can also overlap with circuits supporting general linguistic capabilities, meaning that corrupting them may reduce downstream performance on tasks such as named entity recognition and linguistic acceptability. The authors propose a training-free, inference-time debiasing strategy that reduces bias without parameter updates, while highlighting the trade-off between bias reduction and model utility.

## Why This Paper Matters

LLMs are increasingly integrated into high-stakes domains such as education, healthcare, law, and content moderation. Bias in these systems can contribute to unfair treatment, systemic discrimination, and misinformation.

This paper is important because it moves beyond simply measuring bias and asks **where and how bias is implemented inside the model**. In particular, it examines:

- Whether bias can be localized to specific computational edges and layers.
- Whether those bias-related circuits remain stable under changes in prompts or model state.
- Whether bias circuits are distinct from circuits supporting general language capabilities.
- Whether localized interventions can reduce bias without retraining the model.

The work therefore connects **causal mechanistic analysis** with practical bias mitigation.

## Research Problem

### Problem Addressed

The paper investigates demographic and gender biases in LLMs through the lens of Mechanistic Interpretability. It asks whether these biases can be localized to distinct computational substructures and evaluates their:

- **Stability** under perturbations.
- **Generalizability** across prompt variations and models.
- **Functional overlap** with broader linguistic capabilities.

### Gap in Previous Research

The analysis identifies several gaps:

- Previous debiasing approaches have largely relied on fine-tuning or data augmentation, which can be computationally expensive and lack transparency.
- Earlier MI research often focused narrowly on individual neurons or attention heads.
- Existing studies were frequently limited to a single model or bias type.
- There is limited understanding of whether bias-related components generalize across prompts, remain stable under perturbation, or overlap with circuits supporting other language tasks.

## Main Contributions

### 1. Localized Encoding of Bias

Demographic and gender biases are shown to be concentrated in specific edges and layers rather than uniformly distributed throughout GPT-2 and Llama-2.

### 2. Analysis of Circuit Instability

Edges identified as important for bias are sensitive to:

- Grammatical changes.
- Lexical changes.
- Model fine-tuning.

This suggests that bias circuits are not necessarily robust, universal components.

### 3. Functional Entanglement with Language Tasks

The paper measures overlap between bias-related circuits and circuits required for general language tasks. Corrupting bias-related edges can negatively affect:

- Linguistic acceptability (**CoLA**).
- Named entity recognition (**CoNLL-2003**).

### 4. Inference-Time Debiasing

The authors propose a training-free strategy that uses EAP to identify important edges and substitutes their activations with those from a corrupted input while keeping other activations clean. This enables bias mitigation without gradients or parameter updates.

## Methodology

### Edge Attribution Patching

**Edge Attribution Patching (EAP)** is the primary method for identifying important computational edges. It estimates an edge's causal importance by approximating the change in a task metric `L` when a corrupted activation is patched into that edge.

EAP is computationally efficient, requiring:

- Two forward passes.
- One backward pass.

### Sparse Autoencoders

**Sparse Autoencoders (SAEs)** are used as a secondary method to validate localization and overlap findings by decomposing model activations into interpretable features.

## Model Architecture

| Model | Parameters | Layers | Attention Heads / Layer | Nodes | Edges |
|---|---:|---:|---:|---:|---:|
| **GPT-2 Small** | 85M | 12 | 12 | 158 | 32,491 |
| **GPT-2 Large** | 708M | 36 | 20 | 758 | 810,703 |
| **Llama-2** | 6.5B | 32 | 32 | 1,058 | 1,592,881 |

## Datasets

- **Demographic Bias Dataset:** Names representing all 224 global nationalities, used to study nationality-based sentiment bias.
- **Gender Bias Dataset:** 320 professions annotated as gender-specific or neutral.
- **CoLA:** Corpus of Linguistic Acceptability, used as a downstream task.
- **CoNLL-2003:** Named Entity Recognition dataset, used as a downstream task.

## Experimental Setup

### Prompt Sets

The study uses:

- **DSS1 and DSS2:** Two sentence structures for demographic bias.
- **GSS1 and GSS2:** Two sentence structures for gender bias.

These prompt variations are used to test grammatical generalizability.

### Sentiment Analysis

A **`Distilbert-base-uncased`** model classifies the sentiment of top-k next-token predictions for demographic prompts.

### Hardware and Framework

Experiments were conducted using:

- Two NVIDIA A100 GPUs.
- The **`Hooked-Transformer`** framework.

## Baselines

- **Evaluate-Baseline:** Computes the average probability difference between positive/male and negative/female tokens across the full dataset.
- **Evaluate-Graph:** Evaluates the unmodified computational graph with all edges intact to establish a performance ceiling.

## Evaluation Metrics

| Metric | Purpose |
|---|---|
| **`L1`** | Measures relative skew using the difference in aggregate probabilities for positive/male versus negative/female tokens among top-k predictions. |
| **`L2`** | Unidirectional metric based only on cumulative probability assigned to positive/male tokens. |
| **Downstream Delta (`δ`)** | Percentage change in CoLA and NER performance after edge corruption. |

## Key Results

### Bias Localization

- **GPT-2 Small:** Ablating the top **40%** of EAP-identified edges reduced the bias metric by **more than 90%**.
- **Llama-2:** Ablating the top **40%** of identified edges also reduced the bias metric by **more than 90%**.
- **GPT-2 Large:** Required **60%** of the top edges to achieve a similar **90%** reduction.

### Layer-Wise Findings

| Model | Layers Associated with Bias |
|---|---|
| **GPT-2 Small** | Layers **2–6** |
| **GPT-2 Large** | Layers **9, 10, 20, 34, 35** |
| **Llama-2** | Layers **0–11 and 30–31** |

### Bias–Task Trade-offs

| Model | Bias Result | Downstream Impact |
|---|---|---|
| **GPT-2 Small** | Demographic bias reduced by approximately **30–36%** | CoLA performance dropped by up to **22.6%**; NER dropped by up to **20.4%** |
| **Llama-2** | Demographic bias reduced by approximately **35%** for DSS2 | Negligible downstream performance drop of **0.01%** |
| **GPT-2 Large** | DSS1 bias **increased by 8.89%** after corruption | Downstream task performance also decreased |

These results demonstrate that reducing bias through structural intervention does not consistently preserve general model performance.

### Stability and Circuit Overlap

- Positive and negative biases of the **same type** show significant overlap.
- Different bias types, such as demographic and gender bias, show **minimal overlap**.
- Demographic bias circuits show **minimal edge overlap** between DSS1 and DSS2.
- Gender bias circuits show greater structural consistency between GSS1 and GSS2.

## Key Takeaways

1. **Bias is highly localized.** Demographic and gender biases are concentrated in particular computational edges and layers.
2. **Bias circuits are unstable.** The important edges can change substantially when prompt phrasing, lexical choices, or model parameters change.
3. **Different biases have different circuits.** Demographic and gender bias circuits show little overlap.
4. **Bias circuits are functionally entangled.** Edges associated with bias can also support general linguistic tasks.
5. **Larger models can be more resilient.** Llama-2 experienced much smaller downstream performance penalties than GPT-2 Small under the reported interventions.
6. **EAP enables causal analysis efficiently.** It provides a way to identify important edges without exhaustive ablation.
7. **Debiasing has trade-offs.** Targeted edge corruption can reduce bias without parameter updates, but may reduce general model utility.
8. **Localization does not guarantee stability.** Finding where bias resides does not necessarily mean that the same circuit will remain responsible for bias under different prompts or model states.

## Important Concepts

### Mechanistic Interpretability (MI)

A field that aims to reverse engineer neural networks into human-understandable algorithms by identifying the components and circuits responsible for specific behaviors.

### Nodes and Edges

In the MI framework used by the paper:

- **Nodes** represent components such as attention heads or MLP layers.
- **Edges** represent information flow between those components.

### Edge Attribution Patching (EAP)

A method for estimating the causal influence of an edge by using gradients with respect to activations to approximate the effect of replacing a clean activation with a corrupted one.

### Symmetric Token Replacement (STR)

A corruption technique in which a specific token, such as a nationality, is replaced with another token of the same length or with a neutral string. This measures model sensitivity to the input while preserving the overall sequence structure.

### Sparse Autoencoders (SAE)

A technique for identifying interpretable features in high-dimensional neural activations by reconstructing activations through a bottleneck with only a small number of active features.

## Limitations

### Explicitly Mentioned by the Authors

- **Restricted bias scope:** Only demographic (nationality) and gender bias are examined; other forms of bias are not explored.
- **Non-transferability:** Circuits identified for one bias type do not necessarily transfer to another, potentially requiring a separate circuit analysis for each bias.
- **Performance degradation:** Edge corruption can negatively affect unrelated tasks such as NER.
- **Perturbation sensitivity:** Bias-related edges can change when the input text or model state is modified.

### Inferred Limitations

- **Prompt dependency:** Bias classification relies heavily on the specific DSS/GSS prompts and may not capture the full complexity of bias in open-ended generation.
- **Hardware sensitivity:** Although EAP is efficient, reproducing exhaustive edge-level analyses for larger models such as Llama-2 may be challenging on consumer-grade hardware.
- **Approximation errors:** EAP relies on a linear approximation of causal importance and may miss nonlinear interactions between edges.

## Critical Evaluation

### Strengths

- Provides a multi-model analysis spanning **GPT-2 Small, GPT-2 Large, and Llama-2**.
- Combines EAP with SAE-based analysis.
- Goes beyond simple localization by examining stability under grammatical variation and fine-tuning.
- Uses standardized datasets and clearly defined metrics.
- Provides architecture specifications, hyperparameters, hardware details, and a public code repository, supporting reproducibility.
- Extensive ablation and perturbation experiments support the conclusions about localization and instability.

### Weaknesses

- The study examines only a limited set of bias types and prompts.
- The inference-time debiasing strategy is not consistently effective across settings.

### Experimental Quality

**High.** The analysis uses standardized datasets, defined `L1` and `L2` metrics, multiple model scales, and extensive causal intervention experiments.

### Reproducibility

The authors provide detailed architecture specifications, hyperparameters, hardware information, and released code, making the study highly reproducible according to the analysis.

### Support for Conclusions

The conclusions regarding **bias localization and circuit instability** are well supported by the ablation and perturbation experiments. The effectiveness of the proposed debiasing strategy is more variable, and the reported GPT-2 Large DSS1 result demonstrates this limitation.

## Further Reading

The source material provides one reference:

1. B. Chandna, Z. Bashir, and P. Sen, *"Dissecting Bias in LLMs: A Mechanistic Interpretability Perspective,"*.

## Bottom Line

The paper argues that demographic and gender biases in LLMs can be traced to specific computational edges and layers, making causal, inference-time intervention possible. However, these circuits are unstable and can overlap with general language capabilities, creating a fundamental trade-off between **bias reduction and model utility**. The results suggest that mechanistic localization is a promising tool for understanding LLM bias, but reliable debiasing requires accounting for prompt sensitivity, circuit instability, and functional entanglement.