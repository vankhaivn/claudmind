# claudmind Setup

This file is a protocol for an AI coding agent (Claude Code or Codex) running on the owner's machine. Follow it from top to bottom to install, upgrade, review, or remove claudmind for the current operating-system user.

> **Read freely, write only what is approved.** Present a plan that names every path you will create or change, wait for explicit approval, then apply exactly that plan. Back up every existing file before changing it. Never print secret values.

## What gets installed

| Path                                  | Role                                               | When it already exists   |
| ------------------------------------- | -------------------------------------------------- | ------------------------ |
| `~/.claudmind/engine.md`              | Rules for recording and using the profile          | Replaced when it differs |
| `~/.claudmind/OWNER.md`               | The owner's profile                                | Never overwritten        |
| `~/.claudmind/MACHINE.md`             | Facts about this machine                           | Never overwritten        |
| Marked block in `~/.claude/CLAUDE.md` | Loads the three files in every Claude Code session | Replaced when it differs |
| Marked block in `~/.codex/AGENTS.md`  | Tells Codex to read the three files before work    | Replaced when it differs |

claudmind adds no command, skill, hook, service, or background process, and it changes no permission or setting. The first time an agent writes to the profile, the runtime may ask the owner to approve the write.

## Step 1 — Inspect (read-only)

1. Detect the runtimes: Claude Code when `~/.claude/` exists, Codex when `~/.codex/` exists. Cover every detected runtime unless the owner selects one.
2. Read `~/.claudmind/` if it exists and the global instruction files `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`. Report only what matters for this setup.
3. Use this repository as the source of truth for `engine.md`, the templates, and the marked blocks below: a local checkout the owner names, or a fresh clone in a temporary directory.

## Step 2 — Choose the mode

- **Review:** the owner asks to review or rebuild the profile. Follow "Review On Request" in `engine.md` and stop.
- **Uninstall:** the owner asks to remove claudmind. Go to [Uninstall](#uninstall).
- **Install or upgrade:** any other request. Both follow the steps below, and the current state of each file decides what changes.

## Step 3 — Present the plan and wait

List every write:

- `~/.claudmind/engine.md`: created, or replaced when it differs from this repository's copy;
- `~/.claudmind/OWNER.md` and `~/.claudmind/MACHINE.md`: created from the templates only when missing;
- each global instruction file that gains the marked block or has one that differs, with its backup path `<file>.claudmind-backup-YYYYMMDD`;
- whether to bootstrap the profile from existing memory ([Step 5](#step-5--bootstrap-the-profile-optional)), which the owner may decline.

Wait for explicit approval. Approving this plan does not approve later, unplanned writes; present an amended plan instead.

## Step 4 — Apply

1. Create `~/.claudmind/` if needed. Copy `engine.md` into it when it is missing or differs. Copy `templates/OWNER.md` and `templates/MACHINE.md` into it only when they are missing; never overwrite an existing profile.
2. **Claude Code.** Add this block to the end of `~/.claude/CLAUDE.md`, creating the file if needed and keeping all existing content. If the block is already there, replace it only when it differs:

   ```markdown
   <!-- claudmind:begin -->

   @~/.claudmind/engine.md
   @~/.claudmind/OWNER.md
   @~/.claudmind/MACHINE.md

   <!-- claudmind:end -->
   ```

3. **Codex.** Add this block to `~/.codex/AGENTS.md` the same way:

   ```markdown
   <!-- claudmind:begin -->

   ## claudmind

   Before meaningful work, read `~/.claudmind/engine.md`, `~/.claudmind/OWNER.md`, and `~/.claudmind/MACHINE.md`, and follow them for the rest of the session.

   <!-- claudmind:end -->
   ```

## Step 5 — Bootstrap the profile (optional)

Run this step only when the approved plan includes it: follow "Review On Request" in `engine.md`.

## Step 6 — Verify

1. `~/.claudmind/` holds `engine.md`, `OWNER.md`, and `MACHINE.md`. A profile that existed before is unchanged, except for entries the owner approved in Step 5.
2. Each changed global instruction file contains exactly one marked block, and a file that existed before has a backup.
3. For each selected runtime, ask the owner to open a new session in any project and ask: "Which claudmind files did you load, and what are their section headings?" The answer should name all three files with their real headings, even while the profile is still empty.

## Step 7 — Report

List the paths created, changed, and backed up; the bootstrap entries written; and anything skipped, with the reason.

## Uninstall

1. Present the plan: remove both marked blocks. Keep `~/.claudmind/` unless the owner asks to delete it.
2. After approval, back up each file, apply the removal, verify that no marked block remains, and report.
