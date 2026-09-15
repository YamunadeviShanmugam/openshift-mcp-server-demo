# Configuration Guide

Configuration docs are split by use case:

| Tab | Page |
|-----|------|
| **MCP Server** | [Configuration](../mcp-server/configuration.md) — CLI flags, TOML, kubeconfig |
| **SRE Agent** | [`agents/sre/sre-agent.toml`](../../agents/sre/sre-agent.toml) + `conf.d/` drop-ins |

## SRE agent config files

```text
agents/sre/
├── sre-agent.toml              # toolsets, denied_resources, disabled_tools
└── conf.d/
    ├── 10-server-instructions.toml
    ├── 20-prompts.toml
    └── 99-local.toml.example   # optional personal overrides
```

Set kubeconfig in **`~/.cursor/mcp.json`** via `--kubeconfig` (absolute path), not `~` in TOML.

Template: [`agents/sre/mcp.json.example`](../../agents/sre/mcp.json.example)

## Reference

- [MCP Server Configuration](../mcp-server/configuration.md)
- [Configuration Reference](../reference/configuration.md)
