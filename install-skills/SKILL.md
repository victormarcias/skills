---
name: install-skills
description: Copy the skills of the repo you are in (the current directory) into ~/.claude/skills, so every project on this machine loads them. Use when the user asks to install, copy or sync their skills to their user, or after a skill is added to or changed in the repo.
---

# Install skills

Copies every skill from the current directory into `~/.claude/skills/`, the personal skills directory Claude Code loads in every project. Nothing else: no links, no absolute paths.

1. **Source is the current directory.** A skill is any folder in it that has a `SKILL.md`. If the current directory has none, say so and stop.
2. **For each skill,** replace the old copy with a fresh one, using relative paths from the current directory: `rm -rf ~/.claude/skills/<skill>` then `cp -R <skill> ~/.claude/skills/<skill>`. Create `~/.claude/skills/` first if it is missing. Only touch the folders of the skills being copied: anything else in `~/.claude/skills/` stays as it is.
3. **Report** one line per skill (copied or replaced). Say to run `/reload-skills` so the current session picks them up; new sessions load them on their own.
