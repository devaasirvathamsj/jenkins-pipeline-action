# Jenkins CI/CD Pipeline for Python Docker Application

![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat&logo=ubuntu&logoColor=white)

A declarative Jenkins pipeline that automates building, running, and cleaning Docker containers for a Python application — reducing manual deployment effort through end-to-end CI/CD automation.

---

## Architecture Overview

```
Code Push / Manual Trigger
          |
          v
Jenkins Pipeline
          |
          |-- Stage 1: Build Docker Image
          |-- Stage 2: Run Docker Container
          |-- Stage 3: Remove Container & Image (Cleanup)
          |
          v
Python Application running on port 8000
```

---

## Tech Stack

| Category | Tool |
|---|---|
| CI/CD | Jenkins |
| Containerization | Docker |
| Application | Python |
| OS | Ubuntu |
| Pipeline Type | Declarative Pipeline |

---

## Project Structure

```
jenkins-pipeline-action/
├── main.py
├── Dockerfile
└── Jenkinsfile
```

---

## Pipeline Stages

### Stage 1 - Build Docker Image
Builds the Docker image from the Dockerfile in the repository root. Uses environment variable `IMAGE_NAME` for the image tag.

### Stage 2 - Run Docker Container
Runs the Docker container in detached mode, mapping port 8000. Uses environment variable `CONTAINER_NAME` for the container name.

### Stage 3 - Remove Container & Image
Cleans up by stopping and removing the container and image after the run. Uses `|| true` to prevent pipeline failure if container or image doesn't exist.

---

## Prerequisites

- Jenkins installed and running
- Docker installed on Jenkins agent
- Jenkins has permission to run Docker commands

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/devaasirvathamsj/jenkins-pipeline-action.git
cd jenkins-pipeline-action
```

### 2. Install Jenkins

```bash
# Add Jenkins repo
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install -y jenkins
```

### 3. Start Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Access Jenkins at `http://localhost:8080`

### 4. Add Jenkins User to Docker Group

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

### 5. Create Jenkins Pipeline Job

1. Jenkins UI open panu
2. New Item click panu
3. Pipeline select panu
4. Pipeline section la SCM select panu
5. GitHub repo URL add panu
6. Branch `main` set panu
7. Save panu

### 6. Run the Pipeline

Build Now click panu - pipeline automatically run aagum.

---

## Jenkinsfile

```groovy
pipeline {
    agent any
    environment {
        IMAGE_NAME = "devaasirvatham/app"
        CONTAINER_NAME = "myapp-container"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 8000:8000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
            }
        }
        stage('Remove Container & Image') {
            steps {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker rmi ${IMAGE_NAME}:latest || true"
            }
        }
    }
}
```

---

## Dockerfile

```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY . .
RUN apt-get update && apt-get install -y python3 python3-pip
ENTRYPOINT ["python3"]
CMD ["main.py"]
```

---

## Useful Commands

```bash
# Check Jenkins status
sudo systemctl status jenkins

# Check Jenkins logs
sudo journalctl -u jenkins

# Check running containers
docker ps

# Check all containers
docker ps -a

# Check Docker images
docker images
```

---

## Key Concepts Demonstrated

- Declarative Jenkins pipeline
- Docker image build and container lifecycle management
- Environment variables in Jenkinsfile
- Automated cleanup after pipeline run
- End-to-end CI/CD automation

---

## Author

Deva Asirvatham SJ
- GitHub: [devaasirvathamsj](https://github.com/devaasirvathamsj)
- Email: devaasirvathamsj@gmail.com

