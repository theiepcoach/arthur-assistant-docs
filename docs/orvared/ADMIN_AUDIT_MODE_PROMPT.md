# Admin Audit Mode Implementation - Lovable Prompt

Use this prompt in Lovable to implement the Admin Audit Mode feature for Orvared.

---

## PROMPT: Admin Audit Mode for Support & Founders

I need to implement an Admin Audit Mode feature for our Orvared special education advocacy platform. This allows founders and support staff to view user data for support/troubleshooting purposes.

### Current Setup
- Multi-tenant system with RLS (Row Level Security)
- User roles: `owner`, `staff`, `client`
- "View As" feature exists but just changes the UI view
- Audit logs table exists for tracking

### Requirements

#### 1. Database Changes
Add new user roles and audit fields:

```sql
-- Add founder role to user_roles enum if not exists
ALTER TYPE user_role ADD VALUE IF NOT EXISTS 'founder';

-- Add tenant_admin role for tenant-level oversight
ALTER TYPE user_role ADD VALUE IF NOT EXISTS 'tenant_admin';

-- Add audit-specific fields to audit_logs table
ALTER TABLE audit_logs ADD COLUMN IF NOT EXISTS is_audit_view BOOLEAN DEFAULT FALSE;
ALTER TABLE audit_logs ADD COLUMN IF NOT EXISTS audit_viewer_user_id UUID REFERENCES auth.users(id);
ALTER TABLE audit_logs ADD COLUMN IF NOT EXISTS audit_view_reason TEXT;
ALTER TABLE audit_logs ADD COLUMN IF NOT EXISTS audit_access_level TEXT; -- 'founder', 'tenant_admin', 'user'

-- Add session tracking for audit mode
ALTER TABLE user_sessions ADD COLUMN IF NOT EXISTS is_audit_mode BOOLEAN DEFAULT FALSE;
ALTER TABLE user_sessions ADD COLUMN IF NOT EXISTS audit_mode_started_at TIMESTAMPTZ;
ALTER TABLE user_sessions ADD COLUMN IF NOT EXISTS audit_access_level TEXT;
```

#### 2. Backend Logic Updates

1. **Update authentication/authorization:**
   - `founder` role bypasses tenant restrictions - can see ALL tenants
   - `tenant_admin` role can see ALL users/data within THEIR tenant
   - When viewing as founder or tenant_admin, use elevated auth context

2. **Access Level Logic:**
   ```
   | Role          | Can View                                |
   |---------------|-----------------------------------------|
   | founder       | All tenants + all users                |
   | tenant_admin  | Own tenant + all users in tenant       |
   | owner         | Own tenant + assigned cases only       |
   | staff         | Assigned cases only                    |
   | client        | Own data only                          |
   ```

2. **Create new API endpoints:**
   - `POST /api/admin/start-audit-mode` - Start audit session
   - `POST /api/admin/stop-audit-mode` - End audit session  
   - `GET /api/admin/audit-logs` - View audit history

3. **Audit logging:**
   - Every data access in audit mode logged with:
     - `is_audit_view: true`
     - `audit_viewer_user_id` (the admin)
     - `audit_view_reason` (e.g., "support ticket #123")

#### 3. Frontend UI Changes

1. **Add Audit Mode Banner:**
   - When in audit mode, show fixed banner at top:
     - "🔍 AUDIT MODE - Read Only"
     - "Viewing as: [User Name]"
     - "This view will auto-expire in 30 minutes"
     - "End Audit Session" button

2. **Update "View As" dropdown:**
   - Add option: "View as Founder (Audit Mode)"
   - Show confirmation dialog before enabling

3. **Disable actions in Audit Mode:**
   - Hide all Edit/Delete buttons
   - Disable form submissions
   - Show "Audit mode is read-only" tooltip on hover

4. **Add to Settings/Admin panel:**
   - Audit Log Viewer page
   - Filter by: user, date range, audit type
   - Export audit logs as CSV

#### 4. Security Features

1. **Auto-expiry:**
   - Audit session expires after 30 minutes
   - Show countdown timer in banner
   - Auto-redirect to normal mode

2. **Notification:**
   - When admin starts audit mode on a tenant:
     - Log a notification in-system
     - Optional: Send email to tenant owner

3. **Limited actions:**
   - Cannot edit any data in audit mode
   - Cannot delete records
   - Cannot download documents (view only)

#### 5. Visual Indicators

| State | Banner Color | Actions Allowed |
|-------|--------------|-----------------|
| Normal | None | All |
| View As (user) | Yellow | As per user role |
| Audit Mode | Red | Read only |

#### 3. Tenant Admin Specific Features

1. **Assignment:**
   - Tenant owner can promote any staff member to `tenant_admin`
   - Added in: User management → Promote to Tenant Admin
   - Stored in `tenant_user_roles` table

2. **Capabilities:**
   - Can view ALL users within their tenant
   - Can view ALL cases/clients/students in tenant
   - Can access ALL documents in tenant
   - Cannot see other tenants' data

3. **Logging:**
   - All tenant_admin views logged with:
     - `audit_access_level: 'tenant_admin'`
     - Which user they're viewing
     - Timestamp and reason

4. **UI for Tenant Admin:**
   - In "View As" dropdown, add: "View as Tenant Admin"
   - Shows banner: "TENANT ADMIN MODE - Viewing All [Tenant] Data"
   - Same read-only restrictions as founder audit mode

---

### Implementation Steps

1. **Database Migration:**
   - Run SQL to add founder role and audit fields

2. **Backend:**
   - Update auth logic for founder bypass
   - Create audit mode endpoints
   - Add audit logging middleware

3. **Frontend:**
   - Add Audit Mode banner component
   - Update "View As" selector
   - Disable actions via prop/component
   - Add Audit Log viewer page

4. **Testing:**
   - Test founder can see all tenants
   - Test audit logs are created
   - Test auto-expiry works
   - Test notifications sent

---

### Key UI Components to Create

```
components/
  ├── admin/
  │   ├── AuditModeBanner.tsx
  │   ├── AuditLogViewer.tsx
  │   └── AuditSessionTimer.tsx
  └── ui/
      └── (existing components with audit-readonly prop)
```

---

### Acceptance Criteria

- [ ] Founder role can see ALL tenant data
- [ ] Tenant Admin role can see ALL users within their tenant
- [ ] Audit mode banner shows with countdown
- [ ] All edit/delete actions disabled in audit mode
- [ ] Every data access logged with audit flag + access level
- [ ] Audit session auto-expires after 30 minutes
- [ ] Can export audit logs as CSV (filterable by access level)
- [ ] Notification sent when audit starts (optional email)
- [ ] Owner can promote staff to tenant_admin

---

### Security Checklist

- [ ] No data modifications allowed in audit mode
- [ ] All audit views logged with user + timestamp + reason
- [ ] Session auto-expires
- [ ] Audit logs immutable (append only)
- [ ] Only founders can enable audit mode