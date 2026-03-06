# Comprehensive Test Cases for PDF Stamping Service

## Unit Tests

### TC-U-001: TextStamper Apply Text Stamp to PDF
**Component:** TextStamper.stamp()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid StampRequest with TEXT type
- Text content provided

**Test Steps:**
1. Create a valid StampRequest with TEXT type and position
2. Call TextStamper.stamp() with PDF bytes and StampRequest
3. Verify output bytes are not null or empty

**Input Data:**
```json
{
  "type": "TEXT",
  "position": "TOP_LEFT",
  "content": "Watermark",
  "fontSize": 12,
  "opacity": 0.5,
  "rotation": 0,
  "scale": 1.0,
  "pages": "ALL"
}
```

**Expected Result:** Non-null output PDF bytes with text stamp applied

**Assertions:**
- Output bytes length > 0
- No exceptions thrown
- Text appears at specified position

### TC-U-002: ImageStamper Apply Image Stamp to PDF
**Component:** ImageStamper.stamp()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid StampRequest with IMAGE type
- Valid image bytes provided

**Test Steps:**
1. Create a valid StampRequest with IMAGE type and position
2. Call ImageStamper.stamp() with PDF bytes and StampRequest
3. Verify output bytes are not null or empty

**Input Data:**
```json
{
  "type": "IMAGE",
  "position": "BOTTOM_RIGHT",
  "content": "<base64_image_bytes>",
  "opacity": 0.8,
  "rotation": 45,
  "scale": 0.5,
  "pages": "1"
}
```

**Expected Result:** Non-null output PDF bytes with image stamp applied

**Assertions:**
- Output bytes length > 0
- No exceptions thrown
- Image appears at specified position with correct rotation and scale

### TC-U-003: HtmlStamper Apply HTML Stamp to PDF
**Component:** HtmlStamper.stamp()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid StampRequest with HTML type
- Valid HTML content provided

**Test Steps:**
1. Create a valid StampRequest with HTML type and position
2. Call HtmlStamper.stamp() with PDF bytes and StampRequest
3. Verify output bytes are not null or empty

**Input Data:**
```json
{
  "type": "HTML",
  "position": "CENTER",
  "content": "<p>HTML Content</p>",
  "opacity": 0.7,
  "rotation": 0,
  "scale": 1.0,
  "pages": "ALL"
}
```

**Expected Result:** Non-null output PDF bytes with HTML content rendered as stamp

**Assertions:**
- Output bytes length > 0
- No exceptions thrown
- HTML content rendered correctly on PDF

### TC-U-004: PdfStamper Overlay PDF Stamp
**Component:** PdfStamper.stamp()
**Priority:** High

**Preconditions:**
- Valid source PDF bytes
- Valid stamp PDF bytes
- Valid StampRequest with PDF type

**Test Steps:**
1. Create a valid StampRequest with PDF type and position
2. Call PdfStamper.stamp() with source PDF and stamp PDF bytes
3. Verify output bytes are not null or empty

**Input Data:**
```json
{
  "type": "PDF",
  "position": "TOP_CENTER",
  "content": "<pdf_bytes>",
  "opacity": 0.6,
  "rotation": 180,
  "scale": 0.8,
  "pages": "1-3"
}
```

**Expected Result:** Non-null output PDF bytes with overlay PDF stamp applied

**Assertions:**
- Output bytes length > 0
- No exceptions thrown
- Stamp PDF overlaid correctly on specified pages

### TC-U-005: PageSelector Parse Page Expressions
**Component:** PageSelector.parsePages()
**Priority:** Medium

**Preconditions:**
- Valid page expression string
- Total page count

**Test Steps:**
1. Call PageSelector.parsePages("ALL", 5)
2. Call PageSelector.parsePages("1,3,5", 5)
3. Call PageSelector.parsePages("2-4", 5)

**Expected Result:** Sets of page indices matching expressions

