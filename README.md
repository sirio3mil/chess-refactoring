# Chess Refactoring

AI-assisted refactoring played like chess: there is a final position to reach, but you get there one move at a time, and every move is small enough for a person to review, understand and defend.

> The AI may think several moves ahead, but every move that lands in the codebase must stay understandable, verifiable and defensible by the team.

This repository is both the description of the method and a [Claude Code](https://code.claude.com) plugin that applies it.

## Why

A coding agent can produce in minutes a refactor that takes a person days to review. Asking for the whole refactor and reviewing the result at the end moves the bottleneck to review, and the team ends up accepting changes it no longer fully understands, even when the tests pass.

Chess Refactoring keeps what the AI is good at, seeing the whole board and planning a sequence, and puts the human checkpoint between moves instead of at the end.

## What it is, and what it isn't

It is not a new set of engineering principles. It builds on well-known practice:

| Practice | What Chess Refactoring takes from it |
|---|---|
| Refactoring (Martin Fowler) | small, behavior-preserving transformations |
| Continuous integration and trunk-based development | small changes, integrated often |
| The refactor step of TDD | verification after every step |
| The Mikado Method | finding prerequisites and ordering changes by dependency |
| Small change lists in code review | review units a person can review in one sitting |

What it adds is a protocol for coding agents: how the target is defined, how the moves are sequenced, what makes a move small enough, what evidence each move must carry, when the agent must stop, and how every move stays traceable to the goal. It coordinates DRY, KISS, YAGNI, SOLID, DDD and the rest instead of competing with them, and it adapts to each project's own standards instead of imposing an ideal one.

## The game

```text
profile ─► plan ─► approval ─► move ─► move ─► … ─► reassess ─► approval ─► move ─► … ─► done
```

| Step | Skill | What happens | Who starts it |
|---|---|---|---|
| Profile | `/chess-refactoring:profile` | Records how the project is verified, reviewed and constrained: check commands and their baseline, verification capability (A good tests, B partial, C none), review unit (commit or merge request), hard constraints, risk zones and the paradigms in use. Keeps documented rules apart from inferred practice and open questions. | Claude or you |
| Plan | `/chess-refactoring:plan <goal>` | Analyzes the board, defines the target position, its invariants and the "done when" condition, compares strategies and writes the sequence of moves. Stops for your approval. | Claude or you |
| Move | `/chess-refactoring:move` | Plays exactly one approved move: implements it within its scope, verifies it, commits it with a move report, updates the plan and stops. | **Only you** |
| Reassess | `/chess-refactoring:reassess` | At milestones or after a surprise: looks at the board again, checks whether the goal is already met, and proposes plan changes with a decision record. Applies nothing until you approve. | Claude or you |

The `move` skill sets `disable-model-invocation`, so Claude can't start a move through the plugin on its own: every move begins with your command.

The rules every step follows — the decision hierarchy, what makes a good move, risk classes, verification capability, stop conditions and the move report — are in [references/rules.md](references/rules.md).

## What it leaves in your project

```text
docs/refactors/
├── engineering-profile.md   # shared by every refactor in the project
└── <slug>/
    ├── plan.md              # target position, strategy, moves and their status, parking lot, log
    └── decisions.md         # why the plan is what it is, and how it changed
```

Each move is one commit, or one merge request, whose body is the move report and ends with the `Refactor: <slug>` and `Move: <NN>` trailers:

```bash
git log --grep "Refactor: invoice-service-split" --oneline
```

The project's own `CLAUDE.md`, `AGENTS.md` and `.claude/rules/` always take precedence: the profile links them instead of repeating them.

## Install

The plugin is listed in the [coding-skills](https://github.com/sirio3mil/coding-skills) marketplace. Enable it per project in the project's versioned `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "coding-skills": {
      "source": { "source": "github", "repo": "sirio3mil/coding-skills", "ref": "v1.1.0" },
      "autoUpdate": false
    }
  },
  "enabledPlugins": {
    "chess-refactoring@coding-skills": true
  }
}
```

Project settings enable the plugin but don't install it. On each machine, once per project, run this from the project folder, then restart the session or run `/reload-plugins`:

```bash
claude plugin install chess-refactoring@coding-skills --scope project
```

To install it for yourself in every project instead:

```bash
/plugin install chess-refactoring --marketplace sirio3mil/coding-skills
```

It works on its own, and pairs well with the `coding-base`, `coding-arch` and `coding-solid` plugins from the same marketplace, which the rules defer to for the detail of each principle.

To try a local copy without installing it: `claude --plugin-dir /path/to/chess-refactoring`.

## Status

Version 0.1. The method and the skills are written but not yet proven on a real refactor, so expect the rules on move size, milestones and the report to change once they are.

## Releasing

1. Bump `version` in `.claude-plugin/plugin.json`. It is the cache key: without a bump, machines keep the old copy.
2. Commit, tag `vX.Y.Z` and push with tags.
3. In coding-skills, update this plugin's `ref` in `.claude-plugin/marketplace.json` and release the marketplace too.

## License

[MIT](LICENSE)
