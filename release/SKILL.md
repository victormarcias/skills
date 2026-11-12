---
name: release
description: Cut a new release of the current repo, working on dev and shipping through main. First run: promotes the CHANGELOG's Unreleased section (English and Spanish) into a dated version on dev and opens a "Release <tag>" PR into main. Run again after the PR is merged: tags main and publishes the GitHub Release. Runs only when the user invokes `/release`. The user works on `dev`, and
`main` only ever changes through a pull request, so a release takes two runs
of the same command, and `/release` works out which one this is:

- **Run 1 (prepare):** promotes the changelog on `dev`, pushes `dev` and opens
  a `main` ← `dev` PR named after the new version, with its description
  written, ready to merge.
- **Run 2 (publish), after the user merged that PR:** tags `main` and
  publishes the GitHub Release.

It never merges, pushes to `main` or switches branches.

Follows the [Keep a Changelog](https://keepachangelog.com/) format already
used in `CHANGELOG.md` / `CHANGELOG.es.md`: categories `Added`, `Changed`,
`Deprecated`, `Removed`, `Fixed`, `Security` (and their Spanish names,
already present in the file), under a running `## [Unreleased]` section
that gets promoted at release time. If a repo has no changelog yet, create
both files in this format on first use.

0. **Which run?** `git fetch origin main dev --tags`; there must be a
   `github.com` remote and `gh auth status` must succeed (stop and say so if
   not). Read the first `## [x.y.z]` heading of `CHANGELOG.md` on
   `origin/main` (`git show origin/main:CHANGELOG.md`). If that version has no
   tag yet, the release PR was merged: go to **Run 2** (step 10). Otherwise
   continue with **Run 1**.

## Run 1: prepare

1. **Look first.** The checked-out branch must be `dev` and
   `git status --short` must be clean; stop and say so if not. Then:
   - If `git log --oneline origin/main..origin/dev` is empty, there is
     nothing to release: say so and stop.
   - If `gh pr list --base main --head dev` shows an open PR, give its link
     and stop: the user merges it first.

2. **Read the changelog's `## [Unreleased]` section** in `CHANGELOG.md`
   (and the mirrored one in `CHANGELOG.es.md`). This is the content to
   release — it was written as the work happened, so don't regenerate it
   from `git log`. Only if it is empty or the files don't exist yet, build
   it from `git log <last-tag>..origin/dev --oneline`, grouped into the same
   categories, each entry a bold lead-in label plus a short sentence,
   matching the house style already in the file.

3. **Find the last tag:** `git describe --tags --abbrev=0 origin/main`.
   Match its exact style (prefix, digit count) for the new tag. If there is
   none, this is the first release: start at `0.1.0` unless the user gave
   an explicit version.

4. **Decide the version bump**, unless the user already gave one explicitly
   (`/release major`, `/release minor`, `/release patch`, or an exact
   version like `/release 2.0.0`). Base it on which categories are
   non-empty in the Unreleased section:
   - While the major version is still `0` (pre-1.0, unstable API): any
     `Added` → bump the minor version; `Fixed`/`Changed`/`Security` only →
     bump the patch version. `Removed`/breaking content does **not** force
     a major bump before `1.0.0`.
   - Once the major version is `1` or higher: `Removed` with a breaking
     change → major; `Added` → minor; everything else → patch.
   When more than one applies, use the highest bump it implies.

5. **Promote the section:** rename `## [Unreleased]` to
   `## [<tag>] - <YYYY-MM-DD>`, keep its categories and bullets as they are,
   and add a fresh, empty `## [Unreleased]` above it. Do the same, mirrored,
   in `CHANGELOG.es.md`. Make sure no other heading already uses `<tag>`.

6. **Update the compare-link footer** at the bottom of both files:
   `[Unreleased]: <repo-url>/compare/<tag>...HEAD` and
   `[<tag>]: <repo-url>/compare/<previous-tag>...<tag>` (or
   `<repo-url>/releases/tag/<tag>` when there is no previous tag).

7. **Commit the changelog files** on `dev`, one short message
   (`Updates CHANGELOG for <tag>.`), following the `commit` skill's
   conventions.

8. **Push `dev` and open the PR.** Invoking `/release` is the permission to
   push `dev` for this. Write the description to a temp file and run
   `gh pr create --base main --head dev --title "Release <tag>"
   --body-file <file>`. The description has no "Generated with Claude Code"
   line and contains:
   - `## Highlights`: a short summary, not a copy of the changelog section:
     one bullet per notable change, bold lead-in label like the changelog,
     each starting with a fitting emoji (✨ added, 🔧 changed, 🗑️ removed,
     🐛 fixed, 🔒 security), skipping minor items.
   - A closing block:

     ```markdown
     **Full changelog:** [CHANGELOG.md](<repo-url>/blob/dev/CHANGELOG.md) ·
     [Español](<repo-url>/blob/dev/CHANGELOG.es.md)

     **Compare:** <repo-url>/compare/<previous-tag>...<tag>
     ```

   - A last line: merge with "Create a merge commit" (never squash or
     rebase, which would make `dev` diverge from `main`).

9. **Report**, in the chat's language and nothing else: the new tag, the
   previous tag (or "first release"), the bump level chosen and why, and the
   PR link. Tell the user to merge it with a merge commit and run `/release`
   again.

## Run 2: publish (the release PR is merged)

10. **Check** that `git merge-base --is-ancestor origin/dev origin/main`
    passes; if not, `main` is missing commits from `dev`: say so and stop.
    Tag `main` without checking it out: `git tag -a <tag> origin/main -m
    "<tag>"` then `git push origin <tag>`, where `<tag>` is the version found
    in step 0. This is the one tag action the skill does without asking; it
    never touches branches or other tags.

11. **Publish the GitHub Release** with the same `## Highlights` summary as
    the PR description (one emoji bullet per notable change; reuse it from
    the merged PR's body, `gh pr list --base main --head dev --state merged
    --limit 1 --json body`), closing with the full-changelog and compare
    links, pointing at `main`. Write it to a temp file and run `gh release
    create <tag> --title "<tag>" --notes-file <file> --target main`.

12. **Report**: the tag and the release URL `gh` returns. `dev` is already
    inside `main`, so it needs no sync; if the user wants both identical,
    `git merge --ff-only origin/main` on `dev` does it.
