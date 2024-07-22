---
title: "Deep Dive into Kubernetes Volumes Part 2: Persistent Volumes and Claims with NFS and AWS EBS"
datePublished: Mon Jul 22 2024 07:13:51 GMT+0000 (Coordinated Universal Time)
cuid: clywnhy2k000b0al9b5i161jw
slug: deep-dive-into-kubernetes-volumes-part-2-persistent-volumes-and-claims-with-nfs-and-aws-ebs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721632361041/828b45ff-9c1b-4941-9f60-262701096462.jpeg
tags: kubernetes, devops, k8s, kubernetes-container, kubernetes-persistent-volumes, k8scluster, persistent-volume-claim, persistentvolumes

---

### Introduction

In the first part of our Kubernetes volume series, we explored EmptyDir and HostPath volumes. These volume types are useful for temporary storage and accessing host files, respectively. In this second part, we’ll delve into Persistent Volumes (PV) and Persistent Volume Claims (PVC), focusing on using Network File System (NFS) and Amazon Elastic Block Store (EBS) as storage solutions.

### What are Persistent Volumes (PV) and Persistent Volume Claims (PVC)?

In Kubernetes, the Persistent Volume (PV) and Persistent Volume Claim (PVC) mechanism decouples storage provisioning from pod lifecycle management. This separation allows for better management of storage resources and provides a consistent way to manage persistent data.

* **Persistent Volume (PV):** A PV is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using storage classes. It is a resource in the cluster, similar to a node, and is independent of any individual pod.
    
* **Persistent Volume Claim (PVC):** A PVC is a request for storage by a user. It is similar to a pod in that pods consume node resources and PVCs consume PV resources. PVCs are used to dynamically or statically bind to PVs and can specify the storage capacity and access modes required.
    

### Using NFS for Persistent Storage

Network File System (NFS) is a distributed file system protocol that allows a user on a client computer to access files over a network in a manner similar to how local storage is accessed.

#### Step-by-Step Guide to Using NFS in Kubernetes

1. **Set Up an NFS Server:**
    
    Ensure you have an NFS server running and accessible by your Kubernetes cluster. Install and configure NFS on a server (this can be on-premises or a cloud VM).
    
    You can get the NFS details with your storage admin if you have specific storage team.
    
    We need NFS server IP/Hostname and the path where we want to use k8s storage.
    
2. **Create a Persistent Volume (PV):**
    
    Create a YAML file to define the NFS PV.
    
    ```bash
    [root@k8s-master ~]# cat pv.yml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
            name: k8s-vol
    spec:
            capacity:
                    storage: 2Gi
            accessModes:
                    - ReadWriteMany
            persistentVolumeReclaimPolicy: Recycle
            nfs:
                    path: /vol/my-vol
                    server: nfs-server
    
    [root@k8s-master ~]#
    ```
    
    Apply this configuration to your Kubernetes cluster:
    
    ```bash
    [root@k8s-master ~]# kubectl apply -f pv.yml
    persistentvolume/k8s-vol created
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get pv
    NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
    k8s-vol   2Gi        RWX            Recycle          Available                                   14s
    [root@k8s-master ~]#
    [root@k8s-master ~]#
    ```
    
    Let's see the description of the above pv created.
    
    ```bash
    [root@k8s-master ~]# kubectl describe pv k8s-vol
    Name:            k8s-vol
    Labels:          <none>
    Annotations:     <none>
    Finalizers:      [kubernetes.io/pv-protection]
    StorageClass:
    Status:          Available
    Claim:
    Reclaim Policy:  Recycle
    Access Modes:    RWX
    VolumeMode:      Filesystem
    Capacity:        2Gi
    Node Affinity:   <none>
    Message:
    Source:
        Type:      NFS (an NFS mount that lasts the lifetime of a pod)
        Server:    nfs-server
        Path:      /vol/my-vol
        ReadOnly:  false
    Events:        <none>
    [root@k8s-master ~]#
    ```
    
    Now we created the persistent volume, but to use this volume, now we need to create persistent volume claim.
    
3. **Create a Persistent Volume Claim (PVC):**
    
    Create a YAML file to define the PVC.
    
    ```bash
    [root@k8s-master ~]# cat pvclaim.yml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
            name: app1-vol1
    spec:
            accessModes:
                    - ReadWriteMany
            resources:
                    requests:
                            storage: 1Gi
    
    [root@k8s-master ~]#
    ```
    
    In this yaml file, we are claiming 1Gi of storage from the existing PVs. As of now we have only one PV created, so this requirement can be fulfilled by volume `"k8s-vol".`
    
    Apply this configuration:
    
    ```bash
    [root@k8s-master ~]# kubectl apply -f pvclaim.yml
    persistentvolumeclaim/app1-vol1 created
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get pvc
    NAME        STATUS   VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    app1-vol1   Bound    k8s-vol   2Gi        RWX                           10s
    [root@k8s-master ~]#
    ```
    
    We can see from the above output is that this PVC is bounded from volume k8s-vol.
    
