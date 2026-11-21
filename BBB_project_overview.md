# BBB Penetration Prediction

This project investigates whether conventional molecular physicochemical properties and structural fingerprints can distinguish compounds that penetrate the blood–brain barrier (BBB) from those with limited or no BBB penetration.

The work combines cheminformatics, exploratory data analysis, statistical testing, machine learning, and model-applicability analysis.

## Research question

Can conventional molecular properties distinguish drugs that penetrate the blood–brain barrier (BBB) from those with limited or no penetration, and what characteristics are associated with compounds that are incorrectly classified?

## Hypothesis

BBB-penetrating compounds are expected to show physicochemical features associated with membrane permeability, such as lower polar surface area and moderate lipophilicity. However, these simple descriptors alone may not reliably classify every compound because BBB penetration also depends on active transport, efflux mechanisms, metabolism, protein binding, and experimental assay conditions.

## Dataset

The project uses the B3DB BBB classification dataset.

- Initial dataset size: 7,807 compounds
- BBB-positive compounds: 4,956
- BBB-negative compounds: 2,851
- Final modelling dataset: 7,805 unique compounds
- Duplicate SMILES in modelling data: 0
- Primary label: BBB penetration status (`BBB` vs `BBB-`)

The dataset includes compound identifiers, SMILES structures, logBB values, BBB labels, and source references.

## Molecular descriptors

The following RDKit-derived molecular descriptors were calculated:

| Descriptor | Description |
|---|---|
| Molecular weight (MW) | Approximate molecular size |
| LogP | Lipophilicity |
| TPSA | Topological polar surface area |
| HBD | Hydrogen-bond donors |
| HBA | Hydrogen-bond acceptors |
| Rotatable bonds | Molecular flexibility |
| Aromatic rings | Aromatic ring count |
| Fraction Csp3 | Fraction of sp3-hybridised carbon atoms |

## Methods

### 1. Data preparation

- Loaded the B3DB dataset directly from its GitHub source.
- Removed compounds with missing descriptor values.
- Verified that all SMILES values were unique.
- Encoded BBB penetration as a binary classification target:
  - `BBB` = 1
  - `BBB-` = 0

### 2. Exploratory analysis

Median descriptor values were compared between BBB-positive and BBB-negative compounds.

| Descriptor | BBB-positive median | BBB-negative median |
|---|---:|---:|
| Molecular weight | 330.48 | 424.53 |
| LogP | 2.92 | 1.46 |
| TPSA | 54.03 | 115.06 |
| Hydrogen-bond donors | 1 | 3 |
| Hydrogen-bond acceptors | 4 | 6 |
| Rotatable bonds | 4 | 6 |

BBB-positive compounds tended to have lower molecular weight, lower polar surface area, lower hydrogen-bonding capacity, and higher lipophilicity.

### 3. Statistical testing

Mann–Whitney U tests were used to compare descriptor distributions between BBB-positive and BBB-negative compounds.

All eight descriptors showed statistically significant differences between the two groups, with p-values ranging from approximately \(2.21 \times 10^{-2}\) to effectively zero.

### 4. Baseline classification model

A logistic regression model was trained using the eight molecular descriptors.

- Train/test split: 80/20
- Stratified sampling: Yes
- Random state: 42
- Scaling: StandardScaler
- Model: Logistic regression

Baseline test performance:

| Metric | Score |
|---|---:|
| Accuracy | 0.80 |
| ROC-AUC | 0.828 |
| BBB-positive recall | 0.92 |
| BBB-negative recall | 0.59 |

The baseline model showed strong performance for BBB-positive compounds but weaker performance for BBB-negative compounds, reflecting the class imbalance and the biological complexity of BBB penetration.

### 5. Error analysis

The initial logistic regression model produced:

- 232 false positives: BBB-negative compounds predicted as BBB-positive
- 80 false negatives: BBB-positive compounds predicted as BBB-negative

