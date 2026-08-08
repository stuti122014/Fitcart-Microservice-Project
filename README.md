🚀 CI/CD Deployment of a 3-Tier Application on Azure Kubernetes Service (AKS)
📖 Project Overview

This project demonstrates a production-ready CI/CD pipeline for deploying a cloud-native 3-tier application on Azure Kubernetes Service (AKS). The complete infrastructure is provisioned using Terraform, while GitHub Actions automates the build, test, containerization, and deployment process using Helm.

The solution follows modern DevOps best practices by automating infrastructure provisioning, container image management, and Kubernetes deployments — with traffic routed into the cluster via Azure Application Gateway Ingress Controller (AGIC).
<img width="1160" height="648" alt="image" src="https://github.com/user-attachments/assets/07c47712-3104-4cf8-aa61-689f4fa2d271" />

**CI Workflow**

**git → github → github-actions → build → build-docker-image → Helm Chart / Manifests Repo**

Git – Developers commit and push code changes.
GitHub – Central source code repository.
GitHub Actions – CI pipeline triggers automatically on push/PR.
Build – Application code is compiled/tested.
Build Docker Image – Application is containerized into a Docker image.
Helm Chart / Manifests Repo – Updated Helm charts / Kubernetes manifests are pushed to the manifests repository, referencing the new image tag.

📦 CD Workflow
**Container Registry (Pre-existing Artifacts) → Azure AKS → AGIC → Pods**

Container Registry – Stores pre-existing frontend and backend Docker images.
Azure AKS – The Kubernetes cluster pulls the required images from the registry and deploys them as pods.
AGIC Controller Pod – Watches Kubernetes Ingress resources and automatically configures the Application Gateway to route traffic to the correct backend pods.
Application Gateway (AGIC-enabled) – Acts as the L7 load balancer / entry point, receiving external traffic from the Internet and forwarding it to AKS.
Pods – Frontend and backend application pods running inside AKS serve the actual application traffic.

🌐 Traffic Flow
**Internet → Application Gateway (AGIC-enabled) → AKS → AGIC Controller Pod → Pods**

External users hit the Application Gateway public endpoint, which routes requests through AGIC to the appropriate pods running inside the AKS cluster.

🧰 **Tech Stack**

Layer	Technology
Infrastructure as Code -	Terraform
Source Control	- Git / GitHub
CI/CD	- GitHub Actions
Containerization -	Docker
Container Registry -	Azure Container Registry (ACR)
Orchestration -	Azure Kubernetes Service (AKS)
Ingress / Load Balancing -	Application Gateway Ingress Controller (AGIC)
Package Management -	Helm

📂 **Repository Structure**
.
├── infra/                     # Terraform code for provisioning Azure resources
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── modules/
│       ├── aks/
│       ├── application-gateway/
│       ├── networking/
│       └── acr/
├── app/                        # Application source code (frontend & backend)
│   ├── frontend/
│   └── backend/
├── helm/                       # Helm charts / Kubernetes manifests
│   ├── frontend/
│   └── backend/
├── .github/
│   └── workflows/               # GitHub Actions CI/CD pipeline definitions
│       ├── ci.yml
│       └── cd.yml
└── README.md

⚙️ Prerequisites
Azure subscription with sufficient permissions to create AKS, Application Gateway, ACR, and networking resources
Terraform  - Terraform v1.15.8
Azure CLI
Helm 
kubectl
A GitHub repository with Actions enabled and required secrets configured

🔐 Required GitHub Secrets
 Secret Name	     Description
AZURE_CREDENTIALS	  Service principal credentials for Azure login
ACR_LOGIN_SERVER   	Azure Container Registry login server URL
ACR_USERNAME	      ACR username
ACR_PASSWORD	      ACR password
AKS_CLUSTER_NAME	  Name of the target AKS cluster
AKS_RESOURCE_GROUP	Resource group containing the AKS cluster

🚀 Getting Started
1. Provision Infrastructure with Terraform
cd infra
terraform init
terraform plan -out=tfplan
terraform apply tfplan

**This provisions:**

Resource Group
Virtual Network & Subnets
AKS Cluster (with AGIC add-on enabled)
Application Gateway
Azure Container Registry
Required RBAC role assignments for AGIC to manage Application Gateway

2.** Configure kubectl**
   az aks get-credentials --resource-group dev-nitor-rg-002 --name dev-nitor-cluster-01

3.**CI Pipeline (GitHub Actions)**

On every push to main/feature branches, the CI workflow:

Builds and tests the application
Builds Docker images for frontend and backend
Pushes images to Azure Container Registry
Updates the Helm chart/manifests repo with the new image tag   

 4. **CD Pipeline (GitHub Actions / Helm)**

The CD workflow deploys the updated Helm charts to AKS: 

helm upgrade --install frontend ./helm/frontend -n dev-curd-app
helm upgrade --install backend ./helm/backend -n dev-curd-app

Ingress resources are picked up automatically by the AGIC controller pod, which configures the Application Gateway to route traffic to the new pods.

5. **Access the Application**

Once deployed, the application is accessible the Application Gateway:

kubectl get ingress -n dev-crud-app

✅ Key Features
Fully automated CI/CD pipeline using GitHub Actions
Infrastructure as Code with Terraform for repeatable, version-controlled deployments
AGIC-enabled Application Gateway for native Azure L7 ingress without an extra load balancer
Helm-based deployments for versioned, rollback-friendly releases
Clear separation of CI (build & package) and CD (deploy) responsibilities

📌 Notes
AGIC add-on requires the AKS cluster to use Azure CNI (Node Subnet) networking; it is not supported with Azure CNI Overlay.
Ensure the AGIC identity has the required RBAC role (Contributor) on the Application Gateway resource for it to manage listeners, routing rules, and backend pools.

**🤝 Contributing**

Contributions are welcome. Please open an issue or submit a pull request for any improvements.






