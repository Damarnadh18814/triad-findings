# TRIAD v2 — Findings Repository

Auto-published findings from **TRIAD**, an adversarial verified-reasoning system.

Every entry in this repository was produced by a four-model debate
(Proposer → Challenger → Judge, with a Phi-4 blind-spot Auditor), passed
atomic web-verification, survived a sycophancy role-switch test, and was
ruled `CONFIRMED` by an independent Judge model.

This repository is written to automatically by TRIAD's GitHub publishing
module. Initial commit seeds the `main` branch; subsequent findings arrive
as pull requests authored by the Judge.

## Layout

- `DISCOVERIES.md` — living research document
- `CHANGELOG.md` — every publish event
- `knowledge_base/confirmed/{topic}/{entry_id}.{json,md}` — confirmed entries
- `evidence/{entry_id}/sources.json` — verified sources per entry
- `meta/system_stats.json` — current system state