🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team requires that all credentials necessary for the lab-ops service—including MLflow's admin password, SeaweedFS's access keys, and PostgreSQL passwords—be retrieved from HashiCorp Vault at service startup, rather than being hardcoded into a startup script. A development Vault is currently operational on port 8200, and its web UI can be accessed via the Vault button. Additionally, an MLflow boot wrapper on the host is polling Vault every 5 seconds for the secret/mlflow.admin_password. However, the wrapper can only initiate MLflow once this KV entry is available. Your task is to enable the KV v2 engine in Vault, create the secret, and observe the successful startup of MLflow on port 5000.

1. The Vault UI is on port 8200 (Vault button opens the login page). The dev-mode root token is pre-created and written to /root/code/vault-token; paste the file's contents into the Vault Token login field. (Production deployments would use userpass / AppRole / OIDC instead, but the root token is the shortest path for a dev server.)

2. The MLflow wrapper picks up the new KV entry within ~5 s and execs mlflow server on port 5000. The MLflow UI button then opens the live tracker.

3. The end state must include:
   - A KV v2 secrets engine is enabled at path secret/ — GET /v1/sys/mounts returns secret/ with type: kv and options.version: "2".
   - The secret at path secret/mlflow carries a non-empty admin_password key — GET /v1/secret/data/mlflow (with the root token) returns a JSON body whose data.data.admin_password is a non-empty string.
   - GET http://localhost:5000/ answers 200 – MLflow is running because the wrapper found the password.
   - Running services should not know their own secrets at image-build time. A Vault-first pattern lets you rotate a credential in Vault and restart the consumer to pick up the new value—no rebuild, no config patch, no secret in the commit history. This task's single-service wrapper is the minimum viable version of that pattern; a real deployment replaces the root token with an AppRole login and adds audit logging. 

🛠️ **Solution:**
1. Open the ```vault UI``` from the terminal and use the token from ```vault-token``` file to login.
<img width="975" height="357" alt="image" src="https://github.com/user-attachments/assets/b40e63eb-c265-42d2-80c0-982920e9ab64" />

2. Open the terminal on top right and use the below command to create the secret engine.
```text
write sys/mounts/secret type=kv options=version=2
```
<img width="975" height="432" alt="image" src="https://github.com/user-attachments/assets/ddf6ff83-4c5e-4d10-ae7d-7e14e94d0d3d" />

3. Navigate to secrets and open the secret named ```secret```.
<img width="975" height="421" alt="image" src="https://github.com/user-attachments/assets/c92eb394-9459-4832-9b71-cf20e5af4ac9" />

4. Click on ```Create secret``` and fill the below details as below. Save it.
<img width="975" height="320" alt="image" src="https://github.com/user-attachments/assets/3627a950-bc84-4fc4-86d0-59536a3be57c" />
<img width="975" height="466" alt="image" src="https://github.com/user-attachments/assets/c2a00afe-8c28-4d44-824f-6780ba901c1b" />

5. Open the terminal again and run below command to see if secret was created successfully.
```text
kv-get secret/mlflow
```
<img width="975" height="367" alt="image" src="https://github.com/user-attachments/assets/c2a0823e-19b6-4d9f-bfd5-18f0efcbea22" />

6. Open the ```Mlflow UI``` and verify if accessible.
<img width="975" height="534" alt="image" src="https://github.com/user-attachments/assets/b0aee598-05b6-4b71-b666-a80d52e2d560" />
