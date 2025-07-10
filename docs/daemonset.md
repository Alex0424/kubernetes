[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

A DaemonSet ensures that each node runs one copy of a specific Pod.

Commonly used for log collection, monitoring agents, or storage drivers.

DaemonSet pods can expose metrics that tools like Prometheus scrape and Grafana visualizes.

Example [DaemonSet YAML](https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/controllers/daemonset.yaml)

## Hands On: Create a DaemonSet

Copy yaml content from example above.

```shell
vim sampleds.yaml # Paste the yaml content here
```

```shell
kubectl apply -f samleds.yaml
# ................ created
```

Validate

```shell
kubectl get ds -A
```

```bash
# Output:
NAMESPACE      NAME                       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR
kube-system    ebs-csi-node                   3         3        3         3             3       kubernetes.io/os=linux
kube-system    fluentd-elasticsearch          3         3        3         3             3       <none>
kube-system    kops-controller                1         1        1         1             1       kops.k8s.io/kops-controller-pki=,node-role.kubernetes.io/master=
```

```shell
kubectl get pod -n kube-system
```

```shell
# Output:
NAME                                                                READY   STATUS    RESTARTS    AGE
coredns-7884856795-knjlw                                            1/1     Running   0           48m
coredns-7884856795-sclvx                                            1/1     Running   0           48m
coredns-autoscaler-57dd867df6-vqwjp                                 1/1     Running   0           48m
dns-controller-5869b5468f-g44fs                                     1/1     Running   0           48m
ebs-csi-controller-58c77d6dbc-srbqn                                 6/6     Running   0           48m
ebs-csi-node-6884r                                                  3/3     Running   0           48m
ebs-csi-node-bnsf6                                                  3/3     Running   1 (47m ago) 48m
ebs-csi-node-z59c6                                                  3/3     Running   1 (47m ago) 48m
etcd-manager-events-ip-172-20-37-60.us-east-2.compute.internal      1/1     Running   0           48m
etcd-manager-main-ip-172-20-37-60.us-east-2.compute.internal        1/1     Running   0           48m
**fluentd-elasticsearch-2mdqt**                                     1/1     Running   0           41s
**fluentd-elasticsearch-fbwb4**                                     1/1     Running   0           41s
**fluentd-elasticsearch-jsnh8**                                     1/1     Running   0           41s
kops-controller-w5rsl                                               1/1     Running   0           48m
kube-apiserver-ip-172-20-37-60.us-east-2.compute.internal           2/2     Running   0           49m
kube-controller-manager-ip-172-20-37-60.us-east-2.compute.internal  1/1     Running   0           49m
kube-proxy-ip-172-20-37-60.us-east-2.compute.internal               1/1     Running   0           48m
kube-proxy-ip-172-20-48-52.us-east-2.compute.internal               1/1     Running   0           47m
kube-proxy-ip-172-20-77-191.us-east-2.compute.internal              1/1     Running   0           46m
kube-scheduler-ip-172-20-37-60.us-east-2.compute.internal           1/1     Running   0           48m
```