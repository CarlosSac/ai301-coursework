# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: in an eval bundle, the "Environment" line at the top of
the Candidate repro report. Any version the original reporter used, for
comparison, comes from the Issue section or Thread highlights. In live
mode, the student's own draft report, compared against the version
information stated in the GitHub issue thread.

What good looks like: the report names the exact tool version and
OS/platform it ran on, and states explicitly whether that version
matches the one the issue was filed against or differs from it (a bare
version number with no comparison is not enough when the issue names
its own version).

## Steps

Where it lives: in an eval bundle, the numbered steps or command lines
in the Candidate repro report. In live mode, the same section of the
student's draft report.

What good looks like: every step is a literal, ordered command or
action, with the starting state named (flags, config file contents,
offline vs online). A stranger with the same tool installed could paste
the commands as written and land on the same point. A step like "set up
the project" with no runnable command fails; a step that assumes
unstated config does too.

## Behavior shown

Where it lives: in an eval bundle, the "Actual" (or equivalent) output
excerpt in the Candidate repro report, read against the Current
result / described failure in the Issue section. In live mode, the
draft report's output excerpt against the GitHub issue body's
described behavior.

What good looks like: the excerpt shows the same kind of failure the
issue names (same error type, same missing or wrong output), not a
different error reached by a different path. A control run (the same
steps with the triggering variable removed, showing the contrasting
correct output) is the strongest form of this evidence, matching-issue
and failure-match-issue both draw on this same excerpt.

## Honesty

Where it lives: in an eval bundle, the report's own stated Expected
and Actual conclusion, read against nothing but its own output block.
In live mode, the draft report's concluding sentence against its own
pasted output.

What good looks like: the conclusion claims exactly what the output
shows, no more. An honest "could not reproduce, here is what I observed
instead" backed by real output passes. A conclusion that asserts
reproduction while the output does not show it, or that states more
confidence than the artifact supports, fails.

## Comms

Where it lives: in an eval bundle, the Candidate claim comment and
Candidate repro report, read against the "contribution policy" line
in the Repo facts block. In live mode, the drafted comment against the
target repo's CONTRIBUTING.md, any dedicated AI-policy file, and issue
or PR templates on GitHub (the same surface named in the issue-select
evidence guide's fifth family).

What good looks like: when a stated policy requires disclosing AI
assistance, the comment discloses it, naming the tool and the extent
of the assistance. When no policy is stated, silence passes. A comment
that reads as boilerplate ("same as above, can confirm", a templated
+1) fails regardless of policy, because it is not evidence produced in
the poster's own words.
