<task>
Round two. You wrote astra-report.md. Two other analyses of the same failure now exist in this folder: manager-analysis.md (written by the manager who did the failed work, sealed BEFORE any worker output) and sol-report.md (independent). Read all three plus rounds.md and the shipped deliverable again.

Produce compare-report.md with exactly these sections:
A. WHERE ALL THREE AGREE. The root cause in one sentence all three would sign, then the agreed failure chain.
B. WHERE THEY DISAGREE. Every material disagreement, who holds which position, your verdict with evidence. Be adversarial toward your own report where the others have the better argument, and say explicitly when you change a round-one conclusion.
C. THE SLATE. Rank every candidate from all three sources plus the incumbent for the stated goal. One line each. Name the top three as genuinely different directions, state the recommended lock, and rewrite a candidate only for a concrete named flaw. If the incumbent belongs in the top three, say so.
D. THE ONE PROCESS CHANGE. Pick the owning file from {CANDIDATE_FILES} (read them first). Give the exact text to insert, where, the artifact it produces, and the blocking condition. Under 15 lines. State the revision rule in one sentence. No new files.
E. THE HONEST ANSWER. Three plain sentences: why the work was weak, what changes, whether the process should be trusted again and on what condition.
</task>
<output_contract>Write compare-report.md in this folder. Fill the result schema; status pass only if the file has all five sections.</output_contract>
<verification_loop>test -s compare-report.md && grep -c "^[A-E]\." compare-report.md</verification_loop>
<grounding_rules>Quote, don't paraphrase, when attributing a position. Use only numbers from the pack.</grounding_rules>
<constraints>Read-only everywhere except compare-report.md. No edits to any rule, stage, or deliverable file. No network. No secrets.</constraints>
