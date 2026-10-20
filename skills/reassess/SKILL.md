---
name: reassess
description: Reassess an in-progress Chess Refactoring plan against the code as it is now — the moves that landed, whether the goal is already met, whether the remaining moves are still the best path — and propose changes as a reviewable diff with a decision record, applied only after the user approves. Use at plan milestones, after a move stopped on a surprise, when the goal or constraints change, or when the user says "reassess", "review the plan" or "are we still on track".
argument-hint: "[slug]"
---

# Chess Refactoring — Reassess

Look at the whole board again before the next move. First read sections 3, 4, 9 and 10 of `${CLAUDE_PLUGIN_ROOT}/references/rules.md`, then the engineering profile, the plan and its decisions.

Arguments: $ARGUMENTS

## 1. What happened

- List the moves that landed: `git log --grep "Refactor: <slug>" --format="%h %ad %s" --date=short`. Check whether any was reverted, and whether other changes have touched the same code since.
- Read the plan's log and any `blocked` move: what did it find?
- Gather review signals: ask the user, or read the merge requests if you can. Moves that were hard to review, comments that kept repeating and regressions all say whether moves should be smaller, split differently or verified more.

## 2. Look at the board again

Re-analyze the code the remaining moves will touch, as the plan skill's board analysis does. Then answer:

1. Are the success criteria already met? Then propose closing the plan (`done`) and sending the leftovers to the parking lot.
2. Is the target position still right? Changing the goal is the human's decision: ask, don't change it.
3. Is the remaining sequence still the best? Add, split, reorder or drop moves as needed, and detail every move up to the next milestone.
4. Did the review signals show moves too big or too risky? Adjust their size and verification.

## 3. Propose, then stop

- Show the changes as the moves table before and after, plus a new entry in `decisions.md` with status `proposed`: context, options, decision, and the decision-hierarchy level that settled it.
- Moves already `done` are history: never edit them. A dropped move stays in the table as `dropped`, with the reason, so the plan keeps its trail.
- **Stop and wait for approval.**
- Once approved: apply the changes to `plan.md`, mark the decision `accepted`, add a log line, and commit the plan's files on their own. Then tell the user to run `/chess-refactoring:move`.
