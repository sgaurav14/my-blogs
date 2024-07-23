---
title: "Understanding Health Checks and Liveness Probes in Kubernetes: A Beginner's Guide"
datePublished: Tue Jul 23 2024 12:18:35 GMT+0000 (Coordinated Universal Time)
cuid: clyydtown00000ajza9s9cqsj
slug: understanding-health-checks-and-liveness-probes-in-kubernetes-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721652564280/2805d634-1be2-4d73-a0f4-b9dfea818f2a.png
tags: kubernetes, kubernetes-container, kubernetes-architecture, kubernetes-livenessprobe, kubeweekchallenge, kubernetes-readiness-probe, healthcheck

---

Ensuring your applications are running smoothly and can recover from failures is a crucial part of maintaining a reliable system. Kubernetes provides built-in mechanisms to help with this, called health checks and liveness probes. In this guide, we’ll break down what these are, why they’re important, and how to implement them with simple examples.

### What Are Health Checks and Liveness Probes?

**Health checks** in Kubernetes are used to determine the state of your application. They help Kubernetes decide whether your application is running as expected, needs to be restarted, or should be removed from service.

There are two main types of probes used for health checks:

1. **Liveness Probes**: Determine if your application is alive. If the liveness probe fails, Kubernetes will restart the container.
    
2. **Readiness Probes**: Determine if your application is ready to serve traffic. If the readiness probe fails, Kubernetes will stop sending traffic to the container.
    

### Why Are They Important?

* **Automatic Recovery**: If an application becomes unresponsive, liveness probes can restart it automatically.
    
* **Load Balancing**: Readiness probes ensure traffic is only sent to instances that are ready to handle requests.
    
* **Reduced Downtime**: Helps in minimizing application downtime and maintaining high availability.
    

### Types of Probes

Kubernetes supports three types of probes:

1. **HTTP Probes**: Use an HTTP GET request to check the health of an application.
    
2. **TCP Probes**: Perform a TCP check to ensure the application is running.
    
3. **Exec Probes**: Execute a command inside the container to check its health.
    

### Example: Implementing Liveness and Readiness Probes

Let’s look at a basic example to see how to implement liveness and readiness probes in a Kubernetes Deployment.

#### Step 1: Define Your Application Deployment

Here’s a simple Deployment YAML file for an NGINX application:

```bash
[root@k8s-master ~]# cat pod-monitor.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
        image: nginx:1.19.6
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds:
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 10
[root@k8s-master ~]#
```

#### Step 2: Deploy and Verify

Apply the deployment:

```sh
[root@k8s-master ~]# kubectl apply -f pod-monitor.yml
deployment.apps/nginx-deployment created
[root@k8s-master ~]#
```

Verify the probes:

```sh
[root@k8s-master ~]# kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1     1            1           2m27s
[root@k8s-master ~]#
[root@k8s-master ~]# kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-568c8d488d-z2p9t   1/1     Running   0          2m30s
[root@k8s-master ~]#
[root@k8s-master ~]# kubectl describe pod nginx-deployment-568c8d488d-z2p9t
Name:             nginx-deployment-568c8d488d-z2p9t
Namespace:        default
Priority:         0
Service Account:  default
Node:             k8s-worker1.ha.stalab.ciena.com/10.121.230.194
Start Time:       Mon, 22 Jul 2024 08:40:44 -0400
Labels:           app=nginx
                  pod-template-hash=568c8d488d
Annotations:      cni.projectcalico.org/containerID: 8621a614c5028d1d4a9fd644f4828cfcf25b662b8921333f2b99de73f752cc10
                  cni.projectcalico.org/podIP: 192.168.224.17/32
                  cni.projectcalico.org/podIPs: 192.168.224.17/32
Status:           Running
IP:               192.168.224.17
IPs:
  IP:           192.168.224.17
Controlled By:  ReplicaSet/nginx-deployment-568c8d488d
Containers:
  nginx:
    Container ID:   containerd://6b655adbd150c9c3006517c2b5293fbe08f0bcf3ec0a5baeb6193e930c75a62e
    Image:          nginx:1.19.6
    Image ID:       docker.io/library/nginx@sha256:8e10956422503824ebb599f37c26a90fe70541942687f70bbdb744530fc9eba4
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 22 Jul 2024 08:41:07 -0400
    Ready:          True
    Restart Count:  0
    Liveness:       http-get http://:80/ delay=0s timeout=1s period=10s #success=1 #failure=3
    Readiness:      http-get http://:80/ delay=5s timeout=1s period=10s #success=1 #failure=3
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-xcclq (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  kube-api-access-xcclq:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m36s  default-scheduler  Successfully assigned default/nginx-deployment-568c8d488d-z2p9t to k8s-worker1.ha.stalab.ciena.com
  Normal  Pulling    2m36s  kubelet            Pulling image "nginx:1.19.6"
  Normal  Pulled     2m14s  kubelet            Successfully pulled image "nginx:1.19.6" in 22.102s (22.102s including waiting)
  Normal  Created    2m14s  kubelet            Created container nginx
  Normal  Started    2m14s  kubelet            Started container nginx
[root@k8s-master ~]#
```

We can see the Liveness and Readiness configured for this deployment.

### Example: TCP and Exec Probes

For applications that don’t use HTTP, you can use TCP or Exec probes. Here’s an example using a TCP probe:

```yaml
livenessProbe:
  tcpSocket:
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 20
```

And here’s an example using an Exec probe:

```yaml
livenessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

In this Exec probe example, Kubernetes will execute the `cat /tmp/healthy` command inside the container. If the command exits with a status code of 0, the container is considered healthy.

### Best Practices

* **Keep Probes Simple**: Ensure the probe checks are lightweight and fast.
    
* **Grace Periods**: Use `initialDelaySeconds` and `periodSeconds` to give your application enough time to start and stabilize.
    
* **Monitor Probe Failures**: Use Kubernetes monitoring tools to keep an eye on probe failures and understand why they happen.
    

### Conclusion

Health checks and liveness probes are essential tools in Kubernetes for maintaining the health and availability of your applications. By configuring them correctly, you can ensure your applications are always running smoothly and can recover quickly from failures.

We hope this guide helps you get started with implementing health checks and liveness probes in your Kubernetes deployments. Happy coding!