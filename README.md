# Drug-Affinity-Prediction
**First Model - NN**

Architecture: Two FC branches (one for drugs, one for proteins) → merged → regression head

Input: One-hot SMILES + integer-encoded amino acids

Loss: MSE  Optimizer: Adam  Epochs: 10

Results Summary
I have evaluated the model on four biologically relevant splits — testing whether the model can generalize to new proteins or drugs it hasn't seen during training.

| Split               | CI    | MSE  | R²    | RM²   | Pearson R |
| ------------------- | ----- | ---- | ----- | ----- | --------- |
| **No New Proteins** | 0.669 | 1.02 | –0.12 | –0.03 | 0.31      |
| **New Proteins**    | 0.809 | 0.59 | 0.45  | 0.35  | 0.67      |
| **No New Drugs**    | 0.848 | 0.46 | 0.54  | 0.43  | 0.74      |
| **New Drugs**       | 0.732 | 0.64 | –0.02 | –0.01 | 0.40      |


Interpretation
Ranking ability is strong: High Concordance Index (CI) across all splits shows the model can correctly rank drug-target affinities.

Regression performance varies:
-Best on new proteins and no new drugs
-Poorer when generalizing to new drugs or unseen protein-drug pairs
-Negative R²/RM² indicates poor calibration — the model predicts trends well but struggles with actual values.
