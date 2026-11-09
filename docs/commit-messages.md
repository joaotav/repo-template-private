# Commit messages and PR titles

The format is [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/). For the wording, we follow the [seven rules of a great commit message](https://cbea.ms/git-commit/). Write them in English.

We squash-merge, so each pull request (PR) becomes one commit on `main`:

- the PR title becomes the first line of the commit (the subject);
- the PR description becomes the rest of the commit (the body).

The PR title is the message that matters. A check on every PR fails if the title has no type or an unknown one. The commits on your branch disappear in the squash, so they can be informal.

## Format

```text
<type>(<scope>): <description>

<body>

<footers>
```

- `type` is required. See [Types](#types).
- `(scope)` is optional. See [Scope](#scope).
- A `!` before the colon marks a breaking change: `feat(api)!: require the API version header`.
- The body and footers are optional. In a PR, they come from the description.

A squashed PR on `main` looks like this:

```text
feat(devices): allow bulk registration from a CSV file (#42)

Closes #41

## Description

Admins registered one device at a time, which took hours in large
labs. This PR lets them upload a CSV with up to 500 MAC addresses.

## Testing

The agent added tests for invalid MACs and files over 500 lines.
I uploaded a CSV with 3 valid MACs and 1 invalid one by hand.

## Checklist

- [x] No passwords, tokens, keys or `.env` files in the diff
- [x] Tests added or updated, if applicable
- [x] Docs updated, if applicable
```

## Types

Pick the type that describes the main reason for the change:

- `feat`: something new that users or other code can do. `feat(devices): allow bulk registration`
- `fix`: a bug fix. `fix(login): keep the session after a password change`
- `docs`: documentation only. `docs: explain how to run the tests`
- `style`: formatting or whitespace, with no change in behavior. `style: fix indentation in device forms`
- `refactor`: a code change that neither fixes a bug nor adds a feature. `refactor(devices): move MAC validation into one function`
- `perf`: faster or lighter, same behavior. `perf(reports): load the device list in one query`
- `test`: adding or fixing tests. `test(devices): cover invalid MAC addresses`
- `build`: build system, packaging or dependencies. `build: add a CSV parsing library`
- `ci`: GitHub Actions workflows. `ci: run gitleaks on pull requests`
- `chore`: other maintenance that doesn't change the code or the tests. `chore: remove an unused config file`. Dependabot titles its update PRs `chore(deps): ...`.
- `revert`: undoes an earlier commit. `revert: feat(devices): allow bulk registration`. GitHub's "Revert" button on a merged PR titles the new PR `Revert "feat(devices): ..."`, which fails the title check. Rename it to the `revert:` form.

If a change fits two types, pick the one for the main reason.

## Scope

Optional. One word naming the part of the system the change touches: `fix(login): ...`, `feat(reports): ...`. Once `area/` labels exist, use the same names. Leave the scope out when the change touches many parts.

## Description

The text after the colon.

- Use the imperative mood: "add", not "added" or "adds". It should complete the sentence "If applied, this commit will ...".
- Start with a lowercase letter and don't end with a period. The check doesn't enforce this.
- Keep it short, around 50 characters. GitHub adds ` (#123)` to the end of squash commits.
- Describe the change, not the files: `fix(devices): keep the filter when changing pages`, not `fix: update devices.py`.

## Body

The body is the PR description. [pull-requests.md](pull-requests.md) explains what to write there.

## Footers

- `Closes #123` or `Fixes #123` closes the issue when the commit reaches `main`. `Refs #123` links it without closing it. In a PR, use the `Closes #` line at the top of the template.
- For a breaking change, the `!` in the title is enough. Explain what breaks and how to adapt in the PR's Description, instead of a separate `BREAKING CHANGE:` line.
- `Co-authored-by: Name <email>` credits another person. GitHub adds these by itself when a squashed PR has commits from several people. Don't add these lines for AI tools.

## When you merge

"Squash and merge" fills in the commit message with the PR title and description. Before you confirm, check that the title still describes the whole change. Leave the description as it is.

## Examples

Good:

```text
fix(export): escape commas in CSV fields
refactor(db): replace raw SQL in device queries with the ORM
feat(api)!: require the API version header
docs(adr): add 0002 choose the deployment layout
```

Bad:

```text
fixed bug                # no type, past tense, which bug?
feat: Added new stuff.   # past tense, vague, trailing period
chore: wip               # says nothing
fix: address review      # describes the process, not the change
```
