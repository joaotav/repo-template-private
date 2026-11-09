# 0001. Record architecture decisions

- Status: Accepted
- Date: TODO
- Deciders: TODO

## Context

We make decisions about architecture, tools and conventions in meetings, chat threads and PR comments. Months later nobody remembers why a decision was made, new team members can't find out, and settled questions get reopened.

## Options considered

### Option 1: a wiki page

- Pro: easy to edit, no PR needed.
- Con: drifts away from the code, and changes aren't reviewed.

### Option 2: PR descriptions and commit messages

- Pro: already exists, no extra work.
- Con: the reasoning is scattered over many PRs and hard to find later.

### Option 3: ADRs in the repository

Short Markdown files, one per decision, as described by Michael Nygard in [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) (2011).

- Pro: versioned with the code and reviewed through PRs like any other change.
- Con: takes time to write.

## Decision

We will record significant decisions as ADRs in `docs/architecture/adr/`, using `templates/template.md` and the process in `README.md` in that folder. ADRs are living documents: they're edited when a decision changes, with each change logged in their Changelog. A reversed decision gets a new ADR.

## Consequences

- The reasoning behind a decision sits in the same repo as the code it affects.
- Big decisions take a bit longer, because someone has to write the ADR. The criteria in the README keep this to decisions that are worth it.
- The ADRs are only useful if we keep them up to date when a decision changes.

## Security considerations

None for the decision itself: it's about documentation. But every institution's server has a copy of the ADRs, so they describe risks in general terms and never include secrets or the details of an unfixed weakness.

## Changelog

- TODO: Accepted.
