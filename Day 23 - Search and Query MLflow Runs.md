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
3. Open the experiment with highest run
4. Navigate to Overview and on bottom right you should see **Add tags**. Add the required tags as asked, for the one with highest f1_score add the key: review-status and value: shortlisted. Same for  rejected ones. Refer() on how to add tag.
5. Go back to Evaluation runs and refresh the page and group by **review-status**, you should see something like below.
