🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team has developed a comprehensive fraud-detection training pipeline that includes data validation, Optuna tuning across two model families, model selection against a release threshold, Model Registry registration with a release-lane alias, and a consolidated training report. All of these components are integrated behind a single make train-pipeline command. Currently, the pre-staged system does not function end-to-end, as each invocation of make train-pipeline reveals a wiring issue, and two stages contain unfinished TODO items. To prepare for the release checklist, you must address the necessary updates in the Makefile, src/select_model.py, src/register.py, and src/report.py. Your objective is to resolve the wiring issues and complete the two TODO blocks, ensuring that make train-pipeline executes successfully from start to finish, the MLflow Model Registry contains a fraud-detector version under the staging alias, and reports/training_report.json compiles all upstream artifacts.


1. The MLflow tracking server is already running on port 5000. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty fraud-detection-tuning experiment.

2. The project layout under /root/code/fraud-detection/:
   - data/train.csv – The 200-row synthetic binary-classification dataset the rest of the Training section uses.
   - src/validate_data.py – Schema + null-check gate. Writes reports/validation_status.json. Correct.
   - src/tune.py – Runs 10 Optuna trials across RandomForest and GradientBoosting, each logged as an MLflow run tagged with model_type + params.{n_estimators,max_depth} + metrics.f1_score + the fitted model artefact. Correct.
   - src/select_model.py – Picks the winning run by the training metric and writes reports/selection.json. Has a wiring bug.
   - src/register.py – Registers the selected run's model as fraud-detector; the release-lane alias assignment is left as a # TODO.
   - src/report.py – Aggregates every upstream artefact into reports/training_report.json; the report assembly is left as a # TODO.
   - Makefile – train-pipeline target runs the five stages in order. Has a wiring bug.

3. The end state must include:
    - make train-pipeline completes without non-zero exit.
   - The fraud-detection-tuning MLflow experiment carries at least five trial runs, each with metrics.f1_score.
   - reports/selection.json, reports/validation_status.json, and reports/training_report.json are all present. The training report carries best_model, best_params, metrics, total_trials, and validation_status keys; validation_status is "ok" and total_trials is an integer ≥ 5.
   - The MLflow Model Registry (MLflow UI → Models) shows a fraud-detector registered model with at least one version. That version carries the staging alias and no production alias.

Run make train-pipeline once against the scaffold as-is — the first wiring bug surfaces immediately, and each re-run reveals the next. The two # TODO blocks (the registry alias and the report assembly) do not crash the pipeline; they are caught by the release checklist, so complete them before expecting a clean pass.

🛠️ **Solution:**
1. Open ```src/select_model.py``` and make below changes:
- ```text
  order_by=["metrics.f1_score DESC"]
  ```
- ```text
  score = float(best["metrics.f1_score"])
  ```
2. Add below line after TODO in ```src/register.py```
```text
client.set_registered_model_alias(
    name=REGISTERED_MODEL_NAME,
    alias=RELEASE_ALIAS,
    version=version.version,
)
```
3. Open ```src/report.py``` and add below line after TODO block.
```text
report = {
    "best_model": selection["model_type"],
    "best_params": best_params,
    "metrics": best_metrics,
    "total_trials": total_trials,
    "validation_status": validation["status"],
}
```
4. Open Makefile and reorder the scripts running:
```text
train-pipeline:
    python3 src/validate_data.py
    python3 src/tune.py
    python3 src/select_model.py
    python3 src/register.py
    python3 src/report.py
```
5. Run ```make train-pipeline``` in terminal.