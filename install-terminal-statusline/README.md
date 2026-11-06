# install-terminal-statusline

`/install-terminal-statusline` writes `~/.claude/statusline.sh` and registers it
as the `statusLine` in `~/.claude/settings.json`, so every terminal session
shows the project, the model, a context bar with a red arrow at 100K tokens and
the tokens used. It asks before overwriting a different script and keeps the
rest of the settings untouched.

The script lives inside [SKILL.md](SKILL.md), so the skill needs nothing else.
Run it again after changing the script there.
