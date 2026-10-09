---
name: review-findings
description: Review a branch, pull request, merge request, or diff in one direct inline pass and report every finding in a fixed four-part shape - severity with file and line, the code as-is, the concrete failure scenario, and a comment ready to paste into the review. Use whenever the user asks to review, audit, check, or look over a change, diff, branch, PR, or MR, however phrased ("review this", "any issues here?", "look at my MR"). Static verification only - never runs the test suite or hands off to a multi-agent review unless the user asks.
---

# Review findings — one shape, every repo

When asked to review a branch, PR, MR, or diff: one direct inline pass. Read the diff, read
the touched files for context, reason, report.

## Verification

Static only: the project's analyzer, linter, and format check, and reading the touched tests to
judge what they actually prove. Never run the test suite unless the user asks. Never hand the
review to a multi-agent, background, or cloud review unless the user asks for one by name. If a
finding's correctness hinges on runtime behaviour, say so inside the finding instead of running
it.

## Report format

One block per finding, most severe first, a blank line between every part and between
findings:

```
**[CRITICAL|HIGH|MEDIUM|LOW]** `<file path>:<line>`

<the relevant code, as-is, in a code block>

<plain-language description: the concrete failure scenario or why it matters — name the
specific risk (silent bug, perf cost, behaviour a future edit could break), never just
"this is inconsistent">

Paste ready comment: <short, actionable comment with a concrete suggestion, ready to paste
into the PR or MR as-is>
```

Rules of the shape:

- Severity is always uppercase, always one of those four, always leading the block.
- The `Paste ready comment:` label is literal and capitalized exactly like that.
- No prose paragraphs mixed into a finding, no tables, no numbered headings instead of the
  severity tag.
- **Scope-gap findings** — a file the change *should* touch but doesn't — are legitimate, but
  stated as prose: name the file, say plainly that it is not in the diff, explain why its
  absence matters. Never dress one up with a `file:line` header and a snippet as if it were
  part of the changeset.

## Other review tools

Other review skills, commands, or subagents may be used to *find* issues. Their findings are
still reported in the shape above. A different review format replaces this one only when the
user asks for it by name.
