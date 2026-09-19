# Improving AdaBoost-Based Intrusion Detection System Performance on CIC-IDS 2018 Dataset

A Machine Learning-based Intrusion Detection System (IDS) developed using the **CIC-IDS 2018 dataset** to identify malicious network traffic and investigate different preprocessing, feature engineering, balancing, and classification techniques.

The project explores **PCA, SMOTE, Random Forest, Elastic Net, RNN/LSTM, XGBoost, Decision Tree, and AdaBoost** as part of an experimental intrusion-detection pipeline.

---

## Project Overview

Intrusion Detection Systems (IDS) play an important role in identifying malicious activities and potential cyberattacks within network traffic.

Traditional intrusion detection approaches can face challenges when dealing with:

- Large volumes of network traffic
- High-dimensional feature spaces
- Imbalanced attack classes
- Multiple types of cyberattacks
- False positive and false negative predictions

This project investigates a machine-learning-based IDS using the **CIC-IDS 2018 dataset** and experiments with several preprocessing and classification techniques.

The overall pipeline includes:

**Data Preprocessing → Feature Selection → Data Balancing → Model Training → Model Evaluation**

---

## Dataset

### CIC-IDS 2018

The project uses the **CIC-IDS 2018** network intrusion detection dataset.

The dataset contains benign network traffic together with multiple categories of malicious traffic.

Attack categories observed during the project include:

- Benign
- Bot
- Brute Force - Web
- Brute Force - XSS
- DDoS attack - HOIC
- DoS attacks - GoldenEye
- DoS attacks - Hulk
- DoS attacks - SlowHTTPTest
- DoS attacks - Slowloris
- Infiltration
- SQL Injection

> **Note:** The dataset is not included in this repository because of its large file size. Download the CIC-IDS 2018 dataset separately before running the notebook.

---

## Project Workflow

### 1. Data Preprocessing

The raw network traffic data is prepared before model training.

The preprocessing stage includes:

- Loading the CIC-IDS 2018 dataset
- Data cleaning
- Handling invalid/infinite values
- Encoding categorical features
- Feature scaling
- Normalization/standardization
- Preparing training and testing data

---

### 2. Dimensionality Reduction using PCA

**Principal Component Analysis (PCA)** is used to reduce the dimensionality of the network traffic features.

The implementation retains approximately **95% of the variance**:

```python
pca = PCA(n_components=0.95)
```

In the experiment, the first two principal components accounted for approximately:

```text
PC1 = 0.766
PC2 = 0.220
```

PCA was also used to visualize the distribution of the transformed training data.

---

### 3. Handling Class Imbalance using SMOTE

Network intrusion datasets can contain significant class imbalance because benign traffic and common attacks may greatly outnumber rare attack categories.

The project uses **SMOTE (Synthetic Minority Over-sampling Technique)** to generate synthetic samples for minority classes.

```python
smote = SMOTE(
    random_state=42,
    k_neighbors=1
)
```

This creates a more balanced training dataset for subsequent machine-learning experiments.

---

## Machine Learning Models

Several machine-learning and deep-learning approaches were explored.

### Random Forest

Random Forest was used as one of the tree-based ensemble classifiers after PCA and SMOTE preprocessing.

The model used:

```python
RandomForestClassifier(
    n_estimators=100,
    random_state=42
)
```

Confusion-matrix analysis was performed to study predictions across different traffic classes.

---

### Elastic Net Regression

Elastic Net combines **L1 (Lasso)** and **L2 (Ridge)** regularization.

The experiment used:

```python
ElasticNet(
    alpha=1.0,
    l1_ratio=0.5
)
```

The regression output was converted into class predictions for evaluation.

Experimental accuracy:

```text
Elastic Net Accuracy: 0.0385
```

The low performance illustrates that the tested Elastic Net configuration was not well suited to this intrusion-classification setup.

---

### Recurrent Neural Network / LSTM

A recurrent neural-network experiment was implemented using **LSTM layers**.

Architecture:

```text
Input
  ↓
LSTM (50 units, return_sequences=True)
  ↓
LSTM (50 units)
  ↓
Dense (Sigmoid)
  ↓
Prediction
```

The model was trained using:

- Adam optimizer
- Binary cross-entropy loss
- Batch size: 32
- 5 epochs

Experimental accuracy:

```text
RNN Accuracy: 0.0385
```

This experiment demonstrates the limitations of directly applying the tested binary RNN configuration to the multiclass structure of the dataset.

---

### XGBoost

**Extreme Gradient Boosting (XGBoost)** was investigated as another ensemble-learning technique.

Example configuration:

```python
XGBClassifier(
    n_estimators=100,
    random_state=42
)
```

