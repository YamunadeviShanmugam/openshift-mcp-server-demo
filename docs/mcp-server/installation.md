# Installation

Alternative ways to install **kubernetes-mcp-server** without building from source.

For the **team SRE demo**, always build from source — see the
**[SRE Agent Quick Start](../getting-started/quickstart.md)**.

For generic MCP server use, pick one method below.

## npm / npx

```bash
npx -y kubernetes-mcp-server@latest --help
```

Cursor:

```json
{
  "mcpServers": {
    "kubernetes-mcp-server": {
      "command": "npx",
      "args": [
        "-y",
        "kubernetes-mcp-server@latest",
        "--port",
        "",
        "--kubeconfig",
        "/ABSOLUTE/PATH/to/kubeconfig",
        "--log-file",
        "/tmp/kubernetes-mcp-server.log"
      ]
    }
  }
}
```

## Pre-built binary

Download from [GitHub releases](https://github.com/containers/kubernetes-mcp-server/releases):

```bash
chmod +x kubernetes-mcp-server
./kubernetes-mcp-server --version
```

## Python (uvx)

```bash
uvx kubernetes-mcp-server@latest --help
```

## Docker (HTTP mode)

```bash
docker run -v /path/to/kubeconfig:/kubeconfig:ro \
  -e KUBECONFIG=/kubeconfig \
  -p 8080:8080 \
  quay.io/containers/kubernetes_mcp_server:latest \
  --port 8080
```

Cursor stdio mode uses a **local binary**, not Docker.

## Build from source (OpenShift fork)

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server && make build
```

Details: [Build from Source](build-from-source.md)

## Verify installation

```bash
oc get nodes
./kubernetes-mcp-server --version
./kubernetes-mcp-server --help | grep toolsets
```

## Next steps

- [Cursor Integration](cursor-integration.md)
- [Configuration](configuration.md)
