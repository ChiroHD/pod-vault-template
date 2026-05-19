---
name: pod-vault-setup
description: One-shot interactive wizard that configures a freshly-cloned pod-vault-template into a customized pod vault. Asks the user about pod name, product line, project, members, theming, and registers the pod in the Notion registry. Runs once after clone, then archives itself.
---

# Pod Vault Setup Skill

Interactive wizard for the one-time setup of a new pod vault, after the user has cloned this repo from the `pod-vault-template` GitHub template.

## Trigger

User says **"set up the pod vault"**, **"run pod vault setup"**, **"/pod-vault-setup"**, or this skill is invoked automatically when Claude detects an un-customized clone of `pod-vault-template` (the presence of `_setup_artifacts/` and `{{POD_NAME}}` placeholders in CLAUDE.md is the signal).

## When NOT to run

- If `TEMPLATE_VERSION.md` already has actual values (not `{{...}}` placeholders), the vault has already been set up. Tell the user "this vault is already set up" rather than re-running.
- If the user is in `pod-vault-template` itself (the template repo, not a fork). Setup should not be run inside the template — only in clones of it.

## The Setup Pipeline

### Step 0: Sanity check — am I in the right place?

1. Verify the working directory contains `TEMPLATE_VERSION.md` with placeholder values
2. Verify the directory is a git repo, and that the remote is NOT `ChiroHD/pod-vault-template` (must be a clone, not the template itself)
3. If either check fails, explain the issue and stop

### Step 1: Greet + explain

```
Welcome to Pod Vault Setup!

I detected this is a fresh clone of pod-vault-template. I'm going to walk you
through ~5 minutes of setup to customize this vault for your specific pod
and project. Once done, you'll be ready to start ingesting content.

I'll ask about:
  1. Product line (ChiroHD or SKED)
  2. Pod name + members
  3. First project (name, slug, JIRA epic)
  4. Pod session cadence
  5. Vault theming (color palette)

Ready to start?
```

Wait for "yes" / equivalent. If the user wants to skip something, allow it — collect what they give and use sensible defaults for the rest.

### Step 2: Product line

```
Q1: Which product line is this pod working on?

  1. ChiroHD — chiropractic EHR platform
  2. SKED — separate Adio product line

Pick 1 or 2.
```

Apply choice:
- If ChiroHD: keep `shared/reference/chirohd/` content, delete `shared/reference/sked/.gitkeep` (or the whole sked directory if empty)
- If SKED: swap — keep `shared/reference/sked/` if it has content; otherwise warn the user that SKED reference content is empty and they'll be filling it in
- Record the choice as `{{PRODUCT_LINE}}` and `{{PRODUCT_LINE_SLUG}}` (e.g., `ChiroHD` and `chirohd`)

### Step 3: Pod name

```
Q2: What's the pod name?

This is the short identifier used in the repo name (pod-vault-<podname>) and in
some file paths. Existing pods: PJ, JM. Pick something distinct.

Example: PJ, JM, KL, MR

Your answer:
```

Validate: must be 2-3 letters, alphanumeric. Record as `{{POD_NAME}}` (uppercase, e.g., "MR") and `{{POD_SLUG}}` (lowercase, e.g., "mr").

### Step 4: Pod members

```
Q3a: PM name? (default: Jeff Williams)

Q3b: Engineer name + role?

Q3c: Any other regular pod members? (y/n)
```

Record as:
- `{{PM_NAME}}`, `{{PM_ROLE}}` (e.g., "Senior Product Manager"), `{{PM_GITHUB}}` (ask)
- `{{ENGINEER_NAME}}`, `{{ENGINEER_ROLE}}` (e.g., "Lead Software Engineer"), `{{ENGINEER_GITHUB}}` (ask)
- `{{ENGINEER_CODE_REPO}}` — ask if known, allow "TBD"

### Step 5: First project

