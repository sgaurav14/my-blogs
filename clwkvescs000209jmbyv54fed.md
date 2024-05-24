---
title: "A Beginner's Guide to Setting Up a Minikube Cluster on CentOS 7"
datePublished: Fri May 24 2024 16:02:42 GMT+0000 (Coordinated Universal Time)
cuid: clwkvescs000209jmbyv54fed
slug: a-beginners-guide-to-setting-up-a-minikube-cluster-on-centos-7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716539051016/b245a09d-e120-49b5-80c2-9b240a73b7bf.jpeg
tags: kubernetes, devops, devops-articles, minikube, kubernetes-container, kubernetes-architecture, minikube-setup, kubeweekchallenge

---

Welcome back to our Kubernetes learning series! In our previous installments, we delved into understanding Kubernetes (K8s) as a container orchestration platform and explored the concept of pods - the smallest deployable units in Kubernetes. Now, we're taking the next step in our journey by setting up a Minikube cluster on CentOS 7 machines.

Containerization has revolutionized the way applications are deployed, managed, and scaled. Kubernetes, an open-source container orchestration platform, has become the de facto standard for managing containerized applications. Minikube, on the other hand, provides a lightweight Kubernetes implementation that runs on a single machine, making it ideal for development and testing purposes.

In this guide, we'll walk through the process of setting up a Minikube cluster on CentOS 7 machines. By the end of this tutorial, you'll have a fully functional Kubernetes cluster running on your CentOS 7 environment, continuing our journey into the world of Kubernetes.

### **Prerequisites**

Before we begin, make sure you have the following prerequisites:

1. CentOS 7 machines with at least 2GB of RAM and 2 CPUs.
    
2. Root access to the CentOS machines.
    
3. A stable internet connection.
    

Let's dive in!

### **Step 1: Install Docker**

Minikube requires a container runtime to manage containers. Docker is one of the most popular container runtimes available. To install Docker on CentOS 7, follow these steps:

`sudo yum install -y yum-utils device-mapper-persistent-data lvm2`

