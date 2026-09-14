# ktayl-mdm

> **Master Data / référentiels for the ktayl-solution IS** — canonical, deduplicated **golden records**
> + lookup APIs for the entities every domain shares (clients, brokers, insured legal entities, risk
> objects/sites) plus reference lists (country/currency/product/guarantee). The single source of truth
> for **entity identity** that Underwriting, Policy, Claims, Billing, Reinsurance and the **AI Ops
> Copilot** all resolve against.

**Layer:** ktayl-solution IS · IS Foundations · board **#20**. **Excludes Retrieva.**

## Status
BMAD **Path C** (thin-slice-first). See [`docs/planning/brief.md`](docs/planning/brief.md) — Product
Brief, awaiting sign-off. First delivery = the thin slice the AI Ops Copilot's underwriting v1 needs
(resolve a submission's client/broker/insured-entity/site to golden IDs).

## Not to be confused with
- **Data Platform (#5)** — analytical warehouse/BI. MDM is *operational* golden records and **feeds** Data.
- **Each domain's data** — MDM owns *entity identity*, not domain transactions.
