---
title: "Unlocking Kubernetes Deployment Mastery: A Beginner's Guide!"
datePublished: Mon Jun 03 2024 14:25:46 GMT+0000 (Coordinated Universal Time)
cuid: clwz2cnrr00060al52h6p5wfx
slug: unlocking-kubernetes-deployment-mastery-a-beginners-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1717424580853/4953214f-c6ef-4fb7-87d1-925ac008242c.png
tags: scaling, deployment, kubernetes, k8s, kubernetes-container, replicaset, kubernetes-architecture, pods, k8scluster, k8s-commands

---

Kubernetes, often abbreviated as K8s, is an open-source platform designed to automate the deployment, scaling, and operation of application containers. A **Deployment** in Kubernetes is an abstraction layer for managing a group of identical Pods. Pods are the smallest deployable units of computing that can be created and managed in Kubernetes.

## Features of Kubernetes Deployment

### 1\. Declarative Updates

In Kubernetes, you describe the desired state of your application using a YAML or JSON file. This file, called a **manifest**, includes details like the number of replicas (instances of your application) and the container image to use. Kubernetes ensures that the actual state matches your desired state. This is called **declarative configuration**.

### 2\. Rolling Updates

A rolling update allows you to update your application to a new version without downtime. Instead of stopping all old instances and starting new ones at once, Kubernetes updates the Pods incrementally. This ensures that your application remains available to users during the update.

### 3\. Rollbacks

If an update goes wrong (e.g., the new version has a bug), you can quickly revert to a previous stable version. Kubernetes keeps a history of your Deployment revisions, allowing you to rollback to an earlier version if necessary.

### 4\. Scaling

Scaling involves adjusting the number of replicas (instances) of your application to handle more or less traffic. With Kubernetes Deployments, you can easily increase or decrease the number of Pods.

### 5\. Self-healing

Kubernetes monitors the state of your Pods and automatically replaces any that fail or become unresponsive, ensuring your application is always running smoothly.

## Basic Commands for Deployment Management

Here are some essential commands you'll need to manage Deployments in Kubernetes:

### Create a Deployment

To create a Deployment, you use a YAML file that describes the desired state of your application. For example:

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Siddhartha; sleep 5; done"]
```

### Key Sections Explained

* **apiVersion**: Specifies the API version. For Deployments, this is typically `apps/v1`.
    
* **kind**: Indicates the type of resource, which is `Deployment`.
    
* **metadata**: Contains metadata about the Deployment, such as its name and labels.
    
* **spec**: Describes the desired state of the Deployment.
    
    * **replicas**: The number of Pod replicas to run.
        
    * **selector**: Defines how to identify the Pods managed by this Deployment.
        
    * **template**: Specifies the Pod template, defining the Pods to be created.
        
        * **metadata**: Contains metadata about the Pods.
            
        * **spec**: Describes the container specifications, including the container image and ports.
            

Apply the Deployment using the following command:

```sh
[root@sid-vm deployments]# kubectl apply -f first_deploy.yml
deployment.apps/mydeployments created
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl apply -f first_deploy.yml
deployment.apps/mydeployments created
[root@sid-vm deployments]#
```

### View Deployments

To see the Deployments in your cluster:

```sh
[root@sid-vm deployments]# kubectl get deployments
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
mydeployments   2/2     2            2           15s
[root@sid-vm deployments]#
```

### Update a Deployment

To update your Deployment (e.g., change the container image from ubuntu to centos), modify the `deployment.yaml` file and reapply it:

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 2
   selector:     
    matchLabels:
     name: deployment
   template:
     metadata:
       name: testpod
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: centos
          command: ["/bin/bash", "-c", "while true; do echo Siddhartha; sleep 5; done"]
```

```sh
[root@sid-vm deployments]# kubectl apply -f first_deploy.yml
deployment.apps/mydeployments configured
[root@sid-vm deployments]#
```

### Rollout Status

To check the status of a Deployment update:

```sh
[root@sid-vm deployments]# kubectl rollout status deployment/mydeployments
deployment "mydeployments" successfully rolled out
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
mydeployments-569b8c554-4v86d    1/1     Terminating   0          25m
mydeployments-569b8c554-rsxfp    1/1     Terminating   0          25m
mydeployments-85767d7855-l9bdc   1/1     Running       0          12s
mydeployments-85767d7855-tmqns   1/1     Running       0          47s

[root@sid-vm deployments]# kubectl exec mydeployments-85767d7855-tmqns -it -- /bin/bash
[root@mydeployments-85767d7855-tmqns /]#
[root@mydeployments-85767d7855-tmqns /]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"
[root@mydeployments-85767d7855-tmqns /]#
```

It terminate the older deployment pods and parallel bring up the new ones.

We verified that the new pod is running now on Centos 8 by logging to the pod.

### Rollback a Deployment

To rollback to a previous version of a Deployment:

```sh
[root@sid-vm deployments]# kubectl rollout undo deployment/mydeployments
deployment.apps/mydeployments rolled back
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
mydeployments-569b8c554-rjszj    1/1     Running       0          14s
mydeployments-569b8c554-zzqdb    1/1     Running       0          9s
mydeployments-85767d7855-l9bdc   1/1     Terminating   0          8m33s
mydeployments-85767d7855-tmqns   1/1     Terminating   0          9m8s
[root@sid-vm deployments]# kubectl exec mydeployments-569b8c554-rjszj -it -- /bin/bash
root@mydeployments-569b8c554-rjszj:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04 LTS"
NAME="Ubuntu"
VERSION_ID="24.04"
VERSION="24.04 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo
root@mydeployments-569b8c554-rjszj:/#
```

