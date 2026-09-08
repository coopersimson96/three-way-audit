# Dispatch commands for the three-way audit

`S` is the audit scratch folder. `SKILL_DIR` is the three-way-audit skill folder. Every dispatch: `</dev/null`, `--skip-git-repo-check` (scratch is not a repo), `nohup` with a log, workspace-write so the worker can write only its report inside `S`. The pack files are `chmod a-w` and fingerprinted in `pack.sha256` before any dispatch; verify with `shasum -a 256 -c "$S/pack.sha256"` after every round.

## Round one, both in parallel

```
nohup codex exec </dev/null -s workspace-write -c approval_policy="never" -c model_reasoning_effort=high \
  -C "$S" --skip-git-repo-check -o "$S/sol-result.json" \
  --output-schema "$SKILL_DIR/schemas/codex-result.schema.json" \
  "$(cat "$S/brief.md")

Report path: $S/sol-report.md" > "$S/sol.log" 2>&1 &

nohup codex exec </dev/null -p astra -s workspace-write -c approval_policy="never" \
  -C "$S" --skip-git-repo-check -o "$S/astra-result.json" \
  --output-schema "$SKILL_DIR/schemas/codex-result.schema.json" \
  "$(cat "$S/brief.md")

Report path: $S/astra-report.md" > "$S/astra.log" 2>&1 &
```

Wait with a background `while ps -p <pid1> >/dev/null || ps -p <pid2> >/dev/null; do sleep 10; done`. Typical run: 4 to 6 minutes each.

## Reconciliation

```
nohup codex exec </dev/null -p astra -s workspace-write -c approval_policy="never" \
  -C "$S" --skip-git-repo-check -o "$S/compare-result.json" \
  --output-schema "$SKILL_DIR/schemas/codex-result.schema.json" \
  "$(cat "$S/compare-brief.md")" > "$S/compare.log" 2>&1 &
```

## Gotchas seen on the first run (2026-09-06)

- A codex `-o` result file can appear while the process is still running; wait on the pid, not the file.
- `resume` on a scratch folder that is not a git repo dies; re-dispatch with the brief restated instead.
- `-p astra` needs `~/.codex/astra.config.toml` (per-profile file); a `[profiles.astra]` table in config.toml is rejected on codex-cli 0.153+.
- Workers will claim "verbatim" or "said aloud" checks they did not run. The manager reruns any check that matters.
