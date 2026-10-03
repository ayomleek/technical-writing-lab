# API Reference: Create a Task

**API:** TaskFlow Project Management API
**Version:** v1
**Base URL:** `https://api.taskflow.example.com`

---

## `POST /v1/projects/{projectId}/tasks`

Creates a new task inside an existing project and returns the newly created task.

### Description

Use this endpoint to add a task to a project. Every task requires a title, an assignee, a due date, and a priority level. A longer description is optional. The new task starts with the status `todo`.

The authenticated user must be a member of the project with permission to create tasks (the **Member**, **Manager**, or **Admin** role). The assignee must also be a member of the same project.

---

### Authentication and Headers

| Header | Required | Value | Description |
|---|---|---|---|
| `Authorization` | Yes | `Bearer <access_token>` | OAuth 2.0 access token that identifies the authenticated user. |
| `Content-Type` | Yes | `application/json` | Declares that the request body is JSON. |
| `Accept` | No | `application/json` | Requests a JSON response. This is the default. |
| `Idempotency-Key` | No | A unique string, up to 64 characters (for example a UUID) | If you retry a request with the same key within 24 hours, the server returns the original result instead of creating a duplicate task. |

---

### Path Parameters

| Name | Type | Required | Description |
|---|---|---|---|
| `projectId` | string | Yes | The unique ID of the project that will contain the task (for example `prj_8f3k29`). |

### Query Parameters

This endpoint does not accept query parameters.

### Request Body Parameters

| Name | Type | Required | Constraints | Description |
|---|---|---|---|---|
| `title` | string | Yes | 1 to 200 characters; cannot be blank | A short name that summarizes the task. |
| `description` | string | No | Up to 5,000 characters | A detailed explanation of the work. If omitted, the response returns `null`. |
| `assigneeId` | string | Yes | Must be the ID of a member of the project | The ID of the user responsible for the task (for example `usr_42ab91`). |
| `dueDate` | string (date) | Yes | ISO 8601 format `YYYY-MM-DD`; cannot be in the past | The date by which the task should be completed. |
| `priority` | string (enum) | Yes | One of `low`, `medium`, `high` (lowercase) | How urgent the task is. |

---

### Example Request

**Request**

```http
POST /v1/projects/prj_8f3k29/tasks HTTP/1.1
Host: api.taskflow.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.example
Content-Type: application/json
Idempotency-Key: 3b1f9c52-7d0e-4a6b-8c21-5e9f0a7d4b13
```

**Request body**

```json
{
  "title": "Design login screen mockups",
  "description": "Create high-fidelity mockups for the mobile login and sign-up screens, following the new brand guidelines.",
  "assigneeId": "usr_42ab91",
  "dueDate": "2026-10-16",
  "priority": "high"
}
```

**Equivalent cURL command**

```bash
curl -X POST "https://api.taskflow.example.com/v1/projects/prj_8f3k29/tasks" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
        "title": "Design login screen mockups",
        "description": "Create high-fidelity mockups for the mobile login and sign-up screens, following the new brand guidelines.",
        "assigneeId": "usr_42ab91",
        "dueDate": "2026-10-16",
        "priority": "high"
      }'
```

---

### Successful Response

**Status:** `201 Created`

**Response headers**

| Header | Example | Description |
|---|---|---|
| `Location` | `/v1/projects/prj_8f3k29/tasks/tsk_71c0de` | URL of the newly created task. |
| `Content-Type` | `application/json` | Format of the response body. |

**Response body**

```json
{
  "id": "tsk_71c0de",
  "projectId": "prj_8f3k29",
  "title": "Design login screen mockups",
  "description": "Create high-fidelity mockups for the mobile login and sign-up screens, following the new brand guidelines.",
  "assigneeId": "usr_42ab91",
  "dueDate": "2026-10-16",
  "priority": "high",
  "status": "todo",
  "createdBy": "usr_17f3c8",
  "createdAt": "2026-10-03T09:42:15Z",
  "updatedAt": "2026-10-03T09:42:15Z"
}
```

### Response Body Fields

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique ID generated for the new task. |
| `projectId` | string | ID of the project that contains the task. |
| `title` | string | The task title as saved. |
| `description` | string or null | The task description, or `null` if none was provided. |
| `assigneeId` | string | ID of the assigned user. |
| `dueDate` | string (date) | The due date in `YYYY-MM-DD` format. |
| `priority` | string | `low`, `medium`, or `high`. |
| `status` | string | Always `todo` for a new task. |
| `createdBy` | string | ID of the user who created the task (the authenticated user). |
| `createdAt` | string (datetime) | Creation time in UTC, ISO 8601 format. |
| `updatedAt` | string (datetime) | Time of last modification in UTC, ISO 8601 format. Equal to `createdAt` for a new task. |

---

### HTTP Response Codes

| Code | Name | When it occurs |
|---|---|---|
| `201` | Created | The task was created successfully. The response body contains the new task. |
| `400` | Bad Request | The request could not be understood, for example the body is not valid JSON or a required header is malformed. |
| `401` | Unauthorized | The `Authorization` header is missing, or the access token is invalid or expired. |
| `403` | Forbidden | The token is valid, but the user is not a member of the project or lacks permission to create tasks in it. |
| `404` | Not Found | No project exists with the given `projectId`, or the user is not allowed to know it exists. |
| `409` | Conflict | An `Idempotency-Key` was reused with a different request body than the original request. |
| `415` | Unsupported Media Type | The `Content-Type` header is missing or is not `application/json`. |
| `422` | Unprocessable Entity | The JSON is valid but the data fails validation: a required field is missing, `title` is blank or too long, `priority` is not `low`, `medium`, or `high`, `dueDate` is badly formatted or in the past, or `assigneeId` does not belong to a project member. |
| `429` | Too Many Requests | The client exceeded the rate limit (100 requests per minute per user). Wait for the number of seconds in the `Retry-After` header, then retry. |
| `500` | Internal Server Error | An unexpected error occurred on the server. Retry later; contact support if it persists. |
| `503` | Service Unavailable | The service is temporarily unavailable, for example during maintenance. Retry later. |

### Error Response Format

All error responses use the same structure.

**Example: `422 Unprocessable Entity`**

```json
{
  "error": {
    "code": "validation_failed",
    "message": "One or more fields are invalid.",
    "details": [
      {
        "field": "priority",
        "issue": "Must be one of: low, medium, high."
      },
      {
        "field": "dueDate",
        "issue": "Must not be in the past."
      }
    ],
    "requestId": "req_5d2a8e19"
  }
}
```

Include the `requestId` when contacting support.

---

### Notes

- Task titles are not required to be unique within a project.
- Dates are interpreted without a time zone. The server compares `dueDate` with the current date in UTC when checking whether it is in the past.
- To update or delete a task after creating it, use the `id` returned in the response with the task endpoints (`PATCH` or `DELETE /v1/projects/{projectId}/tasks/{taskId}`).
