# PhonePe Pulse EDA

An exploratory data analysis of India's PhonePe Pulse open dataset — tracking how the country's digital payments behavior evolved quarter by quarter across states and districts from 2018 to 2024. The project turns seven years of aggregated transaction data into a clean narrative about where digital payments grew, how the category mix shifted toward merchant-first flows, and which district-quarters stand out as anomalies worth modeling next.

👉 **Start here: [`main_notebook.ipynb`](./main_notebook.ipynb)**

🎥 **Project video:** https://www.youtube.com/watch?v=Z0OK7tT9yGA

## Research questions

1. How does transaction **volume and value** evolve quarter by quarter at the national, state, and district level?
2. How does the **payment category mix** (P2P vs. merchant, recharge, financial services, etc.) change over time?
3. Once we normalize by registered-user counts, how does **per-user payment intensity** differ across states and districts?
4. Which district-quarters register as **anomalies** relative to their own history, and what do they suggest for downstream modeling?

## Data

- **Source:** [PhonePe Pulse](https://github.com/PhonePe/pulse) — PhonePe's open aggregated transactions dataset (JSON files organized by `aggregated/`, `map/`, `top/` → `transaction/user/insurance`).
- **Coverage analyzed:** 2018Q1 – 2024Q4.
- **Access:** clone the upstream repo into `./pulse/` (see reproduction steps below). The dataset is **not committed** to this repo — it's a separate upstream repository and is gitignored.
- **Preprocessing:** all JSON → tidy `pandas` DataFrame flattening happens inside `main_notebook.ipynb` (no separate ETL script needed). State-level and district-level tables are joined with registered-user counts for normalization; a robust-z-score baseline is computed per district time series.

## How to reproduce

This project runs both locally and on Google Colab. Notebooks should be executed top-to-bottom.

**1. Get the code and dataset**

```bash
git clone https://github.com/ShashankC10/phonePePulseEDA.git
cd phonePePulseEDA
git clone https://github.com/PhonePe/pulse.git pulse     # dataset (separate repo)
test -d pulse/data && echo "Dataset ready at pulse/data"
```

**2. Set up the environment**

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**3. Run the notebooks in this order**

1. `main_notebook.ipynb` — the curated, graded deliverable (read this one first).
2. `checkpoints/checkpoint_1.ipynb` — initial exploratory pass.
3. `checkpoints/checkpoint_2.ipynb` — research-question-driven follow-up.

```bash
jupyter notebook main_notebook.ipynb
# or, for a non-interactive validation run:
jupyter nbconvert --to notebook --execute main_notebook.ipynb --inplace
```

## Key dependencies

Developed on **Python 3.13** locally and also compatible with Colab's default Python 3.11. The full list lives in [`requirements.txt`](./requirements.txt); the headline packages are:

| Package | Version |
|---|---|
| pandas | 2.2.3 |
| numpy | 2.1.3 |
| matplotlib | 3.10.0 |
| scikit-learn | 1.6.1 |
| ruptures | 1.1.10 |
| jupyter | 1.1.1 |

## Repo structure

```
phonePePulseEDA/
├── main_notebook.ipynb        👈 final curated deliverable (start here)
├── checkpoints/
│   ├── checkpoint_1.ipynb     initial EDA checkpoint
│   └── checkpoint_2.ipynb     research-question checkpoint
├── requirements.txt           pinned Python dependencies
├── README.md                  this file
├── .gitignore
└── pulse/                     PhonePe Pulse dataset (cloned separately; gitignored)
    └── data/
        ├── aggregated/
        ├── map/
        └── top/
```

## Results summary

- **Time coverage:** 2018Q1 → 2024Q4 analyzed end-to-end across seven years and four dataset hierarchies.
- **Category mix:** merchant payments' share of total transactions rose materially over the window, reshaping the volume/value mix away from P2P-dominant behavior.
- **Geographic concentration:** a small group of top states continues to account for a disproportionate share of total transaction value; national growth is not geographically uniform.
- **Per-user intensity:** district-level amount-per-user distributions are highly right-skewed — the median district is very different from the heavyweights.
- **Anomalies:** a robust-z-score baseline on district-quarter normalized metrics surfaces a non-trivial set of outlier district-quarters, which are the natural seed for anomaly-detection and change-point modeling in future work.

Full analysis, figures, and interpretation live in [`main_notebook.ipynb`](./main_notebook.ipynb).