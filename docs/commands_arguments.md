# Commands and Arguments


## CMD

Command that will start the container process:

```Dockerfile
FROM ubuntu
CMD echo "Hello World"
```

```Dockerfile
FROM ubuntu
CMD ["/usr/bin/wc","--help"]
```

Similar to CMD but higher priority
```Dockerfile
FROM ubuntu
ENTRYPOINT ["executable", "param1", "param2"]
```

If ENTRYPOINT and CMD are used together then the ENTRYPOINT will run first.

Usually when used together the ENTRYPOINT would be the command and CMD the argument, Example:

```Dockerfile
FROM ubuntu
ENTRYPOINT[echo]
CMD["hi"]
```


## Kubernetes Example

```shell
vim commands.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  labels:
    purpose: demonstrate-command
spec:
  containers:

  - name: command-demo-container
    image: debian
    command: ["printenv"]
    args: ["HOSTNAME", "KUBERNETES_PORT"]

  - name: echo-demo
    image: debian
    env:
      - name: MESSAGE
        value: "hello world"
    command: ["/bin/echo"]
    args: ["$(MESSAGE)"]

  restartPolicy: OnFailure
```

> **Note:**  
> You can only use one `command` and `args` per container.  
> To try both examples, comment/uncomment the relevant lines or create two containers in the pod.
> 2 containers running is not recomended in production unless its side-container or container that will start another container.

env:
- name: MESSAGE
  value: "hello world"
command: ["/bin/echo"]
args: ["$(MESSAGE)"]

start pod

```shell
kubectl apply -f commands.yaml
kubectl get pod

# wait for status: Completed

kubectl logs command-demo
```

[Docs](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)