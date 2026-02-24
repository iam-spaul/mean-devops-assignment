

#  MEAN Stack CRUD Application – DevOps Deployment

---

## Project Overview

This project demonstrates an end-to-end DevOps implementation of a full-stack **MEAN (MongoDB, Express, Angular 15, Node.js)** CRUD application.

The application allows users to:

* Create tutorials
* Retrieve tutorials
* Update tutorials
* Delete tutorials
* Search tutorials by title

The entire system is:

* Containerized using Docker
*  Orchestrated using Docker Compose
*  Deployed on AWS EC2 (Ubuntu)
*  Exposed via Nginx Reverse Proxy
*  Automated using GitHub Actions CI/CD

---

#  Architecture Overview

```
User (Browser)
   ↓
Nginx Reverse Proxy (Port 80)
   ↓
Frontend – Angular (Docker Container)
   ↓
Backend – Node + Express (Docker Container)
   ↓
MongoDB (Docker Container with Persistent Volume)
```

---

# Technology Stack

* Angular 15
* Node.js
* Express.js
* MongoDB
* Docker
* Docker Compose (v2)
* Nginx
* AWS EC2 (Ubuntu 22.04)
* GitHub Actions

---

# 📂 Project Structure

```
mean-devops-assignment/
│
├── backend/
│   ├── app/
│   ├── Dockerfile
│   └── server.js
│
├── frontend/
│   └── Dockerfile
│
├── nginx/
│   └── default.conf
│
├── docker-compose.yml
├── .github/workflows/deploy.yml
└── README.md
```

---

# Docker Configuration

## Backend

* Node 18 base image
* Installs dependencies
* Exposes port 8080
* MongoDB connection via environment variable

MongoDB connection string:

```
mongodb://mongo:27017/dd_db
```

---

## Frontend

* Multi-stage Docker build
* Angular production build
* Served using Nginx container
* Communicates with backend via:

```
/api/tutorials
```

---

## MongoDB

* Official MongoDB image
* Persistent Docker volume attached

---

#  Nginx Reverse Proxy Configuration

All external traffic is exposed via port 80.

`nginx/default.conf`

```
server {
    listen 80;

    location / {
        proxy_pass http://frontend:80;
    }

    location /api {
        proxy_pass http://backend:8080;
    }
}
```

### Why Reverse Proxy?

* Prevents exposing backend port publicly
* Avoids CORS issues
* Centralized traffic management
* Production-ready architecture

---

# Step-by-Step Local Setup

---

## 1️ Clone Repository

```
git clone https://github.com/iam-spaul/mean-devops-assignment.git
cd mean-devops-assignment
```

---

## 2️ Build and Run Containers

```
docker compose up --build
```

Access application at:

```
http://localhost
```

---

# ☁️ AWS EC2 Deployment

---

## 1 Launch EC2 Instance

* OS: Ubuntu 22.04
* Instance type: t3.small
* Security Group:

  * Port 22 (SSH)
  * Port 80 (HTTP)

---

## 2️ Install Docker & Compose

```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 3️ Deploy Application

```
git clone https://github.com/iam-spaul/mean-devops-assignment.git
cd mean-devops-assignment
docker compose pull
docker compose up -d
```

Application URL:

```
http://3.111.219.189/
```

---

# Docker Image Build & Push Process

Manual build (first-time setup):

```
docker build -t <dockerhub-username>/backend:latest ./backend
docker build -t <dockerhub-username>/frontend:latest ./frontend

docker push <dockerhub-username>/backend:latest
docker push <dockerhub-username>/frontend:latest
```

Production deployment uses images instead of build in `docker-compose.yml`.

---

# CI/CD Pipeline – GitHub Actions

---

## Trigger

Pipeline runs automatically on:

```
Push to main branch
```

---

## Workflow Steps

1. Checkout repository
2. Login to Docker Hub
3. Build backend Docker image
4. Build frontend Docker image
5. Push images to Docker Hub
6. SSH into EC2 instance
7. Pull latest Docker images
8. Restart containers

Workflow file location:

```
.github/workflows/deploy.yml
```

---

# GitHub Secrets Used

| Secret Name     | Description             |
| --------------- | ----------------------- |
| DOCKER_USERNAME | Docker Hub username     |
| DOCKER_PASSWORD | Docker Hub access token |
| VM_IP           | EC2 Public IP address   |
| SSH_PRIVATE_KEY | EC2 private key content |

---

# Screenshots Section

will upload soon

---

## CI/CD Configuration and Execution

* Screenshot of `.github/workflows/deploy.yml`
* Screenshot of successful GitHub Actions workflow run

---

## Docker Image Build & Push

* Screenshot of Docker Hub backend image
* Screenshot of Docker Hub frontend image

---

## Application Deployment & Working UI

* Screenshot of application running at:

```
http://3.111.219.189/
```

---

## Nginx Setup

* Screenshot of `nginx/default.conf`
* Screenshot showing port 80 exposure

---

## Infrastructure Details

* EC2 instance dashboard
* Security group inbound rules
* `docker ps` output

---

#  Verification Commands

Check running containers:

```
docker ps
```

Test backend directly:

```
curl http://localhost:8080/api/tutorials
```

Test through reverse proxy:

```
curl http://localhost/api/tutorials
```

---

# DevOps Concepts Demonstrated

* Multi-container orchestration
* Reverse proxy implementation
* Environment-based configuration
* CI/CD automation
* Docker image lifecycle management
* Cloud deployment
* Infrastructure validation
* Production-grade architecture

---

# Conclusion

This project demonstrates a complete end-to-end DevOps workflow including:

* Application containerization
* Cloud deployment on AWS
* Reverse proxy configuration
* Automated CI/CD pipeline
* Production-ready system architecture



---

