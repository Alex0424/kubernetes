# Kubectl

## 📋 System Requirements

- 🐧 Linux-based OS

## 🔧 Install `kubectl` binary

[Based on the official kubectl documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

### 📥 Download the Latest Release
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

### ✅ Validate the Binary
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

# If valid, the output should be: "kubectl: OK"
# If not, it will say: "kubectl: FAILED"
```

### 🛠 Install kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```