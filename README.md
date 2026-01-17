# CSE AI 382 Lab 2.4 – ETL Reproducibility Notebook

This repository contains a small ETL project for CSE AI 382, focused on reproducible data processing, logging, and metrics computation using Pandas in Databricks. The purpose of this repo is to provide a clean, reproducible workflow for transforming and analyzing small restaurant datasets.

---

## Repository Structure

```
├── notebooks/        # Databricks or Jupyter notebooks for ETL tasks
├── sql/              # Independent SQL transformation files (optional)
├── etl_pipeline/     # Python scripts or workflow automation files
├── data_samples/     # Example datasets for testing
└── README.md         # Project overview and instructions
```

Folder Purpose:

* notebooks/ → Contains the ETL notebook `lab_2_4_repro_logging` which:
  * Loads and cleans datasets
  * Performs joins and data transformations
  * Computes metrics like top items, revenue by category, and busiest hour
  * Logs runtime info and saves output metrics
* sql/ → For SQL queries or transformations (not used in this lab but included for future use)
* etl_pipeline/ → Python scripts for automation or reproducible workflows
* data_samples/ → Contains small example CSV files:
  * `menu_items.csv`
  * `order_details.csv`
* README.md → This file, describing the project, folder structure, and how to run the notebook

---

## Project Purpose

The purpose of this repository is to:
1. Demonstrate a reproducible ETL workflow in Databricks.
2. Show proper logging for analysis tracking and debugging.
3. Ensure reproducibility using random seed fixes and SHA-256 hashes of input files.
4. Produce metrics for a small restaurant dataset:
   * Top 5 items by quantity
   * Revenue by category
   * Busiest hour of the day

This project prepares you for larger datasets and more complex ETL pipelines in future labs.

---

## How to Run (Databricks)

1. Log in to Databricks.
2. Go to Repos → Add Repo and connect your GitHub account.
3. Open the repository:
   ```
   csai382_lab_2_4_-GustavoC-
   ```
4. Make sure the repository is on the correct branch (e.g., `feat/etl-notebook`).
5. Verify the input datasets are present in:
   ```
   data_samples/
   ├── menu_items.csv
   └── order_details.csv
   ```
   Python paths for the notebook:
   ```
   /Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/menu_items.csv
   /Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/order_details.csv
   ```
6. Open the notebook:
   ```
   notebooks/lab_2_4_repro_logging
   ```
7. Attach an AWS Serverless Interactive Cluster (or instructor-approved cluster).
8. Run all cells in order. The notebook will:
   * Configure logging (console + file)
   * Fix random seeds for reproducibility
   * Compute SHA-256 hashes for input CSVs
   * Load and clean the datasets
   * Perform ETL transformations with Pandas
   * Compute metrics:
     * Top 5 items by quantity
     * Revenue by category
     * Busiest hour
   * Save logs and ETL outputs

---

## Outputs and Logs

* Logs are saved to:
  ```
  logs/run_<YYYYMMDD_HHMM>.log
  ```
* ETL metrics and cleaned/joined outputs are saved to:
  ```
  /Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/etl_output/
  ```
  - menu_items_loaded.csv
  - order_details_loaded.csv
  - etl_df_cleaned_joined.csv
  - metrics_<timestamp>.csv
* Inline tables are displayed in the notebook for verification.

---

## Reproducibility Artifacts

The following files are generated or maintained for reproducibility:
* `requirements.txt`
* `data_hashes.json`
* Log files in `logs/`
* ETL outputs in `etl_output/`
* This `README.md` and `RUN.md`

---

## Notebook Code Highlights

### 1️⃣ Logging Setup

```python
import logging
import os
from datetime import datetime
# ... (see notebook for full code)
```

### 2️⃣ Reproducibility & Data Hashes

```python
import os, random, numpy as np
import subprocess
import hashlib
import json
# ... (see notebook for full code)
```

### 3️⃣ Load CSVs with Pandas

```python
import pandas as pd
menu = pd.read_csv('/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/menu_items.csv')
orders = pd.read_csv('/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/order_details.csv')
# ... (see notebook for full code)
```

### 4️⃣ Clean, Join, and Save Data

```python
orders['order_date'] = pd.to_datetime(orders['order_date'], errors='coerce')
orders['order_time'] = orders['order_time'].str.strip()
if 'item_id' in orders.columns:
    orders['item_id'] = orders['item_id'].astype('Int64')
menu['item_name'] = menu['item_name'].str.strip()
menu['category'] = menu['category'].str.strip()
etl_df = orders.merge(menu, left_on='item_id', right_on='menu_item_id', how='left')
etl_df = etl_df[['order_id', 'order_date', 'order_time', 'item_name', 'category', 'price']]
menu.to_csv(f'{output_dir}/menu_items_loaded.csv', index=False)
orders.to_csv(f'{output_dir}/order_details_loaded.csv', index=False)
etl_df.to_csv(f'{output_dir}/etl_df_cleaned_joined.csv', index=False)
```

---

## Ethical Reflection

Certain types of information should never be logged, such as personal customer details and passwords. For example, logging a customer's address or credit card number can expose sensitive data to unauthorized access, while logging passwords can lead to serious security breaches. Reproducibility supports accountability and fairness by allowing others to verify analyses and confirm that results are consistent. This is especially important when models or decisions affect people, as reproducible workflows help reduce errors and bias, building trust in data-driven projects.