**Assertions:**
- parsePages("ALL", 5) returns {0,1,2,3,4}
- parsePages("1,3,5", 5) returns {0,2,4}
- parsePages("2-4", 5) returns {1,2,3}

### TC-U-006: StampService Apply Different Stamp Types
**Component:** StampService.applyStamp()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid StampRequest with different types

**Test Steps:**
1. Create StampRequest with TEXT type
2. Call StampService.applyStamp() with TEXT request
3. Repeat with IMAGE, HTML, and PDF types

**Expected Result:** Successful stamping for all types

**Assertions:**
- All stamp types return non-null output bytes
- No exceptions thrown for any type
- Each type applies correctly according to specification

### TC-U-007: MetadataFrontPageService Add Metadata Front Page
**Component:** MetadataFrontPageService.addMetadataFrontPage()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid MetadataFrontPageRequest

**Test Steps:**
1. Create MetadataFrontPageRequest with metadata fields
2. Call MetadataFrontPageService.addMetadataFrontPage() with PDF bytes
3. Verify output PDF has additional page

**Input Data:**
```json
{
  "articleTitle": "Test Article",
  "authors": ["Author One", "Author Two"],
  "logoText": "Journal Logo",
  "doi": "10.1234/test",
  "citation": "Test Citation"
}
```

**Expected Result:** PDF with additional metadata front page

**Assertions:**
- Output PDF has one more page than input
- Metadata front page contains all requested fields
- No exceptions thrown

### TC-U-008: AdFetchService Fetch Advertisement Data
**Component:** AdFetchService.fetchAds()
**Priority:** Medium

**Preconditions:**
- Valid JSON URL endpoint
- Mocked HTTP response

**Test Steps:**
1. Configure mock HTTP server with sample ad JSON
2. Call AdFetchService.fetchAds() with URL
3. Validate returned AdResponse object

**Input Data:**
```json
{
  "publisherId": "test_publisher",
  "journlcode": "TEST001",
  "sections": [
    {
      "id": "section1",
      "path": "/section1",
      "locations": [
        {
          "positionId": "header",
          "positionName": "Header Ad",
          "adData": {
            "adString": "Test Ad",
            "adHtml": "<div>Test Ad HTML</div>"
          }
        }
      ]
    }
  ]
}
```

**Expected Result:** Valid AdResponse object with populated fields

**Assertions:**
- Publisher ID matches input
- Sections list populated
- Location ad data correctly mapped
- No exceptions thrown

### TC-U-009: AdStampService Process Header Ad
**Component:** AdStampService.processHeaderAd()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid AdResponse with header ad data

**Test Steps:**
1. Create AdResponse with header ad
2. Call AdStampService.processHeaderAd() with PDF bytes
3. Verify output PDF contains ad stamp

**Expected Result:** PDF with header ad stamp applied

**Assertions:**
- Output bytes length > 0
- Header ad appears correctly positioned
- No exceptions thrown

### TC-U-0010: AdStampService Append PDF Ad Page
**Component:** AdStampService.appendPdfAdPage()
**Priority:** High

**Preconditions:**
- Valid PDF bytes input
- Valid AdResponse with PDF ad location

**Test Steps:**
1. Create AdResponse with PDF ad location
2. Call AdStampService.appendPdfAdPage() with PDF bytes
3. Verify output PDF has additional page

**Expected Result:** PDF with additional page containing ad content

**Assertions:**
- Output PDF has one more page than input
- New page contains ad content
- No exceptions thrown

## Integration Tests

### TC-I-001: StampController Handle Text Stamp Request
**Components:** StampController, StampService, TextStamper
**Priority:** High

**Preconditions:**
- Running Spring Boot application
- Valid PDF file uploaded
- Text stamp configuration provided

**Test Steps:**
1. Send POST request to /api/v1/stamp with text stamp data
2. Wait for response
3. Verify response contains stamped PDF

**Expected Result:** Successful HTTP 200 response with stamped PDF attachment

