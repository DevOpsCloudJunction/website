---
title: "Kubernetes Basics"
description: "Introduction to Kubernetes"
excerpt: "Explore various strategies and best practices for managing branches in Git"
date: 2024-10-24T12:06:50-04:00
lastmod: 2024-10-24T12:06:50-04:00
draft: false
weight: 100
images: [kubernetes_cloud.jpg]
categories: ["Devops", "Kubernetes"]
tags: ["Devops", "SRE","Kubernetes"]
contributors: [atharva2six]
pinned: false
homepage: false
comments: true
---
# Introduction to Kubernetes
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It was originally designed by Google, and is now maintained by the Cloud Native Computing Foundation (CNCF).

## What is Kubernetes?
Kubernetes provides a framework for deploying and managing containerized applications across multiple hosts. It abstracts the underlying infrastructure, allowing developers to focus on writing code rather than managing infrastructure.
### Key Features of Kubernetes
- Container Orchestration: Automates deployment, scaling, and management of containers.
- Declarative Configuration: Define desired application state, and Kubernetes maintains it.
- Self-healing: Automatically restarts failed containers.
- Resource Management: Efficiently allocates resources (CPU, memory) to containers.
- Scalability: Scale applications horizontally (add/remove replicas) or vertically (increase/decrease resources).
- Multi-cloud Support: Deploy applications across multiple cloud providers.

**Why Kubernetes is essential ?**

Kubernetes is essential for modern applications due to its ability to automate, scale, and manage containerized workloads, ensuring high availability, efficiency, and scalability in complex, distributed environments.

## What is Pod?
- A Pod is the smallest and most basic deployable unit of computing resources. It represents a logical host for one or more containers. A Pod is ephemeral and can be created, scaled, and deleted as needed.

### Kubernetes Pod Management

#### Command to List Pods
```bash
kubectl get pods
```
**Description**: List all pods in the current namespace.

#### Command to Create a Pod Quickly Just with Image
```bash
kubectl run nginx --image=nginx
```
**Description**: Create a pod named `nginx` using the `nginx` image.

#### Command to Describe More Information About the Pod
```bash
kubectl describe pod podname
```
**Description**: Display detailed information about the specified pod.

#### Command to Delete a Pod
```bash
kubectl delete pod podname
```
**Description**: Delete the specified pod.

#### Command to Apply a File
```bash
kubectl apply -f pod.yaml
```
**Description**: Apply the configuration from `pod.yaml` to create or update resources.

---
## What is Replicaset?
- A ReplicaSet (RS) is a controller that ensures a specified number of identical Pod replicas are running at any given time. It provides redundancy, high availability, and scalability for applications.

### Kubernetes ReplicaSet Management

#### Commands to List ReplicaSets
```bash
kubectl get replicaset
```
**Description**: List all ReplicaSets in the current namespace.

#### Command to Delete a ReplicaSet
```bash
kubectl delete replicaset myapp-replica
```
**Description**: Delete a ReplicaSet named `myapp-replica`.

#### Command to Replace a ReplicaSet
```bash
kubectl replace -f replicaset-def.yaml
```
**Description**: Replace an existing ReplicaSet with a new definition from a YAML file.

#### Command to Scale a ReplicaSet
```bash
kubectl scale --replicas=6 -f replicaset-def.yaml
```
**Description**: Scale a ReplicaSet to 6 replicas using a YAML file.

#### Command to Check API Version of ReplicaSet
```bash
kubectl api-resources | grep replicaset
```
**Description**: Check the API version of ReplicaSets.

#### ReplicaSet YAML File Configuration
```bash
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-web-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-web-server
  template:
    metadata:
      labels:
        app: my-web-server
    spec:
      containers:
      - name: web-server
        image: nginx:latest
        ports:
        - containerPort: 80
```
---
## What is Deployment in Kubernetes?
- Deployments are a Kubernetes controller that manages the rollout of new versions of an application, ensuring scalable, self-healing, and controlled deployments.

- Deployment Controller used to manage the following
1. Creation: Deployment creates ReplicaSet, which creates Pods.
2. Scaling: Deployment adjusts ReplicaSet's replica count.
3. Updates: Deployment updates ReplicaSet's Pod template.
4. Deletion: Deployment deletes ReplicaSet and Pods.

### Kubernetes Deployment Management

#### Command to Create a Deployment
```bash
kubectl apply -f deploy.yaml
```
**Description**: Create a deployment using the configuration in `deploy.yaml`.

#### Command to Get All Resources (Deployment, ReplicaSets, Pods)
```bash
kubectl get all
```
**Description**: Get a list of all resources including deployments, ReplicaSets, and pods in the current namespace.

#### Command to Describe a Deployment
```bash
kubectl describe deployment <deployment-name>
```
**Description**: Display detailed information about the specified deployment.
#### Example Deployment YAML Configuration
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-web-server
  template:
    metadata:
      labels:
        app: my-web-server
    spec:
      containers:
      - name: web-server
        image: nginx:latest
        ports:
        - containerPort: 80
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
```
**To apply this configuration, save it to a file (e.g., deployment.yaml) and run**
```bash
kubectl apply -f deployment.yaml
```

**This Deployment configuration**
- Creates a Deployment named "my-web-server".
- Runs 3 replicas of the nginx:latest container.
- Exposes port 80.
- Uses a RollingUpdate strategy with maxSurge=1 and maxUnavailable=0.

## ReplicaSet vs Deployment: Key Differences
### ReplicaSet
- Purpose: A ReplicaSet ensures that a specified number of pod replicas are running at all times. If a Pod goes down, the ReplicaSet automatically starts a new one to replace it.
- Usage: It’s mostly used for ensuring pod replication and high availability. However, it does not manage updates or rollbacks.
- Manual Updates: If you want to update your application (e.g., updating the container image), you’ll have to manually update the ReplicaSet or create a new one.

###  Deployment
- Purpose: A Deployment is a higher-level controller that manages ReplicaSets and provides automated updates, rollbacks, and scaling. It creates and manages ReplicaSets, and also makes updating your application easier.
- Usage: A Deployment is used for managing updates (rolling updates, blue-green deployments, etc.), scaling, and ensuring that your application stays available while changes are applied.
- Automated Updates: With Deployments, you can perform rolling updates to transition from one version of your application to another without downtime. It also allows you to easily roll back to a previous version if an update causes issues.

#### Key Differences
- ReplicaSet only ensures that the correct number of replicas are running, but doesn't manage application updates.
- Deployment handles both replication (through ReplicaSets) and application updates/rollbacks, making it more flexible and powerful for managing applications over time.