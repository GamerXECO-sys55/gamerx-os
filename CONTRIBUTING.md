# Contributing to GamerX OS

GamerX OS is an opinionated project. Contributions are welcome but please match
the spec and existing conventions.

## Where to start

- **General questions / proposals**: open an issue on this repo (`gamerx-os`).
- **Bugs in a specific area**: open the issue on the matching sub-repo (e.g. shell bugs → `gamerx-shell`).
- **Documentation**: PRs to `gamerx-docs`.

## Ground rules

1. **Match the spec.** Read `docs/SPEC.md` and `docs/DECISIONS.md`. If your change conflicts, open an issue first proposing a decision change before writing code.
2. **Match the project's style.** Use the libraries and frameworks already in the project. Don't introduce new dependencies without a clear reason.
3. **Branch + PR.** Never push to `main` directly.
4. **Conventional commits.** Format: `type(scope): subject` — e.g. `feat(shell): add mac-style launcher preset`.
5. **No telemetry.** Ever. Not configurable.
6. **No secrets.** `.gitignore` covers the obvious ones; if you commit one by accident, rotate immediately.

## Commit types

- `feat`: new user-visible feature
- `fix`: bugfix
- `docs`: docs only
- `style`: formatting, no logic change
- `refactor`: code restructure, no behavior change
- `perf`: performance improvement
- `build`: PKGBUILD / packaging changes
- `ci`: GitHub Actions changes
- `chore`: anything else

## Local dev

Each sub-repo has its own README with build instructions. The meta repo just
holds docs.

## Code of conduct

Be kind. Disagree on technical merits. Don't be a jerk.