```
Q4a: Project name (display version)?
     Example: "TRP Vital Signs Export", "PayFac Recurring Billing", "Alina Scheduling Assistant"

Q4b: Project slug (URL-safe, lowercase, hyphenated)?
     Example: "trp-export", "payfac-recurring-billing", "alina"

Q4c: Ticket system?
     1. JIRA (ChiroHD default)
     2. GitLab Issues (SKED default)
     3. Both — split tracked (e.g., Alina pod where one engineer uses JIRA personally,
        team mirrors to GitLab Issues)

     Auto-select based on {{PRODUCT_LINE}} if user just presses enter:
       ChiroHD → 1, SKED → 2

Q4d: Epic reference and URL?
     For JIRA: epic key (e.g., CHD-8366) and JIRA URL
     For GitLab: epic ref (e.g., sked/alina&42) and GitLab URL
     For Both: capture the JIRA epic key + URL first, then optionally a GitLab milestone/epic URL
     (Or "none" if epic isn't created yet — pod can fill in later)

Q4e: Notion problem brief URL? (optional — also covers SKED's Notion "Builds" workspace docs)

Q4f: Project keyword for cross-vault content discovery?
     This is used by `/ingest --from-vault` to find project content in the PM's
     personal vault. Example: "TRP", "PayFac", "Alina", "Drip".

Q4g: Target completion date (or "TBD")?

Q4h: Project advisor? (default: Dr. Cristina Esposito for ChiroHD pods;
     for SKED pods, ask — typically the departing/handoff PM or a SKED-side lead)
```

Record as `{{PROJECT_NAME}}`, `{{PROJECT_SLUG}}`, `{{TICKET_SYSTEM}}`, `{{EPIC_REF}}`, `{{EPIC_URL}}`, `{{NOTION_PROBLEM_BRIEF_URL}}`, `{{PROJECT_KEYWORD}}`, `{{PROJECT_TARGET_DATE}}`, `{{ADVISOR_NAME}}`, `{{ADVISOR_ROLE}}`.

### Step 6: Pod session cadence

```
Q5: Pod session cadence?

  1. Mon/Wed mornings, 8:30 AM – 12:30 PM ET (PJ pod's cadence — ChiroHD-style deep-work)
  2. Tue/Thu mornings, 8:30 AM – 12:30 PM ET (JM pod's cadence — ChiroHD-style deep-work)
  3. Continuous / async — no fixed session (typical SKED-side pattern; pods like Alina
     ship continuously without a fixed pod-session ritual)
  4. Other (specify days, times, or cadence in free-form text)

  Default suggestion based on {{PRODUCT_LINE}}: ChiroHD → 1 or 2, SKED → 3.
  But ask — SKED pods can choose deep-work, and ChiroHD pods can run continuously.
```

Record as `{{POD_SESSION_DAYS}}`, `{{POD_SESSION_TIME}}`, `{{POD_SESSION_DAY_1}}`, `{{POD_SESSION_DAY_2}}`. For "continuous/async" pick, set `{{POD_SESSION_DAYS}}` = `continuous`, `{{POD_SESSION_TIME}}` = `async`, and leave the per-day fields empty. The CLAUDE.md template handles both shapes.

### Step 7: Slack channel + Monday board + code repo (optional)

```
Q6: Slack channel name? (optional)
     ChiroHD examples: "#trp-vitalsigns", "#payfac-recurring"
     SKED examples: "#alina-pod", "#dripcampaigns", "#product-sked"

Q7: Monday board reference? (optional)
     ChiroHD: "Q2 2026 Roadmap — TRP item"
     SKED: "skedlife.monday.com/boards/<id>" — SKED uses its own Monday workspace

Q7c: Sibling code repo URL? (optional — used in CLAUDE.md Sibling Repos table)
     ChiroHD pods: a github.com/ChiroHD/... URL
     SKED pods: a gitlab.com/sked/... URL
     Skip if no code repo exists yet.
```

Record as `{{SLACK_CHANNEL}}`, `{{MONDAY_ROADMAP_REFERENCE}}`, `{{ENGINEER_CODE_REPO}}`.

### Step 8: Vault theming

This step is meaningful — each pod vault should have a visually distinct identity. The CSS palette flows through Obsidian's titlebar, sidebar, tabs, scrollbar, and terminal.

