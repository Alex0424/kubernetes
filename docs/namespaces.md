# Namespaces

Namespaces lets you isolate group of resources within a single cluster.

## Default Kubernetes Namespace

These namespaces gets created automatically when a cluster is created:

- default
- kube-system
- kube-public
- kube-node-lease

Get namespaces: `kubectl get namespaces`

## CLI Commands

Get all objects in default namespace: `kubectl get all`

Show all resources from all namespaces: `kubectl get all-  namespaces`

Services from a specific namespace: `kubectl svc -n kube-system`

Create namespace: `kubectl create ns kubekart`

And run a pod: `kubectl run nginx1 --image=nginx -n kubekart`

Get pod from your namespace: `kubectl get podf -n kubekart`

Delete a namespace: `kubectl delete ns kubekart`

## Specify Namespace in Definition File:

```
cat pod1.yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx12
  namespace: kubekart
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

`kubectl apply -f pod1.yaml`

Extra namespaces example:

- dev
- prod

## Change prefered namespace in kubeconfig file

```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
# Validate
kubectl config view --minify | grep namespace:
```