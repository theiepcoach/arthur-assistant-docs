

# AI Document Processing Tracking — Deep Dive & Fix Plan

## Root Cause Analysis

The tracking system has **two separate write paths** that are out of sync:

### Path 1: `ai-gateway` edge function (main AI entry point)
- Writes to `internal_ai_logs` (with actual_units, estimated_units, metadata)
- Increments `ai_settings.monthly_used_units`
- Updates `seat_monthly_usage`
- Writes to `ai_usage_ledger`
- Works correctly for: summarize, generate_letter, analyze_iep, classify, etc.

### Path 2: `trackAIUsage()` shared helper (bypass functions)
- Used by: `score-evidence`, `voice-command`, `transcribe`, `compare-documents`, `parse-client-document`, `extract-timeline`
- Writes to `ai_requests` table
- Increments `ai_settings.monthly_used_units`
- Writes to `ai_usage_ledger`
- Writes to `seat_monthly_usage`
- **DOES NOT write to `internal_ai_logs`**

### Path 3: `pii-scan` edge function
- Directly increments `ai_settings.monthly_used_units`
- **DOES NOT write to `internal_ai_logs`**
- **DOES NOT write to `ai_usage_ledger`**

## What's Broken

1. **Dashboard shows no data**: The `AIUsage.tsx` page reads from `internal_ai_logs` for charts, feature breakdown, top users, and stats. But 6 edge functions bypass the gateway and use `trackAIUsage()`, which never writes to `internal_ai_logs`. So the dashboard only shows usage from direct ai-gateway calls.

2. **`monthly_used_units` has race conditions**: Both paths do read-then-write (`SELECT monthly_used_units` → `UPDATE monthly_used_units + N`). Under concurrent requests, units can be lost.

3. **`ai_settings` row may not exist**: The `ai-gateway` function reads `ai_settings` but never auto-creates it. Only `trackAIUsage()` auto-creates. If a tenant's first AI call goes through the gateway, usage tracking silently fails.

4. **PII scan usage is invisible**: The `pii-scan` function increments units but writes no log entry, so PII scanning costs are invisible in dashboards.

## Fix Plan

### 1. Update `trackAIUsage()` to also write `internal_ai_logs`

Add an `internal_ai_logs` insert to the shared helper so all bypass functions get dashboard visibility.

| Field | Value |
|-------|-------|
| `tenant_id` | from params |
| `user_id` | from params |
| `ai_request_id` | from the ai_requests insert |
| `estimated_units` | calculated units |
| `actual_units` | calculated units |
| `input_size_bytes` | from params |
| `output_size_bytes` | from params |
| `success` | true |
| `metadata_json` | `{ request_type, task: requestType }` |

### 2. Fix race condition with atomic increment

Replace the read-then-write pattern in both `trackAIUsage()` and `ai-gateway` with an RPC that uses `UPDATE ... SET monthly_used_units = monthly_used_units + $units`. Create a simple SQL function:

```sql
CREATE OR REPLACE FUNCTION increment_ai_usage(p_tenant_id uuid, p_units integer)
RETURNS integer AS $$
  UPDATE ai_settings
  SET monthly_used_units = monthly_used_units + p_units
  WHERE tenant_id = p_tenant_id AND user_id IS NULL
  RETURNING monthly_used_units;
$$ LANGUAGE sql SECURITY DEFINER;
```

### 3. Auto-create `ai_settings` row in `ai-gateway`

Add the same auto-create logic from `trackAIUsage()` into the gateway function so that first-time tenants get a row created.

### 4. Add `internal_ai_logs` write to `pii-scan`

Add a log entry when PII scanning consumes AI units, with `request_type: "pii_scan"`.

### 5. Add `pii_scan` to dashboard feature map

Add the PII scan feature key to `FEATURE_META` in `AIUsage.tsx` so it shows in the "Usage by Feature" breakdown.

## Files to Change

| File | Change |
|------|--------|
| `supabase/functions/_shared/track-ai-usage.ts` | Add `internal_ai_logs` insert; use atomic increment |
| `supabase/functions/ai-gateway/index.ts` | Use atomic increment RPC; auto-create ai_settings if missing |
| `supabase/functions/pii-scan/index.ts` | Add `internal_ai_logs` + `ai_usage_ledger` writes |
| `src/pages/app/AIUsage.tsx` | Add `pii_scan` to `FEATURE_META` |
| Database migration | Create `increment_ai_usage` RPC function |

## Implementation Order

1. Database migration (atomic increment function)
2. Update `trackAIUsage()` shared helper
3. Update `ai-gateway` to use RPC + auto-create
4. Update `pii-scan` logging
5. Update dashboard feature map

