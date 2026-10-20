# Engineering Profile

Chess Refactoring profile for <project>. Created YYYY-MM-DD, last confirmed YYYY-MM-DD.
The project's own instructions take precedence over this file, which records only what refactoring needs and they don't say.

Source tags: `documented` (cite the file), `inferred` (cite the evidence), `to confirm`.

## Sources

- `CLAUDE.md` — <what it covers>

## Verification

**Capability:** A | B | C — <one-line reason>

| Check | Command | Runtime | Baseline | Source |
|---|---|---|---|---|
| Unit tests | `<command>` | ~40 s | passing | documented: `.gitlab-ci.yml` |

| Area | Covered by | Confidence |
|---|---|---|
| `<module>` | `<tests>` | high / partial / none |

## Review

- **Review unit:** commit per move on one branch | merge request per move
- **Platform:** GitHub | GitLab | other
- **Base branch:** `main`
- **Branch naming:** `refactor/<slug>` (or `refactor/<slug>/<NN>-<move>` per merge request)
- **Commit convention:** <convention> (<source>)
- **Review budget:** <rules default, or the team's own>
- **Push and merge requests:** only when asked | allowed

## Constraints

| Rule | Strength | Source |
|---|---|---|
| <rule> | must / should / prefer | <source> |

## Risk zones

| Area | Why | Extra verification |
|---|---|---|
| <path or module> | <reason> | <what is required> |

## Design paradigms in use

| Paradigm | Where | Evidence |
|---|---|---|
| <paradigm> | <paths> | <evidence> |

## Open questions

- [ ] <question> (to confirm)
