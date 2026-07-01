Step 1 – Create Deployment YAML
vim deployment.yml

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
Step 2 – Verify the YAML File
cat deployment.yml

What it does: Displays the contents of the YAML file to verify there are no mistakes before applying it.

Step 3 – Create the Deployment
kubectl apply -f deployment.yml

What it does: Reads the YAML file and creates (or updates) the Deployment in the Kubernetes cluster.

Expected Output

deployment.apps/nginx-deployment created
Step 4 – Verify the Deployment
kubectl get deploy

What it does: Shows all Deployments running in the current namespace.

Expected Output

NAME               READY   UP-TO-DATE   AVAILABLE
nginx-deployment   1/1     1            1
Step 5 – Verify the ReplicaSet
kubectl get rs

What it does: Displays the ReplicaSet automatically created by the Deployment.

Expected Output

NAME                          DESIRED CURRENT READY
nginx-deployment-xxxxx        1       1       1
Step 6 – Verify the Pod
kubectl get pods

What it does: Lists all Pods currently running in the cluster.

Expected Output

NAME                                READY STATUS
nginx-deployment-xxxxx              1/1   Running
Kubernetes Resource Hierarchy
Deployment
      │
      ▼
ReplicaSet
      │
      ▼
Pod
      │
      ▼
Container

Deployment manages ReplicaSets.

ReplicaSet manages Pods.

Pods run Containers.

Step 7 – Watch Pods in Real Time

Open another terminal.

kubectl get pods -w

What it does: Continuously watches Pods and displays live changes such as creation, deletion, or status updates.

Step 8 – Delete the Running Pod

In another terminal:

kubectl delete pod <pod-name>

Example

kubectl delete pod nginx-deployment-77bc6bd484-5gxw2

What it does: Deletes one Pod manually to test Kubernetes Self-Healing.

What Happened?

You will notice something similar:

Running

↓

Terminating

↓

Pending

↓

ContainerCreating

↓

Running
Meaning of Each State
Status	Meaning
Running	Pod is serving the application normally.
Terminating	Pod is shutting down gracefully after deletion.
Pending	Kubernetes has scheduled a replacement Pod.
ContainerCreating	Kubernetes is pulling the image and starting the container.
Running	New Pod is ready and serving traffic.
Self-Healing Demonstration

When you deleted the Pod:

Deployment

↓

ReplicaSet notices

↓

Desired Pods = 1

Current Pods = 0

↓

Creates a New Pod

↓

Application Continues Running

No manual intervention is required.

Step 9 – Scale the Application

Open the Deployment YAML.

vim deployment.yml

What it does: Opens the Deployment file to modify the number of replicas.

Change

replicas: 1

to

replicas: 3

Apply the changes.

kubectl apply -f deployment.yml

What it does: Updates the Deployment with the new desired state (3 replicas).

Verify the Pods.

kubectl get pods

What it does: Confirms that Kubernetes has created three Pods.

Expected Output

nginx-deployment-xxxxx
nginx-deployment-yyyyy
nginx-deployment-zzzzz
Step 10 – Verify ReplicaSet
kubectl get rs

What it does: Confirms that the ReplicaSet now manages three Pods.

Expected Output

DESIRED   CURRENT   READY

3         3         3
Step 11 – Delete One Pod Again
kubectl delete pod <pod-name>

What it does: Deletes one of the three Pods to demonstrate automatic recovery.

Expected Behavior

Even after deleting one Pod:

Desired Pods = 3

↓

Current Pods = 2

↓

ReplicaSet creates a new Pod

↓

Current Pods = 3

Kubernetes automatically restores the desired state.

Key Concepts Learned
Deployment
High-level object used in production.
Manages ReplicaSets.
Supports rolling updates and rollbacks.
ReplicaSet
Ensures the desired number of Pods are always running.
Automatically recreates failed or deleted Pods.
Implements Kubernetes Self-Healing.
Pod
Smallest deployable unit in Kubernetes.
Runs one or more containers.
Self-Healing

If a Pod crashes or is deleted, ReplicaSet automatically creates a replacement Pod without manual intervention.

Scaling

Increasing the replicas value allows Kubernetes to create additional Pods automatically.

Zero Downtime

Kubernetes creates replacement Pods before completely removing old ones, ensuring the application remains available to users.

Commands Used
Command	Purpose
vim deployment.yml	Create or edit the Deployment YAML file.
cat deployment.yml	Display the YAML content for verification.
kubectl apply -f deployment.yml	Create or update the Deployment from the YAML file.
kubectl get deploy	View all Deployments.
kubectl get rs	View ReplicaSets created by Deployments.
kubectl get pods	List all running Pods.
kubectl get pods -w	Watch Pod status changes in real time.
kubectl delete pod <pod-name>	Delete a Pod to test Self-Healing.
Interview Questions
1. Why do we use Deployments instead of creating Pods directly?

Deployments automatically manage Pods, provide self-healing, scaling, rolling updates, and zero downtime, making them suitable for production.

2. Does a Deployment create Pods directly?

No. A Deployment creates a ReplicaSet, and the ReplicaSet creates and manages the Pods.

3. What is the role of a ReplicaSet?

It ensures that the desired number of Pods are always running and recreates Pods if they fail or are deleted.

4. What is Self-Healing in Kubernetes?

Self-Healing is Kubernetes' ability to automatically recreate failed or deleted Pods so the application remains available.

5. What is the difference between the desired state and the current state?

The desired state is the configuration defined in the Deployment (such as replicas: 3), while the current state is the actual number of Pods running. Kubernetes continuously works to make the current state match the desired state.
