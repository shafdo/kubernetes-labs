## 1. Start Kubernetes Cluster

```
$ minikube start
```

## 2. Run a Nginx pod and view information about it

```
$ kubectl run nginx --image nginx

$ kubectl get pods
NAME    READY   STATUS              RESTARTS   AGE
nginx   0/1     ContainerCreating   0          23s

$ kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          5m18s   10.244.0.5   minikube   <none>           <none>
```

## 3. More detailed description

```
$ kubectl describe pod nginx
kubectl describe pod nginx
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 08 Oct 2025 01:03:41 +0530
Labels:           run=nginx
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx:
    Container ID:   docker://c0a700ce721ec73736e820f6a4df058bc56c3e3a76480c56649e96a96cb15e17
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:8adbdcb969e2676478ee2c7ad333956f0c8e0e4c5a7463f4611d7a2e7a7ff5dc
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 08 Oct 2025 01:04:09 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xpd86 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-xpd86:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m9s   default-scheduler  Successfully assigned default/nginx to minikube
  Normal  Pulling    4m8s   kubelet            Pulling image "nginx"
  Normal  Pulled     3m41s  kubelet            Successfully pulled image "nginx" in 26.956s (26.956s including waiting). Image size: 192385293 bytes.
  Normal  Created    3m41s  kubelet            Created container: nginx
  Normal  Started    3m41s  kubelet            Started container nginx
```

## 4. Delete the POD

```
$ kubectl delete pod nginx
pod "nginx" deleted from default namespace
```
