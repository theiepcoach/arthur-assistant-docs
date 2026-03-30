# /src/domain

Pure logic, types, and constants. **No React, no hooks, no components.**

## Rules
- MUST NOT import from `/state`, `/hooks`, `/components`, `/pages`, `/layouts`
- MAY import from other `/domain` files or `/data`
- All exports should be pure functions, types, interfaces, or constants
