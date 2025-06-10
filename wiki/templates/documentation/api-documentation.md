# API Documentation Template

**Target Audience**: Frontend Developers, External Integrators  
**Last Updated**: 2025-06-10 by System  
**Template Type**: REST/GraphQL API Documentation

# [API_NAME] API Documentation

## Overview
**Base URL**: `https://api.example.com/v1`  
**API Version**: v1.0  
**Protocol**: REST/GraphQL  
**Authentication**: Bearer Token  

### Key Features
- RESTful design principles
- JSON request/response format
- Comprehensive error handling
- Rate limiting and throttling
- Swagger/OpenAPI specification

## Authentication

### Bearer Token Authentication
```bash
# Obtain access token
curl -X POST https://api.example.com/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your_username",
    "password": "your_password"
  }'

# Response
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Using the Token
```bash
curl -H "Authorization: Bearer <access_token>" \
  https://api.example.com/v1/users
```

### API Key Authentication (Alternative)
```bash
curl -H "X-API-Key: your_api_key" \
  https://api.example.com/v1/users
```

## Base Information

### Response Format
All API responses follow this structure:
```json
{
  "success": true,
  "data": {},
  "message": "Success message",
  "timestamp": "2025-06-10T12:00:00Z",
  "request_id": "uuid-here"
}
```

### Error Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input provided",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  },
  "timestamp": "2025-06-10T12:00:00Z",
  "request_id": "uuid-here"
}
```

### Rate Limiting
- **Rate Limit**: 1000 requests per hour
- **Headers**: 
  - `X-RateLimit-Limit`: Request limit
  - `X-RateLimit-Remaining`: Requests remaining
  - `X-RateLimit-Reset`: Reset timestamp

## Endpoints

### Users API

#### List Users
```http
GET /v1/users
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page` | integer | No | Page number (default: 1) |
| `limit` | integer | No | Items per page (default: 20, max: 100) |
| `search` | string | No | Search term for filtering |
| `role` | string | No | Filter by user role |

**Request Example:**
```bash
curl -H "Authorization: Bearer <token>" \
  "https://api.example.com/v1/users?page=1&limit=20&role=admin"
```

**Response Example:**
```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": "123e4567-e89b-12d3-a456-426614174000",
        "username": "john_doe",
        "email": "john@example.com",
        "role": "admin",
        "created_at": "2025-01-01T00:00:00Z",
        "updated_at": "2025-06-10T12:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

#### Get User
```http
GET /v1/users/{id}
```

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | User UUID |

**Response Example:**
```json
{
  "success": true,
  "data": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "username": "john_doe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "admin",
    "profile": {
      "avatar_url": "https://example.com/avatar.jpg",
      "bio": "Software engineer"
    },
    "created_at": "2025-01-01T00:00:00Z",
    "updated_at": "2025-06-10T12:00:00Z"
  }
}
```

#### Create User
```http
POST /v1/users
```

**Request Body:**
```json
{
  "username": "jane_doe",
  "email": "jane@example.com",
  "password": "SecurePassword123!",
  "first_name": "Jane",
  "last_name": "Doe",
  "role": "user"
}
```

**Request Example:**
```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "jane_doe",
    "email": "jane@example.com",
    "password": "SecurePassword123!",
    "first_name": "Jane",
    "last_name": "Doe",
    "role": "user"
  }'
