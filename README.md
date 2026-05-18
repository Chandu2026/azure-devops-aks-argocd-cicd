# azure-devops-aks-argocd-cicd
End-to-end CI/CD project using Azure DevOps, Docker, ACR, AKS, Kubernetes, and ArgoCD.
# End-to-End CI/CD Project using Azure DevOps, AKS, ACR, and ArgoCD

## Project Overview

This project demonstrates a complete CI/CD workflow using Azure DevOps for Continuous Integration (CI) and ArgoCD for Continuous Delivery (CD) on Azure Kubernetes Service (AKS).

The application is containerized using Docker, images are stored in Azure Container Registry (ACR), and deployments are managed through Kubernetes manifests using ArgoCD.

---

# Project Architecture

GitHub → Azure DevOps → Docker → Azure Container Registry (ACR) → AKS → ArgoCD → Kubernetes Deployment

---

# Technologies Used

* Azure DevOps
* Docker
* Azure Container Registry (ACR)
* Azure Kubernetes Service (AKS)
* Kubernetes
* ArgoCD
* GitHub

---

# Complete Project Flow

## Step 1: Source Code Management

* Application source code stored in GitHub repository
* Kubernetes manifest files maintained in repository
* Azure DevOps connected with GitHub repository

---

# CI Process using Azure DevOps

## Step 2: Azure DevOps Pipeline Setup

* Created Azure DevOps YAML pipeline
* Configured self-hosted agent
* Connected pipeline with GitHub repository

---

## Step 3: Docker Image Build

Azure DevOps pipeline automatically builds Docker image after code changes.

Example Docker build task:

```yaml id="8hx2vr"
- task: Docker@2
  displayName: Build and Push Image
  inputs:
    command: buildAndPush
    repository: 'resultapp'
    containerRegistry: '$(dockerRegistryServiceConnection)'
    Dockerfile: '$(Build.SourcesDirectory)/result/Dockerfile'
    buildContext: '$(Build.SourcesDirectory)/result'
    tags: |
      $(Build.BuildId)
```

---

## Step 4: Push Docker Image to ACR

* Successfully pushed Docker image to Azure Container Registry
* Verified image storage in ACR

---

# AKS Setup Process

## Step 5: Connect to AKS Cluster

Connected AKS cluster using Azure CLI:

```bash id="uzwsnm"
az login
```

```bash id="pm8u7g"
az aks get-credentials \
--name azuredevops \
--resource-group azurecicd \
--overwrite-existing
```

Verified AKS connection:

```bash id="0vv6h6"
kubectl get nodes
```

---

# ArgoCD Installation and Configuration

## Step 6: Create ArgoCD Namespace

```bash id="s79yvb"
kubectl create namespace argocd
```

---

## Step 7: Install ArgoCD

