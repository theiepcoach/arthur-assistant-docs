# Orvared Codebase Exploration

**Date:** 2026-03-26
**Status:** In Progress

## Stack
- Lovable (React/Supabase)
- Supabase Project ID: zghfgsuwmwfdoyuzjmeg

## Key Files & Structure

### Pages (`src/pages/app/`)
- CasesModule.tsx, CaseDetail.tsx, CaseWorkspace.tsx
- Clients.tsx, ClientDetail.tsx
- CRM.tsx (pipeline/deals)
- Billing.tsx
- AppCalendar.tsx
- ComplianceDashboard.tsx
- Documents.tsx, DocumentCenter.tsx
- Evaluations.tsx
- AISettings.tsx, AIUsage.tsx
- Campaigns.tsx

### Hooks (`src/hooks/`)
- use-crm.ts (deals, contacts)
- Various other hooks

### Layouts
- AdvocateLayout.tsx
- AdminLayout.tsx
- PortalLayout.tsx
- PublicPageLayout.tsx

## Current Task (.lovable/plan.md)
Add Edit and Delete for CRM Engagements:
1. Add `useDeleteDeal` hook
2. Update CRMDeals.tsx with edit/delete UI

## Next Steps
- Continue exploring specific modules
- Document data models
- Map out user flows