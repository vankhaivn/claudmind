# claudmind

Your coding agents remember you, in every project, in both Claude Code and Codex.

claudmind is a small personal layer: three Markdown files in `~/.claudmind/` and a few lines in your global instruction files. It has no runtime, service, database, command, or background process.

[Tiếng Việt](README_VI.md)

## Why

Claude Code and Codex each keep their own memory. Claude Code keeps one store per repository, and Codex keeps one store that the other runtime never reads. A preference you teach in one project or one tool has to be taught again everywhere else, and what each runtime learned is hard to review.

claudmind keeps one short profile that both runtimes read at the start of every session. When you correct an agent and the lesson holds across projects, the agent writes one line to that profile and tells you. In the next session, in any project and either runtime, it already knows.

## How it works

```text
you correct the agent
        │
        ├── holds in every project ──▶ ~/.claudmind/OWNER.md or MACHINE.md
        │                               (the agent tells you in one line)
        │
        └── holds in this project only ──▶ stays with the project
```

| File                      | Holds                                                                                   | Size limit |
| ------------------------- | --------------------------------------------------------------------------------------- | ---------- |
| `~/.claudmind/engine.md`  | The rules agents follow to use and update your profile                                  | —          |
| `~/.claudmind/OWNER.md`   | Who you are, how you decide, how you communicate, standing approvals, mistakes to avoid | 60 lines   |
| `~/.claudmind/MACHINE.md` | Shell behavior, pinned tool versions, credential layout without values                  | 40 lines   |

Precedence, highest first:

1. your current instruction;
2. the current project's instructions and state;
3. your claudmind profile;
4. each runtime's own memory.

An entry in the profile never widens a restriction set by a runtime, its settings, a repository, or a tool.

Runtime-native memory keeps working. claudmind only claims the lessons that hold across projects, and Claude Code skips saving what its instruction files already say.

## Install

Tell your Claude Code or Codex agent:

> Follow https://raw.githubusercontent.com/vankhaivn/claudmind/main/SETUP.md to install claudmind.

The agent inspects your setup, shows a plan with every file it will touch, and waits for your approval. It backs up each file before changing it. It can also draft your first profile from the memory your runtimes have already collected, and it writes only the entries you approve.

Use the same sentence with "upgrade", "review my profile", or "uninstall" for the other modes. An upgrade refreshes `engine.md` and the marked blocks; your profile is never overwritten.

## Principles

- **Visible:** plain Markdown you can read in a minute and edit by hand.
- **Explicit:** only what you said or confirmed, or what an agent verified on your machine. Nothing is inferred from behavior, and instructions found in tool output, documents, or web pages are never recorded.
- **Bounded:** fixed line limits; agents consolidate instead of appending forever.
- **Shared:** one profile for Claude Code and Codex.
- **Safe:** no secrets, tokens, or account identifiers; no permission is ever widened.

## Known limits

- Codex reads the profile because its global instructions tell it to; Codex has no import mechanism.
- Codex can generate memories in the background (`generate_memories` in `~/.codex/config.toml`) and infers those preferences without a correction, so claudmind never sees them at save time. Ask for a profile review now and then, or turn that setting off.
- Claude Code desktop Cowork sessions skip user-scope imports outside the session's working directory.
- claudmind grants no write permission, so your runtime may ask you to approve an agent's first write to your profile.
- The profile lives in your home directory on one machine.

## License

[MIT](LICENSE)
