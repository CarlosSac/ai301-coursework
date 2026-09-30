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

## Things I never post

- A delivery date or a promise to fix the issue.
- "Same as above, can confirm" on someone else's reproduction.
- A reproduction claim not backed by output I actually ran myself.
