Kubernetes History
Kubernetes was originally developed by Google based on their internal system called Borg, which they used to manage thousands of applications running in containers. As companies started adopting containers using Docker, they faced problems in managing large numbers of containers. To solve this, Google released Kubernetes as an open-source project in 2014, and now it is maintained by the Cloud Native Computing Foundation.
Example:
Imagine Google running Gmail, YouTube, and Maps. Each service runs thousands of containers. Manually managing them is impossible. Kubernetes was built to automate this process.


 Docker vs Kubernetes
     
Both Docker and Kubernetes are container orchestration tools, but Kubernetes is more powerful and widely used.
	• Docker is simpler and easier to start.
	• Kubernetes is more advanced, flexible, and production-ready.
Example:
If you are running a small application, Docker might work. But if you are running something like an online shopping platform with high traffic, Kubernetes is preferred because it handles scaling, failures, and networking better.

__________________________________________________________________________________________________
Why Do We Need Kubernetes?
When applications become large:

100s or 1000s of containers
Managing them manually becomes difficult.

Kubernetes solves this by providing:
1. Cluster Nature Applications run across multiple servers (nodes).
2. Auto Healing Failed containers are automatically recreated.
3. Auto Scaling Containers automatically increase/decrease based on load.
4. Enterprise Support
	• Load Balancing 
	• Security 
	• Networking 
	• High Availability
