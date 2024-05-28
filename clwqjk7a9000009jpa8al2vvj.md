---
title: "Demystifying Replication Controller and ReplicaSet in Kubernetes"
datePublished: Tue May 28 2024 15:17:36 GMT+0000 (Coordinated Universal Time)
cuid: clwqjk7a9000009jpa8al2vvj
slug: demystifying-replication-controller-and-replicaset-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716909387294/8bedb107-c139-4537-92be-eb85c6cc21ca.png
tags: kubernetes, devops, kubernetes-container, replicaset, kubernetes-architecture, kuberentes-scaling, replication-controller

---

In the realm of Kubernetes, managing containerized applications efficiently is paramount. Two key components that play a crucial role in ensuring the availability and scalability of applications are Replication Controllers and ReplicaSets. In this beginner-friendly guide, we'll delve into these concepts, unraveling their significance and providing practical examples for better understanding.

### **Understanding Replication Controller**

At its core, a Replication Controller (RC) is a Kubernetes resource responsible for ensuring that a specified number of pod replicas are running at all times. It serves as a basic mechanism for maintaining the desired number of identical pod instances, thereby enhancing fault tolerance and high availability.

#### **Key Features of Replication Controller:**

1. **Pod Replication:** Replication Controllers ensure that a specified number of pod replicas are running, automatically replacing any pods that fail or are terminated.
    
2. **Scaling:** They facilitate horizontal scaling by allowing you to increase or decrease the number of pod replicas based on demand.
    
3. **Self-Healing:** Replication Controllers continuously monitor the state of pods and take corrective actions to maintain the desired replica count, ensuring resilience against failures.
    

### **Example: Deploying a Replication Controller**

Let's dive into a practical example to deploy a Replication Controller in Kubernetes using a YAML manifest file:

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-controller
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```

In this example:

* We define a Replication Controller named `nginx-controller` with a desired replica count of 3.
    
* The selector ensures that the Replication Controller manages pods labeled with `app: nginx`.
    
* The pod template specifies the configuration for each pod, including the container image (`nginx:latest`).
    

Let's deploy this pod in our cluster and see if we have the desired number of replicas are running or not.

```bash
[root@sid-vm rs]# kubectl apply -f rc.yml
replicationcontroller/nginx-controller created
[root@sid-vm rs]#
```

So now we see that a new resource object is created which is known as replicationcontroller. To check, how many replicationcontrollers are running, use command - `kubectl get rc` or `kubectl get replicationcontroller.`Let's get some info of our this replication controller pod.

```bash
[root@sid-vm rs]# kubectl get rc
NAME               DESIRED   CURRENT   READY   AGE
nginx-controller   3         3         3       81s
[root@sid-vm rs]#
[root@sid-vm rs]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-controller-ldnlm   1/1     Running   0          3m24s
nginx-controller-lvlkg   1/1     Running   0          3m24s
nginx-controller-wxtfj   1/1     Running   0          3m24s
[root@sid-vm rs]#
[root@sid-vm rs]# kubectl describe rc nginx-controller
Name:         nginx-controller
Namespace:    default
Selector:     app=nginx
Labels:       app=nginx
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx-container:
    Image:         nginx:latest
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age    From                    Message
  ----    ------            ----   ----                    -------
  Normal  SuccessfulCreate  4m44s  replication-controller  Created pod: nginx-controller-wxtfj
  Normal  SuccessfulCreate  4m44s  replication-controller  Created pod: nginx-controller-lvlkg
  Normal  SuccessfulCreate  4m44s  replication-controller  Created pod: nginx-controller-ldnlm
[root@sid-vm rs]#
```

We can see the pods naming-convention for my above manifest file is `replicationcontrollername-some_string`.

If you wan to delete the created exising replication controller, use command - `kubectl delete rc rc-name`.

```bash
[root@sid-vm rs]# kubectl delete rc nginx-controller
replicationcontroller "nginx-controller" deleted
[root@sid-vm rs]#
[root@sid-vm rs]# kubectl get rc
No resources found in default namespace.
[root@sid-vm rs]#
```

### **Understanding ReplicaSet**

ReplicaSet is an evolution of the Replication Controller, offering more advanced features and capabilities. It serves the same purpose of ensuring a specified number of pod replicas but introduces enhanced selectors for more granular control and support for scaling.

#### **Key Features of ReplicaSet:**

1. **Enhanced Selectors:** ReplicaSets support more powerful label selectors, enabling more flexible and precise control over the pods they manage.
    
2. **Scaling:** They facilitate horizontal scaling by allowing you to adjust the replica count dynamically based on demand.
    
3. **Rolling Updates:** ReplicaSets support rolling updates, ensuring zero downtime during the update process by gradually replacing existing pods with new ones.
    

### **Example: Deploying a ReplicaSet with Scaling**

Let's deploy a ReplicaSet in Kubernetes using a YAML manifest file and demonstrate scaling:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```

