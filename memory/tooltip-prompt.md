# Comprehensive Tooltip Implementation Prompt for Lovable

## Overview
Add helpful tooltips throughout Orvared to help users understand key fields. Special focus on special education workflows, billing, and case management. Use the existing IconTooltip component.

---

## CLIENT MANAGEMENT (CRM)

| Field | Tooltip Content |
| ----- | ----------------|
| Client Name | "Full name of the parent/guardian or adult student" |
| Email | "Primary contact email. Used for login and notifications." |
| Phone | "Best contact number. Include extension if applicable." |
| Address | "Mailing address for correspondence and billing." |
| Client Status | "Active = current case | Inactive = no active matters | Prospective = considering services" |
| Client Tags | "Labels for organization. Example: 'referral-source', 'priority', 'dcfs'" |
| Preferred Contact | "How they prefer to be reached: Phone, Email, Text" |
| Notes | "Private internal notes. Not visible to client." |
| Referral Source | "Where this client came from: 'attorney-referral', 'parent-network', 'google'" |
| Assigned To | "Which staff member is the primary contact for this client" |

---

## STUDENT RECORDS

| Field | Tooltip Content |
| ----- | ----------------|
| Student Name | "Full legal name of the child" |
| Date of Birth | "Required for eligibility verification and age-appropriate services" |
| Grade Level | "Current grade. Determines grade-band services and expectations." |
| School | "The district school the student attends. Affects timeline calculations." |
| Student ID | "District-assigned student ID (from enrollment records)" |
| Primary Disability | "Main IDEA disability category (13 categories). Required for eligibility." |
| Secondary Disabilities | "Additional disability categories, if any" |
| Eligibility Date | "When student was first found eligible for IDEA services" |
| Guardian Name(s) | "Who has educational decision-making rights" |
| Relationship | "Parent, grandparent, foster parent, adult student" |
| Custody Notes | "Who has legal custody. Important for consent and notices." |

---

## CASE MANAGEMENT

| Field | Tooltip Content |
| ----- | ----------------|
| Case Name | "Auto-generated from student name + case type. Can customize." |
| Case Type | "IEP | 504 | Due Process | Transition | Advocacy" |
| Case Status | "Active = working | Closed = resolved | On Hold = waiting on info/documents" |
| Priority | "High = urgent deadline (due process, court) | Medium = standard | Low = no rush" |
| Date Opened | "When you started working on this case" |
| Primary Issue | "Main concern driving services: 'reading-disabled', 'behavioral', 'autism', etc." |
| District Contact | "Who to communicate with at the school district" |
| Case Goals | "What success looks like for this case" |
| Assigned Staff | "Who is leading this case" |
| Related Cases | "Link to related cases (siblings, same family)" |

---

## BILLING & INVOICING

| Field | Tooltip Content |
| ----- | ----------------|
| Hourly Rate | "Your default rate per hour. Can override per-case in case settings." |
| Rate Type | "Hourly = time-based | Flat Fee = fixed price for service | Retainer = prepaid hours" |
| Invoice Due Date | "When payment expected. Affects late fee calculation." |
| Invoice Status | "Draft = not sent | Sent = awaiting payment | Paid = received | Overdue = past due" |
| Payment Method | "Check | Credit Card | Bank Transfer | Cash" |
| Late Fee % | "Percentage charged if paid after due date" |
| Billing Address | "Where to send invoice (may differ from client address)" |
| PO Number | "Purchase Order number if district requires one for payment" |
| Invoice Notes | "Internal notes for this invoice. Not visible to client." |
| Adjustments | "Discounts or credits applied to this invoice" |
| Write-Off | "Amount waived. Requires authorization for write-offs over $" |

---

## TIME TRACKING

| Field | Tooltip Content |
| ----- | ----------------|
| Time Entry Category | "Meeting = IEP/ARD meetings | Document Review = records | Calls = parent calls | Research = case law/norms | Drafting = letters/docs" |
| Duration | "Actual time spent in minutes. Timer rounds to nearest 15 min." |
| Billable | "Billable = can include on invoice | Non-billable = internal time, not charged" |
| Timer Description | "Brief description of work. Appears on invoice if billable." |
| Date of Service | "When the work was done. Must be within case dates." |
| Time Entry Notes | "Detailed notes if needed. Private, not on invoice." |
| Overtime | "Hours beyond standard. Check client agreement for overtime rates." |

---

## DOCUMENTS

| Field | Tooltip Content |
| ----- | ----------------|
| Document Type | "IEP | Evaluation | Correspondence | Report | Medical | Legal" |
| Document Date | "Date on the document (may differ from upload date)" |
| Upload Tags | "Labels for search: 'annual-review', 'prior-year', 'critical', 'requested'" |
| Document Notes | "Internal notes about this document" |
| Confidential | "Mark if contains sensitive info (medical, legal) - restricted access" |
| File Visibility | "Client Visible = can see in portal | Staff Only = internal only" |
| Uploaded By | "Who uploaded this document" |

---

## IEP WORKFLOW

