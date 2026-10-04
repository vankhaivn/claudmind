# Contributing to claudmind

claudmind is deliberately small: an AI-native setup protocol, one engine file, and two profile templates. Keep it that way. Prefer a shorter rule to new machinery, and add no command, skill, hook, service, background process, or permission change.

## Issues

Search open issues and pull requests first. Open an issue before branching for a bug, a behavior change, or anything that needs a design decision; a typo or a tiny wording fix can skip it. A useful issue states the **Problem**, **Desired outcome**, **Acceptance criteria**, and **Non-goals**.

## Branches

Branch from the current `main`, one logical change per branch:

```text
<type>/<issue-number>-<kebab-case-slug>
```

Use `<type>/<kebab-case-slug>` when there is no issue. Types follow Conventional Commits, such as `feat`, `fix`, `docs`, `refactor`, and `chore`. Keep names lowercase and avoid personal, agent, model, or tool names.

## Commits

```text
<type>(<optional-scope>): <imperative summary>
```

Keep the subject at most 72 characters and add a body when the reason or trade-off is not obvious. Make one commit per problem, and leave no `WIP` or `fix stuff` messages in the final history. Do not add `Co-authored-by` trailers for AI models, agents, or tools.

## Pull Requests

Open the pull request against `main` with a Conventional Commit title. The body has:

- **Summary**: what changed;
- **Rationale**: why, when it is not obvious;
- **Validation**: what you checked and the result, including anything not run;
- **Risks / notes**: limits or follow-ups, when relevant;
- `Closes #<issue>` when the pull request completes an issue.

Opening a pull request does not authorize merging it; an agent merges only when the maintainer asks. Squash merge is the default; use a merge commit only when each commit is worth keeping on `main`. After merge, delete the branch and start the next change from an updated `main`.

## Validation

There is no build or test suite. Before opening a pull request, run:

```bash
git diff --check
npx prettier@3 --check "**/*.md"
```

When a change touches `SETUP.md` or `engine.md`, also run the changed flow on your own machine and open a new session in each runtime it affects. Report the result under **Validation**.

## Writing Rules

- Write in English, and keep `README.md` and `README_VI.md` saying the same thing.
- Describe the current contract directly. Keep history in issues, pull requests, and commits.
- Change the version on the first line of `engine.md` only for a release.
- claudmind may name CLAUDART's `.claudart/OWNER.md`, but it must keep working in projects without CLAUDART.
