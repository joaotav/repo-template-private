# Repository setup

One-time steps for a repo admin, when the repository is created. Do them in this order.

## 1. Labels

The issue forms and Dependabot add labels by name, and GitHub silently skips labels that don't exist. Create ours before anyone opens an issue. What each label means is in [repository-guidelines.md](repository-guidelines.md#labels).

With the GitHub CLI (`gh`), logged in as a repo admin:

```sh
repo=OWNER/REPO

# Create ours. --force updates a label that already exists.
gh label create type/bug          --repo "$repo" --force --color d73a4a --description "Something doesn't work as it should"
gh label create type/feature      --repo "$repo" --force --color 0e8a16 --description "New behavior"
gh label create type/refactor     --repo "$repo" --force --color c5def5 --description "Restructuring code without changing what it does"
gh label create type/docs         --repo "$repo" --force --color 0075ca --description "Documentation"
gh label create type/test         --repo "$repo" --force --color fef2c0 --description "Tests"
gh label create type/maintenance  --repo "$repo" --force --color ededed --description "Upkeep that fits no other type, such as config or cleanup"
gh label create type/dependencies --repo "$repo" --force --color 0366d6 --description "Dependency updates"
gh label create priority/high     --repo "$repo" --force --color d93f0b --description "Needs attention soon, including anything that blocks the next deployment"
gh label create security          --repo "$repo" --force --color b60205 --description "Touches login, permissions, secrets or personal data"
gh label create needs-triage      --repo "$repo" --force --color e4e669 --description "Nobody has looked at this issue yet"

# Delete every other label: GitHub's defaults, whichever they are at the time.
gh label list --repo "$repo" --limit 100 --json name --jq '.[].name' |
  grep -v -x -E 'type/.*|priority/high|security|needs-triage' |
  while read -r label; do gh label delete "$label" --repo "$repo" --yes; done
```

Run it once, before adding any `area/` labels, since the last command would delete those too. If you run it right after creating the repo, wait a minute first: GitHub adds its default labels a few seconds after creation.

Or do the same by hand, on the repo's Issues page, under Labels.

## 2. Pull request settings

In Settings > General > Pull Requests:

- [ ] Leave "Allow squash merging" checked, and uncheck "Allow merge commits" and "Allow rebase merging".
- [ ] Under "Allow squash merging", set the default commit message to "Pull request title and description".
- [ ] Check "Always suggest updating pull request branches".
- [ ] Check "Automatically delete head branches".

## 3. Dependabot security updates

In Settings, in the code security section:

- [ ] Turn on "Dependabot alerts".
- [ ] Turn on "Dependabot security updates".

Without these, `.github/dependabot.yml` still opens the weekly update PRs, but security fixes don't arrive right away.

## 4. Fill in the TODOs

Search the repository for `TODO`. The placeholders are:

- the commands to install, run and test the project, in `CONTRIBUTING.md` and `AGENTS.md`;
- the test steps and time limit in `.github/workflows/ci.yml`;
- the package ecosystems in `.github/dependabot.yml`;
- the language and tool ignores in `.gitignore`;
- the date and deciders in ADR 0001;
- the maintainers and the deployment steps in `docs/releases.md`.

Once the stack is decided, also add:

- `.env.example`, with the first variable and fake values only;
- `.editorconfig` and `.gitattributes`, for indentation per language and LF line endings;
- a coverage badge, if the tests report coverage.

## 5. Status badges

Paste these at the top of the repo's `README.md`, replacing `OWNER` with the account or org and `REPO` with the repo name. They show whether the latest run of the tests and of gitleaks on `main` passed, so a red `main` is visible at a glance.

```markdown
[![CI](https://github.com/OWNER/REPO/actions/workflows/ci.yml/badge.svg?branch=main&event=push)](https://github.com/OWNER/REPO/actions/workflows/ci.yml?query=branch%3Amain)
[![gitleaks](https://github.com/OWNER/REPO/actions/workflows/gitleaks.yml/badge.svg?branch=main&event=push)](https://github.com/OWNER/REPO/actions/workflows/gitleaks.yml?query=branch%3Amain)
```

If the repo is private, the badges only show for people logged in to GitHub with access to it. Outside GitHub, such as in an editor's Markdown preview, they appear as broken images.

## CI minutes

GitHub Actions minutes are limited each month. An org owner can see how many are left in the org's Settings, under Billing. See "CI minutes" in [repository-guidelines.md](repository-guidelines.md#ci-minutes).

## Later, with GitHub Team

The team agreements in [repository-guidelines.md](repository-guidelines.md#team-agreements-for-main) can then be enforced with a branch ruleset for `main`:

- require a PR with one approval, and dismiss approvals when new commits are pushed;
- require the `gitleaks` check, `pr-title` after adding `synchronize` back to its triggers, and `tests` after removing `paths-ignore` from `ci.yml` (a required check that's skipped stays pending and blocks the merge);
- require branches to be up to date before merging;
- block force-pushes and deletion;
- decide how urgent fixes get past the approval rule.

A tag ruleset can also limit who creates `v*` tags to the maintainers.

Team also adds:

- draft PRs: mention in `CONTRIBUTING.md` that a draft is a way to get early feedback;
- `CODEOWNERS` that requests reviewers automatically, worth adding once the code layout is settled.
