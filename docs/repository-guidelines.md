# Repository guidelines

How work moves through this repository, and why we chose each rule. For the step-by-step version, see [CONTRIBUTING.md](../CONTRIBUTING.md).

## Branching

We use trunk-based development: `main` is the only long-lived branch, and it must always build and pass the tests.

- Branch from `main` and merge back into `main`. There are no `develop` or release branches.
- Keep branches short-lived. The longer a branch lives, the worse its merge conflicts get and the harder it is to review.
- If unfinished work has to land early, keep it hidden from users until it's ready.

## Merging

PRs are merged with "Squash and merge" only. The other merge buttons are turned off in the repo settings.

Why squash:

- Each commit on `main` is one reviewed PR that passed the checks, so going back to any commit gives you a working state.
- Undoing a feature means reverting one commit.
- Nobody has to clean up their branch history. Commits like "fix typo" and "address review" stay out of `main`.
- `git log --oneline` on `main` reads like a changelog.

The cost: the individual branch commits don't reach `main`. They stay visible in the PR on GitHub, which the squash commit links to with `(#123)`.

The repo is set to use the PR title as the commit subject and the PR description as the commit body. Whatever is in the description ends up in the history of `main`, so write it for someone reading it a year from now.

Either the author or the reviewer who approved can merge.

## Team agreements for `main`

Our GitHub plan (Free) can't enforce rules on private repos. These are agreements: nothing stops someone from breaking them, so reviewers point it out when it happens. With GitHub Team, each one could be enforced in the repo settings.

- **Changes reach `main` through a PR, never a direct push.** Every change on `main` has been read by a second person and passed the checks.
- **One approval from someone who didn't write the code.** If you pushed commits to someone else's PR, you're an author too, so another person approves.
- **An approval covers only the commits the reviewer saw.** After a new push, the reviewer looks again and re-approves. Clicking "Update branch" doesn't count: it only brings in commits already reviewed on `main`. If you resolved merge conflicts by hand, that does count, because you changed code.
- **All checks are green.** `pr-title` checks the title format, `gitleaks` looks for passwords and tokens, and `tests` runs the test suite.
- **The branch is up to date with `main`.** The checks then ran against what `main` will look like after the merge, not an older version.
- **If `main` goes red, fixing it comes first.** `tests` and `gitleaks` also run on every commit that lands on `main`. This catches two PRs that pass on their own but break together, and secrets in anything pushed to `main` without a PR. Whoever merged last fixes it or reverts their PR.
- **Nobody force-pushes to `main` or deletes it.** Rewriting `main` breaks every clone, including the servers at each institution.

If you have GitHub Copilot, you can add it as a reviewer for a quick first pass. It doesn't replace the human approval.

### Urgent fixes

An urgent problem (labeled `priority/high`) that can't wait for a reviewer, such as a broken deployment, can be merged without an approval:

- the checks are still green;
- the PR says why it couldn't wait;
- someone reviews it afterwards. Problems they find get a normal follow-up PR.

## Labels

GitHub's default labels are deleted. We use these:

- `type/bug`: something doesn't work as it should.
- `type/feature`: new behavior.
- `type/refactor`: restructuring code without changing what it does.
- `type/docs`: documentation.
- `type/test`: tests.
- `type/maintenance`: upkeep that fits none of the above, such as config or cleanup.
- `type/dependencies`: dependency updates. Dependabot adds it to its PRs.
- `priority/high`: needs attention soon, including anything that must be fixed before the next deployment. Without this label, an issue has normal priority.
- `security`: touches login, permissions, secrets or personal data.
- `needs-triage`: nobody has looked at this issue yet. The issue forms add it.

Rules:

- Each issue gets exactly one `type/` label. The Bug and Feature forms add theirs; issues opened with Other get theirs at triage.
- PRs don't need a `type/` label, because the PR title already has the type.
- When a part of the system needs its own filter, add an `area/<name>` label, such as `area/reports`. Use the same names as commit scopes.

Some statuses don't get a label:

- Waiting on something: say so in a comment, with a link to what it's waiting on.
- Missing information: ask in a comment and leave `needs-triage` on.
- Won't do, or a duplicate: close the issue with GitHub's reason, "Close as not planned" or "Close as duplicate".

## Big pieces of work

When a piece of work is too big for one PR, split it into sub-issues:

- open a parent issue with the Feature request or Other form, describing the whole goal;
- on the parent, use "Create sub-issue" for each part, or "Add existing issue" for parts that already have an issue;
- each part gets its own PR, which closes its sub-issue with `Closes #`.

The parent shows how many of its sub-issues are closed. Close the parent when the last one is done.

## Test rounds

Each round of user testing runs on a release candidate on the test server, such as `v0.4.0-rc.1`, and gets a milestone named `Test round YYYY-MM-DD`. Issues found during the round go into it, and the milestone page shows how many are closed. Close the milestone when the round's issues are done or moved elsewhere.

An issue can be in only one milestone.

## Releases and deployments

Versions, who releases, and how to deploy are in [releases.md](releases.md).

## Triage

Someone regularly goes through the issues labeled `needs-triage` and, for each one:

- checks the `type/` label, or adds one for issues opened with Other;
- adds `priority/high` if it's urgent or blocks a deployment;
- adds an `area/` label if one fits;
- removes `needs-triage`.

## Dependencies

Dependabot opens update PRs once a week. It waits 7 days after a new version comes out before proposing it, because broken releases are usually caught and pulled in that time. Security fixes aren't delayed: when a dependency has a known vulnerability, the update comes right away.

Review these PRs like any other. Read the changelog for major versions and let the checks run. Don't let them pile up: one week's updates are easier to review than three months' worth.

GitHub Actions in the workflows are pinned to a commit SHA, with the version in a comment: `uses: actions/checkout@<sha> # v7.0.1`. A version tag can be moved to point at different code, but a SHA can't. Dependabot updates both.

## CI minutes

GitHub Actions minutes are limited each month. When they run out, the checks stop running until the next month. To make them last:

- a new push cancels the checks still running for the previous one;
- `tests` doesn't run when a change only touches docs;
- `pr-title` runs only when a PR is opened or its title changes, not on every push;
- `pr-title` and `gitleaks` stop after 5 minutes if they hang;
- Dependabot groups each week's updates into one PR per package ecosystem;
- when possible, people and agents run the tests locally, and they push when the work is ready for the checks.

An org owner can see how many minutes are left in the org's Settings, under Billing.

## Architecture decisions

Significant decisions are recorded as ADRs in [docs/architecture/adr/](architecture/adr/README.md).
