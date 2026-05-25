# Flask CI/CD Web Application

A containerized Python Flask application with an automated CI/CD pipeline using GitHub Actions, Docker Hub, and deployment to a Linux Ubuntu VPS.

## Project Overview

This repository demonstrates a complete, automated modern DevOps pipeline:
1. **Continuous Integration (CI)**: Checks out the code on push to `main`, runs unit tests with `pytest`, builds a Docker image, and pushes it to Docker Hub.
2. **Continuous Deployment (CD)**: Connects to a Linux Ubuntu VPS via SSH and deploys/updates the containerized Flask application using Docker.

## Prerequisites

Before setting up the pipeline, ensure you have:
- **Docker** installed locally (for testing).
- A **GitHub Account** (to host the repository and run actions).
- A **Docker Hub Account** (to host the built Docker image repository).
- An **Ubuntu Linux VPS** (with Docker installed and port 80 open).

## GitHub Secrets Required

You must configure the following Action Secrets in your GitHub repository settings under `Settings` -> `Secrets and variables` -> `Actions` -> `New repository secret`:

| Secret | Description |
| :--- | :--- |
| `DOCKER_USERNAME` | Your Docker Hub username |
| `DOCKER_PASSWORD` | Your Docker Hub password or Personal Access Token (PAT) |
| `SERVER_HOST` | The IP address or domain name of your Linux Ubuntu VPS |
| `SERVER_USERNAME` | The SSH user on your VPS (e.g., `ubuntu` or `root`) |
| `SERVER_SSH_KEY` | The private SSH key content used to authenticate with the VPS |

### 1. Generating SSH Key
To generate a secure SSH key pair, run the following command in your terminal:
```bash
ssh-keygen -t rsa -b 4096
```

### 2. Copying Public Key to Server
To authorize the generated SSH key on your VPS, copy the public key using:
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server-ip
```
*(Replace `user` with your VPS username and `server-ip` with your VPS host IP address. Then, add the contents of the private key `~/.ssh/id_rsa` as the `SERVER_SSH_KEY` secret in GitHub.)*

## Local Development and Docker Commands

To build and run the Flask application locally using Docker:

### Build the Docker Image
```bash
docker build -t flask-cicd-app .
```

### Run the Docker Container
```bash
docker run -p 5000:5000 flask-cicd-app
```
Once started, the application will be accessible at [http://localhost:5000](http://localhost:5000).
