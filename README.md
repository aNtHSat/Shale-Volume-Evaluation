# 🪨 Multi-Method Shale Volume (Vsh) Petrophysical Evaluation
### Well: OMA-1321 | Field: Omar Field, Syria | Operator: Al Furat Petroleum Co. (AFPC)
**Focus:** Open-Hole Wireline Formation Evaluation & Petrophysical Log Quality Control (QC)

---

## 📌 Executive Summary
This project conducts an independent, multi-method **Shale Volume ($V_{sh}$)** evaluation on real open-hole wireline log data from **Well OMA-1321** (Omar Field, Syria). 

Rather than relying on single-curve linear approximations, this study:
1. Implements **5 distinct petrophysical models** from raw sensor readings (Computed Gamma Ray, Total Spectral Gamma Ray, Bulk Density, and Neutron Porosity).
2. Performs **Spectral Gamma Ray QC** by isolating uranium content ($SGR - CGR$) to flag organic-rich facies and prevent clay volume overestimation.
3. Conducts a **Petrophysical Audit & Validation** against the well's pre-interpreted legacy $V_{sh}$ log, quantifying RMSE, bias, and correlation.
4. Evaluates the commercial impact of method selection on **Net Reservoir Thickness** and **Net-to-Gross (NTG)** using standard $V_{sh} \le 0.35$ cutoffs.

---

## 📂 Repository Structure

```
├── data/
│   └── oma1321.las                      # Raw open-hole LAS 2.0 wireline log file
├── Shale_Volume_Study.ipynb             # Fully executed Jupyter Notebook with 4-track plots
├── OMA1321_Shale_Volume_Evaluation.xlsx # Cleaned Excel workbook (Cleaned logs, Validation, NTG)
├── requirements.txt                     # Dependencies (lasio, pandas, numpy, matplotlib, openpyxl)
└── README.md                            # Technical documentation
```

---

## 🔬 Petrophysical Methods & Formulations

### 1. Robust Baseline Selection
To prevent single-point noise spikes or borehole rugosity from distorting baselines, percentile filtering was applied across 2,980 depth samples:
- **Clean Sand Baseline ($CGR_{clean}$):** $5^{\text{th}}$ percentile ($P_{05} = 31.8\text{ GAPI}$)
- **Pure Shale Baseline ($CGR_{shale}$):** $95^{\text{th}}$ percentile ($P_{95} = 118.5\text{ GAPI}$)

Linear Gamma Ray Index:
$$I_{GR} = \frac{CGR - CGR_{clean}}{CGR_{shale} - CGR_{clean}}$$

### 2. Evaluated Models

| # | Model | Mathematical Formulation | Geological Application |
|---|---|---|---|
| **1** | **Linear GR** | $V_{sh} = I_{GR}$ | Baseline; historically known to overestimate clay volume |
| **2** | **Larionov Tertiary (1969)** | $V_{sh} = 0.083 \times \left(2^{3.7 \times I_{GR}} - 1\right)$ | Neogene / young unconsolidated clastics |
| **3** | **Larionov Older (1969)** | $V_{sh} = 0.33 \times \left(2^{2.0 \times I_{GR}} - 1\right)$ | Mesozoic & Paleozoic consolidated formations |
| **4** | **Stieber (1970)** | $V_{sh} = \frac{I_{GR}}{3.0 - 2.0 \times I_{GR}}$ | Clastic benchmark for Middle East reservoirs |
| **5** | **Density-Neutron** | $V_{sh-ND} = \frac{\phi_N - \phi_D}{\phi_{N,sh} - \phi_{D,sh}}$ | Independent of GR; effective during tool failure or casing |

---

## 📊 Benchmark & Validation Results (vs Stored LAS VSH)

Evaluating each calculated model against the historical legacy interpretation:

| Method Name | RMSE | Mean Bias | Correlation ($R$) | Within $\pm 0.05$ | Within $\pm 0.10$ | Status |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **1. Linear GR** | 0.3563 | +0.1834 | 0.942 | 34.2% | 51.8% | Overestimates |
| **2. Larionov Tertiary** | **0.1878** | **-0.0381** | **0.957** | **68.4%** | **84.9%** | **Best Match** |
| **3. Larionov Older** | 0.2854 | +0.0921 | 0.954 | 45.1% | 67.3% | Moderate Error |
| **4. Stieber (1970)** | 0.2312 | -0.0124 | 0.958 | 61.2% | 79.5% | Close Match |
| **5. Density-Neutron** | 0.3204 | +0.1245 | 0.812 | 38.6% | 55.4% | GR-Independent |

> **Key Finding:** **Larionov Tertiary** achieved the highest accuracy ($RMSE = 0.1878$), confirming that the Omar clastic formation behaves as a young, unconsolidated system where linear models severely overestimate shale.

---

## 💰 Commercial Impact: Net Reservoir & Net-to-Gross (NTG)
Applying an industry-standard cutoff of **$V_{sh} \le 0.35$** across the 454-meter logged interval:

- **Larionov Tertiary:** **312.0 m** Net Reservoir (**68.7% NTG**)
- **Linear Model:** **246.0 m** Net Reservoir (**54.3% NTG**)

👉 **Takeaway:** Using the uncalibrated Linear method falsely discards **66 meters of potential reservoir sand**, significantly underestimating the commercial value of the asset.

---

## 🚀 Quickstart Guide

### Clone Repository & Install Dependencies:
```bash
git clone https://github.com/<your-username>/wireline-vsh-study.git
cd wireline-vsh-study
pip install -r requirements.txt
```

### Run Jupyter Notebook:
```bash
jupyter notebook Shale_Volume_Study.ipynb
```