`sudo yum-config-manager --add-repo` [`https://download.docker.com/linux/centos/docker-ce.repo`](https://download.docker.com/linux/centos/docker-ce.repo)

`sudo yum install docker-ce`

`sudo systemctl start docker`

`sudo systemctl enable docker`

```basic
[root@sid-vm ~]# sudo yum install -y yum-utils device-mapper-persistent-data lvm2
Last metadata expiration check: 0:10:30 ago on Fri 24 May 2024 03:43:40 AM EDT.
Package yum-utils-4.0.21-23.0.1.el8.noarch is already installed.
Package device-mapper-persistent-data-0.9.0-7.el8.x86_64 is already installed.
Package lvm2-8:2.03.14-13.0.3.el8_9.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@sid-vm ~]# sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
Adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
[root@sid-vm ~]# sudo yum install docker-ce
Docker CE Stable - x86_64                                                                                                                                                                            8.6 kB/s | 3.5 kB     00:00
Package docker-ce-3:26.0.0-1.el8.x86_64 is already installed.
Dependencies resolved.
=====================================================================================================================================================================================================================================
 Package                                              Architecture                                      Version                                                    Repository                                                   Size
=====================================================================================================================================================================================================================================
Upgrading:
 docker-ce                                            x86_64                                            3:26.1.3-1.el8                                             docker-ce-stable                                             27 M

Transaction Summary
=====================================================================================================================================================================================================================================
Upgrade  1 Package

Total download size: 27 M
Is this ok [y/N]: y
Downloading Packages:
docker-ce-26.1.3-1.el8.x86_64.rpm                                                                                                                                                                    5.4 MB/s |  27 MB     00:05
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                5.4 MB/s |  27 MB     00:05
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                             1/1
  Running scriptlet: docker-ce-3:26.1.3-1.el8.x86_64                                                                                                                                                                             1/1
  Upgrading        : docker-ce-3:26.1.3-1.el8.x86_64                                                                                                                                                                             1/2
  Running scriptlet: docker-ce-3:26.1.3-1.el8.x86_64                                                                                                                                                                             1/2
  Running scriptlet: docker-ce-3:26.0.0-1.el8.x86_64                                                                                                                                                                             2/2
  Cleanup          : docker-ce-3:26.0.0-1.el8.x86_64                                                                                                                                                                             2/2
  Running scriptlet: docker-ce-3:26.0.0-1.el8.x86_64                                                                                                                                                                             2/2
  Verifying        : docker-ce-3:26.1.3-1.el8.x86_64                                                                                                                                                                             1/2
  Verifying        : docker-ce-3:26.0.0-1.el8.x86_64                                                                                                                                                                             2/2

Upgraded:
  docker-ce-3:26.1.3-1.el8.x86_64

Complete!
[root@sid-vm ~]#
[root@sid-vm ~]# sudo systemctl start docker
[root@sid-vm ~]# sudo systemctl enable docker
[root@sid-vm ~]#
```

### **Step 2: Install kubectl**

`kubectl` is a command-line tool used to interact with Kubernetes clusters. You can install `kubectl` using `curl`:

`sudo curl -LO` [`https://storage.googleapis.com/kubernetes-release/release/$(curl`](https://storage.googleapis.com/kubernetes-release/release/$(curl) `-s` [`https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl`](https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl)

`sudo chmod +x ./kubectl`

`sudo mv ./kubectl /usr/local/bin/kubectl`

```basic
[root@sid-vm ~]# sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 49.0M  100 49.0M    0     0  5657k      0  0:00:08  0:00:08 --:--:-- 7648k
[root@sid-vm ~]# sudo chmod +x ./kubectl
[root@sid-vm ~]# sudo mv ./kubectl /usr/local/bin/kubectl
[root@sid-vm ~]#
```

### **Step 3: Install Minikube**

Next, install Minikube using `curl`:

`curl -Lo minikube` [`https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)`   && chmod +x minikube`

`sudo mv minikube /usr/local/bin/`

```basic
[root@sid-vm ~]# curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
>   && chmod +x minikube
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 91.1M  100 91.1M    0     0  7333k      0  0:00:12  0:00:12 --:--:-- 9030k
[root@sid-vm ~]#
[root@sid-vm ~]#
[root@sid-vm ~]# sudo mv minikube /usr/local/bin/
[root@sid-vm ~]#
[root@sid-vm ~]#
```

### **Step 4: Start Minikube Cluster**

Now that you have Docker, kubectl, and Minikube installed, you can start the Minikube cluster:

`minikube start --driver=docker`

```basic
[root@sid-vm ~]# minikube start --driver=docker
* minikube v1.33.1 on Oracle 8.9 (kvm/amd64)
* Using the docker driver based on user configuration
* The "docker" driver should not be used with root privileges. If you wish to continue as root, use --force.
* If you are running minikube within a VM, consider using --driver=none:
*   https://minikube.sigs.k8s.io/docs/reference/drivers/none/

X Exiting due to DRV_AS_ROOT: The "docker" driver should not be used with root privileges.


[root@sid-vm ~]# minikube start --driver=docker --force
* minikube v1.33.1 on Oracle 8.9 (kvm/amd64)
! minikube skips various validations when --force is supplied; this may lead to unexpected behavior
* Using the docker driver based on user configuration
* The "docker" driver should not be used with root privileges. If you wish to continue as root, use --force.
* If you are running minikube within a VM, consider using --driver=none:
*   https://minikube.sigs.k8s.io/docs/reference/drivers/none/
* Using Docker driver with root privileges
* Starting "minikube" primary control-plane node in "minikube" cluster
* Pulling base image v0.0.44 ...
* Downloading Kubernetes v1.30.0 preload ...
    > gcr.io/k8s-minikube/kicbase...:  481.58 MiB / 481.58 MiB  100.00% 9.12 Mi
    > preloaded-images-k8s-v18-v1...:  342.90 MiB / 342.90 MiB  100.00% 6.35 Mi
* Creating docker container (CPUs=2, Memory=2200MB) ...
* Preparing Kubernetes v1.30.0 on Docker 26.1.1 ...
  - Generating certificates and keys ...
  - Booting up control plane ...
  - Configuring RBAC rules ...
* Configuring bridge CNI (Container Networking Interface) ...
* Verifying Kubernetes components...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: storage-provisioner, default-storageclass
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
[root@sid-vm ~]#
```

This command will download the Minikube ISO, start a virtual machine, and deploy Kubernetes components on it.

### **Step 5: Verify Minikube Installation**

To verify that Minikube is running correctly, use the following command:

`kubectl cluster-info`

```basic
[root@sid-vm ~]# kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[root@sid-vm ~]#
[root@sid-vm ~]# kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   58s   v1.30.0
[root@sid-vm ~]#
```

You should see output similar to the following:

```basic
Kubernetes master is running at https://<ip>:<port>
```

### **Step 6: Deploy Your First Application**

Now that your Minikube cluster is up and running, let's deploy a sample application. Create a simple pod using the following YAML definition:

```basic
[root@sid-vm pods]# cat first_pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: first-pod
  labels:
    app: hello-minikube
spec:
  containers:
  - name: c00
    image: ubuntu
    command: ["/bin/bash", "-c", "while true ; do echo Hello Techies ; sleep 5; done"]
[root@sid-vm pods]#
```

Save the above YAML to a file (e.g., `hello-minikube-pod.yaml`) and apply it to your Minikube cluster using the following command:

```basic
[root@sid-vm pods]# kubectl apply -f first_pod.yml
pod/first-pod created
[root@sid-vm pods]#
```

This YAML file describes a pod named `first-po` with a single container named `c00`. The container runs the `ubuntu` image, where we are running a echo command.

Once applied, Kubernetes will create a pod running the specified container, and you can verify its status using `kubectl get pods`.

```basic
[root@sid-vm pods]# kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
first-pod   1/1     Running   0          26s
[root@sid-vm pods]#
```

If you want to detailed output of pod, like what IP has been assigned to the pod, run `kubectl get pods -o wide`.

```basic
[root@sid-vm pods]# kubectl get pods -o wide
NAME        READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
first-pod   1/1     Running   0          70s   10.244.0.3   minikube   <none>           <none>
[root@sid-vm pods]#
```

If you want to ping the Pod IP from your currrent host prompt, it will not able to reach the Pod IP. If you want to ping it, you need to login to minikube cluster first by `minikube ssh` command.

```basic
[root@sid-vm ~]# ping 10.244.0.3
PING 10.244.0.3 (10.244.0.3) 56(84) bytes of data.
From 10.176.253.163 icmp_seq=1 Time to live exceeded
From 10.176.253.163 icmp_seq=2 Time to live exceeded
From 10.176.253.163 icmp_seq=3 Time to live exceeded
^C
--- 10.244.0.3 ping statistics ---
3 packets transmitted, 0 received, +3 errors, 100% packet loss, time 2003ms

[root@sid-vm ~]#
[root@sid-vm ~]# minikube ssh
docker@minikube:~$
docker@minikube:~$ ping 10.244.0.3
PING 10.244.0.3 (10.244.0.3) 56(84) bytes of data.
64 bytes from 10.244.0.3: icmp_seq=1 ttl=64 time=0.378 ms
64 bytes from 10.244.0.3: icmp_seq=2 ttl=64 time=0.159 ms
^C
--- 10.244.0.3 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1053ms
rtt min/avg/max/mdev = 0.159/0.268/0.378/0.109 ms
docker@minikube:~$
```

### **Conclusion**

Congratulations! You've successfully set up a Minikube cluster on CentOS 7 machines and deployed your first application. This is just the beginning of your journey with Kubernetes. You can now explore more advanced features and deploy complex applications on your Minikube cluster.

Happy Kuberneting!