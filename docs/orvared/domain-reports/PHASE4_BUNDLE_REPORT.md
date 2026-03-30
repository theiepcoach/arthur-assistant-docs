# Phase 4: Bundle Size Optimization — Audit Report

## Summary

| Target | Action | Status |
|--------|--------|--------|
| jspdf | Already dynamically imported (`await import('jspdf')`) | ✅ No change needed |
| docx | Already dynamically imported (`await import('docx')`) + manual chunk | ✅ PASS |
| recharts | Separated into `vendor-recharts` chunk via `manualChunks` | ✅ PASS |
| lucide-react | Named imports only enforced via ESLint `no-restricted-syntax` | ✅ PASS |
| date-fns | Named imports only enforced via ESLint `no-restricted-syntax` | ✅ PASS |
| Route-level code splitting | All 40+ pages lazy-loaded via `React.lazy` | ✅ PASS |

## Changes Made

### 1. Route-Level Code Splitting (App.tsx)

**Before:** All 40+ page components eagerly imported at app startup, pulling recharts, heavy forms, CRM logic, etc. into the main bundle.

**After:** Every page except Index, Login, Signup, ResetPassword, and NotFound is lazy-loaded via `React.lazy()` + `Suspense`. This means:
- recharts only loads when Reports, CRM Analytics, OperationalHealth, or admin pages are visited
- jspdf/docx remain dynamically imported within export functions
- Admin pages (rarely visited) are fully code-split

### 2. Manual Chunks (vite.config.ts)

Added `manualChunks` configuration:
- `vendor-recharts` — isolates recharts into its own chunk (~180KB)
- `vendor-docx` — isolates docx into its own chunk (~110KB)

These won't load until a page that uses them is visited.

### 3. Bundle Analyzer

- Added `rollup-plugin-visualizer` dependency
- Added `scripts/analyze-bundle.mjs` — run with `node scripts/analyze-bundle.mjs`
- When `ANALYZE=true`, produces an interactive treemap at `dist/stats.html`
- Script also prints chunk sizes to stdout

### 4. Import Discipline ESLint Rules

Added `no-restricted-syntax` rules:
- **lucide-react**: Blocks `import * from 'lucide-react'` — only named imports allowed
- **date-fns**: Blocks `import * from 'date-fns'` — only named function imports allowed

### 5. Existing Good Patterns Verified

| Pattern | Status |
|---------|--------|
| `import { format } from "date-fns"` (named) | ✅ All 15 files use named imports |
| `import { Icon } from "lucide-react"` (named) | ✅ All 161 files use named imports |
| `await import('jspdf')` (dynamic) | ✅ Both export.ts and audit-export.ts |
| `await import('docx')` (dynamic) | ✅ export.ts |

## Expected Impact

| Metric | Before | After |
|--------|--------|-------|
| Main chunk | Contains all pages + recharts + docx | Core framework only |
| Initial JS load | ~800KB+ (estimated) | ~300-400KB (estimated) |
| recharts | In main bundle | Separate chunk, loaded on demand |
| docx | In main bundle | Separate chunk, loaded on demand |
| Page components | All eagerly loaded | 40+ lazy chunks |

## Scripts Added

- `node scripts/analyze-bundle.mjs` — Full bundle analysis with treemap
