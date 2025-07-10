# Helm

## What is Helm Package Manager?

Helm helps you manage your application as a single package on a Kubernetes cluster.

To run an application, we often need to use multiple Kubernetes objects, such as:

- Deployments
- Services
- Ingress
- Backend Objects:
    - Volumes
    - Secrets
    - ConfigMaps
    - DaemonSets
    - StatefulSets
    - etc.

Using Helm, we can convert these definition files into a single Helm chart.

This means an application becomes a bundle of different objects packaged into a Helm chart, allowing us to deploy the application as a single suite.

This means we deploy the application as a single suit.

Helm provides commands to manage applications, such as:

- `helm install <app>`
- `helm uninstall <app>`
- `helm upgrade <app>` (change settings or versions)
- etc.

## Key Components of Helm in Kubernetes

. **Charts**  
    - A package of pre-configured Kubernetes resources for applications.
2. **Repositories**  
    - Storage locations for managing and sharing Helm charts.
3. **Releases**  
    - Instances of charts running in a Kubernetes cluster.
4. **Values**  
    - Configuration settings that customize charts during deployment.

## Chart Structure

```plaintext
wordpress-chart/
├── templates/
│   ├── ingress.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── NOTES.txt
│   ├── persistent-volume-claims.yaml
│   ├── secret.yaml
│   ├── wordpress-deployment.yaml
│   ├── wordpress-service.yaml
├── Chart.yaml
├── values.yaml
```

1. **Create a Chart**  
    - Use the command: `helm create myapp`

2. **Content of the Chart**  
    - Create a `templates` folder and add `values.yaml` files.

3. **Deploy and Manage**  
    - Deploy the chart: `helm install <release-name> <chart-path>`
    - Upgrade the chart: `helm upgrade <release-name> <chart-name> -f values.yaml`
    - Uninstall the chart: `helm uninstall <release-name>`
    - List releases: `helm list`

4. **Helm Repositories**  
    - Add a repository: `helm repo add bitnami https://charts.bitnami.com/bitnami`
    - Update repositories: `helm repo update`
    - Install a chart from a repository: `helm install my-nginx bitnami/nginx`