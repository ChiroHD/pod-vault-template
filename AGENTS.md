# Agent Instructions — Pod Vault `{{POD_NAME}}`

Cross-tool agent guidance. Most agent context lives in `CLAUDE.md`. This file is for tool-specific or agent-specific instructions that don't fit there.

## For Claude Code

Read `CLAUDE.md` first. Then this file.

### Core principles for working in this vault

1. **The vault is the source of truth for non-code knowledge.** When in doubt, check it. Don't make up facts about decisions, people, or status — read the vault.
2. **Three layers** — operational (current state), compiled (wiki synthesis), raw (history). Each has different update rules. See `CLAUDE.md`.
3. **Ingest skill (`/ingest`) is for new content.** Vault overhaul (`/vault-overhaul`) is for periodic audit. Don't confuse them.
4. **Living artifacts get reconciled on every ingest.** Listed in `.claude/skills/ingest/living-artifacts.md`. Mandatory, not optional.
5. **Wiki updates are mandatory on every ingest.** Even if the answer is "no new topic." See `feedback_wiki_synthesis.md` memory.
6. **Default to asking, not assuming.** When source material is ambiguous or contradicts other sources, surface the question rather than silently resolving. See `feedback_ingest_assumptions.md` memory.
7. **Transcripts must be captured for every meeting ingest.** See `feedback_ingest_transcripts.md` memory.
8. **Don't edit append-only files retroactively** — decisions, meeting notes, raw transcripts, log entries. Append only.

### Common requests + how to handle

| User request | What to do |
|--------------|------------|
| "Ingest this" | Invoke `/ingest` skill. Follow its full pipeline including wiki update + confidence checks. |
| "What decisions have we made about X?" | Read `projects/<project>/decisions/` filtered by topic. |
| "Run a vault overhaul" | Invoke `/vault-overhaul` skill. Mechanical fixes auto-apply; surface judgment calls. |
| "Draft a status update" | Read `tasks.md`, `plan.md`, recent log entries. Synthesize into the requested format. |
| "What's the project plan?" | Read `projects/<project>/plan.md`. |
| "Find <thing>" | Use grep/find against the vault. Match by content, not just filename. |

### What NOT to do

- Don't invent facts when the vault doesn't have them. Ask the user.
- Don't edit decision records or meeting notes after they're filed (only add `status: superseded` if applicable).
- Don't auto-resolve contradictions between sources — surface them.
- Don't commit + push without explicit user request (unless during an `/ingest` or `/vault-overhaul` workflow that explicitly ends with a commit step).
- Don't skip Step 4 of ingest (wiki update) because it requires synthesis judgment. Force the call.

## For other tools / agents

If you're a non-Claude agent operating in this vault: read CLAUDE.md + this file before making any changes. The vault has strict conventions about file naming, frontmatter, and update patterns; violating them creates drift that's hard to detect later.

If you only need to read the vault: feel free to read anything. The vault is designed to be Claude-readable as its primary interface.

## Feedback / improvement loop

If you (the agent) notice a recurring pattern that should be codified as a rule:

- For per-session enforcement: propose a new feedback memory file under `~/.claude/projects/<encoded-cwd>/memory/`
- For cross-vault enforcement: propose extending `.claude/skills/ingest/living-artifacts.md` registry
- For periodic audit: propose a new section in `.claude/skills/vault-overhaul/skill.md`

Surface the proposal to the user; don't make these structural changes silently.
