# Synapse API

**DecodeLabs | Industrial Training Program | Project 2: Backend API Development**
**Theme: "The Nervous System" — Building a Reliable Backend Architecture**

> "A good project is not only built with code, it is built with proper documentation."

---

## Project Overview

**Synapse API** is a RESTful backend API developed for managing tasks efficiently.
This project focuses on backend fundamentals including API design, HTTP methods, request handling, validation, error management, and proper response formatting.

The main goal of this project is to demonstrate how a backend system works as a communication layer between the client and server by following clean REST API principles.

---

## Key Concepts Implemented

| Concept                                  | Implementation                                                                      |
| ---------------------------------------- | ----------------------------------------------------------------------------------- |
| **IPO Model (Input → Process → Output)** | Each API route receives input, processes the request, and returns a proper response |
| **Client-Server Communication**          | `server.js` works as the main API entry point handling incoming requests            |
| **HTTP Methods**                         | GET, POST, PUT, and DELETE methods are implemented in task routes                   |
| **RESTful API Design**                   | Resources are accessed using proper endpoints like `/api/tasks`                     |
| **JSON Data Handling**                   | Express JSON middleware is used for request and response data                       |
| **Input Validation**                     | Middleware validates user input before processing requests                          |
| **HTTP Status Codes**                    | Correct status codes like 200, 201, 204, 400, 404, and 500 are used                 |
| **Error Handling**                       | Centralized error handling improves API reliability                                 |

---

# Project Structure

```
synapse-api/
│
├── server.js
│   └── Main server file and API entry point
│
├── package.json
│   └── Project dependencies and scripts
│
├── routes/
│   └── tasks.js
│       └── Contains all task-related API routes
│
├── middleware/
│   ├── validate.js
│   │   └── Handles request validation
│   │
│   └── errorHandler.js
│       └── Manages API errors
│
└── data/
    └── store.js
        └── Temporary in-memory data storage
```

---

# Installation & Setup

Follow these steps to run the project locally:

### 1. Install Dependencies

```bash
npm install
```

### 2. Start the Server

```bash
npm start
```

The API will start running at:

```
http://localhost:3000
```

---

# API Endpoints

## Health Check

### Request

```
GET /api/health
```

Checks whether the server is active and returns server status information.

---

# Task APIs

## Get All Tasks

```
GET /api/tasks
```

Optional filters:

```
GET /api/tasks?completed=true

GET /api/tasks?priority=high
```

Returns the available tasks in JSON format.

Example Response:

```json
{
 "status":"success",
 "count":2,
 "data":[
  {
   "id":1,
   "title":"Design API endpoints",
   "priority":"high",
   "completed":true
  }
 ]
}
```

---

## Get Single Task

```
GET /api/tasks/:id
```

Response Codes:

| Code | Meaning                 |
| ---- | ----------------------- |
| 200  | Task found successfully |
| 400  | Invalid task ID         |
| 404  | Task not found          |

---

## Create New Task

```
POST /api/tasks
```

Request Body:

```json
{
"title":"Build API",
"description":"Complete backend development",
"priority":"medium",
"completed":false
}
```

Supported Fields:

| Field       | Type    | Required |
| ----------- | ------- | -------- |
| title       | String  | Yes      |
| description | String  | No       |
| priority    | String  | No       |
| completed   | Boolean | No       |

Response:

| Code | Meaning                   |
| ---- | ------------------------- |
| 201  | Task created successfully |
| 400  | Invalid input             |

---

## Update Task

```
PUT /api/tasks/:id
```

Updates existing task details.

Response:

| Code | Meaning              |
| ---- | -------------------- |
| 200  | Updated successfully |
| 400  | Invalid request      |
| 404  | Task not found       |

---

## Delete Task

```
DELETE /api/tasks/:id
```

Deletes a task from the system.

Response:

| Code | Meaning              |
| ---- | -------------------- |
| 204  | Deleted successfully |
| 404  | Task not found       |

---

# Status Codes Used

| Status           | Usage                |
| ---------------- | -------------------- |
| 200 OK           | Successful request   |
| 201 Created      | New resource created |
| 204 No Content   | Successful deletion  |
| 400 Bad Request  | Invalid user input   |
| 404 Not Found    | Resource unavailable |
| 500 Server Error | Unexpected error     |

---

# API Testing Example

```bash
GET /api/health

GET /api/tasks

POST /api/tasks

PUT /api/tasks/1

DELETE /api/tasks/1
```

---

# Future Enhancements

Possible improvements:

* Connect with MongoDB/PostgreSQL database
* Add authentication and authorization
* Add API rate limiting
* Create automated API tests
* Add pagination support
* Deploy API on cloud services

---

# Technologies Used

* Node.js
* Express.js
* REST API
* JSON
* JavaScript

---

## Author

**Easha**

---

Built as part of **DecodeLabs Full Stack Development Track — Project 2 (2026)**
