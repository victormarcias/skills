---
name: install-skills
description: Copy the skills of the repo you are in (the current directory) into ~/.claude/skills, so every project on this machine loads them. Use when the user asks to install, copy or sync their skills to their user, or after a skill is added to or changed in the repo.
---

# Install skills

Copies every skill from the current directory into `~/.claude/skills/`, the personal skills directory Claude Code loads in every project. Nothing else: no links, no absolute paths.

1. **Source is the current directory.** A skill is any folder in it that has a `SKILL.md`. If the current directory has none, say so and stop.
2. **Warn before overwriting, once.** Make a single list of the skills that already exist in `~/.claude/skills/` and show it in one message, saying that copying will overwrite them and replace any changes made directly there. Ask one question for the whole list ("Overwrite these N skills?"), never one per skill, and wait for a clear yes. If none exist yet, there is nothing to overwrite: skip the question. If the user says no, stop and copy nothing.
3. **For each skill,** replace the old copy with a fresh one, using relative paths from the current directory: `rm -rf ~/.claude/skills/<skill>` then `cp -R <skill> ~/.claude/skills/<skill>`. Create `~/.claude/skills/` first if it is missing. Only touch the folders of the skills being copied: anything else in `~/.claude/skills/` stays as it is.
4. **Report** one line per skill (copied or replaced). Say to run `/reload-skills` so the current session picks them up; new sessions load them on their own.
