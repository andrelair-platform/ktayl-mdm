# Product Brief — ktayl Master Data (MDM)

> **BMAD artefact:** Product Brief (Path C — foundational, boundary-crossing → architecture gate applies).
> Status: **DRAFT — awaiting owner sign-off.** Board: **#20 ktayl Master Data (MDM)** (IS Foundations).
> Repo: `ktayl-mdm`. First delivery is a **thin slice** driven by the AI Ops Copilot's underwriting v1.

## 1. What this is (one paragraph)

The **master-data / référentiel layer** of the ktayl insurer IS: canonical, deduplicated **golden
records** + lookup/search APIs for the core business entities every domain shares — **clients
(policyholders), brokers/intermediaries, insured legal entities, risk objects/sites** — plus **reference
lists** (countries, currencies, products, guarantees, risk codes). It is the **transversal #3 layer** in
the [EA Blueprint](../../../minicloud-platform-docs) that Underwriting, Policy, Claims, Billing,
Reinsurance and the **AI Ops Copilot** all resolve against, so entities mean the same thing everywhere.

## 2. Why it exists (the gap it closes)

Today ktayl has **no master data**. Every domain would invent its own client/broker/entity records →
duplication, mismatch, and automations/AI that can't reliably resolve "which client/broker/site is
this?". The EA gap analysis flags MDM as 🔴 (no home) and one of the two biggest transversal gaps.
The copilot's underwriting v1 **cannot** do "client history / broker lookup / exposure by entity"
without it. **This is a prerequisite, not a nice-to-have.**

## 3. Layer + scope discipline (do not conflate)

- **ktayl-solution IS** product, IS Foundations. **Excludes Retrieva.**
- **MDM ≠ Data Platform (#5).** MDM = *operational* golden records + référentiels (transactional
  source of truth for entities). Data Platform = *analytical* warehouse/BI/actuarial. MDM feeds Data,
  not the reverse.
- **MDM ≠ each domain's own data.** A policy lives in Policy Admin; the *client the policy names*
  resolves to an MDM golden record. MDM owns the **entity identity**, not the domain transactions.

## 4. Target users / consumers

Machine-first: **the AI Ops Copilot** (#19) + every business domain service (Underwriting, Policy,
Claims, Billing, Reinsurance, Compliance) via API. Human: data stewards (dedup/merge, later).

## 5. Thin slice (v1 — what underwriting-v1 needs, nothing more)

**In (v1):**
- Canonical entities: **Client, Broker/Intermediary, Insured Legal Entity, Risk Object/Site.**
- Reference lists: **country, currency, product/line, guarantee** (small controlled vocabularies).
- **APIs:** create/upsert, get-by-id (stable golden ID), **search/lookup** (by name/SIREN/etc.),
  list-references. Read-optimised — this is what the copilot calls to resolve a submission's parties.
- **Minimal matching/dedup:** exact + simple fuzzy on name/registration id (SIREN) to avoid obvious
  duplicates; a golden ID per entity. Full stewardship/merge UI is **out of v1**.
- Auditable writes (who/when) + basic validation.

**Out (later):** full survivorship/golden-record rules, steward merge UI, hierarchy/group structures,
sanctions/PEP screening integration (that's Compliance), experts/reinsurers/partners référentiels,
event publication (added with the Integration layer), bulk EDI/broker feeds.

## 6. Success metrics (made measurable in the PRD)

- The copilot can resolve a submission's **client + broker + insured entity + site** to golden IDs.
- No duplicate golden record for the same real entity in the v1 test set (matching works).
- Every consuming domain reads entities from MDM (single source of truth), not local copies.
- Lookup latency fits interactive copilot use.

## 7. High-level approach

A small service on the platform golden path (GAP wrapper-chart + Kargo): a relational store
(PostgreSQL — golden records + references), a clean **read/write API** (REST/JSON; OpenAPI contract for
consumers), simple matching, audit columns. Env-agnostic image, ESO/Vault secrets, netpol-governed.
Deployed like the other custom services. **Architecture/data-ownership decisions → the architecture
gate** (golden-ID scheme, matching strategy, entity boundaries, how domains reference MDM).

## 8. Constraints

Resource-constrained cluster + tight quotas; EU residency + PII (client/entity data → Presidio-aware
handling, governed egress); GAP wrapper-chart + Kargo + CODEOWNERS prod gate; auditability (regulated
insurer). Keep v1 genuinely thin — resist building the full MDM suite.

## 9. Key risks

- **Scope creep** into a full MDM/stewardship platform → mitigated by the strict v1 thin slice above.
- **Entity-boundary disputes** (what MDM owns vs domains) → settled at the architecture gate + ADR.
- **Matching quality** (false merges) → conservative matching in v1; steward review deferred.

## 10. Delivery path & gates

**Path C** (foundational, data-ownership boundary) but delivered as a **thin slice first**. Gates:
brief sign-off → **architecture + data-ownership review** (golden-ID + matching + entity boundaries) →
thin-slice SPEC/epics → readiness gate. Because the pattern (référentiel CRUD/search service) is
well-understood, the PRD/architecture can be **compact** — we don't need the full heavy chain, but the
**data-ownership ADR is mandatory** (it's the whole point of MDM).
