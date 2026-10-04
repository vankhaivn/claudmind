# claudmind Setup

This file is a protocol for an AI coding agent (Claude Code or Codex) running on the owner's machine. Follow it from top to bottom to install, upgrade, review, or remove claudmind for the current operating-system user.

> **Read freely, write only what is approved.** Present a plan that names every path and setting you will create or change, wait for explicit approval, then apply exactly that plan. Back up every existing file before changing it. Never print secret values.

## What gets installed

| Path or setting                       | Role                                               | On upgrade    |
| ------------------------------------- | -------------------------------------------------- | ------------- |
| `~/.claudmind/engine.md`              | Rules for recording and using the profile          | Replaced      |
| `~/.claudmind/OWNER.md`               | The owner's profile                                | Never touched |
| `~/.claudmind/MACHINE.md`             | Facts about this machine                           | Never touched |
| Marked block in `~/.claude/CLAUDE.md` | Loads the three files in every Claude Code session | Verified      |
| Marked block in `~/.codex/AGENTS.md`  | Tells Codex to read the three files before work    | Verified      |
| Write access to `~/.claudmind/`       | Lets agents update the profile from any project    | Verified      |

claudmind adds no command, skill, hook, service, or background process.

## Step 1 — Inspect (read-only)

1. Detect the runtimes: Claude Code when `~/.claude/` exists, Codex when `~/.codex/` exists. Cover every detected runtime unless the owner selects one.
2. Read `~/.claudmind/` if it exists, the global instruction files `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`, the `permissions` object in `~/.claude/settings.json`, and the `[sandbox_workspace_write]` table in `~/.codex/config.toml`. Report only what matters for this setup. Never print credentials, tokens, or other secret values.
3. Use this repository as the source of truth for `engine.md`, the templates, and the marked blocks below: a local checkout the owner names, or a fresh clone in a temporary directory.

## Step 2 — Choose the mode

- **Install:** `~/.claudmind/` does not exist.
- **Upgrade:** `~/.claudmind/` exists. Compare the version in the first line of the installed `engine.md` with the repository's copy.
- **Review:** the owner asks to review or rebuild the profile. Follow "Review On Request" in `engine.md` and stop.
- **Uninstall:** the owner asks to remove claudmind. Go to [Uninstall](#uninstall).

## Step 3 — Present the plan and wait

List every write:

- files created under `~/.claudmind/`: all three on install, `engine.md` only on upgrade;
- each global instruction file that gains the marked block, with its backup path `<file>.claudmind-backup-YYYYMMDD`;
- each permission change, with the exact key and value;
- whether to bootstrap the profile from existing memory ([Step 5](#step-5--bootstrap-the-profile-optional)), which the owner may decline.

Wait for explicit approval. Approving this plan does not approve later, unplanned writes; present an amended plan instead.

## Step 4 — Apply

1. Create `~/.claudmind/` and copy `engine.md` into it. Copy `templates/OWNER.md` and `templates/MACHINE.md` into it only when they are missing.
2. **Claude Code.** Add this block once to the end of `~/.claude/CLAUDE.md`, creating the file if needed and keeping all existing content:

   ```markdown
   <!-- claudmind:begin -->

   @~/.claudmind/engine.md
   @~/.claudmind/OWNER.md
   @~/.claudmind/MACHINE.md

   <!-- claudmind:end -->
   ```

3. **Codex.** Add this block once to the end of `~/.codex/AGENTS.md`, creating the file if needed and keeping all existing content:

   ```markdown
   <!-- claudmind:begin -->

   ## claudmind

   Before meaningful work, read `~/.claudmind/engine.md`, `~/.claudmind/OWNER.md`, and `~/.claudmind/MACHINE.md`, and follow them for the rest of the session.

   <!-- claudmind:end -->
   ```

4. **Write access.** Use the absolute path of `~/.claudmind`, merge with existing values, and never remove or reorder unrelated entries:
   - Claude Code: in `~/.claude/settings.json`, add the path to `permissions.additionalDirectories` and add `Edit(~/.claudmind/**)` to `permissions.allow`.
   - Codex: in `~/.codex/config.toml`, add the path to `writable_roots` under `[sandbox_workspace_write]`.

   If a runtime still asks before the first write to the profile, the owner can approve it in that prompt.

## Step 5 — Bootstrap the profile (optional)

Run this step only when the approved plan includes it.

1. Read runtime-native memory:
   - Claude Code: `~/.claude/projects/*/memory/*.md`, focusing on entries typed `user` or `feedback`.
   - Codex: `~/.codex/memories/memory_summary.md`, focusing on the user profile and preferences.
   - Personal rules that both global instruction files repeat.
2. Keep only lessons that hold across projects. Merge a lesson found in several projects into one entry. Drop anything project-specific, stale, contradicted, or containing secrets, account identifiers, or personal data about other people.
3. Show the drafted `OWNER.md` and `MACHINE.md` entries, each with its date and the store it came from. Write only the entries the owner approves.
4. Leave runtime-native memory unchanged. Tell the owner that it can stay on: the engine claims only lessons that hold across projects, and turning native memory off would also lose its automatic notes and future improvements.

## Step 6 — Verify

1. `~/.claudmind/` holds `engine.md`, `OWNER.md`, and `MACHINE.md`. On upgrade, `OWNER.md` and `MACHINE.md` are byte-identical to their state before the upgrade.
2. Each changed global instruction file contains exactly one marked block and has a backup.
3. Every changed settings file still parses as valid JSON or TOML, and the permission entries are present.
4. Ask the owner to open a new session in any project and ask: "What does claudmind say about me?" The answer should come from `OWNER.md`.

## Step 7 — Report

List the paths created, changed, and backed up; the permission changes; the bootstrap entries written; and anything skipped, with the reason.

## Uninstall

1. Present the plan: remove both marked blocks and the permission entries claudmind added. Keep `~/.claudmind/` unless the owner asks to delete it.
2. After approval, back up each file, apply the removal, verify that no marked block remains, and report.
