# Spatial Accident Clustering & OLAP Analysis

> Density-based spatial clustering and multi-dimensional OLAP analysis of 7.7M+ US traffic accidents (2016–2023) to surface high-risk accident hotspots, validate them statistically, and benchmark the approach against simpler baselines.

**Course:** Data Mining — CSCI 5455, University of Colorado Denver

**Team:** Shreesh Gurjar · Prasad Belsare

---

## Overview

This project analyzes the [US Accidents (2016–2023)](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents) dataset to answer a practical question: **where are the real accident hotspots in the US, and what makes each one dangerous?**

Rather than relying on fixed map grids, it uses **DBSCAN** to discover dense accident zones of arbitrary shape, profiles each hotspot against national averages, validates the clusters with standard internal metrics, and compares the results against both a naive grid baseline and K-Means to justify the modeling choices.

---

## Key Features

- **Robust preprocessing pipeline** — deduplication, timestamp parsing, accident-duration calculation, temporal feature engineering (hour, day, month, season), and median/mode imputation for missing values.
- **Multi-dimensional OLAP analysis** — three pivot-table "cubes" slicing accident counts, durations, and visibility across State, County, Season, Severity, and Time of Day.
- **Density-based spatial clustering** — DBSCAN with the Haversine metric on geographic coordinates to detect hotspots without pre-specifying cluster count, automatically separating noise from genuine dense zones.
- **Automated parameter tuning** — sensitivity analysis across `eps` and `min_samples` combinations, selecting the optimal pair by minimizing noise while preserving cluster density.
- **Cluster profiling** — each hotspot is characterized by accident count, average severity/duration vs. the global mean, dominant city and weather, and an auto-assigned distinguishing trait (High Severity, Long Delays, Snow Prone).
- **Quantitative validation** — Silhouette Score and Davies-Bouldin Index with interpretive gauge charts confirm cluster quality.
- **Baseline benchmarking** — a 0.5° grid-based hotspot detector and a K-Means run are compared against DBSCAN to demonstrate why density-based clustering wins on this data.
- **Interactive geospatial map** — a Folium map with `FastMarkerCluster` renders all clustered accident points across the country.

---

## Tech Stack

| Tool | Purpose |
| --- | --- |
| Python 3.10+ | Core language |
| pandas / NumPy | Data manipulation and numerical operations |
| scikit-learn | DBSCAN, K-Means, validation metrics |
| SciPy | Z-score outlier detection |
| Matplotlib / Seaborn | Static analytical visualizations |
| Folium | Interactive geospatial mapping |

---

## Methodology

1. **Load & preprocess** the dataset — clean, parse timestamps, engineer temporal features, impute missing values.
2. **OLAP analysis** — build three multi-dimensional cubes to explore accident patterns across space, time, and severity.
3. **Parameter sensitivity sweep** — test `eps_km` ∈ {2, 5, 10} × `min_samples` ∈ {10, 30, 50}, visualize via heatmap, and auto-select the best combination.
4. **DBSCAN clustering** — run density-based clustering on geographic coordinates using the Haversine metric.
5. **Profile clusters** — rank the top 10 hotspots and tag each with its distinguishing trait.
6. **Validate** — compute Silhouette and Davies-Bouldin scores with interpretive gauges.
7. **Benchmark** — compare against grid-based and K-Means baselines.
8. **Visualize** — generate heatmaps, severity bar charts, and an interactive Folium hotspot map.

---

## Why DBSCAN over K-Means?

- No need to specify the number of clusters upfront — unknown for geographic data.
- Discovers clusters of arbitrary shape, essential for real-world hotspot boundaries.
- Automatically labels isolated points as noise instead of forcing them into a cluster.

Silhouette scores for both methods are computed and compared to confirm this choice quantitatively.

---

## Getting Started

### Prerequisites

```bash
pip install -r requirements.txt
```

### Dataset

Download the [US Accidents (2016–2023)](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents) dataset from Kaggle and place the CSV in the project root. The notebook expects a sampled file:

```
US_Accidents_March23_sampled_500k.csv
```

Update `DATASET_PATH` in the notebook if your filename differs.

### Run

Open the notebook and run all cells:

```bash
jupyter notebook spatial_accident_analysis_Code.ipynb
```

Or upload it to [Google Colab](https://colab.research.google.com/) and run there — no local setup required.

---

## Project Structure

```
.
├── spatial_accident_analysis_Code.ipynb   # Main analysis notebook
├── requirements.txt                       # Python dependencies
├── data/                                  # Dataset (not tracked)
└── README.md
```

---

## Results

- DBSCAN surfaced well-separated, high-density accident clusters validated by Silhouette and Davies-Bouldin scores.
- Cluster profiling revealed distinct hotspot types — high-severity zones, long-delay corridors, and snow-prone regions.
- Density-based clustering produced more meaningful, noise-aware hotspots than the fixed-grid baseline and K-Means.

---

## Limitations & Future Work

- Clustering is computed on a sampled subset (capped at 50k points) for performance; full-dataset clustering would require distributed processing.
- Hotspot detection is purely spatial — incorporating temporal density (space-time clustering) could surface time-specific risk zones.
- Severity and weather are profiled descriptively; a predictive model could forecast hotspot risk from these features.

---

## License

Released for academic purposes as part of CSCI 5455 at the University of Colorado Denver.
