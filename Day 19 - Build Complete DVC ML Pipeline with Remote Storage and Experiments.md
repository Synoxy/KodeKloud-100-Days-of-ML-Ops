🚨 **Problem:**

Complete the xFusionCorp Industries fraud-detection production DVC pipeline. Three stages are already wired in dvc.yaml, two remain, and the pipeline must finish as a reproducible, SeaweedFS-backed, v1.0-tagged release.

- A project exists at /root/code/ml-pipeline/ with Git and DVC initialised. The params.yaml is in place and the .dvc/config is pre-configured to push to the SeaweedFS bucket dvc-storage at http://localhost:8333.

- The ingest, validate, and preprocess stages are already declared in dvc.yaml, but one of them contains an incorrect output path that prevents dvc repro from completing. Find and fix it.

- The remaining two stages need to be added:
  - train – Depends on the preprocessed dataset and scripts/train.py; reads n_estimators, max_depth, test_size, and random_seed from params.yaml; outputs models/model.pkl and data/processed/test_split.csv; declares metrics.json as a DVC metric with cache: false.
  -  evaluate – Depends on models/model.pkl, data/processed/test_split.csv, and scripts/evaluate.py; outputs reports/evaluation.json declared with cache: false.

- The two scripts you need are pre-staged at /root/code/ml-pipeline/scripts-staging/train.py and scripts-staging/evaluate.py. Copy them into scripts/ before adding the stages.

- Run the full pipeline with dvc repro, push the cache to the SeaweedFS remote with dvc push, and tag the current state as v1.0.

- Commit every change to Git so the release is fully captured.

🛠️ **Solution:**
1. run  below commands in terminal.
  
```text
cp /root/code/ml-pipeline/scripts-staging/train.py scripts/
cp /root/code/ml-pipeline/scripts-staging/evaluate.py scripts/
```
2. Open **dvc.yaml** and update the preprocess stage  **outs**
```text
preprocess:
    cmd: python scripts/preprocess.py
    deps:
      - data/raw/data.csv
      - scripts/preprocess.py
    outs:
      - data/processed/clean.csv
```
3. Add train and evaluate stage post preprocess stage.
```text
train:
    deps:
      - scripts/train.py
      - data/processed/clean.csv
    cmd: python scripts/train.py
    outs:
      - models/model.pkl
      - data/processed/test_split.csv
    metrics:
      - metrics.json:
          cache: false
    params:
      - n_estimators
      - max_depth
      - test_size
      - random_seed
  evaluate:
    cmd: python scripts/evaluate.py
    deps:
      - models/model.pkl
      - data/processed/test_split.csv
      - scripts/evaluate.py
    metrics:
      - reports/evaluation.json:
          cache: false
```
4. Run below commannds.
```text
- dvc repro
- git add dvc.yaml dvc.lock metrics.json reports/evaluation.json
- dvc push
- git tag v1.0
- git show v1.0 #You should see dvc.lock in the commit.
- dvc status
- dvc pull
```