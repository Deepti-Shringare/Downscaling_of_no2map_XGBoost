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
|ERA5 LAND| U10,V10,WINDSPEED,Temperature, Surface pressure|
|VIIRS| Night time dataset|
|Worldpop| Population density|
|CPCB| Sation wise NO2|

---

## 3. Feature Engineering

Predictor variables include:
- Meteorological parameters (temperature, wind, humidity)
- Land-use proxies
- Population density
- Road density
- Spatial coordinates (lat/lon)

All features are resampled to a common 500 m grid.

---

## 4. Machine Learning Downscaling

- Model: XGBoost Regression
- Target: NO₂ column enhancement factor
- Training strategy:
  - Spatial station holdout
  - Temporal separation
- Output: Fine-resolution **relative NO₂ column proxy**

⚠️ The ML output is **unitless** and represents relative spatial variation.

---

## 5. Column-Conserving Correction

To preserve satellite mass consistency:

\[
NO₂_{fine} = \frac{NO₂_{ML}}{NO₂_{coarse}} \times NO₂_{TROPOMI}
\]

This ensures:
- No artificial increase in total column mass
- Physical interpretability of fine-scale outputs

---

## 6. Surface NO₂ Estimation

Surface NO₂ concentration is estimated using:

\[
NO₂_{surface} = \frac{NO₂_{column}}{PBL} \times M_{NO₂}
\]

Where:
- PBL = Planetary Boundary Layer height (m)
- \(M_{NO₂}\) = molar mass of NO₂ (46 g/mol)

Final unit: **µg/m³**

---

## 7. Visualization & Inspection

Implemented in Google Earth Engine:
- Split-panel visualization (coarse vs fine)
- Discrete color mapping for pollution levels
- Pixel-level inspector panel
- High-risk surface NO₂ alert masking

---

## 8. Validation Strategy

- Spatial consistency with satellite patterns
- Qualitative comparison with CPCB station trends
- Visual coherence across urban emission hotspots
