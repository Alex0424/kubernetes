# Kubernetes

If Docker Engine fails, then all the containers will be down and users would not be able to access them

This is where Kubernetes comes into play to create multiple docker engines called Docker nodes in kubernetes wich will give high availability

![image](https://github.com/user-attachments/assets/cca32334-c0ff-4273-89a8-4e8f3fbe2b41)


If the container on third node fails then the container would get migrated to a healthy node

## Container Orchestration

Cluster of Docker/worker nodes where the Orchastrator would distribute the container

Docker nodes would be one single pool of resource which is foult tolerant.

## Orchestration Tools

- Docker Swarm
- Kubernetes
- Mesosphere Marathon
- AWS ECS & EKS
- Azure Container Service
- Google Container Engine
- CoreOS Fleet
- OpenShift

## Google + Containers (2014)

Everything at Google, from Search to Gmail, is packaged and run in a Linux container. Each week we launch more than 2 billion container instances across our global data centers, and the power of containers has enabled both more reliable services and higher, more-efficient scalability. Now we´re taking another step toward making capabilities available to developers everywhere.

### History

Kubernetes is created by Google to manage their containers AKA Borg

Mid-2014: Google introduced Kubernetes as an open source version of Borg.

July 21-2015: Kubernetes v1.0 gets released. Along with the release, Google partnered with the Linux Foundation to form the Cloud Native Computing Foundation (CNCF).

2016: Kubernetes Goes Mainstream!
- Kops, Minikube, kubeadm etc
- September 29: Pokemon GO! Kubernetes Case Study Released!

2017: Enterprice Adoption
- Google and IBM announce Istio
- Github runs on Kubernetes
- Oracle joined the Cloud Native Computing Foundation



## 
