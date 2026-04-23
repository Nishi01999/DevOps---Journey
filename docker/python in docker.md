# DevOps Notes — Running Python in Docker

**Topic:** How to run Python in Docker, base images explained, 4 ways, EC2 hands-on
**Source:** Abhishek Veeramalla – Docker Zero to Hero
**Date:** ___________

---

## 1. The Core Concept — What Docker Replaces

Without Docker you need:
1. A machine (laptop / EC2)
2. Python installed on it
3. Your code file

With Docker, the container **replaces steps 1 and 2** — it brings its own mini-environment with Python already inside. You only provide the code.

```
Your Code + Base Image (Python/Ubuntu) + Dockerfile = Running Container
```

---

## 2. Why Ubuntu or Bash Appears in Dockerfiles

Every Dockerfile needs a `FROM` statement — a **base image** to start from. You have two approaches:

### Approach A — Ubuntu Base (Manual)
Start with a full Ubuntu OS, then manually install Python:
```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y python3
```
Use when your app needs other Ubuntu system tools alongside Python.

### Approach B — Python Base (Recommended)
Python already pre-installed — just write your code:
```dockerfile
FROM python:3.9-slim
```
Cleaner, smaller, faster. Use for most Python apps.

### Base Image Comparison

| Base Image | What it is | Size | Best for |
|---|---|---|---|
| `ubuntu:latest` | Full Ubuntu OS | ~77 MB | Apps needing OS-level tools |
| `python:3.9` | Python on Debian | ~900 MB | Feature-rich Python apps |
| `python:3.9-slim` | Python on minimal Debian | ~125 MB | Most Python apps — sweet spot |
| `python:3.9-alpine` | Python on Alpine Linux | ~50 MB | Smallest possible image |

---

## 3. Four Ways to Run Python in Docker

---

### Way 1 — Interactive Shell (Quickest, No Files Needed)

```bash
docker run -it python:3.9
```

Python shell opens immediately:
```
>>> print("hello world")
hello world
```

No Dockerfile. No files. Exit with `exit()`.

---

### Way 2 — One-Liner Command

```bash
docker run python:3.9 python3 -c "print('hello world')"
# Output: hello world
```

`-c` means "run this string as Python code directly". No files, no Dockerfile.

---

### Way 3 — Dockerfile with Python Base (The DevOps Way ✅)

**Step 1 — Create project folder**
```bash
mkdir hello-python && cd hello-python
```

**Step 2 — Create app.py**
```bash
nano app.py
```
```python
print("hello world")
```

**Step 3 — Create Dockerfile**
```bash
nano Dockerfile
```
```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY app.py .
CMD ["python3", "app.py"]
```

**Step 4 — Build the image**
```bash
docker build -t hello-python .
```

**Step 5 — Run the container**
```bash
docker run hello-python
# Output: hello world
```

---

### Way 4 — Dockerfile with Ubuntu Base

```dockerfile
FROM ubuntu:latest
WORKDIR /app
RUN apt-get update && apt-get install -y python3
COPY app.py .
CMD ["python3", "app.py"]
```

Ubuntu is the base OS and you manually install Python.
Use this when your app needs extra Ubuntu packages/tools alongside Python.

---

## 4. Dockerfile Line-by-Line Explained (Way 3)

| Instruction | What it does |
|---|---|
| `FROM python:3.9-slim` | Pull Python base image from Docker Hub. Python pre-installed. |
| `WORKDIR /app` | Create and switch to `/app` folder inside the container |
| `COPY app.py .` | Copy your app.py from your machine into `/app` in the container |
| `CMD ["python3", "app.py"]` | Command that runs when container starts — runs your app |

### RUN vs CMD — Critical Difference (Interview Topic)

| | `RUN` | `CMD` |
|---|---|---|
| When does it run? | At **image build** time | At **container start** time |
| Used for | Installing packages, setup | Running your application |
| Creates image layer? | Yes | No |
| Example | `RUN apt-get install python3` | `CMD ["python3", "app.py"]` |

> Rule: RUN = build time setup. CMD = runtime execution.

---

## 5. Running on EC2 Instance — Full Walkthrough

### Step 1 — Launch EC2
- AWS Console → EC2 → Launch Instance
- Ubuntu 22.04, t2.micro (free tier)
- Allow SSH (port 22) in security group

### Step 2 — SSH into EC2
```bash
ssh -i your-key.pem ubuntu@your-ec2-public-ip
```

### Step 3 — Install Docker on EC2
```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
# Log out and log back in after this step
```

### Step 4 — Create your files
```bash
mkdir hello-python && cd hello-python
nano app.py       # type: print("hello world")
nano Dockerfile   # paste Dockerfile from Way 3
```

### Step 5 — Build and Run
```bash
docker build -t hello-python .
docker run hello-python
# Output: hello world
```

You just ran Python inside Docker on a cloud server. That is real DevOps.

---

## 6. All 4 Ways — Summary Table

| Way | Command | Needs Dockerfile? | Best for |
|---|---|---|---|
| Interactive shell | `docker run -it python:3.9` | No | Quick testing |
| One-liner | `docker run python:3.9 python3 -c "..."` | No | One-off commands |
| Dockerfile — Python base | Build + run | Yes | Clean Python apps |
| Dockerfile — Ubuntu base | Build + run | Yes | Apps needing OS tools |

---

## 7. The Big Picture — Why This Matters in DevOps

```
Developer writes Python app
        ↓
DevOps engineer writes Dockerfile
        ↓
docker build → image created locally
        ↓
docker push → image pushed to Docker Hub / AWS ECR
        ↓
EC2 / Kubernetes pulls the image
        ↓
docker run → app runs in production
```

The whole point: **same image runs identically everywhere** — your laptop, EC2, Kubernetes cluster. No more "it works on my machine" problems ever again.

---

## 8. Interview Questions

**Q: What is the difference between CMD and RUN in a Dockerfile?**
A: RUN executes at image build time — used to install packages and set up the environment. CMD executes at container start time — used to run the application. RUN creates a new image layer. CMD does not.

**Q: Why use `python:3.9-slim` instead of `ubuntu` as base image for a Python app?**
A: `python:3.9-slim` already has Python installed — no extra setup needed, smaller image size, faster builds. Ubuntu requires manually installing Python via apt-get, adds more layers, and produces a heavier image unnecessarily.

**Q: What does `-it` flag do in `docker run -it`?**
A: `-i` keeps STDIN open (interactive mode). `-t` allocates a pseudo terminal (TTY). Together `-it` gives you an interactive shell inside the container — like SSHing into a mini Linux machine.

**Q: What is WORKDIR in a Dockerfile?**
A: Sets the working directory inside the container for all subsequent instructions. Like running `cd` inside the container. Docker creates it automatically if it doesn't exist.

**Q: What does the `.` mean in `docker build -t hello-python .`?**
A: The `.` tells Docker to look for the Dockerfile in the current directory. It sets the build context — Docker sends all files in this directory to the Docker Daemon to use during the build.

