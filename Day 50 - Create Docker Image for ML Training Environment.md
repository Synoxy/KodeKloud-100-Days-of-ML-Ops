🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team has created a Docker image for the fraud-detection training environment, allowing every engineer to achieve consistent results by executing the command docker run ml-trainer:v1. A scaffold for the Dockerfile is located at /root/code/ml-docker/, with its construction outlined as numbered TODOs. Your objective is to complete the Dockerfile in accordance with the team's standards, ensuring that the command docker build -t ml-trainer:v1 . executes successfully and that every Python import required by the training code is correctly resolved within the image.

1. The Docker daemon is already running. docker version can be run in a VS Code terminal to confirm.

2. The project layout under /root/code/ml-docker/:
   - train.py – 10-row synthetic fraud-detection training stub; fits a RandomForest, prints accuracy + F1, and persists the model to /app/model.pkl with joblib.dump(...). Correct and must remain intact.
   - Dockerfile – The image definition, scaffolded as five numbered TODOs (base image, working directory, dependency install, copy, command). Author each to the team standard.

3. The end state must include:
   - The base image is python:3.11-slim.    
   - WORKDIR /app is set.
   - The pip install line installs every package the training code imports (scikit-learn, pandas, numpy, joblib).
   - docker images ml-trainer:v1 lists the built image.
   - docker run --rm ml-trainer:v1 python3 -c "import sklearn, pandas, numpy, joblib; print('OK')" prints OK.

🛠️ **Solution:**
1. Add below in ```Dockerfile```
```text
FROM python:3.11-slim
WORKDIR /app
RUN pip install --no-cache-dir scikit-learn pandas numpy joblib
COPY train.py /app/train.py
CMD ["python3","train.py"]
```
2. Run below commands:
```text
docker build -t ml-trainer:v1 .
```
```Output:``` 
<img width="975" height="558" alt="image" src="https://github.com/user-attachments/assets/a32074dd-6b3e-4827-8559-053a68bc3c74" />

```text
docker images
```
```text
docker run --rm ml-trainer:v1 python3 -c "import sklearn, pandas, numpy, joblib; print('OK')"
```
```Output:```

<img width="861" height="186" alt="image" src="https://github.com/user-attachments/assets/22cdb74a-986e-4ced-b7d7-5e6287f12ad4" />
