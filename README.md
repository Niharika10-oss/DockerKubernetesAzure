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











