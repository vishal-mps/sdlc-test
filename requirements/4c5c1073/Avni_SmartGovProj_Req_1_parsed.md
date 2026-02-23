Table of Contents
1. Document Overview	3
2. Scope Definition	5
3. Proposed Technical Approach	7
4. Development & Delivery Methodology	8
5. Data & Integration Approach	11
6. Security, Privacy & Compliance	12
7. Quality Engineering Approach	12
8. Performance & Scalability	13
9. Deployment & Release Strategy	13
10. Operations & Support	13
11. Risk & Mitigation	14
12. Milestones & Timeline	15
13. Open Items & Decisions Required	15

Technical Project Approach Document
1. Document Overview
1.1 Purpose 
This document outlines the approach for building the Smart Project Governance & Access Control System prototype. It is intended to help stakeholders understand what will be built, what is not included, key dependencies, and high-level effort and cost considerations. The document will be used by senior stakeholders for alignment, review, and approval before implementation begins.
1.2 Project Background
Currently, employee, project, allocation, and access-related information exist across multiple systems, for example: HROne for tracking working hours; accesses are usually granted over emails with no track record, etc. This makes governance, visibility, and control difficult. The goal of this project is to create a central governance cockpit that consolidates people, project, allocation, timesheets, and access data into a single system.
1.3 Objectives & Success Criteria
Objectives
Establish a centralized, auditable employee master that governs identity, hierarchy, and lifecycle across the organization.
Enable structured project creation and controlled visibility to ensure clear ownership and governance throughout the project lifecycle.
Optimize workforce utilization by enforcing capacity limits and providing real-time visibility into employee workload.
Ensure secure, project-bound, and time-bound access through approval-driven workflows with full traceability.
Capture accurate, compliant effort data through controlled time entry and approval processes.
Deliver real-time, role-based insights to support operational oversight and informed decision-making.

Success Criteria
Employee data can be successfully imported and viewed in a People Master screen.
Project allocations are system-driven and fully auditable, eliminating manual or meeting-based assignment decisions.
Employee overload and underutilization are prevented through enforced percentage-based allocation limits.
Clear ownership and accountability are ensured through immutable, approved timesheet records.
All system access is project-bound, time-bound, fully audited, and automatically revoked when no longer required.
Timesheet data (billable and non-billable) is visible at project and employee level.
Reports accurately reflect allocations, access permissions, and project hours.
The system is stable, usable, and demonstrable to stakeholders.  

2. Scope Definition
2.1 In-Scope 
The following items are included in this project:
1. Employee Management 
Login and authentication logic
Centralized Employee Master as the single source of truth 
Onboarding, updates, and deactivation by Admin/HR 
360° employee profile (role, department, projects, allocation, attendance) 
Reporting manager mapping 
Full audit trail for all employee-related actions 
2. Project Management 
Project creation with auto-generated unique Project ID (PID) 
Assignment of Project Owner 
Project metadata (duration, type, department) 
Controlled visibility for sensitive fields (e.g., budget for leadership) 
Mid-project updates (dates, scope, allocations)
3. Resource Allocation 
Percentage-based allocation per project 
Real-time utilization view per employee 
Visual load indicators (flag extra efforts) 
Hard enforcement of 100% utilization cap 
Blocking of over-allocation at the system level 
Heatmap for manager-driven task assignment using filters 
Foundation for future “Best Fit” algorithm 
4. Access Governance 
Access request initiation by employees 
Approval workflows (Employee → Manager → IT/Admin) 
Project-bound and role-bound access 
Time-bound access with auto-expiry 
Flag orphan access
Manual revocation by Manager/Admin 
Centralized Access Matrix (who has what and why) 
Complete audit trail for all access actions 

5. Timesheet Management 
Daily project-wise time entry by employees 
Billable vs non-billable classification 
Manager approval/rejection workflow 
Immutable records (no manager edits) 
Compliance tracking and reminders 
Analytics for utilization and burnout detection 
6. Reporting & Dashboards 
Allocation summary (who is working on what) 
Employee utilization views 
Access matrix dashboard 
Project-wise and employee-wise hours 
Manager and leadership-level views 

