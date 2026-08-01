🚨 **Problem Statement:**
The xFusionCorp Industries ML platform team deploys Flask-based inference APIs as Docker images, incorporating Docker-native HEALTHCHECK instructions. This allows operators to easily verify the serving status of the process by running docker inspect --format='{{.State.Health.Status}}'. However, the draft Dockerfile located at /root/code/ml-health/ currently does not meet these standards; executing docker inspect --format='{{.State.Health.Status}}' ml-health-api returns unhealthy, and docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1 shows no exposed ports. Your task is to rectify the HEALTHCHECK target and include the missing EXPOSE declaration.

1. The Docker daemon is already running. docker version can be run in a VS Code terminal to confirm. With the current image built and run as ml-health-api, docker inspect --format='{{.State.Health.Status}}' ml-health-api reports unhealthy and docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1 shows no exposed ports.

2. The project layout under /root/code/ml-health/:
   - app.py – Flask API serving GET /health (returns {"status": "ok"} / 200) and POST /predict (returns a rule-based fraud flag) on port 8085. Correct.
   - Dockerfile – python:3.11-slim, installs flask, copies app.py, carries a HEALTHCHECK + CMD. The corrected image is built as ml-health:v1 and run as a container named ml-health-api with host port 8085 published.

3. The end state must include:
   - docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1 reports map[8085/tcp:{}].
   - After docker run, docker inspect --format='{{.State.Health.Status}}' ml-health-api reads healthy within ~15 seconds.
   - curl http://localhost:8085/health returns {"status": "ok"} with HTTP 200.

🛠️ **Solution:**
1. Update the Docker image and below:
   - Update
   ```text
   CMD python3 -c "import urllib.request; urllib.request.urlopen('http://localhost:8085/health')" || exit 1
   ```
  - Add
  ```text
  EXPOSE 8085
  ``` 
2. Build the docker image with name: 
```text
docker build -t "ml-health:v1" .
```
3. Run the container with name:
```text
docker run -d -p 8085:8085 --name ml-health-api ml-health:v1
```
<img width="975" height="151" alt="image" src="https://github.com/user-attachments/assets/97bdb0a9-5847-4805-a712-d8eab3341cbd" />

4. Verify the end state:
   - Inspect health 
   ```text
   docker run -d -p 8085:8085 --name ml-health-api ml-health:v1
   ```
   - Verify Ports
   ```text
   docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1
   ```
<img width="975" height="102" alt="image" src="https://github.com/user-attachments/assets/b0385cd2-ceb1-44c1-97b5-298a4f820dd5" />
