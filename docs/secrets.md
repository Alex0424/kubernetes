# [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.

# Base64 Encoding Method

There is a way to encrypt a secret but thats outside the scope of this docs.

!!! info "Base64 Encoding Example"

    You can use `base64` to encode and decode secrets for Kubernetes.  
    Kubernetes stores secret data as base64-encoded strings, so any values you provide will be encoded automatically when you create a Secret.

        # Encode a string to base64
        echo -n "secretpass" | base64
        # Output: c2VjcmV0cGFzcw==

        # Decode a base64 string
        echo 'c2VjcmV0cGFzcw==' | base64 --decode
        # Output: secretpass

### Create Secret | Imperative

```shell
kubectl create secret generic db-secret --from-literal=MYSQL_ROOT_PASSWORD=somecomplexpassword
# Output: secret/db-secret created
```

### Create Secret from Files

```shell
# Create files needed for rest of example.
echo -n 'admin' > ./username.txt
echo -n '1f2d1e2e67df' > ./password.txt

kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
```

### View Secret

```shell
$ kubectl get secret db-secret -o yaml
apiVersion: v1
data:
  MYSQL_ROOT_PASSWORD: c29tZWNvbXBsZXhwYXNzd29yZA==
kind: Secret
metadata:
  ...
```

### Create Secret | Declarative

First, encode your secret value using base64:

```shell
echo -n "somecomplexpassword" | base64
# Output: c29tZWNvbXBsZXhwYXNzd29yZA==
```

Then create a YAML file (e.g., `db-secret.yml`):

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  my_root_pass: c29tZWNvbXBsZXhwYXNzd29yZA==
```

### POD Reading Secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
  labels:
    app: db
    project: infinity
spec:
  containers:
    - name: mysql-container
      image: mysql:5.7
      envFrom:
        - secretRef:
            name: db-secret
```

### POD Reading Specific Secret Key

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: db-pod
  labels:
    app: db
    project: infinity
spec:
  containers:
    - name: mysql-container
      image: mysql:5.7
      env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: my_root_pass
```

## Docker config Secrets

Create Docker secret

```yaml
kubectl create secret docker-registry secret-tiger-docker \
  --docker-email=tiger@acme.example \
  --docker-username=tiger \
  --docker-password=pass1234 \
  --docker-server=my-registry.example:5000
```

This command creates a Secret of type kubernetes.io/dockerconfigjson

Retrieve the .data.dockerconfigjson field from that new Secret and decode the data:

```
kubectl get secret secret-tiger-docker -o jsonpath='{.data.*}' | base64 -d
```

Output

```
{
  "auths": {
    "my-registry.example:5000": {
      "username": "tiger",
      "password": "pass1234",
      "email": "tiger@acme.example",
      "auth": "dGlnZXI6cGFzczEyMzQ="
    }
  }
```

Create a Pod that uses your Secret

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg
spec:
  containers:
  - name: private-reg-container
    image: <your-private-image>
  imagePullSecrets:
  - name: secret-tiger-docker
```

## Hands On

```shell
echo -n "admin" | base64

echo -n "mysecretpass" | base 64

vim mysecret.yaml
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
data:
  username: YWRtaW4=
  password: bXlzZWNyZXRwYXNz
type: Opaque
```

VULNERABILITY: hardcoded-credentials Embedding credentials in source code risks unauthorized access

```shell
kubectl create -f mysecret.yaml
# secret/mysecret created
```
```shell
vim readsecret.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
    - name: mycontainer
      image: redis
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: username
              optional: false  # same as default; "mysecret" must exist and include a key named "username"
        - name: SECRET_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysecret
              key: password
              optional: false  # same as default; "mysecret" must exist and include a key named "password"
  restartPolicy: Never
```

```shell
kubectl create -f readsecret.yaml
# secret/mysecret created
```

```shell
kubectl get pod
```

```bash
# OUTPUT
NAME                 READY   STATUS    RESTARTS   AGE
secret-env-pod       1/1     Running   0          24s
```

```shell
kubectl exec --stdin --tty secret-env-pod -- /bin/bash
root@secret-env-pod:data# echo $SECRET_USERNAME
admin
root@secret-env-pod:data# echo $SECRET_PASSWORD
mysecretpass
```
