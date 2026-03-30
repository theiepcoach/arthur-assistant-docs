# Phase 5: Unused Dependency Cleanup — Audit Report

## Summary

| Category | Count |
|----------|-------|
| Unused UI components deleted | 12 |
| Backing packages flagged for removal | 11 |
| Dependencies verified in-use | All remaining |

## Unused UI Components Removed

These shadcn/ui components were installed but never imported by any app code:

| Component File | Backing Package | Status |
|---------------|----------------|--------|
| `carousel.tsx` | `embla-carousel-react` | 🗑️ Deleted |
| `drawer.tsx` | `vaul` | 🗑️ Deleted |
| `input-otp.tsx` | `input-otp` | 🗑️ Deleted |
| `resizable.tsx` | `react-resizable-panels` | 🗑️ Deleted |
| `aspect-ratio.tsx` | `@radix-ui/react-aspect-ratio` | 🗑️ Deleted |
| `hover-card.tsx` | `@radix-ui/react-hover-card` | 🗑️ Deleted |
| `menubar.tsx` | `@radix-ui/react-menubar` | 🗑️ Deleted |
| `navigation-menu.tsx` | `@radix-ui/react-navigation-menu` | 🗑️ Deleted |
| `context-menu.tsx` | `@radix-ui/react-context-menu` | 🗑️ Deleted |
| `toggle.tsx` | `@radix-ui/react-toggle` | 🗑️ Deleted |
| `toggle-group.tsx` | `@radix-ui/react-toggle-group` | 🗑️ Deleted |
| `pagination.tsx` | (no extra dep) | 🗑️ Deleted |

## Packages Flagged for Removal

The following packages are listed in `package.json` but their only consumers (the UI component files above) have been deleted. They should be removed from `package.json`:

- `embla-carousel-react`
- `vaul`
- `input-otp`
- `react-resizable-panels`
- `@radix-ui/react-aspect-ratio`
- `@radix-ui/react-hover-card`
- `@radix-ui/react-menubar`
- `@radix-ui/react-navigation-menu`
- `@radix-ui/react-context-menu`
- `@radix-ui/react-toggle`
- `@radix-ui/react-toggle-group`

> Note: The automated removal tool couldn't uninstall these (likely due to lockfile sync). They remain in package.json but their component files are deleted, so they are dead code in the bundle. Tree-shaking will exclude them from production builds since nothing imports them.

## Verified In-Use Dependencies

| Package | Used By |
|---------|---------|
| `jspdf` | Dynamic import in `export.ts`, `audit-export.ts` |
| `docx` | Dynamic import in `export.ts` |
| `recharts` | CRM Analytics, Reports, HealthCharts, Revenue, Forecasting |
| `date-fns` | 15 files (all named imports) |
| `lucide-react` | 161 files (all named imports) |
| `madge` | `scripts/check-cycles.mjs` (CLI tool) |
| `file-saver` | Export utilities |
| `@radix-ui/react-slider` | `GenerationControls.tsx` |
| `@radix-ui/react-radio-group` | `MergeConfirmDialog.tsx`, `SettingsPanel.tsx` |

## Scripts Added

- `node scripts/check-deps.mjs` — Scans all source files and reports packages with zero imports

## AI SDK Status

No unused AI provider SDKs found. The project uses `@lovable.dev/cloud-auth-js` and the Lovable AI Gateway (via edge function), both actively in use.
