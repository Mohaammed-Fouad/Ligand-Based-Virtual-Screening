# DYRK1b Inhibitor Virtual Screening Pipeline

This repository contains the computational workflow developed for the **9th Advanced In Silico Drug Design Workshop (2026)**. The pipeline was designed to screen a blind dataset of **3,000 compounds** to identify potential **DYRK1b inhibitors**.

The approach utilizes a **Rank-Sum Consensus Strategy**, integrating ligand-based similarity, physicochemical filtering, and structure-based docking scores to minimize false positives.

---

## 📌 Project Overview

The challenge involved filtering a massive dataset to finding the top 100 high-confidence leads. Our solution moves beyond single-metric reliance by implementing a multi-stage funnel:

1. **Preprocessing:** Standardization and salt removal of raw library files.
2. **Ligand-Based Triage:** 2D Similarity screening using Morgan Fingerprints and Butina Clustering to ensure chemical diversity.
3. **Physicochemical Filtering:** Strict adherence to Lipinski's Rule of Five and Veber's criteria.
4. **Consensus Ranking:** A "Goldilocks" selection method combining docking affinity (AutoDock Vina) and similarity rankings.

---

## 📂 Repository Structure

```bash
├── data/
│   ├── raw_library.sdf          # Original blind dataset
│   ├── active_seeds.smi         # Known DYRK1b inhibitors (Reference)
│   └── docking_results.xlsx     # Output from AutoDock Vina (Energy scores)
├── scripts/
│   ├── 01_mol_cleaner.py        # Salt stripper & SDF-to-SMILES converter
│   ├── 02_similarity_triage.py  # Tanimoto search, Butina clustering & PhysChem filters
│   └── 03_consensus_scorer.py   # Rank-Sum integration & Visualization
├── output/
│   ├── final_100_leads.xlsx     # Final submission file
│   └── consensus_plot.png       # Visualization of the selection zone
└── README.md

```

---

## 🚀 Methodology & Workflow

### Phase 1: Preprocessing & Standardization

* **Script:** `01_mol_cleaner.py`
* **Logic:** Converts raw `.sdf` or `.mol2` files to standardized SMILES. It utilizes `rdMolStandardize.LargestFragmentChooser` to strip salts, solvents, and ions to ensure accurate molecular weight calculations.

### Phase 2: Similarity Screening & Diversity Clustering

* **Script:** `02_similarity_triage.py`
* **Fingerprinting:** Generates Morgan Fingerprints (Radius 2, 2048-bit) for the blind dataset.
* **Similarity Search:** Calculates Tanimoto Similarity against a reference set of known actives.
* **Clustering:** Implements **Butina Clustering** (Threshold: 0.35) to select representative centroids, preventing structural redundancy in the top candidates.
* **ADMET Filters:** Filters compounds based on Lead-Like criteria.

### Phase 3: Rank-Sum Consensus Selection

* **Script:** `03_consensus_scorer.py`
* **Concept:** To avoid the bias of a single modality, we calculate a consensus score:


* *Lower* similarity rank = Closer to known active.
* *Lower* docking rank = Better binding energy (more negative).


* **Selection:** The top 100 compounds with the lowest combined rank are selected as the final submission.

---

## 📊 Visualization

The pipeline automatically generates a **Consensus Plot** (`consensus_plot.png`) to visualize the "Selection Zone."

* **X-Axis:** Similarity Score (Higher is better)
* **Y-Axis:** Docking Score (Lower/More Negative is better)
* **Red Points:** The top 100 "Goldilocks" candidates selected for submission.

---

## 🛠️ Usage

### 1. Install Dependencies

```bash
pip install rdkit pandas matplotlib seaborn openpyxl tqdm

```

### 2. Run the Cleaning Script

Upload your raw SDF file to generate a clean SMILES list.

```python
python scripts/01_mol_cleaner.py

```

### 3. Run the Similarity Screen

This will output the top 500 diverse candidates for docking.

```python
python scripts/02_similarity_triage.py

```

*Input:* `cleaned_compounds.smi`, `active_seeds.smi`
*Output:* `final_500_selection.xlsx`

### 4. Run the Consensus Scorer

After running docking externally (e.g., AutoDock Vina), merge the results here.

```python
python scripts/03_consensus_scorer.py

```

*Input:* `final_500_selection.xlsx`, `docking_results.xlsx`
*Output:* `FINAL_SUBMISSION_100.xlsx`

---

## 🤝 Acknowledgements

* **Event:** 9th Advanced In Silico Drug Design Workshop (2026)
* **Tools Used:** RDKit, AutoDock Vina, Python (Pandas/Seaborn).
* **Special Thanks:** To my team members and supervisors for the strategic planning and "stress-testing" of our algorithms.
