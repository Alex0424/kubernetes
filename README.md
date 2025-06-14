# Kubernetes

📖 For full documentation, see the [`/docs`](./docs) folder.

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

## Kubernetes Provides

- Service discovery and load balancing
- Storage orchestration
- - SAN, NAV, EBS volumes, CEPH storage
- Automated rollouts and rollbacks
- Automatic bin packing
- Self-healing
- Secret and configuration management

## Kubernetes Architecture

![image](https://github.com/user-attachments/assets/a0cf3c89-a652-48d7-b1be-310dbe17f925)
![image](https://github.com/user-attachments/assets/58c13b95-e46f-46dc-9263-36107432d88c)

### Master: Kube API Server
- Handles all the requests and enables communication across stack services.
- Component on the master that exposes the Kubernetes API.
- It is the frontend for Kubernetes control plane.
- Admins connects to it usin Kubectl CLI.
- Web Dashboard can be integrated using this API.
- and many more integration

### Master: ETCD Server
- Stores all the information for Kubernetes cluster
- Consist and highly-availabe key and value data store as Kubernetes backing store for all cluster data.
- Kube API store retrives info from it.
- Should be backed up regularly.
- Stores current state of everything in the cluster.

### Master: Kube Sheduler
- watches newly created pods that has no nodes assigned and select a node for them to run on.
- Factors taken into account for sheduling a decisions include:
  - individual and collective resource requirements,
  - hardware/software/policy constraints,
  - affinity and anti-affinity specifications,
  - data locality,
  - inter-workload interference and deadliner

### Master: Controller Manager

Logically, each controller is a separate process,

To reduce the complexity, they are all compiled into a single

These controllers include:
- Node Controller: Responsible to noticing and responding when nodes goes down.
- Replication Controller: Responsible for maintaining the correct amount of pods for every replication controller object in the system.
- Endpoint Controller: Populates the endpoints object (join services and pods)
- Service Account and Token Controller: Create default account and tokens for new namespace

### Node Components

Kubelet
- An agent that runs on each node in the cluster. It makes sure that containers are running in a pod.

Kube Proxy
- network proxy that runs on each node in your cluster
- Network Rule
  - rules allow network communication to your pods outside or inside of your cluster

Container Runtime: Kubernetes supports several container runtime
- Docker
- containerd
- cri-o, rktlet
- Kubernetes CRI (Container Runtime Interface)

### PODS

![image](https://github.com/user-attachments/assets/1a492305-50a8-4ab1-88b5-b237ab6cda97)
![image](https://github.com/user-attachments/assets/4c7ad2c4-1018-4de7-bc98-86ab389f6883)

### Addons

DNS, Web UI, Container Resource Monitoring, Cluster Level Logging.

## Kubernetes Setup Tools

Hard Way: Manual Setup

Testing: Minikube
- One Node Kubernetes cluster on your computer

Kubeadm:
- Multi node Kubernetes Cluster
- Can be created on any plattforms VM's, EC2, physical machines etc

Kops:
- Multi node Kubernetes Cluster on AWS
