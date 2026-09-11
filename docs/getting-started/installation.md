# Installation Guide

Detailed installation instructions for all platforms and methods.

## Installation Methods

### Method 1: npm (Recommended) ⭐

**Best for**: Most users, automatic updates, cross-platform

#### Requirements
- Node.js v14+
- npm v6+

#### Installation

```bash
# One-time installation (optional)
npm install -g kubernetes-mcp-server

# Or use npx (recommended - always runs latest)
npx -y kubernetes-mcp-server@latest --read-only
```

#### Verification
```bash
npx kubernetes-mcp-server --help
```

**Advantages:**
- ✅ Automatic updates via `npx`
- ✅ No local installation needed
- ✅ Works on macOS, Linux, Windows
- ✅ Easiest to configure in clients

**Disadvantages:**
- ❌ Requires Node.js/npm
- ❌ First run downloads ~50MB

### Method 2: uvx (Python) 🐍

**Best for**: Python-focused environments, Python venv users

#### Requirements
- Python 3.8+
- uv package manager

#### Installation

```bash
# Install uv first
pip install uv

# Then run
uvx kubernetes-mcp-server --read-only
```

#### Verification
```bash
uvx kubernetes-mcp-server --help
```

**Advantages:**
- ✅ Python ecosystem
- ✅ Similar to npx but for Python
- ✅ Isolated environment

**Disadvantages:**
- ❌ Requires Python installation
- ❌ Requires uv package manager

### Method 3: Native Binary 🔧

**Best for**: Minimal dependencies, production deployment, static binaries

#### Requirements
- Linux, macOS, or Windows OS
- x86_64 or ARM64 architecture

#### Installation

```bash
# Download latest release
RELEASE_URL="https://github.com/containers/kubernetes-mcp-server/releases/download"
LATEST_VERSION=$(curl -sL https://api.github.com/repos/containers/kubernetes-mcp-server/releases/latest | jq -r '.tag_name')

# For Linux x86_64
wget "${RELEASE_URL}/${LATEST_VERSION}/kubernetes-mcp-server-linux-x86_64"
chmod +x kubernetes-mcp-server-linux-x86_64

# For macOS (Intel)
wget "${RELEASE_URL}/${LATEST_VERSION}/kubernetes-mcp-server-darwin-x86_64"
chmod +x kubernetes-mcp-server-darwin-x86_64

# For macOS (Apple Silicon)
wget "${RELEASE_URL}/${LATEST_VERSION}/kubernetes-mcp-server-darwin-arm64"
chmod +x kubernetes-mcp-server-darwin-arm64

# For Windows
# Download from: https://github.com/containers/kubernetes-mcp-server/releases
# Extract and add to PATH
```

#### Verification
```bash
./kubernetes-mcp-server-linux-x86_64 --help
```

#### Optional: Add to PATH

```bash
# Move to /usr/local/bin for easy access
sudo mv kubernetes-mcp-server-linux-x86_64 /usr/local/bin/kubernetes-mcp-server

# Then use directly
kubernetes-mcp-server --read-only
```

**Advantages:**
- ✅ No dependencies
- ✅ Fast startup
- ✅ Available for all major OS
- ✅ Easy to version control
- ✅ Great for container deployments

**Disadvantages:**
- ❌ Manual updates needed
- ❌ Binary size ~50MB

### Method 4: Docker Container 🐳

**Best for**: Isolated environments, CI/CD, Kubernetes deployments

#### Requirements
- Docker installed and running
- kubeconfig volume mount

#### Installation

```bash
# Run directly (no local installation)
docker run -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  ghcr.io/containers/kubernetes-mcp-server:latest \
  --port 8080 --read-only
```

#### For HTTP Mode (Recommended for Docker)

```bash
docker run \
  --name kubernetes-mcp-server \
  -v ~/.kube/config:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  -d \
  ghcr.io/containers/kubernetes-mcp-server:latest \
  --port 8080 --read-only

# Access at http://localhost:8080/mcp
```

#### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubernetes-mcp-server
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kubernetes-mcp-server
  template:
    metadata:
      labels:
        app: kubernetes-mcp-server
    spec:
      serviceAccountName: mcp-viewer
      containers:
      - name: kubernetes-mcp-server
        image: ghcr.io/containers/kubernetes-mcp-server:latest
        args:
          - --port
          - "8080"
          - --read-only
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "64Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "500m"
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: mcp-viewer
  namespace: default
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: mcp-viewer
rules:
- apiGroups: [""]
  resources: ["pods", "namespaces", "events", "nodes"]
  verbs: ["get", "list", "watch"]
- apiGroups: ["apps"]
  resources: ["deployments", "statefulsets", "daemonsets"]
  verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: mcp-viewer-binding
subjects:
- kind: ServiceAccount
  name: mcp-viewer
  namespace: default
roleRef:
  kind: ClusterRole
  name: mcp-viewer
  apiGroup: rbac.authorization.k8s.io
```

**Advantages:**
- ✅ Complete isolation
- ✅ Consistent across environments
- ✅ Easy Kubernetes deployment
- ✅ Network-based access

**Disadvantages:**
- ❌ Docker overhead
- ❌ Port exposure needed
- ❌ Network latency vs local

## Post-Installation

### 1. Verify kubeconfig Access

```bash
kubectl get namespaces
```

If this fails, your kubeconfig isn't accessible. Fix it before continuing.

### 2. Test MCP Server

```bash
npx kubernetes-mcp-server --help
```

### 3. Configure Your Client

See [Configuration](configuration.md) for your specific client.

### 4. First Query

```
"List all pods in the default namespace"
```

## Updating

### For npm Installation

```bash
# npx always uses latest
npx -y kubernetes-mcp-server@latest --read-only

# If installed globally, update
npm update -g kubernetes-mcp-server
```

### For Binary Installation

```bash
# Download latest from releases
# Backup old binary
mv kubernetes-mcp-server kubernetes-mcp-server.backup

# Download and test new version
```

### For Docker

```bash
# Pull latest image
docker pull ghcr.io/containers/kubernetes-mcp-server:latest

# Recreate container
docker rm kubernetes-mcp-server
docker run ... # Same docker run command as above
```

## Comparison Table

| Method | Ease | Speed | Dependencies | Updates | Best For |
|--------|------|-------|--------------|---------|----------|
| npm | ⭐⭐⭐ | ⭐⭐ | Node.js | Auto | Most Users |
| uvx | ⭐⭐ | ⭐⭐ | Python | Auto | Python Users |
| Binary | ⭐⭐⭐ | ⭐⭐⭐ | None | Manual | Production |
| Docker | ⭐⭐ | ⭐ | Docker | Manual | CI/CD |

## Troubleshooting

### "Command not found"

Install the required runtime:
- **npm**: https://nodejs.org/
- **uvx**: https://astral.sh/uv/
- **Docker**: https://docker.com/

### "Cannot connect to cluster"

```bash
# Test kubeconfig
kubectl get namespaces

# If it fails, kubeconfig isn't accessible
# Check path, permissions, and validity
```

### "Permission denied"

```bash
# Check you can access cluster
kubectl auth can-i get pods --all-namespaces
```

## Next Steps

- [Configuration Guide](configuration.md)
- [Quick Start](quickstart.md)
- [SRE Workflows](../workflows/cluster-health.md)

---

**Installation complete?** → [Configure Your Client](configuration.md)
