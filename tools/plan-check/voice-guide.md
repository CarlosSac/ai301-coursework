# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a newcomer to open-source contribution working through this issue
as coursework, with a general computer science background and comfort
in Python. Readers should expect careful, specific claims about what
I've checked, not deep familiarity with this codebase.

## Rules I write by

### Rule: No promised fix or date

I say what I'll investigate next, never when I'll be done or that I'll
fix it.

- Wrong: "I'll have a PR up by tomorrow with the fix."
- Right: "Next I want to check the header-merge path against the
  multidict versions mentioned above and report back."

### Rule: Name the specifics, not the vibe

Every line names a concrete version, command, or behavior, not "this
issue" or "the bug."

- Wrong: "I want to work on this issue."
- Right: "I can reproduce the missing Content-Type header on 3.2.4
  with exactly one custom header present."

### Rule: Sound like a person on this issue, not a template

No opener that could be pasted onto any issue anywhere.

- Wrong: "Hi! I'm interested in contributing to this project. Can I
  take this?"
- Right: "Hi, I'm working through this as a first contribution. I can
  reproduce X, and next I want to check Y."

### Rule: Admit it's a first contribution, plainly

I state my experience level rather than letting silence imply more
than I have.

- Wrong: (leaving it out, letting silence imply expertise)
- Right: "I'm working through this as a first contribution."

### Rule: State an uncertain approach as a plan with its open question named

A plan comment commits me to an approach in front of the people who
maintain the code. When I am not sure, I say what I will do and what
I have not verified, not a settled-sounding claim.

- Wrong: "The fix is to add the key to .env.example, and that is the right direction."
- Right: "I plan to add the key to .env.example. Which side should change is still a maintainer call, and I have not checked it against anything beyond the docs."

### Rule: Name other directions already on the thread

If a maintainer, another plan, or an open PR already points somewhere,
I name it and say how mine relates, instead of writing as if mine is
the only plan.

- Wrong: "Here is my plan." (with an open PR on the same fix, unmentioned)
- Right: "PR #77 takes a broader approach to the same mismatch. Mine is the one-line version, and I would follow whichever direction maintainers prefer."

## Things I never post

- A delivery date or a promise to fix the issue.
- "Same as above, can confirm" on someone else's reproduction.
- A reproduction claim not backed by output I actually ran myself.
- "Same approach as above" in place of my own plan.
- A timeline for the pull request ("this week", "shortly").
