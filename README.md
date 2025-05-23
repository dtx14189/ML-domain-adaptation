# Domain Adaptation for Cell Type Classification in scRNA-seq Data

This project explores domain adaptation for cell type classification in single-cell RNA sequencing (scRNA-seq) data. Each cell is represented by a high-dimensional feature vector, where the features correspond to gene expression levels across thousands of genes. The target variable is the cell type label (e.g., T cell, B cell, macrophage).

The central challenge lies in training a model on labeled data from healthy individuals (source domain) and applying it to unlabeled data from diseased individuals (target domain), where the expression profiles may differ significantly due to disease-induced changes. Because labeled data from the diseased domain is unavailable during training, the model must learn domain-invariant representations — features that are informative of cell type but not specific to the domain — to generalize effectively to the target distribution.

The project evaluates several strategies for achieving this, with a focus on entropy regularization, which encourages the model to make confident predictions on the target domain despite not having labels.

## Dataset

- Source: [biopsy_RNA_h5ad](https://figshare.com/articles/dataset/biopsy_RNA_h5ad/21919425?file=38883240)
- Content:
  - ~29,000 healthy cells
  - ~49,000 diseased cells
  - ~28,000 gene-expression features per cell
  - 12 fine-annotated cell types (after filtering)

## Models
1. Multi-class Perceptron – Baseline linear classifier
2. Logistic Regression with L2 Regularization
3. Kernel PCA + Logistic Regression – Incorporates non-linear feature transformation
4. Vanilla Neural Network 
5. Domain-Adapted Neural Network – Adds entropy minimization to improve generalization to diseased cells

## Methodology
- Dimensionality Reduction: PCA to 100 components was used for most models.
- Model Evaluation:
  - Training/Validation on healthy cells
  - Testing on diseased cells
- Metrics:
  - Accuracy
  - Macro F1 Score
  - Negative Log-Likelihood (NLL)

## Implementation Details
- Scikit-learn for multi-class perceptron, logistic regression w/ L2 regularzation, and Kernel PCA logistic regression

### Neural Networks (PyTorch)
- Architecture:
  - 3 hidden layers
  - 1024 or 4096 units per layer (depending on model)
  - Leaky ReLU activations
  - Batch normalization and dropout
- Optimization:
  - Optimizer: SGD with learning rate scheduling
  - Epochs: 60
  - Batch Size: 64

## Results

| Model                            | Test Accuracy | F1 Score | NLL   |
|----------------------------------|---------------|----------|--------|
| Multi-Class Perceptron           | 83.29%        | –        | –      |
| Logistic Regression              | 85.93%        | 0.844    | 0.378  |
| Kernel PCA + Logistic Regression | 79.70%        | 0.761    | 0.683  |
| Neural Network                   | 86.18%        | 0.846    | 0.363  |
| Domain-Adapted Neural Network    | 86.87%        | 0.855    | 0.351  |

## Key Takeaways

- Logistic regression on PCA features performs well because PCA removes noise and redundancy, making the data easier to separate linearly.
- Kernel PCA introduces nonlinearity but can overfit on small or noisy data, often without improving performance.
- Entropy regularization helps domain adaptation by encouraging confident predictions on unlabeled target data, improving generalization.
- Complex models need proper regularization and domain-aware loss functions to avoid overfitting and to handle shifts between training and test domains.

## More Details
For more details about our models or our results, please refer to the report in this repository.
