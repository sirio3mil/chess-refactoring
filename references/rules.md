# Chess Refactoring — Rules of the Game

The shared rules every Chess Refactoring skill follows. Each skill says which sections it needs.

> The AI may think several moves ahead, but every move that lands in the codebase must stay understandable, verifiable and defensible by the team.

## 1. Vocabulary

| Term | Meaning |
|---|---|
| Board | The system as it is now: code, dependencies, tests, conventions. |
| Target position | The structure the refactor aims for, with the invariants that must survive it. |
| Move | One reviewable change with a single intent that leaves the system working. |
| Review unit | What a human reviews per move: a commit on the refactor branch, or a merge request. Set in the profile. |
| Milestone | A planned checkpoint where the plan is reassessed before moving on. |
| Engineering profile | How this project is verified, reviewed and constrained. |

## 2. Project rules first

The project's own instructions (`CLAUDE.md`, `AGENTS.md`, `.claude/rules/`, `CONTRIBUTING`, ADRs) outrank this file, and the engineering profile records how they apply to refactoring. Where they disagree with these rules, follow the project and say so.

## 3. Decision hierarchy

When options conflict, settle them in this order. A lower level never overrides a higher one.

1. **Non-negotiable constraints** — security, data integrity, regulation, public contracts, required compatibility.
2. **System behavior** — the behavior that must be preserved, and the requirement that motivates the refactor.
3. **The project's architecture and standards** — adopted design decisions, conventions, verification mechanisms.
4. **General engineering principles** — DRY, KISS, YAGNI, SOLID and the others that apply (section 7).
5. **Move-sequence economy** — less risk, less review cost, fewer moves, earlier value.

This orders authority, not importance: principles still matter, they just can't contradict the system's real constraints and goals. When two options are close, present both, recommend one and name the level that decided it. When the decision depends on information you don't have, say so and ask instead of inventing certainty.

## 4. What makes a good move

- **One intent.** It can be stated in one sentence without "and".
- **Mechanical apart from semantic.** Moving, renaming or extracting code without changing logic is one move; changing logic is another. A reviewer can verify a pure move quickly only if nothing else hides in it. ("Make the change easy, then make the easy change." — Kent Beck)
- **Behavior-preserving**, unless the plan labels the move `behavioral` because the goal requires it.
- **Leaves the system working.** It builds, the checks pass, it could ship. No move relies on a later move to be correct.
- **Independently revertible**, without undoing later moves by hand.
- **Within the review budget.** The reviewer can answer in one sitting: what changed, why, is it correct, did behavior change, and can I accept it without trusting later moves? As a default, keep a semantic diff under about 300 changed lines; a mechanical diff can be larger when it states how it was produced and how to check it. The profile may set the team's own budget.
- **Smaller as risk grows** (section 5).

Small is not the same as harmless: twenty "simple" files can change behavior together, and three lines can break billing. Judge both review effort and risk.

## 5. Risk classes

| Class | Typical areas | Extra requirements |
|---|---|---|
| low | internal code with good coverage, mechanical moves | the profile's standard checks |
| medium | shared modules, partial coverage, moves across modules | targeted tests for the behavior touched |
| high | money, authentication and authorization, data integrity and migrations, public APIs and contracts, concurrency, regulated logic | characterization or contract tests landed in an earlier move, the smallest possible move, a flag for specialist review |

## 6. Verification capability

The profile rates how well the project can show that behavior is preserved:

- **A — good automated tests.** Use them as the safety net; add tests for any new risk a move introduces.
- **B — partial tests.** Before a risky move in an uncovered area, plan a characterization move that pins the current behavior.
- **C — little or no tests.** Don't impose TDD on the whole project. Choose what is viable per move: characterization tests for a critical function, an integration test for a key operation, golden-master comparison of outputs before and after, contract tests at boundaries.

Green checks are evidence, not proof: say what they don't cover. If there is no reliable way to show that a high-risk move preserves behavior, stop and agree a verification strategy with the human before playing it. Adding tests before a move is not TDD and needn't be called so.

## 7. Principles in context

Apply the principles that fit the problem, not all of them everywhere. If the project enables the `coding-base`, `coding-arch` or `coding-solid` plugins from the coding-skills marketplace, use those skills for the detail.

| Principle | Use it to | Trap to avoid |
|---|---|---|
| DRY | merge duplicated *knowledge* | unifying code that only looks alike but encodes rules that will evolve apart |
| KISS | reduce the complexity a developer meets | splitting a 300-line service into fifteen classes "for cleanliness" |
| YAGNI | reject abstractions with no present need | interfaces, factories and plug-in points for hypothetical futures |
| SOLID | find responsibilities that change for different reasons, and dependencies on details | treating every class with many methods as an SRP violation |
| Separation of Concerns, Law of Demeter | find boundaries and coupling | layers the project doesn't need |
| Boy Scout Rule | improve what a move touches | letting a move grow into a renovation (section 8) |
| DDD, hexagonal, CQRS and other paradigms | respect the model the project already uses | imposing one it doesn't use; adopt one only if the domain's complexity justifies it and the human agrees |

The AI can infer a domain model from code but cannot confirm it matches the business. When two concepts look equivalent in code but may differ for the business, the right move is an open question for a domain expert, not a code change.

## 8. Opportunities found on the way

Anything worth improving that the current move doesn't need goes to one of three places, never silently into the diff:

1. **The current move**, only if it is tiny, sits inside the lines the move already touches, and doesn't raise risk.
2. **A new move**, proposed through reassess, if it serves the goal.
3. **The parking lot** in the plan, if it doesn't.

## 9. Stop conditions

Stop, report and wait for the human when:

- a check fails and can't be fixed within the move's scope;
- the move needs non-trivial changes outside its declared scope, or exceeds the review budget;
- you find something that invalidates the plan: an unknown responsibility, consumer or contract;
- the move would touch a `must` constraint or a high-risk area the plan didn't anticipate;
- the next step needs a business or domain decision;
- the baseline already fails in a way that would hide the move's effect.

Don't improvise a new strategy mid-move. Explain what you found, what it changes and why, and recommend reassessing.

## 10. Human control

- Never approve your own plan or plan change.
- Never start the next move unprompted: one move per `/chess-refactoring:move`.
- Never push, merge or open merge requests unless the user asks or the profile allows it.
- Never rewrite moves that were already reviewed; fix forward with a new move.
- The goal ends the game: when the success criteria are met, propose closing the plan. Leftover ideas go to the parking lot.

## 11. Move report

Every move's commit body, or merge request description, carries a report proportional to its risk: a low-risk move can be three lines, a high-risk one covers every field.

```text
<subject in the project's commit convention>

Why: <the problem this move solves, and how it serves the target position>
What: <what changed, in reviewer terms>
Preserves: <behavior that must not change, and how it was checked>
Evidence: <checks run and their results>
Alternatives: <options considered and why this one; medium and high risk>
Risks: <what the evidence does not cover; where to look first>

Refactor: <slug>
Move: <NN>
```

The `Refactor` and `Move` trailers make every move traceable with `git log --grep "Refactor: <slug>"`.