Many false positives had low polarity and high lipophilicity, suggesting that simple physicochemical rules can overestimate BBB penetration for structurally complex molecules.

### 6. Scaffold-based evaluation

To reduce the risk that the model simply memorises structurally similar compounds, a scaffold-based train/test split was introduced.

- Training compounds: 6,382
- Test compounds: 1,423
- Shared scaffolds between train and test sets: 0

This is a more conservative evaluation because it tests whether the model generalises to chemically distinct scaffold families.

### 7. Model comparison

Several models were evaluated using the scaffold-based split:

| Model | Accuracy | Balanced accuracy | ROC-AUC | MCC |
|---|---:|---:|---:|---:|
| Logistic regression | 0.7716 | 0.7507 | 0.8379 | 0.5392 |
| Random forest | 0.8173 | 0.8089 | 0.8880 | 0.6264 |
| Gradient boosting | 0.7948 | 0.7864 | 0.8780 | 0.5801 |
| Morgan fingerprint random forest | 0.8046 | 0.7927 | 0.9017 | 0.6011 |

Random forest using molecular descriptors achieved the strongest balanced performance, while Morgan fingerprint-based random forest achieved the highest ROC-AUC.

### 8. Applicability domain analysis

Morgan fingerprints were used to measure the maximum structural similarity between each test compound and the training set.

| Similarity group | Compounds | Accuracy | Balanced accuracy |
|---|---:|---:|---:|
| Low similarity (< 0.3) | 197 | 0.6345 | 0.6010 |
| Moderate similarity (0.3–0.5) | 454 | 0.7423 | 0.6911 |
| High similarity (0.5–0.7) | 597 | 0.8760 | 0.8702 |
| Very high similarity (> 0.7) | 175 | 0.9143 | 0.8968 |

Prediction accuracy increased substantially with greater structural similarity to the training set. This demonstrates the importance of applicability-domain analysis in computational drug-discovery models.

## Key findings

- Molecular size, polarity, hydrogen-bonding capacity, and lipophilicity are strongly associated with BBB penetration.
- BBB-positive compounds generally have lower TPSA, fewer hydrogen-bond donors and acceptors, and higher LogP.
- A simple descriptor-based logistic regression model reaches approximately 80% accuracy and 0.83 ROC-AUC.
- Scaffold-based evaluation provides a more realistic estimate of generalisation than a random split.
- Random forest improves performance, reaching 0.82 accuracy, 0.81 balanced accuracy, and 0.89 ROC-AUC under scaffold-based evaluation.
- Morgan fingerprints improve ranking performance, reaching 0.90 ROC-AUC.
- Model performance depends strongly on structural similarity to the training data.
- High-confidence errors still occur, particularly for large, highly polar, or structurally complex compounds.

## Limitations

- BBB penetration is a complex biological process and cannot be fully captured by static molecular descriptors.
- The dataset labels may combine evidence from different experimental sources and definitions of BBB penetration.
- Class imbalance may affect model calibration and error rates.
- Scaffold-based evaluation is more conservative but may still contain hidden analogue bias.
- The model is intended for research and educational purposes, not clinical decision-making.

## Future work

- Add external validation using an independent BBB dataset.
- Include additional descriptors such as ionisation state, charge, solubility, and 3D descriptors.
- Compare transformer-based molecular representations with classical fingerprints.
- Perform uncertainty quantification and conformal prediction.
- Investigate transporter-mediated BBB penetration separately from passive permeability.
- Deploy the trained model through a simple Streamlit web application.

## Technologies used

- Python
- pandas
- NumPy
- RDKit
- scikit-learn
- Matplotlib
- Seaborn
- SciPy
- Google Colab

## Repository structure

```text
BBB-Penetration-Prediction/
│
├── BBB.ipynb
├── README.md
├── requirements.txt
├── data/
│   └── b3db_classification.tsv
├── figures/
└── results/
