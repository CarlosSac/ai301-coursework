# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

Where it lives:
- Eval bundle: the "Plan context" block holds the plan's steps, its "Not in scope" line, its "Files" list, its test plan, and any deviation or deferral note. The "Candidate PR" block's "Diff" section shows what changed (each `--- a/` and `+++ b/` pair is one file); its "Title" and "Description" sections hold the PR's claims about the change.
- Live mode: `plan.md` (its Scope, Files I will touch, Approach, and `## Deviations` sections); the branch's diff from `git diff main...HEAD` and `git diff main...HEAD --stat`; the title (first line) and description of `pr_draft.md`.

What good looks like: every changed file and hunk falls inside the plan's scope or a deviation note, nothing the plan scopes out appears in the diff, and every planned item is in the diff or deferred in a note the description restates. Silent drift runs both ways: the diff does more than the plan (a new option, a rewrite, a rename pass) or less (a planned docs update missing). A description that claims "exactly as planned" or "docs updated" over a diff that says otherwise is drift too, not a comms problem.

## Test evidence (harness category: not-tested)

Where it lives:
- Eval bundle: the "Test evidence" section of the candidate PR, plus any before/after in the description; read against the plan context's test plan and repro evidence. The repo facts' "pull requests" and "contribution policy" lines name the repo's suite or check command, if any.
- Live mode: `test_evidence.md` (the repro steps re-run on `main` and on the branch, and the output of `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`); the Testing section of `pr_draft.md`; the test plan in `plan.md`; the checks listed in `.github/PULL_REQUEST_TEMPLATE.md`.

What good looks like: the plan's own repro command is shown with its before and after output, the after matches the plan's expected result, and the code path it exercises is the one the diff changes. Every scenario the test plan names has an observable result or an honest "not run". The repo's own checks are reported by name with their outcome, and a failing one is reported with its reason. "Tests pass" or "works on my machine" with nothing named is not evidence.

## Diff quality (harness category: unreviewable)

Where it lives:
- Eval bundle: the "Diff" section (every `+` and `-` line) and the "Commits" list.
- Live mode: `git diff main...HEAD` and `git log main..HEAD --format='%h %s'`.

What good looks like: every added or removed line belongs to the fix, its tests, or an entry the repo asks for, so a reviewer sees the change and nothing else. Debris tells: `print`, `eprintln!`, `console.log`, or `DEBUG` lines; commented-out code or earlier attempts; unused functions or `allow(dead_code)`; new TODO or FIXME notes; re-indented blocks, reordered imports, duplicated lines, or whitespace-only changes on lines the fix does not otherwise touch; commits named "wip", "fix", or "misc cleanups".

## Standards and comms (harness category: standards-wall)

Where it lives:
- Eval bundle: the repo facts' "pull requests" line (template sections, checklist items, issue link format, changelog or whatsnew asks) and "contribution policy" line (including any AI policy); the description and diff for where each ask is met.
- Live mode: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with `Closes #`, Changes, Testing checklist, Screenshots / Demo, Notes for Reviewers) and `docs/CONTRIBUTING.md` (including removing a seeded bug's `@pytest.mark.xfail` marker and matching `pyproject.toml` suppression); the course's rule that the AI-use disclosure goes in Notes for Reviewers; the description in `pr_draft.md`.

What good looks like: every template section has real content (or "N/A" with a reason), the issue link line is filled with the right number, any changelog or whatsnew entry the repo asks for is in the diff, Testing boxes are ticked only where the evidence shows them true, and where an AI policy asks for disclosure the description names the tool and how it was used, in the contributor's own words. A visibly ignored ask is a fail even under an excellent fix. (Whether the description's claims match the diff belongs to plan fidelity, above.)
