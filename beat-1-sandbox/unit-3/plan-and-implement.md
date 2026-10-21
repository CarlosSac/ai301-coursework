# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

CarlosSac

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-6327887340

Plan for #73, built on my reproduction above (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5921817407) at `f89c06f`. I'm working through this as a first contribution.

**Diagnosis.** In my repro, `README.md:24` and `docs/SETUP.md:47` both tell you to set `OPENROUTER_API_KEY`, but after the Quick Start's own `cp .env.example .env`, `grep -c "OPENROUTER_API_KEY" .env` prints `0` and `Settings().openrouter_api_key` is `''`. My control run shows the name is not stale: appending `OPENROUTER_API_KEY=sk-or-test-0000` by hand makes `Settings()` load `'sk-or-test-0000'`, because `core/config.py` declares the field and sets `case_sensitive = False`. So the evidence points at `.env.example` as the file missing the variable, though it doesn't prove that is the side maintainers want changed.

**Scope.**
- In: add one `OPENROUTER_API_KEY=sk-or-your-key-here` line to the `# LLM provider` block of `.env.example`, under `OPENAI_API_KEY`.
- Not in: `README.md`, `docs/SETUP.md`, `core/config.py`, the `LLM_PROVIDER` default and its `Options:` comment, `OPENROUTER_BASE_URL` / `OPENROUTER_MODEL`, and any Python code. I'm leaving the `Options:` comment alone because a search of the Python files for `llm_provider` and `openrouter_api_key` matches only their declarations in `core/config.py`, so I can't show that an `openrouter` option does anything.

**Approach.** On my fork, branch `fix/73-env-example-openrouter-key` from `main`, edit only `.env.example`, and check the diff is one file with one added line.

**Test.** I'll re-run my repro steps before and after. Before: step 3's count is `0` and step 4 shows `openrouter_api_key = ''`. Expected after: the count is `1`, step 4 shows `openrouter_api_key = 'sk-or-your-key-here'`, and the step 5 control still loads `'sk-or-test-0000'`. The README, SETUP.md and `core/config.py` should be unchanged.

