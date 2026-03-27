# Beyond Potency: A Multi-Target Lead Discovery Pipeline for *S. aureus* Efflux Pump Inhibitors

![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg?style=for-the-badge&logo=python)
![RDKit](https://img.shields.io/badge/Chemoinformatics-RDKit-green.svg?style=for-the-badge)
![NetworkX](https://img.shields.io/badge/Network_Theory-NetworkX-red.svg?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-MSc_Dissertation-orange.svg?style=for-the-badge)

## 📋 Table of Contents
- [1. Project Motivation](#1-project-motivation)
- [2. The Scientific Challenge: NorA Efflux](#2-the-scientific-challenge-nora-efflux)
- [3. The 6-Phase Computational Workflow](#3-the-6-phase-computational-workflow)
- [4. Mathematical Scoring & Prioritization](#4-mathematical-scoring--prioritization)
- [5. Advanced Validation: PCA & ADMET](#5-advanced-validation-pca--admet)
- [6. Data Integrity & Reproducibility](#6-data-integrity--reproducibility)
- [7. Key Findings](#7-key-findings)
- [8. Installation & Usage](#8-installation--usage)

---

## 1. Project Motivation
Antimicrobial Resistance (AMR) is one of the top 10 global public health threats. Traditional antibiotic discovery often fails because it focuses solely on **potency** ($IC_{50}$), ignoring how effectively a molecule uses its size and lipophilicity to reach its target. This project implements a **"Smart Data"** approach to drug discovery, prioritizing molecules that are efficient, drug-like, and strategically positioned within the bacterial resistance network.

---

## 2. The Scientific Challenge: NorA Efflux
*Staphylococcus aureus* utilizes the **NorA pump** (a Major Facilitator Superfamily transporter) to actively extrude fluoroquinolones and other biocides. 
* **The Problem:** Many potent inhibitors are too "greasy" (high LogP) or too large, leading to poor pharmacokinetics and off-target toxicity.
* **The Solution:** Identifying inhibitors with high **Ligand Efficiency (LE)** and **Lipophilic Ligand Efficiency (LLE)** that can block NorA while maintaining a "drug-like" profile.

---

## 3. The 6-Phase Computational Workflow

### **Phase 0: Data Retrieval & Sanitization**
Data is fetched via the **ChEMBL API** for Target ID `CHEMBL5114`. 
* **Sanitization:** Salt stripping, neutralization, and valence checking using RDKit.
* **Normalization:** Conversion of varied bioactivity units to $pIC_{50}$ ($-\log_{10}(M)$).

### **Phase 1: Physicochemical Feature Engineering**
We calculate the "Rule of 5" descriptors for the entire library:
* **MW:** Molecular Weight (Da)
* **cLogP:** Calculated Octanol-Water Partition Coefficient
* **TPSA:** Topological Polar Surface Area (Å²)
* **HAC:** Heavy Atom Count

### **Phase 2: Efficiency Profiling**
We move beyond raw potency by calculating:
* **LE (Ligand Efficiency):** Efficiency per heavy atom.
* **LLE (Lipophilic Ligand Efficiency):** Potency relative to lipophilicity—a key predictor of ADMET success.

### **Phase 3: Proteome-Wide Mapping**
Using **Taxonomy ID: 1280**, we expand the search to the entire *S. aureus* proteome to check if our NorA leads have known activity against other resistance targets (e.g., GrlA, GyrA).

### **Phase 4: Bipartite Network Topology**
Using `NetworkX`, we construct a bipartite graph. This allows us to identify **Hub Inhibitors**—molecules that sit at the center of the resistance network, offering a multi-target "polypharmacology" approach.

### **Phase 5: Multi-Criteria Decision Analysis (MCDA)**
We rank the compounds using a weighted composite score (see [Section 4](#4-mathematical-scoring--prioritization)).

### **Phase 6: Advanced Validation**
* **PCA:** Principal Component Analysis to map the structural novelty of our leads.
* **ADMET:** Stress-testing leads against **Lipinski** and **Veber** rules.

---

## 4. Mathematical Scoring & Prioritization

The final selection of the **"Champion Scaffold"** is governed by our integrated scoring function:

$$Score = (pIC_{50\_norm} \times 0.4) + (LLE_{norm} \times 0.4) + (Degree_{norm} \times 0.2)$$

### **Key Formulas:**
- **LLE:** $pIC_{50} - cLogP$ (Target: > 5 for drug-likeness)
- **LE:** $\frac{1.37 \times pIC_{50}}{HAC}$ (Target: > 0.3 kcal/mol/HA)

---

## 5. Advanced Validation: PCA & ADMET
A critical part of this research is the **PCA (Principal Component Analysis)**. By reducing the molecular descriptors ($MW, LogP, TPSA, HAC, pIC_{50}$) into two principal components, we can visualize the **Chemical Territory** of our leads.

* **PC1:** Typically represents molecular size and complexity.
* **PC2:** Typically represents polarity and binding efficiency.
Our "Champion" leads were validated as structural outliers, suggesting they represent a **novel chemotype** against NorA.

---

## 6. Data Integrity & Reproducibility
To ensure the academic rigor required for an MSc dissertation, this repository includes an **Audit Protocol**:
1. **Source Traceability:** Every lead is linked to its unique ChEMBL ID.
2. **Calculation Audit:** A "Truth Table" is generated comparing raw atomic counts (HBD, HBA, Rotatable Bonds) against the final ADMET scores.
3. **Data Completeness:** The pipeline ensures 0% data loss during the transition from Phase 0 to Phase 6.

---

## 7. Key Findings
* **Primary Lead:** `CHEMBL5416410` identified as the high-efficiency scaffold.
* **ADMET Status:** Passed 6/6 criteria (Lipinski + Veber compliance).
* **Network Impact:** Identified as a "Hub" molecule with high connectivity across the *S. aureus* resistance proteome.

---

## 8. Installation & Usage

### **Setup**
```bash
# Clone the repository
git clone [https://github.com/festusoladayoonline-debug/Beyond-Potency-NorA.git](https://github.com/festusoladayoonline-debug/Beyond-Potency-NorA.git)

# Install necessary libraries
pip install rdkit pandas numpy matplotlib seaborn networkx scikit-learn chembl_webresource_client