```
Q8: Vault theming — pick a color palette.

I'll check sibling pod vaults to see what's already taken, then present
options.

[Claude runs: ls ~/dev/chirohd/pod-vault-*/.obsidian/snippets/pod-identity.css
 and reads the `Pod Vault X — <name> identity` first line from each]

Currently taken:
  - Lavender (PJ)
  - Forest (JM)

Available palettes (defined in .claude/skills/pod-vault-setup/color-palettes.json):

  1. Crimson — deep red, bold + warm
  2. Ocean — deep blue, calm + technical
  3. Amber — warm gold, energetic
  4. Slate — cool gray-blue, restrained + professional
  5. Plum — deep purple, rich + saturated
  6. Teal — blue-green, fresh + crisp
  7. Rust — burnt orange, grounded + earthy
  8. Indigo — deep blue-purple, focused + serious
  9. Moss — muted olive-green, soft + natural
  10. Rose — dusty pink, warm + gentle
  C. Custom — provide your own hex value for the primary accent (the
     setup skill will derive light/dark/subtle/terminal-bg automatically)

Pick a number, letter, or palette name.
```

Apply choice:
- Read `color-palettes.json`
- Substitute the chosen palette's values into `.obsidian/snippets/pod-identity.css`, `.obsidian/snippets/terminal-bladerunner.css`, and `.obsidian/appearance.json`'s `accentColor`
- Record as `{{POD_THEME_NAME}}`, `{{POD_ACCENT_COLOR}}`, `{{POD_ACCENT_LIGHT}}`, `{{POD_ACCENT_SUBTLE}}`, `{{POD_ACCENT_DARK}}`, `{{POD_TERMINAL_BG}}`
- For custom: derive the lighter/darker/subtle variants from the provided hex using HSL adjustments (lighten 20%, lighten 35%, darken 25%, very dark variant for terminal). Surface the derived values for user confirmation before applying.

### Step 9: Confirmation summary

Display all collected values in a single confirmation block:

```
Setup summary:

Product line:       {{PRODUCT_LINE}}
Pod name:           {{POD_NAME}}
Pod members:        {{PM_NAME}}, {{ENGINEER_NAME}}
First project:      {{PROJECT_NAME}} ({{PROJECT_SLUG}})
Ticket system:      {{TICKET_SYSTEM}}
Epic ref:           {{EPIC_REF}}
Code repo:          {{ENGINEER_CODE_REPO}}
Pod cadence:        {{POD_SESSION_DAYS}}
Slack channel:      {{SLACK_CHANNEL}}
Theming:            {{POD_THEME_NAME}} ({{POD_ACCENT_COLOR}})

Proceed? (y to apply, n to revise)
```

### Step 10: Apply

When user confirms, execute in this order:

1. **Substitute placeholders.** Walk every file in the repo (skip `.git/`, `.obsidian/cache/`, `examples/`). For each `{{PLACEHOLDER}}` found, replace with the collected value. Files to scan: all `.md`, `.json`, `.css`, `.gitignore` patterns. Use the placeholder list compiled across all questions above.

2. **Rename project skeleton.** Move `projects/_template/` to `projects/{{PROJECT_SLUG}}/`. Keep the `_template` name for future projects in this pod? Leave a stub README in `projects/_template/` explaining the convention.

   Actually: copy `projects/_template/` to `projects/{{PROJECT_SLUG}}/` (keep both — `_template/` stays as a starting point for future projects in the same pod).

3. **Generate TEMPLATE_VERSION.md.** Get the current template's commit hash via:
   ```bash
   gh api /repos/ChiroHD/pod-vault-template/commits/main --jq .sha
   ```
   Populate `{{TEMPLATE_COMMIT_HASH}}` and `{{CREATED_DATE}}` (today, ISO format).

4. **Bootstrap per-project memory.**
   - Determine the per-project memory path: `~/.claude/projects/-Users-<user>-dev-chirohd-pod-vault-{{POD_SLUG}}/memory/`
   - Create the directory
   - Copy three memory templates from `.claude/skills/pod-vault-setup/memory-templates/` to that location, substituting placeholders ({{PM_NAME}}, {{ENGINEER_NAME}}, etc.)
   - Generate the `MEMORY.md` index

