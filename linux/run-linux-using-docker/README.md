# Running Linux Using Docker

📅 **Date Practiced:** 2026-03-26
🏷️ **Topic:** Docker | Linux | Ubuntu | Containers | Windows

---

## What You Need

- ✅ Docker Desktop installed and running (green icon at bottom left)
- ✅ PowerShell open on Windows

---

## Method 1 — Quick and Simple (Just to Practice)

Open PowerShell and run ONE command:

```powershell
docker run -it ubuntu bash
```

Your terminal changes to:
```
root@a1b2c3d4e5f6:/#
```

You are inside Ubuntu Linux! 🎉

### What each part means
| Part | Meaning |
|------|---------|
| `docker run` | Create and start a container |
| `-it` | Interactive terminal |
| `ubuntu` | Use Ubuntu Linux image |
| `bash` | Start bash shell inside |

### ⚠️ Important Note
This is **temporary** — when you type `exit` everything is deleted!
Good for quick practice only — not for saving work!

```bash
# Exit container (everything deleted!)
exit
```

---

## Method 2 — Permanent Container (Abhishek's Method)

Keeps your data and settings even after restart.

### Step 1 — Create Shared Folder
```powershell
mkdir C:\tmp\ubuntu-data
```

### Step 2 — Run Full Docker Command
```powershell
docker run -dit `
  --name ubuntu-container `
  --hostname ubuntu-dev `
  --restart unless-stopped `
  --cpus="2" `
  --memory="4g" `
  --mount type=bind,source=C:\tmp\ubuntu-data,target=/data `
  -p 2222:22 `
  -p 8080:80 `
  --env TZ=Asia/Kolkata `
  --env LANG=en_US.UTF-8 `
  ubuntu:latest /bin/bash
```

> ⚠️ Uses backtick `` ` `` not `\` in Windows PowerShell

### Step 3 — Enter Ubuntu
```powershell
docker exec -it ubuntu-container bash
```

### Step 4 — You Are Inside Linux! ✅
```
root@ubuntu-dev:/#
```

---

## Method 3 — Specific Ubuntu Version

```powershell
# Ubuntu 22.04 LTS
docker run -it ubuntu:22.04 bash

# Ubuntu 20.04 LTS
docker run -it ubuntu:20.04 bash

# Tiny Linux — only 5MB!
docker run -it alpine sh
```

---

## Command Flags Explained

| Flag | Meaning |
|------|---------|
| `-d` | Detached — runs in background |
| `-i` | Interactive — keeps input open |
| `-t` | Terminal — gives proper terminal |
| `--name` | Give container a name |
| `--hostname` | Sets name shown in terminal prompt |
| `--restart unless-stopped` | Auto restart if Windows reboots |
| `--cpus="2"` | Limit to 2 CPU cores max |
| `--memory="4g"` | Limit to 4GB RAM max |
| `--mount` | Share folder between Windows and container |
| `-p 2222:22` | Map SSH port |
| `-p 8080:80` | Map web server port |
| `--env TZ` | Set timezone to India |
| `--env LANG` | Set UTF-8 language |
| `ubuntu:latest` | Use latest Ubuntu image |
| `/bin/bash` | Start bash shell |

---

## After Entering Linux — First Commands

```bash
# Update Ubuntu package list
apt-get update

# Install essential tools
apt-get install git -y
apt-get install curl -y
apt-get install wget -y
apt-get install vim -y
apt-get install tree -y

# Check OS version
cat /etc/os-release

# Check who you are
whoami

# Check current directory
pwd

# Check timezone (should show Asia/Kolkata)
date

# Check RAM
free -m

# Check disk space
df -h

# Check CPUs
nproc
```

---

## Daily Container Management Commands

```powershell
# Enter container (most used!)
docker exec -it ubuntu-container bash

# Check if container is running
docker ps

# Check all containers including stopped
docker ps -a

# Stop container
docker stop ubuntu-container

# Start container again
docker start ubuntu-container

# Restart container
docker restart ubuntu-container

# Check resource usage (CPU and RAM)
docker stats ubuntu-container

# See container logs
docker logs ubuntu-container

# Delete container permanently
docker rm -f ubuntu-container
```

---

## Test Shared Folder

Inside container:
```bash
# Go to shared folder
cd /data

# Create a test file
echo "Hello from Ubuntu!" > testfile.txt

# Verify file exists
ls -la
```

On Windows:
- Open File Explorer
- Go to `C:\tmp\ubuntu-data`
- You should see `testfile.txt` ✅

---

## Method 1 vs Method 2 — Full Comparison

| | Method 1 (Quick) | Method 2 (Permanent) |
|--|-----------------|---------------------|
| Command | `docker run -it ubuntu bash` | Full long command |
| Data saved? | ❌ Lost on exit | ✅ Always saved |
| Has a name? | ❌ Random name | ✅ ubuntu-container |
| Auto restart? | ❌ No | ✅ Yes |
| Shared folder? | ❌ No | ✅ Yes |
| Resource limits? | ❌ No | ✅ Yes |
| Good for | Quick testing | Daily DevOps learning |

---

## Docker Image vs Docker Container

```
Docker Image                Docker Container
────────────                ────────────────
Like a recipe               Like the cooked food
Read only template          Running instance of image
ubuntu:latest               ubuntu-container
Stored on disk              Running in memory
Can create many             Created from image
containers from one image
```

```bash
# See all downloaded images
docker images

# Download image without running
docker pull ubuntu

# Remove an image
docker rmi ubuntu
```

---

## Common Errors and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `docker: command not found` | Docker Desktop not running | Open Docker Desktop |
| `port already in use` | Port conflict | Change `-p 2222:22` to `-p 2223:22` |
| `cannot connect to daemon` | Docker not started | Open Docker Desktop → wait for 🟢 |
| `memory limit too high` | Not enough RAM | Change `--memory="4g"` to `--memory="2g"` |
| Container exits immediately | No command to keep it running | Make sure `/bin/bash` is at end |
| `No such container` | Container not created yet | Run the docker run command first |

---

## Interview Questions from This Topic

1. What is Docker?
2. What is the difference between a Docker image and a container?
3. What does `docker run -it ubuntu bash` do?
4. What is the difference between `-d` and `-it` flags?
5. What does `docker exec -it` do?
6. What is `docker ps` used for?
7. What is the difference between `docker stop` and `docker rm`?
8. What is a bind mount in Docker?
9. What does `--restart unless-stopped` mean?
10. What is Docker Hub?
11. How do you limit CPU and memory for a container?
12. What is the difference between `docker run` and `docker start`?

---

## My Practice Checklist

- [ ] Ran Method 1 — `docker run -it ubuntu bash`
- [ ] Ran basic Linux commands inside container
- [ ] Exited and noticed container was deleted
- [ ] Created shared folder `C:\tmp\ubuntu-data`
- [ ] Ran Method 2 — full permanent container command
- [ ] Entered container using `docker exec`
- [ ] Installed git and curl inside container
- [ ] Tested shared folder file sync with Windows
- [ ] Stopped and started container successfully
- [ ] Can explain difference between Method 1 and Method 2

---

*Part of my DevOps learning journey — Abhishek Veeramalla's Zero to Hero Playlist*
