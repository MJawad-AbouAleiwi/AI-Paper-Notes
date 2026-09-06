# Research Summary: Is Cosine-Similarity of Embeddings Really About Similarity?

## TL;DR

This paper shows that cosine similarity between learned embeddings can be arbitrary and non-unique in common linear Matrix Factorization models. The key issue is a rescaling degree of freedom: latent dimensions can be rescaled without changing model predictions, while the same rescaling can substantially change cosine similarities. The authors show that product-based regularization permits this ambiguity, whereas individual-matrix regularization produces unique cosine similarities. Simulated experiments with known item clusters demonstrate that arbitrary scaling can prevent cosine similarity from recovering the underlying similarity structure.

## Paper Information

| Field | Details |
|---|---|
| Title | *Is Cosine-Similarity of Embeddings Really About Similarity?* |
| Authors | Harald Steck, Chaitanya Ekanadham, and Nathan Kallus |
| Year | 2024 |
| Research area | Low-dimensional feature embeddings, semantic similarity quantification, recommender systems, and linear Matrix Factorization models |

## One-Paragraph Summary

The paper investigates the mathematical validity of using cosine similarity on learned low-dimensional embeddings to measure semantic similarity. Through analytical derivations focused on linear Matrix Factorization models, the authors demonstrate that cosine similarity can produce arbitrary, non-unique, and potentially meaningless results. They identify a critical degree of freedom in training objectives that regularize the product of embedding matrices, allowing latent dimensions to be rescaled arbitrarily without changing model predictions while fundamentally changing cosine similarity values. The study compares two regularization schemes: product-based regularization, where similarities are non-unique, and individual-matrix regularization, where the resulting similarities are unique. Experiments using simulated data with known ground-truth clusters show that cosine similarity can fail to recover true underlying similarities depending on these scaling choices. The authors suggest alternatives such as training directly with cosine similarity or incorporating normalization during learning.

## Research Problem

### What problem does the paper address?

The paper examines whether cosine similarity applied to learned low-dimensional feature embeddings reliably represents semantic similarity. It argues that, in certain models, cosine similarity can depend on arbitrary properties of the learned representation rather than the underlying relationships captured by the model.

### Why is this problem important?

Embeddings are widely used in machine learning to quantify similarity between discrete entities such as words, items, or users. Cosine similarity is a dominant metric because it emphasizes directional alignment rather than vector norm. If the resulting similarities can be arbitrary, applications and interpretations based on those similarities may be fundamentally misleading.

### Gap in Previous Research

Previous research has reported inconsistent empirical performance: cosine similarity sometimes works well, while simple dot products can outperform it. The authors identify an overlooked degree of freedom in learned embeddings-the rescaling of latent dimensions-as an explanation for these inconsistencies.

## Main Contribution

- **Analytical derivation of arbitrariness:** The authors provide a formal mathematical argument showing that certain linear models allow embedding columns to be arbitrarily rescaled while keeping model predictions unchanged.
- **Comparison of regularization schemes:** They distinguish between product-based regularization, which permits non-unique similarities, and individual-matrix regularization, which produces unique results.
- **Identification of the rescaling degree of freedom:** The paper shows that training objectives invariant to column rescaling through a diagonal matrix can invalidate the resulting cosine similarities.
- **Proposed remedies:** The authors suggest training models directly with cosine similarity, using techniques such as layer normalization, projecting smoothed data back into the original feature space, or applying normalization before or during learning.
- **Experimental validation:** Simulated data with known item clusters is used to demonstrate how different modeling and scaling choices affect recovery of the underlying similarity structure.

## Methodology

### Model

The study uses linear Matrix Factorization models, which are equivalent to linear autoencoders. The objective is to estimate a low-rank matrix.

### Dataset

The experiments use simulated interaction data with:

- `n = 20,000` users
- `p = 1,000` items
- `C = 5` ground-truth item clusters
- Power-law popularity distributions

### Experimental Setup

Matrices are learned under two different regularization objectives. A low-rank constraint is used to complement the full-rank analytical derivations.

### Baseline

The primary baseline is the "true" item-item similarity defined by the ground-truth clusters in the simulated data.

### Evaluation

The authors evaluate whether the learned similarities recover the known cluster structure. Similarity matrices are visualized as heatmaps after sorting items by their ground-truth cluster assignments.

## Key Results

### Product Regularization: Non-Unique Similarities