Feature-importance analysis was also performed as part of the XGBoost experiment.

Features highlighted in the analysis included:

- Protocol
- Destination Port

---

### Decision Tree

Decision Tree classification was evaluated across the different intrusion categories.

The project includes:

- Multiclass confusion matrix
- Precision
- Recall
- F1-score
- ROC analysis

The ROC analysis demonstrates that classification performance differs considerably across individual attack classes.

---

### AdaBoost

AdaBoost was evaluated as an ensemble classification approach.

The implemented classifier used:

```python
AdaBoostClassifier(
    n_estimators=50,
    random_state=42
)
```

### AdaBoost Experimental Results

| Metric | Result |
|---|---:|
| Accuracy | 0.85 |
| Macro Precision | 0.85 |
| Macro Recall | 0.85 |
| Macro F1-score | 0.85 |
| Weighted Precision | 0.85 |
| Weighted Recall | 0.85 |
| Weighted F1-score | 0.85 |

The binary experiment produced the following confusion matrix:

```text
[[91, 14],
 [21, 94]]
```

Class-wise performance:

| Class | Precision | Recall | F1-score | Support |
|---|---:|---:|---:|---:|
| 0 | 0.83 | 0.87 | 0.85 | 105 |
| 1 | 0.87 | 0.82 | 0.85 | 115 |

These results correspond to the specific AdaBoost experiment included in the project and should not be interpreted as a direct comparison with every multiclass experiment in the notebook.

---

## Evaluation Metrics

The models were analyzed using several classification metrics:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion Matrix
- ROC curves
- Class-wise performance

These metrics provide a broader view of model behavior than accuracy alone, particularly when working with imbalanced intrusion-detection datasets.

---

## Visualizations

The project contains visual analyses including:

- PCA visualization
- Confusion matrices
- Class-wise precision, recall and F1-score
- RNN learning curve
- XGBoost feature importance
- Multiclass ROC curves

These visualizations help analyze feature representation, model behavior and classification performance.

---

## Technologies Used

### Programming Language

- Python

### Data Processing

- Pandas
- NumPy

### Machine Learning

- Scikit-learn
- XGBoost
- Imbalanced-learn

### Deep Learning

- TensorFlow
- Keras

### Visualization

- Matplotlib
- Seaborn

### Development Environment

- Jupyter Notebook / Google Colab

---

## Repository Structure

```text
ML-Based-Network-Intrusion-Detection/
│
├── ML_ProjREV3.ipynb
│   └── Main implementation and experiments
│
├── Anthrax Detection Technology Project Proposal by Slidesgo.pptx
│   └── Project presentation
│
├── .gitignore
│   └── Excludes the large CIC-IDS 2018 dataset and temporary files
│
└── README.md
    └── Project documentation
```

The original CIC-IDS 2018 CSV file is intentionally excluded from GitHub because of its large size.

---

## Team Members

| Team Member | Registration Number | Contribution |
|---|---|---|
| Sujana S | 22MIA1142 | Feature selection, PCA, SMOTE and Ensemble Feature Selection |
| Daphni Michelle B | 22MIA1159 | Random Forest, reinforcement-learning techniques and bagging |
| Sree Darshne J | 22MIA1167 | Elastic Net Regression, Recurrent Neural Networks and XGBoost classification |

---

## Key Experimental Observations

The project demonstrates that intrusion-detection performance depends strongly on both preprocessing and the selected learning algorithm.

PCA substantially reduced the feature space while retaining most of the variance represented in the experiment.

SMOTE was introduced to address class imbalance before model training.

Tree-based and ensemble approaches were explored alongside regression and neural-network approaches.

The experiments also demonstrate that simply applying a more complex model does not guarantee improved intrusion-detection performance. In the tested configurations, Elastic Net and RNN produced poor results, while the separate AdaBoost experiment achieved substantially stronger binary classification performance.

---

## Future Improvements

Possible extensions to the project include:

- Hyperparameter optimization
- Improved multiclass handling
- More robust feature-selection techniques
- Better treatment of highly imbalanced attack categories
- Deep-learning architectures designed specifically for network traffic
- Comparison using consistent train/test splits across all models
- Cross-validation
- Precision-Recall and ROC-AUC analysis
- Real-time network traffic inference
- Explainable AI techniques such as SHAP
- Model deployment as a real-time IDS service

---

## Academic Context

This project was developed as part of academic coursework at **Vellore Institute of Technology (VIT), Chennai**.

### Authors

**Sujana S**  
**Daphni Michelle B**  
**Sree Darshne J**

---

## Disclaimer

This repository is intended for **academic and educational purposes**. The intrusion-detection models and experimental results should not be considered production-ready cybersecurity systems.
