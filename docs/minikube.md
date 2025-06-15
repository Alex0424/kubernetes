# Setup Local Test Enviroment with Minikube

## 📋 System Requirements

- 🐧 Linux-based OS
- Docker or a compatible VM hypervisor (e.g., VirtualBox, Hyper-V)
- [kubectl](./setup_kubectl.md) installed

## 📦 Install Binary Minikube

[This is a copy of the official install docs](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download)

### 🔧 Binary Installation

```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

## 🟢 Start your cluster

```bash
minikube start
```

## 📡 Interact with your cluster
```bash
kubectl get po -A
```

Or

```bash
minikube kubectl -- get po -A
```

### 🖥️ Deploy dashboard
```bash
minikube dashboard
```

### 🧱 Get Nodes
```bash
kubectl get nodes
```

## 📦 Deploy a Test Application
```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0

kubectl expose deployment hello-minikube --type=NodePort --port=8080

# Display service
kubectl get services hello-minikube

# Access this service by letting minkube launch a web browser for you
minikube service hello-minikube

# Alternativly enable port forwarding
kubectl port-forward service/hello-minikube 7080:8080
```

## ❌ Delete Cluster & Resources
```bash
kubectl get svc
kubectl delete svc hello-minikube

kubectl get deploy
kubectl delete deploy hello-minikube

minikube stop
minikube delete
```

## 🛠 Commands to Manage Your Cluster

```bash
minikube pause                                      # Pause Kubernetes without impacting deployed applications

minikube unpause                                    # Unpause a paused instance

minikube stop                                       # Halt the cluster

minikube config set memory 9001                     # Change the default memory limit (requires a restart)

minikube addons list                                # minikube addons list

minikube start -p aged --kubernetes-version=v1.16.1 # Create a second cluster running an older Kubernetes release

minikube delete --all                               # Delete all of the minikube clusters
```