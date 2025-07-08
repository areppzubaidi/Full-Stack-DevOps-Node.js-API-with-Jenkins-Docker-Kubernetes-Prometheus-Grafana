# 🚀 Full-Stack DevOps: Node.js API with Jenkins, Docker, Kubernetes, Prometheus & Grafana

This project demonstrates how to build, test, containerize, monitor, and deploy a Node.js + Express API using:

- 🐳 Docker
- ☸️ Kubernetes
- 🤖 Jenkins (CI/CD)
- 📈 Prometheus & Grafana (Monitoring)

Perfect for DevOps learners and practical cloud engineers.

---

## 🛠 Tech Stack

| Tool              | Purpose                             |
|-------------------|--------------------------------------|
| Node.js + Express | Backend API                         |
| Docker            | Containerization                    |
| Jenkins           | CI/CD pipeline                      |
| Kubernetes        | App deployment and service orchestration |
| Prometheus        | Metric collection                   |
| Grafana           | Metric visualization                |

---

## 🗂 Project Structure

```

simple-node-api/
├── Jenkinsfile                # Jenkins pipeline config
├── k8s/
│   ├── deployment.yaml        # App Deployment
│   ├── service.yaml           # App Service
│   ├── prometheus-config.yaml# Prometheus targets
│   ├── prometheus-deploy.yaml# Prometheus Deployment
│   └── grafana-deploy.yaml   # Grafana Deployment
├── src/
│   └── index.js              # Express app + metrics
├── Dockerfile
├── .dockerignore
├── package.json
└── README.md

````

---

## ⚙️ Jenkins CI/CD Pipeline

### 📄 Jenkinsfile

This file defines the stages of your Jenkins pipeline:

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "simple-node-api"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/simple-node-api.git'  // Replace with your repo
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} $DOCKER_USER/${DOCKER_IMAGE}:${DOCKER_TAG}
                        docker push $DOCKER_USER/${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/'
            }
        }
    }
}
````

> 🔒 You'll need to create DockerHub credentials (`dockerhub-creds`) in Jenkins

---

## 🐳 Docker Guide

```bash
docker build -t simple-node-api .
docker run -p 3000:3000 simple-node-api
```

---

## ☸️ Kubernetes Guide

```bash
kubectl apply -f k8s/
minikube service simple-node-service
```

---

## 📈 Monitoring with Prometheus & Grafana

* `/metrics` endpoint exposed using `prom-client`
* Scraped by Prometheus (`k8s/prometheus-config.yaml`)
* Visualized in Grafana

```bash
minikube service grafana  # Login: admin/admin
```

---

## 🧪 Sample Metric Output

```text
# HELP node_app_requests_total Total number of requests
# TYPE node_app_requests_total counter
node_app_requests_total 8
```

---

## 📦 What's Next?

* Add liveness/readiness probes
* Set up Helm chart
* Integrate Slack/email alerts in Jenkins
* Use Prometheus Alertmanager for notification

---

## 👤 Author

**Nur Ariff**
GitHub: [@your-username](https://github.com/your-username)
Portfolio: [nur-ariff.dev](https://nur-ariff.dev)

---

## 📄 License

MIT – Free to use and distribute.

```

---

## ✅ Recap of Action Items

| Task                     | Status     |
|--------------------------|------------|
| Replace GitHub Actions   | ✅ Jenkinsfile added  
| Update README             | ✅ Jenkins, Docker, K8s, Monitoring  
| DockerHub push step      | ✅ Included in Jenkinsfile  
| Kubernetes deploy step   | ✅ Included in Jenkinsfile  

---

Would you like me to:
- Zip the entire Jenkins-ready project?
- Add a Jenkins dashboard screenshot example?
- Help set up Jenkins inside Kubernetes using Helm?

Let’s make this project fully enterprise-ready.
```
