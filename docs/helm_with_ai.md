# Helm with AI

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

Use AmazonQ to read definition files and create the chart for us.


Prompt AmazonQ this text and wait for AI to built the chart.

```
Create helm charts from these kubernetes definitions file. Use release name in metadata. Replace other hardcoded values into variables and add those variables in values.yaml file.
```

Check the chart to make sure everything is correct.

Lint the chart

```shell
helm lint wordpress-chart
```

```shell
helm template wordpress-chart
```

if everything looks good launch Kubernetes Chart

```shell
helm install wp wordpress-chart -n
```
