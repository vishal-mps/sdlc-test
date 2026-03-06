# Technical Specifications

## System Architecture

The PDF Stamping Service is a Spring Boot application built on the JVM that provides functionality for applying stamps (text, images, or HTML content) to PDF documents. The system follows a layered architecture with clear separation of concerns:

- **Presentation Layer**: REST API controllers (`StampController`) handle incoming HTTP requests and coordinate with services
- **Service Layer**: Core business logic resides in service classes (`StampService`, `MetadataFrontPageService`, `AdStampService`, `AdFetchService`) that orchestrate stamping operations
- **Stamper Layer**: Specialized implementations (`TextStamper`, `ImageStamper`, `HtmlStamper`, `PdfStamper`) handle specific stamping techniques
- **Model Layer**: Data transfer objects and enums define the structure of requests, responses, and configuration parameters
- **Exception Handling**: Global exception handler (`GlobalExceptionHandler`) standardizes error responses
- **External Integration**: Uses iTextPDF libraries for PDF manipulation and RestTemplate for external API calls

Components communicate through well-defined interfaces and data models. The system processes PDF documents by reading input bytes, applying configured stamps using specialized stamper implementations, and returning modified PDF bytes. The architecture supports both file path-based operations and multipart file uploads for flexibility in deployment environments.

## Data Models

### StampType
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| TEXT | enum value | - | Text-based stamping |
| IMAGE | enum value | - | Image-based stamping |
| HTML | enum value | - | HTML content stamping |
| PDF | enum value | - | PDF overlay stamping |

**Relationships:**
- Used by `StampRequest` to specify stamping method
- Referenced by `StampService` to route to appropriate stamper

### StampResponse
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| success | boolean | - | Indicates operation success |
| message | String | - | Status message |
| outputPath | String | - | Path to output file |
| fileSizeBytes | long | - | Size of output file in bytes |

**Relationships:**
- Returned by stamping operations in `StampController`
- Used in file path stamping workflows

### StampRequest
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| type | StampType | required | Type of stamp to apply |
| position | StampPosition | required | Position on page |
| x | double | optional | Custom X coordinate |
| y | double | optional | Custom Y coordinate |
| opacity | float | optional | Stamp transparency (0.0-1.0) |
| rotation | double | optional | Rotation angle in degrees |
| scale | double | optional | Scale factor |
| pages | String | optional | Page specification |
| fontSize | int | optional | Font size for text stamps |
| fontColor | String | optional | Font color for text stamps |
| text | String | optional | Text content for text stamps |
| imageUrl | String | optional | Image URL for image stamps |
| htmlContent | String | optional | HTML content for HTML stamps |
| stampPdfPath | String | optional | Path to stamp PDF for PDF stamps |

**Relationships:**
- Input model for stamping operations
- Used by `StampService` to configure stamping behavior

### StampPosition
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| TOP_LEFT | enum value | - | Top left corner |
| TOP_CENTER | enum value | - | Top center |
| TOP_RIGHT | enum value | - | Top right corner |
| CENTER_LEFT | enum value | - | Center left |
| CENTER | enum value | - | Center |
| CENTER_RIGHT | enum value | - | Center right |
| BOTTOM_LEFT | enum value | - | Bottom left corner |
| BOTTOM_CENTER | enum value | - | Bottom center |
| BOTTOM_RIGHT | enum value | - | Bottom right corner |
| CUSTOM | enum value | - | Custom coordinates |

**Relationships:**
- Used by `StampRequest` to specify stamp placement
- Referenced by stamper implementations for positioning logic

### MetadataFrontPageRequest
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| logoText | String | optional | Logo text to display |
| articleTitle | String | required | Article title |
| authors | List<String> | optional | List of author names |
| journalCode | String | optional | Journal identifier |
| citationInfo | String | optional | Citation information |
| doi | String | optional | Digital Object Identifier |
| includeDate | boolean | optional | Whether to include current date |
| includeDoi | boolean | optional | Whether to include DOI |

