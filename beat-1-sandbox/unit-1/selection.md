# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73

**Verdict output**

## Repo-level checks (apply to all three issues)

- **maintainer-alive**: last default-branch commit 2026-09-16, within 365 days of 2026-09-22 → **pass**
- **not-archived**: archived: no → **pass**
- **ai-policy-allows**: no CONTRIBUTING.md, no stated policy, silence passes → **pass**

## Issue #73 — README/.env.example key mismatch

- scope-bounded: single file-consistency fix, 1–2h estimate, no comments → **pass**
- not-claimed: no assignees, no linked PRs, no comments → **pass**
- good-first-issue: labeled "good first issue" → **pass**
- **Verdict: accept**

## Issue #72 — `verify_password` raises on malformed hash

- scope-bounded: single bounded fix + remove one xfail marker, 1–2h → **pass**
- not-claimed: no assignees, no linked PRs, no comments → **pass**
- good-first-issue: labeled "good first issue" → **pass**
- **Verdict: accept**

## Issue #69 — Output parser crashes on JSON array fallback

- scope-bounded: single bounded fix to fallback handling, 2–4h, no design ambiguity or abandoned PRs in thread → **pass**
- not-claimed: no assignees, no linked PRs; jacho15's comments are a classmate's claim/repro, which per the Path Review house rule does not block → **pass**
- good-first-issue: labeled "good first issue" → **pass**
- **Verdict: accept**

## Ranked read-out (fit profile: Python/docs, unambiguous scope, avoid security internals and heavy env setup)

1. **#73** — cleanest fit: pure docs/config consistency fix, fastest (1–2h), zero security surface, output is unambiguous.
2. **#69** — Python fix in the RAG output parser, bounded and non-security, but slightly larger (2–4h) and touches parsing logic rather than plain docs.
3. **#72** — otherwise solid, but the fix lives in `core/security.py` (password-hash verification), which sits in the security-sensitive internals this profile prefers to avoid.

```json
[
    {
        "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
        "checks": [
            {
                "name": "maintainer-alive",
                "grade": "pass",
                "evidence": "Last default-branch commit 2026-09-16, within 365 days of 2026-09-22"
            },
            {
                "name": "not-archived",
                "grade": "pass",
                "evidence": "archived: no"
            },
            {
                "name": "scope-bounded",
                "grade": "pass",
                "evidence": "Single task: make README.md and .env.example agree; estimated 1–2 hours"
            },
            {
                "name": "not-claimed",
                "grade": "pass",
                "evidence": "assignees: none, linked PRs: none, 0 comments"
            },
            {
                "grade": "pass",
                "evidence": "No CONTRIBUTING.md or stated AI policy"
            },
            {
                "name": "good-first-issue",
                "grade": "pass",
                "evidence": "Labeled 'good first issue' by owner Aburke225"
            }
        ],
        "verdict": "accept"
    },
    {
        "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
        "checks": [
            {
                "name": "maintainer-alive",
                "grade": "pass",
                "evidence": "Last default-branch commit 2026-09-16, within 365 days of 2026-09-22"
            },
            {
                "name": "not-archived",
                "grade": "pass",
                "evidence": "archived: no"
            },
            {
                "name": "scope-bounded",
                "grade": "pass",
                "evidence": "Single fix to fallback path in output_parser.py; estimated 2–4 hours, no design uncertainty in thread"
            },
            {
                "name": "not-claimed",
                "grade": "pass",
                "evidence": "assignees: none, linked PRs: none; jacho15's claim comment is a classmate and per house rule does not block"
            },
            {
                "name": "ai-policy-allows",
                "grade": "pass",
                "evidence": "No CONTRIBUTING.md or stated AI policy"
            },
            {
                "name": "good-first-issue",
                "grade": "pass",
                "evidence": "Labeled 'good first issue' by owner Aburke225"
            }
        ],
        "verdict": "accept"
    },
    {
        "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
        "checks": [
            {
                "name": "maintainer-alive",
                "grade": "pass",
                "evidence": "Last default-branch commit 2026-09-16, within 365 days of 2026-09-22"
            },
            {
                "name": "not-archived",
                "grade": "pass",
                "evidence": "archived: no"
            },
            {
                "name": "scope-bounded",
                "grade": "pass",
                "evidence": "Single fix: fail closed instead of raising, plus removing one xfail marker; estimated 1–2 hours"
            },
            {
                "name": "not-claimed",
                "grade": "pass",
                "evidence": "assignees: none, linked PRs: none, 0 comments"
            },
            {
                "name": "ai-policy-allows",
                "grade": "pass",
                "evidence": "No CONTRIBUTING.md or stated AI policy"
            },
            {
                "name": "good-first-issue",
                "grade": "pass",
                "evidence": "Labeled 'good first issue' by owner Aburke225"
            }
        ],
        "verdict": "accept"
    }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Run 1 (full): `agreement: 18/20 scored items  (bar: 18/20: PASS)` — two disagreements on issue-01 and issue-19, both clear-accept issues that my initial scope-bounded check incorrectly rejected.

Partial re-run (--only issue-01,issue-19) after revising the scope-bounded pass condition to clarify that a multi-step doc task or a bug with listed potential causes does not fail the check: `agreement: 2/2 scored items`

Run 2 (full, --save-run): `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

