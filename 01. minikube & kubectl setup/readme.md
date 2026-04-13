## 1. Install kubectl

- `kubectl` is a tool that we use to manage Kubernetes resources and clusters after it is setup using minikube.

- Installing `kubectl` beforehand installing `minikube` helps minikube to configure kubectl afterwards.

- Download kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

  ```
  $ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

  $ chmod +x kubectl
  $ mkdir -p ~/.local/bin
  $ mv ./kubectl ~/.local/bin/kubectl
  $ reset
  $ kubectl version --client
  ```

## 2. Install minikube

- Minikube creates and manages a cluster, while kubectl interacts with and controls it

- Download & install: https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download

  ```
  $ curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

  $ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
  ```

- Find the driver name for your OS: https://minikube.sigs.k8s.io/docs/drivers/

## 3. Starting the cluster & deploy a service

- Start a Kubernetes cluster with minikube

```
$ minikube start        # Start the k8 cluster

$ minikube status       # Check the status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

- Now that the cluster is started. We can use kubectl to control the cluster.

- Bellow commands we can get the nodes running. Right now there are only 1 node (my local machine).

```
$ kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   3m11s   v1.34.0
```

- Create a new deployment: A deployment manages Pods for you. Think of it like a manager of pods. It handles Scaling (replicas), Updates (rolling updates), Self-healing (recreate failed pods).

- Rather than directly interacting with pods managed by a deployment. We must interact with the deployment.

```
$ kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
```

- Get all deployments

```
$ kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
hello-minikube   1/1     1            1           33m
```

- Bellow command creates a service. A service exposes your pods to the outside giving it a stable network endpoint exposing your app ([docs](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download#Service:~:text=4-,Deploy%20applications,-Service))
  1. Gives a stable network endpoint
  2. Routes traffic to Pods
  3. Load balances between Pods

```
$ kubectl expose deployment hello-minikube --type=NodePort --port=8080

or

$ kubectl expose deployment hello-minikube --type=NodePort --port=8081 --target-port=8080
```

- Now we can get the URL for the service

```
$ minikube service hello-minikube --url
http://192.168.49.2:31779
```

![alt text](image.png)

### Question: NodePort vs Port vs TargetPort

- TargetPort (optional) = Port inside the Pod which is your app is running on 8080.

- Port = Port of the Service (internal cluster port). Other services inside cluster use this (8081). This option (`port`) will be used for both pod and cluster port mappings if TargetPort is not provided.

- NodePort = auto (31779). This is what accessable by us.

```
Browser → 192.168.49.2:31779 (NodePort)
        → Service:8081
        → Pod:8080
```

## 4. Cleanup

```
$ kubectl delete services hello-minikube

$ kubectl delete deployment hello-minikube

$ kubectl get pods
No resources found in default namespace.
```

- Now http://192.168.49.2:31779 will no longer work since the application is stopped.
