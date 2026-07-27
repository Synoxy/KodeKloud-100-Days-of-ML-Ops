🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team has deployed a fraud-detection model as a Docker image. However, the current runtime image includes all packages required for the training phase and the training source itself, resulting in an unnecessarily large image. Your objective is to refactor the single-stage Dockerfile located at /root/code/ml-serve/ into a multi-stage build. This should comprise a builder stage that trains the model and generates model.pkl, followed by a runtime stage that installs only the dependencies necessary for serving and copies the trained model from the builder stage.

1. The Docker daemon is already running. docker version can be run in a VS Code terminal to confirm.

2. The project layout under /root/code/ml-serve/:
   - train_model.py – Fits a 10-tree RandomForest on the shared 10-row synthetic fraud set and writes /app/model.pkl via joblib.dump(...). Correct and must remain intact.
   - serve.py – Flask app loading the model and exposing POST /predict + GET /health on port 8080. Correct and must remain intact.
   - Dockerfile – A single-stage build that installs scikit-learn, pandas, numpy, joblib, and flask, runs the trainer at build time to bake the model in, and serves. The reader rewrites this file.

3. The end state must include:
   - The Dockerfile carries at least two FROM instructions; the first is given a name (e.g. AS builder) so a later stage can reference it.
   - The builder stage produces /app/model.pkl (the trained model).
   - The runtime stage contains /app/model.pkl (copied out of the builder stage) and serve.py.
   - The runtime stage's pip install line installs only the four packages serve.py needs: flask, joblib, numpy, scikit-learn.
   - docker images ml-serve:v1 lists the built image; docker run --rm -p 8090:8080 ml-serve:v1 exposes /health returning {"status": "ok"} on port 8090.

🛠️ **Solution:**
1. Update the ```Dockerfile as below```
```text
FROM python:3.11-slim AS builder
WORKDIR /app
RUN pip install --no-cache-dir scikit-learn pandas numpy joblib flask
COPY train_model.py /app/train_model.py
COPY serve.py /app/serve.py
RUN python3 /app/train_model.py

FROM python:3.11-slim AS runtime
WORKDIR /app
RUN pip install --no-cache-dir flask joblib numpy scikit-learn
COPY --from=builder /app/model.pkl /app/model.pkl
COPY --from=builder /app/serve.py /app/serve.py
EXPOSE 8080
CMD ["python3", "/app/serve.py"]
```
2. Build the docker image and verify the image built.
```text
docker build -t ml-serve:v1 .
```
```text
docker images
```
<img width="975" height="746" alt="image" src="https://github.com/user-attachments/assets/76bcfdad-fc64-4c7e-8d35-8fd6a1699faf" />

3. Run the container using the built image.
```text
docker run --rm -p 8090:8080 ml-serve:v1
```
<img width="970" height="259" alt="image" src="https://github.com/user-attachments/assets/5f792ac6-1141-4f87-872d-3493886f9dad" />

4. Open another terminal and run the below curl command, you should see ```{"status":"ok"}```
<img width="975" height="186" alt="image" src="https://github.com/user-attachments/assets/da41c117-11e1-4b91-83f2-00848c8e706b" />
