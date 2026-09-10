# Fed-CBS: A Heterogeneity-Aware Client Sampling Mechanism for Federated Learning via Class-Imbalance Reduction

## TL;DR

Fed-CBS addresses performance degradation in Federated Learning under non-IID data by selecting clients whose combined data is more class-balanced. The method introduces the Quadratic Class-Imbalance Degree (QCID) to measure class imbalance and uses a sequential sampling strategy to reduce it. The paper shows that the optimal class-balanced client-selection problem is NP-hard, while the proposed strategy has a computational complexity of `O(N×M²)`. Across MNIST, CIFAR-10, Fashion-MNIST, and FEMNIST, Fed-CBS improves accuracy and convergence compared with random selection and state-of-the-art baselines, with reported gains of up to 7% in test accuracy and up to 2.8× faster convergence.

## Paper Information

| Field | Details |
|---|---|
| Title | Fed-CBS: A Heterogeneity-Aware Client Sampling Mechanism for Federated Learning via Class-Imbalance Reduction |
| Authors | Jianyi Zhang, Ang Li, Minxue Tang, Jingwei Sun, Xiang Chen, Fan Zhang, Changyou Chen, Yiran Chen, and Hai Li |
| Year | 2023 |
| Research Area | Federated Learning, Client Selection, Non-IID Data |

## Why This Paper Matters

Federated learning commonly operates with heterogeneous, non-IID data distributed across many clients. Because only a subset of clients can participate in each communication round, random selection can produce a grouped dataset with severe class imbalance. This can hurt model generalization and slow convergence.

Fed-CBS focuses on client selection as a way to reduce this imbalance. Instead of treating clients as interchangeable, it uses information about their label distributions to select a subset whose combined data is closer to a balanced class distribution.

The approach is designed to avoid requiring the server to access raw client data or exact local label distributions. This makes the proposed selection mechanism relevant to privacy-sensitive federated learning settings.

## Research Problem

### Problem

The paper studies the performance degradation and slower convergence of federated learning under non-IID data when standard random client selection is used.

In practical cross-device FL systems, only a subset of devices can participate in each round because of communication and resource constraints. Random selection can therefore produce a grouped dataset that is highly imbalanced across classes.

### Why It Is Important

If selected clients do not adequately represent the global data distribution, the global model may become biased or fail to converge toward an optimal state. This is particularly important for large-scale FL systems where data is naturally heterogeneous and participation is limited.

### Gap in Previous Research

According to the analysis, existing client-selection methods often lack a formal criterion for measuring selection quality and do not clearly identify class imbalance as the fundamental source of degradation.

Previous approaches, including Fed-cucb by Yang et al. (2020), have several limitations:

- They may require a class-balanced auxiliary dataset at the server.
- They can rely on inaccurate estimates of local label distributions.
- These estimation approaches become less suitable as the number of classes increases.
- Some methods use deterministic greedy selection without performance guarantees.

Fed-CBS aims to provide a theoretically grounded, privacy-preserving, and computationally efficient alternative.

## Main Contributions

1. Class-imbalance insight
   - The authors identify and experimentally verify class imbalance in the aggregated data of selected clients as a primary cause of performance degradation under non-IID settings.

2. QCID metric
   - The paper introduces the Quadratic Class-Imbalance Degree (QCID) to quantify the class imbalance of a selected client subset.
   - The analysis shows that QCID can be computed using inner products of scaled local label distributions, avoiding the need for raw client data or exact local distributions at the server.

3. Fed-CBS sampling mechanism
   - The proposed method sequentially selects clients to reduce class imbalance.
   - The sampling process balances exploitation, which favors clients that improve class balance, with exploration, which favors clients that have been selected less frequently.

4. Theoretical analysis
   - The paper proves that finding the optimal class-balanced client subset is NP-hard.
   - It provides convergence analysis for Fed-CBS.
   - The proposed sequential strategy has computational complexity `O(N×M²)`.

