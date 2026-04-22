# WSL Error Fix — `getpwnam` & `chdir` Failed on Ubuntu

**Topic:** Troubleshooting WSL User & Home Directory Errors
**Environment:** Windows 11 + WSL2 + Ubuntu 24.04
**Date:** ___________

---

## The Error

```
<3>WSL (5480 - Relay) ERROR: CreateProcessParseCommon:996: getpwnam(nishi) failed 17
<3>WSL (5480 - Relay) ERROR: CreateProcessParseCommon:1005: getpwuid(1000) failed 17
```

---

## What the Error Means

| Error | Meaning |
|---|---|
| `getpwnam(nishi) failed 17` | WSL is looking for username `nishi` in `/etc/passwd` but can't find it |
| `getpwuid(1000) failed 17` | WSL is looking for a user with UID 1000 but it doesn't exist or is broken |
| `failed 17` | Error code 17 = `EEXIST` — entry missing or corrupted |
| Logged in as `root` instead of your user | WSL fell back to root because default user was not found |

---

## Root Causes Found

### 1. Username field was empty in `/etc/passwd`
The entry in `/etc/passwd` looked like this:
```
:x:1001:1001::/home/nishikush:/bin/sh
```
The username before the first `:` was completely missing.

### 2. UID and GID were wrong
WSL expects the default user to have **UID 1000** but the user had UID `1001`.

### 3. Shell was `/bin/sh` instead of `/bin/bash`

### 4. Home directory `/home/nishikush` did not exist on disk

### 5. WSL default user was set to `nishi` but actual username was `nishikush`

---

## Full Fix — Step by Step

### Step 1 — Open PowerShell as Administrator
- Press `Windows + R` → type `powershell` → press `Ctrl+Shift+Enter`
- Prompt looks like: `PS C:\WINDOWS\system32>`

---

### Step 2 — Check your WSL distro name
```powershell
wsl --list --verbose
```
Note the exact name (e.g. `Ubuntu`)

---

### Step 3 — Open Ubuntu as Root
```powershell
wsl -d Ubuntu -u root
```
Prompt changes to `root@Nishi:~#`

---

### Step 4 — Fix `/etc/passwd`
```bash
nano /etc/passwd
```
Find the broken line (username field empty):
```
:x:1001:1001::/home/nishikush:/bin/sh
```
Change it to:
```
nishikush:x:1000:1000::/home/nishikush:/bin/bash
```
Save: `Ctrl+O` → `Enter` → `Ctrl+X`

---

### Step 5 — Fix `/etc/group`
```bash
nano /etc/group
```
Find the line with empty name and UID 1001:
```
:x:1001:
```
Change it to:
```
nishikush:x:1000:
```
Save and exit.

---

### Step 6 — Create Home Directory
```bash
mkdir -p /home/nishikush
chown nishikush:nishikush /home/nishikush
chmod 755 /home/nishikush
```

---

### Step 7 — Reset Password
```bash
passwd nishikush
```
Enter new password twice.

---

### Step 8 — Exit and Set Default User
```bash
exit
```
Back in PowerShell:
```powershell
wsl --terminate Ubuntu
wsl -d Ubuntu -u nishikush
```

---

### Step 9 — Verify Everything is Fixed
```bash
whoami
# nishikush

pwd
# /home/nishikush

cat /etc/passwd | grep nishikush
# nishikush:x:1000:1000::/home/nishikush:/bin/bash
```

---

### Step 10 — Fix Default Landing Directory
```bash
echo "cd ~" >> ~/.bashrc
source ~/.bashrc
```
This ensures you always land in `/home/nishikush` on startup instead of `/mnt/c/Users/...`

---

## Key Concepts Learned

| Concept | Explanation |
|---|---|
| `/etc/passwd` | Stores all user accounts — format is `username:x:UID:GID:comment:home:shell` |
| `/etc/group` | Stores all groups on the system |
| `/etc/shadow` | Stores encrypted passwords (needs sudo to read) |
| UID 1000 | First real human user on Linux/WSL always gets UID 1000 |
| `getpwnam` | Linux function that looks up a user by username |
| `getpwuid` | Linux function that looks up a user by UID |
| SIGINT / Error 17 | System error meaning entry not found or already exists |
| `chown` | Changes ownership of a file or folder |
| `chmod 755` | Sets folder permissions: owner gets all, others get read+execute |

---

## Commands Used in This Fix

| Command | What it did |
|---|---|
| `cat /etc/passwd` | Read all user entries |
| `nano /etc/passwd` | Edited the passwd file directly |
| `nano /etc/group` | Edited the group file directly |
| `mkdir -p /home/nishikush` | Created the missing home directory |
| `chown nishikush:nishikush /home/nishikush` | Gave ownership to the user |
| `chmod 755 /home/nishikush` | Set correct permissions on home folder |
| `passwd nishikush` | Reset the user password |
| `wsl --list --verbose` | Listed all WSL distros and their status |
| `wsl -d Ubuntu -u root` | Opened WSL as root without password |
| `wsl --terminate Ubuntu` | Shut down the WSL distro |

---

## Common Mistakes to Avoid

- ❌ Never run `wsl` commands inside Ubuntu — they only work in PowerShell/CMD
- ❌ Never edit `/etc/passwd` carelessly — a broken entry locks you out
- ❌ Never forget to create the home directory after fixing `/etc/passwd`
- ✅ Always use `wsl -d <distroname> -u root` to recover when locked out
- ✅ Always verify with `cat /etc/passwd | grep username` after making changes

---
-
-
-
