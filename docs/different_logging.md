# Logging

## Test Environments

Before deploying to production, always verify the application in different environments:

1. Test in local environment
2. Test in test/dev environment
3. Finally in production environment

---

## Debugging Your Kubernetes Cluster

Mistakes and bugs can happen — be prepared to investigate and resolve them efficiently.

### Troubleshooting Pods

List pods and their status:

```bash
kubectl get pod
kubectl get pod -o wide
```

## Viewing Pod Logs and Events

#### Level 1: View Full Pod Definition

```
kubectl get pod <pod-name> -o yaml
```

#### Level 2: Describe the pod (Look at Events for errors)

```
kubectl describe pod <pod-name>
```

#### Level 3: View Pod logs (commands/process outputs)

```
kubectl logs <pod-name>
```

## Scenario Example: Debugging a Broken Image

Suppose you find the following error in the logs:

```
# Logs:
kubelet     Pulling image "nginax:1.15.0"
kubelet     Failed to pull image "nginax:1.15.0"
```

The issue is a misspelled image name (nginax instead of nginx). To fix it:

#### Step 1: Delete the Pod

```bash
kubectl delete pod nginx12
```

#### Step 2: Edit the Pod

```bash
# pod2.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx12
spec:
  containers:
  - name: 
    image: nginx:1.15.0 # <-- Corrected image name
    ports:
    - containerPort: 80
```

#### Step 3: Apply the Updated Pod

```bash
kubectl apply -f pod2.yaml
# output example: pod/nginx12 created
```

#### Step 4: Verify the Pod is Running
```bash
kubect get pod
```

You should see:
```
nginx12 1/1 Running
```