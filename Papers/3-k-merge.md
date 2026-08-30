# K-Merge: Online Continual Merging of Adapters for On-device Large Language Models

## TL;DR

**K-Merge** addresses the problem of managing many LoRA adapters on resource-constrained devices with a fixed storage budget. It continually merges incoming adapters based on their similarity while using a history-aware running average to avoid favoring newer tasks. **K-Merge++** adds a similarity threshold to preserve storage capacity for future diverse tasks. Across Llama-3.2 and Qwen-2.5 models, eight languages, and five task types, K-Merge consistently outperforms standard merging baselines such as TIES and DARE while requiring only **0.04-0.18 seconds** for adapter integration.

## Paper Information

| Field | Details |
|---|---|
| **Title** | *K-Merge: Online Continual Merging of Adapters for On-device Large Language Models* |
| **Authors** | Donald Shenaj, Ondrej Bohdal, Taha Ceritli, Mete Ozay, Pietro Zanuttigh, Umberto Michieli |
| **Year** | 2026 |
| **Research Area** | On-device Large Language Models (LLMs), Parameter-Efficient Fine-Tuning (PEFT), Model Merging |

## One-Paragraph Summary

The paper introduces **K-Merge**, a framework for managing Low-Rank Adapters (LoRAs) in resource-constrained, on-device environments. Instead of storing a separate adapter for every task, K-Merge continually integrates incoming task adapters into a fixed budget of **K multi-tasking adapters**. Incoming LoRAs are compared using cosine similarity and either stored in an available slot or merged with the most similar existing adapter. A history-aware running average prevents recently arriving adapters from dominating previously merged tasks. **K-Merge++** extends this approach with a similarity threshold calibrated using an auxiliary LoRA dataset, helping preserve storage for diverse future tasks and improving robustness to unfavorable task arrival orders. Experiments using Llama-3.2 and Qwen-2.5 across eight languages and multiple problem types show that K-Merge consistently outperforms standard merging baselines such as TIES and DARE.

## Why This Paper Matters

On-device LLM deployment faces strict **storage and memory constraints**. Although LoRA makes task-specific fine-tuning relatively lightweight, maintaining a separate adapter for every language, user preference, or specialized capability can still become impractical.

K-Merge addresses this by maintaining a **fixed number of adapters** and grouping similar tasks together. This makes it possible to support a larger variety of capabilities without storing every individual adapter.

The approach is particularly relevant to scenarios where:

- Tasks arrive **sequentially**.
- Future tasks are **unknown in advance**.
- Original adapters or training data may no longer be available after merging.
- Storage capacity is strictly limited.
- Adapter integration needs to be computationally lightweight.

## Research Problem

### Problem

The paper formalizes the problem of **online continual merging of LoRA adapters** for on-device LLMs under a fixed storage budget.

Tasks arrive sequentially, and the system must:

1. Incorporate new capabilities.
2. Preserve performance on previously encountered tasks.
3. Operate within a fixed budget of **K adapters**.

### Research Gap

Existing PEFT approaches generally assume a static task setting. Conventional model-merging methods such as **TIES** and **DARE** also assume that all task-specific adapters are available simultaneously for a single merging operation.

This does not match the on-device scenario considered by the paper, where:

- Tasks arrive one at a time.
- Future tasks are unknown.
- Previous adapters may no longer be available.
- Training data may no longer be accessible.
- Storage is limited to a fixed number of adapters.

## Main Contributions

### 1. A New Practical Setting

The authors formalize **online continual merging of adapters**, reflecting the sequential task-arrival setting of on-device LLMs.

### 2. K-Merge and K-Merge++

The paper introduces a lightweight, data-free merging framework based on:

- Adapter similarity scoring.
- Clustering similar adapters.
- History-aware running averages.

**K-Merge++** additionally introduces an adaptive similarity threshold to manage the storage budget more effectively.

### 3. Comprehensive Evaluation

The method is evaluated using:

- Llama-3.2 and Qwen-2.5.
- Eight languages.
- Five task types.
- Storage budgets from **K = 1 to 8**.
- Multiple task orderings.

## Methodology

K-Merge follows a sequential pipeline for comparing, storing, clustering, and merging LoRA adapters.

