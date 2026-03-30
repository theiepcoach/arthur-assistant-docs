# PII Redaction Layer
# Protects sensitive information before AI processing
# Last Updated: 2026-03-30

---

## Overview

Before any document is sent to the AI, we must strip all personally identifiable information (PII). This layer ensures:
- Student names are removed
- SSNs are redacted
- Dates of birth are removed
- Addresses are stripped
- Phone numbers are redacted
- Email addresses are removed
- Student IDs are removed

**Goal:** The AI never sees identifiable information about the student.

---

## PII Detection Patterns

### 1. Social Security Numbers
```
Pattern: \b\d{3}-\d{2}-\d{4}\b
Format: XXX-XX-XXXX
Replacement: [SSN REDACTED]
```

### 2. Dates of Birth
```
Patterns:
- \b\d{1,2}/\d{1,2}/\d{2,4}\b (MM/DD/YYYY)
- \b\d{1,2}-\d{1,2}-\d{2,4}\b (MM-DD-YYYY)
- \b\d{1,2}\.\d{1,2}\.\d{2,4}\b (MM.DD.YYYY)
- \b(January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s+\d{4}\b
Replacement: [DOB REDACTED]
```

### 3. Phone Numbers
```
Patterns:
- \b\d{3}-\d{3}-\d{4}\b (XXX-XXX-XXXX)
- \b\(\d{3}\)\s*\d{3}-\d{4}\b ((XXX) XXX-XXXX)
- \b\d{3}\.\d{3}\.\d{4}\b (XXX.XXX.XXXX)
- \b\+1\s*\d{3}\s*\d{3}\s*\d{4}\b (International)
Replacement: [PHONE REDACTED]
```

### 4. Email Addresses
```
Pattern: \b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b
Replacement: [EMAIL REDACTED]
```

### 5. Addresses
```
Patterns to detect:
- \b\d+\s+[A-Z][a-z]+(\s+[A-Z][a-z]+){1,3}\s+(Street|St|Avenue|Ave|Road|Rd|Boulevard|Blvd|Drive|Dr|Lane|Ln|Way|Court|Ct|Place|Pl)\b
- \b[A-Z][a-z]+,\s*[A-Z]{2}\s+\d{5}(-\d{4})?\b (City, State ZIP)
Replacement: [ADDRESS REDACTED]
```

### 6. Student IDs
```
Patterns:
- \bSID\d+\b
- \bStudent\s*ID:?\s*\d+\b
- \bID\d{6,10}\b
Replacement: [STUDENT ID REDACTED]
```

### 7. Parent/Guardian Names
```
Common contexts:
- "Parent/Guardian:" or "P/G:"
- "Mother:" / "Father:"
- "Guardian:" followed by name
Replacement: [PARENT NAME REDACTED]
```

### 8. Medical/Health Information
```
Patterns:
- Diagnosis codes (ICD-10 starting with letters)
- Medical record numbers
- Insurance IDs
Replacement: [MEDICAL INFO REDACTED]
```

---

## Implementation: Rule-Based Redaction (Fast)

```typescript
// pii-redaction.ts

interface RedactionResult {
  redactedText: string;
  redactedItems: RedactedItem[];
}

interface RedactionItem {
  type: string;
  original: string;
  replacement: string;
  position: number;
}

export function redactPII(text: string): RedactionResult {
  const redactedItems: RedactedItem[] = [];
  
  // Social Security Numbers
  const ssnPattern = /\b\d{3}-\d{2}-\d{4}\b/g;
  text = text.replace(ssnPattern, (match, offset) => {
    redactedItems.push({
      type: 'SSN',
      original: match,
      replacement: '[SSN REDACTED]',
      position: offset
    });
    return '[SSN REDACTED]';
  });

  // Dates of Birth (multiple formats)
  const dobPatterns = [
    /\b\d{1,2}\/\d{1,2}\/\d{2,4}\b/g,
    /\b\d{1,2}-\d{1,2}-\d{2,4}\b/g,
    /\b(January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s+\d{4}\b/gi
  ];
  
  dobPatterns.forEach(pattern => {
    text = text.replace(pattern, (match, offset) => {
      redactedItems.push({
        type: 'DOB',
        original: match,
        replacement: '[DOB REDACTED]',
        position: offset
      });
      return '[DOB REDACTED]';
    });
  });

  // Phone Numbers
  const phonePatterns = [
    /\b\d{3}-\d{3}-\d{4}\b/g,
    /\b\(\d{3}\)\s*\d{3}-\d{4}\b/g,
    /\b\d{3}\.\d{3}\.\d{4}\b/g
  ];
  
  phonePatterns.forEach(pattern => {
    text = text.replace(pattern, (match, offset) => {
      redactedItems.push({
        type: 'PHONE',
        original: match,
        replacement: '[PHONE REDACTED]',
        position: offset
      });
      return '[PHONE REDACTED]';
    });
  });

  // Email Addresses
  const emailPattern = /\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b/g;
  text = text.replace(emailPattern, (match, offset) => {
    redactedItems.push({
      type: 'EMAIL',
      original: match,
      replacement: '[EMAIL REDACTED]',
      position: offset
    });
    return '[EMAIL REDACTED]';
  });

  // Student IDs
  const studentIdPatterns = [
    /\bSID\d+\b/gi,
    /\bStudent\s*ID:?\s*\d+\b/gi,
    /\bID\d{6,10}\b/g
  ];
  
  studentIdPatterns.forEach(pattern => {
    text = text.replace(pattern, (match, offset) => {
      redactedItems.push({
        type: 'STUDENT_ID',
        original: match,
        replacement: '[STUDENT ID REDACTED]',
        position: offset
      });
      return '[STUDENT ID REDACTED]';
    });
  });

  return {
    redactedText: text,
    redactedItems
  };
}
```

