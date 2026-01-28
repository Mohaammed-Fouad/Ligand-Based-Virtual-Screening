# Ligand-Based-Virtual-Screening
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![RDKit](https://img.shields.io/badge/Cheminformatics-RDKit-green)
![Scikit-Learn](https://img.shields.io/badge/ML-OneClassSVM-orange)

## 📌 Overview
This repository implements a **Ligand-Based Virtual Screening (LBVS)** pipeline designed to identify potential hits within a "blind" dataset of chemical compounds. 

Unlike traditional binary classification, which requires both active and inactive datasets, this tool utilizes **One-Class Classification** to define the chemical space of known active compounds. It employs a consensus approach combining **Tanimoto Similarity** (explicit similarity) and **One-Class SVM** (implicit chemical space mapping) to filter potential drug candidates.

## ⚙️ Methodology
The pipeline processes chemical structures (SMILES) through the following workflow:

1.  **Feature Extraction:** Converts molecular structures into **Morgan Fingerprints (ECFP4)** with a radius of 2 and 2048 bits.
2.  **Similarity Search:** Calculates the **Tanimoto Coefficient** for every blind compound against the known active set.
3.  **Anomaly Detection (Machine Learning):**
    * Trains a **One-Class Support Vector Machine (SVM)** with an RBF kernel on the active compounds.
    * Establishes a decision boundary to distinguish "active-like" compounds from outliers in the blind set.
4.  **Consensus Scoring:** Filters for "Golden Hits"—compounds that satisfy both high structural similarity (>0.7) and positive ML prediction.

## 🛠️ Dependencies
* **Python 3.x**
* **RDKit** (Molecular handling and descriptors)
* **Scikit-learn** (Machine Learning implementation)
* **Pandas & NumPy** (Data manipulation)

```bash
pip install rdkit scikit-learn pandas numpy
