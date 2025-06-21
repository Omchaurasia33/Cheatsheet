
# 🚀 React App Deployment with Jenkins & Docker (CI/CD Setup)

This guide explains how to deploy a React app using **Jenkins running in Docker**, with **CI/CD pipeline** that builds and runs a Docker container for the app.

## 📝 Prerequisites

- Docker installed
- Git installed
- Internet access to clone repo and install packages

---

## 🐳 Step 1: Run Jenkins in Docker

Run Jenkins container as root (required to use Docker inside):

```bash
docker run -d   --name jenkins   -u 0   -p 8080:8080 -p 50000:50000   -v /var/run/docker.sock:/var/run/docker.sock   -v jenkins_home:/var/jenkins_home   jenkins/jenkins:lts
```

### ✅ Notes:
- Jenkins will be available at: `http://localhost:8080`
- Mounting Docker socket allows Jenkins to run Docker commands.

---

## 🛠️ Step 2: Install Docker Inside Jenkins Container

Open a terminal to Jenkins container as root:

```bash
docker exec -u 0 -it jenkins bash
```

Then install Docker CLI:

```bash
apt-get update
apt-get install -y docker.io
```

---

## 📦 Step 3: Sample React App Repository

Use this GitHub repo (with Dockerfile) for testing:

📂 **Repo:** [https://github.com/Omchaurasia33/jenkintesting](https://github.com/Omchaurasia33/jenkintesting)

This repo contains a basic React app with a Dockerfile to build it.

---

## 🔧 Step 4: Jenkins Pipeline Script

Create a new **Pipeline Job** in Jenkins with the following script:

```groovy
pipeline {
  agent any

  environment {
    IMAGE_NAME = 'my-react-app'
    CONTAINER_NAME = 'react-app-container'
    APP_PORT = '3000' // Will be accessible at http://localhost:3000
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/Omchaurasia33/jenkintesting.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        echo "Building Docker image..."
        sh "docker build -t ${IMAGE_NAME} ."
      }
    }

    stage('Run Docker Container') {
      steps {
        echo "Running Docker container..."
        sh "docker rm -f ${CONTAINER_NAME} || true"
        sh "docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:80 ${IMAGE_NAME}"
      }
    }
  }

  post {
    always {
      echo 'Cleaning workspace...'
      cleanWs()
    }
  }
}
```

---

## 🌐 Step 5: Access Your React App

After a successful build, open:

📲 `http://localhost:3000`

Your app is now running in a Docker container!

---

## 🧹 Optional: Run React App Manually (without Jenkins)

To run the app manually:

```bash
docker run -d --name testjenkin -p 3000:80 testjenkin
```

---

## ✅ Check Running Containers

To verify everything is running:

```bash
docker ps
```

---

## 📌 Summary

| Task                        | Command / Link |
|-----------------------------|----------------|
| Run Jenkins in Docker       | `docker run -d ...` |
| Install Docker in Jenkins   | `apt-get install -y docker.io` |
| Jenkins Pipeline Repo       | [jenkintesting](https://github.com/Omchaurasia33/jenkintesting) |
| Jenkins Access              | `http://localhost:8080` |
| React App Access            | `http://localhost:3000` |

---

## 🔐 Working with Private GitHub Repos

If your repository is **private**, Jenkins running in Docker **won't have access by default** — even if your main machine is logged into GitHub.

### ❌ Why it won't work by default:
- Jenkins container is isolated and cannot access your local GitHub credentials.
- You must explicitly provide credentials **inside Jenkins**.

### ✅ Solution 1: Use a Personal Access Token (HTTPS)
1. Generate a token from GitHub with `repo` access:  
   `GitHub > Settings > Developer Settings > Personal Access Tokens`.
2. In Jenkins:
   - Go to **Manage Jenkins > Credentials > Global > Add Credentials**
   - Select `Username with password`
   - **Username:** Your GitHub username  
   - **Password:** Your GitHub token  
   - Save it with an ID like `github-token`
3. Use this in your pipeline:

```groovy
git credentialsId: 'github-token', url: 'https://github.com/YourUsername/your-private-repo.git'
```

---

### 🔑 Solution 2: Use SSH Key (Alternative)
1. Generate an SSH key pair on your machine:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

2. Add the **public key** (`id_rsa.pub`) to GitHub →  
   `Settings > SSH and GPG keys`
3. In Jenkins:
   - Go to **Manage Jenkins > Credentials > Global > Add Credentials**
   - Choose `SSH Username with private key`
   - Username: `git`
   - Private Key: Paste contents of `id_rsa`
   - ID: `github-ssh-key`
4. Use this in your pipeline:

```groovy
git credentialsId: 'github-ssh-key', url: 'git@github.com:YourUsername/your-private-repo.git'
```

---

### 🛠️ Troubleshooting

If not configured properly, you'll get errors like:

```
fatal: could not read Username for 'https://github.com': No such device or address
```

Ensure your credentials are correctly added in Jenkins and used in the `git` step.
