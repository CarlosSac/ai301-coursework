# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Dates of the last 5 default-branch commits in the repo-facts block | The most recent default-branch commit is within 365 days of the bundle's capture date (or today in live mode) | required |
| not-archived | The "archived:" field on the repo line in repo-facts | The field reads "no" | required |
| scope-bounded | Issue body, labels, and the full comment thread | Issue describes a single, implementable piece of work intended to land in one pull request. Fails if: (a) the issue is explicitly a coordination point for many separate PRs from different contributors (self-described as a megaissue, tracking issue, or explicitly says PRs are welcome for any part of it); (b) the core implementation approach is genuinely unresolved and requires a product decision that no maintainer has settled on; (c) the thread shows 2 or more closed or abandoned linked PRs combined with ongoing design uncertainty and no maintainer committing to a specific implementation path; or (d) it is a pure support or usage question. A bug report with listed potential causes, or a multi-step doc task that cohesively belongs in one PR, does not fail this check. | required |
| not-claimed | "assignees:" and "linked PRs:" in the repo-facts block; claim comments in the thread | No open linked PRs AND no currently active assignee. Closed PRs and bot-cleared stale claims do not block. | required |
| ai-policy-allows | "contribution policy" line in repo-facts, including any linked CONTRIBUTING.md excerpts or dedicated AI policy files | No explicit ban on AI-generated or AI-assisted contributions. Silence passes. Disclosure or personal-review requirements pass. An outright ban ("we do not accept AI-generated code") fails. | required |
| good-first-issue | Labels listed in the issue header | Issue carries a "good first issue" or equivalent newcomer-friendly label added by a maintainer | preferred |

## Verdict rule

Accept if and only if every required check passes. One required check failing is enough to reject. Preferred checks never change the verdict; they rank accepted issues from best to least fit. Treat unclear as fail: if the evidence a required check needs is genuinely absent from the bundle, grade the check fail and reject the issue.
