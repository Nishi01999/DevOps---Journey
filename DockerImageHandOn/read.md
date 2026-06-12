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
docker build -t yourusername/my-first-docker-image:latest .
The . at the end means "look for Dockerfile in current directory"
Latest : tag to unify docker image id.
Yourusername: username of docker from which we logged in
my-first-docker-image: this is the repo that we will use.

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
Step 5 — Run a Container from Your Image
docker run -it yourusername/my-first-docker-image:latest
Output:
Hello World
Isme tag dete hai taaki docker hub me pehchan sake apne image ko. Ye ek docker image id hogi ise assigned, wo yaad rkhna mushkil hota hai isliye tag add krte hain.(latest tag hi hai)

Step 6 — Push Image to Docker Hub
docker push yourusername/my-first-docker-image:latest
	Now anyone in the world can pull and run your image with:
			docker pull yourusername/my-first-docker-image
________________________________________________________
<img width="925" height="1966" alt="image" src="https://github.com/user-attachments/assets/651707fb-3571-4783-9a4b-4cad3c2e2143" />

