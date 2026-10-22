# Query cookbook

Worked examples per query shape. `RECALL_SCRIPT` names the absolute path of the
separately installed private engine.

## Exact identifier (strongest evidence — always include it)

```bash
python3 "$RECALL_SCRIPT" search "api-7f9c4b2d1e-xkq2p OOM"               # pod name
python3 "$RECALL_SCRIPT" search "3fa2b1c9 heartbeat job"                  # 8-hex uuid prefix works
python3 "$RECALL_SCRIPT" search "#4471"                                   # PR number
python3 "$RECALL_SCRIPT" search "myapp-base-v3"                           # snapshot/artifact name
```

Identifiers are matched exactly via the entity index and a dedicated token
leg — they surface even when they only ever appeared inside tool output.

## Error strings (quote the distinctive run of words)

```bash
python3 "$RECALL_SCRIPT" search "canceling statement due to lock timeout"
python3 "$RECALL_SCRIPT" search "driver failed programming external connectivity"
```

The full string is tried as a phrase before term matching; paste it verbatim.

## Time windows (mechanical, never narrated)

```bash
python3 "$RECALL_SCRIPT" search "greptile review" --since 2026-05-01 --until 2026-06-01
```

Timestamps are UTC — convert the user's local "yesterday"/"last week" before
setting the window, and prefer a window one day wider on each side.

## Cross-worktree / cross-harness

```bash
python3 "$RECALL_SCRIPT" search "billing harness origin" --cwd side-project   # work done in another checkout
python3 "$RECALL_SCRIPT" search "queue worker cron integration" --harness codex
python3 "$RECALL_SCRIPT" related --cwd ~/work/projects/myrepo                  # no query at all
```

## Paraphrase (you remember the shape, not the words)

```bash
python3 "$RECALL_SCRIPT" search "sandboxes kept timing out on the first command after a cold start"
```

Works when enough content words overlap. If it returns nothing or junk: add
any identifier you can recover (a filename, a branch word, an error fragment)
— one identifier converts a fuzzy query into an exact one. The WHY line shows
which terms carried each hit; if the WHY terms are all generic words, treat
the result as weak.

## Scripting

```bash
python3 "$RECALL_SCRIPT" search "…" --paths --limit 20     # bare paths, one per line, exit 0
```

Empty stdout = confident no-answer. Non-zero exit = the engine itself failed.
