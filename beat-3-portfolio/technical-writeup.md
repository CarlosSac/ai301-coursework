# My First Contribution to DrumBeatRepo: Fixing Warnings and Cleaning Up Config

## Overview

DrumBeatRepo is an open-source, browser-based drum machine and rhythm library: anyone can open it in a web browser to explore, customize, and play back drum patterns across different musical genres without installing any software. In issue #627, the codebase was generating lint warnings and carrying leftover configuration settings that no longer served a purpose. They didn't break the app for end users, but they created noise in the lint check and left dead config lines that made maintenance harder. I tracked down the source of the three ESLint warnings and updated the code so the lint runs cleanly, then identified and deleted nine config lines that had no effect on which lint rules actually ran. After feedback during code review, I added a regression test, drafted with Claude Code, that I reviewed and confirmed fails without the fix. The pull request ([#633](https://github.com/Babali42/DrumBeatRepo/pull/633)) was merged into the main branch on November 8, closing issue #627, and right after the merge the maintainer added me to the repository's contributor list for my code contribution.

## What happened

I expected to see more issues and have a challenging task to complete. I knew some warnings might be easier to remove and some might need adjustments, like the readonly fix.

The issue mentioned about 75 warnings, so I expected a large cleanup, but running the linter locally only showed 3. I asked about the discrepancy on the issue thread, and the owner explained where the 75 figure had come from and clarified the exact scope for the fix.

```
src/app/infrastructure/adapters/audio-engine/audio-engine.adapter.ts
  218:36  warning  Only readonly types allowed  functional/prefer-readonly-type
src/app/ui/services/sequencer/sequencer.service.ts
  63:7  warning  Unused eslint-disable directive (...)
  65:7  warning  Unused eslint-disable directive (...)
✖ 3 problems (0 errors, 3 warnings)
```

The linter flagged `clearTrackEvents()` because its parameter wasn't marked readonly, but changing the type directly broke compilation since the method called `events.clear()`. To fix this, I moved `events.clear()` out of `clearTrackEvents()` and into `solo()`, allowing `clearTrackEvents()` to safely accept a readonly parameter. Because `clearTrackEvents()` only had two callers (`solo()` and `mute()`), this change was safe: `solo()` kept its exact behavior, while `mute()` already deletes the entire map from `trackStepMap` on the very next line anyway.

```diff
-  private clearTrackEvents(events: Map<number, WAAClock.Event>): void {
+  private clearTrackEvents(events: ReadonlyMap<number, WAAClock.Event>): void {
     for (const [, event] of events) {
       event.clear();
     }
-
-    events.clear();
   }
 ...
         this.clearTrackEvents(events);
+        events.clear();
```

Those 9 `'off'` lines were disabling functional ESLint rules that were not active in the resolved ruleset anyway, making them complete no-ops. To be certain removing them wouldn't quietly change how the linter behaved, I didn't just delete them and hope for the best. Before making any edits, I ran ESLint's `--print-config` on three representative file types: a service, a component, and a spec file. After removing the 9 lines, I ran `--print-config` again and diffed the outputs. The only differences were the removed entries themselves (all set to 0), proving the effective configuration was identical:

| File | Removed entries (all were 0) | Added | Changed |
|---|---|---|---|
| sequencer.service.ts | 5 functional rules | none | none |
| app.component.ts | 6 functional rules | none | none |
| sequencer.service.spec.ts | 5 functional rules | none | none |

I intentionally left non-functional rules like `prefer-readonly-parameter-types` and the test block's `no-unused-vars` untouched, then confirmed the linter reported zero problems and that all test suites passed.

With the linter reporting zero problems and all test suites passing (91 in Karma, 29 in Vitest, and 37 in Scala), I opened PR #633.

> "Add a regression test for solo map cleanup. The mute test does not exercise solo(). [...] Without this assertion, removing events.clear() can leave stale entries without failing the adapter tests." (CodeRabbit, 2026-11-08)

CodeRabbit pointed out that two items needed tests, but my change only touched one of the parts. So I went back to see how the existing tests were written, to use them as templates, and asked Claude to draft a test. Then I reviewed it, made sure it was actually testing what it was supposed to, and checked that it failed without the change:

```ts
adapter.syncTracks([{ ...kick, isSolo: true }, snare]);

expect(adapter['trackStepMap'].get('Snare')?.size).toBe(0);
expect(adapter['trackStepMap'].get('Kick')?.size).toBeGreaterThan(0);
```

```
AudioEngineAdapter should empty the other tracks' scheduled events when a track is soloed FAILED
    Expected 2 to be 0.
TOTAL: 1 FAILED, 91 SUCCESS
```

The owner merged the change. Afterward, I pulled the latest changes and ran the lint and tests on `main` locally to double-check everything: the lint reported zero problems, and the new test passed alongside the existing suite (Karma 92/92).

> "I'll complète the PR as is, it's nice to have one more test too." (Babali42, 2026-11-08)

## Limitations and what's next

There are a couple of clear limitations to this change: moving `events.clear()` into `solo()` was a small tweak to satisfy the lint rule rather than a deeper cleanup of the audio engine, and I only added the unit test after code review, leaving out CodeRabbit's extra suggestion to add a `mute()` assertion as well. For a next step, I could follow up with a small PR to add that `mute()` test assertion or check back with the owner on whether those two untouched no-op rules should eventually be re-enabled.
