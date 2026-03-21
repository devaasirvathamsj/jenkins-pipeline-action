# Jenkins Pipeline with Docker Deployment

# Project Overview
This repository demonstrates a Jenkins pipeline that builds, runs, and cleans up a Dockerized application.  
It covers:
- Building Docker images from a Dockerfile
- Running containers with port mapping
- Automating cleanup of containers and images
- Using Jenkinsfile for pipeline as code

---

# Tech Stack
- Jenkins (Pipeline as Code)  
- Docker (Image build + container run)  
- Ubuntu (Base image in Dockerfile)  
- Python3 (Application runtime)  
- Bash scripting (automation steps inside pipeline)  

---

# Repository Structure
- `Jenkinsfile` → Defines pipeline stages (Build, Run, Cleanup)  
- `dockerfile` → Docker image definition (Ubuntu + Python3 + app)  
- `main.py` → Sample Python application entrypoint  

