# Setup Kubernetes with Kops

## 📋 Prerequisites

- 🐧 Linux-based OS
- [kubectl](./setup_kubectl.md) installed
- Registered domain (e.g., from Namecheap)
- A subdomain configured via AWS Route 53 with 4 NS (Name Server) records

## ☁️ Launch EC2 Instance

- Name: `kops`
- AMI (Amazon Machine Image): `Ubuntu Server 24.04 LTS` (but its recomended to use 22.04 as its stable)
- Instance type: `t2.micro`
- Key pair: `Name: 'kopskey'`, `Key pair type: 'RSA'`, `Private key file format: '.pem'`
- Security group: `Name: 'kops-sg'`, `Inbound rules: SSH from My IP}`

## 👤 Create IAM User

- **Name**: `kopsadmin`
- **Permissions**: `Attach policies directly -> AdministratorAccess`

### 🔐 Create Access key

- Choose **Command Line Interface (CLI)**
- Copy the **Access Key ID** and **Secret Access Key**

## 🔑 Connect to EC2 Instance

```bash
chmod 600 kopskey.pem
ssh -i kopskey.pem ubuntu@@<your_ec2_public_ip>
```

Update and install AWS CLI:
```bash
sudo apt update
sudo snap install aws-cli --classic
```

Configure AWS CLI:
```bash
aws configure
```
```
AWS Access Key ID [None]: *********************
AWS Secret Key [None]: **************************
Default region name [None]: us-east-1
Default output format [None]: json
```

### 🔐 Generate SSH keys

```bash
ssh-keygen
ls ~/.ssh
# output authorized_keys id_ed25519 id_ed25519.pub
```

### 🛠️ [Install Kops](https://kops.sigs.k8s.io/getting_started/install/#linux)
```bash
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops

sudo mv kops /usr/local/bin/kops
```

### 🧰 Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# check if installed
kubectl version --client
```

## 🪣 Create a bucket with Amazon S3

In the AWS Console or CLI:

- Bucket type: `General purpose`
- Bucket name: `kopsstate1877` <= This needs to be uniqe

## 🌐 Create the DNS Zone with Amazon `Route 53`

Using example domain: alexanderlindholm.net

1. Go to Route 53 > Hosted Zones

2. Create a Public Hosted Zone:

domain name: `kubevpro.alexanderlindholm.net`

Type: `Public hosted zone`

### 🧾 Copy NS Records

1. Click into your new hosted zone

2. Under Records, copy the 4 NS records (e.g., ns-xxx.awsdns-xx.com)

## 🔗 Update DNS Records in Namecheap

In your domain provider’s dashboard, add:

| Type      | Host     | Value                | TTL       |
|-----------|----------|----------------------|-----------|
| NS Record | kubevpro | ns-xxx.awsdns-xx.com | Automatic |
| NS Record | kubevpro | ns-xxxx.awsdns-xx.org| Automatic |
| NS Record | kubevpro | ns-xxx.awsdns-xx.net | Automatic |
| NS Record | kubevpro | ns-xxxx.awsdns-xx.uk | Automatic |

replace Value column with the 4 NS Type Records from AWS Route 53

## 🚀 Create Your Kubernetes Cluster

```
kops create cluster \
  --name=kubevpro.alexanderlindholm.net \
  --state=s3://kopsstate1877 \
  --zones=us-east-1a,us-east-1b \
  --node-count=2 \
  --node-size=t3.small \
  --control-plane-size=t3.medium \
  --dns-zone=kubevpro.alexanderlindholm.net \
  --node-volume-size=12 \
  --control-plane-volume-size=12 \
  --ssh-public-key ~/.ssh/id_ed25519.pub
```

Apply the configuration:
```
kops update cluster --name=kubevpro.alexanderlindholm.net --state=s3://kopsstate1877 --yes --admin
```

### ✅ Validate the Cluster

```
kops validate cluster --name=kubevpro.alexanderlindholm.net --state=s3://kopsstate1877
```

### 🔍 Useful CommandsKube

Get kubeconfig:
```
cat .kube/config
# config is created by master node
```

List nodes:
```
kubectl get nodes
```

## 🗑️ Delete the Cluster
```
kops delete cluster --name=kubevpro.alexanderlindholm.net --state=s3://kopsstate1877 --yes
```

Then power off or delete the EC2 instance (Kops)

## ⚠️ Troubleshoot

List instance groups:
```
kops get ig --name=kubevpro.alexanderlindholm.net --state=s3://kopsstate1877

# Output Example:
# NAME				ROLE		MACHINETYPE	MIN	MAX	ZONES
# control-plane-us-east-1a	ControlPlane	t3.medium	1	1	us-east-1a
# nodes-us-east-1a		Node		t3.small	1	1	us-east-1a
# nodes-us-east-1b		Node		t3.small	1	1	us-east-1b
```

Edit an instance group (e.g., if an AMI is invalid):
```
kops edit ig nodes-us-east-1a --name=kubevpro.alexanderlindholm.net --state=s3://kopsstate1877
```

🌐 Verify DNS
```
dig +short api.kubevpro.alexanderlindholm.net
# output example: 
# 5.34.68.27
```

You should see this IP listed in Route 53 > Hosted Zones under A or CNAME record for your subdomain.