# Drug-Affinity-Prediction
-----**First Model - NN**------

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
-Poorer when generalising to new drugs or unseen protein-drug pairs
-Negative R²/RM² indicates poor calibration — the model predicts trends well but struggles with actual values.

----**Second Model - CNN**-------


Using a convolutional neural network (CNN) improved performance in generalising to new proteins and drugs (especially for ranking via CI).
However, prediction accuracy for new drugs remains low (R² < 0), indicating difficulty in learning transferable representations from one-hot encoded SMILES strings.

| Split               | CI    | MSE   | R²    | RM²   | Pearson R |
| ------------------- | ----- | ----- | ----- | ----- | --------- |
| **No New Proteins** | 0.691 | 0.659 | 0.10  | 0.04  | 0.35      |
| **New Proteins**    | 0.802 | 0.525 | 0.39  | 0.27  | 0.63      |
| **No New Drugs**    | 0.812 | 0.497 | 0.38  | 0.25  | 0.63      |
| **New Drugs**       | 0.470 | 0.629 | –0.25 | –0.03 | \~0       |


**What Improved:**

More stable training: Loss reduces steadily across all splits.

Higher CI across most splits, showing improved ranking of affinities.

Slight MSE drop and better R² compared to the FCN model for “New Proteins” and “No New Drugs”.

**What’s Still Challenging:**

New Drugs in Test Split is problematic:
CI dropped to 0.47
R² became negative, and the Pearson correlation is ~0

Suggests the model fails to generalise to unseen SMILES strings

The CNN model handles proteins well, likely because embeddings + convolutions extract useful features from amino acid sequences.
One-hot SMILES may not be expressive enough — many drugs share substructure patterns not captured by this representation.
