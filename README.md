# PhonePe Pulse EDA Project

## 1. Problem Statement and Motivation
India's digital payments ecosystem has grown rapidly, but adoption and transaction intensity vary across regions and over time.  
This project analyzes the PhonePe Pulse dataset to understand:
- how transaction volume/value evolves quarter by quarter,
- how payment category mix changes over time,
- how activity differs across states and districts after user normalization.

Motivation:
- build a rigorous, portfolio-ready data mining workflow,
- ground later modeling decisions in evidence from exploratory analysis,
- surface actionable hypotheses for anomaly detection and forecasting.

## 2. Dataset Choice Rationale (A/B/C Summary)
Three candidates were considered:
1. Instacart Online Grocery Shopping Dataset 2017
2. PhonePe Pulse
3. BTS (KDD 2025 tie-strength benchmark)

Final selected dataset in this repository: **PhonePe Pulse**.

Why PhonePe Pulse was selected:
- aligns with course techniques: clustering-ready feature engineering, anomaly-oriented analysis, trend mining;
- supports beyond-course methods: change-point detection and hierarchical/spatiotemporal forecasting;
- matches implemented notebook sections A-F end-to-end (coherent submission narrative).

Trade-offs:
- aggregated data (no user-level event sequence reconstruction),
- no direct market-basket frequent itemset setting like Instacart.

## 3. Reproducibility Steps
### Prerequisites
- Python 3.10+ recommended
- Git installed

### Add dataset (required)
From the project root (`/Users/shashank/Documents/phonePePulseEDA`), clone the PhonePe Pulse dataset into a folder named `pulse`:
```bash
# HTTPS
git clone https://github.com/PhonePe/pulse.git pulse

# or SSH
git clone git@github.com:PhonePe/pulse.git pulse
```

Expected location after clone:
- `/Users/shashank/Documents/phonePePulseEDA/pulse/data/`

Quick check:
```bash
test -d pulse/data && echo "Dataset ready at pulse/data"
```

### Setup environment
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Run notebook
```bash
jupyter notebook eda.ipynb
```
Then run all cells from top to bottom.

Optional validation run:
```bash
jupyter nbconvert --to notebook --execute eda.ipynb --inplace
```

## 4. Key EDA Findings and Next-Step Modeling Plan
### Key findings
- Time coverage analyzed: **2018Q1 to 2024Q4**.
- Merchant payment share increased substantially over the period.
- State-level concentration remains meaningful (top states account for a large share of total amount).
- District-level amount-per-user distribution is highly skewed.
- Robust z-score baseline identifies a non-trivial set of district-quarter anomalies.

### Next-step modeling plan
1. anomaly detection on district-quarter normalized metrics,
2. clustering of district/state behavioral profiles.

Beyond-course:
1. change-point detection on state-level quarterly trajectories,
2. hierarchical/spatiotemporal forecasting (state -> district).

Deliverables for next checkpoint:
- modeling notebook with justification for each algorithmic choice,
- evaluation tables and error analysis,
- interpretation of regional heterogeneity and bias implications.
