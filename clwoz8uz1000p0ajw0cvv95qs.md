---
title: "Simplifying Labels, Selectors, and Node Selectors in Kubernetes"
datePublished: Mon May 27 2024 13:01:08 GMT+0000 (Coordinated Universal Time)
cuid: clwoz8uz1000p0ajw0cvv95qs
slug: simplifying-labels-selectors-and-node-selectors-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716811063306/f1001203-9b8d-4e9d-b633-15cd5047570c.png
tags: kubernetes, selectors, labels, k8s, kubernetes-container, kubernetes-architecture, k8s-commands, kubernetes-node-selector

---

In the dynamic world of Kubernetes, managing your applications efficiently requires mastering the concepts of labels, selectors, and node selectors. These foundational elements serve as the building blocks for organizing, identifying, and deploying resources within your Kubernetes cluster. Let's explore these concepts in more detail, breaking them down into easy-to-understand explanations with practical examples for beginners.

### **Labels: Adding Meaningful Tags to Your Kubernetes Objects**

Labels are like descriptive tags that you attach to various Kubernetes objects such as pods, services, and deployments. They provide metadata that helps Kubernetes understand the purpose, ownership, or any other distinguishing characteristics of each object.

#### **Example:**

Imagine you have several pods running different parts of your application. You can label them with information like `app: frontend`, `app: backend`, or `environment: production`. These labels act as markers, allowing Kubernetes to organize and manage your resources effectively.

```yaml
# Example of labeling a pod
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  labels:
    app: frontend
    environment: production
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

Let's deploy the above pod with the configured labels as per above manifest file.

```bash
[root@sid-vm labels-selectors]# kubectl apply -f label.yml
pod/frontend-pod created
[root@sid-vm labels-selectors]#
[root@sid-vm labels-selectors]# kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
frontend-pod   1/1     Running   0          87s
[root@sid-vm labels-selectors]#
```

If you want to check what labels you have configured on your pods, run command `kubectl get pods --show-labels`.

```bash
[root@sid-vm labels-selectors]# kubectl get pods --show-labels
NAME           READY   STATUS    RESTARTS   AGE     LABELS
frontend-pod   1/1     Running   0          3m11s   app=frontend,environment=production
[root@sid-vm labels-selectors]#
```

We can see the labels name that we configured from the manifest file. Now take a scenario where you have to configured a label imperatively to existing pod. Then in this case, we use command - `kubectl label pod podname key=value`

```bash
[root@sid-vm labels-selectors]# kubectl label pods frontend-pod myname=sid
pod/frontend-pod labeled
[root@sid-vm labels-selectors]#
[root@sid-vm labels-selectors]# kubectl get pods --show-labels
NAME           READY   STATUS    RESTARTS   AGE     LABELS
frontend-pod   1/1     Running   0          6m35s   app=frontend,environment=production,myname=sid
[root@sid-vm labels-selectors]#
```

Here we see that after running the imperative command, the pods get labelled and we see the new label in our output.

### **Selectors: Finding Your Resources with Precision**

Selectors are like search queries that Kubernetes uses to locate resources based on their labels. They enable you to filter and identify specific objects within your cluster.

#### **Equality-Based Selectors:**

Think of equality-based selectors as exact matches. You specify a label, and Kubernetes finds resources that exactly match that label. (= \[eqaual to\], != \[not equal to\])

```yaml
# Example of equality-based selector
selector:
  matchLabels:
    app: frontend
```

Let's try to do filtering from command line interface by giving some label name and its value - `kubectl get pods -l key=value`

```bash
[root@sid-vm pods]# kubectl get pods -l myname=sid
NAME           READY   STATUS    RESTARTS   AGE
frontend-pod   1/1     Running   0          59s
[root@sid-vm pods]#
```

In the above example, we are finding the pos labeled `myname: sid`. You can use an equality-based selector to instruct Kubernetes to retrieve pods with that exact label.. Now take a example where we check for the key is not equal to value. The command make clear your concept - `kubectl get pofs -f key!=value` (key value should not match).

```bash
[root@sid-vm pods]# kubectl get pods -l app!=frontend
NAME         READY   STATUS    RESTARTS   AGE
simple-pod   1/1     Running   0          6m59s
[root@sid-vm pods]#
```

And from the above two commands, we can filter the pods with the labels by Equality based.

#### **Set-Based Selectors:**

Set-based selectors offer more flexibility by allowing you to define criteria for label matching using set operations like `in`, `notIn`, `exists`, and `doesNotExist`. This allows you to perform more nuanced searches based on multiple labels or label values.

```yaml
# Example of set-based selector
selector:
  matchExpressions:
    - key: environment
      operator: In
      values:
        - production
        - staging
