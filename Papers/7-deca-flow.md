# DeCaFlow: A Deconfounding Causal Generative Model

## TL;DR

DeCaFlow is a causal generative model designed to estimate interventional and counterfactual effects from observational data when hidden confounders are present. It combines Causal Normalizing Flows with variational inference and proximal causal inference, using proxy variables to account for unmeasured confounding. The model is trained once per dataset and can answer a broad class of identifiable causal queries without query-specific retraining. Experiments on Sachs, Ecoli70, and the Law School dataset show that DeCaFlow outperforms several baselines and can approach the performance of an oracle with access to hidden confounders.

## Paper Information

| Field | Details |
| --- | --- |
| Title | DeCaFlow: A deconfounding causal generative model |
| Authors | Alejandro Almodóvar, Adrián Javaloy, Juan Parras, Santiago Zazo, Isabel Valera |
| Year | 2025 |
| Research area | Causal Inference, Generative Modeling, Machine Learning |

## Summary

DeCaFlow is a deconfounding causal generative model designed for causal inference on continuous variables in the presence of unmeasured hidden confounders.

The method extends Causal Normalizing Flows (CNFs) with a variational autoencoder-like architecture consisting of:

- A generative network based on a conditional normalizing flow.
- A deconfounding network based on a conditional normalizing flow.
- Proxy variables that provide information about hidden confounders.

DeCaFlow is trained using observational data and the underlying causal graph. It uses ELBO-based training with KL balancing to prevent posterior collapse.

The resulting model can estimate both interventional and counterfactual queries that are identifiable through do-calculus and proximal causal inference.

Experiments on the Sachs, Ecoli70, and Law School datasets show that DeCaFlow performs better than several existing causal generative and deconfounding methods. On the evaluated tasks, its performance can match that of an oracle model with access to the hidden confounders.

## Research Problem

### Problem

The paper addresses causal inference when hidden, unmeasured confounders affect observed variables.

In particular, it focuses on estimating:

- Interventional effects.
- Counterfactual effects.

The challenge is that observational data can contain confounding that cannot be directly adjusted for because the relevant variables are not observed.

### Why the Problem Matters

Causal relationships are important for evaluating interventions in areas such as:

- Healthcare
- Marketing
- Education

Randomized controlled trials are often infeasible because of ethical, financial, or practical constraints. As a result, causal analysis frequently relies on observational data, where hidden confounders can introduce bias.

### Gap in Previous Research

Existing Causal Generative Models, including standard Causal Normalizing Flows, generally assume causal sufficiency and therefore do not account for hidden confounders.

Existing deconfounding approaches also have limitations:

- Some require training a new model for each causal query.
- Some are restricted to particular causal effects, such as Average Treatment Effects.
- Some only work with specific causal graph structures.

## Main Contributions

### DeCaFlow Model

DeCaFlow integrates the structural constraints of Causal Normalizing Flows with a variational inference framework to account for hidden confounding.

### Unified Causal Inference

A single trained model can estimate a broad class of identifiable interventional and counterfactual queries.

### Theoretical Extension

The authors extend proximal causal inference results to counterfactual queries and settings involving common observed ancestors. The analysis shows that DeCaFlow provides correct estimates when the corresponding queries are identifiable.

### Scalability and Flexibility

The method is demonstrated on complex causal graphs, including the Ecoli70 dataset with 43 variables and 3 hidden confounders, without requiring query-specific retraining.

## Methodology

### Model

DeCaFlow, or Deconfounding Causal Normalizing Flow, is a variational causal generative model.

### Architecture

The model consists of two main components.

#### Generative Network

The decoder is a masked conditional autoregressive normalizing flow. It maps exogenous noise and hidden confounders to observed variables while respecting the causal graph.

#### Deconfounding Network

The encoder is a conditional normalizing flow that approximates the posterior distribution of hidden confounders given the observations.

### Training

DeCaFlow is trained using the Evidence Lower Bound (ELBO), which balances reconstruction quality and a KL-divergence term.

KL balancing is used to prevent posterior collapse, where the latent variables become uninformative.

### Datasets

| Dataset | Description |
| --- | --- |
| Sachs | Protein-signaling network dataset with semi-synthetic additive and non-additive noise |
| Ecoli70 | Gene network dataset with 43 variables |
| Law School | Real-world dataset containing 21,790 students, used for counterfactual fairness evaluation |
| Synthetic ablation graphs | Used to evaluate sensitivity to proxies and latent dimensionality |

### Experimental Setup

The model is trained using observational data and the causal graph.

Hyperparameters are selected using Maximum Mean Discrepancy (MMD) on validation sets to ensure that the model matches the observational distribution.

### Baselines

The experiments compare DeCaFlow against:

- Causal Normalizing Flows (CNF)
- Additive Noise Models (ANM)
- Deep Causal Models (DCM)
- Deconfounder
- Oracle: a CNF with access to hidden confounders

### Evaluation Metrics

The analysis uses:

- Mean Absolute Error (MAE) for Average Treatment Effect and counterfactual samples.
- Root Mean Square Error (RMSE) for fairness tasks.
- Maximum Mean Discrepancy (MMD) for fairness tasks.

## Key Results

### ATE and Counterfactual Estimation

