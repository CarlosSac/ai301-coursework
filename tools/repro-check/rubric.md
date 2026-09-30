# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| matches-issue | The candidate's reproduction input and command, read against the original issue's input and command. | Pass if the input and command are the same, or a documented functional equivalent (e.g. piping input vs reading a file, same effective config). Fail if the input, syntax, or command differs in a way that changes what is being tested. | required |
| failure-match-issue | The candidate's observed error or output, read against the failure the issue describes. | Pass if the observed error or output matches the kind of failure described in the issue, not a different kind of error. Also pass an honest, evidenced cannot-reproduce: a real attempt with artifacts shown, that names what differed from the issue's conditions and what a triggering setup would likely need. Fail a cannot-reproduce claim with no real attempt or artifacts, or one that does not name what differed. | required |
| env-recorded | The report's environment record (tool version and OS), read against any version the issue body itself states. | Pass if the report states the tool version and OS it was run on. If the issue body itself states the version it was filed against and the report's version differs from it, the report must say so explicitly. If the report's stated version is the same as the issue's, the shared number is sufficient on its own, no separate comparison sentence is needed. Speculation in the thread about a possibly-related dependency version does not itself trigger this requirement, only the issue's own explicit version statement does; if the issue states no version, the report's own stated version is sufficient on its own. | required |
| steps-followable | The report's steps or commands, read for concreteness and order. | Pass if the steps are concrete, ordered commands or clearly specified configuration a stranger could follow to reach the same triggering condition, even if incidental details (e.g. non-essential file contents) are paraphrased, as long as the trigger itself is stated unambiguously. Fail if a step is vague (e.g. "set up the project") with no runnable command, or the trigger condition itself is left unspecified. | required |
| conclusion-from-evidence | The report's stated conclusion, read against its own output. | Pass if the conclusion is supported by the output given, and the output does not contradict it. An honest, evidenced cannot-reproduce is a pass. | required |
| conventions-respected | The repo's stated conventions (from the repo-facts block or evidence guide, e.g. an AI-disclosure or AI-review policy), read against the candidate comment. | Treat the comment as AI-assisted work for the purposes of judging compliance. If the stated policy explicitly asks contributors to disclose AI use (naming the tool or extent of assistance), fail unless the comment contains that explicit disclosure. If the policy instead only asks that comments be written in the contributor's own words or reflect a human's understanding, without asking for a disclosure statement, pass when the comment is specific and reasoned rather than generic or templated; fail only if it reads as boilerplate or non-substantive. If no policy is stated, pass. | required |
| comment-not-boilerplate | The candidate's claim comment (and repro comment if present), read for genericness and promised outcomes. | Pass if the comment is specific to this issue (names a concrete version, behavior, or next step) and does not promise to fix the issue or deliver by a specific time. Fail if the comment reads as generic flattery or template language that could be pasted on any issue, or promises a fix or delivery date. | required |

## Verdict rule

Accept only if every required check passes. Preferred checks never change the verdict. Unclear counts as fail.
