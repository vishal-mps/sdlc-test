# stack.md

# Technology Stack Overview

## Executive Summary
The PDF Stamping Service is a Spring Boot application built on the JVM that provides functionality for applying stamps (text, images, or HTML content) to PDF documents. The system follows a layered architecture with clear separation of concerns, utilizing modern Java technologies and well-established PDF processing libraries. It operates as a traditional server deployment with no database dependencies, focusing entirely on PDF manipulation and transformation. The core architecture leverages Spring Boot's auto-configuration capabilities alongside specialized PDF libraries to deliver robust document stamping functionality.

## Core Technologies

### Programming Language
**Language:** Java
**Rationale:** Java was chosen for its platform independence, strong typing, extensive ecosystem of libraries, and suitability for enterprise-level document processing applications. The JVM runtime provides consistent execution across different environments.

### Framework
**Framework:** Spring Boot
**Key Features Used:** 
- Auto-configuration for rapid application setup
- RESTful web services through Spring MVC
- Dependency injection for loose coupling
- Global exception handling
- Configuration management through application.yml

### Database
**Type:** none
**ORM/Driver:** None required as the application is stateless and does not persist data

### Runtime Environment
**Runtime:** JVM

## Supporting Technologies

### Authentication & Authorization
none

### External Integrations
- iText 7 pdfHTML for HTML rendering with native link support
- Apache PDFBox (implied by usage of PDF stamper components)

### Development & Build Tools
- Maven or Gradle (implied by standard Spring Boot project structure)
- Lombok for code generation
- Jackson for JSON processing (included with Spring Boot)

### Testing Stack
- JUnit 5 for unit testing
- Mockito for mocking dependencies
- Spring Boot Test for integration testing
- Test containers (implied by testing practices)

### Deployment
**Model:** traditional server

## Architecture Patterns
The system follows a layered architecture pattern with clear separation between:
- Presentation Layer: REST API controllers (`StampController`)
- Service Layer: Core business logic in service classes (`StampService`, `MetadataFrontPageService`, `AdStampService`, `AdFetchService`)
- Stamper Layer: Specialized implementations (`TextStamper`, `ImageStamper`, `HtmlStamper`, `PdfStamper`)
- Model Layer: Data transfer objects and enums defining request/response structures
- Exception Handling: Global exception handler (`GlobalExceptionHandler`) for standardized error responses

The system also employs interface-based design with dependency injection for stampers, supporting extensibility for new stamp types.

## Dependencies
**Production Dependencies:**
- Spring Boot 2.x (framework)
- iText 7 (PDF manipulation)
- Apache PDFBox (PDF processing)
- RestTemplate (HTTP client)
- Lombok (code generation)
- Jackson (JSON processing)

**Development Dependencies:**
- JUnit 5 (testing)
- Mockito (mocking)
- Spring Boot Test (integration testing)
- Lombok (compile-time annotation processing)

---

# assumptions.md

# Assumptions & Inferences

## Purpose
This document lists assumptions made while analyzing the codebase. Validate with the development team.

## Project-Level Assumptions
- **Project Purpose:** A service for applying stamps (text, images, or HTML content) to PDF documents with configurable positioning and metadata handling.
  - Confidence: High
  - Evidence: Architecture map explicitly states this purpose, and all SRS requirements directly support this functionality

## Technical Assumptions
- **Spring Boot Version:** 2.x series
  - Confidence: Medium
  - Evidence: Architecture map references Spring Boot framework, and typical Spring Boot projects use v2.x or v3.x; no explicit version specified
- **iText Version:** 7.x series
  - Confidence: Medium
  - Evidence: Architecture map mentions "iText 7 pdfHTML", but doesn't specify exact version number
- **Apache PDFBox Version:** 2.x series
  - Confidence: Medium
  - Evidence: Architecture map implies usage but doesn't specify version
- **Maven/Gradle Build Tool:** Implied by standard Spring Boot project structure
  - Confidence: High
  - Evidence: Standard Spring Boot project layout and dependencies suggest Maven or Gradle
- **Lombok Usage:** Present in project structure
  - Confidence: High
  - Evidence: Architecture map references Lombok as a dependency, and code structure shows Lombok annotations
- **Testing Framework:** JUnit 5 + Mockito + Spring Boot Test
  - Confidence: Medium
  - Evidence: Test cases reference these frameworks, but not explicitly stated in architecture map

## Functional Assumptions
- **No User Roles or Authentication:** The system is publicly accessible with no authentication
  - Confidence: High
  - Evidence: Architecture map explicitly states "authModel: none", and SRS requirement US-013 confirms public API endpoints
- **File Upload Limits:** 50MB per file, 55MB total
  - Confidence: High
  - Evidence: Explicitly mentioned in SRS requirement US-010 and configuration section
- **PDF Processing Scope:** All operations work with standard PDF formats
  - Confidence: High
  - Evidence: Architecture map mentions iText 7 and Apache PDFBox, which support standard PDF formats
- **HTML Rendering:** Uses iText 7 pdfHTML for rendering with hyperlink preservation
  - Confidence: High
  - Evidence: Explicitly stated in architecture map and SRS requirement US-003

## Non-Functional Assumptions
- **Performance Expectation:** 10 seconds response time for typical PDF documents
  - Confidence: High
  - Evidence: Explicitly stated in NFR-001
- **Concurrency Support:** Up to 100 simultaneous requests
  - Confidence: High
  - Evidence: Explicitly stated in NFR-002
- **Memory Management:** Efficient processing without excessive consumption
  - Confidence: High
  - Evidence: Explicitly stated in NFR-007
- **Thread Safety:** Stateless components and thread-safe libraries
  - Confidence: High
  - Evidence: Explicitly stated in NFR-014

## Missing or Unclear Elements
- **Exact iText Version:** Not specified in architecture map or SRS
- **Exact Apache PDFBox Version:** Not specified in architecture map or SRS
- **Specific Spring Boot Version:** Not explicitly stated in architecture map
- **Build Tool Version:** Not specified (though implied)
- **Testing Coverage Details:** While test cases are provided, exact coverage percentage is unknown
- **Monitoring and Metrics:** No specific observability tools mentioned
- **Caching Strategy:** Not documented in architecture map or SRS
- **Logging Configuration Details:** Only general logging mentioned in NFR-010

## Recommendations for Validation
1. Confirm exact versions of iText 7 and Apache PDFBox being used in the implementation
2. Verify the specific Spring Boot version targeted by the build
3. Validate that the 50MB/55MB file size limits are actually enforced in the current implementation
4. Review the actual test coverage and confirm the test suite matches documented cases
5. Confirm whether any caching mechanisms are implemented for performance optimization
6. Validate the specific logging configuration and monitoring approach used in production
7. Verify that all documented error handling scenarios are actually implemented and tested
8. Confirm the exact deployment environment specifications (JVM version, OS, containerization if applicable)