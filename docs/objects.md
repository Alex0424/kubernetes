# Objects in K8S

## Pod

- The smallest deployable unit in Kubernetes.
- Represent a single instance of running processes in your cluster
- Can contain one or more containers that share storage and network
- Pods that run a single container
    - The "one-container-per-pod" model is the most common Kubernetes use case (Kubernetes manages the Pods rather than the containers directly).
- Multi Container POD
    - Tightly coupled and need to share resources
    - One main container and other as sidecar or init container (or both)
    - Each Pod is meant to run a single instance of a given application
    - Should use multiple Pods to scale horizontally

Definitions file in example similar to docker compose / docker run. `pod-setup.yml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp-pod
  labels:
    app: frontend
    project: infinity
spec:
  containers:
    - name: httpd-container
      image: httpd
      imagePullPolicy: IfNotPresent
      ports:
        - name: http-port
          containerPort: 8080 # exposed port
```

Then to create the pod run this command: 
```bash
kubectl create -f pod-setup.yml
```

Use commands below to get info

```bash
kubectl get pod
kubectl describe pod webapp-pod
kubectl get pod webapp-pod -o yaml
kubectl get pod webapp-pod -o yaml > webpod-definition.yml
```

Edit pod webapp-pod

```
kubectl edit webapp-pod
```



Type of Kind:

| Kind       | API Version            |
|------------|------------------------|
| Pod        | v1                     |
| Service    | v1                     |
| Deployment | apps/v1                |
| Ingress    | networking.k8s.io/v1   |



[Pods docs](https://kubernetes.io/docs/concepts/workloads/pods/)

## Service

- Provides a stable network endpoint to access Pods.
- Supports different types: ClusterIP (default), NodePort, LoadBalancer, and ExternalName.
- Enables load balancing across multiple Pod replicas.

## Replica Set

- Ensures a specified number of Pod replicas are running at any given time.
- Automatically replaces failed or terminated Pods to maintain the desired count.

## Deployment

- Provides declarative updates for Pods and ReplicaSets.
- Manages rollouts and rollbacks of application versions.
- Supports updating container images via image tags.

## Config Map

- Stores non-sensitive configuration data as key-value pairs.
- Used to decouple configuration artifacts from application code.
- Can inject data into Pods as environment variables, command-line arguments, or configuration files.

## Secret

- Stores sensitive data (e.g., passwords, tokens, SSH keys) in base64-encoded format.
- Prevents exposing sensitive information in plain text.
- Can be mounted as files or exposed as environment variables in Pods.

## Volumes

- Provide persistent or temporary storage for Pods.

- Volume types include:
    - **emptyDir** – Temporary storage shared between containers in a Pod.
    - **hostPath** – Mounts a file or directory from the host node.
    - **persistentVolumeClaim** (**PVC**) – Abstraction for durable storage, often backed by cloud storage solutions.
    - **configMap**/**secret** – Mount configuration or secret data as files.
    - **nfs**, **csi**, **awsElasticBlockStore**, **etc**. – Other network and cloud-specific storage options.