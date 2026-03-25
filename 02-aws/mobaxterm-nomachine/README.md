1. How to connect to EC2 using MobaXterm (step by step)
1. Open MobaXterm
2. Click Session → SSH
3. Remote host = your EC2 public IP
4. Username = ubuntu (or ec2-user)
5. Check "Use private key" → select your .pem file
6. Click OK → you are inside EC2!

2. How to transfer files between laptop and EC2 using MobaXterm
- After connecting, left side shows EC2 files
- Just drag and drop files from your laptop to EC2
- No extra commands needed!

3. Tried these hands-on connecting via MobaXterm:
bash# Check which server i was on
hostname

# Check the OS
cat /etc/os-release

# Check free disk space
df -h

# Check RAM
free -m

# Create a test file
touch myfile.txt

# List files
ls -la
```

---

### 4. NoMachine 
```
- NoMachine is used when EC2 has a desktop (GUI) installed
- In real DevOps work, servers never have a desktop
- So NoMachine is rarely used
- MobaXterm is what you will use daily

What to Add to Your GitHub README
Just add these 3 sections to the README I already generated for you:
Section 1 — Hands-on Practice
markdown## What I Practiced
- Connected to EC2 using MobaXterm
- Transferred files using drag and drop
- Ran basic Linux commands after connecting
Section 2 — Commands I ran after connecting
markdown## Commands I Ran After Connecting via MobaXterm
- hostname → to verify I am inside EC2
- cat /etc/os-release → to check the OS
- df -h → to check disk space
- free -m → to check RAM
- touch myfile.txt → to create a test file
Section 3 — My observation
markdown## My Observation
- MobaXterm is much easier than PowerShell for beginners
- The file browser on the left side is very helpful
- NoMachine is not needed for DevOps — only for desktop environments


 
