# [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.

# Encoding Method

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
````

## Encryption