# Tour Map (working title)

A website that takes an artist's list of tour dates — musicians, comedians, poets, any touring
act — and plots them on a map, so you can see at a glance whether they're playing anywhere near
you, or near someone you're buying tickets for.

## Status

| Gate | State |
|---|---|
| Ideation | **Parked** 2026-11-16 — Bandsintown already solves it (see `ideation/decision.md`) |
| Discovery | Not started |
| Design | Not started |
| Build | Not started |
| Test | Not started |
| Release | Not started |

## How this project is run

Delivered through the [agentic-sdlc-toolkit](https://github.com/edspearxy/agentic-sdlc-toolkit)
plugin, enabled for this repo in `.claude/settings.json`. Gates are run in order with
`/ideation`, `/discovery`, `/design`, `/build`, `/test`, `/release`, `/retro`; run
`/toolkit-doctor` after any plugin install or update.

This repo holds only this project's artefacts (`ideation/`, `discovery/`, `design/`, `build/`,
`test/`, `release/`, `retro/`) and its own `memory/corrections/`. Rubrics, templates and agents
are read from the plugin — never copy them in here.

Every piece of development traces to a single tracker issue (one PR ↔ one issue).
