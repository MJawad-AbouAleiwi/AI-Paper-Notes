# Discovering Significant Topics from Legal Decisions with Selective Inference

## TL;DR

This paper proposes an automated pipeline for discovering topics in legal decisions that are statistically associated with case outcomes. The pipeline combines outcome-information masking, text preprocessing, topic modeling, LASSO regression, selective inference, and manual legal evaluation.

The experiments cover Uniform Domain Name Dispute Resolution Policy (UDRP) cases and European Court of Human Rights (ECHR) cases involving Articles 3, 6, and 8.

Key reported results include:

- LSA generally produces the strongest model fit and the largest number of significant topics.
- One UDRP LSA specification reports MDR = 0.431 and AUROC = 0.914.
- Legally fine-tuned BERTopic performs better than the general BERTopic model in the reported comparisons.
- Adding topic features reduces the number of significant panellist effects in UDRP models.
- Several discovered topics correspond to recognizable legal fact patterns.
- The results are correlative and do not establish causation or the absence of institutional bias.

## Paper Information

| Field | Details |
| --- | --- |
| Title | *Discovering Significant Topics from Legal Decisions with Selective Inference* |
| Author | Jerrold Soh Tsin Howe |
| Year | 2024 |
| Research area | Legal Language Processing, topic models, text-as-data, domain-name disputes, and ECHR cases |

## Why This Paper Matters

Much legal information is stored in natural-language documents, making computational analysis difficult and often labor-intensive.

The paper focuses on discovery rather than analysis or identification. Instead of starting with a predefined set of candidate legal predictors, the proposed workflow searches legal decision texts for candidate topics that are statistically associated with outcomes.

The main contribution is the integration of:

- Outcome-information masking
- Topic modeling
- LASSO regression
- Selective inference
- Manual legal interpretation

The authors describe the contribution as incremental and emphasize the combination of existing techniques rather than introducing a completely new topic-modeling algorithm.

## Research Problem

The paper addresses the problem of discovering statistically significant topics or candidate predictors from unstructured legal decisions and relating them to legal outcomes.

The paper distinguishes three tasks:

- Analysis: estimating weights when candidate variables are already available.
- Identification/extraction: extracting known candidate variables from text.
- Discovery: finding candidate variables when they are not known in advance.

## Methodology

### 1. Pre-processing and outcome-information masking

The pipeline first removes sections and phrases that directly reveal case outcomes.

Depending on the model, preprocessing can include:

- Lowercasing
- Stopword removal
- Lemmatisation

This preprocessing is mainly used for LSA, while BERTopic is trained on raw text.

For the ECHR analysis, the authors use:

- Procedure
- Circumstances
- Relevant Law

The analysis focuses on clear violation/non-violation outcomes and excludes cases labeled "both".

The authors note two important limitations of masking:

- It can remove useful information.
- It may not remove all outcome-related information because the presentation of case facts can itself reflect the preferred disposition.

### 2. Topic representations

The paper compares four text representations:

| Representation | Description |
| --- | --- |
| OHE | Indicators for n-grams in the corpus vocabulary |
| LSA | Uses TF-IDF and SVD to produce lower-dimensional topic features |
| BTO_M | General BERTopic model |
| BTO_L | Legally fine-tuned BERTopic model |

BERTopic works by embedding text chunks, reducing embedding dimensionality, clustering the resulting representations, and extracting topic terms using a bag-of-words representation and c-TFIDF.

Exact underlying neural-network architectures and all language-model training details are not specified in the supplied paper evidence.

### 3. LASSO regression

The topic features are entered into LASSO outcome models.

LASSO is used because legal text can create a high-dimensional setting where the number of features approaches or exceeds the number of observations. Legal outcomes can also be imbalanced or rare.

### 4. Selective inference

After LASSO selects features, the authors apply selective-inference procedures to obtain p-values after post-selection de-biasing.

The paper uses:

- `selectiveInference`
- `glmnet`

### 5. Topic evaluation

The authors manually inspect statistically prominent topics, focusing on topics with:

- Large positive or negative coefficients
- Small p-values

A legally trained evaluator examines topic n-gram distributions and cases strongly associated with selected topics to determine whether they correspond to recognizable legal patterns.

## Datasets

### UDRP

The study uses cases resolved under the Uniform Domain Name Dispute Resolution Policy.

- Initial raw cases: 27,634
- Usable observations: 22,653

