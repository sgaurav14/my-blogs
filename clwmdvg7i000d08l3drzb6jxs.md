---
title: "Mastering Kubernetes Pods: A Practical Guide for Beginners"
datePublished: Sat May 25 2024 17:27:18 GMT+0000 (Coordinated Universal Time)
cuid: clwmdvg7i000d08l3drzb6jxs
slug: mastering-kubernetes-pods-a-practical-guide-for-beginners
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716657957435/e538b32d-380e-4ca2-ac0c-f7d6ce98e098.jpeg
tags: kubernetes, devops, containers, containerization, k8s, pods, container-orchestration, k8scluster

---

Welcome back, Kubernetes enthusiasts! In this guide, we'll delve into the practical aspects of working with Pods in Kubernetes, building upon the theory we covered in our previous blog post. If you haven't already, be sure to check out our last blog post where we introduced Kubernetes, explained what Pods are, and even guided you through installing Minikube on CentOS 7. You can find it \[here\](insert link to previous blog post). We'll be using the knowledge and skills you gained there as a foundation for the hands-on lab we'll be conducting in this tutorial.

### **1\. Understanding Pods and YAML Manifests**

**Simple Pod YAML:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

In this example, we have a basic YAML manifest for a Pod named `simple-pod`. It consists of a single container running the Nginx image with port 80 exposed.

Let's deploy this nginx pod. And check whether it is running or not.

```basic
[root@sid-vm pods]# kubectl apply -f first_pod.yml
pod/simple-pod created
[root@sid-vm pods]#
```

Let's check whether it is running or not.

```basic
[root@sid-vm pods]# kubectl get pods
NAME         READY   STATUS    RESTARTS   AGE
simple-pod   1/1     Running   0          32s
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods -o wide
NAME         READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
simple-pod   1/1     Running   0          35s   10.244.0.6   minikube   <none>           <none>
[root@sid-vm pods]#
```

Hurray! we successfully our first pod which is running nginx.

### **2\. Using Annotations in Pods**

**Pod with Annotations:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: annotated-pod
  annotations:
    description: "A sample Pod with annotations"
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

Here, we've added annotations to the Pod metadata section to provide additional context or information about the Pod.

Now, let's deploy this pod.

```basic
[root@sid-vm pods]# kubectl apply -f annotation_pod.yml
pod/annotated-pod created
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods
NAME            READY   STATUS    RESTARTS   AGE
annotated-pod   1/1     Running   0          33s
simple-pod      1/1     Running   0          30h
[root@sid-vm pods]#
```

If we have to see what annotation we have given to this pod, we can check this via `kubectl describe pod podname` command. We can see all the details of the pod from this command.

```basic
[root@sid-vm pods]# kubectl describe pod annotated-pod
Name:             annotated-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Sat, 25 May 2024 12:41:26 -0400
Labels:           <none>
Annotations:      description: A sample Pod with annotations
Status:           Running
IP:               10.244.0.4
IPs:
  IP:  10.244.0.4
Containers:
  nginx-container:
    Container ID:   docker://96c9bfca84547290c8839c9b1de8f29be328b8144ca2116c8e1689146f7b9601
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:a484819eb60211f5299034ac80f6a681b06f89e65866ce91f356ed7c72af059c
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sat, 25 May 2024 12:41:31 -0400
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-z9xsb (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-z9xsb:
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
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  108s  default-scheduler  Successfully assigned default/annotated-pod to minikube
  Normal  Pulling    106s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     103s  kubelet            Successfully pulled image "nginx:latest" in 3.069s (3.069s including waiting). Image size: 187634551 bytes.
  Normal  Created    103s  kubelet            Created container nginx-container
  Normal  Started    103s  kubelet            Started container nginx-container
[root@sid-vm pods]#
```

### **3\. Multi-Container Pods**

