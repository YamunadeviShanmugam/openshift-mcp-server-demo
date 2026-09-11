# Frequently Asked Questions

Common questions about the OpenShift MCP Server.

## General Questions

### What is the OpenShift MCP Server?

The OpenShift MCP Server is a tool that enables seamless integration between AI assistants and OpenShift clusters using the Model Context Protocol (MCP).

### How does it work?

The MCP Server acts as a bridge between your AI assistant and OpenShift, allowing the AI to:
- Query cluster information
- Execute commands
- Monitor resources
- Manage applications

### Who should use it?

- Site Reliability Engineers (SREs)
- DevOps engineers
- Cluster administrators
- Platform teams

## Installation & Setup

### What are the system requirements?

- Linux or macOS
- Python 3.8+
- OpenShift CLI (oc)
- Network access to OpenShift API

### How do I install the MCP Server?

```bash
pip install mcp-server
mcp-server init
```

### Do I need special permissions?

Yes, your user account needs permissions for the resources you want to access. Typically you need:
- View pods, services, deployments
- View logs and events
- (Optional) Create/modify resources

### How do I configure it?

Create a `config.yaml` file and set environment variables:

```bash
export OPENSHIFT_TOKEN="your-token"
export OPENSHIFT_API_URL="https://api.openshift.local:6443"
```

## Operations

### Can I use it in production?

Yes, but follow these practices:
- Use dedicated service accounts
- Enable audit logging
- Implement network policies
- Regular security reviews

### How do I manage multiple clusters?

Configure multiple cluster contexts:

```yaml
clusters:
  - name: production
    api_url: https://api.prod.openshift.local:6443
  - name: staging
    api_url: https://api.staging.openshift.local:6443
```

### What if I have network restrictions?

The MCP Server must be able to reach the OpenShift API. If behind a firewall:
- Configure proxy settings
- Use VPN or bastion hosts
- Whitelist API endpoints

## Security

### Is my data secure?

Data is encrypted in transit (TLS) and at rest. Sensitive data is stored securely:
- Tokens stored in secure memory
- Credentials never logged
- Audit trails maintained

### How do I rotate credentials?

```bash
# Get new token
oc create token default

# Update configuration
export OPENSHIFT_TOKEN="new-token"

# Restart MCP Server
mcp-server restart
```

### What about compliance?

The MCP Server supports:
- SOC2 compliance
- HIPAA requirements
- PCI-DSS standards
- Custom audit policies

## Troubleshooting

### The server won't start

Check the logs:

```bash
mcp-server logs
```

Common issues:
- Invalid configuration
- Port already in use
- Missing dependencies

### I'm getting authentication errors

Verify your token:

```bash
oc auth can-i get pods
```

### Performance is slow

- Check cluster load
- Review query complexity
- Enable caching if available

## Advanced Questions

### Can I create custom tools?

Yes! See the [Custom Toolsets](../advanced/custom-toolsets.md) guide for details.

### How do I integrate with CI/CD?

The MCP Server can be integrated with:
- GitHub Actions
- GitLab CI
- Jenkins
- ArgoCD

### What's the API structure?

See the [API Reference](../advanced/api-reference.md) for complete documentation.

### How do I contribute?

Visit the GitHub repository and check the contributing guidelines.

## More Help

- **Documentation**: See the [SRE Workflows](../workflows/) section
- **Examples**: Check the examples directory
- **Community**: Join our Slack/Discord for support
- **Issues**: Report bugs on GitHub
