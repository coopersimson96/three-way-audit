<task>
You are an independent reviewer for {WHO}'s {PROCESS}. The complaint, verbatim: "{USER_QUOTE}".

Context: {ONE_PARAGRAPH: what shipped, what the goal was, what is frozen and may not change, the key facts with dates and numbers}.

Read, in this order:
1. The deliverable as shipped: {PATH}
2. The record of what was tried: ./rounds.md (in this folder)
3. The rules that governed the work: {PATHS: kill list, laws, stage contract, tests}
4. The data: {PATHS: what's-working, predictions, metrics}
5. Optional depth: {PATHS}

Deliver a file named in the last line of this prompt with exactly these sections:
A. ROOT CAUSE. Why did {THE FAILURE}? Name the failure in the PROCESS (what step was skipped or done in the wrong order), not just in the output. Rank up to 3 causes; the first must be the one that, if fixed, fixes the others.
B. EVIDENCE. Quote the exact lines from the rounds file and the rules and data that support each cause.
C. {CANDIDATES}. Draft {N} alternatives you believe are genuinely stronger than what shipped, under these constraints: {CONSTRAINTS}. For each: the shape, the anchor or receipt, the assumption it rests on, and why it beats the incumbent. One must be deliberately ten percent too far.
D. PROCESS FIX. The smallest change to the process that would have produced C in round one. Name the exact step, where it goes in the existing flow, and the artifact it produces. No new rule piles; one move.
E. CONFIDENCE. One paragraph: would you bet on C over the incumbent, by how much, reasoning against the data you read. Do not invent numbers.
</task>
<output_contract>Write your report to the path in the final line. Fill the result schema; status pass only if the report exists and has all five sections.</output_contract>
<verification_loop>test -s <report path> && grep -c "^[A-E]\." <report path> (must print 5 or more)</verification_loop>
<grounding_rules>Ground every claim in a file you read. Quote, don't paraphrase, when citing the complainant. Use only numbers found in the pack. If a file is missing, say so and continue.</grounding_rules>
<constraints>Read-only on every path above except your report file. Do not edit any deliverable, rule, or skill file. Do not print, echo, or write any secret, token, or API key. Do not run any network command.</constraints>
