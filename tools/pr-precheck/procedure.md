# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` and confirm the issue's repo matches its `Repo:` line. If the line still holds a placeholder, or the repo does not match, stop and say so. In eval mode, skip this step.
2. Read `rubric.md` and `references/evidence-guide.md`. List the checks and the verdict rule before opening the package.
3. Read the repo facts (live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`). Write down, one per line: every section and checklist item the PR template asks for, every explicit PR ask in the contributing guide (issue link, changelog entry, tests, test command), and the AI policy, or "no stated AI policy". These feed `template-asks-met`, `repo-checks-reported`, and `ai-policy-honored`.
4. Read the issue and its thread highlights. Note the reported behavior and any maintainer direction.
5. Read the plan before anything the PR says. Write down: the files and changes it puts in scope, everything it puts out of scope, its test plan as a list of scenarios with expected results, and every deviation or deferral note. Reading the plan first means the diff gets judged against the plan, not against the description's account of the plan.
6. Read the diff next. List each changed file, and for each hunk write one line saying what it changes.
7. Read the test evidence, then the commits, then the title and description last, the way a reviewer reads them after seeing the code.
8. Read the whole package once before grading any check. Do not grade while reading.

## Evidence gathering

Each gather is a side-by-side pairing. Record the quotes; the checks grade from them.

1. `diff-matches-plan`: put the hunk list from read-order step 6 next to the plan's scope list from step 5. Mark each hunk as "plan step", "deviation note", "test or required entry", or "not in plan". Then walk the plan's in-scope list and mark each item "in diff", "omitted with note in plan and description", or "omitted silently".
2. `description-matches-diff`: list every factual claim in the title and description about what the change does or touches, including fidelity claims such as "exactly as planned" or "no changes beyond". Next to each, quote the hunk that bears it out, or write "not in diff" or "contradicted by" with the hunk.
3. `evidence-decisive`: put the plan's test plan scenarios next to the test evidence. Mark which scenario is the plan's main repro. For the main repro, quote the command and the before and after output shown, or write "claim without output" or "different path". For every other scenario or control, quote the line that states its result, or write "not reported". Note which code path the evidence exercises and whether the diff changes that path.
4. `repo-checks-reported`: quote the repo's stated suite or check requirement from read-order step 3, or record "none stated". Quote the evidence line that reports each one's outcome, or write "not shown".
5. `diff-clean`: scan every added and removed line for the debris tells in the rubric (debug output, commented-out code, dead code, new TODO or FIXME, re-indentation, import reordering, duplicated lines, whitespace churn). Quote each hit with its file.
6. `template-asks-met`: for each ask listed in read-order step 3, quote where the description or diff meets it, or write "missing".
7. `ai-policy-honored`: quote the AI policy line. If it requires disclosure, quote the description's disclosure sentence or write "no disclosure".
8. `commits-descriptive`: list the commit messages.
9. Live mode: take the plan from `plan.md`, the diff and commits from `git diff main...HEAD` and `git log main..HEAD`, the title and description from `pr_draft.md`, the evidence from `test_evidence.md`, the thread from the issue on GitHub, and the repo facts from the template and `docs/CONTRIBUTING.md`, as the evidence guide directs. The course's disclosure rule counts as a stated AI policy that requires disclosure.
10. Eval mode: use only the bundle text and fetch nothing.

## Check execution

1. Run the checks in rubric table order, one at a time. Grade each from its own gathered quotes, not from how other checks came out.
2. Grade each check `pass`, `fail`, or `unclear`, with one line naming the quote, hunk, or output line that decided it.
3. Apply the pass condition as written. If a check passes by its condition but feels wrong, it still passes; note the tension in the summary.
4. Grade `unclear` only when the evidence the check needs is absent from the package (for example, no test evidence section at all). Not having looked is not unclear; go back to the gathering step.
5. If the repo states no template, no suite requirement, or no AI policy, the check that depends on it passes by its condition.
6. A check may be graded from the gathered quotes without rereading the package. Reread only when a quote is missing or two quotes conflict.
7. Where this procedure is silent, note the gap in the summary. Do not invent a step.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check passes. Preferred checks never change the verdict. Count `unclear` as `fail`.
2. If any required check failed or was unclear, the verdict is `reject`. The deciding check is the first failing or unclear required check in rubric table order; quote its deciding line in the summary, then name any other failed checks.
3. If every required check passed, the verdict is `accept`.
4. Live mode: hold the title and description against `voice-guide.md` and list any rule they break, quoting the rule. This never changes the verdict. Also list each fix the student needs to make for every failed check, one line each.
5. Show the short summary, one line per check. End with the JSON block from `SKILL.md`, with the checks in rubric order, as the last thing in the output, and write nothing after it.