```

**Response Example:**
```json
{
  "success": true,
  "data": {
    "id": "987fcdeb-51a2-43d1-9f45-123456789abc",
    "username": "jane_doe",
    "email": "jane@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "role": "user",
    "created_at": "2025-06-10T12:00:00Z"
  },
  "message": "User created successfully"
}
```

#### Update User
```http
PUT /v1/users/{id}
```

**Request Body:**
```json
{
  "first_name": "Jane",
  "last_name": "Smith",
  "email": "jane.smith@example.com"
}
```

#### Delete User
```http
DELETE /v1/users/{id}
```

**Response Example:**
```json
{
  "success": true,
  "message": "User deleted successfully"
}
```

### Projects API

#### List Projects
```http
GET /v1/projects
```

#### Get Project
```http
GET /v1/projects/{id}
```

#### Create Project
```http
POST /v1/projects
```

#### Update Project
```http
PUT /v1/projects/{id}
```

#### Delete Project
```http
DELETE /v1/projects/{id}
```

## Error Codes

### HTTP Status Codes
| Code | Description | When Used |
|------|-------------|-----------|
| 200 | OK | Successful GET, PUT requests |
| 201 | Created | Successful POST requests |
| 204 | No Content | Successful DELETE requests |
| 400 | Bad Request | Invalid request format |
| 401 | Unauthorized | Missing or invalid authentication |
| 403 | Forbidden | Valid auth but insufficient permissions |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Resource conflict (duplicate email) |
| 422 | Unprocessable Entity | Validation errors |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server-side error |

### Application Error Codes
| Code | Description | Resolution |
|------|-------------|------------|
| `VALIDATION_ERROR` | Input validation failed | Check request format |
| `AUTHENTICATION_FAILED` | Invalid credentials | Verify username/password |
| `TOKEN_EXPIRED` | Access token expired | Refresh token |
| `INSUFFICIENT_PERMISSIONS` | User lacks permissions | Contact administrator |
| `RESOURCE_NOT_FOUND` | Requested resource missing | Verify resource ID |
| `DUPLICATE_RESOURCE` | Resource already exists | Use different identifier |
| `RATE_LIMIT_EXCEEDED` | Too many requests | Wait before retrying |

## SDK Examples

### JavaScript/TypeScript
```javascript
// Install: npm install @company/api-client
import { ApiClient } from '@company/api-client';

const client = new ApiClient({
  baseURL: 'https://api.example.com/v1',
  apiKey: 'your-api-key'
});

// Get users
const users = await client.users.list({
  page: 1,
  limit: 20
});

// Create user
const newUser = await client.users.create({
  username: 'john_doe',
  email: 'john@example.com',
  password: 'SecurePassword123!'
});
```

### Python
```python
# Install: pip install company-api-client
from company_api import ApiClient

client = ApiClient(
    base_url='https://api.example.com/v1',
    api_key='your-api-key'
)

# Get users
users = client.users.list(page=1, limit=20)

# Create user
new_user = client.users.create({
    'username': 'john_doe',
    'email': 'john@example.com',
    'password': 'SecurePassword123!'
})
```

### cURL Examples Collection
```bash
# Set base variables
API_BASE="https://api.example.com/v1"
TOKEN="your-access-token"

# List users
curl -H "Authorization: Bearer $TOKEN" "$API_BASE/users"

# Get specific user
curl -H "Authorization: Bearer $TOKEN" "$API_BASE/users/123"

# Create user
curl -X POST "$API_BASE/users" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"username":"test","email":"test@example.com"}'
```

## Webhooks

### Webhook Events
| Event | Description | Payload |
|-------|-------------|---------|
| `user.created` | New user registered | User object |
| `user.updated` | User profile updated | User object |
| `user.deleted` | User account deleted | User ID |
| `project.created` | New project created | Project object |

### Webhook Configuration
```bash
curl -X POST https://api.example.com/v1/webhooks \
  -H "Authorization: Bearer <token>" \
  -d '{
    "url": "https://your-app.com/webhooks",
    "events": ["user.created", "user.updated"],
    "secret": "webhook-secret-for-verification"
  }'
```

### Webhook Payload Example
```json
{
  "event": "user.created",
  "timestamp": "2025-06-10T12:00:00Z",
  "data": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "username": "john_doe",
    "email": "john@example.com"
  },
  "signature": "sha256=..."
}
```

## Testing

### Postman Collection
Import our Postman collection: [Download Link]

### Sandbox Environment
- **Base URL**: https://sandbox-api.example.com/v1
- **Test API Key**: `test_abc123def456`
- **Rate Limits**: Same as production
- **Data**: Sample data available

## Support

### Getting Help
- **Documentation**: https://docs.example.com
- **Support Email**: api-support@example.com
- **Slack**: #api-support
- **Status Page**: https://status.example.com

### Changelog
- **v1.2.0**: Added project management endpoints
- **v1.1.0**: Added webhook support
- **v1.0.0**: Initial API release

---

**API Team**: @api-team  
**Last Updated**: 2025-06-10  
**Next Review**: 2025-07-10
