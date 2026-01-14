# csai-382-repos-and-notebooks
CSAI-382 starting template for the 2.3 assignment GitHub Repository &amp; Notebooks

## Project Description
This repository serves as a starter template for CSAI-382 students working on data science projects. It includes a standard folder structure and configuration files to help organize your work effectively.

## Folder Structure
```
csai-382-repos-and-notebooks/
├── notebooks/          # Jupyter notebooks for analysis and exploration
├── sql/               # SQL scripts and queries
├── etl_pipeline/      # ETL (Extract, Transform, Load) pipeline scripts
├── data_samples/      # Sample datasets for testing and development
├── .gitignore         # Git ignore rules for Python, Jupyter, VSCode, and OS files
└── README.md          # This file
```

## How to Use
1. Clone this repository to your local machine
2. Place your Jupyter notebooks in the `notebooks/` folder
3. Store SQL scripts in the `sql/` folder
4. Add ETL pipeline code to the `etl_pipeline/` folder
5. Put sample data files in the `data_samples/` folder
6. Update this README with project-specific information

## Notes
- The `.gitignore` file is configured to exclude common Python, Jupyter, VSCode, and OS-specific files
- Each folder contains a `.keep` file to ensure the folder structure is preserved in Git
- Customize this template to fit your specific project needs

## Project overview
This repo is a lightweight starter for the CSAI-382 2.3 assignment, focused on running notebooks both locally and in Databricks. It includes a minimal project structure so you can practice committing code, tracking experiments, and producing reproducible artifacts. Use it as the base for exploring version control, data logging, and collaboration workflows.

## How to run

### Local
1. Install Python 3.10+ and create a virtual environment.
2. Install dependencies (for example, `pip install -r requirements.txt` if you add one for your work).
3. Launch Jupyter or VS Code and open the notebooks in this repo. Ensure any data files referenced in your notebooks are stored in a tracked `data/` subfolder or an external path you note in the notebook.

### Databricks
1. Import this repository into your Databricks workspace (or use Repos to sync from GitHub).
2. Attach a cluster with the runtime your assignment specifies (or latest ML runtime).
3. Run notebooks from the Workspace UI. Persist outputs to DBFS paths (e.g., `/dbfs/FileStore/your-project/...`) and keep any table creations or job configs in version control where possible.

## Artifacts and logging
- Store raw data, intermediate datasets, and generated figures under a dedicated `artifacts/` directory (or a cloud bucket/DBFS path you reference in notebooks).
- Record model checkpoints or metrics exports with clear filenames and dates; prefer open formats like CSV/JSON for portability.
- Keep a short `RUN.md` or notebook cell pointing to the exact artifact locations so others can find and reproduce results.

## Reproducibility and run steps
Follow the assignment-provided RUN steps (see the course materials link you add to `RUN.md`) whenever you execute or re-execute experiments. Document package versions, cluster/runtime configs, and parameter choices alongside your commits so teammates can replay the same workflow.

## Ethics reflection
Avoid logging personally identifiable information (e.g., names, emails) and protected attributes (e.g., race, gender) unless you have explicit approval and safeguards. Excluding these fields from logs reduces privacy risks and mitigates biased downstream use. Thorough, reproducible experiment records—covering data versions, code commits, and parameters—enable accountability reviews and fairness audits by making decisions traceable.

## AI usage guidance
You may consult AI tools for drafting code or text, but you are responsible for verifying correctness, citing any AI-assisted sections per assignment rules, and ensuring the final work meets course academic integrity policies.
