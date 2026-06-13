🚨 **Problem:**
A xFusionCorp Industries data scientist has accumulated ten runs in the fraud-detection MLflow experiment. Your task is to triage those runs via the MLflow UI: mark the single best-performing candidate as the shortlisted model, and flag every clearly under-performing run for removal.

- The MLflow tracking server is already running on port 5000, and the fraud-detection experiment has been pre-populated with ten runs. The runs can be viewed via the MLflow UI button → fraud-detection experiment.

- Using the MLflow UI, complete the triage below. The end state is what is tested—the path taken through the UI is not.
  - Shortlist the best candidate. Among all runs where metrics.f1_score > 0.85, the single run with the highest f1_score must carry a run-level tag: key review-status, value shortlisted.
  - Reject the under-performers. Every run where metrics.f1_score < 0.75 must carry a run-level tag: key review-status, value rejected.
  - The other runs (those in the 0.75 ≤ f1 ≤ 0.85 band, and the second-best shortlisting candidate) must carry no review-status tag at all.

🛠️ **Solution:**
1. Open the ML-Flow App using the top right button.
2. Navigate to Experiments and open the **fraud_detection** experiment and go to **Evaluation Runs**.
<img width="1125" height="462" alt="image" src="https://github.com/user-attachments/assets/a7366534-77c9-49c9-ad86-791b084d7f8a" />
3. Open the experiment with highest run
4. Navigate to Overview and on bottom right you should see **Add tags**. Add the required tags as asked, for the one with highest f1_score add the key: review-status and value: shortlisted. Same for rejected ones. Refer( [Day 22 - Create and Organize MLflow Experiments.md](https://github.com/Synoxy/KodeKloud-100-Days-of-ML-Ops/blob/24454e8ac36bbbb4cc3f2707f3145cbca16a5e1a/Day%2022%20-%20Create%20and%20Organize%20MLflow%20Experiments.md) ) on how to add tag.
<img width="1125" height="557" alt="image" src="https://github.com/user-attachments/assets/30131237-4cdc-44b6-ae3e-54cbbd3b4791" />
5. Go back to Evaluation runs and refresh the page and group by **review-status**, you should see something like below.
<img width="1125" height="608" alt="image" src="https://github.com/user-attachments/assets/526b2933-53ec-4465-9d4e-bc72b593097e" />
