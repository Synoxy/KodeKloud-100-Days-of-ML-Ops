🚨 **Problem:**

The xFusionCorp Industries ML team manages model hyperparameters through params.yaml so experiments can vary without code changes. The fraud-detection project's train stage already wires params.yaml for n_estimators, but dvc repro currently fails. Correct the parameter wiring and demonstrate that DVC re-runs the train stage when the parameter changes.

- A project exists at /root/code/fraud-detection/ with a three-stage DVC pipeline (process_data, split_data, train) and a params.yaml already in place. Do not modify the Python files.

- The train stage in dvc.yaml references the n_estimators parameter. Every name listed under params: must resolve to a key in params.yaml.

- Review params.yaml, correct whatever prevents dvc repro from completing, and run the full pipeline.

- Demonstrate that DVC tracks parameter changes by updating n_estimators to a different value (for example 200). Run dvc repro again—only the train stage should re-execute, the new value must be recorded in dvc.lock, and models/model.pkl must be regenerated.

The DVC extension's PARAMS section under the DVC view will surface the values from params.yaml directly in the editor.

🛠️ **Solution:**
1. Update **n_estimator** to **n_estimators** in **dvc.yaml** and run **dvc repro**.
2. After dvc repro successfully runs, update **n_estimators** value to **200** and run **dvc repro** again.