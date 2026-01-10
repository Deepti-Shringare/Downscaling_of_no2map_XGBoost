# Limitations

Despite strong physical grounding, the study has several limitations:

---

## 1. ML Proxy Nature
The fine-resolution NO₂ predicted by the ML model is a **relative proxy**, not an absolute column measurement. Physical correction is required before interpretation.

---

## 2. Surface Conversion Assumptions
- Assumes uniform vertical mixing within PBL
- Neglects chemical lifetime and vertical NO₂ gradients
- Does not account for nocturnal residual layers

---

## 3. Temporal Resolution
- Monthly averaging masks short-term pollution spikes
- Not suitable for hourly exposure assessment

---

## 4. Ground Data Constraints
- Limited number of CPCB stations
- Station distribution biased towards urban centers

---

## 5. Satellite Uncertainties
- TROPOMI retrieval errors under clouds
- Reduced sensitivity near the surface

---

## 6. Not a Regulatory Product
The estimated surface NO₂ values:
- Are **not CPCB-certified**
- Should not be used for compliance monitoring
- Are intended for research and decision support

---

## 7. Generalization
Model is trained for Delhi and may not directly generalize to other regions without retraining.

---

## 8. Computational Constraints
High-resolution GEE operations are subject to:
- Memory limits
- Export size constraints
