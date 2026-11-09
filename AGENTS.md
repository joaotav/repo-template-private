# AGENTS.md

Instructions for AI coding agents working in this repository.

## Hard rules

- Work on a branch and open a pull request. Never push to `main` and never force-push.
- When possible, run the tests locally before pushing. Push when the work is ready for the checks, not after every commit. Every push uses CI minutes, which are limited.
- Real secrets live only in files named `.env`. Never read, print, create or edit a `.env` file. Read `.env.example` to see which variables exist, and use placeholder values in code, tests and examples.
- Use made-up personal data in tests, fixtures and examples: `user@example.edu.br`, `02:00:00:00:00:01`, `192.0.2.10`. Never copy real emails, names, MAC addresses or IP addresses from logs or databases.
- When you add a dependency or change anything in `.github/workflows/`, say so in the PR description.
- When you open an issue with `gh` or the API, add its `type/` label and `needs-triage`, since the issue forms are skipped.
- If you find a security problem, tell the person you're working with. Don't describe it in an issue, PR, commit or code comment. See [SECURITY.md](SECURITY.md).
- Leave AI attribution out of commits and PR descriptions: no `Co-authored-by` lines for AI tools and no "Generated with" lines.
- Write commit messages, PR titles, PR descriptions and docs in English, even when the conversation is in another language.

## Before you act, read

- **Commit message or PR title:** [docs/commit-messages.md](docs/commit-messages.md).
- **PR description:** [docs/pull-requests.md](docs/pull-requests.md), then fill in [.github/pull_request_template.md](.github/pull_request_template.md).
- **Branch names, reviews, merging:** [CONTRIBUTING.md](CONTRIBUTING.md).
- **Architectural decision** (major new dependency such as a database or framework, data format, interface between components): [docs/architecture/adr/README.md](docs/architecture/adr/README.md).
- **Tags, releases, deployments:** [docs/releases.md](docs/releases.md). Only create a tag or release when the person asks.
- **Labels, milestones, sub-issues, triage:** [docs/repository-guidelines.md](docs/repository-guidelines.md).

## Project

TODO: the commands to install dependencies, run the project and run the tests.