On the Sachs and Ecoli70 datasets, DeCaFlow consistently outperformed CNFs, ANMs, and DCMs.

Its performance matched the oracle model in both additive and non-additive settings.

### Proxy Sensitivity

Increasing the number of proxy variables systematically improved estimation of confounded causal queries.

The analysis attributes this to the increased likelihood of satisfying the completeness condition required for identifiability.

### Latent Dimensionality

DeCaFlow was robust when the dimension of the hidden-confounder representation was overspecified.

Underestimating the latent dimension, however, resulted in increased error.

### Counterfactual Fairness

On the Law School dataset, a DeCaFlow-based fair predictor reduced the MMD between group distributions from 0.163 to 0.0054.

It also achieved lower RMSE than the previous fair baselines FairK and FairAdd.

## Key Findings

1. DeCaFlow integrates proximal causal inference with generative flows to perform deconfounding using informative proxies.
2. The model inherits the identifiability properties of Causal Normalizing Flows for variables that are not directly affected by hidden confounders.
3. A single training process can support all identifiable queries in a graph, avoiding query-specific retraining.
4. ELBO-based training with KL balancing allows the model to handle a latent space larger than the actual number of hidden confounders without degrading performance.
5. Using normalizing flows for both the encoder and decoder allows the model to act as a universal density approximator for the observational distribution and latent posterior.

## Why This Paper Matters

DeCaFlow addresses an important limitation of causal generative models: the assumption that all relevant confounders are observed.

Its main practical advantage is the combination of:

- Hidden-confounder modeling.
- Proxy-based deconfounding.
- Interventional inference.
- Counterfactual inference.
- A single trained model for multiple identifiable queries.

The experimental results also suggest that the approach can scale beyond simple causal graphs and can be applied to downstream problems such as counterfactual fairness.

## Important Concepts

### Causal Normalizing Flows

Causal Normalizing Flows are generative models that use invertible transformations constrained by a causal graph to model structural causal equations.

### Hidden Confounders

Hidden confounders are variables that influence both treatment and outcome but are not observed in the dataset.

### Proxy Variables

Proxy variables are observed variables that carry information about hidden confounders and can be used to account for their effects.

### ELBO

The Evidence Lower Bound is an objective used in variational inference to approximate the log-likelihood of observed data while regularizing the latent representation.

### KL Balancing

KL balancing is a training technique designed to prevent the KL-divergence component of the objective from becoming ineffective and the latent variables from becoming uninformative.

### Do-Calculus

Do-calculus is a framework for transforming interventional queries into expressions involving observational data.

## Limitations

### Explicit Limitations

The paper's stated limitations include:

- The model assumes that all variables are continuous.
- The structural causal model must have C1-diffeomorphic causal equations conditioned on the hidden confounders.
- The underlying causal graph must be known and acyclic.
- Identifying confounded effects depends on the existence of informative proxy variables.

### Inferred Limitations

The analysis also identifies several potential limitations:

- Training deep normalizing flows for very large graphs, such as graphs with hundreds or thousands of variables, may be computationally expensive.
- Errors in the supplied causal graph, such as missing edges or incorrect directions, would likely result in biased causal estimates.
- The method's ability to match the observational distribution is sensitive to the size and quality of the training dataset.

## Critical Evaluation

### Strengths

- Strong theoretical foundation combining Causal Normalizing Flows and proximal causal inference.
- Flexible support for complex causal graphs and different types of causal queries.
- Superior performance over the evaluated baselines on synthetic and real-world-inspired tasks.
- Robustness to overspecified latent dimensionality.
- Ability to support multiple identifiable queries after a single training process.

### Weaknesses

- Requires a known causal graph, which can be difficult to obtain in practical applications.
- Restricts the model to continuous variables, limiting its applicability to categorical and ordinal data.

### Experimental Quality

The experiments are described as thorough and include standard causal benchmarks such as Sachs and Ecoli70.

The evaluation covers multiple aspects of the method, including:

- Proxy sensitivity.
- Latent-dimensionality robustness.
- Causal effect estimation.
- Counterfactual fairness.

### Reproducibility

The methodology uses established components such as masked autoregressive flows and standard ELBO training. The analysis therefore considers the approach to have high reproducibility.

### Support for the Conclusions

The reported experimental results strongly support the paper's claims. DeCaFlow consistently matches the oracle's performance in the evaluated settings while outperforming other non-deconfounding and per-query approaches.

## Key Takeaways

- DeCaFlow provides a unified framework for causal inference under hidden confounding.
- It combines generative flows with proximal causal inference and proxy variables.
- The model can estimate both interventional and counterfactual effects.
- More informative proxy variables can improve deconfounding.
- Overspecifying the latent dimensionality is relatively robust, while underestimating it increases error.
- A single trained model can answer multiple identifiable causal queries.
- DeCaFlow can also support downstream applications such as counterfactual fairness.
- The approach depends on continuous variables, a known acyclic causal graph, suitable structural equations, and informative proxies.

## Further Reading / Related Work

The provided analysis cites the following paper:

[1] A. Almodóvar, A. Javaloy, J. Parras, S. Zazo, and I. Valera, "DeCaFlow: A deconfounding causal generative model," in *Proc. 39th Conference on Neural Information Processing Systems (NeurIPS 2025)*, 2025.