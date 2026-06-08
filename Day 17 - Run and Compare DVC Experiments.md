🚨 **Problem:**

The xFusionCorp Industries data science team compares multiple training runs with different hyperparameters using DVC experiments. Run three experiments that vary the n_estimators hyperparameter, identify the best-performing one, and promote it to the tracked workspace.

- A project exists at /root/code/fraud-detection/ with a parameterised DVC pipeline already in place. params.yaml contains n_estimators: 100 and the baseline pipeline has been run once.

- Run three DVC experiments, each with a different value for n_estimators across a reasonable range (for example 50, 200, and 500). Each experiment should produce a fresh metrics.json.

- Compare the experiments and choose the one whose f1_score is the highest.

- Apply the chosen experiment to the workspace so its n_estimators, metrics.json, and models/model.pkl become the tracked state.

🛠️ **Solution:**

1. Open the terminal and run below commands:
```text

dvc exp run -S n_estimators=50

dvc exp run -S n_estimators=200

dvc exp run -S n_estimators=500

```
2. Run **dvc exp show** and you should see below response. Note pick the higher values for accuracy and f1_score.

- dvc exp run -S n_estimators=500
├── **91f9f09** [sappy-kibe]   01:39 AM       0.85       0.83   500            16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7

- dvc exp run -S n_estimators=200  
├── **084e6f4** [minim-kail]   01:39 AM       0.94       0.92   200            16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7 

- dvc exp run -S n_estimators=50
├── **7a7eb01** [savvy-chiv]   01:39 AM     0.9175     0.8975   50             16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7

3. Apply the one with highest score, in this case its for **n_estimators=200**. Run below command.
```text
**dvc exp apply 084e6f4**
```