4. **Use the PVC in a Pod:**
    
    Create a YAML file to define a pod that uses the PVC.
    
    ```bash
    [root@k8s-master ~]# cat deploy.yml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-app-deploy
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: python-app
      template:
        metadata:
          labels:
            app: python-app
        spec:
          containers:
            - name: c00
              image: centos
              command: ["/bin/bash", "-c", "sleep 1000000"]
              volumeMounts:
                - name: python-vol
                  mountPath: "/tmp/pvdata"
          volumes:
            - name: python-vol
              persistentVolumeClaim:
                claimName: app1-vol1
    
    
    
    [root@k8s-master ~]#
    ```
    
    In the above deployment file, we are using our newly created pvc to use as a volume.
    
    Apply this configuration:
    
    ```bash
    [root@k8s-master ~]# kubectl apply -f deploy.yml
    deployment.apps/my-app-deploy created
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get deploy
    NAME            READY   UP-TO-DATE   AVAILABLE   AGE
    my-app-deploy   2/2     2            2           15s
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get pods
    NAME                             READY   STATUS    RESTARTS        AGE
    my-app-deploy-5db4bdf479-88jj5   1/1     Running   0               20s
    my-app-deploy-5db4bdf479-hm495   1/1     Running   0               20s
    [root@k8s-master ~]#
    ```
    
    Let's do some operations on one of the pods, create some files inside the volume and then delete the pod. The new pod should come with the older data as we are using persistent volume here.
    
    ```bash
    [root@k8s-master ~]# kubectl exec -it my-app-deploy-5db4bdf479-88jj5 -- /bin/bash
    [root@my-app-deploy-5db4bdf479-88jj5 /]#
    [root@my-app-deploy-5db4bdf479-88jj5 /]# cd /tmp/pvdata/
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# ls
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]#
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# touch siddhartha
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# ls
    siddhartha
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]#
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# touch gaurav
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]#
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# ls
    gaurav  siddhartha
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]#
    [root@my-app-deploy-5db4bdf479-88jj5 pvdata]# exit
    command terminated with exit code 127
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get pods
    NAME                             READY   STATUS    RESTARTS        AGE
    my-app-deploy-5db4bdf479-88jj5   1/1     Running   0               3m4s
    my-app-deploy-5db4bdf479-hm495   1/1     Running   0               3m4s
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl delete pod my-app-deploy-5db4bdf479-88jj5
    pod "my-app-deploy-5db4bdf479-88jj5" deleted
    
    [root@k8s-master ~]#
    [root@k8s-master ~]# kubectl get pods
    NAME                             READY   STATUS    RESTARTS        AGE
    my-app-deploy-5db4bdf479-fcfvp   1/1     Running   0               34s
    my-app-deploy-5db4bdf479-hm495   1/1     Running   0               3m58s
    [root@k8s-master ~]#
    ```
    
    Now, let's login to the new pod and see if our previous created files are present or not.
    
    ```bash
    [root@k8s-master ~]# kubectl exec -it my-app-deploy-5db4bdf479-fcfvp -- /bin/bash
    [root@my-app-deploy-5db4bdf479-fcfvp /]#
    [root@my-app-deploy-5db4bdf479-fcfvp /]# cd /tmp/pvdata/
    [root@my-app-deploy-5db4bdf479-fcfvp pvdata]#
    [root@my-app-deploy-5db4bdf479-fcfvp pvdata]# ls
    gaurav  siddhartha
    [root@my-app-deploy-5db4bdf479-fcfvp pvdata]#
    [root@my-app-deploy-5db4bdf479-fcfvp pvdata]#
    ```
    
    Hurray! our files are present even after the pod deletion. So by using this we can make our data more persistent and this make our environment more stable.
    

### Using AWS EBS for Persistent Storage

Amazon Elastic Block Store (EBS) provides block-level storage volumes for use with Amazon EC2 instances. These volumes are used in Kubernetes for persistent storage.

#### Step-by-Step Guide to Using AWS EBS in Kubernetes

1. **Create an EBS Volume:**
    
    In the AWS Management Console, create an EBS volume in the same availability zone as your Kubernetes cluster.
    
2. **Create a Persistent Volume (PV):**
    
    Create a YAML file to define the EBS PV.
    
    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: ebs-pv
    spec:
      capacity:
        storage: 10Gi
      accessModes:
        - ReadWriteOnce
      awsElasticBlockStore:
        volumeID: <YOUR_EBS_VOLUME_ID>
        fsType: ext4
    ```
    
    Apply this configuration to your Kubernetes cluster:
    
    ```bash
    kubectl apply -f ebs-pv.yaml
    ```
    
3. **Create a Persistent Volume Claim (PVC):**
    
    Create a YAML file to define the PVC.
    
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: ebs-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 10Gi
    ```
    
    Apply this configuration:
    
    ```bash
    kubectl apply -f ebs-pvc.yaml
    ```
    
4. **Use the PVC in a Pod:**
    
    Create a YAML file to define a pod that uses the PVC.
    
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: ebs-test-pod
    spec:
      containers:
        - name: busybox
          image: busybox
          command: ["/bin/sh", "-c", "sleep 1000"]
          volumeMounts:
            - mountPath: "/mnt/ebs"
              name: ebs-volume
      volumes:
        - name: ebs-volume
          persistentVolumeClaim:
            claimName: ebs-pvc
    ```
    
    Apply this configuration:
    
    ```bash
    kubectl apply -f ebs-test-pod.yaml
    ```
    

### Conclusion

In this second part of our Kubernetes volume series, we explored how to use Persistent Volumes (PV) and Persistent Volume Claims (PVC) with NFS and AWS EBS. These storage solutions provide robust and scalable options for managing persistent data in Kubernetes. By understanding and implementing these concepts, you can ensure that your applications have reliable and consistent access to storage resources.

Stay tuned for more in-depth guides and tutorials on Kubernetes and other cloud-native technologies. Happy learning! 🌐💡