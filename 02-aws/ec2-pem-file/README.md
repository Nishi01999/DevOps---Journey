# What is a .pem File?

## Simple Explanation
A .pem file is a private key file AWS gives you when you create an EC2 instance.
It works like a physical key — AWS keeps the lock (public key) on the server,
and you keep the key (.pem file) to open it.

## How SSH Authentication Works
1. You run: ssh -i "your-key.pem" ubuntu@<public-ip>
2. Your laptop sends the private key to EC2
3. EC2 matches it with the stored public key
4. If matched → Access granted ✅

## Key Facts
- Downloaded only ONCE during EC2 creation
- Never share it with anyone
- If lost → cannot be recovered, must create new key pair
- Must set permission: chmod 400 your-key.pem

## Commands
# Fix permission (Linux/Mac)
chmod 400 your-key.pem

# Connect using .pem
ssh -i "your-key.pem" ubuntu@<your-ec2-public-ip>

## Why chmod 400?
SSH refuses to work if .pem is readable by others.
chmod 400 = only YOU can read the file.

## Interview Answer
A .pem file is a private key that works on asymmetric encryption.
AWS stores the public key on EC2, you keep the private key.
During SSH, both keys are matched — if they match, access is granted.

## Interview Questions
1. What is a .pem file?
2. What happens if you lose the .pem file?
3. Why do we use chmod 400?
4. What encryption does SSH use?
5. Can two EC2 instances share the same .pem file?
```

---

## Step 4 — Commit (save) the file

1. Scroll down to the bottom of the page
2. You will see a box that says **"Commit new file"**
3. In the first text box, type a short message like:
```
Add notes on .pem file and SSH authentication
```
4. Leave everything else as default
5. Click the green **"Commit new file"** button

---

## Step 5 — Verify it looks good

1. Go back to your repo home page
2. Click `02-aws` folder → `ec2-pem-file` folder
3. You should see your `README.md` nicely formatted with headings and code blocks ✅

---

## Your Repo Structure So Far
```
