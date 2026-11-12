---
name: install-skills
description: Symlink the skills of the repo you are in (the current directory) into ~/.claude/skills, so every project on this machine loads them and edits in the repo are live. Use when the user asks to install, link or sync their skills to their user, or after a skill is added to or removed from the repo.
---

# Install skills

Links every skill from the current directory into `~/.claude/skills/`, the
personal skills directory Claude Code loads in every project. Each entry is a
symlink to the skill's folder in the repo, so a change to the repo is live
without installing again.

1. **Source is the current directory.** A skill is any folder in it that has
   a `SKILL.md`. If the current directory has none, say so and stop.
2. **Warn before replacing, once.** Make a single list of the skills that
   already exist in `~/.claude/skills/` as a real folder or as a symlink
   pointing somewhere else, and show it in one message, saying that linking
   will replace them and drop any changes made directly there. Ask one
   question for the whole list ("Replace these N skills?"), never one per
   skill, and wait for a clear yes. Skip the question when there is nothing
   to replace. If the user says no, stop and link nothing.
3. **For each skill,** create `~/.claude/skills/` first if it is missing.
   Skip a skill whose symlink already points to the right folder. Otherwise
   move the old entry aside with
   `mv ~/.claude/skills/<skill> "$TMPDIR/<skill>.old"` and run
   `ln -s "$PWD/<skill>" ~/.claude/skills/<skill>`. The link target is
   absolute, because a relative one breaks when resolved from
   `~/.claude/skills/`. Only touch the folders of the skills being linked:
   anything else in `~/.claude/skills/` stays as it is.
4. **Clean up dead links.** A symlink in `~/.claude/skills/` that points into
   the current directory but whose target no longer exists belonged to a
   removed skill: remove it with `rm`.
5. **Report** one line per skill (linked, replaced, already linked or
   removed). Say to run `/reload-skills` so the current session picks new
   skills up; new sessions load them on their own.
