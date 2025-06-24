# Deployment

Upgrade your apps (pod images).

Rollbacks if something goes wrong.

A deployment controller provides declarative updates for Pods and ReplicaSets (Deployment creates ReplicaSet to manage number of PODS).

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate.

## Update Image Tag

example tags:

- nginx:1.0
- nginx:2.0
- nginx:3.0

With deployments you can upgrade from old image tag e.g: `nginx:1.0` to `nginx:3.0`.

If something goes wrong then the image will roll back to old version `nginx:1.0`.

## Hands On

`vim nginx-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

Run the deployment
```bash
kubectl apply -y nginx-deployment.yaml

kubectl get deployments

# example output:
# NAME               READY   UP-TO-DATE   AVAILABLE   AGE
# nginx-deployment   0/3     0            0           1s


kubectl get deploy

kubectl get rs

kubectl get pod

kubectl describe pod <pod-name>
```

## Updating a deployment

```bash
kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
```

check if new version was changed:
```
kubectl describe pod <pod-name>
```

search for `image:`

confirm that the version is set to `1.16.1`

## Rollback to a Previous Revision

check if there is old revision

```
kubectl get rs
```

output:
```
NAME                       DESIRED
nginx-deployment-11111111  0 <-- previous revision
nginx-deployment-11111111  3
```

```
kubectl rollout undo deployment/nginx-deployment
```

```
kubectl get rs
```

output:
```
NAME                       DESIRED
nginx-deployment-11111111  0 
nginx-deployment-11111111  3 <-- previous revision
```

```
kubectl get pod
kubectl describe pod <pod-name> | grep Image
```

check revision rollout history
```
kubectl rollout history deployment/nginx-deployment
```

rollout to revision number
```
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

## Delete Deployment

```
kubectl get deploy
```

```
kubectl delete deploy <name>
```

## Remember

imperetive is for learning, testing purpose.

But in production do it through definition files.

## Link to DOCS

[Link to Kubernetes deployment - official docs](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)