### Step 1: Similarity Scoring

For an incoming adapter, the system compares it against the stored adapters.

The matrices are flattened into vectors, and the system computes the **average cosine similarity** between the incoming adapter and each stored adapter.

### Step 2: Clustering and Merging

If the storage budget has not been reached:

> The incoming adapter is stored in a new slot.

If the budget is full:

> The incoming adapter is merged with the stored adapter having the highest similarity.

### Step 3: K-Merge++ Thresholding

K-Merge++ introduces a similarity threshold.

A new adapter is merged only when its similarity with the closest stored adapter exceeds the threshold. Otherwise, it is stored separately if capacity remains.

The threshold is calibrated using the **median similarity from an auxiliary dataset of LoRAs**.

### Step 4: History-Aware Merging

The merged adapter is updated using, so that each task has an equal contribution to the resulting adapter.

## Models and LoRA Configuration

### Base Models

- Llama-3.2-1B-Instruct
- Qwen-2.5-1.5B-Instruct
- Llama-3.2-3B-Instruct

### LoRA Configuration

| Parameter | Value |
|---|---:|
| Rank | 32 |
| \(\alpha\) | 128 |
| Llama-3.2-1B adapter size | 23M parameters / 27 MB |
| Qwen-2.5-1.5B adapter size | 37M parameters / 34 MB |

## Dataset and Tasks

The evaluation contains **40 sequential tasks** across **8 languages** and **5 problem types**.

### Languages

- English
- Spanish
- French
- German
- Italian
- Korean
- Japanese
- Chinese

### Tasks

| Task | Dataset |
|---|---|
| Smart Reply | Persona-Chat Synthetic |
| Summarization | SAMSum |
| Tone Adjustment | Sound Natural |
| Question Answering | SQuAD |
| Grammar Error Correction | Write & Improve, Merlin, ECSpell, GitHub Typo Corpus |

## Experimental Setup

| Setting | Configuration |
|---|---|
| Storage budget | K = 1-8 |
| Optimizer | AdamW |
| Learning rate | \(5 \times 10^{-5}\) |
| Dropout | 0.05 |
| Batch size | 3 |
| Sequential adapters | 40 |
| Task orderings | 3 random orderings |
| GPUs | 10 × NVIDIA A40 |
| CPUs | Intel Xeon Gold 5218 |

## Baselines

K-Merge is compared with several approaches adapted to the online setting.

### Standard Merging

- Linear
- TIES
- DARE
- DARE-TIES

### Continual Merging

- OPCM (Orthogonal Projection Continual Merging)

### Performance Boundaries

- **Zero-shot:** lower performance boundary
- **Single-task LoRA:** upper performance boundary

## Evaluation Metric

Performance is measured using a **Normalized Aggregate Score**.

The metric averages the ratio between merged-adapter performance and single-task-adapter performance across encountered tasks.

Task-specific metrics include:

- \(F_1\)
- \(F_{0.5}\)
- ROUGE

## Key Results

### Performance

For **Llama-3.2-1B**, K-Merge achieved:

| Storage Budget | Aggregate Score |
|---:|---:|
| K = 3 | 0.73 |
| K = 5 | 0.80 |
| K = 7 | 0.82 |

K-Merge and K-Merge++ consistently outperform standard merging baselines across the tested storage budgets.

### Robustness to Task Ordering

Under a **worst-case task ordering**, K-Merge++ achieved:

| Method | K = 3 |
|---|---:|
| **K-Merge++** | **0.73** |
| Linear | 0.68 |
| TIES | 0.58 |

This demonstrates greater resilience to unfavorable task arrival sequences.

### Integration Latency

Adapter integration takes approximately:

> **0.04-0.18 seconds**

when managing **2-8 stored multi-tasking adapters**.

### Similarity Metric

Cosine similarity performed best for clustering:

| Similarity Metric | Score at K = 5 |
|---|---:|
| **Cosine** | **0.78** |
| \(L^1\) | 0.67-0.69 |
| \(L^2\) | 0.67-0.69 |
| \(L^\infty\) | 0.67-0.69 |

### Storage Efficiency

Storing all **40 individual adapters** requires approximately **1 GB**.

