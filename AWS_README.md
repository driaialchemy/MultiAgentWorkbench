# AWS — agentstack source README

**Repository:** `C:\Users\msell\OneDrive\aialchemyrepos\agentstack`  
**Remote:** https://github.com/driaialchemy/agentstack  
**Role:** Local governed multi-agent reference implementation (Phases 1–6)  
**AWS migration workspace (sibling):** `C:\Users\msell\OneDrive\aialchemyrepos\aws`  
**Governance rule:** **No charter, no run.**

This document is the AWS-migration-aware README for the source tree. Classic local docs remain: `README.md`, `AGENT_CHARTER.md`, `ARCHITECTURE.md`, `DEMO_SCRIPT.md`.

---

## Project purpose

Demonstrate a single integrated stack where agents, skills, policy, gates, audit, memory/recovery, and evidence reporting work together. Every workflow must bind to a completed charter and accountability owner.

**Portfolio proof:**

> I built a governed multi-agent intelligence stack that can produce evidence showing which agents acted, which skills were authorized, which gates passed or failed, what approvals were required, what risks were controlled, what failures occurred, and why the final output can or cannot be trusted.

---

## Current status (as of 2026-07-15)

### Source repository (`agentstack`)

| Area | Status |
|------|--------|
| Phases 1–6 | **Complete** (synthetic data only) |
| Stabilization pass | **Complete** (blocked envelopes, audit normalization, demo UI clarity, regression tests) |
| Phase 7 | **Not started** — do not add |
| AWS service code in source | **None** — source stays local Streamlit reference |
| Runtime artifacts | `storage/` and `reports/` gitignored beyond `.gitkeep`; repopulate on workflow runs |

| Phase | Capability |
|-------|------------|
| 1 | Streamlit shell, charter, synthetic data, audit log |
| 2 | Skill registry, policy engine, gate engine, skill executor |
| 3 | Governed agents, linear orchestrator |
| 4 | Analytics & reporting on synthetic structured data |
| 5 | Memory, checkpoints, quarantine, recovery, dead-letter |
| 6 | Cost/trust/security governance, approvals, incidents, evidence exports |

### AWS migration (`../aws`)

| Migration phase | Status |
|-----------------|--------|
| AWS-0 – AWS-3B | Not evidenced in the AWS workspace |
| AWS-4 — Free-tier architecture lock | **Complete** (2026-07-14) |
| AWS-5 — Cost and safety guardrails | **Complete** (2026-07-14) |
| AWS-6 — Infrastructure synth validation | **Next** — not started |
| AWS-7+ (endpoints, adapters, Streamlit bridge) | Not started |

**No AWS deployment is authorized** until a later phase explicitly allows account mutations. AWS-6 may authorize local `cdk synth` only — still no deploy unless that phase’s definition of done says so.

**Approved topology (locked in AWS-4):**

```text
Local Streamlit UI
        ↓
API Gateway HTTP API
        ↓
Lambda governed workflow handler
        ↓
DynamoDB + S3 + CloudWatch Logs
```

Same governance path on AWS: Charter → Orchestrator → Agent → Skill → Policy/Gates → Audit → Checkpoint/Evidence → Verified output.

---

## Repository boundaries

| Path | Role |
|------|------|
| `C:\Users\msell\OneDrive\aialchemyrepos\agentstack` | Source / functional reference — keep Phases 1–6 green |
| `C:\Users\msell\OneDrive\aialchemyrepos\aws` | Migration docs, CDK scaffolding, static guardrail tests |

Unless a future phase explicitly authorizes coordinated source changes, AWS migration work writes only under `aws/`.

---

## Install (local source)

```powershell
cd C:\Users\msell\OneDrive\aialchemyrepos\agentstack
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

## Launch Streamlit

```powershell
streamlit run app.py
```

Default URL: http://localhost:8501

## Run source tests

```powershell
python -m pytest tests/ -v
```

Last documented suite result: **85 passed** (verified 2026-07-06 after housekeeping).

## AWS workspace validation (sibling only)

```powershell
cd C:\Users\msell\OneDrive\aialchemyrepos\aws
python -m pip install -r requirements.txt
python -m pytest tests/ -q
```

Last documented AWS-5 result: **68 passed** (static tests; no AWS API calls).

---

## Key governance concepts

- **Charter gate:** incomplete charter blocks orchestration and governance skills.
- **Policy engine:** authorizes skills by registry, source, workflow, size, and approval state.
- **Gate engine:** pre/post checks; Phase 6 adds cost, trust, security, and human-approval gates when context supplies evaluations.
- **Skill executor:** sole execution path for skill and analytics logic.
- **Audit logger:** JSONL at `storage/audit_log.jsonl`.
- **Memory/recovery:** validate-before-write, checkpoints, quarantine, dead-letter.
- **Evidence:** policy/skill/audit summaries and governance verdict under `storage/evidence/`.

---

## Demo workflow (happy path)

1. Complete **Workflow Charter** (including accountability owner).
2. **Run Workflow** → structured preview + synthetic database.
3. Review **Output**, **Agents & Orchestration**, **Memory & Recovery**.
4. Open **Advanced Governance & Evidence** for verdict and artifacts.
5. Inspect **Audit / Evidence Viewer**.

Guided walkthrough: `DEMO_SCRIPT.md`.

---

## Repository structure (source)

```text
agentstack/
  app.py
  AGENT_CHARTER.md
  ARCHITECTURE.md
  DEMO_SCRIPT.md
  AWS_README.md              # this file
  AWS_CURRENT_HANDOFF.md     # migration-aware handoff
  agents/
  analytics/
  evidence/
  governance/
  memory/
  orchestration/
  skills/
  storage/
  reports/
  tests/
```

---

## Out of scope (source)

External APIs, real client data, production deployment, SSO, multi-tenant hosting, autonomous loops, dynamic routing, vector databases, RAG memory, full DLP, external compliance certification, Word/PDF/PPTX export, contract risk pipeline hardwiring, source Phase 7.

**Naming clarity:** Migration phase **AWS-7** (governed backend endpoints) is not the source project’s excluded Phase 7.

---

## Related AWS docs (sibling workspace)

- `C:\Users\msell\OneDrive\aialchemyrepos\aws\README.md`
- `C:\Users\msell\OneDrive\aialchemyrepos\aws\CURRENT_HANDOFF.md`
- `C:\Users\msell\OneDrive\aialchemyrepos\aws\docs\AWS_FREE_TIER_MIGRATION_WORKFLOW.md`
- `C:\Users\msell\OneDrive\aialchemyrepos\aws\docs\AWS_5_COST_AND_SAFETY_GUARDRAILS.md`

---

## License

See repository owner for license terms.
