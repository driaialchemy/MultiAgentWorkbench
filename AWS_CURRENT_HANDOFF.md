# AWS — agentstack current handoff

**Updated:** 2026-07-15  
**Source repo:** `C:\Users\msell\OneDrive\aialchemyrepos\agentstack`  
**AWS workspace:** `C:\Users\msell\OneDrive\aialchemyrepos\aws`  
**Charter:** `AGENT_CHARTER.md` — **No charter, no run.**

Use this handoff for AWS-migration-aware context. The classic local handoff remains in `current-handoff.md`. Sibling AWS detail: `aws\CURRENT_HANDOFF.md`.

---

## Completed status

### Source (`agentstack`)

- **Phases 1–6:** complete (governed stack through advanced governance/evidence)
- **Stabilization pass:** complete (result envelope, audit normalization, demo docs, regression tests)
- **Housekeeping (2026-07-06):** stale demo artifacts removed from tracking; `storage/` and `reports/` gitignored beyond `.gitkeep`
- **Phase 7:** not started — do not add
- **AWS code in source:** none — remains local Streamlit reference

### AWS migration (`aws`)

- **AWS-4 — Free-tier architecture lock:** complete (2026-07-14)
- **AWS-5 — Cost and safety guardrails:** complete (2026-07-14)
- **AWS-6+:** not started
- **Deployment:** not authorized

---

## Latest test results

### Source suite

```text
cd C:\Users\msell\OneDrive\aialchemyrepos\agentstack
python -m pytest tests/ -q
→ 85 passed (documented 2026-07-06)
```

### AWS workspace suite (static only)

```text
cd C:\Users\msell\OneDrive\aialchemyrepos\aws
python -m pytest tests/ -v --tb=short
→ 68 passed in 0.10s (documented 2026-07-14)
```

No `cdk synth`, no deploy, no AWS API calls during AWS-5.

---

## Architecture summary

### Local governed path (source)

```text
Streamlit UI → Charter validation → LinearAgentOrchestrator → Agent
→ execute_skill → Policy engine → Gate engine → Skill body
→ Audit logger → Memory/checkpoint/recovery → Evidence exports
```

Linear only — no agent-to-agent messaging, no autonomous loops, no dynamic routing.

### Approved AWS topology (locked AWS-4)

```text
Local Streamlit UI
        ↓
API Gateway HTTP API
        ↓
Lambda governed workflow handler
        ↓
DynamoDB (audit / run / checkpoints / DLQ metadata)
S3 (evidence / reports / artifacts)
CloudWatch Logs (limited retention)
```

Governance path is preserved on AWS; API Gateway is a thin front door, not a second orchestrator.

---

## High-signal files

### Source (functional reference)

- `agents/`, `skills/`, `governance/`, `memory/`, `evidence/`, `orchestration/`, `analytics/`
- `app.py`, `AGENT_CHARTER.md`, `ARCHITECTURE.md`, `DEMO_SCRIPT.md`
- `tests/test_phase*.py`, `tests/test_stabilization_hardening.py`
- `AWS_README.md`, `AWS_CURRENT_HANDOFF.md` (this pair)

### AWS workspace (migration)

- `docs/AWS_FREE_TIER_MIGRATION_WORKFLOW.md` — architecture lock
- `docs/AWS_5_COST_AND_SAFETY_GUARDRAILS.md` — cost/safety guardrails
- `infra/guardrails.py`, `infra/config.py`, `infra/stack.py`, `app.py`, `cdk.json`
- `tests/test_denied_services.py`, `tests/test_guardrails.py`
- `README.md`, `CURRENT_HANDOFF.md`

---

## Known risks / unfinished work

- Source working tree may still show pre-existing local dirty state (README/handoff edits, deleted runtime artifacts, untracked `agentstack_project_summary.docx`) — treat as pre-existing unless a phase authorizes cleanup commits.
- Full-suite source run may occasionally flake on `test_orchestrator_runs_full_analytics_workflow` under load — re-run if it fails once.
- Cost/trust/security/approval gates activate only when context supplies evaluation data (by design).
- Live demo walkthrough via `DEMO_SCRIPT.md` not confirmed by a human dry-run in this session.
- AWS-0–AWS-3B still not evidenced historically in the AWS workspace.
- CDK app exists in `aws/` but **must not** be synth’d until AWS-6 authorizes it.
- Operator manual account steps (MFA, budget alerts, non-root identity, region) are documented in AWS-5 but not claimed complete.

---

## Exact next task

**AWS-6 — Infrastructure synth validation** (work only under `aws/`)

1. Do not modify `agentstack` unless a future phase explicitly authorizes coordinated source changes.
2. Do not deploy; do not create/modify AWS account resources unless AWS-6 DoD explicitly allows it.
3. If authorized: local `cdk synth` validation only, then record results in the AWS workspace handoff/README.
4. Do **not** implement endpoints (AWS-7), storage adapters (AWS-8), or Streamlit adapter (AWS-9).
5. Preserve **No charter, no run.**

### Optional parallel (source only — no new features)

If validating the local demo instead of migration:

1. `streamlit run app.py`
2. Walk `DEMO_SCRIPT.md` end-to-end
3. Fix only bugs found — no Phase 7, no new capabilities

---

## Commands

```powershell
# Source
cd C:\Users\msell\OneDrive\aialchemyrepos\agentstack
python -m pytest tests/ -v
streamlit run app.py

# AWS workspace (static tests only until AWS-6)
cd C:\Users\msell\OneDrive\aialchemyrepos\aws
python -m pytest tests/ -v
```

---

## Strict out of scope

Do **not** add or expand:

- Source Phase 7 or new major source capabilities
- External APIs, real client data, production deployment claims
- SSO, multi-tenant hosting, external compliance certification
- Autonomous loops, dynamic routing, agent-to-agent messaging
- Vector DBs, RAG, long-term memory tiers
- Full DLP, Word/PDF/PPTX export
- Contract risk pipeline hardwiring
- Bedrock / NAT / VPC / RDS / OpenSearch / EKS / SageMaker / EC2 / ECS / Fargate / Step Functions unless a new architecture lock explicitly permits them
- AWS deploy before an authorized phase

Preserve existing Phase 1–6 behavior; source tests must stay green. AWS migration writes stay under `aws/` by default.
