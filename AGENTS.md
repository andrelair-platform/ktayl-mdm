# AGENTS.md — ktayl-mdm

Tiny repo-specific context. Org rules (`minicloud-gitops/.claude/rules/*`) still apply.

## Policy
- **ktayl-solution IS** product (IS Foundations). **NEVER involve Retrieva** (separate product).
- **MDM = operational golden records + référentiels** (transactional source of truth for *entity
  identity*): clients, brokers, insured legal entities, risk objects/sites + reference lists.
  - **≠ Data Platform #5** (that's analytical warehouse/BI; MDM feeds it, not the reverse).
  - **≠ domain data** — a policy lives in Policy Admin; the client it names resolves to an MDM golden ID.
- **Keep v1 a genuine thin slice** (see `docs/planning/brief.md` §5) — resist building a full MDM suite.
- The **data-ownership ADR is mandatory** (golden-ID scheme, matching strategy, entity boundaries).
- Deploy via GAP wrapper-chart + Kargo + CODEOWNERS. Never commit CLAUDE.md.

## What it is
Read-optimised référentiel service (PostgreSQL + REST/OpenAPI): create/upsert, get-by-golden-id,
search/lookup, list-references; conservative exact+fuzzy matching; audit columns. Primary consumer =
the **AI Ops Copilot** (#19) resolving a submission's parties, plus every business domain.

## Status
BMAD Path C (thin-slice-first). Product Brief drafted (`docs/planning/brief.md`) — awaiting sign-off.
Board **#20**.