**Assertions:**
- HTTP status 200 OK
- Response contains PDF content
- Content-Type header set to application/pdf
- File size greater than original

### TC-I-002: StampController Handle Dynamic Stamp Request
**Component:** StampController, DynamicStampService
**Priority:** High

**Preconditions:**
- Running Spring Boot application
- Valid PDF file uploaded
- Dynamic stamp configuration provided

**Test Steps:**
1. Send POST request to /api/v1/stamp/dynamic with configuration
2. Wait for response
3. Verify response contains stamped PDF

**Expected Result:** Successful HTTP 200 response with stamped PDF attachment

**Assertions:**
- HTTP status 200 OK
- Response contains PDF content
- Content-Type header set to application/pdf
- File size greater than original

### TC-I-003: GlobalExceptionHandler Handle StampingException
**Components:** GlobalExceptionHandler, StampingException
**Priority:** Medium

**Preconditions:**
- Running Spring Boot application
- Controller throws StampingException

**Test Steps:**
1. Send request that triggers StampingException
2. Wait for response
3. Verify error response format

**Expected Result:** HTTP 400 response with structured error JSON

**Assertions:**
- HTTP status 400 Bad Request
- Response contains timestamp
- Response contains error description
- Response contains custom message

### TC-I-004: StampController Handle File Path Stamp Request
**Component:** StampController, FilePathStampService
**Priority:** Medium

**Preconditions:**
- Running Spring Boot application
- Valid file paths configured
- Valid stamp configuration provided

**Test Steps:**
1. Send POST request to /api/v1/stamp/file-path with file paths
2. Wait for response
3. Verify response contains stamped PDF

**Expected Result:** Successful HTTP 200 response with stamped PDF attachment

**Assertions:**
- HTTP status 200 OK
- Response contains PDF content
- Content-Type header set to application/pdf
- File size greater than original

## End-to-End Tests

### TC-E-001: Complete Text Watermarking Workflow
**Scenario:** User wants to add a text watermark to a PDF
**Priority:** High

**Test Steps:**
1. User uploads PDF file
2. User selects text watermark option
3. User enters watermark text and settings
4. User submits stamping request
5. System processes and returns stamped PDF

**Expected Result:** Successfully stamped PDF with text watermark applied to all pages

**Assertions:**
- PDF file is processed without errors
- Text watermark appears on all pages
- Watermark properties match user settings
- Output file is downloadable

### TC-E-002: Complete HTML Stamp Workflow
**Scenario:** User wants to add HTML content as stamp to PDF
**Priority:** High

**Test Steps:**
1. User uploads PDF file
2. User selects HTML stamp option
3. User enters HTML content and positioning
4. User submits stamping request
5. System processes and returns stamped PDF

**Expected Result:** Successfully stamped PDF with HTML content rendered as stamp

**Assertions:**
- PDF file is processed without errors
- HTML content renders correctly on PDF
- Positioning matches user settings
- Output file is downloadable

### TC-E-003: Complete Advertisement Integration Workflow
**Scenario:** User wants to add advertisements to PDF using JSON endpoint
**Priority:** High

**Test Steps:**
1. User uploads PDF file
2. User selects advertisement option
3. User provides JSON URL for ads
4. User submits stamping request
5. System fetches ads and applies them

**Expected Result:** Successfully stamped PDF with advertisements applied

**Assertions:**
- Ads are fetched from JSON endpoint
- Advertisements are applied correctly
- PDF is processed without errors
- Output file is downloadable

### TC-E-004: Complete Metadata Front Page Workflow
**Scenario:** User wants to add metadata front page to PDF
**Priority:** High

**Test Steps:**
1. User uploads PDF file
2. User selects metadata option
3. User enters metadata fields
4. User submits stamping request
5. System processes and returns stamped PDF

**Expected Result:** Successfully stamped PDF with metadata front page added

