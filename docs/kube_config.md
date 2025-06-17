# What is Kube Config File?

The kubeconfig file is a configuration file used by `kubectl` to access Kubernetes clusters. By default, it is located at `~/.kube/config`

## The Kubeconfig File Organizes Information About:

1. Cluster – API server addresses and associated settings
2. Users – Credentials and authentication information
3. Namespaces – Default namespace for kubectl commands (A namespace in Kubernetes is a way to divide cluster resources between multiple users or applications.)
4. Authentication mechanisms – How users authenticate with clusters

## kubeconfig example

```
apiVersion: v1
kind: Config

proxy-url: https://proxy.host:3128

clusters:
- cluster:
    proxy-url: http://proxy.example.org:3128
    server: https://k8s.example.org/k8s/clusters/c-xxyyzz
  name: development

users:
- name: developer

contexts:
- context:
    cluster: development
    user: developer
  name: development

current-context: development
```

The **contexts** section maps a cluster to a user, allowing `ubectl` to know which credentials to use for which cluster.

The **current-context** defines which context is used by default when running `kubectl` commands.

If you have `kubectl` installed on another machine, you can copy the kubeconfig file to that machine to access the same Kubernetes cluster from both.

`current-context` 

if you have cubectl installed on another machine and you can copy cubeconfig file to that machine then you can talk to api master node from both machines 

Find more information see the [official kubeconfig docs](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).