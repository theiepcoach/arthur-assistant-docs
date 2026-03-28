# Special Education Evaluator Module Enhancement Analysis
**For Orvared**  
**Date:** 2026-03-28

---

## Executive Summary

This document analyzes what special education evaluators do and provides recommendations for enhancing Orvared's evaluator module. Key focus areas: workflow optimization, role-based access control, and the "lead evaluator" assignment model.

---

## Part 1: What Does a Special Education Evaluator Actually Do?

### The Core Role

A special education evaluator (often a school psychologist, diagnostician, or licensed specialist) is responsible for determining whether a student qualifies for special education services under IDEA. This is NOT the same as being a special education teacher or advocate.

### Key Responsibilities

**1. Referral Management**
- Receive/process referrals for special education evaluation
- Track referral source (parent request, teacher referral, screening results)
- Document timeline requirements from day one

**2. Multi-Disciplinary Team (MDT) Coordination**
The evaluator is typically the coordinator of the MDT, which includes:
- Parents/guardians
- At least one regular education teacher
- At least one special education teacher
- LEA representative (can approve resources)
- Professionals who can interpret evaluation results
- The student (when transition services are discussed)

**3. Evaluation Planning**
- Review existing data (grades, discipline records, attendance, prior interventions)
- Determine what areas need assessment based on suspected disability
- Assign specific assessment components to different team members
- Create evaluation timeline (Texas: 45 school days + 30 calendar days to ARD)

**4. Assessment Administration**
- Conduct/oversee psychological testing
- Conduct academic achievement testing
- Observe student in classroom environment
- Review medical/developmental history
- Interview parents and teachers

**5. Report Writing (FIIE - Full and Individual Initial Evaluation)**
- Synthesize all data into comprehensive report
- Address all 13 disability categories
- Include eligibility determination for each category
- Provide educational recommendations
- Ensure report is provided to parents 5 school days before initial ARD meeting

**6. ARD Committee Meeting Support**
- Present evaluation findings
- Help team interpret results
- Ensure procedural compliance
- Document eligibility decision

### State-Specific Timeline Pressures (Texas Example)

| Trigger | Deadline |
|---------|----------|
| Parent requests evaluation | District has 15 school days to provide consent form |
| Parent signs consent | District has 45 school days to complete evaluation |
| Evaluation complete | Report due to parents 5 school days before ARD |
| ARD meeting | Must be held within 30 calendar days of evaluation completion |

Missing deadlines = potential due process violations.

---

## Part 2: Current Orvared Evaluator Module Assessment

*Note: Without direct codebase access, this is based on documented features and IEP evaluation prompts.*

### What Appears to Exist

- `Evaluations.tsx` page
- Document processing for IEPs (via Arthur)
- Case management linking clients to evaluations
- CRM for pipeline management

### Likely Gaps

**Workflow Gaps:**
1. ❌ No referral intake tracking with deadlines
2. ❌ No evaluation timeline/tickler system
3. ❌ No multi-disciplinary team assignment/coordination
4. ❌ No assessment task delegation (who does what)
5. ❌ No progress notes against evaluation milestones
6. ❌ No PWN (Prior Written Notice) workflow automation
7. ❌ No consent tracking with expiration alerts

**Access Control Gaps:**
1. ❌ No role-based access for different evaluator types
2. ❌ No "lead evaluator" vs "assistant evaluator" distinction
3. ❌ No supervisor/reviewer approval workflows
4. ❌ No audit trail for who changed what in evaluation

**Reporting Gaps:**
1. ❌ No compliance deadline dashboards
2. ❌ No state-specific timeline calculators
3. ❌ No eligibility category tracking across cases

---

## Part 3: The Lead Evaluator Question

### What You're Currently Considering

You've assigned a "lead evaluator" to oversee the evaluation process. This is a reasonable instinct, but let's unpack alternatives.

### Option A: Lead Evaluator Model (Current Thinking)

**What it looks like:**
- One evaluator "owns" the case
- Others can assist but lead signs off
- Similar to how some districts structure it

**Pros:**
- Clear accountability
- Simple to implement
- Familiar to many districts

**Cons:**
- Can create bottlenecks if lead is overloaded
- Doesn't reflect how MDT actually works (collaborative, not hierarchical)
- May not fit independent evaluators who work solo
- Doesn't handle cases needing multiple disciplinary experts well

### Option B: MDT Coordinator Model (Recommended)

**What it looks like:**
- Any team member can be "coordinator" based on case needs
- Coordinator doesn't necessarily do all assessment - orchestrates
- Report synthesis role vs. assessment execution role
- More flexible for different case types

**Pros:**
- Matches IDEA's MDT structure
- Scales better for complex cases
- Works for both district teams and independent evaluators
- Clearer separation: who assesses vs. who coordinates

**Cons:**
- Slightly more complex to configure
- Requires clearer workflow definitions

### Option C: Case Manager + Specialist Model

