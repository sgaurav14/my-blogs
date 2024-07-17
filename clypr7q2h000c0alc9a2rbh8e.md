---
title: "Part 1: Understanding Kubernetes Volumes - EmptyDir and HostPath"
datePublished: Wed Jul 17 2024 11:23:29 GMT+0000 (Coordinated Universal Time)
cuid: clypr7q2h000c0alc9a2rbh8e
slug: part-1-understanding-kubernetes-volumes-emptydir-and-hostpath
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721206296842/b1ff4cfc-2658-4485-a0f5-4691e43659e5.png
tags: kubernetes, k8s, kubernetes-container, kubernetes-architecture, kubernetes-volume, k8scluster, emptydir, hostpath

---

## Introduction

Kubernetes, an open-source container orchestration platform, helps in deploying, scaling, and managing containerized applications efficiently. One of the essential features in Kubernetes is its volume management system, which allows data to persist beyond the lifecycle of a single container. In this blog, we’ll delve into what volumes are, explore the various types available in Kubernetes, and provide detailed examples to aid in understanding.

## What are Kubernetes Volumes?

In Kubernetes, a volume is a directory accessible to the containers in a pod. Unlike ephemeral storage, which is tied to the container's lifecycle, volumes outlive individual containers and allow for data persistence and sharing across containers within the same pod. Volumes in Kubernetes address the issues of data persistence and data sharing among containers.

## Types of Kubernetes Volumes

Kubernetes supports a wide variety of volume types, each catering to different needs and use cases. Let's explore some of the most common ones in detail.

### 1\. EmptyDir

An `EmptyDir` volume is created when a pod is assigned to a node and exists as long as that pod runs on the node. This volume starts empty and is typically used for scratch space, caches, or logs. It is deleted when the pod is removed from the node for any reason.

We can use this when we want to share contents between multiple containers on the same pod and not to the local machine.

When a pod is removed from a node for any reasons, the data in that emptydir is deleted forever.

#### Key Points:

* **Lifecycle**: Exists as long as the pod is running.
    
* **Use Case**: Temporary storage, caching, logs.
    
* **Persistence**: Non-persistent; data is lost when the pod is deleted.
    

#### Example

```bash
[root@k8s-master vols]# cat emptydir.yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
spec:
  containers:
    - name: c00
      image: centos
      command: ["/bin/bash", "-c", "sleep 1000"]
      volumeMounts:
        - name: appvol
          mountPath: "/tmp/myapp"


    - name: c01
      image: ubuntu
      command: ["/bin/bash", "-c", "sleep 1000"]
      volumeMounts:
        - name: appvol
          mountPath: "/tmp/program"

  volumes:
    - name: appvol
      emptyDir: {}
[root@k8s-master vols]#
```

In this example, the `EmptyDir` volume named `appvol` is mounted at `/tmp/myapp` and `/tmp/program` inside both containers within the pod.

Let's deploy this manifest file.

```bash
[root@k8s-master vols]# kubectl apply -f emptydir.yml
pod/myapp created
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
myapp   2/2     Running   0          28s
[root@k8s-master vols]#
```

Let's login in to these pod and check the volume that we created is present or not.

First, login to the first pod.

```bash
[root@k8s-master vols]# kubectl exec -it myapp -c c00 -- /bin/bash
[root@myapp /]# cd /tmp/myapp/
[root@myapp myapp]#
[root@myapp myapp]# pwd
/tmp/myapp
[root@myapp myapp]#
[root@myapp myapp]# ls
[root@myapp myapp]#
[root@myapp myapp]# touch container1-file
[root@myapp myapp]# ls
container1-file
[root@myapp myapp]#
```

We can see that the requested mounted directory for the first container `/tmp/myapp` is present. Also we created one file inside this directory to validate if this file is visible on the second container volume as the emptyDir volumes are shared among all the containers configured on a single pod.

Let's check for second container as well.

```bash
[root@k8s-master vols]# kubectl exec -it myapp -c c01 -- /bin/bash
root@myapp:/#
root@myapp:/# cd /tmp/program/
root@myapp:/tmp/program#
root@myapp:/tmp/program# pwd
/tmp/program
root@myapp:/tmp/program#
root@myapp:/tmp/program# ll
total 0
drwxrwxrwx. 2 root root 29 Jul 16 17:11 ./
drwxrwxrwt. 1 root root 21 Jul 16 17:10 ../
-rw-r--r--. 1 root root  0 Jul 16 17:11 container1-file
root@myapp:/tmp/program#
root@myapp:/tmp/program# touch container2-file
root@myapp:/tmp/program# ll
total 0
drwxrwxrwx. 2 root root 52 Jul 16 17:12 ./
drwxrwxrwt. 1 root root 21 Jul 16 17:10 ../
-rw-r--r--. 1 root root  0 Jul 16 17:11 container1-file
-rw-r--r--. 1 root root  0 Jul 16 17:12 container2-file
root@myapp:/tmp/program#
```

In this container as well, we can see the requested mounted directory `/tmp/program` is present. Also we validated that the 1st file which we created on container 1 is also visible here. We also created one more file from this container.

