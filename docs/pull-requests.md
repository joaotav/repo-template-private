# How to open a pull request

The description becomes the body of the commit on `main`, including the tips
between `<!--` and `-->`. Replace each tip with your text, or delete it.

## Title

The title becomes the commit message on `main`, so follow the format
`type(scope): description`. Example: `feat(devices): allow bulk registration`.
The types are listed in [commit-messages.md](commit-messages.md).

## Closes #

If the PR resolves an issue, replace `#` with its number: `Closes #41`.
The issue closes by itself when the PR is merged into `main`. If there's no issue, delete the line.

## Description

In a few sentences: what was wrong or missing, and what you changed to fix it.

## Testing

Describe what you ran and what you saw, step by step, as if someone else
were going to repeat it. Don't paste the output of the automated tests: CI already shows it.

If an AI agent wrote the code, we recommend both:

- asking the agent to test it thoroughly: run the tests, and add tests for the new behavior and the cases that could break it;
- testing it yourself by hand, when you can, the way a user would.

If you didn't test, write "Not tested" and explain why.

## Checklist

Check each item, then mark it with `[x]`.

- **Passwords, tokens, keys, `.env`:** look through the diff for any secret
  value, including in tests and examples. Real values go only in `.env`.
- **Tests:** new behavior and fixed bugs get a test. If no test applies,
  for example in a docs-only PR, mark the item anyway.
- **Docs:** if the PR changes how something works or how it's used, update
  the docs that describe it, such as the README, `docs/` or an ADR.

## Full example

**Title:** `feat(devices): allow bulk registration`

```
Closes #41

## Description
Admins registered one device at a time, which took hours in
large labs. This PR lets them upload a CSV with up to 500 MACs.

## Testing
The agent added tests for valid lines, invalid MACs and files over 500
lines, and they pass. Then I tested it by hand: I started the system
locally and logged in as an admin. I uploaded a CSV with 3 valid
MACs and 1 invalid one. The 3 showed up in the list; the invalid one showed
"Invalid MAC on line 4". I logged in as a regular user and the upload
button doesn't show up.

## Checklist
- [x] No passwords, tokens, keys or `.env` files in the diff
- [x] Tests added or updated, if applicable
- [x] Docs updated, if applicable
```