| Field | Tooltip Content |
| ----- | ----------------|
| IEP Type | "Initial = first IEP | Annual = yearly review | Triennial = 3-year reeval | Amendment = mid-year change" |
| IEP Status | "Draft = being developed | Pending = awaiting meeting | Active = approved | Expired = needs renewal" |
| Goal Area | "Domain: Reading | Writing | Math | Speech | OT | PT | Behavior | Functional | Transition" |
| Baseline | "Measurable starting point. Example: 'Reads 45 WPM with 70% accuracy'" |
| Measurable Criteria | "How we'll know goal is met: '90% accuracy across 3 data points' or '8/10 trials'" |
| Short-Term Objectives | "Smaller milestones leading to annual goal. Required for some disability categories." |
| Progress Monitoring | "How often progress measured: Weekly | Bi-weekly | Monthly | Quarterly" |
| Service Minutes | "Minutes per week of this service. Check state for minimums per disability." |
| Service Location | "Where service delivered: General Ed Classroom | Special Ed Room | Therapy Room | Community" |
| Provider | "Who provides this service: Special Ed Teacher | Speech Therapist | OT | Counselor" |
| Extended School Year | "Summer services recommended. Based on regression/recoupment analysis." |
| Prior Written Notice | "Required before any change in placement or services. District must provide." |

---

## EVALUATIONS

| Field | Tooltip Content |
| ----- | ----------------|
| Evaluation Type | "Initial = first eval | Triennial = 3-year reeval | Follow-up | Independent | Psychological" |
| Evaluation Status | "Scheduled = upcoming | In Progress = being conducted | Complete = report done | Waiting = on hold" |
| Requested By | "Who requested this evaluation: Parent | District | Physician" |
| Assessment Areas | "Cognitive | Academic | Speech/Language | OT/PT | Adaptive | Behavior | Social-Emotional" |
| evaluator | "Who is conducting the evaluation" |
| Report Date | "When report will be completed" |
| Meeting Date | "When IEP team will review results" |

---

## DEADLINES & TIMELINES

| Field | Tooltip Content |
| ----- | ----------------|
| Deadline Type | "IEP Due | Response Due | Meeting Scheduled | Due Process | Court Date" |
| Due Date | "When action required. Check state timeline for IEP meetings." |
| Reminder | "When to be reminded: 1 day | 3 days | 1 week | 2 weeks before" |
| Assigned To | "Who is responsible for this deadline" |
| Status | "Pending | In Progress | Completed | Missed" |
| Escalate If | "Trigger escalation if not completed by this date" |

---

## LAW LIBRARY

| Field | Tooltip Content |
| ----- | ----------------|
| State Selection | "Select the state whose regulations to check against" |
| IDEA Category | "One of 13 disability categories under IDEA" |
| Crosswalk | "Maps IEP requirements to state-specific regulations" |
| Search | "Search federal IDEA, state regs, or case law" |
| Citation Format | "Use proper legal citation: '34 CFR 300.XXX' or 'Texas Edu. Code 29XXX'" |

---

## SETTINGS

| Field | Tooltip Content |
| ----- | ----------------|
| Firm Name | "Your business name on invoices and documents" |
| Default Hourly Rate | "Used for new cases unless overridden" |
| Time Zone | "Affects deadlines and billing calculations" |
| Invoice Template | "Which template for invoice appearance" |
| Email Signature | "Appears on all client emails" |
| Two-Factor Auth | "Required for all staff. Adds security for client data." |
| Session Timeout | "Auto-logout after inactivity. HIPAA recommends 15 min." |

---

## DASHBOARD

| Field | Tooltip Content |
| ----- | ----------------|
| Active Cases | "Cases with status Active - your current workload" |
| Pending Deadlines | "Deadlines approaching - sorted by due date" |
| Revenue MTD | "Money invoiced this month (not necessarily collected)" |
| Utilization | "Billable hours / total hours worked. Target typically 60-80%." |
| Upcoming IEPs | "IEP meetings in next 30 days" |

---

## IMPLEMENTATION PRIORITY

### Phase 1 - Must Have (Critical Fields)
1. Hourly Rate
2. Case Status  
3. Baseline
4. Measurable Criteria
5. Progress Monitoring
6. Service Minutes
7. Document Type
8. Billable/Non-billable

### Phase 2 - Should Have (Common Fields)
1. Time Entry Category
2. Priority Level
3. IEP Type
4. Invoice Status
5. Primary Disability
6. Student Grade Level
7. Rate Type
8. Client Status
9. Case Type
10. Deadline Type

### Phase 3 - Nice to Have (Remaining Fields)
All others - can be added incrementally

---

## Implementation Notes

```tsx
// Use existing IconTooltip component:
<IconTooltip content="Your tooltip text here" />

// Place next to field labels or info icons
// For fields with existing tooltips, update content to be more helpful
// Test on mobile - tooltips should work on touch
```

Send this to Lovable in phases:
1. First - Phase 1 fields (8 tooltips)
2. Second - Phase 2 fields (10 tooltips)  
3. Third - Phase 3 fields (remaining ~50 tooltips)

Total: 68+ tooltips covering all major areas of the app.