5. **Adio-aware product reference cleanup.**
   - If product is ChiroHD: delete `shared/reference/sked/` entirely (SKED reference content doesn't apply to a ChiroHD pod and would just be noise on the wikilink graph)
   - If product is SKED: keep the entire `shared/reference/sked/` tree (README + Tooling Map + Stakeholder Map + Process Notes + Glossary — the seed orientation a SKED pod needs on day one) and delete `shared/reference/chirohd/` (the new pod will reference ChiroHD content via cross-vault lookup if needed, not by carrying a full copy)

6. **Adio-aware people-notes curation.** The template seeds `shared/people/` with five Adio-wide profiles. Some apply to both product lines; some are ChiroHD-only.
   - **Always keep** (Adio-level execs, both product lines): `Cristina Esposito.md`, `Gabe Doty.md`, `Luke Doty.md`
   - **ChiroHD only**: `Ryan Caskey.md` (Head Developer, ChiroHD eng), `Ryan Oakes.md` (VP Payments, ChiroHD PayFac) — delete these if `{{PRODUCT_LINE}}` is SKED
   - **SKED-specific seed profiles** (only relevant if `{{PRODUCT_LINE}}` is SKED — the SKED stakeholder profiles live in `shared/reference/sked/SKED Stakeholder Map.md` and don't need to be promoted to `shared/people/` until the pod has actual interaction history with them; leave that promotion to the first SKED pod's first overhaul)

7. **Remove template-only files.** These exist for template contributors and shouldn't ship in a live pod:
   - Delete `PLACEHOLDERS.md` (catalog of `{{PLACEHOLDER}}` tokens — irrelevant once substitution has happened)
   - Delete `examples/demo-pod/` (worked example — the live pod doesn't need to carry the demo content; it's available in the template repo for reference)

8. **Generate the public-facing README.** The template ships with a setup-focused `README.md` for first-run guidance. Once setup completes, replace it with a live-pod README so the GitHub repo page shows useful context to anyone who finds it.
   - Read the live-pod template at `.claude/skills/pod-vault-setup/README-template.md`
   - Substitute the placeholders below, then overwrite `/README.md` in the new pod vault
   - Then delete `.claude/skills/pod-vault-setup/README-template.md` (no longer needed)

   Placeholders to fill in (in addition to the standard ones already collected):
   - `{{POD_ACCENT_COLOR_NOHASH}}` — `{{POD_ACCENT_COLOR}}` with the leading `#` stripped (shields.io URL format)
   - `{{PROJECT_ONE_LINE_DESCRIPTION}}` — derive from the user's earlier project answers; if a Notion problem brief was provided, read its first paragraph; otherwise ask the user for a one-sentence description
   - `{{SIBLING_PODS_LIST}}` — build a bulleted Markdown list by querying the Notion Pod Registry (data source `44fdeca6-f7f8-4b3c-b1f7-690f24e3f371`) for rows where `Status = Active` and `Pod Slug != {{POD_SLUG}}`. Format each row as:
     ```
     - **{{OTHER_POD_NAME}}:** [ChiroHD/pod-vault-{{OTHER_POD_SLUG}}](https://github.com/ChiroHD/pod-vault-{{OTHER_POD_SLUG}}) — {{OTHER_PROJECT_NAME}}
     ```
     If the registry query is unavailable, fall back to scanning `~/dev/chirohd/pod-vault-*/` for local sibling vaults and listing those.

9. **Create the GitHub repo.** Per the architectural decision (2026-05-19), pod vaults of BOTH product lines live in the `ChiroHD/` GitHub org for now — even SKED pod vaults. (Code repos differ — SKED code is on GitLab — but the pod vault itself is always a GitHub repo under `ChiroHD/`.)
   ```bash
   gh repo create ChiroHD/pod-vault-{{POD_SLUG}} --public --source=. --remote=origin --description "Pod vault for the {{POD_NAME}} pod working on {{PROJECT_NAME}} ({{PRODUCT_LINE}})"
   ```
   (Adjust public/private based on the user's preference if they ask; default to public matching siblings.)

10. **Register pod in Notion registry.**
   Use Notion MCP to append a row to the unified Pod Registry database (data source ID + URL in `.claude/skills/pod-vault-setup/notion-registry.md`). The same registry holds both ChiroHD and SKED pods; the `Product Line` column is the discriminator. Row data:
   - Pod Name: `{{POD_NAME}}`
   - Pod Slug: `{{POD_SLUG}}`
   - Product Line: `{{PRODUCT_LINE}}` (must be exactly `ChiroHD` or `SKED`)
   - Primary Project: `{{PROJECT_NAME}}`
   - Members: `{{PM_NAME}} (PM), {{ENGINEER_NAME}} (Engineer)`
   - GitHub Repo: `https://github.com/ChiroHD/pod-vault-{{POD_SLUG}}`
   - JIRA Epic / GitLab Epic: `{{EPIC_URL}}` (works for either system — the URL itself tells you which)
   - Status: Active
   - Created: today
   - Theme: `{{POD_THEME_NAME}}`
   - Template Commit: `{{TEMPLATE_COMMIT_HASH}}`
   
   If Notion MCP isn't available, surface the row data + the database URL so the user can paste manually.

11. **Initial commit + push.**
    ```bash
    git add -A
    git commit -m "Initial pod setup — {{POD_NAME}} pod, {{PROJECT_NAME}} project ({{PRODUCT_LINE}})"
    git push -u origin main
    ```

12. **Self-archive.**
   - Move this skill from `.claude/skills/pod-vault-setup/` to `_setup_artifacts/skills/pod-vault-setup/`
   - Add a `_setup_artifacts/README.md` explaining "These are the setup-time files preserved for reference. The setup skill auto-archived itself here after first run."
   - The skill is no longer loadable from `.claude/skills/` — exactly what we want.

### Step 11: Final message

```
✅ Pod vault setup complete!

What just happened:
  - All {{PLACEHOLDER}}s substituted across the vault
  - Project skeleton renamed to projects/{{PROJECT_SLUG}}/
  - TEMPLATE_VERSION.md populated with commit {{TEMPLATE_COMMIT_HASH}}
  - Per-project memory directory bootstrapped
  - GitHub repo created at https://github.com/ChiroHD/pod-vault-{{POD_SLUG}}
  - Pod registered in Notion at <Pod Registry URL>
  - Theming applied: {{POD_THEME_NAME}} ({{POD_ACCENT_COLOR}})
  - Setup skill archived to _setup_artifacts/

What to do next:
  1. Restart Obsidian (or reload) to pick up the new theme
  2. Read README.md for orientation
  3. Drop your first content (transcript, problem brief, etc.) into inbox/ or
     projects/{{PROJECT_SLUG}}/raw/, then say "ingest this"
  4. After a week of ingests, run /vault-overhaul to refresh indexes

If anything looks wrong: this is your vault — edit anything, or ask me to fix it.
```

## Important Rules

- **Run only once.** After Step 10, the skill is archived and cannot be re-run from `.claude/skills/`. If the user wants to re-do setup, they should re-clone the template.
- **Asks before applying.** Step 9 confirmation is mandatory. Don't skip it.
- **Defaults are sane.** If the user wants to skip a question, use the documented default and surface it in the confirmation.
- **Adio-aware, not generic.** This template is for Adio-internal use. If a future use case demands a fully-generic template, that's a separate project.
- **Theming is locked in.** Once chosen, the palette is recorded in pod-identity.css. To change it later, the user edits the CSS directly. Don't surface a "change theme" workflow in this skill.
- **Notion registry write is best-effort.** If it fails (auth, network, MCP not loaded), surface the row data so the user can paste manually. Don't block setup completion on it.

## Failure modes to handle

- **User doesn't have `gh` CLI installed.** Detect early, ask user to install + return.
- **User isn't authenticated to GitHub.** Suggest `gh auth login`.
- **Repo name collision** (`pod-vault-<slug>` already exists). Surface, ask user for an alternative slug.
- **Notion MCP not loaded.** Skip step 7 with a flagged "manual registration needed" + the row data printed.
- **User aborts mid-setup.** Don't apply partial changes — the placeholders should remain untouched so the next setup attempt starts clean.
- **Color palette already taken by sibling vault.** Warn the user, but allow override if they want to use it anyway.

## After setup

- The first task in `tasks.md` should be "Set up local dev environment" + "Ingest first content."
- The first log entry in `log.md` should reflect the setup event.
- Once the user has ingested their first piece of real content and the vault has > 0 decisions / meetings / raw sources, ask them to run `/vault-overhaul` to verify everything's clean.