issue-01 (source: conda/conda#16475, category: clear-accept, gold: accept).

First run: my rubric returned **reject**, with the scope-bounded check grading **fail**. The issue body lists five separate doc changes (a new task page, updates to `manage-pkgs.rst`, `pip-interoperability.rst`, `new-features.md`, and an optional troubleshooting entry). My initial scope-bounded condition said "not an umbrella or tracking issue (a list of sub-tasks meant to be split into separate work)." The model read this multi-item list as an umbrella and failed the check.

The gold label is **accept** because all five changes serve a single coherent deliverable: moving one feature's documentation to a permanent home. The distinguishing factor is that these changes are meant to land in one PR by one contributor, not be claimed separately — the opposite of an umbrella. Once I added "A multi-step doc task that cohesively belongs in one PR does not fail this check" to the pass condition, the model graded issue-01 correctly.

**Check rationale**

The scope-bounded check, as currently written in `tools/issue-select/rubric.md`:

> Issue describes a single, implementable piece of work intended to land in one pull request. Fails if: (a) the issue is explicitly a coordination point for many separate PRs from different contributors (self-described as a megaissue, tracking issue, or explicitly says PRs are welcome for any part of it); (b) the core implementation approach is genuinely unresolved and requires a product decision that no maintainer has settled on; (c) the thread shows 2 or more closed or abandoned linked PRs combined with ongoing design uncertainty and no maintainer committing to a specific implementation path; or (d) it is a pure support or usage question. A bug report with listed potential causes, or a multi-step doc task that cohesively belongs in one PR, does not fail this check.

This wording evolved from a simpler condition that just said "not a list of sub-tasks meant to be split." I added conditions (a)–(d) with specific evidence anchors after the first run showed the model could not reliably distinguish "multi-item list within one PR" from "umbrella coordinating multiple PRs." The explicit examples at the end (bug with listed causes, multi-step doc task) were the fix that moved issue-01 and issue-19 from false reject to accept.

**Trade-offs**

The scope-bounded check's condition (c) — "2 or more closed or abandoned linked PRs combined with ongoing design uncertainty" — will miss an issue that has had many abandoned attempts but happens to have only one formally linked PR (with the others mentioned only in comments). A contributor taking such an issue would find the real difficulty hidden in the thread rather than in the linked PRs sidebar.

To verify nothing else changed: after the partial re-run on issue-01 and issue-19 showed both now pass, I did a full run that scored 20/20, confirming no previously correct verdict flipped.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** Issue #73 is a documentation consistency fix — README and `.env.example` disagree on which API key to use, and `core/config.py` defines both. The work is reading two config files, checking what the code actually expects, and updating whichever file is wrong. That is the kind of unambiguous, bounded task I was looking for: no deep domain knowledge required, no complex environment to spin up, and the expected output (two files that agree) is easy to verify. The 1–2 hour estimate fits a first contribution.

2. **What the verdict identified correctly, and what I weighed that the rubric could not.** The rubric correctly flagged all five required checks as pass: the repo has recent commits, is not archived, the issue is a single bounded task, nothing is claimed, and there is no AI policy. What the rubric cannot see is that the scope is unusually safe to touch — README and `.env.example` are pure text with no runtime side effects, so a mistake there is easily caught in review and carries no risk of breaking anything for other users. That made #73 more attractive than #69 (parser logic) or #72 (security code) even though all three passed the rubric.

3. **Anticipated difficulty in claiming it.** No comments and no linked PRs as of grading, so there is nothing to signal active competition. The Path Review house rule means any classmate claim comment I might see later does not block me. The main uncertainty is whether the "correct" answer is to update README, update `.env.example`, or update both — but `core/config.py` defines both keys, so reading that file will settle it quickly without requiring a discussion with the maintainer first.
