# Signup Flow Documentation
## What Users See When Signing Up for Orvared

---

## 1. Landing Page (Public)

**URL:** orvared.com

**Elements:**
- Hero section with value prop
- Features overview
- Pricing button
- "Sign Up Free" CTA
- Login link

---

## 2. Sign Up Page

**URL:** orvared.com/signup

**Required Fields:**
- Email address
- Password (with strength indicator)
- First name
- Last name
- Phone number (optional)

**Optional Fields (can skip):**
- Company/firm name (can add later)

**Flow:**
1. User enters email/password
2. Click "Create Account"
3. Email verification sent (optional - some platforms skip this)
4. Redirect to plan selection

---

## 3. Plan Selection Page

**URL:** /signup/plan (or shown as modal)

**Plans Offered:**

| Plan | Price | Features |
|------|-------|----------|
| **Starter** | $0/mo | 1 user, 5 clients, basic features |
| **Advocate** | $129/mo | Advocacy workflows, case organization, parent support |
| **Evaluator** | $79/mo | Evaluation workflows, documentation, defensible reporting |

---

## 4. Add-ons Selection Page

**URL:** /signup/addons

**Add-ons Offered:**

| Add-on | Price | Description |
|--------|-------|-------------|
| **Compliance Engine** | $30/mo | Identify gaps and organize defensible compliance workflows |
| **Evidence Builder** | $40/mo | Turn scattered records into structured case evidence |
| **IEP Structuring** | $25/mo | Create organized, readable, and usable workflow outputs |
| **Workflow Automation** | $20/mo | Reduce repeated manual tasks and save time |
| **Analytics** | $25/mo | See trends, workload patterns, and case insights faster |
| **Team Access** | $30/mo | Support collaboration across staff or partners |

**Selection:**
- Checkboxes for each add-on
- Running total shown
- User can select multiple or skip all
- "Continue" button

---

## 5. Demo Mode (First - No Account Required)

**URL:** /demo (accessible from landing page)

**What Users See:**
- Fully interactive demo version of the platform
- Does NOT save their data
- Walks them through key features:
  - Adding a client
  - Creating a case
  - Processing an IEP
  - Generating an invoice
- "Try Demo" button prominent on landing page

---

## 6. Checkout/Billing Page (After Demo)

**URL:** /signup/billing

**User Flow:**
1. User tries demo and wants to continue
2. Prompts to create account or sign in
3. Selects plan (Advocate or Evaluator)
4. Selects add-ons (optional)
5. Enters billing info

**Billing:**
- Credit card input (Stripe)
- Billing address
- Coupon code field
- Order summary:
  - Plan: $X/mo
  - Add-ons: $X/mo
  - Total: $X/mo
- "Complete Subscription" button
- Terms of Service checkbox

**Post-Signup:**
- Stripe Customer ID created
- Subscription status: "active"
- Tenant created in database
- Redirected to Get Started guide

---

## 7. Onboarding Survey (Optional but Recommended)

**Shown after first login for new users:**

"Help us customize your experience"

| Question | Options |
|----------|---------|
| What's your role? | Advocate / Attorney / Parent / District / Other |
| What's your primary focus? | IEP Advocacy / Due Process / Compliance / Full-Service |
| How did you hear about us? | Search / Social / Referral / Other |

**Purpose:** Personalize dashboard, set defaults, inform marketing

---

## 8. First Login → Get Started Guide

**URL:** /app/onboarding (shown once)

**Checklist Items:**
1. ✅ Set up firm name and hourly rate
2. ✅ Add your first client
3. ✅ Create a student record
4. ✅ Upload your first document
5. ✅ Log your first time entry
6. ✅ Generate your first invoice

---

## 8. Post-Setup Dashboard

**URL:** /app/dashboard

**What's shown:**
- Getting Started widget (progress)
- Recent activity
- Upcoming deadlines
- Quick actions (New Client, New Case, Timer)

---

## Signup Summary Checklist

- [ ] Landing page with CTA
- [ ] Sign up form (email, password, name)
- [ ] Plan selection (Starter/Pro/Business)
- [ ] Add-ons selection (IEP Scanner, Portal, SMS, Priority)
- [ ] Checkout with Stripe
- [ ] Email verification (optional)
- [ ] Onboarding survey
- [ ] Get Started guide after login
- [ ] Welcome dashboard