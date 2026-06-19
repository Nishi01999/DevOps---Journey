Step 1: Update Package Repository
sudo apt update
Purpose:
	• Refreshes package information from configured repositories.

Step 2: Install Java
Jenkins requires Java to run.
	sudo apt install openjdk-17-jdk -y

Verify Java Installation:
	java -version

Expected Output:
openjdk version "17.x.x"


Step 3: Add Jenkins Repository Key

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null

Purpose:
	• Adds Jenkins GPG signing key.
	• Allows Ubuntu to verify Jenkins packages.


Step 4: Add Jenkins Repository

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null

Purpose:
	• Registers Jenkins repository with APT.



Step 5: Update Package List
sudo apt update

Error Encountered
		
		APT update failed with:
NO_PUBKEY 7198F4B714ABFC68
The repository 'https://pkg.jenkins.io/debian-stable binary/ Release' is not signed.
Full Error:
W: GPG error: https://pkg.jenkins.io/debian-stable binary/ Release:
The following signatures couldn't be verified because the public key is not available:
NO_PUBKEY 7198F4B714ABFC68
E: The repository is not signed.

<img width="947" height="450" alt="image" src="https://github.com/user-attachments/assets/ad4ba087-56da-4a3e-becf-6cf9e2310105" />


Root Cause Analysis
Ubuntu attempted to verify the Jenkins repository using key:
7198F4B714ABFC68

However, the imported Jenkins key was:
5BA31D57EF5975CA

Since the repository signature and imported key did not match, Ubuntu refused to trust the repository.
This is a security feature of APT.

Resolution Attempt
Removed old key and repository:
	sudo rm -f /etc/apt/sources.list.d/jenkins.list
sudo rm -f /usr/share/keyrings/jenkins-keyring.gpg

Re-imported Jenkins key:
	curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key \
| sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

Re-added repository:
	echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" \
| sudo tee /etc/apt/sources.list.d/jenkins.list

Result:
	• Repository signature issue persisted.
	• Installation could not continue.


