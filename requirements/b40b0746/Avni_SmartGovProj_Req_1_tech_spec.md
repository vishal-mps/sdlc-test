# Technical Specification

## 1. System Overview

**Project Name:** Smart Project Governance & Access Control System

**Description:** A centralized system for managing employee data, project allocations, timesheets, and access controls, ensuring governance, visibility, and control.

**Architecture Pattern:** Microservices

### Key Design Decisions

- Use a microservices architecture to ensure modularity and scalability.
- Implement RBAC for secure access control.
- Utilize API-driven integrations for data consistency.

## 2. Data Model

### Entity: Employee

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the employee. |
| name | String | Employee's full name. |
| email | String | Employee's email address. |
| role | String | Employee's role in the organization. |
| department | String | Employee's department. |
| manager_id | UUID | Unique identifier of the employee's manager. |
| status | String | Employee's status (active, inactive, on leave, etc.). |
| created_at | Timestamp | Timestamp of when the employee record was created. |
| updated_at | Timestamp | Timestamp of the last update to the employee record. |

**Relationships:** manager_id

### Entity: Project

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the project. |
| name | String | Name of the project. |
| description | String | Description of the project. |
| owner_id | UUID | Unique identifier of the project owner. |
| start_date | Date | Start date of the project. |
| end_date | Date | End date of the project. |
| status | String | Status of the project (active, completed, on hold, etc.). |
| created_at | Timestamp | Timestamp of when the project record was created. |
| updated_at | Timestamp | Timestamp of the last update to the project record. |

**Relationships:** owner_id

### Entity: Allocation

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the allocation. |
| employee_id | UUID | Unique identifier of the employee. |
| project_id | UUID | Unique identifier of the project. |
| percentage | Float | Percentage of the employee's time allocated to the project. |
| start_date | Date | Start date of the allocation. |
| end_date | Date | End date of the allocation. |
| status | String | Status of the allocation (active, completed, on hold, etc.). |
| created_at | Timestamp | Timestamp of when the allocation record was created. |
| updated_at | Timestamp | Timestamp of the last update to the allocation record. |

**Relationships:** employee_id, project_id

### Entity: Timesheet

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the timesheet. |
| employee_id | UUID | Unique identifier of the employee. |
| project_id | UUID | Unique identifier of the project. |
| date | Date | Date of the timesheet entry. |
| hours | Float | Number of hours worked on the project. |
| is_billable | Boolean | Indicates if the hours are billable. |
| status | String | Status of the timesheet entry (approved, pending, rejected). |
| created_at | Timestamp | Timestamp of when the timesheet entry was created. |
| updated_at | Timestamp | Timestamp of the last update to the timesheet entry. |

**Relationships:** employee_id, project_id

### Entity: AccessRequest

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Unique identifier for the access request. |
| employee_id | UUID | Unique identifier of the employee. |
| project_id | UUID | Unique identifier of the project. |
| start_date | Date | Start date of the access request. |
| end_date | Date | End date of the access request. |
| status | String | Status of the access request (pending, approved, rejected). |
| created_at | Timestamp | Timestamp of when the access request was created. |
| updated_at | Timestamp | Timestamp of the last update to the access request. |

**Relationships:** employee_id, project_id

## 3. API Design

| Method | Path | Description |
|--------|------|-------------|
| GET | `/employees` | Retrieve a list of all employees. |
| POST | `/employees` | Create a new employee. |
| GET | `/employees/{id}` | Retrieve details of a specific employee. |
| PUT | `/employees/{id}` | Update details of a specific employee. |
| DELETE | `/employees/{id}` | Delete a specific employee. |
| GET | `/projects` | Retrieve a list of all projects. |
| POST | `/projects` | Create a new project. |
| GET | `/projects/{id}` | Retrieve details of a specific project. |
| PUT | `/projects/{id}` | Update details of a specific project. |
| DELETE | `/projects/{id}` | Delete a specific project. |
| GET | `/allocations` | Retrieve a list of all allocations. |
| POST | `/allocations` | Create a new allocation. |
| GET | `/allocations/{id}` | Retrieve details of a specific allocation. |
| PUT | `/allocations/{id}` | Update details of a specific allocation. |
| DELETE | `/allocations/{id}` | Delete a specific allocation. |
| GET | `/timesheets` | Retrieve a list of all timesheets. |
| POST | `/timesheets` | Create a new timesheet entry. |
| GET | `/timesheets/{id}` | Retrieve details of a specific timesheet entry. |
| PUT | `/timesheets/{id}` | Update details of a specific timesheet entry. |
| DELETE | `/timesheets/{id}` | Delete a specific timesheet entry. |
| GET | `/accessrequests` | Retrieve a list of all access requests. |
| POST | `/accessrequests` | Create a new access request. |
| GET | `/accessrequests/{id}` | Retrieve details of a specific access request. |
| PUT | `/accessrequests/{id}` | Update details of a specific access request. |
| DELETE | `/accessrequests/{id}` | Delete a specific access request. |

