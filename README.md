# Synapse API

**DecodeLabs | Industrial Training Kit | Project 2: Backend API Development**
**Theme: "The Nervous System" — Engineering the Backend API & Architectural Integrity**

> "If it isn't documented, it doesn't exist."

## What This Project Demonstrates

This is a small, fully working REST API for managing a list of **tasks**.
It was built specifically to satisfy the Project 2 requirements from the
training kit by mapping each "anatomical" concept from the slides to real
code:

| Slide Concept | Where it lives in this project |
|---|---|
| **IPO Model** (Input → Process → Output) | Every route handler in `routes/tasks.js` follows this pattern |
| **The Synaptic Gap** (Client ↔ Network ↔ Server) | `server.js` is the API Gateway / Brain Stem receiving all requests |
| **HTTP Methods** (GET / POST / PUT / DELETE) | `routes/tasks.js` |
| **RESTful Naming** (resources are nouns, methods are verbs) | `/api/tasks`, `/api/tasks/:id` — never `/getTasks` |
| **The Neurotransmitter (JSON)** | `express.json()` + all responses are JSON |
| **The Gatekeeper Rule** ("Never trust the client") | `middleware/validate.js` |
| **Server's Tone** (semantic status codes) | 200, 201, 204, 400, 404, 500 used correctly throughout |
| **Circuit Breaker / Resilience** | `middleware/errorHandler.js` |
| **Documentation & DX** | This README |

## Project Structure

```
synapse-api/
├── server.js              # Entry point — the "Brain Stem" / API Gateway
├── package.json
├── routes/
│   └── tasks.js            # All /api/tasks endpoints (the resource)
├── middleware/
│   ├── validate.js         # The "Gatekeeper" — input validation
│   └── errorHandler.js      # 404 + global 500 error handler
└── data/
    └── store.js            # In-memory "database" (swap for real DB later)
```

## Getting Started

```bash
# 1. Install dependencies
npm install

# 2. Start the server
npm start

# Server runs at http://localhost:3000
```

## API Reference

### Health Check

```
GET /api/health
```
Returns `200 OK` with a status message and timestamp. Use this to confirm
the "nervous system" is alive.

### List Tasks

```
GET /api/tasks
GET /api/tasks?completed=true
GET /api/tasks?priority=high
```

Returns `200 OK` with all tasks, optionally filtered by `completed` or
`priority`.

**Example response:**
```json
{
  "status": "success",
  "count": 2,
  "data": [
    {
      "id": 1,
      "title": "Design API endpoints",
      "description": "Plan out GET and POST routes for the task resource",
      "priority": "high",
      "completed": true,
      "createdAt": "2026-06-10T09:00:00.000Z"
    }
  ]
}
```

### Get a Single Task

```
GET /api/tasks/:id
```

| Status | Meaning |
|---|---|
| `200 OK` | Task found and returned |
| `400 Bad Request` | `:id` is not a positive integer |
| `404 Not Found` | No task exists with that id |

### Create a Task

```
POST /api/tasks
Content-Type: application/json
```

**Body:**
```json
{
  "title": "Write integration tests",
  "description": "Cover all CRUD endpoints",
  "priority": "medium",
  "completed": false
}
```

| Field | Type | Required | Notes |
|---|---|---|---|
| `title` | string | ✅ | min length 3 |
| `description` | string | ❌ | defaults to `""` |
| `priority` | string | ❌ | one of `low`, `medium`, `high`; defaults to `medium` |
| `completed` | boolean | ❌ | defaults to `false` |

| Status | Meaning |
|---|---|
| `201 Created` | Task created. Response includes a `Location` header pointing to the new resource |
| `400 Bad Request` | Validation failed (e.g. missing/short title, invalid priority) |

### Update a Task

```
PUT /api/tasks/:id
Content-Type: application/json
```

Send only the fields you want to change — all fields are optional, but
the body cannot be empty.

| Status | Meaning |
|---|---|
| `200 OK` | Task updated successfully |
| `400 Bad Request` | Invalid `:id`, empty body, or invalid field values |
| `404 Not Found` | No task exists with that id |

### Delete a Task

```
DELETE /api/tasks/:id
```

| Status | Meaning |
|---|---|
| `204 No Content` | Task deleted successfully (no response body) |
| `400 Bad Request` | `:id` is not a positive integer |
| `404 Not Found` | No task exists with that id |

## Status Code Vocabulary Used

| Code | Name | When it's returned |
|---|---|---|
| `200` | OK | Successful GET / PUT |
| `201` | Created | Successful POST |
| `204` | No Content | Successful DELETE |
| `400` | Bad Request | Failed validation (client's fault) |
| `404` | Not Found | Unknown route or missing resource |
| `500` | Internal Server Error | Unexpected server fault (circuit breaker) |

## Example: Full curl Walkthrough

```bash
# Health check
curl http://localhost:3000/api/health

# Create a task
curl -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "Ship the API", "priority": "high"}'

# List all tasks
curl http://localhost:3000/api/tasks

# Get one task
curl http://localhost:3000/api/tasks/1

# Update a task
curl -X PUT http://localhost:3000/api/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{"completed": true}'

# Delete a task
curl -X DELETE http://localhost:3000/api/tasks/1
```

## Ideas for Extending This Project (per the Conclusion slide)

- Swap `data/store.js` for a real database (MongoDB/PostgreSQL) without
  changing any route signatures.
- Add authentication (AuthN) and authorization (AuthZ) middleware.
- Add a `429 Too Many Requests` rate limiter.
- Write automated tests for every status code path (200/201/204/400/404/500).
- Add pagination to `GET /api/tasks` for large datasets.

---
## Author

**Easha **

* GitHub: https://github.com/easha1708
* DecodeLabs Full Stack Development Track

## License

This project was created for educational and internship evaluation purposes under the DecodeLabs Industrial Training Program.

---

*Built with Express.js — Project 2, Full Stack Development Track, DecodeLabs (2026).*

