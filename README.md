# Backend SIEM system
## Overview

Built a Spring Boot and PostgreSQL backend that ingests, stores, and queries structured security logs through REST APIs. Implemented filtering and retrieval by event type, username, IP address, and log ID using a layered Controller–Service–Repository architecture, with DTOs separating persistence models from API responses. Designed the project as a foundation for future SIEM features including anomaly detection, alerting, and real-time analysis.
---

## Features

### Log Ingestion
- Accepts security logs via REST API
- Stores logs in PostgreSQL database
- Automatically assigns ID and timestamp

### Log Retrieval
- Fetch all logs
- Fetch individual log by ID
- Filter logs by:
  - eventType
  - username
  - ipAddress

### Data Transformation
- Uses DTOs for API responses
- Prevents direct exposure of database entities

### Debug Support
- Simple endpoint to verify controller availability

---

## Architecture

The application is structured into the following layers:

Client → Controller → Service → Repository → Database

### Controller Layer
Handles HTTP requests and returns responses.

### Service Layer
Contains business logic and DTO mapping.

### Repository Layer
Uses Spring Data JPA to communicate with PostgreSQL.

### Model Layer
Defines the database entity structure.

### DTO Layer
Defines request and response contracts for the API.

---

## Data Model

### LogEntry

Represents a security event stored in the database.

| Field      | Type      | Description                     |
|------------|----------|---------------------------------|
| id         | Long     | Unique identifier (auto-generated) |
| eventType  | String   | Type of event (e.g. LOGIN_FAILED) |
| username   | String   | User involved in the event     |
| ipAddress  | String   | Source IP address              |
| message    | String   | Description of event          |
| timestamp  | DateTime | Time event was created        |

---

## API Endpoints

### Create Log

**POST /logs**

Creates a new security log entry.

#### Example Request

```json
{
  "eventType": "LOGIN_FAILED",
  "username": "admin",
  "ipAddress": "185.10.20.30",
  "message": "Invalid password attempt"
}
```
Example Response
```json
{
  "id": 1,
  "eventType": "LOGIN_FAILED",
  "username": "admin",
  "ipAddress": "185.10.20.30",
  "message": "Invalid password attempt",
  "timestamp": "2026-06-01T21:00:00"
}
```
## Get All Logs

**GET /logs**

Returns all stored logs.

---

## Filter Logs

**GET /logs**

Supports query parameters for filtering.

### Examples

- `/logs?eventType=LOGIN_FAILED`
- `/logs?username=admin`
- `/logs?ipAddress=185.10.20.30`

Only one filter is applied per request.

---

## Get Log by ID

**GET /logs/{id}**

Returns a single log entry by its ID.

If the log does not exist, an error response is returned.

---

## Debug Endpoint

**GET /logs/debug**

Returns a simple message confirming that the controller is running.

---

## Technologies Used

- Java 21
- Spring Boot
- Spring Web (REST API)
- Spring Data JPA
- Hibernate ORM
- PostgreSQL
- Maven
