# Releases and deployments

A release is a version of the system with a name, like `v0.4.0`. A deployment is a server starting to run one. Servers run releases, never `main`: `main` can have an urgent fix that hasn't been reviewed yet, or be red for a while, and institutions shouldn't get either.

## Versions

Versions look like `vMAJOR.MINOR.PATCH`, and start at `v0`:

- **MINOR** for a normal release: `v0.3.0`, then `v0.4.0`.
- **PATCH** for a release that only fixes the previous one: `v0.4.1`.
- **Release candidates** go to the test server first, with an `-rc` suffix: `v0.4.0-rc.1`, then `v0.4.0-rc.2` if the test round finds problems. When the round passes, the same commit is released without the suffix: `v0.4.0`.
- **`v1.0.0`** is the first version an institution runs in production. From then on, MAJOR goes up when a release needs institutions to change something by hand, such as their configuration.

## Who releases

The maintainers create releases: TODO: list them. Anyone can ask them for one.

Our GitHub plan (Free) lets anyone with write access create a tag, so this is an agreement. With GitHub Team, a tag ruleset could enforce it.

## Before a release

- The checks on the latest commit on `main` are green.
- Nothing that blocks a deployment is open: search issues for `is:open label:priority/high`.
- For a final release (no `-rc`): the test round for its release candidate is done. Its milestone is closed, or the issues left in it were moved.

If `main` has changes that haven't been through a test round, talk with the maintainers before releasing a patch.

## Making a release

Release candidates are created on `main`. A final release is created on the same commit as its last release candidate, so institutions get exactly what was tested, even if more PRs were merged into `main` during the test round. Those wait for the next release candidate.

A release candidate, on the repo page:

1. Go to Releases and click "Draft a new release".
2. In "Choose a tag", type the new version, such as `v0.4.0-rc.1`, and create it on `main`.
3. In "Previous tag", pick the last final release, such as `v0.3.0`, so the notes cover everything since then.
4. Click "Generate release notes". It lists the PRs merged since the previous tag, by title. Add a short summary on top if it helps.
5. Check "Set as a pre-release".
6. Click "Publish release".

The same from the command line:

```sh
gh release create v0.4.0-rc.1 --target main --generate-notes --notes-start-tag v0.3.0 --prerelease
```

A final release, from the command line in a clone of the repo. Replace `v0.4.0-rc.2` with the last release candidate:

```sh
git fetch --tags
gh release create v0.4.0 --target "$(git rev-parse 'v0.4.0-rc.2^{commit}')" --generate-notes --notes-start-tag v0.3.0
```

Fixes for a released version go to `main` like any other change, followed by a new release from `main`. There are no release branches.

## Deploying

TODO: the deployment steps. They depend on where the system runs, which isn't decided yet. The general shape on a server:

```sh
# Back up the data first.
git fetch --tags
git checkout v0.4.0
# TODO: the command that rebuilds and restarts the system.
```

After deploying, edit the release on GitHub and add a line under "Deployed", at the top of the notes:

```text
Deployed:
- test server, 2026-11-18
- Example University (pilot), 2026-11-25
```

Write where and when, nothing more. Server addresses, credentials and backup locations stay out of GitHub.
