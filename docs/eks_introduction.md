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

