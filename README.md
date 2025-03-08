## 📌 **Deploying a Vite App on Kubernetes with Nginx**  

### 🔹 **1️⃣ Prerequisites**  
Before you begin, make sure you have:  
✅ **Kubernetes (Kind or Minikube)** installed  
✅ **kubectl** configured and working  
✅ **Docker** installed  

---

### 🔹 **2️⃣ Create the Deployment & Service**  
Create a `dep-and-svc.yml` file with the following content:  

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vite-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vite-app
  template:
    metadata:
      labels:
        app: vite-app
    spec:
      containers:
        - name: vite-container
          image: your-dockerhub-username/vite-app:latest # Change this to your Docker image
          ports:
            - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: vite-service
spec:
  selector:
    app: vite-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

---

### 🔹 **3️⃣ Deploy the Application**
Apply the Deployment and Service using:  
```sh
kubectl apply -f dep-and-svc.yml
```
This will:  
✅ Create a **Deployment** (`vite-deployment`)  
✅ Expose it via a **Service** (`vite-service`)  

---

### 🔹 **4️⃣ Port Forward the Service**
To access the Vite app, forward the port:  
```sh
sudo kubectl port-forward svc/vite-service 80:80
```
Now, open **http://localhost:80** in your browser. 🎉  

---

### 🔹 **5️⃣ Verify the Deployment**
Check if the pod is running:  
```sh
kubectl get pods
```
Check logs if needed:  
```sh
kubectl logs -f <pod-name>
```
To Delete the deployment and svc 
```sh
kubectl delete -f dep-and-svc.yml
```

---

## 🚀 **Summary**
| Task                          | Command |
|--------------------------------|----------------------------------|
| Deploy app                    | `kubectl apply -f dep-and-svc.yml` |
| Check running pods             | `kubectl get pods` |
| Port forward service to local  | `sudo kubectl port-forward svc/vite-service 80:80` |
| View app in browser            | [http://localhost:80](http://localhost:80) |

---

Now your **Vite app** is running inside **Kubernetes** using **Nginx**! 🚀🔥  
