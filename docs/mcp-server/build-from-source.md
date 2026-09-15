# Build from Source

Build **`kubernetes-mcp-server`** from the OpenShift fork.

## Repository

- **OpenShift fork:** [github.com/openshift/openshift-mcp-server](https://github.com/openshift/openshift-mcp-server)
- **Upstream:** [github.com/containers/kubernetes-mcp-server](https://github.com/containers/kubernetes-mcp-server)

## Build

```bash
git clone https://github.com/openshift/openshift-mcp-server.git
cd openshift-mcp-server
grep '^go ' go.mod    # install matching Go version
make build
./kubernetes-mcp-server --version
```

Output: `./kubernetes-mcp-server` in the repo root.

## Smoke test (mcp-inspector)

```bash
make build
npx @modelcontextprotocol/inspector@latest $(pwd)/kubernetes-mcp-server
```

## All platforms

```bash
make build-all-platforms
```

## Wire to Cursor

See [Cursor Integration](cursor-integration.md) — use the built binary path in `command`.

## Makefile targets

```bash
make help
make test
make lint
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Go version mismatch | Install Go version from `go.mod` |
| MCP client won't connect | Use `--port ""` for Cursor stdio |
| Missing toolsets | Pass `--toolsets core,openshift,...` or use SRE agent TOML |

## Pre-built alternative

```bash
npx -y kubernetes-mcp-server@latest --help
```

See [Installation](installation.md) for npm, binary releases, and Docker.
