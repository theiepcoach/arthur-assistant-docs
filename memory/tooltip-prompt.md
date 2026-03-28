# Tooltip Implementation Prompt for Lovable

## Overview
Add helpful tooltips throughout the Orvared interface to help users understand key fields, especially those related to special education workflows. Use the existing `IconTooltip` component that's already in the codebase.

---

## Background
- IconTooltip component already exists: `src/components/app/IconTooltip.tsx`
- Already 116 tooltips in use across the app
- Add tooltips to fields that aren't self-explanatory, especially special education specific terms

---

## Tooltip Content to Add

### Billing & Invoicing

**Hourly Rate Field**
- Content: "Your default rate per hour. Can be overridden for individual cases."
- Placement: Next to hourly rate input in invoice builder

**Invoice Due Date**
- Content: "When payment is expected. Affects late fee calculation if enabled."
- Placement: Next to due date picker

**Time Entry Category**
- Content: "Meeting = IEP meetings and ARD conferences | Document Review = reviewing records and reports | Calls = Parent/advocate phone calls | Research = Case research | Drafting = Writing letters/documents"
- Placement: Next to category dropdown in time entry

**Invoice Status**
- Content: "Draft = Not sent yet | Sent = Awaiting payment | Paid = Payment received | Overdue = Past due date"
- Placement: Next to status badge

---

### Case Management

**Case Status**
- Content: "Active = Currently working on case | Closed = Case resolved | On Hold = Paused pending info"
- Placement: Next to status dropdown

**Priority Level**
- Content: "High = Urgent timeline (due process, court dates) | Medium = Standard timeline | Low = Flexible, no rush"
- Placement: Next to priority selector

**Client Visibility**
- Content: "Controls what the client can see in their portal. Turn off to hide from client."
- Placement: Next to visibility toggle

---

### Document Center

**Document Type**
- Content: "IEP = Individualized Education Program | Evaluation = Testing/assessment reports | Correspondence = Letters from district | Report = Progress reports, other documents"
- Placement: Next to document type dropdown

**Upload Tags**
- Content: "Optional labels to help organize and search documents later. Example: 'prior-year', 'annual-review'"
- Placement: Next to tags input

---

### Student Records

**Primary Disability**
- Content: "The main disability category that qualifies the student for IDEA services. See 13 IDEA categories."
- Placement: Next to disability dropdown

**School District**
- Content: "Determines which district's policies and timelines apply. Must be the district the student attends."
- Placement: Next to district selector

**Parent/Guardian**
- Content: "The contact responsible for making educational decisions. Links to client record."
- Placement: Next to parent dropdown

---

### IEP Workflow

**Baseline Data**
- Content: "Measurable starting point showing current performance. Example: 'Reads 45 WPM with 70% comprehension'"
- Placement: Next to baseline text area in goal editor

**Measurable Criteria**
- Content: "How we'll know the goal is mastered. Include accuracy %, duration, or trial count. Example: '90% accuracy across 3 data points'"
- Placement: Next to criteria field in goal editor

**Progress Monitoring**
- Content: "How often progress will be measured. More frequent = better data for adjustments."
- Placement: Next to frequency dropdown

**Goal Area**
- Content: "Reading, Writing, Math, Speech, Behavior, Functional Skills, Transition - choose the primary area this goal addresses"
- Placement: Next to area dropdown

**Service Minutes**
- Content: "Minutes per week of this service. Check with state for minimums."
- Placement: Next to minutes input

---

### Time Tracking

**Billable vs Non-Billable**
- Content: "Billable = Can be included on invoice | Non-billable = Internal time, not charged to client"
- Placement: Next to billable toggle

**Timer Description**
- Content: "Brief description of what you're working on. Will appear on invoice if billable."
- Placement: Next to description input

---

## Implementation Guidelines

1. Use existing IconTooltip component
2. Keep tooltips concise (1-2 sentences max)
3. Add specific examples where helpful
4. Match tone to existing tooltips (helpful, not technical)
5. Test that tooltips are visible on both light and dark backgrounds
6. Don't add tooltips to obvious fields (name, email, phone)

---

## Priority Order

### First Priority (Must Have)
- Hourly Rate
- Case Status  
- Baseline
- Measurable Criteria
- Progress Monitoring

### Second Priority (Should Have)
- Time Entry Category
- Priority Level
- Document Type
- Service Minutes
- Billable/Non-Billable

### Third Priority (Nice to Have)
- Invoice Due Date
- Client Visibility
- Upload Tags
- School District
- Goal Area

---

## Deliverable
Add IconTooltips to the fields listed above, using the content provided. Place them next to the relevant form field using the existing pattern:

```tsx
<IconTooltip content="Your tooltip text here" />
```