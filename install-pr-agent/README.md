# install-pr-agent

`/install-pr-agent` adds the PR Agent workflow to the repo you are in, so
every pull request gets an automatic review from PR-Agent running on
DeepSeek. It copies [pr_agent.yml](pr_agent.yml) into
`.github/workflows/` and checks that the repo has a `DEEPSEEK_API_KEY`
secret, which the user sets with `gh secret set`. Instructions:
[SKILL.md](SKILL.md).

The action is pinned to a PR-Agent release commit, and only the repo's
owner can run it from a PR comment.
