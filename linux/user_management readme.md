# Linux User Management & Permissions

> Part of my **DevOps Journey** — Week 1: Linux Fundamentals  
> Practised on: Ubuntu WSL2 on Windows  
> Topics: Users · Groups · Permissions · chmod · chown · sudo

---

## What I Learned

User management is the process of creating, modifying, and deleting user accounts on a Linux system and controlling what each user is allowed to do. It is the foundation of Linux security.

Three core concepts:
- **Users** — individual accounts, each with a username, password, and home folder
- **Groups** — collections of users who share the same permissions
- **Permissions** — rules that define what a user or group can do with each file and folder

---

## Why Multiple Users Exist

### On a Personal Machine
| Scenario | Reason |
|---|---|
| Family sharing one laptop | Each person gets private space — files stay separate |
| Developer testing | Create test accounts to simulate different permission levels |
| Service accounts | Nginx, MySQL, Docker auto-create their own system users |

### On a Company Server
| Role | What they get | What they cannot touch |
|---|---|---|
| DevOps engineers | Full sudo access | Nothing restricted |
| Developers | Deploy code, check logs | Database configs, system files |
| DBAs | Database folders only | Application code |
| QA engineers | Run tests, read logs | Production configs |
| Interns | Read logs only | Everything else |

---

## Understanding Permissions

Every file in Linux has permissions for three types of users:

```
-rwxr-xr-- 1 nishi developers 1234 Apr 6 script.sh
 │   │   │
 │   │   └── Others (everyone else): r-- = read only
 │   └────── Group (developers):     r-x = read + execute
 └────────── Owner (nishi):          rwx = read + write + execute
```

### Permission Types

| Symbol | Name | On a file | On a folder |
|---|---|---|---|
| `r` | Read | View file contents | List files inside |
| `w` | Write | Edit the file | Create/delete files inside |
| `x` | Execute | Run as a program | Enter with `cd` |
| `-` | None | No permission | No permission |

---

## The Number System (chmod)

| Number | Permission | Calculation |
|---|---|---|
| `7` | `rwx` | 4+2+1 |
| `6` | `rw-` | 4+2+0 |
| `5` | `r-x` | 4+0+1 |
| `4` | `r--` | 4+0+0 |
| `0` | `---` | 0+0+0 |

### Common chmod Commands

```bash
chmod +x script.sh          # make script executable
chmod 755 script.sh         # owner: full, group+others: read+execute
chmod 644 config.txt        # owner: read+write, group+others: read only
chmod 600 ~/.ssh/id_rsa     # owner only: read+write (SSH keys)
chmod 777 file.sh           # everyone full access — avoid in production
```

---

## User Management Commands

### Users

```bash
# Create user with home folder and bash shell
sudo useradd -m -s /bin/bash alice

# Set password
sudo passwd alice

# Add user to a group
sudo usermod -aG developers alice

# Give sudo access
sudo usermod -aG sudo alice

# Delete user and their home folder
sudo userdel -r alice

# Switch to another user
su - alice

# Switch to root
sudo su -

# Go back to normal user
exit

# See current user
whoami

# See user details and groups
id alice
```

### Groups

```bash
# Create a group
sudo groupadd developers

# Delete a group
sudo groupdel developers

# See all groups a user belongs to
groups alice

# List all groups on system
cat /etc/group
```

### Permissions

```bash
# Change permissions
chmod 755 script.sh

# Change owner
sudo chown alice script.sh

# Change owner and group together
sudo chown alice:developers script.sh

# Change recursively for entire folder
sudo chown -R alice:developers /var/www/app/

# View permissions
ls -l script.sh
```

---

## Real World DevOps Setup Example

```bash
# Step 1 — Create groups
sudo groupadd developers
sudo groupadd devops
sudo groupadd readonly

# Step 2 — Create users
sudo useradd -m -s /bin/bash alice    # developer
sudo useradd -m -s /bin/bash charlie  # devops engineer
sudo useradd -m -s /bin/bash diana    # intern

# Step 3 — Assign to groups
sudo usermod -aG developers alice
sudo usermod -aG devops charlie
sudo usermod -aG sudo charlie          # charlie gets sudo
sudo usermod -aG readonly diana

# Step 4 — Set folder permissions
sudo chown root:developers /var/www/app/
sudo chmod 775 /var/www/app/           # developers can write, others read only

sudo chown root:readonly /var/log/app/
sudo chmod 750 /var/log/app/           # readonly group can read, others nothing
```

---

## The Core Principle

> **Least Privilege** — Every user gets exactly the access they need to do their job, and absolutely nothing more.

This is the most important security concept in Linux user management. Overprivileged accounts are the most common source of security breaches in companies.

---

## Key Files for User Management

| File | Contains |
|---|---|
| `/etc/passwd` | All user accounts on the system |
| `/etc/shadow` | Encrypted passwords (root only) |
| `/etc/group` | All groups on the system |
| `/home/username/` | Each user's personal home folder |
| `/etc/sudoers` | Controls who can use sudo and how |

---

## Quick Reference Table

| Task | Command |
|---|---|
| Create user | `sudo useradd -m -s /bin/bash username` |
| Set password | `sudo passwd username` |
| Add to group | `sudo usermod -aG groupname username` |
| Give sudo | `sudo usermod -aG sudo username` |
| Delete user | `sudo userdel -r username` |
| Create group | `sudo groupadd groupname` |
| See all users | `cat /etc/passwd` |
| See your groups | `groups` |
| Switch user | `su - username` |
| Switch to root | `sudo su -` |
| Back to normal | `exit` |
| Change permissions | `chmod 755 filename` |
| Make executable | `chmod +x filename` |
| Change owner | `sudo chown user:group filename` |
| View permissions | `ls -l filename` |

---

## Tools Used
![Linux](https://img.shields.io/badge/Linux-Ubuntu_WSL2-E95420?style=flat&logo=ubuntu)
![Bash](https://img.shields.io/badge/Shell-Bash-4EAA25?style=flat&logo=gnu-bash)

---

*Part of my DevOps learning journey — [View full roadmap](../README.md)*
