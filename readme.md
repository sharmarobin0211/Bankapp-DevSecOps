# 🚀 DevSecOps Mega Project – Springboot BankApp

An **end-to-end DevSecOps project** for deploying a **Spring Boot Bank Application** on **AWS EKS**, built with a modern DevSecOps toolchain including Terraform, Docker, Jenkins, ArgoCD, Helm, Prometheus, Grafana, and more.

> **Author**: [TrainWithShubham](https://github.com/sharmarobin0211/Bankapp-DevSecOps.git)
> **Open Source Contributions Welcome!**

---

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline** for a Spring Boot bank application with **DevSecOps** practices, hosted on **AWS EKS**. It covers:

- Infrastructure provisioning with **Terraform**
- Containerization with **Docker**
- Deployment automation with **ArgoCD & Helm**
- Monitoring with **Prometheus & Grafana**
- Security via **DevSecOps** tools
- Continuous Integration via **Jenkins**
- Optional custom domain + SSL setup

---

## 🛠️ Tech Stack

| Layer           | Tools/Services                            |
|----------------|--------------------------------------------|
| IaC            | Terraform                                  |
| Cloud Provider | AWS (IAM, EC2, EKS, Route53, S3)           |
| Containerization| Docker                                    |
| Orchestration  | Kubernetes (EKS)                           |
| CI/CD          | Jenkins + ArgoCD                           |
| Observability  | Prometheus + Grafana                       |
| Security       | IAM, Cert-Manager, Ingress, (DevSecOps TBD)|
| GitOps         | ArgoCD                                     |

---

## 📁 Project Structure

DevSecOps-mega-project/
├── app/ # Spring Boot bank application
├── docker/ # Dockerfile
├── terraform/ # Terraform configuration files
├── k8s/ # Kubernetes manifests (YAMLs)
├── helm-charts/ # Helm charts (optional)
├── jenkins/ # Jenkinsfile and pipeline configs
└── README.md
---

## 🚧 Setup Instructions

> **Pre-requisites**:
> - AWS account with admin access
> - DockerHub account
> - GitHub account
> - VSCode with Ubuntu terminal (WSL for Windows users)

---

### ✅ [Step-by-Step Setup Guide](#)

Refer to the complete [setup guide here](#) or check each setup step from the list below:

1. **[Create AWS IAM user](#step-1-create-an-iam-user)**
2. **[Set up VSCode with Linux Terminal](#step-2-setting-up-vscode)**
3. **[Fork & Clone the Repo](#step-3-fork-and-clone)**
4. **[Install & Configure AWS CLI](#step-4-install-aws-cli)**
5. **[Dockerize & Push the App](#step-5-build-and-push-docker-image)**
6. **[Provision Infra using Terraform](#step-6-set-up-infrastructure-with-terraform)**
7. **[Install DevOps Tools on EC2](#step-7-install-essential-tools)**
8. **[Create EKS Cluster with eksctl](#step-8-create-kubernetes-cluster)** # its optional to make eks by using eksctl or Iac(terraform)
9. **[Install ArgoCD](#step-9-set-up-argocd)** #Complete Guide for setting up a ArgoCD in (argoCD.md)
10. **[Configure Ingress, Helm & SSL](#step-10-installing-helm-ingress-controller)**
11. **[Create ArgoCD App](#step-11-creating-application-in-argocd)**
12. **[Expose App via Ingress/NodePort](#step-12-exposing-application)**
13. **[Set Up Jenkins CI](#step-13-setting-up-jenkins)**
14. **[Set Up Monitoring with Prometheus + Grafana](#step-14-setting-up-observability)**

---

## 🧪 CI/CD Pipeline

The Jenkins pipeline automates:

- Code checkout from GitHub
- Docker build & push to DockerHub
- Image tag updates in K8s YAMLs
- GitOps deployment via ArgoCD

> Webhooks are used for automated builds on code push.

---

## 🔐 DevSecOps Practices (To Be Integrated)

Planned or in-progress security measures:

- Image vulnerability scanning (Trivy, Clair)
- Dependency checks (OWASP Dependency-Check)
- CI/CD security gates (SonarQube, ZAP)
- Role-based access control in EKS

---

## 📊 Observability

With **Prometheus + Grafana** stack, you can:

- Monitor cluster and app metrics
- Visualize resource usage
- Set alerts (if needed)
- Track HPA activity and pod scaling behavior

---

## 🌐 Accessing the Application

- **Via NodePort**:  
  `http://<worker-node-public-ip>:<nodeport>`

- **Via Ingress + ALB + Domain**:  
  `https://your-custom-domain.com`

---

### 🔍 Areas to Contribute

- 🔒 Fix login issues post domain mapping
- 🔐 Add full **DevSecOps** automation
- ⚙️ Optimize deployment/monitoring
- 🧹 Refactor manifests or add Helm charts
- 📖 Improve documentation

---

## 🏁 Final Thoughts

This project is a great way to showcase your practical DevOps skills, whether you're a beginner looking to learn or a professional aiming to polish your portfolio.

> **Like it? Star it ⭐ | Fork it 🍴 | Contribute 🤝**

---

## 📬 Contact

**TrainWithShubham**  
- GitHub: [@sharmarobin0211](https://github.com/sharmarobin0211/Bankapp-DevSecOps.git)   
- YouTube: [TrainWithShubham](https://youtube.com/@TrainWithShubham)

---

## 📜 License

This project is licensed under the **MIT License**.

---

> **_The project ends here. But your learning journey continues..._**

