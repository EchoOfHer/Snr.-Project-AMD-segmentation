# 🧠 Pre-Training Phase: MAPLES-DR Dataset Preparation for Retinal Analysis

This repository handles the **data preparation and preprocessing phase** (Steps 01-04) of the **MESSIDOR dataset** and its **MAPLES-DR extension**. The primary objective is to clean, analyze, and structure the data locally before migrating to platforms like **Kaggle** for the actual Model Training phase.

---

## 📖 About the Dataset

The **MESSIDOR dataset** is a widely used medical imaging dataset for studying **diabetic retinopathy (DR)**. It contains high-resolution **color fundus images** along with diagnostic labels.

To enhance research in **segmentation and explainability**, this project utilizes **MAPLES-DR**, an extension of MESSIDOR that provides **pixel-wise annotations of retinal structures**.

---

## 🔬 MAPLES-DR Extension

**MAPLES-DR (MESSIDOR Anatomical and Pathological Labels for Explainable Screening of Diabetic Retinopathy)** is a public dataset that builds on MESSIDOR by adding:

* 🧠 Expert annotations from **retinologists**
* 🧩 Pixel-level segmentation maps
* 🔍 Labels for **10 retinal structures**, including:
  * Optic disc & cup
  * Macula
  * Blood vessels
  * Microaneurysms
  * Hemorrhages
  * Exudates
  * Cotton wool spots
  * Drusen
  * Neovascularization

It contains annotations for **198 MESSIDOR images** and is designed to improve **model explainability and reliability in DR screening**.

⚠️ **Note:**
* MAPLES-DR **does not include the original fundus images**.
* The raw MESSIDOR images were downloaded separately and aligned with the labels via our preprocessing pipeline.

---

## 🎯 Purpose of This Pre-Training Repository

This local repository is exclusively designed to execute the **Data Engineering Workflow**:
* 📂 **Organize:** Provide a highly structured and matched dataset directory.
* 🧪 **Analyze:** Perform deep Exploratory Data Analysis (EDA) on pixel areas and lesion distributions.
* ⚙️ **Preprocess:** Standardize images via Auto-cropping, CLAHE illumination correction, and 512x512 resizing.
* 🔀 **Stratify:** Safely split the dataset (Train/Val/Test) while preserving rare lesion occurrences.

Once this pipeline completes, the `Processed_Dataset/` is ready to be uploaded to Cloud Platforms (e.g., Kaggle, Colab) for deep learning.

---

## 🚀 Pre-Training Pipeline Steps

### 1. Matched & Integrated Dataset (`step01_matching_data.ipynb`)
Generated automatically by indexing and matching **MESSIDOR raw fundus images** with **MAPLES-DR expert annotations** using `Image_ID`.
* **Total Verified Images:** **198 unique fundus images**
* **Total Lesion Classes:** **12 classes**

### 2. Exploratory Data Analysis (`step02_eda.ipynb`)
Analyzed the completeness, distribution, and quality of the matched dataset.
* Investigated pixel area proportions for disease classes.
* Validated multi-class occurrences (minimum 2 anatomical classes per image).
* Generated automated visual audits (Overlay Presentations) to verify mask alignments.
* **Outputs:** Exported to `EDA_Results/`.

### 3. Data Preprocessing (`step03_preprocessing.ipynb`)
Standardized the dataset for model training.
* **Auto-Cropping & Square Padding:** Removed dark borders and applied padding to prevent aspect ratio distortion.
* **Resizing:** Scaled to `512x512` (Images via `INTER_CUBIC`, Masks via `INTER_NEAREST`).
* **Illumination Correction & CLAHE:** Enhanced lesion contrast across all fundus images.
* **Multi-Channel Masking:** Stacked binary masks into a single `.npy` array per image.
* **Outputs:** Exported to `Processed_Dataset/`.

### 4. Dataset Stratification and Splitting (`step04_train_test_split.ipynb`)
Safely divided the preprocessed dataset into Training, Validation, and Testing sets.
* **Multi-label Stratification:** Ensured proportional distribution of diseases across splits using binary concatenation keys (e.g., "11010").
* **Rare Class Isolation:** Prevented the loss of rare lesion presentations by forcefully assigning single-occurrence groups to the Training set.
* **Split Ratio:** **70% Train** / **15% Validation** / **15% Testing**.
* **Outputs:** Exported index references to `train_split.csv`, `val_split.csv`, and `test_split.csv`.

---

## 📚 References

### 📌 MESSIDOR
Decencière et al., *Feedback on a publicly distributed database: the MESSIDOR database*, Image Analysis & Stereology, 2014.

### 📌 MAPLES-DR
Lepetit-Aimon et al., *MAPLES-DR: MESSIDOR Anatomical and Pathological Labels for Explainable Screening of Diabetic Retinopathy*, Scientific Data, 2024.

### 📄 BibTeX
```bibtex
@article{maples_dr,
  title={MAPLES-DR: MESSIDOR Anatomical and Pathological Labels for Explainable Screening of Diabetic Retinopathy},
  author={Lepetit-Aimon, Gabriel and Playout, Clément and Boucher, Marie Carole and Duval, Renaud and Brent, Michael H and Cheriet, Farida},
  journal={Scientific Data},
  volume={11},
  number={1},
  pages={914},
  year={2024},
  doi={10.1038/s41597-024-03739-6}
}
```