2.2 Out-of-Scope
Payroll, billing, or invoicing functionality
Performance management or appraisal systems
Real-time access provisioning to third-party tools
Mobile application development
Advanced analytics
Production-grade security, compliance, or audit certifications
2.3 Assumptions & Dependencies
Employee data required for the prototype will be available via mocked HROne API responses or sample datasets aligned with actual HROne data structures.
Timesheet data for each employee will be available via mocked Jira API responses representing billable and non-billable work logs.
Access to Microsoft Entra ID (Graph APIs) to retrieve manager-employee mapping for accurate hierarchy representation on people master screen. 
Employees are filling their timesheets regularly on the Jira.
The structure and fields of mocked HROne and Jira data will be sufficiently close to real systems to validate workflows and reports.
The success of the prototype is measured by functional demonstration, not live system 

3. Proposed Technical Approach
3.1 Solution Overview
The proposed solution will be built as a set of integrated modules forming a centralized governance system.
The process begins by onboarding employee information using mocked HROne data, which enables the People Master screen and acts as the base for all downstream modules. This master data is used to define employee roles, hierarchy, managers, and locations.
Timesheet data is then sourced from mocked Jira inputs to capture billable and non-billable hours. This serves two purposes: first, to understand billable effort for client charging, and second, to calculate employee utilization and identify over- or under-allocation.
Projects are created with unique project IDs, and employees are allocated to projects using percentage-based allocation. Allocation and timesheet data together provide a clear view of planned versus actual effort.
An access request and approval workflow tracks all system and project access granted to employees, ensuring visibility and governance. All modules come together to provide a unified view of people, projects, utilization, access, and approvals within a single system.
3.2 Technology Stack

4. Development & Delivery Methodology
The Smart Governance & Access Control System will be built as a long-term enterprise platform. The delivery strategy is designed to ensure logical sequencing, data integrity, and predictable scale—from an MVP to a fully governed enterprise product.
4.1 Delivery Model
The project will follow an iterative, dependency-aware delivery model. Modules will be delivered in a defined sequence, where foundational capabilities are completed first and dependent modules are built on top of them.
Each iteration will:
Deliver a usable, production-ready increment
Validate dependent functionality only after prerequisite data and workflows exist
Reduce rework by ensuring upstream systems are stable before downstream features are built
Allow early business validation while preserving architectural integrity
The delivery model follows these principles:
Foundation First – Core system-of-record modules are built before workflows and analytics
Incremental Value – Every phase results in a usable product state
Controlled Expansion – New modules are added only when dependencies are mature
Milestone-Based Reviews – Each phase ends with leadership demos and sign-off
The project progresses through the following macro phases:
Core Data Foundation
Governance & Workflow Enablement
Operational Tracking
Enterprise Control & Security
Analytics & Decision Intelligence
Each phase builds on the previous one, ensuring that the platform evolves in a predictable and auditable manner.

4.2 Development Approach
The system will be developed using a modular but dependency-driven approach. Modules are not built in isolation; they are sequenced based on data and workflow dependencies.
The delivery order is as follows:
People Master (Mocked HROne Data)
This module is the foundation of the entire system.
It establishes: 
Employee identities
Roles and departments
Reporting managers
Skills and attributes
Project Creation & Resource Allocation
Built on top of People Master, this phase introduces: 
Projects with ownership and lifecycle
Allocation of employees to projects
Percentage-based load modeling
Hard enforcement of 100% utilization cap
Timesheet Module (Mocked Jira Data)
The Timesheet module is a critical dependency for: 
Calculating actual employee utilization
Validating allocation accuracy
Determining billable vs non-billable effort
Detecting overwork and burnout
Access Request & Approval Module
This module depends on: 
Employee roles (from People Master)
Project context (from Project & Allocation modules)
Justified, role-bound and project-bound access
Approval workflows
Time-bound access with auto-expiry
Full audit trails
Reporting & Governance Dashboards
Dashboards are built last because they depend on stable data from: 
People
Projects
Allocations
Timesheets
Access workflows
Allocation summaries
Utilization analytics
Access matrices
Project effort views

4.3 DevOps & CI/CD Strategy
All code will be version-controlled using Git.
Builds and basic checks will run automatically on code commits.
Docker will be used to containerize services for consistency across environments.
Environment-specific configurations will be maintained for local and development setups.
Logging will be enabled across services to support debugging and traceability during development and demos. 

5. Data & Integration Approach
5.1 Data Architecture
Relational data architecture will be used. Employees database mocked from the HR One will be used as the master database. 
API request to fetch HR One data will be triggered at multiple instants to update attendance and working hours.
Projects, allocations, timesheets, and access records will be linked using unique identifiers.
5.2 Integration Strategy
The integration approach will be API-driven and loosely coupled.
Employee data will be on boarded using mocked HROne API responses or sample datasets.
For accurate manager-employee mapping people master will also leverage Microsoft Entra ID (via MS Graph APIs). 
Timesheet data will be ingested using mocked Jira API responses.
Integrations will be designed to mirror real-world API contracts to ease future transition to live systems.
Scheduled jobs will be used for periodic data ingestion where required.
Asynchronous messaging will be used for event-driven actions such as notifications.

