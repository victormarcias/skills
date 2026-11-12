# release

`/release` cuts a release working on `dev`. `main` only changes through a PR,
so it takes two runs. The first, from a clean `dev`:

1. promotes the changelog's `## [Unreleased]` section (Keep a Changelog
   categories, English and the mirrored `CHANGELOG.es.md`) into a dated
   version, working out the semver bump from which categories have content;
2. commits that on `dev`, pushes `dev`, and opens a "Release <tag>" PR into
   `main` whose description has a short emoji-bulleted "Highlights" summary,
   the changelog and compare links, and no "Generated with Claude Code" line.

You merge the PR with "Create a merge commit" (never squash or rebase, so
`dev` stays inside `main`) and run `/release` again: this time it tags `main`
and publishes the GitHub Release with the same Highlights. It never merges,
pushes to `main` or switches branches.

It needs a GitHub remote and `gh` signed in; pass an explicit bump or
version (`/release major`, `/release 2.0.0`) to override the automatic guess.
Instructions: [SKILL.md](SKILL.md).
