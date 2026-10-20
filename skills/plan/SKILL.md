---
name: plan
description: Plan a refactor the way a chess player plans a game — analyze the board, define the target position and its invariants, compare strategies, and write an ordered sequence of small, reviewable, behavior-preserving moves to docs/refactors/<slug>/plan.md, then stop for human approval. Use when the user wants to refactor, restructure, decouple, extract or modernize code beyond one small change, or says "chess plan", "plan this refactor" or "split this refactor into reviewable steps".
argument-hint: "<goal>"
---

# Chess Refactoring — Plan

Turn a refactoring goal into a plan a human can approve: where we are going, what must not change, and the sequence of moves that gets there. First read `${CLAUDE_PLUGIN_ROOT}/references/rules.md` in full.

The goal: $ARGUMENTS

## 1. Prepare

- Read the engineering profile (`docs/refactors/engineering-profile.md`, or the path the project uses). If it's missing or clearly stale, run the `profile` skill first. If the user declines, list your assumptions about verification and review at the top of the plan.
- If the goal is vague, ask what problem justifies the refactor and what would show it is solved. Don't plan around a goal you had to guess.
- Choose a short kebab-case slug. The plan's files go in `docs/refactors/<slug>/`.

## 2. Analyze the board

Read the code; don't guess from names. Find, and cite as `path:line`:

- dependencies that keep the target code from changing in isolation;
- seams: stable entry points where a new structure can go in without touching its consumers;
- the consumers and shared contracts the refactor reaches: APIs, events, database schemas, public classes;
- high-risk code (rules, section 5), and how well each area is verified (profile);
- moves that unlock several others, and moves that are independent;
- improvements this goal doesn't need, which should wait.

Use churn (`git log --format= --name-only -- <path> | sort | uniq -c | sort -rn`) to spot hotspots. Keep verified facts apart from assumptions.

## 3. Define the target position

Write the problem, the target structure, the invariants that must not change, observable success criteria, the "done when" condition, and what is out of scope. The done condition ends the game: without one, the refactor never finishes.

## 4. Choose a strategy

When there is a real choice, compare two strategies (three at most) on number of moves, reversibility, review cost, risk, and how soon the first move delivers value. Recommend one and name the decision-hierarchy level that settles it (rules, section 3). Prefer growing the new structure behind a stable seam over changing many consumers first. If only one strategy is sensible, say why in one line.

## 5. Sequence the moves

Follow rules section 4, and:

- With verification capability B or C, the first moves pin the behavior that the risky moves will touch.
- Keep mechanical and semantic changes in separate moves.
- Make every dependency between moves explicit.
- Put a milestone every three to five moves: each one is a reassess checkpoint.
- Detail only the moves up to the first milestone; later moves stay one-line intentions. As in chess, calculate the next moves concretely and keep a plan for the rest: reassess details them once the board is clearer.
- Include behavior changes only if the goal requires them, labeled `behavioral`.

## 6. Write and stop

1. Write `plan.md` from [plan-template.md](plan-template.md) with `status: draft`, and `decisions.md` from `${CLAUDE_PLUGIN_ROOT}/references/decisions-template.md` with the strategy choice as D1. If the project keeps ADRs, record architecture-level decisions there too, in its format, and link them. Keep the plan proportional to the refactor: the plan is reviewed too, so a reviewer should get through it in about ten minutes, and the board analysis keeps only the findings that shape the moves.
2. Show the user the target position, the chosen strategy and why, the moves table and the open questions.
3. **Stop and wait.** The user approves, changes or rejects the plan; change it and show it again as often as needed.
4. Once approved: set `status: approved` and the approval date, create the refactor branch as the profile says, and commit the profile and the plan's files on their own, so the plan is reviewed before or with the first move. Then tell the user to run `/chess-refactoring:move`. Don't start the first move.
