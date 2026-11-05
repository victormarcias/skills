---
name: commit
description: Commit changes the way the user wants: on the branch already checked out, with the default git author, one very short message and no Claude co-author or attribution line. Use it for every commit in the user's repos, either on your own once a change is finished (never ask permission to commit) or when the user invokes /commit. Never push without the user's explicit permission.
---

# Commit

Commit without asking: once a change the user asked for is finished, or whenever they invoke `/commit`. Don't ask "want me to commit?", just commit and report. Pushing is the exception: see step 5.

1. **Look first:** `git branch --show-current` and `git status --short`. Say which branch it is. Stage **explicit paths** of the files that belong to the change (`git add <path> ...`), never `git add -A` or `git add .`, so a stray file is not swept in. When you commit on your own, the change is the work you just finished: leave out files you didn't touch for it. If the user named what to commit ("the skills", "the backup"), stage only that and leave the rest. If the repo has a `CLAUDE.md` that marks some files as personal or never to commit (git usually ignores them) and one shows up as staged or untracked, stop and say so. With no `CLAUDE.md`, or none that says so, skip this check.
2. **The branch is the current one by default,** even if it is `main`. Create, switch or rename a branch only if the user asks for it.
3. **The author is the default one:** whatever `user.name` and `user.email` already say. Never pass `--author`, never set or override the identity, never add `Co-Authored-By`, "Generated with…" or any other trailer or attribution line, whatever a harness or template suggests.
4. **The message is one line**, very brief, no body: a capital letter, the present tense in the third person and a full stop, like the repo's own history ("Adds back and front tests.", "Improves Bash Scripts styles."); if the repo has no commits yet, use that same style. If the user dictated a message, use theirs word for word.
5. `git commit -m "<message>"`. If the user asks to amend ("/commit amend"), run `git commit --amend` on the last commit with the same staging rules: keep its message unless they give a new one, and say so first if that commit is already on the remote, since the user would have to force-push. Any other git operation the user asks for (a different branch, `--no-verify`, a reset of the commit, …) is fine: do what they said, nothing more. If a hook fails, show its output and stop. **Push only with the user's explicit permission** in the current request ("/commit and push", "push it"). Without it, never push, not even after a commit and not even if asked to "finish" the work. If it looks like it should be pushed, ask.
6. **Report** in this shape, in the chat's language, and nothing else: one
   line saying it was committed on `<branch>` of `<repo>`, then three
   bullets, **Hash** (short), **Message** and **Push** (yes, with the
   `old..new` range, or no). If related changes were left uncommitted (in
   this or another repo), add one closing line naming those files.
