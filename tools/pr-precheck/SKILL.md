---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
pull request ready to submit? A PR package is a candidate pull request
(its title, description, commits, diff, and test evidence), read
against the plan it claims to implement (including that plan's
deviation notes and test plan) and the issue that plan belongs to,
along with the repo's stated standards for pull requests. Answer only
that question, for exactly one package per run. Do not grade the plan
itself, re-diagnose the issue, review code style for its own sake, or
suggest a different fix. You answer by executing `procedure.md`, which
applies the checks in `rubric.md` to evidence located with
`references/evidence-guide.md`, never from gut feel.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode**: the student is checking their own PR before opening
  it. Run from the top folder of the student's clone of their fork, on
  their branch. Read these inputs, and nothing else counts as the
  package:
  - `plan.md` in the working copy: the posted plan, including its
    `## Deviations` section. Deviation notes are part of the plan.
  - The branch's diff: run `git diff main...HEAD` (three dots) for the
    content and `git diff main...HEAD --stat` plus
    `git log main..HEAD --format='%h %s'` for the file list and the
    commits. This is every committed change the branch makes relative
    to `main`. Uncommitted changes are not part of the PR; ignore them,
    and never treat `plan.md`, `comment.md`, `pr_draft.md`, or
    `test_evidence.md` as part of the diff.
  - `pr_draft.md`: the first line is the PR title, everything under it
    is the description.
  - `test_evidence.md`: the captured before/after output and the
    repo's check results.
  - The issue named in the request: read its thread with
    `gh issue view <number> --repo <repo> --comments` (or the GitHub
    API or web page).
  - The repo's standards: `.github/PULL_REQUEST_TEMPLATE.md` and
    `docs/CONTRIBUTING.md` in the working copy. Treat this course's
    requirement as part of the stated policy: every Path Review PR
    must carry an AI-use disclosure in the student's own words in the
    Notes for Reviewers section.

  A house-chain student reads the house plan and the house repro pack
  in place of their own plan and repro; the same checks grade the same
  things. If any input file is missing, grade the checks that depend on
  it from what is present and let the missing evidence decide those
  checks; do not go looking for substitutes elsewhere on disk.
- **Eval mode**: you are given a package bundle (a markdown file with
  repo facts, the issue and thread highlights, a plan-context block,
  and the candidate PR's title, description, commits, diff, and test
  evidence). The bundle is the whole world. Every fact comes from the
  bundle text; fetch nothing, run nothing, read no other file. Eval
  mode always grades a complete package: every check in the rubric,
  then the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still holds a placeholder (anything in angle
brackets, such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading
and tell the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope. If the issue, or the
repo the branch's PR would target, is not the repo named on that line,
refuse to grade and say which repo is in scope. Apply the house rules
scope.md lists as how evidence is read there: the PR comes from the
student's own fork branch, it delivers one posted plan for one issue,
and every section of the PR template gets real content. In eval mode,
ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`, the student's own rules for
writing upstream. Hold the outgoing PR text, the title and the
description in `pr_draft.md`, against those rules. In the summary,
list each rule the draft breaks, quoting the rule's name and the
offending line. The voice guide never changes the verdict on its own:
it is personal style, and the verdict comes only from the rubric's
checks. In eval mode, ignore `voice-guide.md` entirely; voice carries
no gold labels, and the universal communication checks live in the
rubric.

## Component reads

- `rubric.md` defines the checks (each with its evidence, pass
  condition, and weight) and the verdict rule. It decides what to
  judge.
- `references/evidence-guide.md` is the map: for each evidence family
  it says where the evidence lives in a bundle and in a live working
  copy, and what good looks like.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, and verdict assembly. Execute it as
  written, step by step. Where it is silent on a step you need, say so
  in the summary as a procedure gap and proceed with the closest
  written step; never invent a new check or a new source.

If `rubric.md` has no checks in its table, or `procedure.md` has no
steps under its headings, stop and say which file is empty. Do not
grade: this tool cannot grade without a rubric and a procedure, by
design, and checks invented at runtime are noise.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; reservations go in the
check evidence lines. Before the JSON block, show a short readable
summary: one line per check with its grade, the deciding check if the
verdict is reject, and (live mode) any voice-guide rules the draft
breaks. Then end the reply with this fenced JSON block, valid and
last, with nothing after it. Use the rubric's check names, in rubric
order, one entry per check:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode the PR does not exist yet, so set `item` to the issue URL
followed by the branch name. In eval mode, set `item` to the bundle id
(for example `pkg-07`).

## Grading discipline

- Evidence first: never grade a check without naming the file, hunk,
  quote, or output line that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse PR whose diff matches its
  plan and whose evidence is decisive can be ready, and a polished,
  confident description can be hiding drift. Read the diff itself
  against the plan, and the evidence itself against the test plan;
  never trust the description's own claims about either.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict
  rule directs. Where the rule is silent, an unverifiable claim is a
  failing one: a PR you cannot verify from the package is not ready to
  submit.
- An honestly disclosed shortfall is not a failure: a PR that does
  less than everything, and says so in the plan's notes and in its
  description, can be ready.