K-Merge can achieve approximately **80% of single-task performance** using only **5 adapters (~135 MB)**.

This corresponds to an **86.5% reduction in storage requirements**.

## Key Findings

1. **Task-Centric Clustering**  
   Adapters trained for similar problem types tend to have higher similarity and cluster together more naturally than adapters grouped by language.

2. **History Awareness Matters**  
   Without the history-aware running average, newer adapters can disproportionately influence the merged model and degrade performance on earlier tasks.

3. **Thresholding Preserves the Budget**  
   K-Merge++ prevents highly similar early adapters from unnecessarily consuming storage slots, leaving capacity for future diverse tasks.

4. **Generalization Across Models**  
   K-Merge shows consistent gains across both Llama-3.2 and Qwen-2.5.

5. **Efficient Deployment**  
   The data-free merging process and low integration cost make the approach suitable for real-time adapter management on resource-constrained devices.

## Important Concepts

### Low-Rank Adaptation (LoRA)

LoRA freezes the base LLM weights and introduces trainable low-rank matrices \(B\) and \(A\), reducing the number of parameters required for task-specific fine-tuning.

### Cosine Similarity

K-Merge compares LoRA adapters by flattening their update matrices:

$$
\Delta W = BA
$$

It then uses cosine similarity to measure how similar the resulting adapter updates are.

### History-Aware Running Average

The running average accounts for how many adapters have already been merged into a cluster, preventing recent adapters from having disproportionate influence.

### Similarity Threshold

K-Merge++ uses a threshold \(s\) to determine whether an incoming adapter is sufficiently similar to an existing cluster to be merged.

## Limitations

### Author-Reported Limitations

- **Model and Adapter Scope:** The evaluation is limited to LLMs and LoRA adapters. Multimodal models and other adapter types, such as IA3 and prefix tuning, are not evaluated.
- **Scale Constraints:** Due to compute limitations, experiments focus on models between **1B and 3B parameters** rather than massive-scale LLMs.
- **K-Merge Redundancy:** Basic K-Merge without thresholding can waste storage when early incoming LoRAs are highly similar, potentially reducing performance over long sequences.

### Inferred Limitations

- **Perfect Task Routing:** The methodology assumes that the system can perfectly identify which merged adapter should be used at inference time. An imperfect task classifier could reduce performance.
- **Threshold Calibration:** K-Merge++ requires an auxiliary LoRA dataset to calibrate \(s\). Its effectiveness therefore depends on how representative this dataset is of future tasks.

## Critical Evaluation

### Strengths

- Addresses a practical problem in on-device AI.
- Provides a lightweight and computationally efficient solution.
- Does not require original training data during merging.
- Does not require all adapters to be available simultaneously.
- Demonstrates robustness across multiple languages, task types, models, and task orderings.

### Weaknesses

- Relies on perfect task routing during inference.
- K-Merge++ requires an additional threshold-calibration phase.
- Evaluation is limited to relatively small LLMs and LoRA adapters.

### Experimental Quality

The experiments cover multiple model families, languages, task types, storage budgets, and task orderings. Results are averaged over **three random task orderings**.

### Reproducibility

The paper provides detailed information about:

- Hyperparameters.
- Hardware.
- Algorithmic steps.
- Experimental configuration.

This supports reproducibility based on the information provided.

### Evidence Support

The reported conclusions are supported by aggregate-score improvements over standard merging baselines and by robustness experiments involving different task orderings.

## Key Takeaways

1. **K-Merge enables continual adapter merging under a fixed storage budget.**
2. **Functional/task similarity is more useful for clustering than language alone.**
3. **History-aware averaging helps prevent recency bias during sequential merging.**
4. **K-Merge++ improves robustness by preserving storage capacity for future diverse tasks.**
5. **Integration takes less than 0.2 seconds in the reported experiments.**
6. **Five adapters (~135 MB) can achieve approximately 80% of single-task performance while reducing storage requirements by 86.5%.**

## Further Reading

[1] D. Shenaj, O. Bohdal, T. Ceritli, M. Ozay, P. Zanuttigh, and U. Michieli, *K-Merge: Online Continual Merging of Adapters for On-device Large Language Models*, 2026.