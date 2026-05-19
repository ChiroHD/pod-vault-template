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
     Example: "TRP Vital Signs Export", "PayFac Recurring Billing"

Q4b: Project slug (URL-safe, lowercase, hyphenated)?
     Example: "trp-export", "payfac-recurring-billing"

Q4c: JIRA epic? (or "none" if not created yet)
     Example: CHD-8366

Q4d: Notion problem brief URL? (optional)

Q4e: Project keyword for cross-vault content discovery?
     This is used by `/ingest --from-vault` to find project content in the PM's
     personal vault. Example: "TRP" or "PayFac".

Q4f: Target completion date (or "TBD")?

Q4g: Project advisor? (default: Dr. Cristina Esposito for ChiroHD pods)
```

Record as `{{PROJECT_NAME}}`, `{{PROJECT_SLUG}}`, `{{JIRA_EPIC}}`, `{{JIRA_EPIC_URL}}`, `{{NOTION_PROBLEM_BRIEF_URL}}`, `{{PROJECT_KEYWORD}}`, `{{PROJECT_TARGET_DATE}}`, `{{ADVISOR_NAME}}`, `{{ADVISOR_ROLE}}`.

### Step 6: Pod session cadence

```
Q5: Pod session days?

  1. Mon/Wed mornings (PJ pod's cadence)
  2. Tue/Thu mornings (JM pod's cadence)
  3. Other (specify)

Pick 1, 2, or 3.
```

Record as `{{POD_SESSION_DAYS}}`, `{{POD_SESSION_TIME}}` (default "8:30 AM – 12:30 PM ET"), `{{POD_SESSION_DAY_1}}`, `{{POD_SESSION_DAY_2}}`.

### Step 7: Slack channel + Monday board (optional)

```
Q6: Slack channel name? (optional, e.g., "#trp-vitalsigns")
Q7: Monday board reference? (optional, e.g., "Q2 2026 Roadmap — TRP item")
```

Record as `{{SLACK_CHANNEL}}`, `{{MONDAY_ROADMAP_REFERENCE}}`.

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
JIRA epic:          {{JIRA_EPIC}}
Pod session days:   {{POD_SESSION_DAYS}}
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
   - If product is ChiroHD: delete `shared/reference/sked/` entirely (the README there is template-only context that doesn't apply to a live ChiroHD pod)
   - If product is SKED: keep `shared/reference/sked/README.md` (the TBD note is the right starting state) and delete `shared/reference/chirohd/` (the new pod will reference ChiroHD content via cross-vault lookup if needed, not by carrying a full copy)

6. **Remove template-only files.** These exist for template contributors and shouldn't ship in a live pod:
   - Delete `PLACEHOLDERS.md` (catalog of `{{PLACEHOLDER}}` tokens — irrelevant once substitution has happened)
   - Delete `examples/demo-pod/` (worked example — the live pod doesn't need to carry the demo content; it's available in the template repo for reference)

7. **Create the GitHub repo.**
   ```bash
   gh repo create ChiroHD/pod-vault-{{POD_SLUG}} --public --source=. --remote=origin --description "Pod vault for the {{POD_NAME}} pod working on {{PROJECT_NAME}}"
   ```
   (Adjust public/private based on the user's preference if they ask; default to public matching siblings.)

8. **Register pod in Notion registry.**
   Use Notion MCP to append a row to the Pod Registry database (database ID stored in `.claude/skills/pod-vault-setup/notion-registry.md` after Phase 4 of the build). Row data:
   - Pod Name: `{{POD_NAME}}`
   - Product Line: `{{PRODUCT_LINE}}`
   - Primary Project: `{{PROJECT_NAME}}`
   - Members: `{{PM_NAME}}, {{ENGINEER_NAME}}`
   - JIRA Epic: `{{JIRA_EPIC_URL}}`
   - GitHub Repo: `https://github.com/ChiroHD/pod-vault-{{POD_SLUG}}`
   - Status: Active
   - Created: today
   - Template Commit: `{{TEMPLATE_COMMIT_HASH}}`
   
   If Notion MCP isn't available, surface the row data + the database URL so the user can paste manually.

9. **Initial commit + push.**
   ```bash
   git add -A
   git commit -m "Initial pod setup — {{POD_NAME}} pod, {{PROJECT_NAME}} project"
   git push -u origin main
   ```

10. **Self-archive.**
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
