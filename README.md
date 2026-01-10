# Downscaling of NO₂ Maps using XGBoost and Google Earth Engine

This project presents a **physically-consistent machine learning framework** for downscaling coarse-resolution satellite NO₂ observations to fine spatial resolution over Delhi, India. The approach integrates **Sentinel-5P TROPOMI NO₂**, **meteorological reanalysis**, and **ground-based CPCB station data**, with deployment and visualization on **Google Earth Engine (GEE)**.

The project is designed as:
- A **research-grade air quality downscaling study**
- A **production-ready geospatial ML pipeline**
- A **solo, end-to-end project** suitable for DRDO / ISRO / product-based company evaluation

---

## 📌 Problem Statement

Satellite-based NO₂ products (e.g., TROPOMI) provide global coverage but at coarse spatial resolution (~7 km), limiting their usability for:
- Urban exposure analysis
- City-scale pollution management
- Localized alert systems

This project addresses the gap by generating **500 m resolution NO₂ maps** while preserving physical consistency with satellite observations.

---

## 🧠 Core Idea

1. Learn **sub-grid spatial patterns** of NO₂ using ML (XGBoost)
2. Downscale coarse satellite columns using high-resolution predictors
3. Apply **column-conserving correction**
4. Convert column NO₂ to **surface concentration** using PBL height
5. Visualize and inspect outputs interactively using GEE

---

## 🧠 Methodology Overview
## 🧠 Methodology Overview

```mermaid
flowchart TD
    A[Data Collection] --> B[CPCB Station NO2]
    A --> C[Population Density]
    A --> D[VIIRS Night-time Lights]
    A --> E[ERA5-Land Meteorology]
    A --> F[ERA5-Atmosphere]

    E --> E1[Wind: u10, v10]
    E --> E2[Temperature]
    E --> E3[Surface Pressure]

    F --> F1[Boundary Layer Height]
    F --> F2[Cloud Cover]

    B --> G[Data Preprocessing & Merging]
    C --> G
    D --> G
    E --> G
    F --> G

    G --> H[Model Training & Testing]
    H --> H1[Random Forest]
    H --> H2[XGBoost]

    H --> I[500m × 500m Fishnet Grid]
    I --> J[High-resolution NO2 Prediction]

    J --> K[Surface NO2 Estimation]
    K --> L[GeoTIFF Output]

    L --> M[GEE Visualization]




## 🛰️ Data Sources

| Data | Source | Resolution |
|----|------|------------|
| NO₂ Column | Sentinel-5P TROPOMI | ~7 km |
| Meteorology | ERA5 Reanalysis | ~30 km |
| Land Features | MODIS, population, roads | 500 m |
| Ground Truth | CPCB (39 stations, Delhi) | Point-based |

---

## ⚙️ Methodology Overview

- Feature engineering using meteorology, land-use, and emission proxies
- ML model training using **XGBoost regression**
- Monthly NO₂ prediction at 500 m resolution
- Column-conserving scaling using TROPOMI
- Surface NO₂ estimation using ERA5 PBL height
- Interactive inspection via GEE split-panel UI

➡️ Full details: [`METHODOLOGY.md`](./METHODOLOGY.md)

---

## 🗺️ Google Earth Engine Integration

- Monthly aggregation of satellite data
- Physically corrected surface NO₂ estimation
- Pixel-wise inspection panel
- Discrete color-coded alert visualization
- ML coverage masking

---

## 📊 Outputs

- Fine-resolution NO₂ column proxy (500 m)
- Surface NO₂ concentration (µg/m³)
- Interactive GEE App with inspector panel
- GeoTIFF exports for further analysis

---

## ⚠️ Disclaimer

This project is **research-oriented**. Surface NO₂ values are estimates and should not be interpreted as regulatory measurements.

---

## 👩‍💻 Author

**Deepti Shringare**  
B.E. EXTC | Air Quality ML | Earth Observation  
Solo Project (End-to-End)
