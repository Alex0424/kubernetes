# Helm Hands On

## WordPress Setup

### Workflow for Helm

1. **Create Kubernetes Definition Files**  
    - Set up WordPress using Kubernetes objects:
        - Deployment
        - Service
        - Persistent Volume Claim (PVC)
        - Ingress
        - Secret

2. **Convert to Helm**  
    - Use AI tools to develop Helm charts efficiently.
        - Amazon Q (Generative AI)

3. **Deploy and Manage**  
    - Deploy the chart: `helm install <release-name> <chart-path>`
    - Upgrade the chart: `helm upgrade <release-name> <chart-name> -f values.yaml`
    - Uninstall the chart: `helm uninstall <release-name>`
    - List releases: `helm list`

4. **More Options in Helm**  
    - Use Code Assistant to implement Dev Best practices.

## Demo

[Have kubectl on your PC](../setup_kubectl)

[Have a kubernetes cluster installed on your AWS EC 2 instance](../setup_kops/#create-your-kubernetes-cluster)

```shell
kubectl get nodes
# NAME        STATUS         ROLE
# i-dg3       Ready          node
# i-gha       Ready          control-plane
# i-7df       Ready          node
```

setup ingress controller:

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.2/deploy/static/provider/aws/deploy.yaml
```

Display and copy entire cubeconfig file:

```shell
cat ~/.kube/config
```

create a kubeconfig file on your PC

```shell
mkdir ~/.kube
vim ~/.kube/config # Paste the entire cubeconfig contet here.
```

### Create a Worpress App & MySQL DB in Helm

How?

1. Open a browser, find the definition files, copy paste and make changes.

    1. In google, search for `wordpress kubernetes definitions`
    2. You should find a page like [`Deploying WordPress and MySQL with Persistent Volumes`](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)
    3. Teke this resources and create files, change settings and run it

2. Ask AmazonQ AI to create it for us.
    1. Install and Setup AmazonQ from VSC Extensions
    2. Use the prompt to create the resources/files
    3. AmazonQ is currently the most accurate AI for Kubernetes

### Build with AmazonQ

Make sure we have Nginx ingress controller:
```shell
kubectl get ns  # NameSpaces
# NAME              STATUS   AGE
# default           Active   46m
# ingress-nginx     Active   21m   <---
# kube-node-lease   Active   46m
# kube-public       Active   46m
# kube-system       Active   46m
```

Check storage class:

```shell
kubectl get sc # StorageClass
```
```bash
# OUTPUT
NAME                   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
default                kubernetes.io/aws-ebs   Delete          Immediate              false                  47m
gp2                    kubernetes.io/aws-ebs   Delete          Immediate              false                  47m
kops-csi-1-21 (default) ebs.csi.aws.com        Delete          WaitForFirstConsumer   true                   47m
kops-ssd-1-17          kubernetes.io/aws-ebs   Delete          WaitForFirstConsumer   true                   47m
```

As you see we have EBS type volume that was created by 'Kops' for AWS.

So we need to mention this EBS volume type as 'Storage Class'.

Make sure to have all the information before writing the prompt.

### AmazonQ Prompt Demo

Open a new foulder with VSC.

Prompt AmazonQ this text and wait for AI to build the files.

```
Wordpress setup kubernetes definitions files. Separate files for wordpress app, mysql (needs to be version 8.0), app service, db service, PVC, secret and ingress. PVC should use storage class default, secret file should contain all the db users and db passwords for mysql and wordpress both. Ingress will be nginx with hostname wordpress.alexanderlindholm.net.
```

7 files should be created:
```shell
.
├── mysql-deployment.yaml
├── mysql-pvc.yaml
├── mysql-service.yaml
├── wordpress-deployment.yaml
├── wordpress-ingress.yaml
├── wordpress-pvc.yaml
├── wordpress-secret.yaml
├── wordpress-service.yaml

1 directory, 7 files
```

Install [Helm](https://helm.sh/docs/intro/install/)

Create Helm chart

```shell
helm create wp-chart
```

Additional example/template files should be created in wp-chart directory:

```shell
.
├── mysql-deployment.yaml
├── mysql-pvc.yaml
├── mysql-service.yaml
├── wordpress-deployment.yaml
├── wordpress-ingress.yaml
├── wordpress-pvc.yaml
├── wordpress-secret.yaml
├── wordpress-service.yaml
└── wp-chart
    ├── charts
    ├── Chart.yaml
    ├── templates
    │   ├── deployment.yaml
    │   ├── _helpers.tpl
    │   ├── hpa.yaml
    │   ├── ingress.yaml
    │   ├── NOTES.txt
    │   ├── serviceaccount.yaml
    │   ├── service.yaml
    │   └── tests
    │       └── test-connection.yaml
    └── values.yaml

5 directories, 17 files
```

Delete all `.yaml` files in templates directory:

```shell
rm wp-chart/templates/*.yaml
```

Delete the wp-chart directory:

```shell
rm -rf wp-chart/templates/tests/
```

Clear all text in `values.yaml`:

```shell
echo "" > wp-chart/values.yaml
```

File structure should now look like this:
```shell
.
├── mysql-deployment.yaml
├── mysql-pvc.yaml
├── mysql-service.yaml
├── wordpress-deployment.yaml
├── wordpress-ingress.yaml
├── wordpress-pvc.yaml
├── wordpress-secret.yaml
├── wordpress-service.yaml
└── wp-chart
    ├── charts
    ├── Chart.yaml
    ├── templates
    │   ├── _helpers.tpl
    │   └── NOTES.txt
    └── values.yaml

4 directories, 12 files
```

Copy ai created kubernetes definition files to templates directory:

```shell
cp ./* ./wp-chart/templates/
```

File Structure should now look like:
```shell
.
├── mysql-deployment.yaml
├── mysql-pvc.yaml
├── mysql-service.yaml
├── wordpress-deployment.yaml
├── wordpress-ingress.yaml
├── wordpress-pvc.yaml
├── wordpress-secret.yaml
├── wordpress-service.yaml
└── wp-chart
    ├── charts
    ├── Chart.yaml
    ├── templates
    │   ├── _helpers.tpl
    │   ├── mysql-deployment.yaml
    │   ├── mysql-pvc.yaml
    │   ├── mysql-service.yaml
    │   ├── NOTES.txt
    │   ├── wordpress-deployment.yaml
    │   ├── wordpress-ingress.yaml
    │   ├── wordpress-pvc.yaml
    │   ├── wordpress-secret.yaml
    │   └── wordpress-service.yaml
    └── values.yaml

4 directories, 20 files
```

in `mysql-deployment.yaml`

replace

```yaml
metadata:
  name: wordpress-mysql
```

to

```yaml
metadata:
  name: {{ include "word-chart.fullname" . }}-app
```

replace

```yaml
image: mysql:8.0
```

to

```yaml
image: {{ .Values.mysql.image.repository }}:{{ .Values.mysql.image.tag }}
```

Add values in `mysql-deployment.yaml`

```yaml
mysql:
  image:
    repository: mysql
    tag: 8.0
```

Then to deploy the app run

```shell
helm install demo ./wp-chart
```