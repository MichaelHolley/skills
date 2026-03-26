---
name: rewrite-gh-issue
description: Rewrite an existing GitHub issue for the current repository using the GitHub CLI, codebase context, and any matching issue template. Use when the user wants to improve, clarify, normalize, or fully rewrite a GitHub issue by issue number.
---

# Goal

Improve an existing GitHub issue by gathering the current issue content, investigating the local codebase for relevant context, asking focused follow-up questions to fill in missing details, and rewriting the issue in a clearer, more actionable structure. If a matching GitHub issue template exists in the workspace, use its style and structure. Always ask the user for final confirmation before updating the issue title and body.

# Inputs

Expected input from the user:

- a GitHub issue number
- optional intent, such as:
  - make this issue more actionable
  - rewrite this bug report
  - turn this into a proper feature request
  - align this with our issue template
  - clean up and update issue 123

# Tools and environment assumptions

This skill assumes:

- the current working directory is a local clone of the target GitHub repository
- the GitHub CLI `gh` is installed and authenticated
- the current git remote points to the repository containing the issue
- the workspace may contain issue templates under common GitHub locations

Common template locations to inspect:

- `.github/ISSUE_TEMPLATE/`
- `.github/issue_template/`
- `docs/`
- repository root for issue form or markdown templates

# High-level behavior

Follow this approach:

1. Validate the environment.
2. Fetch the issue by number using the GitHub CLI.
3. Inspect the current issue title, body, labels, and metadata.
4. Explore the codebase, docs, tests, and configuration related to the issue.
5. Ask the user targeted clarifying questions in a “grill-me” style.
6. Try to answer unresolved questions from the repository before asking more.
7. Find a matching issue template if one exists.
8. Rewrite the title and body in a cleaner, more complete form.
9. Present the proposed rewritten issue to the user.
10. Ask for explicit confirmation.
11. Only after confirmation, update the issue title and body using `gh issue edit`.

# Rewrite requirements

The rewritten issue should:

- have a clearer, more specific title
- be concise but complete
- use repository-specific terminology where appropriate
- follow the matched issue template when applicable
- remove ambiguity
- make assumptions explicit
- avoid invented facts
- preserve important existing details unless they are clearly obsolete or misleading

Prefer including, where applicable:

- summary
- current behavior
- expected behavior
- reproduction steps
- scope
- technical context
- proposed direction
- acceptance criteria
- risks or constraints
- unanswered questions
