# API Reference

Complete API reference for the OpenShift MCP Server.

## Base URL

```
http://localhost:3000/api
```

## Authentication

All API requests require an API token header:

```
Authorization: Bearer YOUR_API_TOKEN
```

## Endpoints

### Health Check

```
GET /health
```

Returns the health status of the MCP Server.

### List Tools

```
GET /tools
```

Returns all available tools.

### Execute Tool

```
POST /tools/{tool-id}/execute
Content-Type: application/json

{
  "parameters": {
    "key": "value"
  }
}
```

Executes a specific tool with given parameters.

### Get Tool Details

```
GET /tools/{tool-id}
```

Returns detailed information about a specific tool.

## Response Format

All responses follow this format:

```json
{
  "status": "success",
  "data": {},
  "error": null,
  "timestamp": "2024-01-01T00:00:00Z"
}
```

## Error Codes

- `200` - Success
- `400` - Bad Request
- `401` - Unauthorized
- `404` - Not Found
- `500` - Server Error

## Rate Limiting

API requests are limited to 1000 requests per hour.

## Examples

### Get All Tools

```bash
curl -X GET http://localhost:3000/api/tools \
  -H "Authorization: Bearer TOKEN"
```

### Execute Tool

```bash
curl -X POST http://localhost:3000/api/tools/health-check/execute \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"parameters":{"cluster":"prod"}}'
```