---

## Implementation: Context-Aware Redaction (AI-Powered)

For more complex documents where simple patterns aren't enough, use AI to identify PII:

```typescript
// pii-redaction-ai.ts

const PII_DETECTION_PROMPT = `
You are a PII (Personally Identifiable Information) detection system.

Analyze the following document and identify ALL instances of PII that should be redacted before AI processing.

PII TO FIND AND REDACT:
1. Student names (any capitalized name that appears to be a student's name)
2. Parent/guardian names
3. Dates of birth
4. Addresses (street addresses, cities with states)
5. Phone numbers
6. Email addresses
7. Student ID numbers
8. Medical record numbers
9. Social Security numbers
10. Any other identifying information

Return a JSON object with the original text and all PII instances found:

{
  "redactions": [
    {"original": "student name here", "type": "STUDENT_NAME", "redacted": "[STUDENT NAME REDACTED]"},
    {"original": "123-45-6789", "type": "SSN", "redacted": "[SSN REDACTED]"}
  ]
}

Document to analyze:
{DOCUMENT_CONTENT}

Respond ONLY with valid JSON, no other text.
`;

export async function detectPIIWithAI(documentText: string): Promise<RedactionResult> {
  // This would call the AI with the PII detection prompt
  // and return structured redaction data
}
```

---

## Pipeline Integration

### Document Processing Flow

```
1. User selects document for AI Review
2. Fetch PDF from Supabase Storage Bucket
3. Extract text from PDF
4. Run PII Redaction Layer:
   a) First pass: Rule-based (fast)
   b) Second pass: AI-powered (if needed for complex docs)
5. Verify no PII remains
6. Send clean text to AI for analysis
7. Return results to user
```

### Verification Step

Before sending to AI, always run verification:

```typescript
export function verifyNoPIIRemains(text: string): boolean {
  const patterns = [
    /\d{3}-\d{2}-\d{4}/,  // SSN
    /[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}/,  // Email
    /\d{3}-\d{3}-\d{4}/,  // Phone
    /\b\d{1,2}\/\d{1,2}\/\d{2,4}\b/  // Date
  ];
  
  return !patterns.some(pattern => pattern.test(text));
}
```

---

## User Notification

When PII redaction is complete, show user:

> "✅ PII Protection Active - All personally identifiable information has been automatically detected and redacted before AI processing. Student names, SSNs, DOBs, and contact info are stripped."

---

## Logging (for audit)

Track what was redacted (without storing the actual PII):

```typescript
interface RedactionLog {
  timestamp: Date;
  documentId: string;
  tenantId: string;
  piiTypesRedacted: string[];  // e.g., ['SSN', 'DOB', 'EMAIL']
  redactedCount: number;
}
```

---

## Testing the PII Layer

Test with sample documents containing:
- [ ] Social Security Numbers
- [ ] Dates of Birth (multiple formats)
- [ ] Phone Numbers (multiple formats)
- [ ] Email Addresses
- [ ] Street Addresses
- [ ] Student IDs
- [ ] Parent Names
- [ ] Medical Information

Verify the AI never receives any of this information.

---

*This PII redaction layer is the security foundation for the AI Document Review system.*