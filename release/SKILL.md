---
name: release
description: Cut a new tagged release of the current repo from main. Promotes the CHANGELOG's Unreleased section (English and Spanish) into a dated version, works out the semver bump, tags and pushes main, and publishes a GitHub Release with a short emoji-bulleted Highlights summary via `gh`. Runs only when the user invokes /release.
---

# Release

Runs only when the user invokes `/release`. Cuts a release on `main`, even
though the changes being released live on `dev` — `main` only ever moves by
fast-forward merge from `dev`, so `/release` never switches branches or
merges; it just checks the two are in sync.

Follows the [Keep a Changelog](https://keepachangelog.com/) format already
used in `CHANGELOG.md` / `CHANGELOG.es.md`: categories `Added`, `Changed`,
`Deprecated`, `Removed`, `Fixed`, `Security` (and their Spanish names,
already present in the file), under a running `## [Unreleased]` section
that gets promoted at release time. If a repo has no changelog yet, create
both files in this format on first use.

1. **Look first.** `git fetch origin main dev`. Confirm `main` already has
   every commit from `dev`: `git merge-base --is-ancestor origin/dev
   origin/main`. If it fails, stop and tell the user to merge `dev` into
   `main` first — `/release` never does that merge itself. Also confirm
   `git status --short` is clean on the branch checked out, there is a
   `github.com` remote, and `gh auth status` succeeds; stop and say so if
   any check fails.

2. **Read the changelog's `## [Unreleased]` section** in `CHANGELOG.md`
   (and the mirrored one in `CHANGELOG.es.md`). This is the content to
   release — it was written as the work happened, so don't regenerate it
   from `git log`. Only if it is empty or the files don't exist yet, build
   it from `git log <last-tag>..origin/dev --oneline` against `dev`,
   grouped into the same categories, each entry a bold lead-in label plus a
   short sentence, matching the house style already in the file.

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
   in `CHANGELOG.es.md`.

6. **Update the compare-link footer** at the bottom of both files:
   `[Unreleased]: <repo-url>/compare/<tag>...HEAD` and
   `[<tag>]: <repo-url>/compare/<previous-tag>...<tag>` (or
   `<repo-url>/releases/tag/<tag>` when there is no previous tag).

7. **Commit the changelog files** on the branch already checked out, one
   short message (`Updates CHANGELOG for <tag>.`), following the `commit`
   skill's conventions. This is an ordinary commit: it still needs the
   user's explicit permission to push.

8. **Tag and push, on `main`, without checking it out:**
   `git tag -a <tag> origin/main -m "<title>"`, then
   `git push origin <tag>`. This is the one tag action this skill does
   without asking each time it runs — the global Git rule's standing
   exception for `/release`; it still never touches branches or other tags.

9. **Publish the GitHub Release** with a short, separate summary — not a
   copy of the changelog section — as `## Highlights`: one bullet per
   notable change, bold lead-in label like the changelog, each starting
   with a fitting emoji (✨ added, 🔧 changed, 🗑️ removed, 🐛 fixed, 🔒
   security), skipping minor items. Close with:

   ```markdown
   **Full changelog:** [CHANGELOG.md](<repo-url>/blob/main/CHANGELOG.md) ·
   [Español](<repo-url>/blob/main/CHANGELOG.es.md)

   **Compare:** <repo-url>/compare/<previous-tag>...<tag>
   ```

   Write it to a temp file and run `gh release create <tag> --title "<tag>"
   --notes-file <file> --target main`.

10. **Report**, in the chat's language and nothing else: the new tag, the
    previous tag (or "first release"), the bump level chosen and why, and
    the release URL `gh` returns. Remind the user the changelog commit is
    still local if they haven't said to push.
