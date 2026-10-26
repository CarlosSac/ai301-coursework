# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/243

**Branch**

`fix/73-env-example-openrouter-key`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--only calib-01,calib-02,calib-03,calib-04 --include-calibration` (a trap check before spending on a full run): `agreement: 0/0 scored items`, since calibration packages are never scored; all four calibration rows agreed (`calib-01 accept`, `calib-02`, `calib-03`, `calib-04` all `reject`).
2. Full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`, with `categories: clear-accept 4/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. Every reject agreed; pkg-05, pkg-13, and pkg-19 (all clear accepts) were rejected with `failed: evidence-decisive`.
3. `--only pkg-05,pkg-13,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration` after loosening `evidence-decisive`: `agreement: 7/7 scored items` (the three misses flipped to accept; the four not-tested canaries and calib-04 still rejected).
4. Full run, saved with `--save-run eval-run.txt`: `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 5/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

**Package analysis**

pkg-02 (Textualize/rich#4208, category clear-accept). Gold label: `accept`. My rubric's verdict in the final run: `reject`, `failed: diff-clean`, with the evidence line "save_html and save_svg hunks remove and re-add identical lines ('html = self.export_html(', 'theme=theme,', 'svg = self.export_svg(', 'title=title,'), which indicates re-printed lines or whitespace churn; only the clear= line should change."

The grader read it that way because `diff-clean` lists "duplicated or re-printed identical lines" as a debris tell, and the pkg-02 diff really does show `-        html = self.export_html(` followed by `+        html = self.export_html(` (and the same for `theme=theme,` and `title=title,`). Read against the plan, though, those lines are just the hunk around the one real change in each call, `clear=clear,` becoming `clear=False,`, and nothing else in the diff is unrelated. The same package agreed (`accept`) in run 2 with the same `diff-clean` wording, so this is a borderline read of my own pass condition rather than a missing check. I wrote that tell for pkg-18, whose gold note describes "two blocks of identical re-printed lines (pure churn)". My rubric doesn't distinguish identical lines a diff shows around a real one-token change from identical lines that are pure churn, and pkg-02 lands on the wrong side of that gap.

**Check rationale**

> | evidence-decisive | The test evidence (and any before/after in the description), read against the plan's test plan and the repro steps it re-runs. | Pass if the plan's own repro is re-run on the changed code path and shows observable output for the after (the command and what it printed or did), with the before either shown or quoted from the repro, AND every other failure mode the test plan names is mentioned with its result or explicitly disclosed as not run. Controls and secondary scenarios pass when their result is stated in a line ("control unchanged", "prints the warning"); only the plan's main repro needs pasted output. Fail if the main repro's after is a claim without output ("tested locally", "works now", "verified on my machine"), if the evidence only exercises a path the fix does not change (a control instead of the repro, a different request shape), or if it never mentions a failure mode the test plan names. | required |

It reads this way because of run 2. My first version said "every other scenario or failure mode the test plan names is reported with an observable result", and the grader applied it literally: pkg-05 failed because "the plan's same-key scenario is only the bare claim 'Same-key redefine prints the one-time warning'", pkg-13 because "the plan's no-ampersand control is only asserted ('No-ampersand control unchanged')", and pkg-19 because "the test plan's two controls are only asserted". All three are clear accepts that paste full before/after output for the main repro. So I split the condition in two. The main repro still needs pasted output, which keeps pkg-04 ("tested locally, colors work now") and pkg-10 ("verified working on my machine") failing. Secondary scenarios and controls only need their result stated. I kept "never mentions a failure mode the test plan names" as a fail on purpose, because that is the line calib-04 turns on: it re-runs repro 1 and is "silent on the --exec abort the plan named". I also kept "a control instead of the repro", because that is pkg-07's failure (it "runs only the single-file control").

**Trade-offs**

Loosening `evidence-decisive` gives up some strictness: a PR can now pass with a control or second scenario that is only claimed in a sentence, not shown, so a description that writes "control unchanged" without running it would get through. I accepted that because the main repro still needs real output. To check that the loosening didn't open the not-tested category, I re-ran all four not-tested packages as canaries in run 3 (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`), plus `calib-04` as a free trap check. All five still rejected, and run 4 kept `not-tested 4/4`.

The case I accept missing is pkg-02 above (and pkg-16, which flipped in run 4 on `description-matches-diff`). I did not loosen `diff-clean`'s "duplicated or re-printed identical lines" tell to fix pkg-02, because pkg-18's gold reject rests on exactly that kind of churn, and its note calls it "arguable because the debris is mechanical churn around a right fix". Loosening the tell would likely flip pkg-18 from a correct reject to a wrong accept and trade one miss for another. With the bar met at 18/20 and every category matched, I kept the stricter tell.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