6. Security, Privacy & Compliance
6.1 Security Architecture
Role-based access control (RBAC) will be enforced across the application.
Authentication will be handled using secure tokens (JWT).
User passwords will be securely stored using hashing (BCrypt).
APIs will be protected to ensure users can access only authorized data.
Sensitive data access (employee details, access permissions) will be restricted by role.
Audit logs will be maintained for access requests and approvals.
6.2 Compliance
The system will follow basic enterprise data privacy principles.
Employee data will be used only for governance and reporting purposes.
Mocked data will be used for prototype implementation.
Access to personal and sensitive information will be role-restricted.
Compliance requirements will be revisited during production rollout.
7. Quality Engineering Approach
7.1 Testing Strategy
Unit testing for core business logic.
API testing to validate request, response, and authorization flows.
Integration testing for dependent modules (People, Projects, Timesheets, Access).
Manual testing for key user workflows and dashboards.
Regression testing before major releases.

7.2 Test Automation
Automated tests will be written for critical APIs.
Test cases will cover authentication, role validation, and data consistency.
Automation will focus on high-risk and high-usage workflows.
Automated tests will be integrated into the CI pipeline.

8. Performance & Scalability
8.1 Performance Targets
API response times to remain within acceptable limits for standard user actions.
Dashboard pages to load within reasonable time for expected data volumes.
Background jobs (allocation calculations, reports) to run without impacting user experience.
8.2 Scalability Approach
Architecture designed to support additional modules in future phases.
9. Deployment & Release Strategy
9.1 Environment Strategy
Development Environment – used by developers/ interns 
SIT – System Integration Testing – used by QA for testing end-to-end workflow.
UAT – used by stakeholders to give business validation
9.2 Release Management
Feature-based releases aligned with iteration milestones.
Versioned deployments to track changes.
Rollback strategy for quick recovery in case of issues.
10. Operations & Support
10.1 Monitoring & Observability
Application logs enabled for error tracking and debugging.
Monitoring of API health and background jobs.
Alerts for critical failures in core workflows (timesheets, access approvals).
10.2 Support Model
Initial support handled by the development team.
Issues tracked using a ticketing or issue management system.
Basic documentation provided for users and administrators.

11. Risk & Mitigation
Risk ID	Risk Description	Impact	Mitigation
R1	Unavailability or mismatch of HROne mock data	Medium	Use sample schemas aligned with expected HROne structure
R2	Jira timesheet data not available or incomplete	High	Use mocked Jira worklogs with realistic data patterns
R3	Dependency between modules causing delays	Medium	Follow dependency-aware delivery and phased development
R4	Scope creep during implementation	Medium	Provide a section after project creation to clearly inform about in-scope and out of scope deliverables and any extra cost attached to them
R5	Performance issues with reports	Low	Use indexing, caching, and background processing
R6	Security gaps in access control	High	Enforce RBAC, audit logs, and secure authentication
R7	Limited/ delayed access to MS Entra ID APIs	Medium	Fall-back to HR One based mapping.

12. Milestones & Timeline
1st BUILD – Week 1 March 2026
Login – authentication/ authorization logic
Employee data mock and display as people master with displays all employee attributes like – team, role, manager, project allocation, skills, etc (HR One and MS Entra ID)
Show current project allocation of employees – along with under/over utilized triggers
Project creation Module – projects database creation
Automatic unique PID allocation 
Display of required employees along with their allocation percentages 
2nd BUILD 
Timesheets import from JIRA
Extra hours on holidays should be logged
Velocity of employee should be calculated
Perform resource allocation calculations
3rd BUILD 
Access workflow module
Power BI Dashboards display
4th BUILD 
Make recommended upgradations and enhancements
Recommending best-match employee.
Velocity of employee based on previously completed projects.
Scope Creep – flag extra efforts (Add this functionality in later stages)
13. Open Items & Decisions Required
What all access can be granted and what amount of data is available. 
How to include scope creep, reusable and non-reusable customization, flag extra efforts, calculation of estimated efforts. 
Gamifying Timesheet Entry – create a leaderboard