# TriggerESM
Can a general-purpose protein language model predict how mutations in RNA Polymerase II's trigger loop affect fitness?

## Overview
The trigger loop of yeast RNA Polymerase II (Rpb1, residues 1076-1106) is a key part of the enzyme's catalytic cycle. Dutagaci et al. (2023) built a purpose-trained model to predict mutation effects there. This project tests whether embeddings from the general-purpose protein language model ESM-2, paired with a simple linear model, can predict the same outcome.

## Data
- 83 trigger-loop mutants with a continuous fitness-based phenotype
- Source: supplementary data (S29, "Training data" sheet) of Dutagaci et al., *PLOS Computational Biology* (2023). The data file is [included in this repo / not included; download it from the paper].
- Wild-type sequence: UniProt P04050 (RPB1_YEAST)

## Method
1. Extracted the trigger-loop sequence (residues 1076-1106) and built a mutant sequence for each entry.
2. Ran each mutant sequence through ESM-2 using Hugging Face Transformers with a model sizes 0f 8M (`esm2_t6_8M_UR50D`).
3. Used the embedding at the mutated position as the feature vector.
4. Trained Ridge regression (scikit-learn, default alpha) and evaluated it with 5-fold cross-validation.

## Results
| Model | Features | CV R² |
|---|---|---|
| ESM-2 8M | Mutation-site embedding | [0.51] |

## Limitations
- Only 83 samples, so each validation fold holds about 13 mutants. Scores varied across folds ([range]), so results are preliminary.
- Cross-validated R² is not directly comparable to the correlation (R) reported in the original paper.
- Ridge alpha was not tuned, and no held-out test set was scored.

## Next steps
- Tune regularization and add a simple baseline (e.g., one-hot encoding of the substituted amino acid)
- Compare against whole-sequence (mean-pooled) embeddings
- Try larger ESM-2 models and other protein language models

## Reference
Dutagaci B., et al. (2023). *PLOS Computational Biology*. https://doi.org/10.1371/journal.pcbi.1010999


