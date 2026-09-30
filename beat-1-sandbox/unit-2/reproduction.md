# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

CarlosSac

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5921603359

Hello,

I'd like to take this one as a first contribution.

As I read it: the Quick Start in README.md tells a new setup to add OPENROUTER_API_KEY to .env, but .env.example doesn't define that variable, its LLM_PROVIDER comment only offers mock and openai. core/config.py declares fields for both openai_api_key and openrouter_api_key, so the two docs point a fresh setup toward different key names depending on which one is read first.

Next I want to follow the Quick Start's own copy step on a clean checkout, load core/config.py's Settings object against the result, and check whether any other setup doc points the same way as the README, before reporting back on which side needs to change. I'll post a reproduction report with my environment, steps, and what I observe.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5921817407

Reproduction result 

Result: reproduced. Following the Quick Start leaves you with a `.env` that has no `OPENROUTER_API_KEY` line, and the app's own settings object resolves that key to an empty string.

## Environment

- OS: macOS 27.0.1 (build 26A434), arm64
- git 2.54.0 (Apple Git-157)
- Python 3.9.6 (system `python3`), in a throwaway venv with `pydantic` 2.13.5 and `pydantic-settings` 2.11.0, the two packages `core/config.py` imports. Note: `pyproject.toml` declares `requires-python = ">=3.11"`, so I am below the project's supported version; both packages installed and ran fine, and this check only loads the settings class. I did not run `make setup` or `docker compose`; neither is needed to compare what the docs ask for against what the template ships.
- Code state: fresh clone of my fork `CarlosSac/pathreview-ai301-fa26-s1`, `main` at `f89c06fc3ff292df2a04a39ac51319d32a76b779` (`f89c06f`, 2026-09-16), clean working tree before and after. The issue names no version or platform, and nothing here is platform specific.

## Steps

From the repo root of a fresh clone at that commit:

```bash
# 1. What the setup docs ask for
grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md

# 2. What the template ships for the provider
sed -n '16,19p' .env.example

# 3. The Quick Start's own copy step, verbatim, then look for the key it named
cp .env.example .env
grep -c "OPENROUTER_API_KEY" .env

# 4. What the application actually resolves
python3 -m venv /tmp/prvenv
/tmp/prvenv/bin/pip install "pydantic[email]>=2.5.0" "pydantic-settings>=2.1.0"
/tmp/prvenv/bin/python3 -c "
from core.config import Settings
s = Settings()
print('llm_provider       =', repr(s.llm_provider))
print('openai_api_key     =', repr(s.openai_api_key))
print('openrouter_api_key =', repr(s.openrouter_api_key))
"

# 5. Control: add the key the README names, reload
printf '\nOPENROUTER_API_KEY=sk-or-test-0000\n' >> .env
/tmp/prvenv/bin/python3 -c "
from core.config import Settings
print('openrouter_api_key =', repr(Settings().openrouter_api_key))
"
```

## Expected

`README.md` tells the reader to add `OPENROUTER_API_KEY` to `.env`, and to create that `.env` by copying `.env.example`. Following both instructions should leave a `.env` with somewhere to put that key.

## Actual

Step 1, both setup documents name the key:

```
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
```

Step 2, the complete provider block of `.env.example` (lines 16 to 19):

```dotenv
# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here
```

Step 3, after the documented copy step:

```
0
```

The `.env` the Quick Start tells you to create has zero occurrences of the key the same Quick Start line tells you to set.

Step 4, what the app resolves from that `.env`:

```
llm_provider       = 'mock'
openai_api_key     = 'sk-your-key-here'
openrouter_api_key = ''
```

Step 5, the control run, after appending the key by hand:

```
openrouter_api_key = 'sk-or-test-0000'
```

## Why the control matters

The control rules out the reading that `OPENROUTER_API_KEY` is a stale name the app no longer supports. `core/config.py` lines 44 to 47 set the loader explicitly:

```python
    class Config:
        env_file = ".env"
        env_file_encoding = "utf-8"
        case_sensitive = False
```

`case_sensitive = False` is why the uppercase `OPENROUTER_API_KEY` maps onto the lowercase `openrouter_api_key` field declared on line 20. So the variable the README names is real and supported, and the template is the file that omits it.

## What I am not claiming

- I did not start the application or make a provider API call. This covers the documented setup path only: what the docs ask for, what the copied `.env` contains, and what `Settings` loads.
- `grep -rn "llm_provider\|openrouter_api_key" --include="*.py" .` matches only the declarations in `core/config.py`. No other module reads them yet, and `get_embedding_provider` in `ingestion/embeddings/provider.py` line 125 rejects anything outside `'mock'` and `'openai'`. So my evidence shows the docs and the template disagree and that the key resolves empty; it does not settle whether `.env.example` should gain the OpenRouter entries or the two setup docs should point at `OPENAI_API_KEY` instead. That looks like a maintainer call, and I would want direction before opening a PR.
- One machine, one OS. I would not expect a platform difference here, since nothing in the check is platform specific, but I have only run it on the environment recorded above.

## One thing beyond the issue text

