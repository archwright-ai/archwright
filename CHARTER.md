# ArchWright — Project Charter

**The Architecture Auditor for AI Agents** — *"SonarQube for AI Agents"*

---

## Vision

Make the architectural quality of AI agents measurable — so teams can prove a
deployed solution follows industry best practices, the way SonarQube made code
quality measurable.

## Problem Statement

Teams deploy agentic AI solutions with rich telemetry but no straight answer to a
basic question: *is the architecture actually working as intended?* Existing tools
are descriptive — they show what happened on a dashboard — but none evaluate whether
the agent is *well-built*. ArchWright audits a deployed agent against industry best
practices and produces a report that gives teams and their customers confidence the
solution is built to last.

## Target Users

**1. Developer / Architect — the practitioner.**
Builds and maintains the agent. Today they can see traces on a dashboard but can't
tell whether the design is sound or where to improve. ArchWright gives them specific,
trace-grounded findings with remediation steps they can act on — the "what to fix and
why" a dashboard never provides. *The refine-and-remediate loop.*

**2. Practice Lead / Principal Architect — the assurer.**
Owns quality across many client engagements. Today they have no consistent, objective
way to compare or vouch for the agent solutions their teams deliver. ArchWright gives
them a repeatable audit against a published rulebook — the same yardstick applied to
every engagement — turning "I trust my team" into "here's the scored evidence."
*The cross-engagement audit.*

**3. Customer / Client Stakeholder — the recipient.**
Receives the delivered solution and must justify it inside their own organization.
Today they take the vendor's word for quality. ArchWright gives them a measurable,
standards-referenced report they can carry to their own leadership as independent
evidence the solution meets industry best practices. *The assurance artifact.*

> **One artifact, three audiences:** the same audit output serves as a to-do list
> for the developer, a scorecard for the practice lead, and an assurance certificate
> for the customer.

## Goals & Success Criteria

*(Targets below are validated in the Week 7 seeded-fault benchmark.)*

1. **Detection accuracy** — ≥95% precision on structural rules; ≥80% precision and
   recall on statistical rules; measured on the seeded-fault benchmark with ≥30
   traces per rule.
2. **Cost value** — ≥40% cost-per-task reduction on the demo agent from cost findings,
   with no answer-quality regression on a fixed eval set.
3. **Responsiveness** — applying the top-3 findings raises the overall architecture
   score by ≥1 grade band.
4. **Performance** — a 10,000-trace audit completes in <5 minutes on a laptop.
5. **Quality** — all 8 rules unit-tested, CI green; the tool runs on traces alone and
   degrades gracefully when config/payloads are absent.

## Scope

**In scope:** OTel batch-file ingestion; 8 deterministic rules across 5 pillars;
LangGraph demo agent; pillar scoring + HTML report; seeded-fault benchmark;
cost-catalog with Measured/Estimated tagging.

**Out of scope (Parking Lot):** live OTel collector; other frameworks (CrewAI/ADK);
LLM-judge layer (stretch only); SaaS / multi-tenancy; realtime alerting; Gemini
Enterprise adapter; log-ingestion adapter; outcome-label integration.

## Assumptions & Constraints

- ~8 weeks, solo, <10 hrs/week, Python.
- Agents emit OpenTelemetry `gen_ai.*` traces.
- OTel agent conventions are experimental — the parser tolerates schema drift.
- Deterministic-first design: no LLM required for core findings.
- Customer-facing credibility rests on the external, published rulebook — not on
  ArchWright's self-assessment.

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Ingestion takes longer than planned | Batch-file approach de-risks the pipeline |
| Scope creep starves the benchmark | Rules frozen at 8; benchmark work protected |
| OTel schema drift | Parser tolerates missing/renamed fields |
| Over-claiming certainty in reports | Framed as conformance-to-best-practices, not a correctness guarantee |

## Deliverables

CLI tool (`archwright run`), published rulebook (8 rules), HTML audit report,
seeded-fault benchmark + results, LangGraph demo agent, and capstone writeup +
presentation.

---

*See `ROADMAP.md` for the phased plan and `ADR-000`–`ADR-007` for architecture
decisions.*