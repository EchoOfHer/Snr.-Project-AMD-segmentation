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

## 4. Data Preprocessing
The `step03_preprocessing.ipynb` pipeline prepares and standardizes the dataset for model training. The key operations performed include:

* **Auto-Cropping & Square Padding:** Automatically removes excess black borders around the fundus and applies square padding to preserve the aspect ratio without distorting the eye shape.
* **Resizing:** Standardizes all images and masks to a uniform `512x512` resolution (using `INTER_CUBIC` for images and `INTER_NEAREST` for masks to preserve binary lesion labels).
* **Illumination Correction & CLAHE:** Enhances the contrast of lesions by correcting uneven background lighting and applying Contrast Limited Adaptive Histogram Equalization.
* **Multi-Channel Mask Stacking:** Combines individual binary masks of the target classes into a single multi-channel `.npy` array per image, optimizing it for segmentation model inputs.
* **Validation & Sanity Checks:** Analyzes absolute pixel loss vs. proportion preservation before and after the resize operation, ensuring tiny lesions (e.g., Microaneurysms) remain intact and properly aligned.

All preprocessed images are saved in `Processed_Dataset/images/` and the stacked masks in `Processed_Dataset/masks_multichannel/`.

## 5. Dataset Stratification and Splitting
The `step04_train_test_split.ipynb` handles the safe division of the preprocessed dataset into Training, Validation, and Testing sets while strictly maintaining the distribution of lesions. Key features include:

* **Multi-label Stratification:** Groups images by concatenating the binary presence of all target classes (e.g., "11010") to guarantee proportional distribution across splits.
* **Rare Class Isolation:** Isolates rare combinations (groups containing only 1 sample) and safely assigns them to the Training set to prevent the model from missing unique lesion presentations.
* **Data Splitting Strategy:** Splits the remaining robust dataset into **70% Train**, **15% Validation**, and **15% Testing**.
* **Visual Sanity Check:** Verifies the physical alignment of original masks against preprocessed `512x512` images for the most complex rare cases.

The output splits are saved as references in `Processed_Dataset/train_split.csv`, `val_split.csv`, and `test_split.csv`, ready to be loaded by PyTorch DataLoaders.