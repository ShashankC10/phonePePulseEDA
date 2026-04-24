# PhonePe Pulse EDA: Archetypes and Defectors

This project analyzes India’s PhonePe Pulse dataset from **2018Q1 to 2024Q4** to study whether India’s digital payments growth is one uniform national trend or a collection of distinct district-level behavioral patterns.

Instead of only ranking states or districts by raw transaction volume, this project normalizes district behavior by registered users, groups districts into interpretable digital-payment archetypes, and then identifies “defectors”: districts that behave unusually compared with their own peer group.

**Start here:** [`main_notebook.ipynb`](./main_notebook.ipynb)  
**Project video:** https://www.youtube.com/watch?v=Z0OK7tT9yGA

---

## Project overview

India’s digital payments ecosystem grew rapidly between 2018 and 2024. However, national-level growth can hide important local differences. A district with high total transaction volume may simply have a larger user base, while a smaller district may show unusually high engagement once normalized by registered users.

This project asks a more behavioral question:

> Are Indian districts becoming similar in digital-payment behavior, or do they form distinct payment archetypes with unusual outliers inside each group?

The final analysis focuses on district-level normalized payment behavior, clustering, tier movement, and cluster-conditioned anomaly detection.

---

## Research questions

1. Do Indian districts fall into interpretable digital-payment behavioral archetypes after normalizing by registered users?
2. How stable are those archetypes between **2019Q4** and **2024Q4**?
3. Which districts persistently behave differently from their own archetype peers?
4. Does cluster-conditioned anomaly detection reveal more meaningful outliers than a single global national baseline?

---

## Dataset

This project uses the public **PhonePe Pulse** dataset.

Dataset source:  
https://github.com/PhonePe/pulse

The dataset contains aggregated digital payment statistics across India, including:

- transaction counts,
- transaction amounts,
- registered users,
- app opens,
- state-level and district-level breakdowns,
- quarterly data from 2018 onward.

For this project, I focus mainly on district-level behavior from **2018Q1 to 2024Q4**.

The raw dataset is not committed to this repository. It should be cloned separately into a local `pulse/` folder.

---

## Data preprocessing

The PhonePe Pulse dataset is organized as nested JSON files. The analysis extracts and reshapes those JSON files into clean, analysis-ready tables.

The main preprocessing steps are:

1. Clone or download the official PhonePe Pulse dataset.
2. Parse state-level and district-level JSON files.
3. Standardize state and district names.
4. Combine quarterly records into a single longitudinal dataset.
5. Join transaction, user, and app-open information.
6. Create normalized district-level features, including:
   - `txns_per_user`
   - `amount_per_user`
   - `opens_per_user`
7. Prepare district-quarter records for clustering and anomaly detection.

All preprocessing and analysis steps are included in `main_notebook.ipynb`.

---

## Methodology

The project is organized into two connected phases.

### Phase 1: Finding district archetypes

The first phase asks whether Indian districts can be grouped into a small number of interpretable digital-payment archetypes.

To do this, the notebook uses K-Means clustering on per-user normalized features. The analysis focuses on normalized behavior rather than raw scale, because raw transaction volume tends to favor large districts.

The main features used for behavioral comparison are:

- transactions per registered user,
- transaction amount per registered user,
- app opens per registered user.

The notebook compares district assignments between **2019Q4** and **2024Q4** to understand whether districts remain in the same behavioral tier or move across tiers over time.

### Phase 2: Finding defectors within archetypes

After forming behavioral peer groups, the second phase identifies districts that behave unusually relative to their own archetype.

This matters because a district may not look anomalous nationally, but it may be highly unusual when compared with similar districts.

The notebook compares global anomaly detection with cluster-conditioned anomaly detection using:

- robust z-score,
- Isolation Forest,
- Local Outlier Factor.

The key idea is that anomaly detection becomes more meaningful when districts are compared against their own peer group rather than against the entire country.

---

## Key findings

The main findings are:

1. **India’s digital payment growth is not uniform.**  
   Districts fall into distinct behavioral archetypes after normalizing by registered users.

2. **Per-user normalization changes the interpretation.**  
   Some high-volume districts look less unusual after accounting for their user base, while some smaller districts become more interesting once normalized.

3. **The district archetype structure is relatively stable, but district membership changes.**  
   The analysis finds a stable structure of three broad behavioral tiers, but many districts move between tiers from 2019Q4 to 2024Q4.

4. **Cluster-conditioned anomaly detection is more informative than a single global baseline.**  
   Running anomaly detection within each behavioral tier reveals districts that would be missed or misinterpreted by a global anomaly model.

5. **The most useful comparison is district-to-peer, not district-to-country.**  
   A district’s payment behavior is easier to interpret when compared with similar districts rather than with all districts nationally.

---

## Repository structure

```text
phonePePulseEDA/
├── README.md
├── main_notebook.ipynb
├── requirements.txt
├── .gitignore
├── checkpoints/
│   ├── checkpoint_1.ipynb
│   └── checkpoint_2.ipynb
└── pulse/
    └── data/
        ├── aggregated/
        ├── map/
        └── top/