# About Dataset

This project uses two main datasets:

## 1. Raw Dataset

### MESSIDOR
- Contains **1,200 raw Fundus images**
- Image sets: **Base11 – Base14**

### MAPLES-DR
- Contains **198 lesion segmentation images**
- Includes **segmentation masks**
- Divided into **Training** and **Testing** sets
- Covers multiple **lesion classes**

## 2. Matched & Integrated Dataset (`MAPLES_Matched`)
Generated automatically by `step01_matching_data.ipynb` by indexing and matching **MESSIDOR raw fundus images** with **MAPLES-DR expert annotations** using `Image_ID`.

* **Total Verified Images:** **198 unique fundus images** (138 Train / 60 Test)
* **Total Lesion Classes:** **12 classes**

### 📁 Directory Structure
```text
MAPLES_Matched/
├── images/                  # 198 matched raw fundus images (.tif / .jpg)
└── masks/                   # Binary segmentation masks (.png) grouped by class
    ├── BrightUncertains/
    ├── CottonWoolSpots/
    ├── Drusens/
    ├── Exudates/
    ├── Hemorrhages/
    ├── Macula/
    ├── Microaneurysms/
    ├── Neovascularization/
    ├── OpticCup/
    ├── OpticDisc/
    ├── RedUncertains/
    └── Vessels/
```
## 3. Exploratory Data Analysis (EDA)
The dataset exploration and visual analysis are handled in `step02_eda.ipynb`. The main objectives and findings include:

* **Image Resolution & Properties Analysis:** Summarized the physical characteristics and resolutions of the fundus images.
* **Target Class Distribution:** Analyzed the occurrence of 5 primary target classes (`OpticDisc`, `Macula`, `Exudates`, `Hemorrhages`, `Drusens`).
* **Lesion Area Percentage:** Investigated the relative pixel area size of disease/lesion classes to understand the scale of abnormalities (excluding normal anatomy like Optic Disc and Macula).
* **Class Occurrence & Combinations:**
  * Verified dataset integrity by ensuring all images contain the base anatomical structures (minimum 2 classes per image).
  * Analyzed the most frequent combinations of lesions appearing together in a single eye.
* **Visual Audit (Overlay Presentation):** Implemented an automated visualization tool to plot "Hero Images" with color-coded transparency overlays, allowing researchers to visually verify the alignment of masks against raw fundus images side-by-side.

All generated plots and statistical summaries (CSVs) from this phase are automatically exported to the `EDA_Results/` directory.