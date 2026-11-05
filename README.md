# skills

Victor's own [Claude Code skills](https://code.claude.com/docs/en/skills), kept in one repo instead of one copy per project.

## Skills

| Skill | What it does |
| --- | --- |
| [commit](commit/) | Commits finished changes without asking (or on `/commit`) on the checked-out branch, with one short message and no co-author line; pushes only if you say so. |

## Layout

One folder per skill, named after the skill:

```
<skill-name>/
├── SKILL.md    # the instructions Claude Code loads
└── README.md   # a short summary for people skimming the repo
```

Every skill has both files. The README summarizes; the instructions live only in `SKILL.md`. Everything here is written in English.

## How Claude Code picks a skill up

Claude Code loads a skill from `<skills dir>/<skill-name>/SKILL.md`. The personal skills directory, `~/.claude/skills/`, is loaded in every project on this machine, and an entry there can be a symlink to a folder elsewhere on disk. So each skill here is exposed by linking it into that directory:

```bash
ln -s ~/Documents/GitHub/skills/<skill-name> ~/.claude/skills/<skill-name>
```

Edits to a linked skill are picked up in the current session without a restart. A skill added after Claude Code started may need `/reload-skills` the first time.

## Adding a skill

1. Create `<skill-name>/SKILL.md` and `<skill-name>/README.md`.
2. Link it into `~/.claude/skills/` as above.
3. Add a row to the table in this README.
