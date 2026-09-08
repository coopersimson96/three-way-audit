# Three-Way Audit

A root-cause audit for AI work that comes back weak. Claude Code acts as the manager: it writes and seals its own analysis first, two different models review the same evidence independently, a heavy model reconciles all three reports, and the manager ships one small process change into the file that already owns the failed step.

## Why you want this

The model that made a mistake carries the same blind spots when it grades the mistake. Ask it "are you sure?" and the same brain often returns "yes." This method forces independent views before the original model gets to decide what changes.

I built it after short-form hook revisions came back weak three rounds in a row. My sealed analysis and two blind reports all found the same skipped step: the manager chose a hook shape before finding a claim the video could prove. I added one eight-line step to the existing process, and the problem stopped.

The output is concrete: three root-cause reports, one reconciliation that tests their disagreements, and one process change that is already live when the audit ends.

## How it works

### Stage 0: scope and evidence pack

State the failure in one sentence, freeze what cannot change, and put the shipped deliverable, every attempt, the governing rules, relevant data, and unused facts into one read-only evidence pack. Missing facts are named instead of inferred.

### Stage 1: sealed manager analysis

Before any model is dispatched, the manager writes its own factual account, one-line root cause, and better path. The file is timestamped and never edited after round one starts. Its modification time is the receipt that the manager did not rewrite history after seeing the other reports.

### Stage 2: blind worker round

GPT-5.6 Sol and GPT-6 Astra receive the identical brief and evidence pack in parallel. Neither sees the manager analysis or the other worker's report. Each must identify the failed process step, ground it in quoted evidence, propose alternatives, name the smallest process fix, and state confidence without inventing numbers.

### Stage 3: reconciliation

Astra then reads all three analyses. It states their common ground, rules on every disagreement with evidence, ranks every proposed solution, and argues against its own first report wherever another analysis is stronger. It must specify one process change precisely enough to make in a single edit.

### Stage 4: manager verdict and the one change

The manager decides, implements exactly one change under 15 lines in the file that already owns the failed step, runs that file's lint or test, archives the receipts, and reports the result in BLUF. If an existing rule was skipped, the change fixes when that rule runs instead of adding another rule.

| Check | Pass condition |
|---|---|
| Sealed | `manager-analysis.md` mtime precedes both round-one dispatch times |
| Blind | Neither round-one worker was given the other's report or the manager file |
| Complete | Both reports and the compare report carry sections A to E |
| Grounded | Every performance number traces to a file in the pack |
| One change | Exactly one insert, under 15 lines, in the owning file, lint clean |
| Receipts | Any machine check named by the audit was actually run and its output kept |
| Archived | Folder copied under `audits/` before the report to the user |

## What you need

- Claude Code.
- The OpenAI Codex CLI, installed and logged in.
- Access to two models through Codex.

Set the default model in `~/.codex/config.toml`:

```toml
model = "gpt-5.6-sol"
```

Create the per-profile file `~/.codex/astra.config.toml`:

```toml
model = "gpt-6-astra"
model_reasoning_effort = "high"
```

The skill invokes that profile with `-p astra`. Any two different models work. The point is to use two brains that did not make the mistake.

## Install

Paste this into Claude Code:

> Install this skill globally: https://github.com/coopersimson96/three-way-audit

Or install it manually:

```sh
git clone https://github.com/coopersimson96/three-way-audit ~/.claude/skills/three-way-audit
```

## Use

```text
/three-way-audit
```

You can also say "three-way audit," "3-way audit," "get sol and astra to independently analyze," "compare all findings and land on," "post-mortem this with the workers," or "why did this come back weak."

Hand it the weak deliverable, what you tried, and the rules the work was supposed to follow. Include constraints and evidence that cannot be reconstructed later.

## Sample output

```text
BLUF | short-form hook audit

Root cause: The manager chose a hook shape before identifying a claim the
video could prove, so three revision rounds polished an unsupported premise.

Where the three agreed: All three reports identified the same skipped
proof-selection step and grounded it in the attempt log and process rules.

One change now live: An eight-line proof-first gate now runs inside the
existing hook step and blocks shape selection until a supportable claim is named.

Honest answer: Trust the process again only when the next run produces the
claim and its receipt before a hook shape. If that artifact is missing, stop.
```

## Gotchas

- A Codex `-o` result file can appear while the process is still running. Wait on the process ID, not the file.
- `resume` fails when the scratch folder is not a Git repository. Dispatch again with the full brief.
- The Astra profile needs the per-profile file shown above. A `[profiles.astra]` table in the main config is rejected by Codex CLI 0.153 and later.
- Workers may claim they ran a check they did not run. The manager reruns every check that affects the decision.

## License

MIT.
