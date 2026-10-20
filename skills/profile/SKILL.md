---
name: profile
description: Build or refresh the Chess Refactoring engineering profile of the current project — how changes are verified, reviewed and constrained — and write it to docs/refactors/engineering-profile.md. Use before planning a refactor, when the profile is missing or stale, or when the user says "profile this project", "chess profile" or "how would we verify a refactor here".
argument-hint: "[refresh]"
---

# Chess Refactoring — Profile

Record how this project verifies, reviews and constrains changes, so that every plan and move fits this project instead of a generic ideal. First read sections 2, 5 and 6 of `${CLAUDE_PLUGIN_ROOT}/references/rules.md`.

## Principles

- **Don't duplicate project instructions.** `CLAUDE.md`, `AGENTS.md`, `.claude/rules/`, `CONTRIBUTING` and ADRs stay the source of truth: link them, and record only what refactoring needs that they don't say.
- **Don't invent the profile.** Tag every entry with its source: `documented` (cite the file), `inferred` (cite the evidence) or `to confirm` (a question for the human).
- **Describe, don't judge.** Existing conventions aren't assumed good, but deciding what to change belongs to a plan, not to the profile.
- **Give every rule a strength:** `must` (no exception without a human decision), `should` (an exception needs a stated reason) or `prefer` (style).

## Steps

1. **Location.** Use `docs/refactors/engineering-profile.md`, unless the project's instructions say where docs go. If the file exists, or the argument is `refresh`, read it first and keep every answer the human already confirmed.
2. **Read the sources:** project instructions, CI configuration, build and test manifests, test layout, linters and type checkers, CODEOWNERS, ADRs, and the last 30 commit subjects for the commit convention.
3. **Verification.** Find the commands for tests, static analysis, type checks and build. Run the fast ones once to learn whether the baseline passes; for commands that are slow or need services, record that instead of running them. Map which areas the tests really cover, by module, and rate the verification capability A, B or C (rules, section 6).
4. **Review.** Find the review unit (a commit per move on one branch, or a merge request per move), the platform (GitHub, GitLab, other), the base branch, the branch and commit conventions, and whether you may push or open merge requests. Default to "only when asked".
5. **Constraints and risk zones.** List the hard constraints with their strength and source, and the high-risk areas (rules, section 5) with the extra verification each needs.
6. **Design paradigms in use** — layered, hexagonal, DDD, procedural, … — with evidence.
7. **Write** the profile from [template.md](template.md). Keep it short: a reader should get the picture in two minutes.
8. **Ask** the `to confirm` items as one short numbered list, update the profile with the answers, and stop. Planning is a separate step.

Don't commit the profile: the plan step commits it with the first approved plan, or the user does.
