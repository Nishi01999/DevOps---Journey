Multi Container Pod (MySQL + Adminer)

Step 1 – Create YAML

vi mysql-adminer-pod.yaml


insert:

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


Step 2 – Create Pod

kubectl apply -f mysql-adminer-pod.yaml


Step 3 – Verify Pod

kubectl get pods
Expected:
NAME            READY   STATUS
mysql-adminer   2/2     Running

Step 4 – View Logs

MySQL:
kubectl logs mysql-adminer -c mysql
Adminer:
kubectl logs mysql-adminer -c adminer

Step 5 – Access Adminer

kubectl port-forward pod/mysql-adminer 8084:8080
<img width="822" height="107" alt="image" src="https://github.com/user-attachments/assets/e1f54306-3b31-40a1-b6b5-8e17a1cd2b9f" />

Open:

http://localhost:8084
Login:
System   : MySQL
Server   : 127.0.0.1
Username : root
Password : root
Database : Leave Empty
<img width="1567" height="682" alt="image" src="https://github.com/user-attachments/assets/352d18ed-cd25-46f8-b39f-7147344e992d" />

___________________________________________________
Learning Outcome
How many Pods?
1
How many Containers?
2
Communication Method?
localhost
