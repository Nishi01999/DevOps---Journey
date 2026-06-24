Step 1 – Create YAML
vi nginx-pod.yaml
Paste:
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80

Step 2 – Create Pod
kubectl apply -f nginx-pod.yaml
Expected:
pod/nginx-pod created

Step 3 – Verify Pod
kubectl get pods
Expected:
NAME        READY   STATUS
nginx-pod   1/1     Running

Step 4 – Access Application
kubectl port-forward pod/nginx-pod 8081:80
Open:
http://localhost:8081
You should see the Nginx welcome page.

Step 5 – Delete Pod
kubectl delete -f nginx-pod.yaml
