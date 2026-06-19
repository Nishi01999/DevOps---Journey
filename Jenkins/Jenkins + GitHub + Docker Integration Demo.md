
Objective
Integrate Jenkins with GitHub, pull source code automatically, build a Docker image, and deploy a container.

Step 1: Install Git
sudo apt update
sudo apt install git -y
Verify installation:
git --version
Expected Output:
git version 2.x.x

Step 2: Verify Docker
docker --version
Check Docker service:
sudo systemctl status docker
Start Docker if required:
sudo systemctl start docker
sudo systemctl enable docker

Step 3: Give Jenkins Permission to Access Docker
Add Jenkins user to Docker group:
sudo usermod -aG docker jenkins
Restart services:
sudo systemctl restart docker
sudo systemctl restart jenkins
Verify:
sudo -u jenkins docker ps
No permission errors should appear.

Step 4: Open Jenkins
Access Jenkins:
http://SERVER-IP:8080
or
http://localhost:8080
Login to Jenkins.

Step 5: Create Freestyle Job
Navigate to:
Dashboard
   →
New Item
   →
github-demo-job
   →
Freestyle Project
   →
OK

Step 6: Configure GitHub Repository
Scroll to:
Source Code Management
Select:
Git
Repository URL:
https://github.com/nishanttotla/DockerStaticSite.git
Branch:
*/master

Step 7: Configure Build Step
Navigate to:
Build
   →
Add Build Step
   →
Execute Shell
Paste:
echo "Code downloaded successfully"

pwd

ls -la

sudo docker build -t myweb:v1 .
Click:
Save

Step 8: Execute Build
Click:
Build Now
Open:
Build History
   →
#1
   →
Console Output
Expected Output:
Fetching from GitHub
Checking out source code
Successfully built
Successfully tagged myweb:v1
_______________________________________________________________________________
got failed attempt beacuse sudo was used. This is just the demo and for practice
Failed attemp show this log:
<img width="862" height="152" alt="image" src="https://github.com/user-attachments/assets/c13efe63-1e64-41ee-976c-1b01cf8bbe39" />

 
successful attemp show:

<img width="605" height="213" alt="image" src="https://github.com/user-attachments/assets/a373f43c-4f61-4ba9-8f2c-dd863ae6af4f" />

________________________________________________________________________________
Step 9: Verify Docker Image
On Jenkins server:
docker images
Expected:
REPOSITORY   TAG
myweb        v1
<img width="1517" height="62" alt="image" src="https://github.com/user-attachments/assets/403bd984-63f5-4780-bcdc-5bece15b1b55" />


Step 10: Deploy Container
Run:
docker run -d --name myweb -p 9090:80 myweb:v1
Verify:
docker ps
Expected:
CONTAINER ID
IMAGE
myweb:v1
0.0.0.0:9090->80/tcp

<img width="1580" height="150" alt="image" src="https://github.com/user-attachments/assets/cfa67f35-d111-4f47-8784-5536897e6d15" />


Step 11: Access Application
Open browser:
http://SERVER-IP:9090
or
http://localhost:9090
Expected Page:
Hello from Docker
_____________________________________________________________
Troubleshooting

1.Container Name Already Exists
  docker rm -f myweb

Re-run:
docker run -d --name myweb -p 9090:80 myweb:v1

2.Port Already In Use
Check:
sudo ss -tulpn | grep 9090
Use another port:
docker run -d --name myweb -p 9091:80 myweb:v1

3.Jenkins Docker Permission Error
  sudo usermod -aG docker jenkins

  sudo systemctl restart docker

  sudo systemctl restart jenkins

End-to-End Flow
Developer ↓ Push Code ↓ GitHub Repository ↓ Jenkins Job Triggered ↓ Source Code Downloaded ↓ Docker Image Built ↓ Container Created ↓ Application Deployed ↓ Users Access Application
