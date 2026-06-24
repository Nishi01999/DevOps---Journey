Create YAML File
vi mysql-adminer.yaml

YAML File
apiVersion: v1
kind: Pod

metadata:
  name: mysql-adminer

spec:
  containers:

  - name: mysql
    image: mysql:5.7
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: root

  - name: adminer
    image: adminer
    ports:
    - containerPort: 8080

6. Understanding YAML
API Version
apiVersion: v1
Basic Kubernetes API.

Kind
kind: Pod
We are creating a Pod.

Metadata
metadata:
  name: mysql-adminer
Pod information.
Pod name:
mysql-adminer

MySQL Container
- name: mysql
Container name.
image: mysql:5.7
Downloads MySQL image.
env:
- name: MYSQL_ROOT_PASSWORD
  value: root
Sets MySQL root password.
Equivalent Docker Command:
docker run -d \
-e MYSQL_ROOT_PASSWORD=root \
mysql:5.7

Adminer Container
- name: adminer
Adminer container.
image: adminer
Database UI.
ports:
- containerPort: 8080
Application runs on port 8080.

7. Deploy Pod
Apply YAML:
kubectl apply -f mysql-adminer.yaml
Expected Output:
pod/mysql-adminer created

8. Verify Pod
Check Pod Status:
kubectl get pods
Expected:
NAME             READY   STATUS
mysql-adminer    2/2     Running

Understanding READY
2/2
Means:
2 Containers Running
2 Containers Expected

9. Describe Pod
Useful command:
kubectl describe pod mysql-adminer
Shows:
Events
Container status
Pod IP
Image details

10. View Logs
MySQL Logs:
kubectl logs mysql-adminer -c mysql
Adminer Logs:
kubectl logs mysql-adminer -c adminer
Why -c ?
Because Pod contains multiple containers.
-c mysql
means:
Show logs of MySQL container

11. Access Adminer
Port Forward:
kubectl port-forward pod/mysql-adminer 8084:8080
Open:
http://localhost:8084
OR
kubectl port-forward pod/mysql-adminer 9090:8080
Open:
http://localhost:9090

12. Adminer Login
Login Details:
System    : MySQL
Server    : 127.0.0.1
Username  : root
Password  : root
Database  : Leave Empty

13. How Does Adminer Reach MySQL?
Both containers are inside the same Pod.
Pod
 ├── MySQL
 └── Adminer
Communication:
Adminer
   |
localhost
   |
MySQL
Server Value:
127.0.0.1
or
localhost
works because both containers share the same network namespace.

14. Important Commands
Create Pod
kubectl apply -f mysql-adminer.yaml
List Pods
kubectl get pods
Detailed Information
kubectl describe pod mysql-adminer
MySQL Logs
kubectl logs mysql-adminer -c mysql
Adminer Logs
kubectl logs mysql-adminer -c adminer
Port Forward
kubectl port-forward pod/mysql-adminer 8084:8080
Delete Pod
kubectl delete pod mysql-adminer
