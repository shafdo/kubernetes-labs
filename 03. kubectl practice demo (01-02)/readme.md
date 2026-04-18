# Kubernetes Lab Questions – Minikube Basics

### **Objective**

Test your understanding of Minikube, Deployments, Pods, and Services.

---

## **Questions**

1. **Start the Cluster**
   - Start a Minikube cluster.
   - Verify the cluster status.
   - List the nodes in the cluster.

2. **Create a Deployment**
   - Create a Deployment named `hello-minikube` using the image `kicbase/echo-server:1.0`.
   - List all Deployments.
   - List all Pods.
   - Get detailed information about one of the Pods using `kubectl describe pod <pod-name>`.

3. **Expose the Deployment**
   - Expose the `hello-minikube` deployment as service port on **port 8081**, targeting pod port **8080**.
   - Get the URL for the service using Minikube.
   - Open the URL in a browser and verify that the service is working.

4. **Inspect Logs**
   - View the logs of the Deployment to verify that the container is running correctly.

5. **Bonus Challenge: Scaling**
   - Scale the `hello-minikube` Deployment to **3 replicas**.
   - Verify that all Pods are running.
   - Access each Pod via the NodePort URL.

6. **Bonus Challenge: Self-Healing**
   - Delete one of the Pods manually.
   - Verify that the Deployment automatically recreates the deleted Pod.

7. **Cleanup**
   - Delete the `hello-minikube` Service.
   - Delete the `hello-minikube` Deployment.
   - Confirm that there are no remaining Pods in the cluster.

---

## **Solutions / Commands**

### 1. Start the Cluster

```bash
minikube start
minikube status
kubectl get nodes
```

### 2. Create a Deployment

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl get deployments
kubectl get pods
kubectl describe pod <pod-name>
```

### 3. Expose the Deployment

```bash
kubectl expose deployment hello-minikube --type=NodePort --port=8081 --target-port=8080
minikube service hello-minikube --url
# Open the URL in browser to verify
```

### 4. Inspect Logs

```bash
kubectl logs deployment/hello-minikube
```

### 5. Bonus Challenge: Scaling

```bash
kubectl scale deployment hello-minikube --replicas=3
kubectl get pods
# Verify all 3 Pods are Running
minikube service hello-minikube --url
# Access the service via NodePort
```

### 6. Bonus Challenge: Self-Healing

```bash
kubectl delete pod <pod-name>
kubectl get pods
# Observe that the Deployment automatically recreates the deleted Pod
```

### 7. Cleanup

```bash
kubectl delete service hello-minikube
kubectl delete deployment hello-minikube
kubectl get pods
# Confirm "No resources found"
```

# Note

- What is the difference between:
  - kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
  - kubectl run hello-minikube --image=kicbase/echo-server:1.0

- Quick distinction — both run containers, but different abstractions:
  - kubectl create deployment
    - Creates a Deployment object
    - Deployment manages Pods (self-healing, rolling updates, scaling)
    - Recommended for real apps

  - kubectl run
    - Creates just a Pod by default (in newer K8s, it can also create Deployment if --restart=Always)
    - No Deployment management by default
    - Mostly for quick testing / one-off Pods
    - If that pod goes down that's it. It is not respawn with another like in deployments.

  ```
  Deployment = “managed Pod”
  kubectl run = “just a Pod (quick & dirty)”
  ```