As we enter the command to rollback to the previous version, we see that the newly created pods got terminated and the older pods with ubuntu image came up with new pod Id.

### Scale a Deployment

To change the number of replicas:

```sh
[root@sid-vm deployments]# kubectl scale deployment/mydeployments --replicas=4
deployment.apps/mydeployments scaled
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl get pods
NAME                            READY   STATUS              RESTARTS   AGE
mydeployments-569b8c554-fnkbn   0/1     ContainerCreating   0          3s
mydeployments-569b8c554-n6zlh   0/1     ContainerCreating   0          3s
mydeployments-569b8c554-rjszj   1/1     Running             0          3m58s
mydeployments-569b8c554-zzqdb   1/1     Running             0          3m53s
[root@sid-vm deployments]# kubectl get deploy
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
mydeployments   4/4     4            4           37m
[root@sid-vm deployments]#
```

### View Deployment History

To see the history of a Deployment:

```sh
[root@sid-vm deployments]# kubectl rollout history deployment/mydeployments
deployment.apps/mydeployments
REVISION  CHANGE-CAUSE
2         <none>
3         <none>

[root@sid-vm deployments]#
```

## Handling Failed Deployments

Deployments can fail for various reasons, including:

* **Image Pull Errors**: The specified container image is not available or doesn't exist.
    
* **Resource Constraints**: Insufficient CPU or memory resources.
    
* **Configuration Errors**: Errors in the deployment manifest, such as incorrect environment variables.
    
* **Network Issues**: Problems with network connectivity or DNS resolution.
    

### Debugging Failed Deployments

**Example Manifest File**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: my-app:1.0
        ports:
        - containerPort: 80
```

Let's deploy the above deployment manifest.

```bash
[root@sid-vm deployments]# kubectl apply -f error_deploy.yml
deployment.apps/my-app created
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl get deploy
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
my-app   0/3     3            0           23s
[root@sid-vm deployments]#
```

We can see that we deployed the deployment but pods are not creating.

### Example of Handling a Failed Deployment

Suppose you deployed an application, but the Pods are not starting due to an image pull error. Here’s how you can debug and fix the issue:

1. **Get the list of Pods**
    
    ```sh
    [root@sid-vm deployments]# kubectl get pods
    NAME                      READY   STATUS              RESTARTS   AGE
    my-app-5b764c67f8-hd6kn   0/1     ImagePullBackOff    0          2m41s
    my-app-5b764c67f8-j8cf4   0/1     ImagePullBackOff    0          2m41s
    my-app-5b764c67f8-tx7ql   0/1     ImagePullBackOff    0          2m41s
    ```
    
2. **Describe the problematic Pod**
    
    ```sh
    [root@sid-vm deployments]# kubectl describe pod my-app-5b764c67f8-hd6kn | tail -20
    Volumes:
      kube-api-access-hvczl:
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
      Type     Reason     Age                  From               Message
      ----     ------     ----                 ----               -------
      Normal   Scheduled  3m20s                default-scheduler  Successfully assigned default/my-app-5b764c67f8-hd6kn to minikube
      Normal   Pulling    96s (x4 over 3m17s)  kubelet            Pulling image "my-app:1.0"
      Warning  Failed     93s (x4 over 3m6s)   kubelet            Failed to pull image "my-app:1.0": Error response from daemon: pull access denied for my-app, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
      Warning  Failed     93s (x4 over 3m6s)   kubelet            Error: ErrImagePull
      Warning  Failed     69s (x6 over 3m6s)   kubelet            Error: ImagePullBackOff
      Normal   BackOff    55s (x7 over 3m6s)   kubelet            Back-off pulling image "my-app:1.0"
    [root@sid-vm deployments]#
    ```
    
3. **Look for image pull errors** The output will show events related to image pulling, such as:
    
    ```yaml
    Failed to pull image "my-app:1.0": rpc error: code = Unknown desc = Error response from daemon: pull access denied for my-app, repository does not exist or may require 'docker login'
    ```
    
4. **Fix the image reference in the deployment manifest** and redeploy:
    

Modify your `deployment.yaml` file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app-container
        image: nginx
        ports:
        - containerPort: 80
```

Then apply the Deployment:

```sh
[root@sid-vm deployments]# kubectl apply -f error_deploy.yml
deployment.apps/my-app configured
[root@sid-vm deployments]#
```

Now, let's see the status of our deployment and status of the pods.

```bash
[root@sid-vm deployments]# kubectl get deploy
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
my-app   3/3     3            3           5m29s
[root@sid-vm deployments]#
[root@sid-vm deployments]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
my-app-97d8bc7bf-b8tkf   1/1     Running   0          54s
my-app-97d8bc7bf-jhbzt   1/1     Running   0          51s
my-app-97d8bc7bf-nhtc9   1/1     Running   0          57s
```

And finally after correcting the image of the container in our manifest file, our deployment came up successfully.

## Conclusion

Kubernetes Deployments provide a powerful and flexible way to manage stateless applications. They simplify the process of updating, scaling, and maintaining applications, ensuring high availability and resilience. By mastering Deployments, you can ensure your applications run smoothly and handle changes gracefully in a Kubernetes environment. Whether you’re rolling out new features, scaling to meet demand, or recovering from errors, Deployments are an essential tool in your Kubernetes toolkit.