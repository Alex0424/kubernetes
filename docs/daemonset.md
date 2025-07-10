[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

A DaemonSet ensures that each node runs one copy of a specific Pod.

Commonly used for log collection, monitoring agents, or storage drivers.

DaemonSet pods can expose metrics that tools like Prometheus scrape and Grafana visualizes.

Example [DaemonSet YAML](https://raw.githubusercontent.com/kubernetes/website/main/content/en/examples/controllers/daemonset.yaml)