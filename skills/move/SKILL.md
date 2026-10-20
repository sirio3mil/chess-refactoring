---
name: move
description: Play exactly one move of an approved Chess Refactoring plan — implement it within its scope, verify it, commit it with a move report, update the plan, and stop. Started only by the user.
disable-model-invocation: true
argument-hint: "[slug] [move-number]"
---

# Chess Refactoring — Move

Play one move of an approved plan, then stop. First read `${CLAUDE_PLUGIN_ROOT}/references/rules.md` in full, then the engineering profile, the plan and its decisions.

Arguments: $ARGUMENTS

## 1. Check the position

Find the plan: the slug given, or the only plan under `docs/refactors/` whose status is `approved` or `in-progress`; if there are several, ask. Then stop and report if any of these doesn't hold:

- The plan is `approved` or `in-progress`.
- The working tree has no uncommitted changes. If it has some, ask: never fold them into a move.
- You are on the right branch, as the profile says: the refactor branch or, with a merge request per move, a new branch for this move from the base branch, or from the previous move's branch if that one isn't merged yet (say that it stacks).
- The baseline checks pass, or fail only in the way the profile records.

## 2. Pick the move

Take the move number given, or else the first `pending` move whose dependencies are `done`. If it is still a one-line intention, stop and suggest `/chess-refactoring:reassess` to detail it. Otherwise set it to `in-progress` and state it in two lines: intent, scope, verification.

## 3. Play it

- Change only what the move's intent and scope cover. Do mechanical moves mechanically, with the language's refactoring tools or a verifiable search and replace, and say how.
- Handle anything else you notice as rules section 8 says: a tiny in-scope fix, or a note for the parking lot.
- If you hit a stop condition (rules, section 9), stop at once: don't commit, leave the changes in the working tree for the user to inspect, mark the move `blocked` with the reason, explain what you found, and recommend reassessing.

## 4. Verify

1. Run the profile's checks and the move's own verification, and compare with the baseline.
2. Review your own diff before anyone else does: it stays within the declared scope, it doesn't mix mechanical and semantic changes, it changes no behavior unless the move says so, nothing unrelated slipped in, and it fits the review budget. If it's too big, don't commit: propose how to split it and stop.

## 5. Record

1. In the plan, mark the move `done`, set the plan to `in-progress` if it was `approved`, and add a log line. If the move revealed something that affects later moves, say so in that line.
2. Commit the code and the plan update together: the subject in the project's commit convention, the move report as the body (rules, section 11).
3. With a merge request per move, write its title and description (the move report). Push or open it only if the user asked or the profile allows it.

## 6. Hand over

Tell the user, briefly: what changed, the evidence, the risks the checks don't cover, where the reviewer should look first, and what the next move is. If this move reached a milestone or surfaced a surprise, recommend `/chess-refactoring:reassess` before the next one.

Then stop. Never start the next move on your own.
