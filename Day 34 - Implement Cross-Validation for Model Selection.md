🚨 **Problem Statement:**

The xFusionCorp Industries ML platform team evaluates fraud-detection candidates with k-fold cross-validation so every candidate is measured on multiple folds of an imbalanced dataset. A draft cross-validation scaffold exists at /root/code/fraud-detection/src/models/cross_validate.py, but its report does not match the release checklist and the fold strategy does not preserve the class ratio. Your task is to correct the scaffold so the cross-validation report lands in the expected shape.

1. The MLflow tracking server is already running on port 5000. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard  oads with an empty fraud-detection-cv experiment.

2. The project layout under /root/code/fraud-detection/:
   - data/train.csv – A pre-generated 200-row synthetic binary-classification dataset with an imbalanced class split (roughly 70 / 30). Do not regenerate it.
   - src/models/cross_validate.py – The cross-validation scaffold. Every concern other than the splitter and the aggregate schema is correctly wired: fold iteration, per-fold metric computation, nested MLflow runs under a parent, JSON persistence, and artefact logging.
   - reports/ – Where the cross-validation report must land.

3. Open src/models/cross_validate.py in the VS Code editor, correct the two problems that keep the report from meeting the release checklist, save, and run the script.

4. The end state must include:
   - A file at /root/code/fraud-detection/reports/cv_results.json (absolute path, inside the project's reports directory).
   - That JSON contains exactly these seven top-level keys: mean_accuracy, std_accuracy, mean_f1, std_f1, mean_roc_auc, std_roc_auc, folds. Every mean_* and std_* value is numeric.
   - The folds value is a list of five per-fold dictionaries; each carries the keys fold, accuracy, f1, roc_auc.
   - The cross-validation splitter is stratification-aware – Each fold preserves the dataset's class ratio.
   - One parent MLflow run in the fraud-detection-cv experiment with five nested children (fold-1 through fold-5), each logging the per-fold metrics.

StratifiedKFold is already imported at the top of the scaffold—no new imports are required. The fix is confined to the CV splitter and the aggregate dict.

🛠️ **Solution:**
1. Open ```cross_validate.py``` and make the below update:
```text
cv = StratifiedKFold(n_splits=N_SPLITS, shuffle=True, random_state=42)
```
```text
aggregate = {
    "mean_accuracy": round(float(np.mean(acc_vals)), 6),
    "std_accuracy": round(float(np.std(acc_vals, ddof=1)), 6),
    "mean_f1": round(float(np.mean(f1_vals)), 6),
    "std_f1": round(float(np.std(f1_vals, ddof=1)), 6),
    "mean_roc_auc": round(float(np.mean(auc_vals)), 6),
    "std_roc_auc": round(float(np.std(auc_vals, ddof=1)), 6),
    "folds": fold_results,
}
```
2. Run: ```python cross_validate.py``` and you shold see below details.