**Assertions:**
- Metadata front page is added to PDF
- All metadata fields appear correctly
- PDF has additional page
- Output file is downloadable

## Error & Edge Case Tests

### TC-ERR-001: Invalid PDF File Upload
**Scenario:** User uploads corrupted or invalid PDF file
**Priority:** High

**Test Steps:**
1. Upload corrupted PDF file
2. Submit stamping request
3. Observe system response

**Expected Result:** Appropriate error response with descriptive message

**Assertions:**
- HTTP status 400 or 500
- Error message indicates invalid PDF
- No crash or unhandled exception
- System logs error appropriately

### TC-ERR-002: Missing Required Fields in Request
**Scenario:** User submits stamping request with missing required fields
**Priority:** High

**Test Steps:**
1. Send stamping request without required fields
2. Observe system response

**Expected Result:** Validation error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message identifies missing fields
- No processing occurs
- Response contains validation details

### TC-ERR-003: Invalid Page Range Specification
**Scenario:** User specifies invalid page range in stamp request
**Priority:** Medium

**Test Steps:**
1. Submit stamping request with invalid page range (e.g., "abc")
2. Observe system response

**Expected Result:** Appropriate error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message indicates invalid page range
- No processing occurs
- System handles gracefully

### TC-ERR-004: Exceed Maximum File Size Limit
**Scenario:** User uploads file exceeding configured limit (50MB)
**Priority:** High

**Test Steps:**
1. Upload file larger than 50MB limit
2. Submit stamping request
3. Observe system response

**Expected Result:** File size exceeded error response

**Assertions:**
- HTTP status 413 Payload Too Large
- Error message indicates file size limit exceeded
- No processing occurs
- System logs appropriate warning

### TC-ERR-005: Invalid Stamp Type Provided
**Scenario:** User specifies unsupported stamp type
**Priority:** Medium

**Test Steps:**
1. Submit stamping request with invalid stamp type
2. Observe system response

**Expected Result:** Validation error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message indicates invalid stamp type
- No processing occurs
- System handles gracefully

### TC-ERR-006: Invalid Position Value
**Scenario:** User specifies invalid stamp position
**Priority:** Medium

**Test Steps:**
1. Submit stamping request with invalid position value
2. Observe system response

**Expected Result:** Validation error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message indicates invalid position
- No processing occurs
- System handles gracefully

### TC-ERR-007: Network Failure During Ad Fetch
**Scenario:** External ad service unavailable during ad fetching
**Priority:** Medium

**Test Steps:**
1. Configure ad service to return timeout/error
2. Submit stamping request with ad configuration
3. Observe system response

**Expected Result:** Appropriate error response indicating network issue

**Assertions:**
- HTTP status 500 Internal Server Error
- Error message indicates network failure
- System logs error appropriately
- No partial processing occurs

### TC-ERR-008: Invalid HTML Content in Stamp
**Scenario:** User provides malformed HTML content for stamp
**Priority:** Medium

**Test Steps:**
1. Submit stamping request with malformed HTML
2. Observe system response

**Expected Result:** Processing error response

**Assertions:**
- HTTP status 400 Bad Request or 500 Internal Server Error
- Error message indicates HTML parsing issue
- No processing occurs
- System handles gracefully

### TC-ERR-009: Empty Input File
**Scenario:** User uploads empty PDF file
**Priority:** Medium

**Test Steps:**
1. Upload empty PDF file
2. Submit stamping request
3. Observe system response

**Expected Result:** Appropriate error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message indicates empty file
- No processing occurs
- System handles gracefully

### TC-ERR-010: Invalid Rotation Angle
**Scenario:** User specifies rotation angle outside valid range
**Priority:** Medium

**Test Steps:**
1. Submit stamping request with rotation > 360 degrees
2. Observe system response

**Expected Result:** Validation error response

**Assertions:**
- HTTP status 400 Bad Request
- Error message indicates invalid rotation
- No processing occurs
- System handles gracefully