**What it looks like:**
- One person (could be non-evaluator) manages case logistics
- Licensed evaluators handle only their assessment domain
- Report compilation is a distinct role

**Pros:**
- Best for high-volume operations
- Allows evaluators to focus on assessment
- Paralegal-like support model

**Cons:**
- May be overkill for smaller operations
- Requires more staff

### Recommendation

**Start with Option B (MDT Coordinator)** but build it flexibly so it can evolve. The key insight: the person who coordinates the evaluation may NOT be the person who conducts the assessment.

---

## Part 4: Recommended Enhancements for Orvared

### Phase 1: Core Workflow Automation

**1. Referral Intake Module**
```
Fields:
- Student name, DOB, grade
- Referring party (parent/teacher/other)
- Date of referral
- Suspected disability area(s)
- Prior interventions attempted
- Automatic deadline calculation based on state
```

**2. Evaluation Timeline Tracker**
- Visual timeline showing days elapsed vs. deadline
- Automated alerts at key intervals (e.g., "5 school days remaining")
- State selector (TX, KY, CO, etc.) for accurate deadline calc

**3. Consent Management**
- Track consent received date
- Monitor consent validity periods
- Alert for expired/incomplete consent

**4. Assessment Task Assignment**
- Create tasks for each assessment component
- Assign to specific team members
- Track completion status
- Link to standardized assessments used

### Phase 2: Role-Based Access Control

**Role Definitions:**

| Role | Permissions |
|------|-------------|
| **Administrator** | Full access, can view all cases, configure system |
| **MDT Coordinator** | Create/manage evaluations, assign tasks, view team workload |
| **Lead Evaluator** | Sign off on reports, supervise other evaluators, review submissions |
| **Evaluator** | Conduct assessments, write sections, submit for review |
| **Assistant** | Support tasks, data entry, no final submissions |
| **Parent/Guardian** | View-only access to their student's documents (portal) |

**Access Control Matrix:**

| Action | Admin | Coordinator | Lead | Evaluator | Assistant |
|--------|-------|-------------|------|-----------|-----------|
| Create case | ✓ | ✓ | ✓ | ✓ | ✗ |
| Assign evaluator | ✓ | ✓ | ✓ | ✗ | ✗ |
| Submit evaluation | ✓ | ✓ | ✗ | ✓ | ✗ |
| Approve/sign off | ✓ | ✓ | ✓ | ✗ | ✗ |
| View all cases | ✓ | ✓ | ✗* | ✗* | ✗ |
| Delete case | ✓ | ✗ | ✗ | ✗ | ✗ |

*May see assigned cases only

### Phase 3: Report Collaboration

**Evaluation Report Workspace:**
- Sections locked for editing once submitted
- Version history for report drafts
- Comments/feedback within sections
- Approval workflow (Evaluator → Lead → Final)
- Integration with Arthur for AI-assisted drafting

### Phase 4: Compliance Dashboard

**Metrics to Track:**
- Cases approaching deadline (30/15/5 days)
- Overdue cases
- Missing consent/documents
- Evaluation stages (Referral → Consent → Assessment → Report → ARD → Eligibility)

---

## Part 5: Specific Feature Recommendations

### Must-Have (Critical)

1. **Timeline Calculator**
   - Input: State, consent date, instructional days remaining
   - Output: Key deadlines (PWN due, FIE due, ARD due)

2. **Task Management for MDT**
   - Create, assign, track assessment tasks
   - Due dates linked to evaluation timeline

3. **Basic Role Access**
   - At minimum: Admin, Evaluator, View-only
   - No lead evaluator needed for MVP, just role + case assignment

### Should Have (Important)

4. **Consent Tracking**
   - Status: Not requested, Pending, Received, Expired
   - Automated reminders

5. **Evaluation Report Templates**
   - Pre-built sections matching state requirements
   - Progress through completion %

6. **Dashboard Widgets**
   - Active evaluations count
   - Upcoming deadlines (7-day view)
   - Cases by stage

### Nice to Have (Later)

7. **Parent Portal**
   - Parents view documents, not edit
   - Secure access link

8. **AI-Assisted Report Writing**
   - Use Arthur to draft from assessment data
   - Human review required (obviously)

---

## Summary of Recommendations

1. **Don't start with "lead evaluator"** — start with a flexible MDT coordinator role that can be assigned to whoever makes sense for the case

2. **Build the timeline engine first** — deadlines are the heartbeat of special education evaluation; everything else builds around that

3. **Add role-based access but keep it simple** — need at least 3 roles: Admin, Evaluator, Read-only

4. **Task assignment is the killer feature** — letting coordinators assign specific assessment pieces to different specialists will be high-value

5. **Think workflow, not just forms** — evaluators don't just fill out forms, they coordinate people, timelines, and documents

---

*Want me to dive deeper into any specific area? I can prototype the role definitions, map out the data model for the timeline tracker, or research specific state requirements further.*