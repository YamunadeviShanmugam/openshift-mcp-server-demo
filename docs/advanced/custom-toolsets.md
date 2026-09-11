# Custom Toolsets

Learn how to create and manage custom toolsets for specialized OpenShift operations.

## Overview

Custom toolsets allow you to extend the MCP Server with domain-specific tools and workflows.

## Creating a Custom Toolset

### Step 1: Define Your Tools

Create a new tool definition:

```yaml
tools:
  - name: custom-health-check
    description: Custom cluster health verification
    parameters:
      cluster: string
      checks: array
```

### Step 2: Implement Logic

Implement the toolset logic in your MCP Server.

### Step 3: Register with MCP

Register your toolset with the MCP Server configuration.

## Best Practices

- Keep toolsets focused on specific domains
- Document all parameters clearly
- Include error handling
- Test thoroughly before production use

## Examples

### Example 1: Custom Compliance Check

```yaml
tools:
  - name: custom-compliance
    description: Run custom compliance checks
    parameters:
      policy: string
```

### Example 2: Custom Health Metrics

```yaml
tools:
  - name: custom-metrics
    description: Collect custom metrics
```

## Troubleshooting

If custom toolsets aren't working:

1. Verify the toolset is registered
2. Check the MCP Server logs
3. Validate YAML configuration
4. Test with sample parameters
