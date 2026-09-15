# Lab task brief — distilled for the presentation

Full original brief lives in the team's Google Doc and
`location-based-webxr/TASK.md`. This is a distilled version, keeping only
what's relevant for building the final presentation.

## Team

Maria Elia & Nico Hellthaler, working in a fork of
[`cs-util-com/location-based-webxr`](https://github.com/cs-util-com/location-based-webxr).

## The product, in one paragraph

An author (e.g. castle staff) walks a route once, uses an **authoring mode**
to drop waypoints, attach assets (2D sprites, GLTF/GLB 3D models, audio
files) to them, and records a breadcrumb trail by walking it. The app exports
everything into a single `tour.zip` (a `tour.json` plus the asset files). The
author uploads that zip to an everyday cloud-storage service (Google Drive,
Dropbox, OneDrive, Box) and generates a QR code that opens the app in
**viewing mode** pointed at the zip's URL. A visitor scans the QR code; the
viewing mode streams `tour.json` and each asset out of the zip on demand via
byte-range requests (no full download up front), then guides them along an
AR breadcrumb trail of glowing orbs. As the visitor approaches a waypoint, an
object (e.g. a knight) appears: assets are quietly pre-loaded while still far
away, the object becomes visible once close, and tapping it plays a
historical story (audio + floating text transcript). A toggleable 2D map
shows real-time position and points of interest. No backend database — the
zip is the entire state.

## The architecture that makes it splittable

Everything flows through one shared contract, agreed on **before** splitting
component work:

- **`tour.json` schema** — the on-disk/on-wire format: waypoints (lat/lon,
  proximity radii, ordering), assets they reference, breadcrumb trail points.
  Lat/lon is only the persisted form; at runtime everything works in
  world-space meters.
- **Redux store** — single source of truth at runtime for the 2D UI, the
  Three.js/WebXR scene, and live GPS/device state. Assets are referenced by
  id, never file path, via a small asset-provider interface
  (`getAssetUrl(id)`, `release(id)`) that hands out Blob URLs on demand.

This contract is pinned in `plans/Shared-Contract.md` (decisions D1–D17) in
the TourBuilder package.

## Goal 1 — components first

Ten components, each built as an isolated, individually testable piece with
its own tiny demo page, unit tests for pure logic, and (where
movement-dependent) replay e2e tests against real recorded outdoor walks. See
`COMPONENTS.md` for the list actually built.

## Goal 2 — composition

Once components are individually approved, wire them through the store into
one app with two modes, chosen by whether a `?tour=…` URL is present:

- **Authoring**: onboarding gate → authoring tools (drop/attach/record) →
  packaging + QR.
- **Viewing**: scan QR → cloud-storage tour source range-reads the zip →
  onboarding gate → proximity state machine driving the AR scene + 2D map,
  with wayfinding and tap-to-play audio/transcript.

## Presentation requirements (from the organizers)

- ~15 min presentation + 5 min Q&A, both team members present.
- Cover: what the component does and why it's useful to the project; your
  contributions with a live demo or code walkthrough; architecture; lessons
  learned (what was hard, what surprised you, what you'd do differently, how
  pair programming worked); how AI tools were used and how contributing to an
  existing codebase changed that compared to building from scratch.
- Visual content (diagrams, demo recordings) over walls of code.
