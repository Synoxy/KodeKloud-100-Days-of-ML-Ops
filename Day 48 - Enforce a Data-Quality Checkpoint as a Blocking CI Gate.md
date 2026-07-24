🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team runs the drift_check Great Expectations checkpoint on the fraud-detector repository; however, it is currently not enforced in the continuous integration (CI) process. The data-quality workflow installs Great Expectations but does not execute the checkpoint, which means that a pull request that introduces data drift could potentially be merged without any checks. A teammate has already submitted a pull request to integrate the checkpoint into the workflow. Your objective is to configure the checkpoint as a blocking merge gate: ensure that the data-quality job executes python3 -m src.gx_run as a standard (non-continue-on-error) step, so that if the checkpoint fails, the job will also fail, thereby preventing the merge.


1. The Gitea UI is on port 3000 (Gitea button). Admin credentials: gitea-admin / gitea2026. The repo is at http://localhost:3000/gitea-admin/fraud-detector and a working clone is at /root/code/fraud-detector, already checked out on branch enforce-data-quality-gate. The PR is pre-opened.

2. The current data-quality job in .gitea/workflows/data-quality.yml checks out the repo and installs great_expectations, pandas, numpy—but does not run the checkpoint. src/gx_run.py bootstraps the GE project in-workspace, runs the drift_check checkpoint against data/transactions.csv, and exits non-zero when the data violates the suite. A blocking step that invokes python3 -m src.gx_run, pushed to the enforce-data-quality-gate branch, turns the checkpoint into a real merge gate.

3. The end state must include:
   - The data-quality job has a step whose command runs python3 -m src.gx_run.
   - That step is blocking: no continue-on-error: true, and no || true / ; true-style suffix that would swallow a failure.
   - The PR head commit's combined status reaches success (the current transactions.csv is clean, so the gate passes).

🛠️ **Solution:**
1. Add below in TODO in ```data-quality.yml```
```text
- name: Run drift_check checkpoint
  run: |
    python3 -m src.gx_run
```
2. Push the code to the repo.
```
git add .gitea/workflows/data-quality.yml
git commit -m "Enforce drift_check as blocking merge gate"
git push origin enforce-data-quality-gate  
```
3. Login to Gitea UI and merge the request. You should github action running.