**Relationships:**
- Input model for metadata front page generation
- Used by `MetadataFrontPageService`

### FilePathStampRequest
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| inputPath | String | required | Path to input PDF |
| outputPath | String | required | Path for output PDF |
| type | StampType | required | Type of stamp to apply |
| position | StampPosition | required | Position on page |
| x | double | optional | Custom X coordinate |
| y | double | optional | Custom Y coordinate |
| opacity | float | optional | Stamp transparency (0.0-1.0) |
| rotation | double | optional | Rotation angle in degrees |
| scale | double | optional | Scale factor |
| pages | String | optional | Page specification |
| fontSize | int | optional | Font size for text stamps |
| fontColor | String | optional | Font color for text stamps |
| text | String | optional | Text content for text stamps |
| imageUrl | String | optional | Image URL for image stamps |
| htmlContent | String | optional | HTML content for HTML stamps |
| stampPdfPath | String | optional | Path to stamp PDF for PDF stamps |

**Relationships:**
- Input model for file path-based stamping operations
- Used by `StampController` for file path operations

### DynamicStampRequest
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| publisherId | String | required | Publisher identifier |
| journalCode | String | required | Journal code |
| strategy | String | required | Injection strategy |
| configuration | Configuration | required | Stamp configuration |

**Configuration Inner Class Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| position | StampPosition | optional | Position on page |
| alignment | String | optional | Alignment for text |
| addLogo | boolean | optional | Whether to add logo |
| logoText | String | optional | Logo text |
| addText | boolean | optional | Whether to add custom text |
| customText | String | optional | Custom text content |
| addHtml | boolean | optional | Whether to add HTML |
| htmlContent | String | optional | HTML content |
| addDoi | boolean | optional | Whether to add DOI |
| addDate | boolean | optional | Whether to add date |
| isAd | boolean | optional | Whether to include advertisement |
| adLink | String | optional | Advertisement link |

**Relationships:**
- Input model for dynamic stamping operations
- Used by `StampController` for dynamic stamping

### AdJsonRequest
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| inputPath | String | required | Path to input PDF |
| outputPath | String | optional | Path for output PDF |
| adType | String | required | Type of advertisement |
| adJsonUrl | String | required | URL to advertisement JSON |

**Relationships:**
- Input model for advertisement JSON stamping
- Used by `StampController` for ad stamping operations

### Section
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | - | Section identifier |
| path | String | - | Navigation path |
| locations | List<AdLocation> | - | Advertisement locations |

**Relationships:**
- Part of `AdResponse` structure
- Represents advertising sections in the system

### AdResponse
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| publisherId | String | - | Publisher identifier |
| journlcode | String | - | Journal code |
| sections | List<Section> | - | Advertising sections |

**Relationships:**
- Output model for advertisement fetching
- Used by `AdFetchService` and `AdStampService`

### AdLocation
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| positionId | String | - | Position identifier |
| positionName | String | - | Position name |
| adData | AdData | - | Advertisement data |

**Relationships:**
- Part of `Section` structure
- Represents individual advertisement placements

### AdData
**Fields:**
| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| adString | String | - | Advertisement text |
| adHtml | String | - | Advertisement HTML |
| adId | String | - | Advertisement identifier |
| startDate | String | - | Start date |
| endDate | String | - | End date |

**Relationships:**
- Part of `AdLocation` structure
- Contains advertisement content and metadata

## API Contracts

