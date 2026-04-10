# Advanced Coding Project — Madrid Air Quality

**Group:** Future data scientists  
**Members:** Giulia Castelli (300691), Anna Granzotto (300051), Maria Maggiora (297281)  
**Course:** Advanced Coding for Data Analytics

---

## Project Overview

This project analyses the [METRAQ](https://huggingface.co/datasets/dmariaa70/METRAQ-Air-Quality) Madrid air quality dataset, covering measurements from 24 monitoring stations across the city. The analysis spans 10 tasks including data loading, quality assessment, imputation, temporal and spatial analysis, network modelling, parallelization, and forecasting.

The notebook is: `madrid_air_quality_complete.ipynb`

---

## Dataset

Two options are available — switch between them in the **first code cell** (cell under "Load Data"):

| Option | Description | How to activate |
|--------|-------------|-----------------|
| **Sample (default)** | `data/sample_dataset.csv` — a smaller local CSV for development and testing | Already active (default) |
| **Full dataset** | ~64M rows from HuggingFace (`dmariaa70/METRAQ-Air-Quality`) | Uncomment Option B in the Load Data cell |

> **Note:** All results and outputs shown in the notebook were produced using the sample dataset. The code is designed to run identically on the full dataset.

---

## Repository Structure

```
project/
├── madrid_air_quality_complete.ipynb   # Main analysis notebook
├── requirements.txt                    # Python dependencies
├── README.md                           # This file
├── project.pdf                         # Assignment instructions
└── data/
    ├── sample_dataset.csv              # Sample data for development
    └── Madrid air quality full dataset # Full dataset (local copy, optional)
```

---

## Setup and Installation

### 1. Create a virtual environment (recommended)

```bash
python3 -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Launch the notebook

```bash
jupyter notebook madrid_air_quality_complete.ipynb
```

---

## Running the Notebook

Run all cells top-to-bottom in order. The notebook is self-contained — each task builds on the dataframe and variables created in previous tasks.

**Key variables created early and reused throughout:**

| Variable | Description |
|----------|-------------|
| `df` | Full working dataframe |
| `df_pollutants` | Subset filtered to pollutant variables |
| `df_weather` | Subset filtered to weather variables |
| `df_traffic` | Subset filtered to traffic variables |
| `monthly_sensor` | Monthly aggregated data per sensor |
| `knn_graphs` | Dictionary of spatial KNN graphs (k=2,3,4,5) |
| `corr_graphs` | Dictionary of correlation networks at various thresholds |

---

## Tasks Summary

| Task | Description |
|------|-------------|
| 1 | Load data, inspect structure, EDA, spatial map, time-series |
| 2 | Missingness analysis, temporal and sensor-specific gaps, data quality checks |
| 3 | Imputation: mean, median, KNN, regression — comparison via KDE + MAD + KS test |
| 4 | Temporal analysis: long-term trends, seasonality, cross-station stability |
| 5 | Spatial network: KNN and distance-threshold graphs, community detection |
| 6 | Correlation network: pairwise Pearson similarity, threshold sensitivity |
| 7 *(optional)* | Diffusion propagation model on spatial network |
| 8 | Parallelization: per-(year, sensor) correlation matrices, speedup benchmark |
| 9 *(optional)* | Forecasting NO2 from weather and traffic using Ridge, Random Forest, Gradient Boosting |
| 10 | Final visualization dashboard summarising all key findings |

---

## Notes on Parallelization (Task 8)

Each parallel worker **independently reads the data file** and loads only the rows for its assigned (year, sensor) pair — no shared in-memory state is passed between processes. This design scales to the full 64M-row dataset without memory bottlenecks.
