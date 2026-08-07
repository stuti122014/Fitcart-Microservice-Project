🚀 CI/CD Deployment of a 3-Tier Application on Azure Kubernetes Service (AKS)
📖 Project Overview

This project demonstrates a production-ready CI/CD pipeline for deploying a cloud-native 3-tier application on Azure Kubernetes Service (AKS). The complete infrastructure is provisioned using Terraform, while GitHub Actions automates the build, test, containerization, and deployment process using Helm.

The solution follows modern DevOps best practices by automating infrastructure provisioning, container image management, and Kubernetes deployments — with traffic routed into the cluster via Azure Application Gateway Ingress Controller (AGIC).
The architecture follows GitOps principles where GitHub acts as the single source of truth. Any change pushed to the repository automatically triggers CI/CD pipelines and synchronizes the desired state with AKS using Argo CD. 
<img width="1166" height="644" alt="image" src="https://github.com/user-attachments/assets/2cac700e-43d6-436e-b847-3a19bdb7ec79" />

