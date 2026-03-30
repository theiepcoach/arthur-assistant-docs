# Architecture Layering & Circular Dependency Report

## Scan Date
Auto-generated as part of Phase 1 architecture hardening.

## Architecture Layers

```
┌─────────────────────────────────────────┐
│  pages / layouts  (consume everything)  │
├─────────────────────────────────────────┤
│  components       (UI only)            │
├─────────────────────────────────────────┤
│  hooks            (→ state, domain)     │
├─────────────────────────────────────────┤
│  state/contexts   (→ domain, data)      │
├─────────────────────────────────────────┤
│  data             (→ domain)            │
├─────────────────────────────────────────┤
│  domain/lib       (pure logic, types)   │
└─────────────────────────────────────────┘
```

Import direction: **downward only**. Type-only imports (`import type`) are exempt.

## Cycle Scan Results

| Category | Count |
|----------|-------|
| Critical cycles | 0 |
| Warning cycles | 0 |
| Layer violations | 1 (tolerated) |

### Tolerated Violation
- `src/hooks/use-toast.ts` → `@/components/ui/toast` — **type-only import** (`import type`), stripped at compile time. No runtime cycle. Marked as accepted.

## Enforcements Added

1. **`scripts/check-cycles.mjs`** — Run `node scripts/check-cycles.mjs` to scan for cycles
2. **ESLint `import/no-cycle`** — Warns on circular imports (max depth 4)
3. **ESLint `no-restricted-imports`** — Layer boundary rules:
   - `domain/lib` cannot import from state, hooks, components
   - `state/contexts` cannot import from hooks, components
   - `hooks` cannot import from components (type-only exempt)
   - `components/pages` unrestricted
4. **Folder READMEs** — `src/domain/`, `src/data/`, `src/state/` with documented rules

## Key Findings

| Finding | Status |
|---------|--------|
| contexts → hooks cycle risk | ✅ None found (contexts never import hooks) |
| lib → hooks cycle risk | ✅ None found |
| lib → components cycle risk | ✅ None found |
| hooks → components runtime import | ✅ Fixed (use-toast.ts uses type-only import) |
| Barrel export hidden cycles | ✅ None found |

## Remaining Warnings
None. Architecture is cycle-free.
