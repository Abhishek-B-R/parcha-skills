# Central Recall usage

Use configured Recall MCP tools when they are available. Central service setup and
implementation are distributed privately. This skill cannot install a service or
supply credentials. Keep an existing remote configuration remote if it fails;
do not silently fall back to a local index.

1. Search with `recall_search`, using useful identifiers, source filters, and UTC
   time boundaries. Treat search snippets as pointers to evidence.
2. Open returned `recall://` receipts with `recall_show`. Copy any structured
   target directly from the returned hit. Follow its continuation cursor until
   `complete` is true, keeping the same target.
3. Use `recall_session_context` for neighboring events. A receipt identifies a
   record, not the entire session's tail or original prompts.
4. Use `recall_exec` with returned `logical_document_id` values when a full
   document is needed. Follow the installed tool's schema and limits.
5. Cite only `opened_receipts` whose returned text supports the answer. State
   gaps when evidence is clipped, unavailable, or incomplete.

For the separately installed CLI, the operator sets `RECALL_SCRIPT` to its
absolute path. Run `python3 "$RECALL_SCRIPT" doctor` to check the configured
mode. Open a remote receipt with:

```bash
python3 "$RECALL_SCRIPT" show '<recall://receipt>'
```

Local file paths and `--tail`, `--around`, and `--prompts` flags do not open remote
receipts. If the configured service cannot export a complete session or native
relationship graph for Recap, report that limitation.

Capture durable information only when the user requests it. Prefer
`recall_capture` for selected decisions or findings and return the canonical
receipt. Do not capture secrets, hidden reasoning, whole transcripts, or
unselected third-party content. Forget only the exact receipt with
`recall_forget`; do not infer record identity from search text.

Source and visibility permissions come from the configured service. Default to
private capture, and use shared visibility only when the user explicitly
selects it. Never print credentials or search unrelated credential stores.