### ECHR

The study examines European Court of Human Rights cases involving:

- Article 3
- Article 6
- Article 8

The analysis uses clear violation/non-violation outcomes and balances the dataset through random under-sampling.

The total number of ECHR observations is not specified in the supplied paper evidence.

## Experimental Setup

For the UDRP experiments:

- Each topic model extracts exactly 250 topics.
- The authors report approximately 90% compression of the original TF-IDF matrix.
- The LSA-based selection process explains about 61% of the original variance.
- Three UDRP settings are evaluated:
  - A: one-panellist GTLD cases
  - B: all GTLD cases
  - C: all cases

For the ECHR main specification:

- 100 topic features are used.
- The analysis covers Articles 3, 6, and 8.

## Baselines

The experiments include:

- Non-text specifications
- White-noise placebo specifications
- OHE text features
- LSA
- BTO_M
- BTO_L

## Evaluation Metrics

### AUROC

Area under the receiver-operating-characteristic curve.

### MDR

Median deviance ratio, which summarizes deviance ratios along the `glmnet` fitting path and is interpreted in the paper as a pseudo-R² measure.

### Selective p-values

Used to determine which selected topics remain statistically prominent after accounting for the selection process.

### Manual legal interpretability

Representative n-grams and strongly associated cases are inspected for selected topics.

## Key Results

### UDRP Results

The non-text baseline specifications produce approximately 50 significant panellists at α = 0.05, even with controls.

After adding topic features, the panellist association becomes weaker.

For LSA, the numbers of significant panellists across the three settings are:

| Setting | Baseline | LSA |
| --- | ---: | ---: |
| A | 53 | 9 |
| B | 50 | 13 |
| C | 49 | 8 |

LSA consistently produces the largest number of significant topics and the highest reported fit scores.

One LSA specification reports:

| Metric | Result |
| --- | ---: |
| Significant topics | 32 |
| Significant panellists | 8 |
| MDR | 0.431 |
| AUROC | 0.914 |

The reported BERTopic comparison is:

| Model | MDR | AUROC |
| --- | ---: | ---: |
| BTO_M | 0.275 | 0.838 |
| BTO_L | 0.295 | 0.849 |

The authors interpret the UDRP results as consistent with the view that high complaint success rates are better explained by case facts than by structural pro-complainant bias. However, they emphasize that the models are correlative and cannot establish the absence of bias.

### ECHR Results

LSA again produces higher reported model fits and more significant topics than the BERTopic models.

For Article 3:

| Model | Significant topics |
| --- | ---: |
| LSA | 15 |
| BTO_M | 3 |
| BTO_L | 4 |

For Article 8, BTO_L has a higher reported AUROC than BTO_M:

| Model | AUROC |
| --- | ---: |
| BTO_M | 0.638 |
| BTO_L | 0.711 |

The paper identifies examples involving:

- Chechen disappearances
- Rejected asylum seekers
- Delayed enforcement of compensation judgments in Ukraine
- Censorship of correspondence in Polish detention settings

Other ECHR quantitative results are not specified in the supplied paper evidence.

## Key Takeaways

1. LSA is the strongest-performing representation in the reported experiments, producing the highest fit measures and largest number of significant topics across the UDRP and ECHR settings.
2. Adding topic features reduces the number and magnitude of significant panellist effects in the reported UDRP comparisons.
3. The discovered relationships are correlative and do not establish causal explanations for legal outcomes.
4. The legally fine-tuned BTO_L model performs slightly better than BTO_M in the reported UDRP comparison and has a higher Article 8 AUROC in the ECHR analysis.
5. Several selected topics are legally interpretable, including administrative deficiencies, famous trademarks, reverse-domain-name hijacking, disappearances, asylum claims, delayed enforcement, and correspondence censorship.
6. The automated discovery process can produce false positives, false negatives, mixed predictors within topics, and topics that are difficult to interpret.

## Strengths

- Integrates masking, topic modeling, LASSO, selective inference, and manual legal interpretation.
- Tests the approach on both UDRP and ECHR decisions.
- Compares multiple text representations.
- Includes non-text and white-noise placebo baselines.
- Combines statistical selection with manual examination of legal examples.
- Reports quantitative results alongside topic-level interpretations.

## Limitations

### Explicitly reported by the authors

