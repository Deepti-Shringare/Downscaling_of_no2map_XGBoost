# Methodology

## 1. Study Area
The study focuses on the National Capital Territory (NCT) of Delhi, India, characterized by high population density, complex emission sources, and severe seasonal air pollution.

---

## 2. Data Sources

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

## 4. Data Preprocessing

Robust preprocessing steps were applied to ensure spatial and temporal consistency:

- **Spatial alignment:**  
  All datasets were reprojected to a common coordinate reference system and resampled to a consistent grid.
- **Temporal matching:**  
  Monthly composites were generated to align datasets with different temporal resolutions.
- **Missing values:**  
  Pixels affected by clouds or data gaps were masked and excluded from model fitting to avoid bias.

These steps ensure reproducibility and engineering robustness.

---



## 5. Model Training

A supervised machine learning model was trained to learn the nonlinear relationship between predictor variables and NO₂ concentrations.

- **Algorithm used:** Extreme Gradient Boosting (XGBoost)
- **Why XGBoost:**  
  XGBoost efficiently captures complex nonlinear interactions, handles heterogeneous predictors, and performs well with limited training samples.

### Train–Test Strategy
- **Temporal split:**  
  Training on earlier months and testing on later months to assess seasonal generalization.
- **Spatial split:**  
  Selected monitoring stations were completely excluded from training to evaluate spatial transferability.

### Model Evaluation
Model performance was assessed using:
- Root Mean Square Error (RMSE)
- Mean Absolute Error (MAE)
- Coefficient of Determination (R²)

This ensures both accuracy and generalization capability.

---

## 6. Fishnet Grid Creation

A fine-resolution fishnet grid was generated to produce high-resolution outputs.

- **Grid size:** 500 m × 500 m
- **Total cells:** Dependent on study area extent
- **Justification:**  
  This resolution balances computational feasibility with urban-scale spatial detail, capturing neighborhood-level NO₂ variability without excessive noise.

---

## 7. Surface NO₂ Estimation (Critical Step)

Downscaled NO₂ column values were converted to surface concentrations using boundary layer physics.

### 7.1 Equation
\[
NO_{2,\;surface} = \frac{NO_{2,\;column}}{PBLH}
\]

### 7.2 Units
- NO₂ column: mol/m²  
- PBLH: meters  
- Surface NO₂: mol/m³

### 7.3 Assumptions
- NO₂ is vertically well-mixed within the planetary boundary layer.
- Contributions above the boundary layer are minimal for surface exposure assessment.
- Monthly mean PBLH adequately represents vertical mixing conditions.

This step anchors the ML output in atmospheric physics.

---

## 8. Output Generation

The final surface NO₂ estimates were exported as GeoTIFF files.

- **Format:** GeoTIFF
- **Projection:** WGS 84
- **Why GeoTIFF:**  
  GeoTIFF is a standard geospatial format compatible with GIS software, cloud platforms, and policy workflows.

---

## 9. Visualization

All intermediate and final products were visualized using Google Earth Engine.

- **Displayed layers:**  
  - Coarse NO₂ columns  
  - Downscaled fine-resolution NO₂  
  - Surface-corrected NO₂ estimates
- **Purpose:**  
  To enable qualitative assessment, spatial comparison, and interactive inspection

  
  ## 10. Model Validation
Model predictions were validated using independent CPCB monitoring stations not used during training. Statistical metrics (RMSE, MAE, R²) were computed to assess agreement between predicted and observed NO₂ concentrations. This validation strategy evaluates both spatial and temporal robustness of the proposed framework.

## 11. Uncertainty and Assumptions

The derived surface NO₂ concentrations represent estimated values subject to uncertainties in satellite retrievals, meteorological reanalysis data, and model assumptions. In particular, the assumption of uniform vertical mixing within the planetary boundary layer may not hold during stable atmospheric conditions. Therefore, the results are intended for scientific analysis and relative spatial assessment rather than regulatory compliance.



