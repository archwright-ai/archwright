# ArchWright — Development Roadmap

*The architecture auditor for AI agents. This roadmap covers the capstone build
(~8 weeks, part-time). Items beyond the capstone are tracked under "Parking Lot".*

---

## Status snapshot

**Current phase:** Week 1–2 — Product Definition + Rulebook

- [x] Name, PyPI (`archwright-ai`) + GitHub claimed, v0.0.1 stub shipped
- [x] Dev environment (venv, uv, git workflow, YAML validation)
- [x] Rules drafted & validated: R-04, H-02, C-01, S-02 (4 of 8)
- [ ] Remaining rules: H-07, S-05, C-03, + one more (4 of 8)
- [ ] Charter one-pager + vision sentence
- [ ] Trace model on paper (Session → Step → LLMCall/ToolCall)

---

## Dependency spine

```
rulebook.yaml ──┐
                ├─► engine ──► scoring ──► report ──► benchmark ──► story
trace model ────┤
     └─ parser ─┘
```

Foundations (rulebook + trace model) are paper artifacts and come first — cheap to
change now, expensive to change once code depends on them.

---

## Phases

### Weeks 1–2 · Foundations  *(current)*
- **Build:** complete the 8-rule rulebook; one-page charter; internal trace-model sketch.
- **Artifacts:** `rules/rulebook.yaml`, `CHARTER.md`, data-model sketch.
- **Done when:** 8 rules validate; each rule's detection explainable in one breath.

### Weeks 2–3 · Ground Truth + Ingestion
- **Build:** demo LangGraph agent (RAG + tools); wire OTel file export; generate trace
  corpus; write parser (OTel spans → pydantic trace model).
- **Artifacts:** `demo_agent/`, trace JSON corpus, `archwright_ai/models.py`,
  `archwright_ai/ingest.py`.
- **Done when:** `archwright` loads a trace file and reports "N sessions, M steps".
- **Risk:** ingestion always takes longer than expected (OTel quirks, missing token
  fields). Budget slack.

### Weeks 3–5 · Rules Engine *(the core)*
- **Build:** engine skeleton (load rulebook → run rules → collect Findings); implement
  each rule as Python with pytest cases against synthetic traces.
- **Artifacts:** `archwright_ai/engine.py`, `archwright_ai/rules/*.py`, `tests/`.
- **Done when:** `archwright run ./traces/` produces findings; every rule has passing tests.
- **Order:** R-04 (structural) → H-02, C-01 (statistical) → S-02 (dataflow).

### Week 5 · Scoring + Cost Catalog
- **Build:** pillar scoring (findings → 0–100 per pillar → overall grade); pricing
  catalog (LiteLLM snapshot); Measured-vs-Estimated tagging.
- **Artifacts:** `archwright_ai/scoring.py`, `archwright_ai/pricing.py`, `pricing.json`.
- **Done when:** a run outputs pillar scores and dollar figures.

### Week 6 · Report
- **Build:** Jinja2 template (from the HTML mock); wire findings + scores in.
- **Artifacts:** `archwright_ai/report.py`, `templates/report.html.j2`.
- **Done when:** `archwright run ./traces/ --out report.html` produces the full report.
  **First end-to-end demo.**
- **Checkpoint:** if ahead, begin the optional LLM-judge stretch goal; if not, skip.

### Week 7 · Benchmark *(results chapter — protect this)*
- **Build:** 3–4 broken agent variants (one seeded anti-pattern each); scripted traffic;
  run auditor; compute precision/recall/F1 per rule.
- **Artifacts:** `benchmark/`, results table.
- **Done when:** e.g. "R-04 detector: 96% precision, 89% recall on 400 seeded traces."

### Week 8 · Story + Polish
- **Build:** before/after remediation experiment (score up, cost down); writeup; deck;
  demo rehearsal; buffer.
- **Artifacts:** capstone report, slides, README.
- **Done when:** 5-minute demo delivered without notes.

---

## Success criteria

- ≥85% precision / ≥80% recall across 8 rules on seeded faults
- 10k-trace audit completes in < 5 min on a laptop
- ≥40% demonstrated cost reduction on the demo agent with unchanged answer quality

---

## Stretch (only if ahead)

- One LLM-as-judge check (faithfulness on rule-flagged traces), BYO key, sampled.

---

## Parking Lot (designed, not built for capstone — discuss, don't implement)

- Live OTel collector service (v0 is batch CLI — ADR-001)
- Additional frameworks (CrewAI, ADK) beyond LangGraph
- Gemini Enterprise adapter (designed; partner go-to-market path)
- Log-ingestion fallback adapter (ADR-006)
- Outcome-label integration (cost-per-successful-outcome)
- SaaS / multi-tenancy / history-trending / CI gate
- Runtime enforcement mode (findings → active guardrails)

---

## Architecture Decisions (log)

- **ADR-000** — Name: ArchWright (`archwright-ai`).
- **ADR-001** — v0 is a batch CLI auditor, not a live service.
- **ADR-002** — SQLite/in-memory storage, not Postgres/ClickHouse.
- **ADR-003** — LangGraph only for capstone; framework-agnosticism argued via OTel.
- **ADR-004** — LLM-judge layer is a stretch goal, fully designed.
- **ADR-005** — Report = Jinja2 → static HTML.
- **ADR-006** — Canonical trace model; inputs are pluggable adapters; logs = deferred fallback.
- **ADR-007** — Rulebook in YAML (comments, prose, peer-tool precedent); machine outputs in JSON.