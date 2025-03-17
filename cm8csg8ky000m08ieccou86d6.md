---
title: "Deploying a Node.js To-Do App with Helm on Kubernetes"
datePublished: Mon Mar 17 2025 08:14:09 GMT+0000 (Coordinated Universal Time)
cuid: cm8csg8ky000m08ieccou86d6
slug: deploying-a-nodejs-to-do-app-with-helm-on-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1742199100923/f30115f3-c6c6-43e7-9e83-3d24255e9d1c.avif
tags: nodejs, kubernetes, devops, helm, helm-chart, kubernetes-nodeport

---

## **Introduction**

In this guide, we will deploy a **Node.js To-Do App** on a Kubernetes cluster using **Helm**. The application runs on **port 8000** and uses the Docker image `sgaurav7/node-app`. We will configure **health checks**, a **NodePort service**, and ensure a seamless deployment.

---

## **1️⃣ Prerequisites**

Before we begin, make sure you have:  
✔ A running Kubernetes cluster  
✔ Helm installed (`helm version`)  
✔ `kubectl` access to your cluster

---

## **2️⃣ Create a Helm Chart**

Run the following command to create a new Helm chart:

```bash
[root@k8s-master projects]# helm create todo-app
Creating todo-app
[root@k8s-master projects]# ls
mysql  todo-app
[root@k8s-master projects]# cd todo-app/
[root@k8s-master todo-app]# ls
charts  Chart.yaml  templates  values.yaml
[root@k8s-master todo-app]#
```

---

## **3️⃣ Define** `values.yaml`

Update `values.yaml` with the **Docker image**, **replicas**, and **NodePort service**:

```yaml
replicaCount: 2

image:
  repository: sgaurav7/node-app
  pullPolicy: IfNotPresent
  tag: latest

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  create: true
  automount: true
  annotations: {}
  name: ""

podAnnotations: {}
podLabels: {}

podSecurityContext: {}

securityContext: {}

service:
  type: NodePort
  port: 8000
  nodePort: 30007

ingress:
  enabled: false
  className: ""
  annotations: {}
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []

resources: {}

livenessProbe:
  httpGet:
    path: /
    port: 8000
readinessProbe:
  httpGet:
    path: /
    port: 8000

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80

volumes: []

volumeMounts: []

nodeSelector: {}

tolerations: []

affinity: {}
```

---

## **4️⃣ Configure Deployment (**`templates/deployment.yaml`)

Modify `deployment.yaml` to use the correct **image**, **port**, and **health probes**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "todo-app.fullname" . }}
  labels:
    {{- include "todo-app.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  selector:
    matchLabels:
      {{- include "todo-app.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      {{- with .Values.podAnnotations }}
      annotations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      labels:
        {{- include "todo-app.labels" . | nindent 8 }}
        {{- with .Values.podLabels }}
        {{- toYaml . | nindent 8 }}
        {{- end }}
    spec:
      {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      serviceAccountName: {{ include "todo-app.serviceAccountName" . }}
      {{- with .Values.podSecurityContext }}
      securityContext:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      containers:
        - name: {{ .Chart.Name }}
          {{- with .Values.securityContext }}
          securityContext:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: todo-port
              containerPort: {{ .Values.service.port }}
              protocol: TCP
          {{- with .Values.livenessProbe }}
          livenessProbe:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.readinessProbe }}
          readinessProbe:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.resources }}
          resources:
            {{- toYaml . | nindent 12 }}
          {{- end }}
          {{- with .Values.volumeMounts }}
          volumeMounts:
            {{- toYaml . | nindent 12 }}
          {{- end }}
      {{- with .Values.volumes }}
      volumes:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
```

---

## **5️⃣ Configure Service (**`templates/service.yaml`)

Ensure the **NodePort service** exposes port **8000** correctly:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ include "todo-app.fullname" . }}
  labels:
    {{- include "todo-app.labels" . | nindent 4 }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 8000
      nodePort: {{ .Values.service.nodePort }}
      protocol: TCP
      name: node-port
  selector:
    {{- include "todo-app.selectorLabels" . | nindent 4 }}
```

---

## **6️⃣ Deploy the To-Do App Using Helm**

### **First-Time Installation**

```bash
[root@k8s-master todo-app]# helm install v1 . --namespace to-do --create-namespace
NAME: v1
LAST DEPLOYED: Mon Mar 17 04:03:07 2025
NAMESPACE: to-do
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace to-do -o jsonpath="{.spec.ports[0].nodePort}" services v1-todo-app)
  export NODE_IP=$(kubectl get nodes --namespace to-do -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
[root@k8s-master todo-app]#
```

### **Upgrade the Application**

If you make any changes:

```bash
helm upgrade todo-app . -n to-do
```

---

## **7️⃣ Verify the Deployment**

### **Check Helm Release**

```bash
[root@k8s-master todo-app]# helm list -n to-do
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
v1      to-do           1               2025-03-17 04:03:07.477506603 -0400 EDT deployed        todo-app-0.1.0  1.16.0    
[root@k8s-master todo-app]#
```

### **Check Running Pods**

```bash
[root@k8s-master todo-app]# kubectl get pods -n to-do
NAME                          READY   STATUS    RESTARTS   AGE
v1-todo-app-5c87b864c-b7c7l   1/1     Running   0          55s
v1-todo-app-5c87b864c-gk7ph   1/1     Running   0          55s
[root@k8s-master todo-app]#
```

### **Check Service**

```bash
[root@k8s-master todo-app]# kubectl get svc -n to-do
NAME          TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
v1-todo-app   NodePort   10.108.87.163   <none>        8000:30007/TCP   72s
[root@k8s-master todo-app]#
```

---

## **8️⃣ Access the To-Do App**

Now your application is accessible via:

```bash
export NODE_PORT=$(kubectl get --namespace to-do -o jsonpath="{.spec.ports[0].nodePort}" services v1-todo-app)
export NODE_IP=$(kubectl get nodes --namespace to-do -o jsonpath="{.items[0].status.addresses[0].address}")
echo http://$NODE_IP:$NODE_PORT
```

📌 Replace `<Node-IP>` with any Kubernetes worker node's IP.

```bash
[root@k8s-master todo-app]# export NODE_PORT=$(kubectl get --namespace to-do -o jsonpath="{.spec.ports[0].nodePort}" services v1-todo-app)
[root@k8s-master todo-app]#   export NODE_IP=$(kubectl get nodes --namespace to-do -o jsonpath="{.items[0].status.addresses[0].address}")
[root@k8s-master todo-app]#   echo http://$NODE_IP:$NODE_PORT
http://10.12.23.19:30007
[root@k8s-master todo-app]#
```

Now our application is accessible on the mentioned URL - http://10.12.23.19:30007

Search this URL on webpage.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742198944400/26c62570-9cc4-4137-b676-2d03dd02fb4a.png align="center")

Hurray!! We successfully deployed our application on k8s-cluster using helm.

---

## **✅ Summary**

* Created a Helm chart for the **To-Do App**
    
* Configured **health probes** to ensure reliability
    
* Deployed the app with a **NodePort service (port 8000)**
    
* Verified everything using `kubectl` and `helm`
    
* Accessed the app using `http://<Node-IP>:30008`
    

🎯 **Now your To-Do App is successfully running on Kubernetes using Helm!** 🚀  
Let me know if you need any modifications. 😊