🚨 **Problem:**

The xFusionCorp Industries ML platform team needs two trained candidates promoted through the MLflow Model Registry so the ops side can track which model version is serving production traffic. Both runs already exist in the fraud-detection experiment. Your task is to register both as versions of a new fraud-detector model, add a model-level description, and assign challenger and champion aliases—all through the MLflow UI.

- The MLflow tracking server is already running on port 5000 and two runs are pre-populated in the fraud-detection experiment: a baseline run (n_estimators=100, max_depth=5, f1_score=0.80) and an improved run (n_estimators=200, max_depth=10, f1_score=0.89). Both runs can be opened via the MLflow UI button → fraud-detection experiment.

- Using the MLflow UI, reach the end state below. The order (baseline first, improved second) matters because MLflow assigns version numbers sequentially within a registered model.

  - A registered model named fraud-detector exists in the Model Registry.
  - The registered model carries a non-empty description that references the word fraud (any phrasing; for example Fraud detection model for xFusionCorp transactions).
  - Version 1 of fraud-detector is the baseline run and carries the alias challenger.
  - Version 2 of fraud-detector is the improved run and carries the alias champion.

The result can be confirmed by opening Model registry → fraud-detector in the MLflow UI. Two versions are listed, the description is shown at the top of the model page, and the alias column (or the Aliases field on each version) indicates challenger on v1 and champion on v2.

🛠️ **Solution:**
1. Navigate to Model Training using mlflow UI button.
2. Go to Runs under Model Training.
<img width="1125" height="294" alt="image" src="https://github.com/user-attachments/assets/c2d96e9a-8540-481c-aab2-def7822384d5" />

3. Open the baseline, click on model and make below changes.
<img width="1125" height="477" alt="image" src="https://github.com/user-attachments/assets/5aa88a43-ce62-4abc-946a-2484f8306f8d" />

4. Add the descripton as: *"Fraud detection model for xFusionCorp transactions"*.
<img width="1125" height="313" alt="image" src="https://github.com/user-attachments/assets/45c06bc5-c6b4-41c7-9f7c-b235ee0702c7" />

5. Click on Register model --> Add new model. Give the model name as: *fraud-detector*.
<img width="1125" height="435" alt="image" src="https://github.com/user-attachments/assets/de681bee-e1d6-4022-a6a0-a20220d2afff" />

6. Click on Register and navigate to the model and Add alias as **challenger** for Version 1.
<img width="1125" height="524" alt="image" src="https://github.com/user-attachments/assets/9be6865d-e1ff-4c31-82f5-f97a36c896ef" />

7. Perfom the same actions for improved run and Add alias as champion for version 2. For improved select the same model while registering the model.

<img width="1125" height="405" alt="image" src="https://github.com/user-attachments/assets/bbc708e0-4bec-4f22-92fe-5518e4b5769e" />
<img width="864" height="525" alt="image" src="https://github.com/user-attachments/assets/8db76097-8b4d-46ff-9cc8-afc96a19f393" />

