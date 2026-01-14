# How to Run the Notebooks

Follow these steps to reproduce the notebook analysis with the provided datasets.

## 1) Clone and set up the environment
1. Clone the repository and enter the folder:
   ```bash
   git clone <repo-url>
   cd csai-382-repos-and-notebooks
   ```
2. (Optional) Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate
   ```
3. Install any required Python packages listed by your instructor or notebook cells (e.g., via `pip install -r requirements.txt` if provided).

## 2) Data availability
- The Week 1 datasets are stored in the `data/` directory:
  - `data/menu_items.csv`
  - `data/order_details.csv`
- These files are read directly by the notebooks; no additional downloads are required.

## 3) Run the notebooks
1. Launch Jupyter Lab or Jupyter Notebook:
   ```bash
   jupyter lab
   ```
   or
   ```bash
   jupyter notebook
   ```
2. Open the relevant notebook(s) and run all cells in order.

## 4) Outputs and logs
- Notebook outputs (charts, tables, etc.) are saved alongside the notebook unless a cell writes them elsewhere.
- Runtime logs should be directed to the `logs/` directory. Create log files inside `logs/` when adding logging to notebooks or scripts (e.g., via Python's `logging` module).
- Configuration defaults can be stored in `config.yaml` (see below).

## 5) Optional configuration
- The `config.yaml` file is provided as a placeholder for any parameters you want to centralize (e.g., file paths, database URIs, or experiment settings).
- Update keys in `config.yaml` and import them in your notebooks/scripts as needed.
