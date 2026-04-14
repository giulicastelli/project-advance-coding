# Advanced Coding Project — Madrid Air Quality

**Group:** Future data scientists  
**Members:** Giulia Castelli (300691), Anna Granzotto (300051), Maria Maggiora (297281)  
**Course:** Advanced Coding for Data Analytics

---

## Project Overview

This project analyses the [METRAQ](https://huggingface.co/datasets/dmariaa70/METRAQ-Air-Quality) Madrid air quality dataset, covering measurements from 24 monitoring stations across the city. The analysis spans 10 tasks including data loading, quality assessment, imputation, temporal and spatial analysis, network modelling, parallelization, and forecasting.

The analysis is split across two notebooks — one per dataset version:

- `madrid_air_quality_Sample_Dataset.ipynb` — runs on the sample CSV (fast, for development)
- `madrid_air_quality_Full_Dataset.ipynb` — runs on the full ~64M row Parquet dataset

---

## Dataset

The project uses the [METRAQ](https://huggingface.co/datasets/dmariaa70/METRAQ-Air-Quality) Madrid air quality dataset. Two separate notebooks are provided, each pre-configured for a different data source:

| Notebook | Dataset | Description |
|----------|---------|-------------|
| `madrid_air_quality_Sample_Dataset.ipynb` | `data/sample_dataset.csv` | Smaller local CSV for development and testing |
| `madrid_air_quality_Full_Dataset.ipynb` | HuggingFace (`dmariaa70/METRAQ-Air-Quality`) | Full ~64M row dataset streamed directly from HuggingFace — no manual download needed |

> **Note:** All results and outputs shown in the notebooks were produced using the sample dataset. Both notebooks are identical in structure and code — only the data loading cell differs.
>
> **Note on the full dataset:** `madrid_air_quality_Full_Dataset.ipynb` loads the data with a single call to `pd.read_parquet("hf://datasets/dmariaa70/METRAQ-Air-Quality")`. This requires an internet connection and the `huggingface-hub` and `pyarrow` packages (both included in `requirements.txt`). The local `data/Madrid air quality full dataset/` folder is excluded from the repository via `.gitignore`.

---

## Repository Structure

```
project/
├── madrid_air_quality_Sample_Dataset.ipynb   # Notebook — sample CSV dataset
├── madrid_air_quality_Full_Dataset.ipynb     # Notebook — full dataset (streamed from HuggingFace)
├── requirements.txt                          # Python dependencies
├── .gitignore                                # Excludes large local data files
├── README.md                                 # This file
├── project.pdf                               # Assignment instructions
└── data/
    └── sample_dataset.csv                    # Sample data for development
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

### 3. Launch a notebook

```bash
# For development / fast runs:
jupyter notebook madrid_air_quality_Sample_Dataset.ipynb

# For full dataset runs:
jupyter notebook madrid_air_quality_Full_Dataset.ipynb
```

---

## Running the Notebooks

Run all cells top-to-bottom in order. Each notebook is self-contained — each task builds on the dataframe and variables created in previous tasks.

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