Under the first objective, item-item cosine similarities vary substantially depending on the choice of the arbitrary diagonal scaling matrix. Even non-extreme choices of can produce similarity matrices that look completely different from the ground truth.

### Individual Regularization: Unique Similarities

The second objective produces a unique solution for cosine similarity and recovers a stable, though not necessarily perfect, similarity structure.

### Full-Rank Self-Similarity Example

In a full-rank model under the first objective, a particular choice produces the identity matrix as the item-item cosine similarity matrix. This means that each item is similar only to itself, with zero similarity to every other item.

## Key Findings

- Cosine similarity can depend on an arbitrary degree of freedom that does not affect model accuracy but substantially changes similarity values.
- The type of regularization matters: regularizing the product of matrices can produce non-unique similarities, while regularizing individual matrices produces unique similarities.
- Normalizing embeddings after learning can be less effective than incorporating normalization into the learning process.
- Cosine similarity is not invariant to latent dimension rescaling in the settings studied by the paper.
- Standard MF models trained with product regularization can fail to recover ground-truth semantic clusters when arbitrary scaling is applied.

## Why This Paper Matters

The paper challenges a common assumption about embedding-based similarity: that a high-quality predictive representation necessarily produces meaningful cosine similarities.

Its main importance is the distinction between predictive equivalence and similarity equivalence. Two parameterizations can make identical predictions while producing different cosine similarities. As a result, model performance alone does not establish that cosine similarity computed from the learned embeddings has a meaningful interpretation.

This is particularly relevant when embeddings are used not only for prediction but also for interpreting relationships between users, items, words, or other entities.

## Important Concepts

### Cosine Similarity

Cosine similarity measures the directional alignment between two vectors.

It focuses on the angle between vectors rather than their magnitude.

### Matrix Factorization

Matrix Factorization decomposes a data matrix into lower-rank matrices and to represent latent features.

### Regularization

Regularization techniques such as penalties constrain learned parameters to reduce overfitting. The paper shows that the specific form of regularization can determine whether cosine similarities are unique.

### Layer Normalization

Layer normalization normalizes inputs across features and is discussed as one possible way to incorporate normalization directly into learning when optimizing for cosine similarity.

## Limitations

### Explicit Limitations

- The analytical derivations are limited to linear models, specifically Matrix Factorization and linear autoencoders.
- There is no standard, objective metric for semantic similarity in the setting studied, leading the authors to rely on simulated data with known ground truth.
- Bias terms are omitted for clarity, although their impact is noted to be limited.

### Inferred Limitations

- The findings may not directly transfer to complex non-linear deep learning models without further derivation, although the authors expect similar issues.
- The controlled simulation does not capture the full complexity of real-world user-item interactions or linguistic contexts.

## Critical Evaluation

### Strengths

- Provides a rigorous mathematical foundation for the central claims.
- Uses closed-form solutions to demonstrate non-uniqueness in common linear models.
- Clearly separates the effects of different regularization schemes.
- Uses simulated data with known cluster structure, making the effect of arbitrary scaling easy to inspect.
- Provides specific simulation parameters and analytical steps that support reproducibility.

### Weaknesses

- The theoretical analysis is limited to linear models.
- The experimental evaluation relies heavily on qualitative visual analysis of similarity heatmaps rather than quantitative benchmarks.

### Experimental Quality

The simulation design controls for popularity bias and cluster structure, allowing the learned similarity matrices to be compared directly against a known ground truth.

### Reproducibility

The study provides specific simulation parameters and analytical steps that can be independently verified.

### Support for Conclusions

The conclusions are supported by both the mathematical analysis and the simulation results. Together, they demonstrate how arbitrary rescaling can change cosine similarities without changing the underlying model predictions.

## Key Takeaways

1. Cosine similarity between learned embeddings can be mathematically arbitrary and non-unique.
2. Predictive accuracy does not guarantee that embedding cosine similarities are meaningful.
3. The form of regularization is a critical factor in similarity stability.
4. Post-hoc normalization does not necessarily resolve the underlying issue.
5. Researchers should be cautious when interpreting cosine similarity as semantic similarity.
6. Possible alternatives include training directly with cosine similarity objectives, incorporating normalization during learning, or projecting data back into the original feature space for similarity calculations.

## Further Reading

[1] H. Steck, C. Ekanadham, and N. Kallus, "Is Cosine-Similarity of Embeddings Really About Similarity?" 2024.