5. Empirical evaluation
   - Fed-CBS is evaluated across multiple datasets and non-IID settings.
   - The reported results show improvements over random selection and baselines such as Fed-cucb.

## Methodology

### Core Idea

Fed-CBS aims to minimize the class imbalance of the clients selected for each communication round.

The server maintains a selection count for each client. It then selects the required number of clients sequentially, using the class-balance quality of the partially selected subset to influence the probability of selecting the next client.

### Quadratic Class-Imbalance Degree

QCID measures the distance between the label distribution of a selected client group and a perfectly balanced uniform distribution.

A lower QCID indicates that the selected clients have a more balanced combined class distribution.

A key theoretical result is that QCID can be computed from inner products of scaled label-distribution vectors. This allows the server to evaluate class balance without directly accessing raw client data.

### Sequential Client Sampling

The selection strategy operates sequentially:

- Maintain the current selected subset.
- Evaluate how adding a candidate client affects class imbalance.
- Balance exploitation of candidates that improve QCID with exploration of clients that have been selected less frequently.
- Continue until the desired number of clients has been selected.

Because the exact optimization problem is NP-hard, this sequential approach serves as a computationally efficient heuristic.

## Experimental Setup

### Datasets

| Dataset | Client Setup / Non-IID Setting |
|---|---|
| MNIST | 100-200 clients with varying non-IID setups |
| CIFAR-10 | 120-200 clients with class-imbalance ratios of 3:1 and 5:1 |
| Fashion-MNIST | 200 clients with Dirichlet `α ∈ {0.1, 0.2, 0.5}` |
| FEMNIST | 3,500 clients, simulating large-scale settings |

### Model Architectures

- MNIST and Fashion-MNIST: MLP with one hidden layer of 64 units.
- CIFAR-10: compact CNN with two convolutional layers using 6 and 16 kernels of `5×5`, followed by hidden layers of 120 and 84 units and three fully connected layers.
- FEMNIST: architecture not specified in the provided analysis.

### Baselines

- `all`: ideal case where all available clients are selected.
- `rand`: random selection of `M` clients.
- `pow-d`: Power-of-Choice client selection.
- Fed-cucb: state-of-the-art method by Yang et al. (2020).
- Fed-cucb+ (Astraea): Fed-cucb with access to exact label distributions.

### Evaluation Metrics

- Test accuracy: final test-set performance.
- Convergence rate: number of communication rounds needed to reach a target accuracy.
- Averaged QCID: measure of how effectively the selection process reduces class imbalance.

## Key Results

### CIFAR-10 Accuracy

| Global Class-Imbalance Ratio | Fed-CBS | Fed-cucb |
|---|---:|---:|
| 3:1 | `56.86±0.34%` | `55.11±0.26%` |
| 5:1 | `57.99±0.46%` | `53.38±1.48%` |

Fed-CBS improves test accuracy over Fed-cucb in both reported class-imbalanced settings.

### Convergence

| Dataset / Setting | Target Accuracy | Fed-CBS | Fed-cucb |
|---|---:|---:|---:|
| CIFAR-10, `α=0.5` | 50% | `384±74` rounds | `1080±309` rounds |
| Fashion-MNIST, `α=0.5` | 82% | `218±36` rounds | `284±51` rounds |
| FEMNIST | 75% | `980±17` rounds | `1124±31` rounds |

On CIFAR-10 with `α=0.5`, Fed-CBS reaches the target accuracy in 384 rounds compared with 1080 rounds for Fed-cucb, corresponding to a reported 2.8× speedup.

### Class-Imbalance Reduction

For CIFAR-10 with `α=0.5`:

| Method | Averaged QCID |
|---|---:|
| Fed-CBS | `0.36±0.04` |
| Fed-cucb | `6.47±0.77` |

The lower QCID for Fed-CBS indicates a substantially more balanced selected client group.

## Key Takeaways

