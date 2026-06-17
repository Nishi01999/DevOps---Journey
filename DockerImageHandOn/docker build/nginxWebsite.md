
# Docker Hands-On: Building and Running a Simple Nginx Website

## Objective

Create a custom Docker image using Nginx and serve a simple HTML page.

Expected Output:
http://localhost:3000

Hello from Docker
```

---

# Step 1: Create index.html

Create a file named `index.html`.

Contents:

```html id="vv6wna"
<h1>Hello from Docker</h1>
```

Verify:

```bash id="az9jr9"
cat index.html
```

Output:

```html id="v2dgsu"
<h1>Hello from Docker</h1>
```

---

# Step 2: Create Dockerfile

Create a file named:

```text id="hmj5s8"
Dockerfile
```

Contents:

```dockerfile id="9ym30s"
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

### Explanation

#### FROM nginx

Pulls the official Nginx image from Docker Hub.

#### COPY

Copies our custom HTML page into Nginx's default web directory.

```text id="4zmml8"
/usr/share/nginx/html/index.html
```

When Nginx receives a request, it serves this file.

---

# Common Mistake

I accidentally wrote:

```dockerfile id="zhlt0y"
COPY index.html /usr/shar/nginx/html/index.html
```

Notice:

```text id="49rtsn"
shar
```

instead of:

```text id="ggjdcl"
share
```

Result:

* Docker copied the file to the wrong location.
* Nginx continued serving its default welcome page.

Lesson:

> Always verify paths used in the Dockerfile.

---

# Step 3: Build the Image

Build the image:

```bash id="af8h74"
docker build -t myweb .
```

Explanation:

```text id="8s86lv"
docker build   → Create image
-t myweb       → Tag image as myweb
.              → Current directory as build context
```

Verify:

```bash id="8uvv2e"
docker images
```

Expected:

```text id="yx3ycm"
myweb   latest
```

---

# Step 4: Run the Container

Start a container:

```bash id="rx9mxn"
docker run -d -p 3000:80 --name myweb-container myweb
```

### Explanation

#### docker run

Creates and starts a container.

#### -d

Runs container in background (detached mode).

#### -p 3000:80

Maps:

```text id="7d95hk"
Host Port      Container Port
3000     --->      80
```

Browser Request Flow:

```text id="nrz6r7"
Browser
   ↓
localhost:3000
   ↓
Docker Host
   ↓
Container Port 80
   ↓
Nginx
```

#### --name myweb-container

Assigns a custom container name.

#### myweb

Image name used to create the container.

---

# Step 5: Verify Container

Check running containers:

```bash id="3r5sbo"
docker ps
```

Expected:

```text id="n8hb8e"
myweb-container
```

---

# Step 6: Check Logs

View container logs:

```bash id="eh3h4k"
docker logs myweb-container
```

Output showed:

```text id="xtv0z6"
nginx/1.31.1
```

This confirmed Nginx was running successfully.

---

# Step 7: Verify File Inside Container

Connect to container:

```bash id="it64s8"
docker exec -it myweb-container bash
```

Check deployed file:

```bash id="3qxrzm"
cat /usr/share/nginx/html/index.html
```

Expected:

```html id="x1iwsw"
<h1>Hello from Docker</h1>
```

This is a useful troubleshooting technique to verify that files were copied correctly during image build.

Exit container:

```bash id="y2jsw0"
exit
```

---

# Troubleshooting Performed

## Issue 1: Dockerfile Not Found

Error:

```text id="8s9bte"
failed to read dockerfile: open Dockerfile: no such file or directory
```

Cause:

```text id="2sjlwm"
dockerFile
```

was used instead of:

```text id="3j0v0l"
Dockerfile
```

Linux is case-sensitive.

Fix:

```bash id="5mt4bx"
mv dockerFile Dockerfile
```

---

## Issue 2: Wrong Nginx Path

Wrong:

```dockerfile id="k2ey4n"
/usr/shar/nginx/html
```

Correct:

```dockerfile id="24v79h"
/usr/share/nginx/html
```

---

## Issue 3: Image Rebuild Required

Docker images are immutable.

After modifying:

```text id="39gc7p"
Dockerfile
index.html
```

I had to rebuild:

```bash id="hvw8rl"
docker build --no-cache -t myweb .
```

and recreate the container.

---

# Commands Learned

Build Image:

```bash id="5jywj4"
docker build -t myweb .
```

Run Container:

```bash id="wxg8fq"
docker run -d -p 3000:80 --name myweb-container myweb
```

List Images:

```bash id="t2qyg7"
docker images
```

List Containers:

```bash id="m0lxck"
docker ps
```

View Logs:

```bash id="k20z11"
docker logs myweb-container
```

Access Container:

```bash id="yc3evz"
docker exec -it myweb-container bash
```

Remove Container:

```bash id="whjqbh"
docker rm -f myweb-container
```

---

# Key Learnings

* Docker images are templates used to create containers.
* Containers are running instances of images.
* Nginx serves files from `/usr/share/nginx/html`.
* Dockerfile naming is case-sensitive.
* Linux file paths are case-sensitive.
* Changes to files require rebuilding the image.
* Container logs are the first place to check during troubleshooting.
* `docker exec` is useful for inspecting files inside a running container.

---


