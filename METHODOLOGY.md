# Methodology

## 1. Study Area
The study focuses on the National Capital Territory (NCT) of Delhi, India, characterized by high population density, complex emission sources, and severe seasonal air pollution.

---

## 2. Data Preprocessing

### 2.1 Satellite NO₂
- Source: Sentinel-5P TROPOMI (OFFL L3)
- Variable: Tropospheric NO₂ column number density (mol/m²)
- Temporal aggregation: Monthly mean (January 2024)
- Spatial clipping to Delhi boundary

### 2.2 Ground Observations
- CPCB station data (39 stations)
- Used for ML training and validation
- Temporal alignment with satellite observations

### 2.3 Meteorological Parameters
| Dataset | Parameters |
|-----|-------|
|ERA5 ATMOS | BLH, Total cloud cover|
|ERA5 LAND| U10, V10, windspeed ,Temperature, Surface pressure|
|VIIRS| Night time dataset|
|Worldpop| Population density|
|CPCB| Sation wise NO2|

---

## 3. Feature Engineering

Predictor variables include:
- Meteorological parameters (temperature, wind, humidity)
- Land-use proxies
- Population density
- Spatial coordinates (lat/lon)

All features are resampled to a common 500 m grid.
To ensure physical relevance and numerical stability, raw datasets were transformed into meaningful predictors:

- **Population count → Population density:**  
  Population counts were normalized by grid-cell area to obtain people per km².
- **Wind components → Wind speed:**  
  Wind speed was computed as:  
  \[
  WS = \sqrt{u^2 + v^2}
  \]
- **Normalization:**  
  Continuous predictors were scaled using min–max normalization to prevent dominance of any single variable during model training.

---

## 5. Data Preprocessing

Robust preprocessing steps were applied to ensure spatial and temporal consistency:

- **Spatial alignment:**  
  All datasets were reprojected to a common coordinate reference system and resampled to a consistent grid.
- **Temporal matching:**  
  Monthly composites were generated to align datasets with different temporal resolutions.
- **Missing values:**  
  Pixels affected by clouds or data gaps were masked and excluded from model fitting to avoid bias.

These steps ensure reproducibility and engineering robustness.

---

## 6. Model Training

A supervised machine learning model was trained to learn the mapping between predictors and NO₂ columns.

- **Algorithm used:** Random Forest Regressor
- **Train–test strategy:**  
  - **Temporal split:** Training on earlier months, testing on later months to evaluate seasonal generalization.
  - **Spatial split:** Selected regions were withheld entirely from training to test spatial transferability.
- **Rationale:**  
  Random Forests are well-suited for nonlinear relationships, robust to multicollinearity, and interpretable.

---

## 7. Fishnet Grid Creation

A fine-resolution fishnet grid was generated to produce high-resolution outputs.

- **Grid size:** 500 m × 500 m
- **Total cells:** Dependent on study area extent
- **Justification:**  
  This resolution balances computational feasibility with urban-scale spatial detail, capturing neighborhood-level NO₂ variability without excessive noise.

---

## 8. Surface NO₂ Estimation (Critical Step)

Downscaled NO₂ column values were converted to surface concentrations using boundary layer physics.

### 8.1 Equation
\[
NO_{2,\;surface} = \frac{NO_{2,\;column}}{PBLH}
\]

### 8.2 Units
- NO₂ column: mol/m²  
- PBLH: meters  
- Surface NO₂: mol/m³

### 8.3 Assumptions
- NO₂ is vertically well-mixed within the planetary boundary layer.
- Contributions above the boundary layer are minimal for surface exposure assessment.
- Monthly mean PBLH adequately represents vertical mixing conditions.

This step anchors the ML output in atmospheric physics.

---

## 9. Output Generation

The final surface NO₂ estimates were exported as GeoTIFF files.

- **Format:** GeoTIFF
- **Projection:** WGS 84
- **Why GeoTIFF:**  
  GeoTIFF is a standard geospatial format compatible with GIS software, cloud platforms, and policy workflows.

---

## 10. Visualization

All intermediate and final products were visualized using Google Earth Engine.

- **Displayed layers:**  
  - Coarse NO₂ columns  
  - Downscaled fine-resolution NO₂  
  - Surface-corrected NO₂ estimates
- **Purpose:**  
  To enable qualitative assessment, spatial comparison, and interactive inspection

