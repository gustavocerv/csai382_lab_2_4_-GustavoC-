# How to Run (Databricks Notebook)

This project is designed to run **inside Databricks using Databricks Repos** with a GitHub-connected repository.

---

## 1) Open the Repository in Databricks

1. Log in to **Databricks**.
2. Go to **Repos → Add Repo** and connect your GitHub account.
3. Open the repository:

   ```
   csai382_lab_2_4_-CervantesG-
   ```
4. Make sure the repository is on the correct branch:

   ```
   feat/etl-notebook
   ```

---

## 2) Verify Data Files

The input datasets are stored in the repository:

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

No additional downloads are required.

---

## 3) Run the Notebook

1. Open the notebook:

   ```
   notebooks/lab_2_4_repro_logging
   ```
2. Attach an **AWS Serverless Interactive Cluster** (or instructor-approved cluster).
3. Run **all cells in order**.

The notebook will:

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

## 4) Outputs and Logs

* **Logs** are saved to:

  ```
  logs/run_<YYYYMMDD_HHMM>.log
  ```
* **ETL metrics output** is saved to:

  ```
  /FileStore/tables/etl_output/metrics_<timestamp>.csv
  ```
* Inline tables are displayed in the notebook for verification.

---

## 5) GitHub Workflow (Databricks Repos)

All version control actions are performed **inside Databricks Repos**.

1. Create a feature branch:

   ```
   feat/etl-notebook
   ```
2. Make small commits with clear messages, for example:

   * `feat: add logging setup`
   * `feat: add reproducibility and data hashing`
   * `feat: add pandas ETL and metrics`
3. Push the branch from Databricks.
4. Open a Pull Request (PR) to `main` in GitHub.
5. Include in the PR:

   * Summary of changes
   * Link to this `RUN.md`
   * Optional: screenshot of log output
6. Merge the PR when complete.

---

## 6) Reproducibility Artifacts

The following files are generated or maintained for reproducibility:

* `requirements.txt`
* `data_hashes.json`
* Log files in `logs/`
* This `RUN.md`

---