### Stamp PDF with Configuration
**Method:** POST /api/v1/stamp
**Auth:** Not Required
**Input:**
```json
{
  "type": "TEXT|IMAGE|HTML|PDF",
  "position": "TOP_LEFT|TOP_CENTER|TOP_RIGHT|CENTER_LEFT|CENTER|CENTER_RIGHT|BOTTOM_LEFT|BOTTOM_CENTER|BOTTOM_RIGHT|CUSTOM",
  "x": 0.0,
  "y": 0.0,
  "opacity": 0.0,
  "rotation": 0.0,
  "scale": 0.0,
  "pages": "ALL|FIRST|LAST|1,3,5-7",
  "fontSize": 0,
  "fontColor": "string",
  "text": "string",
  "imageUrl": "string",
  "htmlContent": "string",
  "stampPdfPath": "string"
}
```
**Output:**
```json
{
  "success": true,
  "message": "string",
  "outputPath": "string",
  "fileSizeBytes": 0
}
```
**Error Responses:**
- 400: Invalid request parameters
- 413: File size exceeds limit
- 500: Internal server error during stamping

**Business Logic:**
1. Validate request parameters
2. Parse page selection expressions
3. Select appropriate stamper based on stamp type
4. Apply stamp to specified pages
5. Return stamped PDF as attachment

### Stamp PDF with File Paths
**Method:** POST /api/v1/stamp/file-path
**Auth:** Not Required
**Input:**
```json
{
  "inputPath": "string",
  "outputPath": "string",
  "type": "TEXT|IMAGE|HTML|PDF",
  "position": "TOP_LEFT|TOP_CENTER|TOP_RIGHT|CENTER_LEFT|CENTER|CENTER_RIGHT|BOTTOM_LEFT|BOTTOM_CENTER|BOTTOM_RIGHT|CUSTOM",
  "x": 0.0,
  "y": 0.0,
  "opacity": 0.0,
  "rotation": 0.0,
  "scale": 0.0,
  "pages": "ALL|FIRST|LAST|1,3,5-7",
  "fontSize": 0,
  "fontColor": "string",
  "text": "string",
  "imageUrl": "string",
  "htmlContent": "string",
  "stampPdfPath": "string"
}
```
**Output:**
```json
{
  "success": true,
  "message": "string",
  "outputPath": "string",
  "fileSizeBytes": 0
}
```
**Error Responses:**
- 400: Invalid request parameters
- 404: Input file not found
- 500: Internal server error during stamping

**Business Logic:**
1. Validate file paths exist
2. Parse request parameters
3. Apply stamp to input PDF
4. Write result to output path
5. Return success response

### Dynamic Stamp Configuration
**Method:** POST /api/v1/stamp/dynamic
**Auth:** Not Required
**Input:**
```json
{
  "publisherId": "string",
  "journalCode": "string",
  "strategy": "string",
  "configuration": {
    "position": "TOP_LEFT|TOP_CENTER|TOP_RIGHT|CENTER_LEFT|CENTER|CENTER_RIGHT|BOTTOM_LEFT|BOTTOM_CENTER|BOTTOM_RIGHT|CUSTOM",
    "alignment": "string",
    "addLogo": true,
    "logoText": "string",
    "addText": true,
    "customText": "string",
    "addHtml": true,
    "htmlContent": "string",
    "addDoi": true,
    "addDate": true,
    "isAd": true,
    "adLink": "string"
  }
}
```
**Output:**
```json
{
  "success": true,
  "message": "string",
  "outputPath": "string",
  "fileSizeBytes": 0
}
```
**Error Responses:**
- 400: Invalid configuration
- 500: Internal server error during stamping

**Business Logic:**
1. Validate dynamic configuration
2. Process each configuration element
3. Apply appropriate stamps to PDF
4. Return stamped PDF as attachment

### Fetch Advertisement Data
**Method:** POST /api/v1/stamp/ad-json
**Auth:** Not Required
**Input:**
```json
{
  "inputPath": "string",
  "outputPath": "string",
  "adType": "string",
  "adJsonUrl": "string"
}
```
**Output:**
```json
{
  "success": true,
  "message": "string",
  "outputPath": "string",
  "fileSizeBytes": 0
}
```
**Error Responses:**
- 400: Invalid request parameters
- 500: Internal server error during ad processing

**Business Logic:**
1. Fetch advertisement data from JSON URL
2. Validate ad data structure
3. Apply advertisement to PDF
4. Return stamped PDF as attachment

