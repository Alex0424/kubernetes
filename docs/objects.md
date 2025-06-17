# Objects in K8S

## Objects



### Pod

- The smallest deployable unit in Kubernetes.
- Represent a single instance of running process in your cluster
- Can contain one or more containers that share storage and network

### Service

- Provides a stable network endpoint to access Pods.
- Supports different types: ClusterIP (default), NodePort, LoadBalancer, and ExternalName.
- Enables load balancing across multiple Pod replicas.

### Replica Set

- Ensures a specified number of Pod replicas are running at any given time.
- Automatically replaces failed or terminated Pods to maintain the desired count.

### Deployment

- Provides declarative updates for Pods and ReplicaSets.
- Manages rollouts and rollbacks of application versions.
- Supports updating container images via image tags.

### Config Map

- Stores non-sensitive configuration data as key-value pairs.
- Used to decouple configuration artifacts from application code.
- Can inject data into Pods as environment variables, command-line arguments, or configuration files.

### Secret

- Stores sensitive data (e.g., passwords, tokens, SSH keys) in base64-encoded format.
- Prevents exposing sensitive information in plain text.
- Can be mounted as files or exposed as environment variables in Pods.

### Volumes

- Provide persistent or temporary storage for Pods.

- Volume types include:
    - **emptyDir** – Temporary storage shared between containers in a Pod.
    - **hostPath** – Mounts a file or directory from the host node.
    - **persistentVolumeClaim** (**PVC**) – Abstraction for durable storage, often backed by cloud storage solutions.
    - **configMap**/**secret** – Mount configuration or secret data as files.
    - **nfs**, **csi**, **awsElasticBlockStore**, **etc**. – Other network and cloud-specific storage options.