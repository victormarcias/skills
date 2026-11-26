---
name: install-pr-agent
description: Add the PR Agent GitHub workflow (Qodo PR-Agent reviewing every pull request with DeepSeek) to the repo in the current directory, and check that its DEEPSEEK_API_KEY secret is set. Use when the user runs /install-pr-agent or asks to add the PR agent or automatic PR reviews to a repo.
model: haiku
---

# Install PR agent

Copies `pr_agent.yml`, next to this file, into the current repo. The
workflow reviews each pull request with PR-Agent and DeepSeek, and answers
PR comments addressed to it.

1. **Check the repo.** Run from the repo's root. Stop if it is not a git
   repo, has no GitHub remote, or is a fork (`gh repo view --json isFork`).
2. **Copy the workflow** to `.github/workflows/pr_agent.yml`. If that file
   exists and is identical, change nothing. If it differs, show a short
   diff and ask once before overwriting.
3. **Check the secret.** Run `gh secret list` and look for
   `DEEPSEEK_API_KEY`. Never read, ask for or handle the key itself. If it
   is missing, tell the user to run `! gh secret set DEEPSEEK_API_KEY`,
   which prompts for the value. Personal accounts have no shared secrets,
   so every repo needs its own.
4. **Leave it uncommitted.** The workflow starts running once it is pushed
   and a pull request is opened or updated.

Report in one line whether the workflow was added, updated or already in
place, and whether the secret is set.