The issue names `README.md` and `.env.example`. `docs/SETUP.md` line 47 points the same way as the README and calls the key required for AI features, so a fix that only touches `README.md` would leave `docs/SETUP.md` saying the same thing.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke test (`--limit 3`, pkg-01/02/03): `2/3 scored items` agreement; pkg-01 failed `env-recorded`.
2. `--only pkg-01,pkg-20` (loosened `env-recorded` to not require a comparison sentence when the issue states no version): `0/2 scored items`: pkg-01 still failed `env-recorded` for a different reason (the grader read thread speculation about a dependency version as "the issue's version"); pkg-20 flipped to a wrong `accept`.
3. `--only pkg-01,pkg-20` (narrowed `env-recorded` to compare only against a version the issue body itself states, not thread speculation; made `conventions-respected` treat every comment as AI-assisted work regardless of how natural it reads): `2/2 scored items` agreement.
4. Full run (partial, unsaved): `agreement: 16/20 scored items (bar: 18/20: below the bar)`; pkg-03, pkg-05, pkg-09, pkg-10 disagreed.
5. `--only pkg-03,pkg-05,pkg-09,pkg-10,pkg-20` (added an honest-cannot-reproduce carve-out to `failure-match-issue`; added an identical-version carve-out to `env-recorded` so matching numbers need no extra sentence): `4/5 scored items`: pkg-03/09/10/20 fixed; pkg-05 newly failed `steps-followable`.
6. `--only pkg-05,pkg-06,pkg-18,pkg-19,pkg-20` (loosened `steps-followable` to allow paraphrased incidental detail as long as the trigger is unambiguous): `4/5 scored items`: pkg-05 fixed; pkg-19 flipped to a wrong `accept` (canary catch).
7. `--only pkg-06,pkg-18,pkg-19,pkg-20` (added a new `comment-not-boilerplate` check): `4/4 scored items` agreement.
8. Full run (unsaved): `agreement: 20/20 scored items (bar: 18/20: PASS)`.
9. Full run (`--save-run eval-run.txt`, confirming/submitted): **`agreement: 20/20 scored items  (bar: 18/20: PASS)`**, matching the committed `eval-run.txt`.

**Package analysis**

`pkg-19` (source: vuejs/core#15205, category: `unfollowable-comms`, gold verdict: `reject`).

My rubric first returned `accept`. Every one of the 6 checks passed, including
`conventions-respected`: vuejs/core states no AI policy in its repo facts, and that check's
pass condition is an unconditional pass when no policy is stated, so it never looked past
that. But the candidate's actual claim comment read: "Hello sir! Great project, I love Vue
and use it every day... Kindly assign it to me, I will fix it within 2 days guaranteed...
please... keep this issue reserved for me." That is generic flattery paired with a promised
fix and a delivery timeline, exactly the "boilerplate over-promising comment" failure mode
the `unfollowable-comms` category description names, even though the attached repro itself
(playground recreation, produced-CSS excerpt, sibling-leak confirmation) was technically
solid and passed every proof-based check. My rubric read it as ready because nothing in it
judged the comment's own genericness or its promises; the gold label rejected it because a
sound repro attached to an unusable comment is still not ready to post. I closed the gap by
adding `comment-not-boilerplate` as its own required check, independent of whether a repo
states a policy at all.

**Check rationale**

`comment-not-boilerplate`, as it now reads in the `rubric.md` uploaded to
`tools/repro-check/`:

> Pass if the comment is specific to this issue (names a concrete version, behavior, or next
> step) and does not promise to fix the issue or deliver by a specific time. Fail if the
> comment reads as generic flattery or template language that could be pasted on any issue,
> or promises a fix or delivery date.

This check did not exist in my first draft. It exists because `pkg-19` (see Package analysis
above) showed that every check I had was proof-based: it judged the repro's artifacts, not
the comment's own words, so a comment could promise a fix "within 2 days guaranteed" and
read as content-free flattery while every other check still passed. `conventions-respected`
looked like the natural home for this, but it only fires when a repo states an explicit
policy, and vuejs/core states none. I rejected folding this judgment into `steps-followable`
too (I considered it, since that check already reads the comment's specificity) because
steps-followable's job is whether a stranger can re-run the reproduction, not whether the
poster over-promised; conflating the two would have made that check's pass condition harder
to apply consistently. A standalone check that runs regardless of stated policy was the fix.

**Trade-offs**

Loosening `steps-followable` to tolerate paraphrased incidental detail (to stop it from
wrongly failing pkg-05, whose report named its trigger condition exactly but paraphrased a
non-essential file's contents) is what let pkg-19 flip to a wrong `accept`. I had added
pkg-19 to that same `--only pkg-05,pkg-06,pkg-18,pkg-19,pkg-20` run specifically as a canary
from the `unfollowable-comms` category the loosened check touches, and it caught the flip
before the confirming full run rather than on it. That flip is what told me
`steps-followable` was never the right place to carry judgment about a comment's own
overpromising or genericness; it is why `comment-not-boilerplate` exists as its own check
instead of being folded into `steps-followable`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
