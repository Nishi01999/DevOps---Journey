
 Docker Images & Dockerfile


🧩 Task 1: Pull and Run an Existing Image
📝 Description
In this task, you will download an image from Docker Hub and run it.
▶️ Steps
docker pull nginx
docker run nginx
✅ Expected Result
Image gets downloaded
Container starts running

🔍 Verify
docker ps


🎯 Learning Outcome
Understand difference between:
Image (downloaded)
Container (running)


🧩 Task 2: Create First Dockerfile

▶️ Step 1: Create a file
echo "Hello from Lab!" > hello.txt

▶️ Step 2: Create Dockerfile
FROM ubuntu
COPY hello.txt /app/
CMD ["cat", "/app/hello.txt"]

▶️ Step 3: Build Image
docker build -t labapp .

▶️ Step 4: Run Container
docker run labapp

✅ Expected Output
Hello from Lab!

🎯 Learning Outcome
Build your own image
Run container from custom image

🧩 Task 3: Modify Output
Change the message and rebuild.

▶️ Step
Edit file:
echo "Nishi modified her Docker image!" > hello.txt
Rebuild:
docker build -t labapp .
docker run labapp

✅ Expected Output
Nishi modified her Docker image!

🎯 Learning Outcome
Understand rebuilding images



🧩 Task 4: NGINX Web Application (No Coding)
Run a web page using Docker.

▶️ Step 1: Create HTML file
<h1>Welcome to Docker Lab 👋</h1>
Save as:
index.html

▶️ Step 2: Create Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/index.html

▶️ Step 3: Build Image
docker build -t myweb .

▶️ Step 4: Run Container
docker run -p 3000:80 myweb

🌍 Open Browser
http://localhost:3000

✅ Expected Output
Web page displaying:
Welcome to Docker Lab 👋

🎯 Learning Outcome
Deploy application using Docker
Port mapping understanding

🧩 Task 5: Build vs Pull
📝 Description
Understand difference practically.

▶️ Commands
docker pull ubuntu
docker build -t customapp .

🎯 Observation
Action
Meaning
pull
Download image
build
Create image


🧩 Task 6: List and Manage Images
▶️ Commands
docker images
docker rmi labapp

🎯 Learning Outcome
Manage Docker images

🔍 Troubleshooting:
If build fails → check Dockerfile name (must be Dockerfile)
If port not working → check container is running
If permission error → use sudo (Linux)
basics

