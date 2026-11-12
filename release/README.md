# release

`/release` cuts a new tagged release on `main`: it promotes the changelog's
`## [Unreleased]` section (Keep a Changelog categories, English and the
mirrored `CHANGELOG.es.md`) into a dated version, works out the semver bump
from which categories have content, tags and pushes that tag on `main`
(without checking it out), and publishes a GitHub Release with a short,
separate "Highlights" summary via `gh`. It needs `main` already in sync
with `dev`, a clean working tree, a GitHub remote, and `gh` signed in; pass
an explicit bump or version (`/release major`, `/release 2.0.0`) to
override the automatic guess.

It's the one git tag action Claude is allowed to create and push on its
own, everywhere this skill is installed — it never merges `dev` into `main`
or switches branches itself; every other tag, branch, or worktree action
stays with you. Instructions: [SKILL.md](SKILL.md).
