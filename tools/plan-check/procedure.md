# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md` and confirm the issue's repo matches its `Repo:` line. If it does not, or the line still holds a placeholder, stop and say so. In eval mode, skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. List the checks and the verdict rule before opening the package.
3. Read the repo facts first. Note the stated bug-report template, the contribution policy, and any AI-use rule, in one line each. These feed `ai-policy-honored` and `thread-direction-engaged`.
4. Read the issue and the thread highlights next. Note the behavior the issue reports, and every piece of maintainer, owner, or contributor direction: an identified culprit, a proposed approach, a patched build, a prior-art PR or open PR on the same fix.
5. Read the repro evidence before the plan. For each step and each control run, note what it shows and what it rules out. Reading it before the plan means the plan's cause gets judged against the evidence, not against the plan's own framing.
6. Read the candidate plan: its cause, in-scope and not-in-scope statements, files or areas, approach, test plan, and risks. Then read the candidate plan comment last, as a maintainer on the thread would read it.
7. Read the whole package once before grading any check. Do not grade while reading.

## Evidence gathering

1. `cause-grounded`: quote the plan's stated cause. Quote the repro lines that bear on it, especially any control, isolating step, or output excerpt. Record whether any quoted repro line rules the cause out.
2. `scope-bounded`: quote the plan's in-scope and not-in-scope lines and list every distinct change the plan names. Record which of them the reported behavior needs and which it does not.
3. `executable`: quote the files or areas the plan names and the approach it commits to. Record every decision the plan leaves open for build time.
4. `test-decisive`: quote the test plan and the repro step it re-runs. Record the observable result it names (an output, exit code, count, or visible state). If it names none, record that.
5. `thread-direction-engaged`: list the direction items noted from the thread highlights. For each, find the plan comment's sentence that follows, answers, or disagrees with it. Record any item the comment never mentions.
6. `ai-policy-honored`: quote the policy line from the repo facts. If it requires disclosure, search the plan comment for a disclosure naming the tool or extent. If it only asks for own words, judge whether the comment is specific to this issue.
7. `unknowns-stated`: quote the plan's risks and unknowns. Record any open question in the repro evidence that the plan treats as settled.
8. Live mode: take the issue-side items (steps 3 to 5 of the read order) from the issue thread and the repo's docs as the evidence guide directs, and the repro evidence from the student's posted repro comment. Take the plan and comment only from the drafts.
9. Eval mode: use only the bundle text and fetch nothing.

## Check execution

1. Run the checks in table order, one at a time. Grade each against its own evidence from the gathering step, not against how the other checks came out.
2. Grade each check `pass`, `fail`, or `unclear`, with one line naming the quote or fact that decided it.
3. Apply the pass condition as written. If a check passes by its condition but feels wrong, it still passes; note the tension in the summary.
4. Grade `unclear` only when the evidence the check needs is absent from the package. Not having looked is not unclear.
5. If the package has no thread direction, no prior art, or no stated policy, the check that depends on it passes by its condition.
6. A check may be graded from the quotes already gathered without rereading the package. Reread only when a quote is missing or two quotes conflict.
7. Where this procedure is silent, note the gap in the summary. Do not invent a step.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes. Preferred checks never change the verdict. Count `unclear` as `fail`.
2. If any required check failed or was unclear, the verdict is `reject`. Name each failed check and quote the line that decided it.
3. If every required check passed, the verdict is `accept`.
4. Live mode: hold the draft plan comment against `voice-guide.md` and list any rule it breaks, quoting the rule. This never changes the verdict.
5. Show a short summary, one line per check. End with the JSON block from `SKILL.md` as the last thing in the output, and write nothing after it.
