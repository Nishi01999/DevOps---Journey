Step 1 – Create YAML
vi wordpress-pod.yaml
Paste:
apiVersion: v1
kind: Pod
metadata:
  name: wordpress-pod

spec:
  containers:

  - name: wordpress
    image: wordpress
    ports:
    - containerPort: 80
    env:
    - name: WORDPRESS_DB_HOST
      value: 127.0.0.1
    - name: WORDPRESS_DB_USER
      value: root
    - name: WORDPRESS_DB_PASSWORD
      value: root
    - name: WORDPRESS_DB_NAME
      value: wordpress

  - name: mysql
    image: mysql:5.7
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: root
    - name: MYSQL_DATABASE
      value: wordpress

Step 2 – Create Pod
kubectl apply -f wordpress-pod.yaml

Step 3 – Verify Pod
kubectl get pods
Expected:
NAME            READY   STATUS
wordpress-pod   2/2     Running

Step 4 – Access WordPress
kubectl port-forward pod/wordpress-pod 8085:80
Open:
http://localhost:8085
You should see the WordPress installation page.
