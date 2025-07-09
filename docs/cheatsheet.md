# [Kubernetes Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/quick-reference/)

- [Kubectl Context and Configuration](https://kubernetes.io/docs/reference/kubectl/quick-reference/#kubectl-context-and-configuration)
- [Creating Objects](https://kubernetes.io/docs/reference/kubectl/quick-reference/#creating-objects)
- [Interacting With Nodes and Cluster](https://kubernetes.io/docs/reference/kubectl/quick-reference/#interacting-with-nodes-and-cluster)


## Useful Commands to Automatically Create YAML Configs

### Create a Declarative YAML for a Pod

```bash
kubectl run nginxpod --image=nginx --dry-run=client -o yaml > ngpod.yaml
cat ngpod.yaml
```

### Create a YAML for a Deployment

```bash
kubectl create deployment ngdep --image=nginx --dry-run=client -o yaml > ngdep.yaml
```
