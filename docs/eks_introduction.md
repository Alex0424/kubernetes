# Amazon Elastic Kubernetes Service

There are ways to create and handle Kubernetes by using Kops, Kubeadm, etc.

Problems with default k8s setup:

  - Its complex to manage a Kubernetes cluster.
  - This requires system administration.

Solutions with EKS:

  - Amazon EKS is an self-managed service.
  - You just have to pass simple information about your cluster details.
  - You can change capacity and scale it whenever you want.
  - Automatically deploys Kubernetes control plane

## Automation with EKS

Use published Terraform modules

- [AWS VPC](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest)
- [AWS EKS](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest)


## Demo

Clone a [predefined terraform code for EKS](https://github.com/hkhcoder/vprofile-project/tree/terraform-eks)

```shell
git clone https://github.com/hkhcoder/vprofile-project.git
```

```shell
git switch terraform-eks
```

## Setup and run Terraform

1. Install Terraform

2. Install Kubectl

3. IAM
  - Create IAM user
  - Premission: `administraitor access`
  - Security credentials: Create and Download the `access key`
  - Type `aws configure` in terminal and paste the credentials

4. Go to the repository you recently cloned(repository `vprofile-project`, branch `terraform-eks`)

5. Run the following commands to create the EKS Cluster

```shell
ls
# eks-cluster.tf  main.tf  outputs.tf  terraform.tf  variables.tf  vpc.tf

terraform init

terraform plan

terraform apply
```

We should have 3 public and 3 private VPC's.

In total we should have 6 subnets.

Create or update a kubeconfig file for your cluster. Replace region-code with the AWS Region that your cluster is in and replace my-cluster with the name of your cluster.
```
aws eks update-kubeconfig --region region-code --name my-cluster
```

Test kubeconfig
```
kubectl get nodes
```