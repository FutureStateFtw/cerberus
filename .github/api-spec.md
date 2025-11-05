# Cerberus API Specification

## Base URL
`/api/v1`

## Endpoints

### Applications

#### GET /applications
Get all applications.

**Parameters:** None

**Response - Success (200):**
```json
[
  {
    "id": 1,
    "name": "Roll Call",
    "description": "App for recording a persons presence at an event."
  }
]
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to retrieve applications"
}
```

---

#### GET /applications/{id}
Get a specific application by ID.

**Parameters:**
- `id` (path parameter, integer) - Application ID

**Response - Success (200):**
```json
{
  "id": 1,
  "name": "Roll Call",
  "description": "App for recording a persons presence at an event."
}
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 1 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to retrieve application"
}
```

---

#### POST /applications
Create a new application. Automatically creates three default roles: 'user', 'admin', and 'authorizer'.

**Parameters:** None

**Request Body:**
```json
{
  "name": "CardPulse",
  "description": "Application for monitoring card access"
}
```

**Response - Success (201):**
```json
{
  "id": 5,
  "name": "CardPulse",
  "description": "Application for monitoring card access"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Application name already exists"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Name is required"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to create application"
}
```

---

#### PUT /applications/{id}
Update an existing application.

**Parameters:**
- `id` (path parameter, integer) - Application ID

**Request Body:**
```json
{
  "name": "CardPulse Updated",
  "description": "Updated description"
}
```

**Response - Success (200):**
```json
{
  "id": 5,
  "name": "CardPulse Updated",
  "description": "Updated description"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Application name already exists"
}
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 5 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to update application"
}
```

---

#### DELETE /applications/{id}
Delete an application. This will cascade delete all associated roles and user role assignments.

**Parameters:**
- `id` (path parameter, integer) - Application ID

**Response - Success (204):**
No content

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 5 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to delete application"
}
```

---

### Application Roles

#### GET /applications/{applicationId}/roles
Get all roles for a specific application.

**Parameters:**
- `applicationId` (path parameter, integer) - Application ID

**Response - Success (200):**
```json
[
  {
    "applicationId": 1,
    "role": "admin",
    "description": "Opens all aspects of the application"
  },
  {
    "applicationId": 1,
    "role": "user",
    "description": "Basic user access"
  },
  {
    "applicationId": 1,
    "role": "authorizer",
    "description": "Can add and remove users from the application"
  }
]
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 1 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to retrieve roles"
}
```

---

#### POST /applications/{applicationId}/roles
Create a new role for an application.

**Parameters:**
- `applicationId` (path parameter, integer) - Application ID

**Request Body:**
```json
{
  "role": "docReader",
  "description": "Can read documents but not edit"
}
```

**Response - Success (201):**
```json
{
  "applicationId": 1,
  "role": "docReader",
  "description": "Can read documents but not edit"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Role already exists for this application"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Role name is required"
}
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 1 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to create role"
}
```

---

#### DELETE /applications/{applicationId}/roles/{role}
Delete a role from an application. This will cascade delete all user role assignments for this role.

**Parameters:**
- `applicationId` (path parameter, integer) - Application ID
- `role` (path parameter, string) - Role name

**Response - Success (204):**
No content

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Role 'docReader' not found for application 1"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to delete role"
}
```

---

### User Roles

#### GET /applications/{applicationId}/user-roles
Get all user role assignments for a specific application.

**Parameters:**
- `applicationId` (path parameter, integer) - Application ID

**Response - Success (200):**
```json
[
  {
    "userRoleId": 1,
    "applicationId": 1,
    "role": "admin",
    "personId": "11502045",
    "justification": "Needs admin access to manage event configurations",
    "addedBy": "10045678",
    "addedAt": "2025-01-15T14:30:00Z"
  },
  {
    "userRoleId": 2,
    "applicationId": 1,
    "role": "user",
    "personId": "11502045",
    "justification": "Basic access for event participation",
    "addedBy": "10045678",
    "addedAt": "2025-01-15T14:35:00Z"
  }
]
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 1 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to retrieve user roles"
}
```

---

#### GET /persons/{personId}/user-roles
Get all user role assignments for a specific person across all applications (user-centric view).

**Parameters:**
- `personId` (path parameter, string) - Person ID

**Response - Success (200):**
```json
[
  {
    "userRoleId": 1,
    "applicationId": 1,
    "applicationName": "Roll Call",
    "role": "admin",
    "personId": "11502045",
    "justification": "Needs admin access to manage event configurations",
    "addedBy": "10045678",
    "addedAt": "2025-01-15T14:30:00Z"
  },
  {
    "userRoleId": 3,
    "applicationId": 2,
    "applicationName": "CardPulse",
    "role": "user",
    "personId": "11502045",
    "justification": "Basic access for card monitoring",
    "addedBy": "10045678",
    "addedAt": "2025-01-16T09:15:00Z"
  }
]
```

**Response - Success (200) - No roles found:**
```json
[]
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to retrieve user roles"
}
```

---

#### POST /applications/{applicationId}/user-roles
Add a user role assignment to an application.

**Parameters:**
- `applicationId` (path parameter, integer) - Application ID

**Request Body:**
```json
{
  "personId": "11502045",
  "role": "admin",
  "justification": "Needs admin access to manage event configurations",
  "addedBy": "10045678"
}
```

**Response - Success (201):**
```json
{
  "userRoleId": 15,
  "applicationId": 1,
  "role": "admin",
  "personId": "11502045",
  "justification": "Needs admin access to manage event configurations",
  "addedBy": "10045678",
  "addedAt": "2025-01-15T14:30:00Z"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Person ID, role, justification, and addedBy are required"
}
```

**Response - Error (400):**
```json
{
  "error": "Bad request",
  "message": "Role 'invalidRole' does not exist for this application"
}
```

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "Application with id 1 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to create user role"
}
```

---

#### DELETE /user-roles/{userRoleId}
Delete a user role assignment.

**Parameters:**
- `userRoleId` (path parameter, integer) - User Role ID

**Response - Success (204):**
No content

**Response - Error (404):**
```json
{
  "error": "Not found",
  "message": "User role with id 15 not found"
}
```

**Response - Error (500):**
```json
{
  "error": "Internal server error",
  "message": "Failed to delete user role"
}
```

---

## Common Error Responses

### 401 Unauthorized
Returned when authentication is required but not provided or invalid.
```json
{
  "error": "Unauthorized",
  "message": "Authentication required"
}
```

### 403 Forbidden
Returned when the authenticated user doesn't have permission to perform the action.
```json
{
  "error": "Forbidden",
  "message": "You do not have permission to perform this action"
}
```