- Class imbalance in the aggregated data of selected clients is identified as a major bottleneck for FL performance under non-IID conditions.
- QCID provides a metric for evaluating this imbalance without requiring raw client data.
- Fed-CBS uses sequential client sampling to reduce class imbalance while balancing exploitation and exploration.
- The optimal class-balanced client-selection problem is NP-hard.
- The proposed sequential strategy has computational complexity `O(N×M²)`.
- Fed-CBS consistently performs better than random selection and the reported state-of-the-art baselines across the evaluated image-classification benchmarks.
- The reported experiments show higher accuracy, lower QCID, and faster convergence.

## Important Concepts

### Non-IID Data

Non-IID data refers to a setting where local client datasets do not follow the same distribution. Clients may have different class proportions or feature distributions.

### Quadratic Class-Imbalance Degree (QCID)

QCID measures the distance between the label distribution of a selected group of clients and a uniform class distribution. Lower QCID means greater class balance.

### Sequential Client Sampling

A probabilistic selection process in which clients are selected one at a time. The probability of selecting the next client depends on the class balance achieved by the clients already selected.

### NP-Hardness

The paper establishes that finding the most class-balanced client subset is NP-hard. This motivates the use of a heuristic sequential sampling strategy rather than exhaustive optimization.

### FedAvg

FedAvg is the standard federated averaging approach in which clients perform local model updates and the server aggregates their model parameters. Fed-CBS functions as a client-selection mechanism that can be used with such optimizers.

## Critical Evaluation

### Strengths

- Provides a theoretical explanation for why random client selection can perform poorly under non-IID data.
- Introduces QCID as a formal metric for measuring class imbalance.
- Uses inner-product information rather than raw client data for the proposed imbalance calculation.
- Provides a computationally efficient sequential selection strategy.
- Evaluates the approach across multiple datasets and non-IID configurations.
- Reports standard deviations and target-accuracy convergence rounds, supporting more robust comparisons.

### Weaknesses

- The secure infrastructure needed to compute the required inner products is not designed or implemented in the paper.
- The relationship between training performance and client-composition diversity is not fully explored.
- The evaluation is limited to image-classification tasks.
- The sequential selection method is a heuristic and may not reach the theoretical global optimum.
- The provided analysis suggests that performance may depend on hyperparameters such as `βm` and the exploration factor `λ`.

### Experimental Quality

The experiments cover MNIST, CIFAR-10, Fashion-MNIST, and FEMNIST under multiple non-IID conditions. The use of standard deviations and target accuracy rounds provides useful evidence for comparing client-selection strategies.

### Reproducibility

The methodology includes training configurations, hyperparameters, and model architectures for the specified datasets. However, full end-to-end reproduction may be challenging without the source code and the specific secure computation protocol used for inner products.

### Support for the Conclusions

The reported results support the paper's central claim that reducing class imbalance can improve federated learning performance. The lower QCID values for Fed-CBS are accompanied by higher accuracy and faster convergence in the reported experiments.

## Limitations

### Explicitly Mentioned by the Authors

- Infrastructure: the paper does not design the secure computing infrastructure required for privacy-preserving inner-product computation.
- Diversity analysis: the relationship between training performance and the specific diversity of client composition is not fully explored.
- Optimal thresholding: determining the optimal threshold `ϵ` for the number of selected clients under strict communication constraints remains an open challenge.

### Inferred Limitations

- Privacy-preserving computation may introduce communication or computational overhead that is not detailed in the analysis.
- Because the client-selection problem is NP-hard, the sequential strategy is a heuristic and may differ from the theoretical optimum.
- The evaluation covers image classification only, so effectiveness on other data types and tasks is not established by the provided analysis.
- Hyperparameters such as `βm` and `λ` may require tuning across different datasets and heterogeneity levels.

## Further Reading

1. Zhang, J. et al. "Fed-CBS: A Heterogeneity-Aware Client Sampling Mechanism for Federated Learning via Class-Imbalance Reduction." Proceedings of the 40th International Conference on Machine Learning (ICML), 2023.