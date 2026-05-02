Repo link: Docker Hub – https://hub.docker.com/repository/docker/niharikakolarkar/sample-html-app/general
⚙️ Environment Preparation
1- Before building and running this project, make sure your environment is ready: Install Docker Desktop. Download from Docker official site (docker.com in Bing).
Verify installation: docker --version
docker info
2- Check Disk Space: Ensure at least 5 GB free on your system drive (C:). 
Get-PSDrive C
3- Verify Docker Engine: docker info
4- Test Connectivity to Docker Hub: docker pull hello-world
5- Configure Registry Mirror (if needed): If you see HTTPS errors when pulling images, add a mirror in Docker Desktop → Settings → Docker Engine:
{
  "registry-mirrors": ["https://mirror.gcr.io"]
}
this will bypass n/w interferences.

🛠 Build & Run
docker build -t sample-html-app:v1 .
docker run -d -p 8080:80 sample-html-app:v1
Open in browser: http://localhost:8080

# 🚀 Docker → Azure Container Registry (ACR)
This mini‑lab demonstrates how to build a Docker image locally and publish it to **Azure Container Registry (ACR)** for cloud deployment.
### 1. Create Resource Group - az group create --name <YourRESOURCEgrpName> --location centralindia
### 2. Create an ACR - az acr create --resource-group MyResourceGroup --name <yourUniqueRegistryName> --sku Basic
### 3. az acr login --name YourRegistryName
### 4. Here’s the correct single‑line format you should run in your local PowerShell terminal: 
az acr import --name <YourRegsitryName> --source docker.io/niharikakolarkar/sample-html-app:v1 --image sample-html-app:v1
I used this as due to n/w issue i wasnt able to tag and push large image. This uses image from docker hub and stores in ACR.
### 5. Verify if image tagged and pushed 
az acr repository show-tags --name <YourRegsitryName> --repository sample-html-app --output table

## ☸️ Deploy to AKS (with ACR Integration)
CREATE AKS CLUSTER
az aks create --resource-group MyResourceGroup --name MyAKSCluster101199 --node-count 1 --generate-ssh-keys
CONNECT KUBECTL TO ACR
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster101199
Navigate to your project folder (where you want to keep the manifest) and create yaml file 
notepad sample-html-app.yaml
Paste the YAML content (Deployment + Service):
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-html-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sample-html-app
  template:
    metadata:
      labels:
        app: sample-html-app
    spec:
      containers:
      - name: sample-html-app
        image: niharikaacr101199.azurecr.io/sample-html-app:v1
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: sample-html-app-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: sample-html-app
APPLY THE YAML FILE
kubectl apply -f sample-html-app.yaml
Verify Pod Status is running
kubectl get pods
Check Service External IP
kubectl get service <Name In Service Block of YAML>
check external IP if app is accessible
### IF PODS ARENT RUNNING AND EXTERNAL IP NOT REACHABLE
1. **Attach ACR to AKS**
   ```powershell
az aks update -n MyAKSCluster101199 -g MyResourceGroup --attach-acr niharikaacr101199
2. Redeploy Pods
kubectl delete pod -l app=sample-html-app
kubectl apply -f sample-html-app.yaml
3. Verify Pod Status is running
kubectl get pods 
4. Check Service External IP
kubectl get service <Name In Service Block of YAML>
###✅ Outcome
1. AKS cluster successfully connected to ACR (niharikaacr101199).
2. Pods redeployed and running with image sample-html-app:v1.
3. Application exposed via LoadBalancer service and reachable at external IP.

🚀 Pipeline Automation (Azure DevOps) 
end‑to‑end CI/CD automation using Azure DevOps pipelines to build, push, and deploy a containerized web app on AKS.
Build & Push Stage
1. Automated Docker image build from a minimal nginx‑based Dockerfile.
2. Resolved push failures to Azure Container Registry by switching to Docker Hub as large images were getting blocked by n/w.
3. Configured a secure Docker Hub service connection in Azure DevOps.
Deploy Stage
1. Applied Kubernetes manifests directly from the pipeline.
2. Fixed deployment errors (azureSubscriptionEndpoint missing) by creating a manual Azure Resource Manager service connection with a service principal.
3. Verified successful rollout with a public LoadBalancer IP (http://20.207.113.205).














