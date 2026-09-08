# Three-way audit procedure

## Stage 0: scope and evidence pack

1. Write the failure in one sentence, in the user's words when they gave them, quoted.
2. Name the frozen constraints (what may not change: a shipped body, a locked keyword, a budget).
3. Assemble the pack as files in one scratch folder: the deliverable as shipped, the record of what was tried (`rounds.md`: every attempt, verbatim, in order, with the user's reply after each), the rules that governed the work (kill lists, laws, stage contracts, tests), the data (what's-working, predictions, metrics), and any facts the manager had but did not use.
4. The pack is read-only for everyone. If a fact is missing, say so in the pack rather than letting a worker infer it.

## Stage 1: sealed manager analysis

Write `manager-analysis.md` before any dispatch. Sections: what happened (numbered, factual), root cause in one line, what would have produced a better result. Be blunt about the manager's own choices; this file is the control that keeps the workers honest and the manager accountable. Timestamp it (`ls -la` is the receipt). Never edit it after Stage 2 starts.

## Stage 2: blind worker round

1. Fill `templates/round1-brief.md`. Same text to both workers; only the report path differs.
2. Dispatch Sol (default profile) and Astra (`-p astra`) in parallel per `dispatch.md`. Read-only outside the folder, no network.
3. Wait for both. Revision Gate: open each report, confirm sections A to E, confirm quotes are real (spot-check two against the pack). A report that praises the manager or invents numbers is sent back once with the specific defect.
4. Do not summarize the reports to the user yet; the reconciliation comes first.

## Stage 3: reconciliation

1. Fill `templates/compare-brief.md`. Astra receives its own report, Sol's, and the sealed manager analysis.
2. Astra must: state where all three agree in one sentence; list every disagreement with a verdict and evidence, arguing against its own round one where the others are stronger; rank every candidate solution from all three sources; specify the single process change precisely enough to implement in one edit (file, insert point, text, artifact, blocking condition); write the three-sentence honest answer to the user.
3. Revision Gate: the manager checks that the recommended change lands in the file that already owns the step, is under 15 lines, and does not duplicate a rule that already exists. If the audit shows an existing rule was simply skipped, the change is a trigger fix (when the step runs), not a new rule.

## Stage 4: manager verdict and the one change

1. The manager decides. It may overrule the reconciliation, but must say why in one sentence, against the evidence.
2. Implement the one change. Run whatever lint or test owns that file. Run any blocking machine check the audit exposed on every candidate, including the incumbent.
3. Archive the whole folder to `<workspace>/audits/<date>-<slug>/`.
4. Write one `feedback_*` memory only if the lesson will recur; link it from MEMORY.md.
5. Report to the user in BLUF: root cause in one sentence, where the three agreed, the one change now live, the candidate slate if there is one, and the honest answer on whether to trust the process again and on what condition.

## Audit

| Check | Pass condition |
|---|---|
| Sealed | `manager-analysis.md` mtime precedes both round-one dispatch times |
| Blind | Neither round-one worker was given the other's report or the manager file |
| Complete | Both reports and the compare report carry sections A to E |
| Grounded | Every performance number traces to a file in the pack |
| One change | Exactly one insert, under 15 lines, in the owning file, lint clean |
| Receipts | Any machine check named by the audit was actually run and its output kept |
| Archived | Folder copied under `audits/` before the report to the user |
