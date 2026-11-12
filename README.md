# skills

Victor's own [Claude Code skills](https://code.claude.com/docs/en/skills), kept in one repo instead of one copy per project.

## Skills

| Skill | What it does |
| --- | --- |
| [commit](commit/) | `/commit`: commits the current changes on the checked-out branch with one short message and no co-author line; pushes only if you say so. |
| [install-skills](install-skills/) | `/install-skills`: copies every skill of the folder you are in into `~/.claude/skills/` so they load in any project. |
| [install-terminal-statusline](install-terminal-statusline/) | `/install-terminal-statusline`: writes the status line script and registers it in `~/.claude/settings.json`, so the terminal shows project, model and tokens. |
| [release](release/) | `/release`: promotes the changelog's Unreleased section into a version, tags `main`, and publishes a GitHub Release with a short Highlights summary. |

## Layout

One folder per skill, named after the skill:

```
<skill-name>/
├── SKILL.md    # the instructions Claude Code loads
└── README.md   # a short summary for people skimming the repo
```

Every skill has both files. The README summarizes; the instructions live only in `SKILL.md`. Everything here is written in English.

## How Claude Code picks a skill up

Claude Code loads a skill from `<skills dir>/<skill-name>/SKILL.md`. The personal skills directory, `~/.claude/skills/`, is loaded in every project on this machine. So each skill here is exposed by copying it into that directory, from the root of this repo:

```bash
cp -R <skill-name> ~/.claude/skills/<skill-name>
```

A copy is not live: copy again after changing a skill. A skill added after Claude Code started may need `/reload-skills` the first time.

## Adding a skill

1. Create `<skill-name>/SKILL.md` and `<skill-name>/README.md`.
2. Copy it into `~/.claude/skills/` as above, or run `/install-skills`.
3. Add a row to the table in this README.
