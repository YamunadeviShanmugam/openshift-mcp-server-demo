# Troubleshooting

Common issues and solutions for the OpenShift MCP Server.

## Connection Issues

### Cannot Connect to OpenShift API

**Problem**: `Connection refused` error when connecting to OpenShift

**Solution**:
1. Verify the API URL is correct
2. Check if the API server is running: `curl https://api.openshift.local:6443/healthz`
3. Verify network connectivity
4. Check firewall rules

```bash
# Test API connection
curl -k https://api.openshift.local:6443/healthz \
  -H "Authorization: Bearer $TOKEN"
```

### Authentication Failed

**Problem**: `401 Unauthorized` error

**Solution**:
1. Verify your API token is valid
2. Check token expiration: `oc whoami`
3. Refresh token if needed
4. Verify token has required permissions

```bash
# Check current token
oc whoami --show-token
```

## MCP Server Issues

### Server Won't Start

**Problem**: MCP Server fails to start

**Solution**:
1. Check configuration file syntax
2. Verify required environment variables are set
3. Check logs for detailed error messages
4. Ensure port is available

```bash
# Check if port is in use
sudo lsof -i :3000
```

### Server Crashes

**Problem**: MCP Server crashes unexpectedly

**Solution**:
1. Check system resources (CPU, memory)
2. Review error logs
3. Check for out-of-memory issues
4. Verify configuration is valid

```bash
# Check memory usage
free -h

# Check CPU usage
top -b -n 1 | head -20
```

## Tool Execution Issues

### Tool Returns No Results

**Problem**: Tool executes but returns no data

**Solution**:
1. Verify parameters are correct
2. Check RBAC permissions
3. Verify resources exist in cluster
4. Review tool logs

```bash
# Check RBAC permissions
oc auth can-i get pods --as=system:serviceaccount:default:mcp-server
```

### Tool Timeout

**Problem**: Tool execution takes too long or times out

**Solution**:
1. Increase timeout value in configuration
2. Optimize query parameters
3. Check cluster performance
4. Review cluster resources

## Network Issues

### DNS Resolution Fails

**Problem**: Cannot resolve OpenShift API hostname

**Solution**:
1. Check DNS configuration
2. Verify hostname is correct
3. Test DNS resolution: `nslookup api.openshift.local`
4. Check network connectivity

### TLS Certificate Errors

**Problem**: `Certificate verification failed` error

**Solution**:
1. Update CA certificates
2. Disable TLS verification (only for testing): `insecure_skip_tls_verify: true`
3. Add custom CA certificate
4. Check certificate expiration

```bash
# Check certificate expiration
echo | openssl s_client -servername api.openshift.local \
  -connect api.openshift.local:6443 | grep -A 2 "Valid"
```

## Performance Issues

### Slow Response Times

**Problem**: MCP Server responds slowly

**Solution**:
1. Check cluster performance
2. Enable debug logging to identify bottlenecks
3. Review resource limits
4. Optimize queries

### High CPU Usage

**Problem**: MCP Server consuming excessive CPU

**Solution**:
1. Check running tools
2. Identify long-running queries
3. Adjust log level (reduce debug logging)
4. Check for infinite loops in custom tools

## Getting Help

### Collect Debug Information

```bash
# Enable debug logging
export MCP_DEBUG=true

# Collect logs
logs=$(oc logs -l app=mcp-server -n default --tail=1000)

# Check cluster events
oc get events -n default --sort-by='.lastTimestamp'
```

### Contact Support

Include these details:
- MCP Server version
- OpenShift version
- Detailed error messages
- Steps to reproduce
- Debug logs
