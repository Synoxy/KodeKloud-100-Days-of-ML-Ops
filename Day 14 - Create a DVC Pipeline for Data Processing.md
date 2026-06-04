🚨 **Problem:**

The xFusionCorp Industries ML team uses DVC pipelines to keep data processing reproducible. A draft dvc.yaml exists in the fraud-detection project, but dvc repro does not complete the full pipeline. Correct the pipeline definition so it runs cleanly end to end.

- A project exists at /root/code/fraud-detection/ with DVC initialised. Python scripts are at src/data/process_data.py and src/data/split_data.py; raw input is at data/raw/transactions.csv. Do not modify the Python files or the input data.

- The corrected pipeline must declare two stages with the following behaviour:
  - process_data – Depends on data/raw/transactions.csv and src/data/process_data.py; produces data/processed/clean_transactions.csv.
  - split_data – Depends on data/processed/clean_transactions.csv and src/data/split_data.py; produces data/processed/train.csv and data/processed/test.csv.
- Review the existing dvc.yaml and correct everything that prevents dvc repro from completing.

- After your changes, dvc repro must run end to end and dvc status must report no stale stages.

Once the pipeline is valid, the DVC extension's PIPELINES section under the DVC view will list both stages and visualise the dependency graph between them.

🛠️ **Solution:**

1. Open dvc.yaml and make below changes:

```text   
stages:
  process_data:
    cmd: python src/data/process_data.py **#Update location**
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv **#Update output**

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - src/data/split_data.py
      - data/processed/clean_transactions.csv **#Add** 
    outs:
      - data/processed/train.csv
      - data/processed/test.csv
```
2. Run **dvc repro** followed by **dvc status**, the pipeline will be be built and you will see below output.

<img width="621" height="490" alt="image" src="https://github.com/user-attachments/assets/e50f37ad-d414-4025-a569-68907f8fc2e0" />


