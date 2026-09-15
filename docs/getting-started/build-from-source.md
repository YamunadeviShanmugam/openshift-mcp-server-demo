# Build from Source

Part of the **[Quick Start](quickstart.md)** flow — Step 1 builds the MCP server binary.

## Repository

[github.com/openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)  
(fork of [containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server))

Compiled binary name: **`kubernetes-mcp-server`**

## Build (Step 1 of demo)

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server
grep '^go ' go.mod    # install matching Go version
make build
./kubernetes-mcp-server --version
```

Then continue [Quick Start Step 2–3](quickstart.md#step-2--clone-the-demo-repo).

## Wire binary to SRE agent (Step 3)

In `~/.cursor/mcp.json`:

```json
"command": "/ABSOLUTE/PATH/openshift-mcp-server/kubernetes-mcp-server",
"args": [
  "--port", "",
  "--config", "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml"
]
```

Template: [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example)

## Build all platforms

```bash
make build-all-platforms
```

Produces `kubernetes-mcp-server-{os}-{arch}` binaries.

## mcp-inspector (optional smoke test)

```bash
cd openshift-mcp-server
make build
npx @modelcontextprotocol/inspector@latest $(pwd)/kubernetes-mcp-server
```

## Makefile targets

```bash
make help
make test
make lint
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Go version mismatch | Install Go from `go.mod` |
| MCP client won't connect | Use `--port ""` for Cursor stdio |
| Missing toolsets | `--config` must be demo `agents/sre/sre-agent.toml` |

## Pre-built alternative (not the demo flow)

For non-demo use: `npx -y kubernetes-mcp-server@latest` or [GitHub releases](https://github.com/containers/kubernetes-mcp-server/releases).  
The **team demo** always uses a local build from the OpenShift fork.
