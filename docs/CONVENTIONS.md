# Documentation Conventions

This repository (`arthur-assistant-docs`) is the **single source of truth** for all AI assistant documentation.

**What goes here:**
- Prompt libraries (SPECIAL_EDUCATION_PROMPT_LIBRARY.md, etc.)
- PII redaction layers
- Knowledge bases (special-education-knowledge.md)
- Daily memory/journals
- Onboarding guides
- IDEA law deep dives
- Competitor research
- Codebase exploration notes
- Demo mode prompts
- Audit records
- Any other reference documentation

**What does NOT go here (stays in repo root):**
- AGENTS.md
- IDENTIFY.md (needs to be IDENTITY.md)
- SOUL.md
- TOOLS.md
- USER.md
- HEARTBEAT.md
- MEMORY.md (runtime file, managed by the AI)

These are **runtime configuration files** that the AI reads on startup. They belong in the workspace root, not in documentation.

---

## Repository Responsibilities

| Repo | Purpose |
|------|---------|
| `arthur-repo` | Core AI config (AGENTS.md, SOUL.md, USER.md, etc.) - NO .md files |
| `orvared` | Application code - NO .md documentation |
| `arthur-assistant-docs` | ALL documentation - THIS IS THE SOURCE OF TRUTH |

---

## Structure

```
docs/
├── *.md                    # Root docs (knowledge, prompts, guides)
├── memory/                 # Daily journals
├── orvared/                # Orvared-specific docs
├── idea-study/             # IDEA law deep dives
├── onboarding/             # Onboarding guides
└── ...
```