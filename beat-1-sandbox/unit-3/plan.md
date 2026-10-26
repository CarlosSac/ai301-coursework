# Plan for #73: README and .env.example disagree about which LLM API key to set

## Diagnosis

My reproduction (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5921817407, commit `f89c06f`) shows the template is the file missing the key. It does not prove that the template is the side maintainers want changed (see Risks).

- Both setup docs name the key (step 1):
  ```
  README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
  docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
  ```
- The provider block of `.env.example` (step 2, lines 16 to 19) defines only `OPENAI_API_KEY`:
  ```
  # LLM provider
  # Options: "mock" (default, no API key needed), "openai"
  LLM_PROVIDER=mock
  OPENAI_API_KEY=sk-your-key-here
  ```
- After the Quick Start's own `cp .env.example .env`, `grep -c "OPENROUTER_API_KEY" .env` prints `0` (step 3), and `Settings()` resolves `openrouter_api_key = ''` (step 4).
- The control (step 5) rules out the reading that `OPENROUTER_API_KEY` is a stale name: after appending `OPENROUTER_API_KEY=sk-or-test-0000` by hand, `Settings().openrouter_api_key` is `'sk-or-test-0000'`. `core/config.py` sets `case_sensitive = False` and declares `openrouter_api_key` on line 20, so the variable is real and supported.

So the cause is that `.env.example`, the file the Quick Start tells a new setup to copy, never names the variable the docs tell them to set.

## Scope

In scope:
- Add one placeholder line, `OPENROUTER_API_KEY=sk-or-your-key-here`, to the `# LLM provider` block of `.env.example`, directly under `OPENAI_API_KEY`.

Not in scope:
- `README.md` and `docs/SETUP.md` (they already name the key the config supports).
- `core/config.py` (no field or default changes).
- The `LLM_PROVIDER` default and its `Options:` comment. My repro's `grep -rn "llm_provider\|openrouter_api_key" --include="*.py" .` matches only the declarations in `core/config.py`, so no code branches on `llm_provider` and listing an `openrouter` option would document behavior I cannot show.
- `OPENROUTER_BASE_URL` and `OPENROUTER_MODEL`, which have working defaults in `core/config.py`.
- Any Python code or tests.

## Files I will touch

- `.env.example` (one line added)

## Approach

1. Create the branch `fix/73-env-example-openrouter-key` from `main` on my fork.
2. Edit `.env.example` only: insert the one placeholder line under `OPENAI_API_KEY=sk-your-key-here`.
3. Check `git diff --stat` shows one file, one insertion, no deletions.

## Test plan

Re-run my Unit 2 repro steps 1 to 5 against the change, from the repo root of the branch, with the same throwaway venv.

- Step 1 (`grep -n "OPENROUTER_API_KEY" README.md docs/SETUP.md`): unchanged, both docs still name the key.
- Step 2 (`sed -n '16,20p' .env.example`): before, the block ends at `OPENAI_API_KEY=sk-your-key-here`. After, a new line `OPENROUTER_API_KEY=sk-or-your-key-here` follows it.
- Step 3 (`cp .env.example .env` then `grep -c "OPENROUTER_API_KEY" .env`): before `0`, expected after `1`.
- Step 4 (`Settings()` load): before `openrouter_api_key = ''`, expected after `openrouter_api_key = 'sk-or-your-key-here'`. `llm_provider` stays `'mock'` and `openai_api_key` stays `'sk-your-key-here'`.
- Step 5 (control, append `OPENROUTER_API_KEY=sk-or-test-0000`, reload): expected to still print `'sk-or-test-0000'`, because the later line should win over the template's placeholder. I will confirm this rather than assume it.

I will save the commands and output from before and after.

## Risks and unknowns

- Which side should change is a maintainer call, and no maintainer has commented on #73. My repro cannot settle it. Adding the key to the template is the smaller change and matches what both docs and `core/config.py` already say.
- Other open PRs on #73 (this list is not complete): by title, #181, #185, #221, #223 and #230 add the key to `.env.example`, #77 also edits the `LLM_PROVIDER` comment and the README, and #215 aligns the docs instead. If maintainers prefer a broader or opposite direction, my change is a subset of or an alternative to those, and I would follow their direction.
- The placeholder is a non-empty string, so a setup that never edits it gets `openrouter_api_key = 'sk-or-your-key-here'` instead of `''`. `OPENAI_API_KEY=sk-your-key-here` already follows the same pattern, and I did not check whether any code path treats a non-empty key differently, since my repro found no module that reads these settings.
- I only ran this on one machine (macOS, system Python 3.9, below the `>=3.11` the project declares).

## Deviations

Nothing changed in substance; the plan held. The build touched only `.env.example`, added the one `OPENROUTER_API_KEY=sk-or-your-key-here` line under `OPENAI_API_KEY` on branch `fix/73-env-example-openrouter-key`, and `git diff --stat` showed one file with one insertion and no deletions.

Two small things differ from what I wrote, and neither changes the scope or the approach:
- Step 5 of the test plan was an expectation that the later line in `.env` would win over the template's placeholder. I ran it and confirmed it: the control still loaded `'sk-or-test-0000'`.
- In step 2 I widened the `sed` range from my repro's `16,19p` to `16,20p` so the new line is visible, which is why the before and after output for that step has one extra line.
- For the PR's test evidence I re-ran steps 1 to 5 on both `main` and the branch in the project's own Python 3.12 `.venv` (built with `pip install -e ".[dev]"`) instead of the throwaway Python 3.9 venv from my repro, so the repo's checks (`make test-unit`, `make lint`, `make typecheck`) could run in the same environment. The results match the 3.9 run.