In this example:

* We define a ReplicaSet named `nginx-replicaset` with a desired replica count of 3.
    
* The selector uses matchLabels to ensure that the ReplicaSet manages pods labeled with `app: nginx`.
    
* The pod template specifies the configuration for each pod, including the container image (`nginx:latest`).
    

We will deploy our above replica-set manifest file by command `kubectl apply -f replicaet.yml`. To check how many replica-sets are running, use command `kubectl get rs` or `kubectl get replicaset`.

```bash
[root@sid-vm replica-set]# kubectl apply -f replicaset.yml
replicaset.apps/nginx-replicaset created
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         1       7s
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       15s
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-lcgtl   1/1     Running   0          98s
nginx-replicaset-r9tvg   1/1     Running   0          98s
nginx-replicaset-sqvpn   1/1     Running   0          98s
[root@sid-vm replica-set]#
```

We can see the pods naming-convention for my above manifest file is `replicaset-name-some_string`.

Let's see spme description for the above replica-set object.

```bash
[root@sid-vm replica-set]# kubectl describe rs nginx-replicaset
Name:         nginx-replicaset
Namespace:    default
Selector:     app=nginx
Labels:       <none>
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx-container:
    Image:         nginx:latest
    Port:          <none>
    Host Port:     <none>
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: nginx-replicaset-r9tvg
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: nginx-replicaset-sqvpn
  Normal  SuccessfulCreate  70s   replicaset-controller  Created pod: nginx-replicaset-lcgtl
[root@sid-vm replica-set]#
```

### **Scaling Replicas**

To scale the number of replicas, you can update the `replicas` field in the ReplicaSet manifest:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 5  # Update the number of replicas to 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
```

By changing the value of `replicas` to 5, Kubernetes will automatically adjust the number of pod replicas to meet the new desired state.

Let's apply our above change to our existing running nginx-replicaset application.

But before let's see the output of number of pods currently running before applying the above changes.

```bash
[root@sid-vm replica-set]# kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   3         3         3       4m4s
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-lcgtl   1/1     Running   0          4m9s
nginx-replicaset-r9tvg   1/1     Running   0          4m9s
nginx-replicaset-sqvpn   1/1     Running   0          4m9s
[root@sid-vm replica-set]#
```

Now, let's apply the changes and check the above thing again.

```bash
[root@sid-vm replica-set]# kubectl apply -f replicaset.yml
replicaset.apps/nginx-replicaset configured
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   5         5         5       5m7s
[root@sid-vm replica-set]#
[root@sid-vm replica-set]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-9wgqv   1/1     Running   0          16s
nginx-replicaset-fwkfc   1/1     Running   0          16s
nginx-replicaset-lcgtl   1/1     Running   0          5m11s
nginx-replicaset-r9tvg   1/1     Running   0          5m11s
nginx-replicaset-sqvpn   1/1     Running   0          5m11s
[root@sid-vm replica-set]#
```

And Hurray! our application scaled up successfully. We can see that now 5 replicas of our application pod is running.

We can acheive the same above by running the imperative command on our replication-set manifest file. Let's do this and assume, now we have no load on the application and we want to reduce the pods count to 1. To achieve the same, run command - `kubectl scale --replicas=1 rs/replicaset-name`.

```bash
[root@sid-vm replica-set]# kubectl scale --replicas=1 rs/nginx-replicaset
replicaset.apps/nginx-replicaset scaled
[root@sid-vm replica-set]# kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
nginx-replicaset   1         1         1       8m37s
[root@sid-vm replica-set]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-replicaset-lcgtl   1/1     Running   0          8m41s
[root@sid-vm replica-set]#
```

Wow, we successfully sclaed down our application. So from the scalig option, we can scale-up or scale-down our replicas according to the load on the application.

### **Conclusion: Mastering Replication in Kubernetes**

Replication Controller and ReplicaSet are fundamental components in Kubernetes that facilitate the reliable and scalable deployment of containerized applications. By understanding their purpose, features, and practical usage, beginners can harness the power of Kubernetes for resilient and efficient application management.

Stay tuned for more Kubernetes tutorials and guides as we continue to explore the rich ecosystem of container orchestration!

Happy deploying! 🚀