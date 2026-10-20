---
refactor: <slug>
status: draft # draft | approved | in-progress | paused | done | abandoned
goal: <one line>
created: YYYY-MM-DD
approved: # YYYY-MM-DD, by <name>
---

# <Title>

## Target position

- **Problem:** <what hurts today, with evidence>
- **Target:** <the structure we want>
- **Invariants:** <behavior, contracts and data that must not change>
- **Success criteria:** <observable checks that show the target is reached>
- **Done when:** <the condition that ends the refactor>
- **Out of scope:** <what this refactor will not touch>

## Board analysis

<Key findings, each cited as `path:line`. Mark assumptions as such.>

## Strategy

| | A: <name> | B: <name> |
|---|---|---|
| Moves | | |
| Reversibility | | |
| Review cost | | |
| Risk | | |
| First value | | |

**Chosen:** <A or B>, because <reason> (decision D1).

## Moves

Kinds: `verification`, `mechanical`, `structural`, `behavioral`.
Statuses: `pending`, `in-progress`, `done`, `blocked`, `skipped`, `dropped`.

| # | Move | Kind | Risk | Depends on | Status |
|---|---|---|---|---|---|
| 01 | <title> | verification | low | — | pending |
| 02 | <title> | mechanical | low | 01 | pending |
| — | Milestone 1: reassess | | | | |
| 05 | <one-line intention, detailed at reassess> | | | | pending |

### 01 — <title>

- **Intent:** <one sentence, no "and">
- **Scope:** <files or modules it may touch>
- **Preserves:** <behavior that must not change>
- **Verification:** <commands to run and any extra evidence>
- **Acceptance:** <what the reviewer can confirm>

## Open questions

- [ ] <question> — <who can answer it>

## Parking lot

<Improvements spotted that the goal doesn't need.>

## Log

- YYYY-MM-DD — Plan drafted.
