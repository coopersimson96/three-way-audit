---
name: three-way-audit
description: >-
  Run an independent three-way root-cause audit on any process failure or weak deliverable. The manager (Claude Code) writes its own analysis first and seals it, GPT-5.6 Sol and GPT-6 Astra each analyze the same evidence pack blind, then Astra reconciles all three against each other and the manager lands one decision and one process change. Trigger phrases: "/three-way-audit", "three-way audit", "3-way audit", "get sol and astra to independently analyze", "compare all findings and land on", "post-mortem this with the workers", "why did this come back weak".
license: MIT
metadata:
  author: Cooper Simson
  version: 1.0.1
  source: https://github.com/coopersimson96/three-way-audit
---

# three-way-audit

Three blind analyses beat one apology. The manager's view is sealed before any worker output exists, the two workers never see each other's round one, and the heavy model does the reconciliation so the manager cannot grade its own homework.

## Flow

1. Read `references/procedure.md`, section "Stage 0: scope and evidence pack", and build the pack.
2. Read `references/procedure.md`, section "Stage 1: sealed manager analysis", and write `manager-analysis.md` BEFORE any dispatch.
3. Read `references/procedure.md`, section "Stage 2: blind worker round", fill `templates/round1-brief.md`, dispatch Sol and Astra in parallel with `references/dispatch.md`.
4. Read `references/procedure.md`, section "Stage 3: reconciliation", fill `templates/compare-brief.md`, dispatch Astra once more.
5. Read `references/procedure.md`, section "Stage 4: manager verdict and the one change", decide, implement the single process change, archive, report in BLUF.

## Hard constraints

Skill-specific, all blocking:

- Stage 1 file exists with a timestamp earlier than both Stage 2 dispatches, or the audit is void.
- The evidence pack is copied in, made read-only, and fingerprinted (`pack.sha256`) before any dispatch; a failed `shasum -c` after a round voids that round.
- Workers run read-only outside the audit folder, no network, no secrets; their reports must carry sections A to E or the result is a fail.
- Round one goes to Sol and Astra with the identical brief; the reconciliation goes to Astra with all three reports.
- Exactly one process change ships, under 15 lines, into the file that already owns that step; no new rule files. It is shown to the user as a diff and written only after an explicit yes; otherwise it is saved as `proposed-change.md` and nothing outside the audit folder changes.
- The audit folder is archived under the owning workspace's `audits/<date>-<slug>/`.

## What to load / do NOT load

| Task | Load | Do NOT load |
|---|---|---|
| Any stage | `references/procedure.md`, that stage's section only | The other stages |
| Writing a brief | The matching file in `templates/` | Both templates at once |
| Dispatching | `references/dispatch.md` | Any broader dispatch documentation unless a flag fails |
| Worked example | The audit folder for the current run | Any other audit folder |

## Outputs

| Artifact | Location |
|---|---|
| `manager-analysis.md`, `rounds.md` or `evidence.md`, `brief.md`, `sol-report.md`, `astra-report.md`, `compare-brief.md`, `compare-report.md` | `<scratchpad>/<slug>/` during the run, then `<workspace>/audits/<date>-<slug>/` |
| The one process change | The file that owns the failed step |
| Lessons | One line in your lessons or rules file if the lesson will recur |