**Unknowns.** Which side should change is still a maintainer call, and I haven't seen a maintainer weigh in. Most of the other open PRs on this issue also add the key to `.env.example`. #77 goes further and edits the `LLM_PROVIDER` comment and the README, and #215 goes the other way and aligns the docs instead. Mine is the smallest version of the template-side change, and I'd follow whichever direction maintainers prefer. I only ran this on one machine (macOS, Python 3.9, below the project's `>=3.11`), and the placeholder resolves to a non-empty value, the same way `OPENAI_API_KEY=sk-your-key-here` already does.

AI disclosure: I used Claude Code to help draft this plan. I ran the reproduction steps myself and will review every diff before keeping it.

---

## Your branch

**Branch**

fix/73-env-example-openrouter-key

**Evidence**

Same five repro steps from my Unit 2 reproduction comment, run from the repo root with the same throwaway venv (Python 3.9.6, pydantic 2.13.5, pydantic-settings 2.11.0). Before is `main` at `f89c06f`; after is commit `45ba063` on `fix/73-env-example-openrouter-key`. The script ran each step with the commands below and deleted the generated `.env` afterwards.

Diff of the change (`git diff --stat`: `.env.example | 1 +`, 1 file changed, 1 insertion(+)):

```diff
--- a/.env.example
+++ b/.env.example
@@ -17,6 +17,7 @@ VECTOR_DB_URL=http://localhost:8001
 # Options: "mock" (default, no API key needed), "openai"
 LLM_PROVIDER=mock
 OPENAI_API_KEY=sk-your-key-here
+OPENROUTER_API_KEY=sk-or-your-key-here
 
 # App settings
```

Before (branch `main`, `f89c06f`):

```
$ grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)

$ sed -n '16,20p' .env.example
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here

$ cp .env.example .env
$ grep -c "OPENROUTER_API_KEY" .env
0

$ python3 -c "from core.config import Settings; s = Settings(); print('llm_provider       =', repr(s.llm_provider)); print('openai_api_key     =', repr(s.openai_api_key)); print('openrouter_api_key =', repr(s.openrouter_api_key))"
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = ''

$ printf '\nOPENROUTER_API_KEY=sk-or-test-0000\n' >> .env
$ python3 -c "from core.config import Settings; print('openrouter_api_key =', repr(Settings().openrouter_api_key))"
openrouter_api_key = 'sk-or-test-0000'
```

After (branch `fix/73-env-example-openrouter-key`):

```
$ grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)

$ sed -n '16,20p' .env.example
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
OPENROUTER_API_KEY=sk-or-your-key-here

$ cp .env.example .env
$ grep -c "OPENROUTER_API_KEY" .env
1

$ python3 -c "from core.config import Settings; s = Settings(); print('llm_provider       =', repr(s.llm_provider)); print('openai_api_key     =', repr(s.openai_api_key)); print('openrouter_api_key =', repr(s.openrouter_api_key))"
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = 'sk-or-your-key-here'

$ printf '\nOPENROUTER_API_KEY=sk-or-test-0000\n' >> .env
$ python3 -c "from core.config import Settings; print('openrouter_api_key =', repr(Settings().openrouter_api_key))"
openrouter_api_key = 'sk-or-test-0000'
```

The only differences between the two runs are step 2 (one extra line), step 3 (`0` becomes `1`) and step 4 (`openrouter_api_key` goes from `''` to `'sk-or-your-key-here'`). Steps 1 and 5 are identical.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run (`--limit 3`, pkg-01/02/03): `agreement: 3/3 scored items`.
2. Full run (`--save-run eval-run.txt`): `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. This is the run saved in `eval-run.txt`, and nothing in the rubric, evidence guide, procedure or SKILL.md changed after it (the fingerprints in the file header match the uploaded files).
3. After the full run, one partial re-grade of pkg-20 alone (`--only pkg-20`, to read its per-check results for the Package analysis below): `agreement: 1/1 scored items`. Partial runs do not write `eval-run.txt`, so the file still holds run 2.

The score of the run in `eval-run.txt` is **20/20**.

**Package analysis**

`pkg-20` (source: ghostty-org/ghostty#11261, category: `thread-convention`, gold verdict: `reject`).

My rubric returned `reject`, matching the gold label. Six of the seven checks passed, including the ones a strong plan usually wins on: `cause-grounded` ("Cause (prev stale after hyperlink-driven page realloc) matches repro; control with hyperlink start removed passes, isolating growth-during-print."), `scope-bounded`, `executable`, `test-decisive`, and `thread-direction-engaged` ("Comment: 'My plan follows the direction proposed here' (recompute prev only when capacity changed) and lands both test cases."). The one failure was `ai-policy-honored`: "AI_POLICY requires disclosing tool and extent of AI use; the plan comment contains no AI disclosure."

That is the whole story of this package. The plan is bounded, grounded and follows what the maintainer proposed in the thread, and the comment even says so. What is missing is a sentence the repo's stated policy requires: ghostty's policy line in the repo facts reads "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance". My rubric treats every plan comment as AI-assisted work, so a comment with no disclosure fails when a policy requires one, however good the plan is. A reader who grades only the plan sees an accept; the gold label rejects because the plan cannot be posted as written.

**Check rationale**

`ai-policy-honored`, quoted exactly as it reads in the `rubric.md` uploaded to `tools/plan-check/`:

| ai-policy-honored | The repo-facts block's contribution policy, read against the plan comment, treating the comment as AI-assisted work. | If the policy requires disclosing AI use (tool or extent), fail unless the comment contains that disclosure. If the policy only asks for comments in the contributor's own words, pass when the comment is specific and reasoned rather than generic. If no policy is stated, pass. | required |

I did not revise this check across runs, because the first full run already agreed on all 20 and it never flipped a package. It reads that way because I carried it over from the Unit 2 `conventions-respected` check, which I had already tightened so that every comment is treated as AI-assisted work no matter how natural it sounds. That clause is the part that catches pkg-20. I kept the three branches (disclosure required, own-words only, no policy) separate on purpose: pkg-03's repo only asks for comments in the contributor's own words and its comment passes on being specific, while pkg-20's repo requires a disclosure and its comment fails without one.

What I rejected instead was a separate "no promised timeline" check for plan comments, which Unit 2's `comment-not-boilerplate` suggested. calib-01's gold label is `accept` even though its comment says "Will send the PR shortly", so such a check would reject a package the gold labels accept. Promises stay a voice-guide matter in live mode and stay out of the rubric.

**Trade-offs**

This check gives up two things. First, when a repo states no AI policy it passes unconditionally, so it never judges the comment's own quality there. A vague or flattering plan comment in a repo with no policy is not caught by this check, and I rely on `thread-direction-engaged` and the plan checks to catch weak plans, which means a plan with a sound body and a generic comment can pass. My own plan comment on #73 is in that position: Path Review's `docs/CONTRIBUTING.md` states no AI policy, so the check passed it by default, and I disclosed Claude Code use anyway.

Second, it fails a well-written plan on a single missing sentence. pkg-20 is exactly that case: the other six checks passed and this one decided the verdict. I accept that, because the gold label agrees and a maintainer under a strict policy would send the comment back.

Nothing else changed because of it: I added no canary run, since the first full run agreed on 20/20 and no check was loosened afterwards. The only check I considered and left out, the promised-timeline check above, would have cost calib-01.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
