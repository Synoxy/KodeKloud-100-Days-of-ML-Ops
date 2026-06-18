🚨 **Problem Statement:**
A xFusionCorp Industries data scientist has trained three candidate models on the same problem and logged them to the model-comparison experiment. Your task is to review the candidates side by side in the MLflow UI and explicitly mark the winning run so downstream tooling can pick it up.

- The MLflow tracking server is already running on port 5000 and the model-comparison experiment has been pre-populated with three runs, each named after its algorithm (RandomForest, GradientBoosting, LogisticRegression) and carrying accuracy and f1_score metrics. The runs can be viewed via the MLflow UI button → model-comparison experiment.

- Using the MLflow UI, inspect the three runs side by side and identify the winner by metrics.f1_score.
  - The run with the highest f1_score must carry a run-level tag: key production-candidate, value true.
  - Neither of the other two runs may carry a production-candidate tag.
The result can be confirmed in the MLflow UI: the model-comparison experiment lists three runs, and only the top-f1_score run shows the production-candidate tag on its detail page.

🛠️ **Solution:**
1. Open MLflow UI and navigate to Training runs.
<img width="1125" height="272" alt="image" src="https://github.com/user-attachments/assets/228846da-e157-41ab-9584-f0a899f3c573" />

2. Add the column *f1_score*.
<img width="1125" height="473" alt="image" src="https://github.com/user-attachments/assets/d5dd98fe-7fbf-487f-963e-287f468e7e6f" />

3. Find the highest *f1_score* model.
<img width="1125" height="279" alt="image" src="https://github.com/user-attachments/assets/a27ce175-f591-40c7-9c41-01e6d67ec1ee" />

4. Open the model and add the tag with key: **production-candidate** and value: **true** .
<img width="1125" height="450" alt="image" src="https://github.com/user-attachments/assets/3804621f-cbf9-4778-a90b-bafea172dcc6" />
<img width="1125" height="473" alt="image" src="https://github.com/user-attachments/assets/4c09c2da-52e5-41bd-947e-90e61ee8f584" />
