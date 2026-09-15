# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A single reveal.js slide deck for the final Software Lab presentation of the
**GPS+SLAM Tour Builder** team (Maria Elia & Nico Hellthaler), covering the
`GpsPlusSlamJs_TourBuilder` package built in the
[`location-based-webxr`](https://github.com/cs-util-com/location-based-webxr)
monorepo (checked out separately, not inside this repo). There is no build
system, package manager, or test suite here — it's static HTML.

## Files

- `index.html` — **the actual slide deck**, this is the file that matters
  (reveal.js 5.1.0, loaded from cdnjs). Open it directly in a browser, or
  serve the directory (`python3 -m http.server`) and navigate to it. No
  build step.
- `assets/` — images/screenshots used by `index.html`.
- `markdown/presentation.md` — outline/content notes for the deck in plain
  markdown, used as a drafting source, not rendered directly.
- `markdown/tour.json` — sample tour data model referenced from the deck.
- `markdown/TASK.md` — distilled version of the lab task brief, scoped to
  what's relevant for presentation content (product spec, component list,
  architecture contract, presentation requirements).
- `markdown/COMPONENTS.md` — short summary of the 10 components actually
  built in `GpsPlusSlamJs_TourBuilder`, for pulling into slide content.

## Editing the deck

`index.html` is a single self-contained file: one `<section>` per
top-level slide, nested `<section>`s for sub-slides within a topic. Content
placeholders are marked with `<div class="placeholder">...</div>` — replace
these with real screenshots/recordings/video embeds before presenting.

The required structure (from the lab organizers) is: introduction/motivation,
implementation details & learnings, results/demo, problems encountered, what
worked well, summary & questions — already scaffolded as the top-level
sections in `index.html`.

Reveal.js, theme, and highlight plugin are pulled from cdnjs at fixed version
5.1.0 — keep any edits consistent with that version's API (no local install).
