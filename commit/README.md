# commit

`/commit` commits the current changes on the branch you already have checked out, with one short message under your own git author. It stages only the files that belong to the change, adds no `Co-Authored-By` or attribution line, and stops if a file marked as personal in the repo's `AGENTS.md` (or `CLAUDE.md`) shows up. Besides a plain commit it can do whatever git operation you ask for in the request, such as `/commit amend`.

It runs only when you invoke it, so you read the diff first, and it never pushes without your explicit permission (`/commit and push`). It only touches the local git repository, plus the remote when you ask it to push. Instructions: [SKILL.md](SKILL.md).
