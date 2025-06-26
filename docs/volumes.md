# Volumes

## Type of Volumes

- awsElasticBlockStore
- azureDisk
- cephhfs
- cinder
- fc (fibre chanel)
- flocker (depricated)
- gcePersistentDisk (for Google Cloud)
- glusterfs (RedHat)
- iscsi
- local
- NFS
- portworxVolume
- vSphere VMDK
- hostPath (not recomended for production)

## hostPath configuration example

```yaml

---
# This manifest mounts /data/foo on the host as /foo inside the
# single container that runs within the hostpath-example-linux Pod.
#
# The mount into the container is read-only.
apiVersion: v1
kind: Pod
metadata:
  name: hostpath-example-linux
spec:
  os: { name: linux }
  nodeSelector:
    kubernetes.io/os: linux
  containers:
  - name: example-container
    image: registry.k8s.io/test-webserver
    volumeMounts:
    - mountPath: /foo # <-- container volume
      name: example-volume 
      readOnly: true
  volumes:
  - name: example-volume
    # mount /data/foo, but only if that directory already exists
    hostPath:
      path: /data/foo # directory location on host
      type: Directory # this field is optional
```

## Hands on

```shell
vim mysqlpod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dbpod
spec:
  os: { name: linux }
  nodeSelector:
    kubernetes.io/os: linux
  containers:
  - name: mysql
    image: mysql:5.7
    volumeMounts:
    - mountPath: /var/lib/mysql # <-- container volume
      name: dbvol
      readOnly: true
  volumes:
  - name: dbvol
    hostPath:
      path: /data
      type: DirectoryOrCreate
```

```shell
kubectl apply -f mysqlpod.yaml

kubectl get pods

kubectl describe dbpod

kubectl delete pod dbpod
```

validate that volume host path is `/data`

validate that mount volume is `/var/lib/mysql`