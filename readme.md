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