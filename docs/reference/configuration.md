# Configuration Reference

Complete reference guide for configuring the OpenShift MCP Server.

## Configuration File

The main configuration file is `config.yaml`:

```yaml
server:
  host: 0.0.0.0
  port: 3000
  debug: false

openshift:
  api_url: https://api.openshift.local:6443
  api_token: ${OPENSHIFT_TOKEN}
  insecure_skip_tls_verify: false

logging:
  level: info
  format: json
  output: stdout

tools:
  enabled:
    - health_check
    - cluster_info
    - pod_management
```

## Server Configuration

### Host and Port

```yaml
server:
  host: 0.0.0.0
  port: 3000
```

- `host`: Bind address (0.0.0.0 for all interfaces)
- `port`: Port number (default: 3000)

### Debug Mode

```yaml
server:
  debug: true
```

Enable debug logging for troubleshooting.

## OpenShift Configuration

### API Connection

```yaml
openshift:
  api_url: https://api.openshift.local:6443
  api_token: ${OPENSHIFT_TOKEN}
  insecure_skip_tls_verify: false
```

- `api_url`: OpenShift API endpoint
- `api_token`: API authentication token
- `insecure_skip_tls_verify`: Skip SSL verification (not recommended for production)

## Logging Configuration

### Log Levels

```yaml
logging:
  level: info
```

Available levels: `debug`, `info`, `warn`, `error`, `fatal`

### Log Format

```yaml
logging:
  format: json
```

Available formats: `json`, `text`

### Log Output

```yaml
logging:
  output: stdout
```

Can be `stdout`, `stderr`, or file path.

## Tools Configuration

### Enable/Disable Tools

```yaml
tools:
  enabled:
    - health_check
    - cluster_info
```

List the tools you want to enable.

## Environment Variables

You can override configuration values with environment variables:

```bash
export OPENSHIFT_TOKEN="your-token"
export MCP_PORT=3001
export MCP_DEBUG=true
```

## Configuration Validation

Validate your configuration:

```bash
mcp-server validate --config config.yaml
```

## Example Configuration

```yaml
server:
  host: 0.0.0.0
  port: 3000
  debug: false

openshift:
  api_url: https://api.production.openshift.local:6443
  api_token: ${OPENSHIFT_TOKEN}
  insecure_skip_tls_verify: false

logging:
  level: info
  format: json
  output: /var/log/mcp-server.log

tools:
  enabled:
    - health_check
    - cluster_info
    - pod_management
    - networking
    - security
```
