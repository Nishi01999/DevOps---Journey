Jenkins + GitHub + Docker Integration

Objective
	Integrate Jenkins with GitHub, pull source code automatically, build a Docker image,
	and deploy a container.


Step 1: Install Git
	sudo apt update
	sudo apt install git -y

Verify installation:
	git --version

Expected Output:
	git version 2.x.x

------------------------------------------------------------------------

Step 2: Verify Docker
	docker --version

Check Docker service:
	sudo systemctl status docker

Start Docker if required:
	sudo systemctl start docker
	sudo systemctl enable docker


------------------------------------------------------------------------


Step 3: Give Jenkins Permission to Access Docker

Add Jenkins user to Docker group:
	sudo usermod -aG docker jenkins

Restart services:
	sudo systemctl restart docker
	sudo systemctl restart jenkins


Verify:
	sudo -u jenkins docker ps


No permission errors should appear.

<img width="952" height="117" alt="image" src="https://github.com/user-attachments/assets/d96ee1ec-4711-4828-a509-fdb7bd7ed162" />



How to correct this error:

Jenkins Docker Permission Error
	sudo usermod -aG docker jenkins
	sudo systemctl restart docker
	sudo systemctl restart jenkins



<img width="857" height="185" alt="image" src="https://github.com/user-attachments/assets/f6edac45-82cd-4d06-a0ac-66cb75be0ad9" />

Why Did Jenkins Fail?
When Jenkins runs a job, it runs as the Linux user:

You tested this by running:
	sudo -u jenkins docker ps

Meaning:
	"Run docker ps as Jenkins user"

Docker replied:
Permission Denied ❌

because:
jenkins user
      ↓
Not a member of docker group
      ↓
Cannot access docker.sock


------------------------------------------------------------------------

Step 4: Open Jenkins
Access Jenkins:
http://SERVER-IP:8080
or
http://localhost:8080
Login to Jenkins.



Learning
Docker commands communicate through:
	/var/run/docker.sock

Only users with Docker permissions can access this socket. Adding Jenkins to the Docker group allows Jenkins jobs to execute Docker commands successfully.


other troubleshootings:

	1. Container Name Already Exists
docker rm -f myweb
Re-run:
docker run -d --name myweb -p 9090:80 myweb:v1

________________________________________________________________________________________


	2. Port Already In Use
Check:
sudo ss -tulpn | grep 9090
Use another port:
docker run -d --name myweb -p 9091:80 myweb:v1