### GET `/employees`

Retrieve a list of all employees.

**Response Body:** List of Employee entities.

### POST `/employees`

Create a new employee.

**Request Body:** Employee entity with required fields.

**Response Body:** Created Employee entity.

### GET `/employees/{id}`

Retrieve details of a specific employee.

**Response Body:** Employee entity.

### PUT `/employees/{id}`

Update details of a specific employee.

**Request Body:** Employee entity with updated fields.

**Response Body:** Updated Employee entity.

### DELETE `/employees/{id}`

Delete a specific employee.

**Response Body:** Confirmation message.

### GET `/projects`

Retrieve a list of all projects.

**Response Body:** List of Project entities.

### POST `/projects`

Create a new project.

**Request Body:** Project entity with required fields.

**Response Body:** Created Project entity.

### GET `/projects/{id}`

Retrieve details of a specific project.

**Response Body:** Project entity.

### PUT `/projects/{id}`

Update details of a specific project.

**Request Body:** Project entity with updated fields.

**Response Body:** Updated Project entity.

### DELETE `/projects/{id}`

Delete a specific project.

**Response Body:** Confirmation message.

### GET `/allocations`

Retrieve a list of all allocations.

**Response Body:** List of Allocation entities.

### POST `/allocations`

Create a new allocation.

**Request Body:** Allocation entity with required fields.

**Response Body:** Created Allocation entity.

### GET `/allocations/{id}`

Retrieve details of a specific allocation.

**Response Body:** Allocation entity.

### PUT `/allocations/{id}`

Update details of a specific allocation.

**Request Body:** Allocation entity with updated fields.

**Response Body:** Updated Allocation entity.

### DELETE `/allocations/{id}`

Delete a specific allocation.

**Response Body:** Confirmation message.

### GET `/timesheets`

Retrieve a list of all timesheets.

**Response Body:** List of Timesheet entities.

### POST `/timesheets`

Create a new timesheet entry.

**Request Body:** Timesheet entity with required fields.

**Response Body:** Created Timesheet entity.

### GET `/timesheets/{id}`

Retrieve details of a specific timesheet entry.

**Response Body:** Timesheet entity.

### PUT `/timesheets/{id}`

Update details of a specific timesheet entry.

**Request Body:** Timesheet entity with updated fields.

**Response Body:** Updated Timesheet entity.

### DELETE `/timesheets/{id}`

Delete a specific timesheet entry.

**Response Body:** Confirmation message.

### GET `/accessrequests`

Retrieve a list of all access requests.

**Response Body:** List of AccessRequest entities.

### POST `/accessrequests`

Create a new access request.

**Request Body:** AccessRequest entity with required fields.

**Response Body:** Created AccessRequest entity.

### GET `/accessrequests/{id}`

Retrieve details of a specific access request.

**Response Body:** AccessRequest entity.

### PUT `/accessrequests/{id}`

Update details of a specific access request.

**Request Body:** AccessRequest entity with updated fields.

**Response Body:** Updated AccessRequest entity.

### DELETE `/accessrequests/{id}`

Delete a specific access request.

**Response Body:** Confirmation message.

## 4. Component Breakdown

### EmployeeService

**Responsibility:** Handles CRUD operations for Employee entities.

**Depends on:** Database

### ProjectService

**Responsibility:** Handles CRUD operations for Project entities.

**Depends on:** Database

### AllocationService

**Responsibility:** Handles CRUD operations for Allocation entities.

**Depends on:** Database

### TimesheetService

**Responsibility:** Handles CRUD operations for Timesheet entities.

**Depends on:** Database

### AccessRequestService

**Responsibility:** Handles CRUD operations for AccessRequest entities.

**Depends on:** Database

### AuthenticationService

**Responsibility:** Manages user authentication and authorization.

**Depends on:** Database

### NotificationService

**Responsibility:** Sends notifications for access requests and approvals.

**Depends on:** EmailService, SMSService

### DashboardService

**Responsibility:** Generates and serves dashboards for reporting and analytics.

**Depends on:** Database

## 5. Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React |
| Backend | Node.js with Express |
| Database | PostgreSQL |
| Infrastructure | AWS |

## 6. Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Unavailability or mismatch of HROne mock data | Medium | Use sample schemas aligned with expected HROne structure. |
| Jira timesheet data not available or incomplete | High | Use mocked Jira worklogs with realistic data patterns. |
| Dependency between modules causing delays | Medium | Follow dependency-aware delivery and phased development. |
| Scope creep during implementation | Medium | Provide a section after project creation to clearly inform about in-scope and out of scope deliverables and any extra cost attached to them. |
| Performance issues with reports | Low | Use indexing, caching, and background processing. |
| Security gaps in access control | High | Enforce RBAC, audit logs, and secure authentication. |

## 7. Open Questions

- What all access can be granted and what amount of data is available.
- How to include scope creep, reusable and non-reusable customization, flag extra efforts, calculation of estimated efforts.
- Gamifying Timesheet Entry – create a leaderboard.