```

We can use set-based selectors to search for pods based on more complex criteria, such as those belonging to either the `production` or `staging` environment.

Let's do a hands-on for the same -  
1\. In this case we are trying to check the setbased, `in` operation where it can check for all the options that you will provide, and if any of the option matches in any pod, it will give you the result.

```bash
[root@sid-vm pods]# kubectl get pods -l 'environment in (production, test)'
NAME           READY   STATUS    RESTARTS   AGE
frontend-pod   1/1     Running   0          10m
[root@sid-vm pods]#
```

2\. In this case we are trying to check the setbased, `notin` operation where it can check for all the options that you will provide, and if any of the option matches it will not exclude that pod and show all the other pods which don't have those labels configured.

```bash
[root@sid-vm pods]# kubectl get pods -l 'environment notin (production, test)'
NAME         READY   STATUS    RESTARTS   AGE
simple-pod   1/1     Running   0          14m
[root@sid-vm pods]#
```

3\. In this case we are trying to check the setbased, `exist`operation where it can check for all the options that you will provide, and if all the labels that you have given matched to the pod, it will provide that pod details in the output. But if one of them got mismatched, it will exclude that pod.

```bash
[root@sid-vm pods]# kubectl get pods -l environment=production,myname=sid
NAME           READY   STATUS    RESTARTS   AGE
frontend-pod   1/1     Running   0          11m
[root@sid-vm pods]#
```

### **Node Selectors: Choosing the Right Environment for Your Pods**

Node selectors help Kubernetes determine which nodes in your cluster should host your pods. They allow you to specify requirements or preferences for the nodes on which your pods should run.

#### **Example:**

Let's say you have pods that require specific hardware capabilities, like SSD storage. You can use node selectors to instruct Kubernetes to schedule these pods only on nodes that meet the specified criteria, ensuring optimal performance and resource utilization.

```yaml
# Example of node selector
apiVersion: v1
kind: Pod
metadata:
  name: ssd-pod
  labels:
      env: dev
spec:
  containers:
  - name: c00
    image: ubuntu
    command: ["/bin/bash", "-c", "while true; do echo Hello Siddhartha; sleep 5; done"]

  nodeSelector:
     disktype: ssd
```

Let's first see, what are the labels labelled on our current node `kubectl get nodes --show-labels` -

```bash
[root@sid-vm pods]# kubectl get nodes --show-labels
NAME       STATUS   ROLES           AGE    VERSION   LABELS
minikube   Ready    control-plane   3d2h   v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux,minikube.k8s.io/commit=5883c09216182566a63dff4c326a6fc9ed2982ff,minikube.k8s.io/name=minikube,minikube.k8s.io/primary=true,minikube.k8s.io/updated_at=2024_05_24T04_59_53_0700,minikube.k8s.io/version=v1.33.1,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
[root@sid-vm pods]#
[root@sid-vm pods]#
```

Now, let'd deploy our above manifest file which will deploy the pod on the node which have labelling of disktype=ssd.

```bash
[root@sid-vm pods]# kubectl apply -f node-selector.yml
pod/ssd-pod created
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl get pods
NAME      READY   STATUS    RESTARTS   AGE
ssd-pod   0/1     Pending   0          53s
[root@sid-vm pods]#
```

We can see the from above output that it is not able to create the pod and it is pending state. Let's see the description for this pod -

```bash
[root@sid-vm pods]# kubectl describe pod ssd-pod | tail -10
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              disktype=ssd
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  3m21s  default-scheduler  0/1 nodes are available: 1 node(s) didn't match Pod's node affinity/selector. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
[root@sid-vm pods]#
```

Uff! it is not able to create because it is not able to find the node having label of disktype as ssd.

Let's give this label to our node. The command for applying a label to any node is `kubectl label node nodename key=value`.

```bash
[root@sid-vm pods]# kubectl label node minikube disktype=ssd
node/minikube labeled
[root@sid-vm pods]# 
[root@sid-vm pods]# kubectl get nodes --show-labels
NAME       STATUS   ROLES           AGE    VERSION   LABELS
minikube   Ready    control-plane   3d2h   v1.30.0   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,disktype=ssd,kubernetes.io/arch=amd64,kubernetes.io/hostname=minikube,kubernetes.io/os=linux,minikube.k8s.io/commit=5883c09216182566a63dff4c326a6fc9ed2982ff,minikube.k8s.io/name=minikube,minikube.k8s.io/primary=true,minikube.k8s.io/updated_at=2024_05_24T04_59_53_0700,minikube.k8s.io/version=v1.33.1,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=
[root@sid-vm pods]# 
```

Now after running the above command, we can see that our node has the label of disktype set to ssd. Let's see the status of our pod, it should be deployed now and in be state of up and running.

```bash
[root@sid-vm pods]# kubectl get pods
NAME      READY   STATUS    RESTARTS   AGE
ssd-pod   1/1     Running   0          9m
[root@sid-vm pods]#
[root@sid-vm pods]# kubectl describe pod ssd-pod | tail -10
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age                    From               Message
  ----     ------            ----                   ----               -------
  Warning  FailedScheduling  4m18s (x2 over 9m20s)  default-scheduler  0/1 nodes are available: 1 node(s) didn't match Pod's node affinity/selector. preemption: 0/1 nodes are available: 1 Preemption is not helpful for scheduling.
  Normal   Scheduled         3m22s                  default-scheduler  Successfully assigned default/ssd-pod to minikube
  Normal   Pulling           3m20s                  kubelet            Pulling image "ubuntu"
  Normal   Pulled            3m17s                  kubelet            Successfully pulled image "ubuntu" in 3.133s (3.133s including waiting). Image size: 76233864 bytes.
  Normal   Created           3m17s                  kubelet            Created container c00
  Normal   Started           3m17s                  kubelet            Started container c00
[root@sid-vm pods]#
```

Hurray! It find the node having the label which it was looking for and deployed our requested pod.

### **Conclusion: Empowering Your Kubernetes Journey**

By understanding the concepts of labels, selectors, and node selectors, you gain a powerful toolkit for managing and orchestrating your Kubernetes resources. These concepts provide the foundation for organizing your applications, locating specific resources within your cluster, and optimizing resource allocation based on your requirements. As you continue your Kubernetes journey, mastering these fundamental concepts will enable you to build resilient, scalable, and efficient deployments with confidence.