### Add Metadata Front Page
**Method:** POST /api/v1/metadata/front-page
**Auth:** Not Required
**Input:**
```json
{
  "logoText": "string",
  "articleTitle": "string",
  "authors": ["string"],
  "journalCode": "string",
  "citationInfo": "string",
  "doi": "string",
  "includeDate": true,
  "includeDoi": true
}
```
**Output:**
```json
{
  "success": true,
  "message": "string",
  "outputPath": "string",
  "fileSizeBytes": 0
}
```
**Error Responses:**
- 400: Invalid metadata parameters
- 500: Internal server error during metadata processing

**Business Logic:**
1. Generate HTML front page with metadata
2. Convert HTML to PDF
3. Prepend metadata PDF to original PDF
4. Return combined PDF as attachment

## Key Algorithms & Business Logic

### Page Selection Algorithm
The system uses `PageSelector.parsePages()` to convert human-readable page expressions into 0-based page indices:
1. Parse expression string (e.g., "ALL", "FIRST", "LAST", "1,3,5-7")
2. Handle range notation (e.g., "5-7" becomes [5,6,7])
3. Validate page numbers against total page count
4. Return set of valid page indices

### Stamp Position Calculation
For predefined positions, the system calculates coordinates based on:
1. Page dimensions (width and height)
2. Position type (TOP_LEFT, CENTER, etc.)
3. Padding offsets for margins
4. Custom positioning when position = CUSTOM

### PDF Processing Flow
1. Read input PDF bytes
2. Parse stamp configuration from request
3. Determine target pages using PageSelector
4. Apply stamp using appropriate stamper implementation
5. Handle transformations (scale, rotate, opacity)
6. Return modified PDF bytes

### Advertisement Processing Flow
1. Fetch ad data from JSON endpoint using AdFetchService
2. Validate ad response structure
3. For header ads: apply HTML stamp to existing pages
4. For PDF ads: append new page with ad content
5. Merge modified PDF with original

### Error Handling Strategy
The system implements centralized error handling through:
1. GlobalExceptionHandler catching specific exceptions
2. StampingException for domain-specific errors
3. Proper HTTP status codes for different error types
4. Consistent error response format with timestamp and message

## External Dependencies

| Dependency | Purpose | Version |
|-----------|---------|---------|
| Spring Boot | Application framework | 2.x |
| iText 7 | PDF manipulation | 7.x |
| Apache PDFBox | PDF processing | 2.x |
| RestTemplate | HTTP client | Spring Boot included |
| Lombok | Code generation | 1.x |
| Jackson | JSON processing | Spring Boot included |

## Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| server.port | integer | 8080 | HTTP server port |
| spring.servlet.multipart.max-file-size | string | 50MB | Maximum file size per upload |
| spring.servlet.multipart.max-request-size | string | 55MB | Maximum request size |
| temp.dir | string | system temp dir | Temporary directory for processing |
| default.font.size | integer | 12 | Default font size for text stamps |
| default.opacity | float | 1.0 | Default opacity for stamps |
| default.rotation | double | 0.0 | Default rotation angle |
| default.scale | double | 1.0 | Default scale factor |

## Security Considerations

### Authentication & Authorization
- No authentication required for API endpoints
- All endpoints are publicly accessible
- No role-based access control implemented

### Input Validation
- Request parameters validated for type and range
- File size limits enforced to prevent DoS attacks
- Page selection expressions sanitized to prevent invalid indices
- HTML content sanitized to prevent XSS vulnerabilities

### Data Protection
- No sensitive data stored in memory
- Files processed in temporary directories
- No persistent storage of user data
- PDF content remains in memory during processing

### File Handling Security
- File paths validated to prevent directory traversal
- Upload limits prevent excessive resource consumption
- Temporary files cleaned up after processing
- No direct file system access exposed through API

## Implementation Notes

### Design Patterns
- Strategy pattern used in stamper implementations
- Factory pattern for selecting appropriate stamper
- Builder pattern for complex request objects
- Template method