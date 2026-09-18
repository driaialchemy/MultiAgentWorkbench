# TRUST taxonomy — agentstack

Classification vocabulary only. No runtime behavior change.

## T — Traceability

Audit events are append-only JSONL records written by `governance/audit_logger.py` (`log_event`, `log_skill_event`, `log_agent_event`) to `storage/audit_log.jsonl`. Evidence bundles reuse those events in `evidence/audit_evidence_report.py` and `evidence/evidence_exporter.py`, so a run can show which agent, skill, policy decision, and gate result produced an outcome.

## R — Runtime-guardrails

Charter validation is the hard runtime gate: incomplete charter or missing accountability owner blocks the run (“No charter, no run”). Field checks live in `governance/workflow_charter.py`; orchestration enforces them in `orchestration/linear_orchestrator.py` before skills execute.

## U — Uncertainty-monitoring

Memory and recovery treat invalid or untrusted state as uncertain rather than silently continuing. Write validation is in `memory/memory_validator.py`; failed or suspect items go to `memory/quarantine_store.py`. Resume/rollback uses `governance/checkpoint_manager.py` and `governance/recovery_manager.py`; unrecoverable work is parked in `governance/dead_letter_queue.py`.

## S — Staged-autonomy

The gate engine in `governance/gate_engine.py` stages what an agent may do: pre-gates before skill execution, post-gates after, plus Phase 6 cost, trust, security, and human-approval gates. Policy authorization that feeds those gates is in `governance/policy_engine.py`; approval holds sit in `governance/approval_queue.py`.

## T — Testing-via-simulation

Gap — not yet implemented. This repo has pytest coverage of live workflow paths (`tests/`) but no dedicated simulation harness for dry-run or counterfactual agent runs.
