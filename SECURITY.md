# Security

## Reporting a security problem

If you find a security problem in this project, report it privately to the project's maintainers, in a direct message or email. Until it's fixed, don't describe it in an issue, a PR, a commit, a code comment or a group chat.

Every institution's server has a copy of this repository, and many people can see its issues and PRs. A weakness described there can be used before it's fixed.

In the report, include what you know:

- what the problem is and which part of the system it affects;
- how to make it happen;
- what someone could do with it.

Only try it on a system you run yourself, such as your local copy. Never on another institution's deployment.

A password, token or key committed by mistake counts too. Report it the same way, and see "If you commit a secret" in [CONTRIBUTING.md](CONTRIBUTING.md).

## Fixing it

The fix goes through a normal PR with the `security` label. Its title and description say what changed, not how to exploit the problem.

## Supported versions

Security fixes go into `main` only.
