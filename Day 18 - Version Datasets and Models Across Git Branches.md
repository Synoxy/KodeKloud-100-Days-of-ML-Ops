🚨 **Problem:**
The xFusionCorp Industries ML team keeps different dataset and model versions on different Git branches so that the team can roll between versions cleanly. Tag the current state as v1.0, produce a v2-improved branch based on a newer dataset, and confirm that switching back restores the original data.

- A project exists at /root/code/fraud-detection/ with a working DVC pipeline and the baseline data/raw/transactions.csv already tracked.

- An improved dataset has been pre-staged at /root/code/fraud-detection/data/raw/transactions_v2.csv and is visible in the file explorer. Do not delete this file.

-  On the main branch, tag the current state as v1.0.

-  Create a new branch named v2-improved. Replace the tracked dataset with the contents of the v2 file, re-track it with DVC, re-run the pipeline, and commit the changes.

- Switch back to the main branch and use dvc checkout to restore the v1 dataset on disk. The restored content must match the hash recorded by the v1.0 tag.

🛠️ **Solution:**

1. Run below commands:
```text
- git checkout main
- git tag v1.0
- git checkout -b v2-improved
- cp data/raw/transactions_v2.csv data/raw/transactions.csv
- dvc add data/raw/transactions.csv
- git add data/raw/transactions.csv.dvc data/raw/.gitignore
- git commit -m "Updated Transaction file"
- dvc repro
- git add dvc.lock
- git commit -m "Added lock file"
- git checkout main
- dvc checkout
```