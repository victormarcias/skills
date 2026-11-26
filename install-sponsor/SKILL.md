---
name: install-sponsor
description: Add the GitHub sponsor button (.github/FUNDING.yml with GitHub Sponsors and Cafecito) to the repo in the current directory. Use when the user runs /install-sponsor or asks to add the sponsor or funding button to a repo.
model: haiku
---

# Install sponsor

Copies `FUNDING.yml`, next to this file, into the current repo. GitHub reads
it to show the Sponsor button with GitHub Sponsors and Cafecito.

1. **Check the repo.** Run from the repo's root. Stop if it is not a git
   repo, has no GitHub remote, or is a fork (`gh repo view --json isFork`).
2. **Copy the file** to `.github/FUNDING.yml`. If that file exists and is
   identical, change nothing. If it differs, show a short diff and ask once
   before overwriting.
3. **Leave it uncommitted.** The button shows up once it is pushed to the
   default branch.

Report in one line whether the file was added, updated or already in place.
