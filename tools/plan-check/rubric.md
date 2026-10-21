# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's stated cause, read against the repro evidence's steps, outputs, and any control run. | Pass if the stated cause explains what the repro evidence shows and nothing in the repro evidence (a control run, an isolating step, a timing or output excerpt) rules it out. Fail if the cause contradicts or ignores that evidence, or if the change treats a symptom while the evidence points at a different cause. | required |
| scope-bounded | The plan's in-scope and not-in-scope statements and its list of changes, read against the one behavior the issue reports. | Pass if the plan is one bounded change that fixes the reported behavior; an explicit deferral of related work, or an honest scope-down with reasons, also passes. Fail if the plan bundles work the fix does not need (refactors, migrations, new options or features, redesigns, CI or harness changes), even when the core fix inside it is right. | required |
| executable | The plan's approach and the files or areas it names. | Pass if a stranger could start now: the plan names the files or areas to change and commits to one approach. Fail if a real decision is deferred to build time (where the fix goes, which layer, "whichever is easier", "investigate first", "profile and optimize"). | required |
| test-decisive | The plan's test plan, read against the repro evidence's steps and observed output. | Pass if the test plan re-runs the reproduced behavior or adds a regression test and names an observable result that tells fixed from not fixed (an output, exit code, count, or visible state). Fail if the outcome is not observable ("run the full suite", "should feel fast", "nothing else should break") or is not tied to the reproduced behavior. | required |
| thread-direction-engaged | The plan comment, read against the thread highlights (maintainer or owner direction, an identified culprit, a proposed approach, a patched build, prior-art PRs). | Pass if the thread contains no such direction, or the comment follows it, or the comment names it and gives a reason for differing, including open PRs on the same fix. Fail if the thread has explicit direction or prior art and the comment never engages it. | required |
| ai-policy-honored | The repo-facts block's contribution policy, read against the plan comment, treating the comment as AI-assisted work. | If the policy requires disclosing AI use (tool or extent), fail unless the comment contains that disclosure. If the policy only asks for comments in the contributor's own words, pass when the comment is specific and reasoned rather than generic. If no policy is stated, pass. | required |
| unknowns-stated | The plan's risks and unknowns, read against what the repro evidence leaves open. | Pass if the plan names what it has not verified or what could change the approach. Fail if open questions are presented as settled. | preferred |

## Verdict rule

Accept only if every required check passes. Preferred checks never change the verdict. Unclear counts as fail.
