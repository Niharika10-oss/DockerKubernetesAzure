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












