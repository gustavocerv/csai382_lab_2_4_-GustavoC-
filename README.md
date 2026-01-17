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

1. Clone the Repository in Databricks

   * Go to Repos → Add Repo and connect your GitHub account.
   * Clone this repository into your Databricks workspace.

2. Open the ETL Notebook

   * Navigate to `notebooks/lab_2_4_repro_logging`.
   * Attach an interactive cluster.

3. Run All Cells

   * The notebook will automatically:

     * Configure logging
     * Fix random seeds for reproducibility
     * Compute SHA-256 hashes for input CSVs
     * Load and clean the datasets
     * Perform ETL transformations
     * Display initial data and metrics
     * Save metrics and log files

---

## Notebook Code Highlights

### 1️⃣ Logging Setup

```python
import logging
import os
from datetime import datetime

# Create logs directory if it doesn't exist
os.makedirs('logs', exist_ok=True)

# Set up log file name with current date and time
now = datetime.now()
log_filename = f"logs/run_{now.strftime('%Y%m%d_%H%M')}.log"

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s | %(levelname)s | %(message)s',
    handlers=[
        logging.StreamHandler(),
        logging.FileHandler(log_filename)
    ]
)

# Log start of run
logging.info("Run started.")
logging.info(f"Cluster/runtime info: AWS Serverless interactive cluster (terminated by inactivity)")

# Paths to GitHub-connected Databricks repo
logging.info(f"menu file path: /Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/menu_items.csv")
logging.info(f"orders file path: /Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/order_details.csv")
logging.info(f"Log file: {log_filename}")
```

---

### 2️⃣ Reproducibility & Data Hashes

```python
import os, random, numpy as np
import subprocess
import hashlib
import json

# Fix random seeds
os.environ['PYTHONHASHSEED'] = '0'
random.seed(0)
np.random.seed(0)
logging.info("Random seeds set to 0")

# Capture environment
!pip freeze > requirements.txt
logging.info("Saved environment packages to requirements.txt")

# Compute SHA-256 hashes for input CSVs
def compute_sha256(file_path):
    sha256_hash = hashlib.sha256()
    with open(file_path, "rb") as f:
        for byte_block in iter(lambda: f.read(4096), b""):
            sha256_hash.update(byte_block)
    return sha256_hash.hexdigest()

data_files = [
    "/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/menu_items.csv",
    "/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/order_details.csv"
]

hashes = {}
for f in data_files:
    if os.path.exists(f):
        hashes[f] = compute_sha256(f)
    else:
        logging.warning(f"File not found: {f}. Skipping hash computation.")

with open("data_hashes.json", "w") as f:
    json.dump(hashes, f, indent=2)

logging.info(f"Data hashes saved to data_hashes.json: {hashes}")
```

---

### 3️⃣ Load CSVs with Pandas

```python
import pandas as pd

menu = pd.read_csv('/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/menu_items.csv')
orders = pd.read_csv('/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/data_samples/order_details.csv')

print('menu_items shape:', menu.shape)
print('order_details shape:', orders.shape)
display(menu.head())
display(orders.head())
```

---

## Outputs and Logs

* Logs: `logs/run_<YYYYMMDD_HHMM>.log`
* Metrics output: `/FileStore/tables/etl_output/metrics_<timestamp>.csv`
* Inline tables displayed in notebook for verification


---


## Clean basic issues

orders['order_date'] = pd.to_datetime(orders['order_date'], errors='coerce')
orders['order_time'] = orders['order_time'].str.strip()
if 'item_id' in orders.columns:
    orders['item_id'] = orders['item_id'].astype('Int64')  # Use nullable integer type to avoid IntCastingNaNError
menu['item_name'] = menu['item_name'].str.strip()
menu['category'] = menu['category'].str.strip()

# Join on menu_items.menu_item_id = order_details.item_id
etl_df = orders.merge(menu, left_on='item_id', right_on='menu_item_id', how='left')

# Create tidy table with useful columns
etl_df = etl_df[['order_id', 'order_date', 'order_time', 'item_name', 'category', 'price']]
display(etl_df.head())

# Save cleaned and joined outputs
import os
output_dir = '/Workspace/Users/gsc314@ensign.edu/csai382_lab_2_4_-GustavoC-/etl_output'
os.makedirs(output_dir, exist_ok=True)
menu.to_csv(f'{output_dir}/menu_items_loaded.csv', index=False)
orders.to_csv(f'{output_dir}/order_details_loaded.csv', index=False)
etl_df.to_csv(f'{output_dir}/etl_df_cleaned_joined.csv', index=False)
print('Saved menu_items, order_details, and cleaned/joined etl_df to etl_output directory.')

---

## Ethical Reflection

Certain types of information should never be logged, such as personal customer details and passwords. For example, logging a customer's address or credit card number can expose sensitive data to unauthorized access, while logging passwords can lead to serious security breaches. Reproducibility supports accountability and fairness by allowing others to verify analyses and confirm that results are consistent. This is especially important when models or decisions affect people, as reproducible workflows help reduce errors and bias, building trust in data driven projects.