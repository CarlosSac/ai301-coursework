# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives.** Eval bundle: the plan's `Cause:` line under `## Candidate plan`, and the `## Repro evidence` block, which holds the numbered steps, any control run, and the Actual/Expected lines. Live: the cause stated in `plan.md`, and the student's posted repro comment on the issue thread (the steps, the output excerpts, and any "control" or "what I am not claiming" section).

**What good looks like.** The cause explains what the repro output shows, and no control or isolating step in the repro rules it out. In calib-01 the cause (the commits view is not refreshed after a push) matches repro steps 3 and 4, where the color flips only after leaving and re-entering the view. A cause that a control run contradicts, or that skips over an isolating step, is the failure this section exists to catch, however confident the plan sounds.

## Scope

**Where it lives.** Eval bundle: the `In:` and `Out:` statements and the list of changes under `## Candidate plan`. Live: the in-scope and not-in-scope lines of `plan.md`, plus the issue body for the one behavior the issue reports.

**What good looks like.** One bounded change that fixes the reported behavior, with an explicit not-in-scope line naming what is left alone. An honest scope-down with reasons also counts as bounded. A plan that adds refactors, migrations, new options, redesigns, or CI changes the fix does not need is a drive-by rewrite, even if one correct fix sits inside it.

## Executability

**Where it lives.** Eval bundle: the `Change:` or approach lines and the named files or areas under `## Candidate plan`. Live: the files-to-touch and approach sections of `plan.md`.

**What good looks like.** A stranger could start without asking the author anything: the files or areas are named and one approach is chosen. In calib-01 the plan names `pkg/gui/controllers/sync_controller.go` and the exact change. Phrases like "somewhere", "whichever is easier", "not sure which layer", or "investigate first" mean real decisions are deferred to build time.

## Test plan

**Where it lives.** Eval bundle: the `Test:` lines under `## Candidate plan`, read against the steps and observed output in `## Repro evidence`. Live: the test-plan section of `plan.md`, read against the steps in the student's posted repro comment.

**What good looks like.** The test plan re-runs the reproduced behavior (or adds a regression test) and names an observable result that tells fixed from not fixed: an expected output, exit code, count, or visible state. In calib-01, "at step 3 the color must flip without leaving the view" is observable. "Run the full test suite" and "nothing else should feel broken" name no outcome for the fix itself.

## Honesty

**Where it lives.** Eval bundle: any risks or unknowns lines in `## Candidate plan` or the plan comment, read against what `## Repro evidence` leaves open (for example, a version or platform not tried). Live: the risks-and-unknowns section of `plan.md`, and the `## Deviations` section after a build.

**What good looks like.** Stated unknowns name what has not been verified or what could change the approach, and the plan does not present them as settled. An honest mid-build deviation is recorded under `## Deviations` in `plan.md` and is part of the plan when it is graded again. False confidence is an open question written as fact.

## Comms

**Where it lives.** Eval bundle: `## Candidate plan comment`, read against `## Thread highlights` (maintainer, owner, or contributor direction; a named culprit; a proposed approach; a patched build; prior-art or open PRs) and the `## Repo facts` block (bug-report template asks, contribution policy, any AI-use rule). Live: the draft in `comment.md`, read against the issue thread and the repo's `CONTRIBUTING.md` and PR template.

**What good looks like.** A thread-aware comment names the direction already on the thread and either follows it or says why it differs, and it engages any open PR on the same fix. In pkg-20 the comment follows the maintainer's proposed approach, and in pkg-03 it names prior art and the open PR. Where the repo requires AI-use disclosure, the comment states the tool or extent. Treat every comment as AI-assisted work. Boilerplate that could sit on any issue, or a comment that ignores explicit maintainer direction, is what this section catches.
