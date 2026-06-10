🚨 **Problem:**
The xFusionCorp Industries ML team is adopting MLflow for experiment tracking. Your task is to bring up a local MLflow tracking server on the ML pipeline workstation so experiments can be logged from the team's training code.

MLflow 3.x is pre-installed on the controlplane. Launch the tracking server in the background so that every end-state requirement below holds.

- The server is listening on port 5000 and is reachable on all interfaces.

- The backend store is a SQLite database at /root/code/mlflow-backend/mlflow.db. The database file must exist after the server has started.

- The artifact root is /root/code/mlflow-artifacts/.

- Any parent directories the server needs must be in place before it starts—MLflow will abort if the backend directory is missing.

- The MLflow UI button at the top of the lab must open a responsive dashboard in the browser. The button routes through the lab proxy, so the server must accept requests from any origin (--cors-allowed-origins '*') and any host header (--allowed-hosts '*') to avoid proxy-related rejections.

The server process must persist in the background so it survives terminal closure.

🛠️ **Solution:**
1. Create the artifacts folder.
```text
mkdir -p /root/code/mlflow-artifacts/
```
2. Run the below command to run the server.
```text
mlflow server \
  --backend-store-uri /root/code/mlflow-backend/mlflow.db \
  --default-artifact-root /root/code/mlflow-artifacts/ \
  --host 0.0.0.0 \
  --port 5000 \
  --cors-allowed-origins '*' \
  --allowed-hosts '*' &
```
3. You should see something like below:
<img width="1911" height="718" alt="image" src="https://github.com/user-attachments/assets/a1e9feae-cffa-4f7f-aeec-e690da7c02c7" />
