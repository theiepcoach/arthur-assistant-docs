# Orvared PII Parsing Features - Lovable Prompts

Use these prompts in Lovable to implement document processing and PII detection features.

---

## PROMPT 1: Document Upload & PII Scanning Setup

```
I need to implement a document upload and PII scanning system for our Orvared special education advocacy platform. Here's the setup:

**Database Tables Already Exist:**
- documents (id, tenant_id, student_id, client_id, file_name, file_path, file_type, category, uploaded_by, created_at)
- We need a new table: document_scan_results

**Requirements:**
1. When a document is uploaded to the documents table, trigger an automatic PII scan
2. The scan should detect and flag these PII types:
   - Social Security Numbers (SSN): XXX-XX-XXXX pattern
   - Dates of Birth: MM/DD/YYYY or DOB patterns
   - Addresses: Street, Ave, Blvd patterns with numbers
   - Phone numbers: (XXX) XXX-XXXX or XXX-XXX-XXXX
   - Email addresses
   - Student names (in document text)
   - Parent/guardian names
   - Medical record numbers
   - District/school names

3. Create table: document_scan_results (
   - id UUID (primary key)
   - document_id UUID (foreign key to documents)
   - scan_status: 'pending', 'processing', 'completed', 'failed'
   - pii_detected: JSONB (list of detected PII with locations)
   - needs_review: BOOLEAN
   - scanned_at TIMESTAMPTZ
   - created_at TIMESTAMPTZ
)

4. Create edge function: process-document-scan (triggered on document insert)
   - Accepts document_id
   - Uses OCR or text extraction on PDF/image
   - Scans extracted text for PII patterns
   - Stores results in document_scan_results

5. Frontend: Add a "Scan for PII" button on document preview
6. Show scan results with highlighted PII locations

Use Supabase Edge Functions and implement proper error handling.
```

---

## PROMPT 2: Auto-Redaction Feature

```
I need to implement an automatic PII redaction feature for our document system:

**Current Setup:**
- documents table with file storage in Supabase Storage
- document_scan_results table tracks PII findings

**Requirements:**
1. Create an "Auto-Redact" button in the document view
2. When clicked, the system should:
   - Retrieve the document file
   - Parse the text content (OCR for PDFs/images)
   - Replace detected PII with [REDACTED] or mask the characters
   - Create a new "redacted" version of the document
   - Store both original and redacted versions

3. Redaction rules:
   - SSN: Replace with XXX-XX-XXXX (keep last 4)
   - DOB: Replace with MM/DD/YYYY0 (keep year if needed)
   - Phone: Replace with (XXX) XXX-XXXX (keep last 4)
   - Email: Replace username with XXX***
   - Names: Replace with [NAME REDACTED]

4. Create table: document_redaction_logs (
   - id UUID
   - document_id UUID
   - original_document_id UUID (the source)
   - redaction_type: 'auto', 'manual'
   - fields_redacted: JSONB
   - created_by UUID
   - created_at TIMESTAMPTZ
)

5. Store redacted files in Supabase Storage bucket: 'redacted-documents'
6. Show both original and redacted versions in document preview

Use edge functions for the redaction processing.
```

---

## PROMPT 3: PII Dashboard & Alerts

```
I need to create a PII Management Dashboard for our Orvared admin panel:

**Existing Tables:**
- documents
- document_scan_results  
- document_redaction_logs

**Requirements:**
1. Create new page: /admin/pii-dashboard

2. Show stats cards:
   - Total documents scanned
   - Documents with PII detected (needs review)
   - Documents redacted this month
   - Compliance score percentage

3. Documents needing review table:
   - Columns: Document Name, Student, PII Type Found, Scan Date, Actions
   - Actions: View Original, View Redacted, Approve, Reject
   - Filter by: scan_status, date_range, student

4. Alert system:
   - When new high-sensitivity PII detected (SSN, DOB), show banner
   - Email/in-app notification to admin
   - Daily digest option

5. Audit log view:
   - Who viewed/downloaded documents with PII
   - Who initiated redactions
   - Export audit log as CSV

6. Bulk actions:
   - Select multiple docs → "Redact All"
   - Select multiple docs → "Mark as Reviewed"

7. Settings page:
   - Enable/disable auto-scan on upload
   - Configure which PII types to flag
   - Set alert thresholds

Use existing design system and make it responsive.
```

---

## PROMPT 4: Privacy Policy & Consent Pages

```
I need to add privacy management pages to our Orvared application:

**Requirements:**
1. Create /privacy-policy page:
   - Explain data collection practices
   - FERPA best practices statement
   - Data encryption details
   - Third-party data handling
   - User rights (access, correction, deletion)
   - Contact information for privacy questions

2. Create /consent-settings page:
   - Show clients what data is collected
   - Toggle for marketing communications
   - Export my data button
   - Delete my data button (with confirmation)
   - Download data as JSON

3. Add to settings menu:
   - Privacy & Compliance section
   - Link to privacy policy
   - Link to consent settings
   - GDPR-style controls (even though not required)

4. Add consent tracking to database:
   - Table: user_consents (
     - id UUID
     - user_id UUID
     - consent_type: 'data_collection', 'marketing', 'analytics'
     - consented: BOOLEAN
     - consented_at TIMESTAMPTZ
     - ip_address TEXT
   )

5. Frontend: Show consent banner on first visit
   - Must accept before using app
   - Record consent with timestamp and IP

Use professional legal-looking design, include last updated date.
```

---

## PROMPT 5: Compliance Reporting

```
I need to generate a compliance reporting feature for our Orvared admin panel:

**Purpose:** Show clients that their data is being handled properly (FERPA best practices)

**Requirements:**
1. Create /admin/compliance-report page

2. Generate PDF compliance reports with:
   - Company name and logo
   - Report date range
   - Data handled summary
   - Security measures in place
   - Encryption status
   - Access audit summary
   - PII detection/reduction stats

3. Automated report generation:
   - Monthly reports (configurable)
   - On-demand report generation
   - Email report to admin

4. Compliance checklist view:
   - Show current compliance status for each area
   - Items: encryption, access controls, audit logs, backup, etc.
   - Show green/red status for each

5. Certifications section:
   - Display any current certifications (SOC2, etc.)
   - Show "FERPA Best Practice" badge if all checks pass

6. Export options:
   - Download as PDF
   - Download as CSV
   - Email to stakeholders

This is a premium feature - ensure it looks professional and trustworthy.
```

---

## Technical Implementation Notes

### Supabase Edge Functions Needed:
1. `process-document-scan` - Triggered on document upload
2. `redact-document` - Process and redact PII
3. `generate-compliance-report` - Create PDF reports

### External Services to Consider:
- **OCR:** Supabase uses pg_search for text search, but for PDFs/images need external service like:
  - Document AI (Google Cloud)
  - AWS Textract
  - Azure Form Recognizer
  - Or free: Tesseract.js (client-side, limited)

### Storage Buckets:
- `documents` - Original uploads
- `redacted-documents` - Redacted versions
- `compliance-reports` - Generated reports

---

## Testing Checklist

Before launching PII features, verify:
- [ ] SSN detection works (XXX-XX-XXXX pattern)
- [ ] DOB detection works  
- [ ] Auto-redaction preserves document formatting
- [ ] Original and redacted versions both viewable
- [ ] Audit logs capture all document access
- [ ] Privacy policy page renders correctly
- [ ] Consent banner appears on first visit
- [ ] Data export includes all user data
- [ ] Data deletion removes all user data

---

Would you like me to generate the SQL for any of these database tables or edge function code?