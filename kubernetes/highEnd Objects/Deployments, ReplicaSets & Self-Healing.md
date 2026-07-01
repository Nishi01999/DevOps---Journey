1. vim deployment.yml	Create or edit the Deployment YAML file.
2. cat deployment.yml	Display the YAML content for verification.

What it does: Opens a new YAML file where we define our Deployment configuration.

Paste the following content:

apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment
  labels:
    app: nginx

spec:
  replicas: 1

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2

        ports:
        - containerPort: 80

3. kubectl apply -f deployment.yml
Create or update the Deployment from the YAML file.

4. kubectl get deploy	
View all Deployments.

5. kubectl get rs	View 
ReplicaSets created by Deployments.

6. kubectl get pods
List all running Pods.

7. kubectl get pods -w
 Watch Pod status changes in real time.

8.  kubectl delete pod nginx-deployment-77bc6bd484-5gxw2	
Delete a Pod to test Self-Healing.

<img width="1036" height="642" alt="image" src="https://github.com/user-attachments/assets/d8738032-0d74-4196-b0f0-eb4ee17a2890" />
Parallel Creation and Deletion
Kubernetes doesn't wait for the old Pod to disappear completely.
Instead,
it starts creating the replacement immediately.
That's why users usually don't notice any interruption.
