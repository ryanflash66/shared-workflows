# Shared PR reviewer

One PR-Agent setup (Claude review, PR summary, code suggestions, security
check) that any of Ryan's repos can call. Fix it here once and every repo
gets the fix on its next run.

## Add it to a repo

1. Copy `callers/web.yml` or `callers/research.yml` to
   `.github/workflows/pr-agent.yml` in that repo. Optionally fill in `focus`
   with what matters most there.
2. Add the `ANTHROPIC_KEY` secret to that repo (personal accounts have no
   account-wide secrets):

   ```
   gh secret set ANTHROPIC_KEY -R ryanflash66/REPO
   ```

   It prompts for the key and never shows it.
3. Optional: add a `.pr_agent.toml` for file filters (`[ignore] glob`) or
   `num_max_findings`. Do not put `extra_instructions` there: the shared
   workflow's value wins. Use `focus` instead.

## Profiles

- `research`: core rules plus code-running file loads and unpinned downloads.
- `web`: core rules plus auth and ownership checks, input reaching HTML, SQL
  or fetches, secrets in browser code, Supabase RLS, payment amount checks,
  CORS and cookies, prompt injection in LLM features.
- `general`: core rules only (secrets, workflow changes, shell commands).

## Why this repo is public

Public repos can only call reusable workflows stored in public repos. Nothing
secret lives here; the key stays in each repo's secrets.
