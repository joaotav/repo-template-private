# Repo template (private)

Contribution docs, issue and PR templates, CI checks and agent settings for a private repository on GitHub Free whose work is mostly done with AI coding agents. On a public repo, GitHub Free can enforce the rules with a ruleset and Actions minutes are free, so some of the workarounds here aren't needed. Some examples and rules come from the project it was first built for, such as institutions and test rounds. Adapt them when you copy, and replace `OWNER/REPO` in the setup commands.

## Using it

1. Copy everything into the new repository except this `README.md`.
2. Follow [docs/repo-setup.md](docs/repo-setup.md): labels first, then the repo settings, then the TODOs.

## What's included

For people:

- `CONTRIBUTING.md`: how to get a change into `main`, step by step. Start here.
- `docs/repository-guidelines.md`: the team agreements and the reasons behind them, plus labels, test rounds, triage, dependencies and CI minutes.
- `docs/commit-messages.md`: the PR title and commit message format.
- `docs/pull-requests.md`: how to fill in the PR template, with a full example.
- `docs/releases.md`: versions, release candidates, how to make a release and record a deployment.
- `docs/architecture/adr/`: architecture decision records, with their template.
- `docs/repo-setup.md`: one-time setup for a repo admin.
- `SECURITY.md`: how to report a security problem.

For AI agents:

- `AGENTS.md`: the rules agents follow, and which doc to read before each kind of task.
- `CLAUDE.md`: makes Claude Code load `AGENTS.md`.
- `.github/copilot-instructions.md`: points Copilot to `AGENTS.md`.
- `.claude/settings.json` and `.cursorignore`: keep Claude Code and Cursor out of `.env` files.

For GitHub:

- `.github/ISSUE_TEMPLATE/`: issue forms for bugs, features and other work.
- `.github/pull_request_template.md`: the PR description template.
- `.github/workflows/pr-title.yml`: checks the PR title format.
- `.github/workflows/gitleaks.yml`: looks for passwords, tokens and keys in new commits.
- `.github/workflows/ci.yml`: runs the tests (a placeholder until the project has tests).
- `.github/dependabot.yml`: weekly dependency update PRs.
- `.github/release.yml`: groups the generated release notes.
- `.gitignore`: keeps `.env` files out of git.