Now let's delete the container and redeploy it and see if the created files are present.

```bash
[root@k8s-master vols]# kubectl delete -f emptydir.yml
pod "myapp" deleted
[root@k8s-master vols]# kubectl get pods
No resources found in default namespace.
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl apply -f emptydir.yml
pod/myapp created
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
myapp   2/2     Running   0          26s
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl exec -it myapp -c c00 -- /bin/bash
[root@myapp /]# cd /tmp/myapp/
[root@myapp myapp]#
[root@myapp myapp]# ls
[root@myapp myapp]#
[root@myapp myapp]# exit
[root@k8s-master vols]# kubectl exec -it myapp -c c01 -- /bin/bash
root@myapp:/#
root@myapp:/# cd /tmp/program/
root@myapp:/tmp/program#
root@myapp:/tmp/program# ls
root@myapp:/tmp/program#
```

We deleted the existing pod and redeployed it. However, we didn't see our previously created files under any of the containers inside the pod. This proves that whenever the pod is deleted, the data present on the EmptyDir volume is wiped out.

### 2\. HostPath

A `HostPath` volume mounts a file or directory from the host node’s filesystem into the pod. This type of volume can be useful for testing or in single-node environments but should be used with caution in production due to potential security risks.

#### Key Points:

* **Lifecycle**: Exists as long as the pod is running.
    
* **Use Case**: Accessing host files, single-node environments.
    
* **Persistence**: Depends on the host filesystem; data is persistent as long as the host node maintains it.
    

#### Example

```bash
[root@k8s-master vols]# cat hostpath.yml
apiVersion: v1
kind: Pod
metadata:
  name: testpod
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: testvol
          mountPath: "/tmp/hostfiles"
  volumes:
    - name: testvol
      hostPath:
        path: /tmp/data
        type: DirectoryOrCreate
[root@k8s-master vols]#
```

Let's deploy the above manifest file.

```bash
[root@k8s-master vols]# kubectl apply -f hostpath.yml
pod/testpod created
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl get pods
NAME      READY   STATUS    RESTARTS   AGE
testpod   1/1     Running   0          14s
[root@k8s-master vols]#
[root@k8s-master vols]# kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE   IP               NODE                              NOMINATED NODE   READINESS GATES
testpod   1/1     Running   0          18s   192.168.224.12   k8s-worker1.ha.stalab.ciena.com   <none>           <none>
[root@k8s-master vols]#
[root@k8s-master vols]#
```

Now we need to validate that the directory path of the host configured in the manifest file has been created on the worker nodes. First, we need to check on which node the pod has been created and then log in to that worker node to validate the required directory.

```bash
[root@k8s-master vols]# kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE     IP               NODE                              NOMINATED NODE   READINESS GATES
testpod   1/1     Running   0          2m42s   192.168.224.12   k8s-worker1.ha.stalab.ciena.com   <none>           <none>
[root@k8s-master vols]#
[root@k8s-master vols]# ssh k8s-worker1
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Wed Jul 17 04:32:37 2024 from 10.121.230.193

[root@k8s-worker1 ~]#
[root@k8s-worker1 ~]# ls -ld /tmp/data
drwxr-xr-x. 2 root root 6 Jul 17 04:31 /tmp/data
[root@k8s-worker1 ~]#
```

Hurray, Kubernetes automatically created the required directory on the nodes and mounted it with our pod. Let's do one more test: we'll create a file in the volume inside the path and validate the file's presence on the node location.

```bash
[root@k8s-master vols]# kubectl exec -it testpod -- /bin/bash
root@testpod:/#
root@testpod:/# cd /tmp/hostfiles/
root@testpod:/tmp/hostfiles# ls
root@testpod:/tmp/hostfiles#
root@testpod:/tmp/hostfiles# touch containerfile
root@testpod:/tmp/hostfiles# ls
containerfile
root@testpod:/tmp/hostfiles#
root@testpod:/tmp/hostfiles#
exit
[root@k8s-master vols]#
[root@k8s-master vols]# ssh k8s-worker1
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Wed Jul 17 04:34:36 2024 from 10.121.230.193
[root@k8s-worker1 ~]#
[root@k8s-worker1 ~]# cd /tmp/data
[root@k8s-worker1 data]#
[root@k8s-worker1 data]# ll
total 0
-rw-r--r--. 1 root root 0 Jul 17 04:36 containerfile
[root@k8s-worker1 data]#
```

Hurray! we can see the files that we created on the container is visible on the worker node path.

### Conclusion

In this blog, we explored two types of Kubernetes volumes: EmptyDir and HostPath. These volumes provide temporary and persistent storage solutions for different use cases. While EmptyDir volumes are suitable for temporary storage within a pod, HostPath volumes allow access to the host node's filesystem. In the next blog, we will cover Persistent Volumes (PV), Persistent Volume Claims (PVC), and Amazon Elastic Block Store (EBS) volumes to provide a more comprehensive understanding of Kubernetes storage solutions.