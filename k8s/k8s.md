# Kubenetes

## Concepts

### Cluster

A cluster is a set of physical or virtual machines and other infrastructure resources used by Kubernetes to run your applications. A cluster consists of at least one worker node and at least one master node.

### Node

### Pod

#### Characteristics

1. **Smallest** unit of Kubernetes

2. **Abstraction** over container

3. Uually **1 application** per pod

4. Each pod has **unique IP**

5. **New IP address** on restart

**Smallest** 

### Service

### Ingress

### Namespace

### ConfigMap

### Secret

### PersistentVolume

### PersistentVolumeClaim

### Job

### CronJob

### DaemonSet

### Deployment

### ReplicaSet

### StatefulSet

### HorizontalPodAutoscaler

## Installation

Reference this [link](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

### Install on Arch Linux

```bash
sudo pacman -S kubectl
```
