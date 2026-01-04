# KIND (Kubernetes IN Docker)

**KIND** is an open‑source tool for running local Kubernetes clusters inside Docker containers. It bootstraps clusters using `kubeadm`, creating lightweight, portable environments that are **CNCF conformant** and closely resemble production Kubernetes.

Unlike VM‑based solutions (e.g., Minikube), KIND leverages Docker for nodes, making it **fast, resource‑efficient, and cost‑free**—ideal for development and testing.

---

## 🚀 Best Use Cases

- **Local development & testing**  
  Test manifests, Helm charts, and operators before deploying to production.

- **CI/CD pipelines**  
  Spin up ephemeral clusters in GitHub Actions, Jenkins, etc. (e.g., via `helm/kind-action`).

- **Multi‑node scenarios**  
  Simulate HA control planes, worker nodes, and networking (CNI included).

- **Kubernetes contributor workflows**  
  Build/test from source code, integrate with `kubetest`.

- **Onboarding & demos**  
  Quick clusters for tutorials/workshops (startup < 1 min after images cached).

⚠️ **Not recommended for:**  
Long‑running production workloads or environments requiring cloud provider integrations (use AKS/EKS instead).

---

## 🖥️ Prerequisites

- Docker (Engine or Desktop) installed and running  
- `kubectl` installed  
- Supported OS: Linux, macOS, Windows  
- Minimum ~2 GB RAM  

---

## 📦 Installation

### 1. Install Docker
- **Windows/macOS:** Install Docker Desktop  
- **Linux:**  
  ```bash
  sudo apt install docker.io
  ```

### 2. Install kubectl
- **Linux/macOS:**  
  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  sudo install kubectl /usr/local/bin/kubectl
  ```

- **Windows (PowerShell):**  
  ```powershell
  curl.exe -LO "https://dl.k8s.io/release/$(curl.exe -L -s https://dl.k8s.io/release/stable.txt)/bin/windows/amd64/kubectl.exe"
  ```
  Verify installation:
  ```powershell
  kubectl version --client
  ```

### 3. Install KIND
- **Linux/macOS:**  
  ```bash
  curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.24.0/kind-linux-amd64
  chmod +x ./kind
  sudo mv ./kind /usr/local/bin/kind
  ```

- **Windows (PowerShell):**  
  ```powershell
  curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.24.0/kind-windows-amd64
  Move-Item kind-windows-amd64.exe C:\Windows\System32\kind.exe
  ```
  Verify installation:
  ```powershell
  kind version
  ```

---

## ⚡ Running a Cluster

### Single‑Node (Quick Start)
```bash
kind create cluster
kubectl cluster-info
```
- Creates 1 control‑plane node with CNI and StorageClass.  
- Default cluster name: `kind`.

### Multi‑Node (3 Nodes Example)
Create a config file `kind-3node.yaml`:
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

Run:
```bash
kind create cluster --name dev-cluster --config kind-3node.yaml
kubectl get nodes
```
- Verifies 3 nodes are ready.  
- Kubeconfig auto‑exported to `~/.kube/config`.

---

## 🔍 Verification & Cleanup

- Check nodes:
  ```bash
  kubectl get nodes -o wide
  kind get clusters
  ```

- Delete cluster:
  ```bash
  kind delete cluster --name dev-cluster
  ```
  This removes the Docker containers and networks. If it fails, open Docker Desktop, go to the Containers tab, and manually delete all related containers.
---
