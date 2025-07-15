# Config Map

https://kubernetes.io/docs/concepts/configuration/configmap/#using-configmaps-as-environment-variables

## Environment Variables

Pods are disposable and do not retain persistent changes.

We can inject environment variables into a Pod at runtime.

### Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-configmap
spec:
  containers:
  - name: envars-test-container
    image: nginx
    env:
    - name: CONFIGMAP_USERNAME
      valueFrom:
        configMapKeyRef:
          name: myconfigmap
          key: username
```

## Create Config Maps - Imperative

https://kubernetes.io/docs/reference/kubectl/generated/kubectl_create/kubectl_create_configmap/

```shell
kubectl create configmap db-config --from-literal=MYSQL_DATABASE=accounts \
 --from-literal=MYSQL_ROOT_PASSWORD=somecomplexpass
```
```bash
configmap/db-config created
```

```shell
kubectl get cm
```

```shell
kubectl get cm db-config -o yaml
```

```shell
kubectl describe cm db-config
```

## Create Config Maps - Declarative

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: db-config
data:
  MYSQL_ROOT_PASSWORD: somecomplexpass
  MYSQL_DATABASE: accounts
```

Apply changes:
```shell
kubectl create -f db-cm.yml
```

### POD Reading Config Maps

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
        - configMapRef:
            name: db-config
      ports:
        - name: db-port
          containerPort: 3306
```

### Select a Key from a Config Map

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
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: db-config
              key: DB_HOST
      ports:
        - name: db-port
          containerPort: 3306
```

## Hands On

```
vim samplecm.yaml
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true
```

```shell
kubectl apply -f samplecm.yaml
```

```shell
kubectl get cm
```

```shell
kubectl get cm game-demo -o yaml
```

There are four different ways that you can use a ConfigMap to configure a container inside a Pod:

1. Inside a container command and args
2. Environment variables for a container
3. Add a file in read-only volume, for the application to read
4. Write code to run inside the Pod that uses the Kubernetes API to read a ConfigMap

```shell
vim readcmpod.yaml
```

```shell
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo-pod
spec:
  containers:
    - name: demo
      image: alpine
      command: ["sleep", "3600"]
      env:
        # Define the environment variable
        - name: PLAYER_INITIAL_LIVES # Notice that the case is different here
                                     # from the key name in the ConfigMap.
          valueFrom:
            configMapKeyRef:
              name: game-demo           # The ConfigMap this value comes from.
              key: player_initial_lives # The key to fetch.
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
  # You set volumes at the Pod level, then mount them into containers inside that Pod
  - name: config
    configMap:
      # Provide the name of the ConfigMap you want to mount.
      name: game-demo
      # An array of keys from the ConfigMap to create as files
      items:
      - key: "game.properties"
        path: "game.properties"
      - key: "user-interface.properties"
        path: "user-interface.properties"
```

```shell
kubectl get cm
```

```shell
kubectl apply -f readcmpod.yaml
```

```shell
kubectl get pod
```

```shell
kubectl exec --stdin --tty configmap-demo-pod -- /bin/sh
```

Validate
```shell
ls /config/
```

```shell
cat /config/game.properties
```

```shell
cat /config/user-interface.properties
```

```shell
echo $PLAYER_INITIAL_LIVES
```

```shell
echo $UI_PROPERTIES_FILE_NAME
```
