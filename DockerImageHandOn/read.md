Step 1 — Login to Docker Hub

docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: abhishekf5
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

Step 2 — Write a Dockerfile
   git clone mygitrepo link
   ls
   cd DockerImageHandOn
   ls
   cat app.py
   vim dockerFile

What Each Line Does

FROM ubuntu:latest

# Set the working directory in the image
WORKDIR /app

# Copy the files from the host file system to the image file system
COPY . /app

# Install the necessary packages
RUN apt-get update && apt-get install -y python3 python3-pip

# Set environment variables
ENV NAME World

# Run a command to start the application
CMD ["python3", "app.py"]
~

Step 3 — Build the Image
docker build -t knishi1999/my-first-doc-image:v1 .
The . at the end means "look for Dockerfile in current directory"
Latest : tag to unify docker image id.
Yourusername: username of docker from which we logged in
my-first-doc-image: this is the repo that we will use.

Inside your username there will be a repo that is going to be created.

got the output called hello world now this is a just a command line application so that's why you got output as hello world.

 what if this is a web application so what happens is your application will start running on this specific machine and you can access the application from your easy to instance or from your browser. App will be live on your browser.



Step 4 — Verify Image was Created
docker images
Output:
REPOSITORY                                              TAG       IMAGE ID       SIZE
yourusername/my-first-docker-image      latest  960d37536dcd  467MB
ubuntu                                                           latest    58db3edaf2be   77.8MB
hello-world                                                   latest    feb5d9fea6a5   13.3kB
knishi1999/my-first-doc-image:v1
                            2b04720f6fef        849MB          225MB

Step 5 — Run a Container from Your Image
docker run -it knishi1999/my-first-doc-image:v1
Output:
Hello World


Step 6 — Push Image to Docker Hub

nishikush@Nishi:~/DevOps---Journey/DockerImageHandOn$ docker push knishi1999/my-first-doc-image:v1
________________________________________________________
<img width="1527" height="262" alt="image" src="https://github.com/user-attachments/assets/843028fe-ada6-494b-a314-d13448c61bdd" />
<img width="1572" height="157" alt="image" src="https://github.com/user-attachments/assets/ba226036-a036-4320-a961-2bbac009a878" />


