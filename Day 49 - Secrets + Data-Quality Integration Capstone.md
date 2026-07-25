🚨 **Problem Statement:**

The xFusionCorp Industries ML platform team is preparing to cut their first end-to-end release of the fraud-detector repository. This release comprises a three-job Gitea Actions workflow that includes: pulling the MLflow credential from Vault, verifying data quality by gating on a Great Expectations checkpoint, and registering the trained model in MLflow. All four services—Vault, MLflow, Gitea, and the Actions runner—are operational. However, the first job of the workflow remains incomplete, as the step for reading from Vault is noted as a TODO. Your final task is to complete this section by scripting the step to pull the MLflow credential from Vault, and subsequently, execute the release across its various user interfaces: staging the credential in Vault, initiating and merging a pull request in Gitea, and promoting the registered model in MLflow.

1. Each of the four UIs has a button at the top of the lab:

   - Gitea (port 3000) – gitea-admin / gitea2026. The fraud-detector repo sits on main; a feature branch production-release is pre-pushed. No pull request has been opened yet.
   - Vault (port 8200) – log in with the token at /root/code/vault-token. The KV v2 engine is enabled at secret/; secret/mlflow is empty.
   - MLflow UI (port 5000) – the Models page is empty.
   - Data Docs – rendered by the data-quality job once the workflow runs.
2. The workflow at .gitea/workflows/production.yml on the production-release branch has three jobs (fetch-secret → data-quality → register-model). The data-quality and register-model jobs are complete; the fetch-secret job's Vault-read step is left as a # TODO in the working clone at /root/code/fraud-detector. The workflow reads a Vault KV key, runs the schema_check GE checkpoint, and registers the trained model as fraud-detector in MLflow; it only triggers on pull_request against main.

3. The end state must include:
   - secret/mlflow has a non-empty mlflow_password key (any value works).
   - The fetch-secret job on the production-release branch reads the KV v2 path secret/data/mlflow and its mlflow_password key (the authored step).
   - A pull request exists from production-release → main and has been merged.
   - The workflow run on that PR's head commit reaches combined status success (all three jobs green).
   - fraud-detector is registered in MLflow with the production alias pointing at one of its versions.

🛠️ **Solution:**
1. Open the vault using the vault-token and create the secret with the path: ```mlflow``` and key: ```mlflow_password```.
<img width="975" height="338" alt="image" src="https://github.com/user-attachments/assets/1a3d9607-61ce-49cb-9c2e-a3f2c823855a" />

2. Open the ```.gitea/workflows/production.yml``` file and update the TODO for fetch-secret job.
```text
VAULT_TOKEN=$(cat /root/code/vault-token)
echo "$VAULT_TOKEN"

PASS=$(curl -s \
  "$VAULT_ADDR/v1/secret/data/mlflow" \
  -H "accept: application/json" \
  -H "X-Vault-Token: $VAULT_TOKEN" \
  | jq -r '.data.data.mlflow_password')

if [ -z "$PASS" ]; then
  echo "::error::Missing mlflow_password in Vault secret — release blocked"
  exit 1
else
  echo "::notice::Fetched mlflow_password (length ${#PASS})"
fi
```
3. Push the changes to git repo: ```fraud-detector``` and login to Gitea.
<img width="975" height="812" alt="image" src="https://github.com/user-attachments/assets/ea83e85e-e38c-426b-b9cf-c636b0fcaef2" />

4. Create a new pull request and then merge the request. Click on Actions and verify all 3 stages have been executed successfully.
<img width="975" height="352" alt="image" src="https://github.com/user-attachments/assets/9fa5e4d9-d3c9-4670-bb40-e5cb48c2f93b" />

5. Open the Mlflow UI and add the alias in the ```fraud-detector``` model. Refer `Day 25 - Register, Version, and Manage Model Lifecycle.md`
<img width="975" height="336" alt="image" src="https://github.com/user-attachments/assets/3210af0a-b5c7-4a4e-bb3f-829520c90b0a" />
