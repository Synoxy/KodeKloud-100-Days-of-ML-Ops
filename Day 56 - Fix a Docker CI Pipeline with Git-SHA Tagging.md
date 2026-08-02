🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team operates a shell-based Docker CI pipeline for the fraud-detection Flask service. In this process, tests are executed, the image is built, a short git SHA is applied as the tag, and the tagged image is subsequently pushed to the local private registry. However, the pre-staged build.sh located at /root/code/ci/ does not currently execute cleanly from start to finish. Your objective is to rectify the configuration so that ./build.sh completes its execution without errors and that the registry catalog displays ml-ci-app tagged with the current git short SHA.

1. The Docker daemon is already running and a registry:2 container named local-registry is already up on host port 5555.

2. The repository layout under /root/code/ci/:
   - app/app.py – Flask service exposing /health + /predict on port 8086. Correct.
   - app/test_app.py – Three pytest cases covering /health, the fraud-flag flow, and the pass-through flow. Correct.
   - app/Dockerfile – python:3.11-slim, installs flask, COPYs app.py, exposes 8086, runs the Flask app. Correct.
   - app/.git/ – A local git repository initialised at startup with a single "Initial CI baseline" commit. Correct.
   - build.sh – Executable shell script with four stages (test → build → tag → push). Needs attention.

3. The end state must include:
   - ./build.sh runs end-to-end without non-zero exit.
   - docker images ml-ci-app:latest lists the locally-built image.
   - curl http://localhost:5555/v2/_catalog lists ml-ci-app in the repositories array.
   - curl http://localhost:5555/v2/ml-ci-app/tags/list lists the current git -C app rev-parse --short HEAD value in the tags array.

🛠️ **Solution:**
1. One the docker image and make below changes:
   - Update the port in ```REGISTRY``` variable and change it to ```"localhost:5555"```
   - Update the command as below for stage 1
    ```text
    python3 -m pytest app/test_app.py
    ```
   - In stage 3 update the variable ```GIT_SHA``` to ```SHA```

2. Build the image(run ./build.sh in Terminal)
3. Run below commands and verify the tags
   ```text
   curl http://localhost:5555/v2/_catalog
   ```
   ```text
   curl http://localhost:5555/v2/ml-ci-app/tags/list
   ```
   <img width="765" height="170" alt="image" src="https://github.com/user-attachments/assets/7453d649-b7d0-4817-8792-a19740ccc506" />
