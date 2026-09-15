# Build from Source (SRE Agent)

Part of the **[SRE Agent Quick Start](quickstart.md)** — Step 1 builds the MCP server binary.

For generic build instructions, see **[MCP Server — Build from Source](../mcp-server/build-from-source.md)**.

## Build (Step 1 of demo)

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server
grep '^go ' go.mod
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
  "--kubeconfig", "/ABSOLUTE/PATH/to/your/kubeconfig",
  "--config", "/ABSOLUTE/PATH/openshift-mcp-server-demo/agents/sre/sre-agent.toml",
  "--log-file", "/tmp/kubernetes-mcp-server.log"
]
```

Template: [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example)

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Go version mismatch | Install Go from `go.mod` |
| MCP client won't connect | Use `--port ""`; add `--kubeconfig` absolute path |
| Help text then disconnect | Check `/tmp/kubernetes-mcp-server.log` |
| Missing toolsets | `--config` must point to `agents/sre/sre-agent.toml` |

## Next steps

- [Quick Start](quickstart.md)
- [MCP Server Build](../mcp-server/build-from-source.md)