- Masking can remove useful information.
- Masking may not eliminate all outcome leakage.
- The discovered associations are not causal.
- Choosing the number of topics involves some arbitrariness.
- BTO hyperparameters were not exhaustively optimized.
- Manual evaluation examines only the top five associated cases for selected topics.
- A single topic can contain multiple predictors.
- Individually insignificant topics could potentially be jointly significant.
- Automated discovery can produce false positives and false negatives.
- The system may assign high topic weights to cases that are not truly on-topic.
- The system may fail to identify significant topics or synthesize related topics.

### Methodological limitations inferred from the paper

- The reported advantage of LSA over BTO may partly reflect unequal optimization because BTO was not exhaustively hyperparameter-optimized.
- The experiments are limited to UDRP decisions and selected ECHR Articles 3, 6, and 8. The paper does not establish performance in other legal domains, jurisdictions, languages, or types of outcomes.
- The workflow depends on available outcome labels and structured covariates.
- Reviewing only five strongly associated cases for selected topics provides a limited assessment of topic interpretability.

## Critical Evaluation

### Experimental quality

The experiments are methodologically substantive within the scope of the paper. They use:

- Two legal corpora
- Multiple text representations
- Multiple UDRP settings
- Non-text baselines
- Placebo specifications
- Selective p-values
- Fit metrics
- Manual legal review

However, the BTO models were not fully optimized according to the authors, and the supplied evidence does not specify the ECHR sample size or all detailed preprocessing and training settings.

Therefore, the experiments support the reported within-paper comparisons but do not establish universal superiority of LSA.

### Reproducibility

Reproducibility is supported by:

- Reported mathematical formulations
- Named software packages such as `selectiveInference` and `glmnet`
- Stated topic counts
- Described datasets
- Outlined preprocessing steps

The supplied paper evidence does not specify:

- The exact code repository for the pipeline
- Complete language-model architecture details
- All hyperparameters
- All training and inference settings

### Are the conclusions supported?

Within the paper's stated scope, the reported evidence supports the narrower conclusion that the pipeline can discover outcome-associated and sometimes legally interpretable topics in the evaluated corpora.

The reported evidence does not establish:

- Causal relationships
- The absence of institutional bias
- Complete recovery of all legal predictors
- Generalization to untested legal domains

## Important Concepts

### Predictors and adjudication

The paper uses "predictor" broadly to mean a variable that predicts a case outcome.

It distinguishes between:

- Legal predictors
- Non-legal predictors
- The adjudication function that maps cases to outcomes

### Discovery

Discovery refers to finding candidate variables when they are not known in advance.

This differs from:

- Analysis, where candidate variables are already available.
- Identification/extraction, where known variables are extracted from text.

### Outcome Leakage

Outcome leakage occurs when text contains information that directly reveals the outcome being modeled.

The paper attempts to reduce leakage through masking but notes that masking can both remove useful information and fail to eliminate all outcome-related signals.

### LSA

Latent Semantic Analysis represents documents using TF-IDF and SVD to compress high-dimensional lexical information into a smaller set of latent topic features.

### BERTopic

BERTopic embeds text chunks, reduces embedding dimensionality, clusters the resulting representations, and describes clusters using c-TFIDF.

### LASSO

LASSO applies a penalty that encourages sparse coefficient selection. This is useful for high-dimensional text data where the number of features can approach or exceed the number of observations.

### Selective Inference

Selective inference accounts for the fact that variables were selected before statistical testing. The paper applies post-selection procedures to compute p-values after LASSO selection.

### AUROC and MDR

- AUROC measures classification discrimination across thresholds.
- MDR summarizes deviance ratios along the `glmnet` fitting path and is interpreted by the paper as a pseudo-R² measure.

## Overall Assessment

The paper presents an integrated approach for discovering statistically significant and potentially interpretable topics in legal decisions.

Its strongest reported result is the performance of LSA across the evaluated experiments, including a UDRP specification with MDR = 0.431 and AUROC = 0.914.

The findings are useful as evidence for correlation-based discovery within the studied datasets. They should not be interpreted as causal explanations of legal outcomes or as proof that institutional bias is absent.

The main qualifications concern outcome masking, topic-count selection, incomplete BTO optimization, limited manual evaluation, and errors in automated topic discovery.

## Further Reading

[1] Howe, Jerrold Soh Tsin. *Discovering Significant Topics from Legal Decisions with Selective Inference*. Version dated January 3, 2024.