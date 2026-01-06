# Downscaling_of_no2map_XGBoost
# 🛰️ High-Resolution NO₂ Downscaling & Alert System  
### Satellite–Ground Data Fusion using ML and Atmospheric Correction

## 📌 Overview
This project develops a **scalable geospatial machine learning system** to generate **fine-resolution (500 m) NO₂ pollution maps** from **coarse satellite observations (~7 km)** and convert them into **actionable surface-level pollution alerts** for urban regions.

The system integrates:
- Satellite NO₂ (TROPOMI)
- Meteorological data (ERA5 – Planetary Boundary Layer)
- Ground observations (CPCB stations – India)
- Machine Learning–based spatial downscaling
- Rule-based alert generation

⚠️ **Note**: Surface NO₂ values are *estimated* using atmospheric correction and are **not direct measurements**.

---

## 🎯 Motivation
Urban air pollution monitoring is limited by:
- Sparse ground monitoring stations
- Coarse satellite spatial resolution
- Lack of neighborhood-level pollution intelligence

This project addresses these gaps by enabling:
- High-resolution pollution mapping
- Localized health-risk alerts
- Decision-support for policy and defense applications

---

## 🧠 Key Contributions
- Downscaling satellite NO₂ from ~7 km to **500 m**
- Fusion of satellite, meteorology, and ground data
- PBL-corrected surface NO₂ estimation
- Column-based and surface-based alert generation
- Scalable, product-ready system design

---

## 🗺️ Data Sources

### 1️⃣ Satellite NO₂ (Column)
- **Platform**: Sentinel-5P (TROPOMI)
- **Dataset**: `COPERNICUS/S5P/OFFL/L3_NO2`
- **Resolution**: ~7 km
- **Variable**: Tropospheric NO₂ column (mol/m²)

---

### 2️⃣ Meteorological Data
- **Source**: ERA5 Hourly 
- **Dataset**: `ECMWF/ERA5/HOURLY` and `ECMWF/ERA5_LAND`
- **Variable**: Boundary Layer Height (meters),toatal cloud cover,Temeprature, pressure,u10,v10,windspeed

Used to approximate vertical mixing for surface NO₂ estimation.

---
- **Source**: VIIRS
- **Dataset**: `VIIRS`
- **Variable**: NIGHT TIME RADIATION

### 3️⃣ Ground Truth (CPCB – India)
- **Source**: Central Pollution Control Board (CPCB)
- **Data**: Station-level surface NO₂ (µg/m³)

⚠️ **Data Access Constraint**  
CPCB does **not provide a public API**.

**Approach Used**:
- Manual CSV downloads from official CPCB portal
- Automated preprocessing, validation, and station alignment
- Quality-control flags for missing or unreliable measurements

This reflects **real-world regulatory data constraints** in India.

---

## 🧮 Methodology

### Step 1: Satellite Data Preprocessing
- Daily averaging
- Cloud and no-data masking
- Spatial clipping to region of interest

---

### Step 2: Machine Learning Downscaling
A machine learning model is trained using:
- Satellite NO₂
- Meteorological parameters
- Population and land-use proxies

**Output**:
- Fine-resolution (500 m) NO₂ *column proxy*

> The ML output represents **relative spatial variability**, not a direct physical measurement.

---

### Step 3: PBL-Corrected Surface NO₂ Estimation
Satellite NO₂ represents a **vertical column concentration**.

Surface-level NO₂ is approximated as:

Surface NO₂ ≈ Column NO₂ / PBL Height

Converted to µg/m³ using molecular mass assumptions.

⚠️ This is an **estimated atmospheric correction**, widely used in air-quality research.

---

## 🚨 Alert System Design

### Column-Based Alerts (Relative Risk)
Percentile-based thresholds:
- P50 → Normal
- P75 → High
- P90 → Severe

Useful where surface estimation is uncertain.

---

### Surface-Level Alerts (Estimated)
| Level | Surface NO₂ (µg/m³) | Interpretation |
|------|--------------------|---------------|
| NORMAL | < 80 | Acceptable |
| HIGH | 80–150 | Health Advisory |
| SEVERE | >150 | Health Risk |

Thresholds are configurable and region-dependent.

---

## 🖥️ Visualization
- Side-by-side comparison:
  - Coarse satellite NO₂
  - Fine ML-downscaled NO₂
  - Estimated surface NO₂
- Interactive pixel inspection
- Alert overlays highlighting risk zones

---

## 🧱 System Architecture