**Multi-Container Pod:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: c00
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo Hello Siddhartha; sleep 5; done"]

  - name: c01
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo Hello Gaurav; sleep 5; done"]
```

In this example, we define a Pod with two containers: one running Nginx and another running a sidecar container.

It's time to deploy multicontainer pod.

```basic
[root@sid-vm pods]# kubectl apply -f multicontainer_pod.yml
pod/multi-container-pod created
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
multi-container-pod   2/2     Running   0          55s
[root@sid-vm pods]#
```

Let's see the description of this pod. We are seeing the bottom 20 lines of this command output.

```basic
[root@sid-vm pods]# kubectl describe pod multi-container-pod | tail -20f
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  2m8s  default-scheduler  Successfully assigned default/multi-container-pod to minikube
  Normal  Pulling    2m6s  kubelet            Pulling image "ubuntu"
  Normal  Pulled     2m3s  kubelet            Successfully pulled image "ubuntu" in 2.986s (2.986s including waiting). Image size: 76233864 bytes.
  Normal  Created    2m3s  kubelet            Created container c00
  Normal  Started    2m2s  kubelet            Started container c00
  Normal  Pulling    2m2s  kubelet            Pulling image "ubuntu"
  Normal  Pulled     2m    kubelet            Successfully pulled image "ubuntu" in 2.97s (2.97s including waiting). Image size: 76233864 bytes.
  Normal  Created    119s  kubelet            Created container c01
  Normal  Started    119s  kubelet            Started container c01
[root@sid-vm pods]#
```

From the above output, we clearly saw that this pod has two containers created inside it.

Suppose a case, if you want to see the logs of this pod. Let's run our usual command:

```basic
[root@sid-vm pods]# kubectl logs -f multi-container-pod
Defaulted container "c00" out of: c00, c01
Hello Siddhartha
Hello Siddhartha
Hello Siddhartha
```

So by default, it is picking the 1st container log to display. If you want any specific container log, you have to specify that container name with `-c` option.

```basic
[root@sid-vm pods]# kubectl logs -f multi-container-pod -c c01
Hello Gaurav
Hello Gaurav
Hello Gaurav
Hello Gaurav
Hello Gaurav
Hello Gaurav
Hello Gaurav
Hello Gaurav
```

### **4\. Environment Variables in Pods**

**Pod with Environment Variables:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-var-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    env:
    - name: ENV_VAR_NAME
      value: "siddhartha"
```

Here, we've added an environment variable `ENV_VAR_NAME` with a value to the Pod's container.

Let's deploy our above pod in our minikube cluster.

```basic
[root@sid-vm pods]# kubectl apply -f env_pod.yml
pod/env-var-pod created
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods
NAME          READY   STATUS    RESTARTS   AGE
env-var-pod   1/1     Running   0          5s
[root@sid-vm pods]#
```

We have declared a environment variable in the above pod, and to verify the assignment of that environment variable and its value, we need to login to the pod first. So, finally we are going to learn, how to login to the pods in interactive mode.

We will use `kubectl exec env-var-pod -it -- /bin/bash` command, where `exec` is the option to do execution on the pod, `-it` means interactive terminal and `-- /bin/bash` is for getting a bash shell.

```basic
[root@sid-vm pods]# kubectl exec env-var-pod -it -- /bin/bash
root@env-var-pod:/#
```

Let's verify our environment value that we set from our above manifest file.

```basic
[root@sid-vm pods]# kubectl exec env-var-pod -it -- /bin/bash
root@env-var-pod:/#
root@env-var-pod:/# echo $ENV_VAR_NAME
siddhartha
root@env-var-pod:/#
```

Hurray! we can see our configured environment value inside the pod.

### **5\. Exposing Ports in Pods**

**Pod with Port Exposed:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: port-expose-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

This YAML manifest defines a Pod with port 8080 exposed on the container.

It's time to deploy this pod as well and see the magic.

```basic
[root@sid-vm pods]# kubectl apply -f port_pod.yml
pod/port-expose-pod created
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods
NAME              READY   STATUS    RESTARTS   AGE
port-expose-pod   1/1     Running   0          36s
[root@sid-vm pods]#
```

Our pod has running nginx on port 8080. If we want to verify the response of our nginx pod, we have to run the command: `curl pod-ip:port`.

Let's get the IP of the pod.

```basic
[root@sid-vm pods]# kubectl get pods -o wide
NAME              READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
port-expose-pod   1/1     Running   0          3m50s   10.244.0.10   minikube   <none>           <none>
[root@sid-vm pods]#
```

And also in the case of minikube cluster, we need to login to the cluster by running `minikube ssh` command. And inside the cluster, run the above curl command accordingly.

```basic
[root@sid-vm pods]# minikube ssh
docker@minikube:~$ curl 10.244.0.10:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
docker@minikube:~$
```

Perfect, our nginx pod is responding with its default page. In this way, we can expose the ports in the pods.

### **6\. Conclusion**

By understanding these practical examples and experimenting with different configurations, you're well on your way to mastering Pods in Kubernetes. Stay tuned for more tutorials and guides as we explore additional Kubernetes concepts and advanced features!

Happy podding!