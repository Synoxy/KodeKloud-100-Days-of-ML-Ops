🚨 **Problem:**
After training a model, the xFusionCorp Industries ML team wants DVC to surface metrics through dvc metrics show and the DVC extension's METRICS view. The fraud-detection pipeline already trains a model and writes a metrics.json, but DVC does not recognise the file as a metric. Wire it in correctly.

- A project exists at /root/code/fraud-detection/ with a three-stage DVC pipeline (process_data, split_data, train). The train stage runs src/models/train.py, which writes the model to models/model.pkl and metrics to metrics.json. Do not modify the Python files.

- The train stage in dvc.yaml must declare metrics.json as a DVC metric output, not as a regular file output. The metric must be declared with cache: false so the JSON lives in Git for diff history rather than in the DVC cache.

- Re-run the pipeline with dvc repro so the metric registration takes effect.

- After your changes, dvc metrics show must report the accuracy and f1_score values from metrics.json.

The DVC extension's METRICS section under the DVC view will surface the same values directly in the editor once the metric is registered.

🛠️ **Solution:**
1. Update dvc.yaml as below by addidng metrics section after outs in train stage.

```text 
stages:
  process_data:
    cmd: python src/data/process_data.py
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv
      - src/data/split_data.py
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

  train:
    cmd: python src/models/train.py
    deps:
      - data/processed/train.csv
      - src/models/train.py
    outs:
      - models/model.pkl
    metrics:
      - metrics.json:
          cache: false
```
2. Run **dvc repro** and then **dvc metrics show**.

<img width="1077" height="857" alt="image" src="https://github.com/user-attachments/assets/46790fae-8eab-4c09-8e4e-1f4bb458ec3c" />