```bash id="0k2w7f"
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verified ArgoCD pods:

```bash id="v4ow7s"
kubectl get pods -n argocd
```

---

## Step 8: Expose ArgoCD Server

Changed ArgoCD service type to LoadBalancer:

```bash id="y0sxk8"
kubectl patch svc argocd-server -n argocd \
-p '{"spec": {"type": "LoadBalancer"}}'
```

Verified external IP:

```bash id="jlwm4p"
kubectl get svc -n argocd
```

---

## Step 9: Access ArgoCD UI

Retrieved ArgoCD admin password:

```bash id="36st5l"
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d
```

Successfully accessed ArgoCD dashboard using external LoadBalancer IP.

---

# CD Process using ArgoCD

## Step 10: Configure Application in ArgoCD

* Connected GitHub repository to ArgoCD
* Configured Kubernetes manifests
* Enabled synchronization between GitHub and AKS

---

## Step 11: Application Deployment

ArgoCD continuously monitors GitHub repository and deploys application changes automatically to AKS.

Verified deployments:

```bash id="tt4r5k"
kubectl get deployments
```

Verified pods:

```bash id="t1hl4x"
kubectl get pods
```

---

# Challenges Faced During the Project

During implementation, several real-time issues were encountered and resolved:

* Docker permission denied issue on self-hosted agent
* Azure CLI login problems
* AKS DNS and connectivity issues
* Azure DevOps pipeline build failures
* YAML pipeline debugging
* Kubernetes namespace troubleshooting
* ArgoCD LoadBalancer exposure issues
* TLS certificate warnings while accessing ArgoCD
* Service configuration and connectivity troubleshooting

---

# Final Outcome

Successfully implemented:

✅ CI pipeline using Azure DevOps
✅ Docker image build and push to ACR
✅ AKS cluster deployment
✅ CD setup using ArgoCD
✅ Kubernetes application synchronization
✅ End-to-end CI/CD workflow

---

# Verification Commands

## Verify AKS Nodes

```bash id="gct6sh"
kubectl get nodes
```

## Verify ArgoCD Pods

```bash id="kuxrr0"
kubectl get pods -n argocd
```

## Verify ArgoCD Services

```bash id="0j0v1h"
kubectl get svc -n argocd
```

## Verify Deployments

```bash id="fcwbv8"
kubectl get deployments
```

---

# Learning Outcome

This project improved practical understanding of:

* CI/CD workflows
* Docker containerization
* Azure DevOps pipelines
* Kubernetes deployments
* AKS cluster management
* ArgoCD Continuous Delivery
* GitOps workflow
* Kubernetes services and networking

---

# Future Improvements

* Helm chart integration
* Monitoring using Prometheus and Grafana
* Ingress controller setup
* SSL certificate configuration
* Multi-environment deployment
* Automated rollback strategy

---

# Screenshots Included

* Azure DevOps successful pipeline
<img width="1912" height="1077" alt="Screenshot 2026-05-17 180408" src="https://github.com/user-attachments/assets/17f5b060-b631-407f-b9f8-2b22ae454ef1" />
<img width="1918" height="1078" alt="Screenshot 2026-05-17 234901" src="https://github.com/user-attachments/assets/bc837fce-1036-4185-8a92-0e056628330d" />
<img width="1918" height="1078" alt="Screenshot 2026-05-17 234915" src="https://github.com/user-attachments/assets/839bdcdc-d061-40f0-9b03-897e89e4090a" />
<img width="1918" height="1078" alt="Screenshot 2026-05-17 234928" src="https://github.com/user-attachments/assets/b28c0ca7-501b-4504-a812-cb49030cfde1" />
* ArgoCD dashboard
<img width="1917" height="1042" alt="Screenshot 2026-05-17 230430" src="https://github.com/user-attachments/assets/536c7dd2-a432-4c78-b969-21fdb65e4a96" />
<img width="1900" height="1007" alt="Screenshot 2026-05-17 232354" src="https://github.com/user-attachments/assets/ed4f1eb1-40cc-48e6-919f-99287cd9281b" />
* Kubernetes pods and services
<img width="977" height="786" alt="Screenshot 2026-05-17 235352" src="https://github.com/user-attachments/assets/1411b70f-94c8-4b73-847a-72b8b795e8a8" />
* Deployment verification outputs
<img width="1915" height="1073" alt="Screenshot 2026-05-17 234652" src="https://github.com/user-attachments/assets/44fef8da-e5e0-4f12-bf4e-e03ce2a045c4" />
* LoadBalancer external access
<img width="1852" height="925" alt="Screenshot 2026-05-17 235413" src="https://github.com/user-attachments/assets/383e8519-326d-4ebe-8cf4-c81a706746de" />
* Successful application synchronization
<img width="1917" height="1073" alt="Screenshot 2026-05-17 234643" src="https://github.com/user-attachments/assets/b859bb6c-db0d-4f2b-a63a-d9a297e24883" />

# Note

This project was implemented for learning and hands-on practice purposes by following DevOps/Kubernetes learning resources and tutorials.

I personally configured the CI/CD workflow, worked on troubleshooting issues, deployed the application on AKS, and integrated ArgoCD for Continuous Delivery to improve my practical DevOps skills.
