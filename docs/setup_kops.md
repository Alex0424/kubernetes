# Setup Kubernetes with Kops

## 📋 Prerequisites

- 🐧 Linux-based OS
- [kubectl](./setup_kubectl.md) installed
- Domain from Namecheap
- SubDomain from AWS `Route 53` with 4 zone NS servers

## Launch EC2 Instance

- Name: `kops`
- AMI (Amazon Machine Image): `Ubuntu Server 24.04 LTS`
- Instance type: `t2.micro`
- Key pair: `name: 'kopskey'`, `Key pair type: 'RSA'`, `Private key file format: '.pem'`
- Security group: `name: 'kops-sg'`, `Inbound rules: {type: ssh, source type: my ip}`

## Create IAM User

- Name: `kopsadmin`
- Permissions options: `Attach policies directly -> AdministratorAccess`

### Create Access key

- Command Line Interface (CLI)
- Copy Access and Secret Access key's

## SSH to VM and run the following commands

```bash
ssh -i kopskey.pem ubuntu@<your_vm_IP>
apt update
snap install aws-cli --classic

aws configure

AWS Access Key ID [None]: *********************
AWS Secret Key [None]: **************************
Default region name [None]: us-east-1
Default output format [None]: json
```

Generate SSH keys

```bash
ssh-keygen
ls ~/.ssh
# output authorized_keys id_ed25519 id_ed25519.pub
```

[Install Kops](https://kops.sigs.k8s.io/getting_started/install/#linux)
```bash
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops

sudo mv kops /usr/local/bin/kops
```

Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# check if installed
kubectl version --client
```

## Create a bucket with Amazon S3

`Create bucket`

- Bucket type: `General purpose`
- Bucket name: `kopsstate956` <= This needs to be uniqe

## Create the DNS Zone with Amazon `Route 53`

`Create hosted zone` at DNS management tab

domain name: `kubernetes_test.alexanderlindholm.net`

more cooming soon