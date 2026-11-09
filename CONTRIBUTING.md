# Contributing

How to get a change into `main`, step by step. The reasons behind each rule are in [docs/repository-guidelines.md](docs/repository-guidelines.md). AI agents follow [AGENTS.md](AGENTS.md), which points to the same docs.

## Before you start

- If the work has an issue, assign yourself so nobody else picks it up, and link it in the PR. To track something new, open an issue: click "New issue" and pick Bug report, Feature request or Other.
- If the change is an architectural decision (a major new dependency such as a database or framework, a data format, how two components talk to each other), write an ADR, in the same PR or before it. See [docs/architecture/adr/](docs/architecture/adr/README.md).

## Setup

TODO: how to install dependencies, run the project and run the tests.

Real passwords, tokens and keys go only in files named `.env`. Git ignores these files. Claude Code and Cursor are set to stay out of them, and other agents are told to in [AGENTS.md](AGENTS.md). To set one up, copy `.env.example` to `.env` and fill in the values. Any other name, such as `.env.local` or `secrets.txt`, gets none of this protection.

## Workflow

1. Create a branch from an up-to-date `main`:

   ```sh
   git switch main && git pull
   git switch -c ana/41-allow-bulk-registration
   ```

   Name it `<github-username>/<issue-number>-<short-description>`. Without an issue, leave out the number: `ana/readme-typo`. To find your username, run `gh api user --jq .login` or look at your GitHub profile. Branches made by Copilot's cloud agent start with `copilot/`, which is fine.
2. Commit as you go. Your branch commits don't need to be polished: the pull request (PR) is squashed into one commit on `main`. When possible, run the tests locally before you push. Push when the work is ready for the checks, not after every commit. Every push runs CI, and our CI minutes are limited.
3. Push and open a PR.
   - **Title:** `type(scope): description`, for example `feat(devices): allow bulk registration`. The title becomes the commit message on `main`. See [docs/commit-messages.md](docs/commit-messages.md).
   - **Description:** fill in the template. [docs/pull-requests.md](docs/pull-requests.md) explains each part and has a full example.
4. Wait for the checks at the bottom of the PR: `pr-title`, `gitleaks` and `tests` (`tests` doesn't run when a PR only changes docs, and `pr-title` only shows up when the PR is opened or its title changes). Fix any red check before asking for review.
5. Ask someone who knows the part of the code you changed to review it ("Reviewers", on the right side of the PR).
6. Answer review comments with new commits. Don't force-push once review has started: reviewers lose track of what changed since they last looked.
7. If `main` has moved on, click "Update branch" on the PR and choose "Update with merge commit". The rebase option rewrites your commits, which causes the same problem as a force-push.
8. Click "Squash and merge" when all of these are true:
   - someone approved, and nobody pushed new commits after that approval (clicking "Update branch" doesn't count, but resolving merge conflicts by hand does);
   - the person who approved didn't push commits to this PR;
   - all checks are green;
   - the branch is up to date with `main`.

   Either the author or the reviewer who approved can merge. Before confirming, check that the title still describes the whole change. GitHub deletes the branch after the merge.
9. After the merge, the checks run again on `main`. If they fail, fix it or revert your PR.

Our GitHub plan (Free) can't enforce the conditions in step 8, so GitHub will let you merge without them. Check them yourself.

Urgent fixes can merge without an approval. See [Urgent fixes](docs/repository-guidelines.md#urgent-fixes).

## Keep pull requests small

Prefer small PRs: reviewers skim big ones and miss things. If a PR mixes a refactor with a behavior change, say so in the description, so the reviewer knows what to look for. If a feature is big, split it into steps that each leave `main` working.

## Reviewing

- When it matters whether a comment blocks the merge, start it with a prefix:
  - `blocking:` must change before merge. Example: `blocking: the upload crashes when the CSV is empty.`
  - `suggestion:` worth considering; the author decides. Example: `suggestion: this loop could use the existing validate_mac().`
  - `question:` you need an answer before you can approve. Example: `question: why 500 as the limit?`
  - `nit:` a minor point that never blocks. Example: `nit: typo in "adress".`
- Read the whole diff, in the PR's "Files changed" tab. If the change has no automated tests, try it yourself when you can.
- Approve when the change leaves the code better than before, even if you'd have written it differently.
- Don't approve a PR you pushed commits to. Ask someone else.
- If the author pushes new commits after you approved, look at them and approve again. Clicking "Update branch" doesn't count, but resolving merge conflicts by hand does.

## If you commit a secret

Once a secret is pushed, assume someone else has it. Deleting the commit doesn't help, because every clone already has a copy. What usually needs to happen:

- tell whoever manages that secret, right away;
- replace it (a new password, token or key), so the leaked one stops working;
- remove it from the code and move it to `.env`.
