# Components built — brief summary

The 10 components from the task spec, as actually implemented in
`GpsPlusSlamJs_TourBuilder/src/components/` (path column is relative to that
package). Each has its own demo page, a pure `core/` unit-tested layer, and a
`view/` (or `runtime/`) layer with the Three.js/DOM/network side.

| # | Component | Path | What it does |
|---|-----------|------|---------------|
| 1 | Clickable billboard | `billboard/` | Yaw-to-face sprite + spatialized audio + in-world transport panel (play/stop, seekable bar). Seed of the AR knight markers. |
| 2 | In-world text | `in-world-text/` | Billboarded paginated text panel; HTML-in-3D with automatic `CanvasTexture` fallback (XR-safe). |
| 3 | Tour data model + store | `store/` (lives at `src/store/`, not under `components/`) | The shared contract: schema types, `validateTour`, Redux slices, selectors, two store factories. |
| 4 | Proximity & zone machine | `proximity/` | `IDLE → PREFETCHING → ACTIVE` per waypoint with hysteresis, pure world-space (X/Z), no GPS/geo math. Machine itself now lives upstream in the AppFramework; replay e2e stays here. |
| 5 | Packaging & QR | `packaging/` | Bundles a `Tour` + assets into an uncompressed `tour.zip`; turns the hosted URL into a scannable link. |
| 6 | Cloud-storage tour source | `cloud-loader/` | `?tour=<zipUrl>` → running tour: byte-range reads of the hosted ZIP, `AssetProvider` by id, local warm copy, no-range fallback. |
| 7 | 2D map overview | `map/` | Toggleable real-time Leaflet map (plain DOM): visitor dot + waypoint markers recoloured by the real proximity driver. |
| 8 | AR viewing scene | `ar-scene/` | The Three.js side of viewing mode: geo→world anchoring, knights that prefetch at 25 m and appear at 10 m, tap-to-play stories with a floating transcript, recycled breadcrumb trail. |
| 9 | Onboarding gate | `onboarding/` | Camera/GPS permission checklist gating a Start button; Start click doubles as the gesture that unlocks the Web Audio API. |
| 10 | Authoring tools | `authoring/` | Drop waypoints at the live (or replayed) GPS position, attach model/sprite/audio, record the breadcrumb trail, export a real `tour.zip`. |

## Supporting (not standalone components)

- `shared/` — cross-component pure helpers (billboard math, canvas panel,
  panel geometry, tap gate, pointer/tap picker, playback loop, resize, clamp).
- `src/store/` — the contract root; dependencies flow components → store
  only (enforced by dependency-cruiser).
- `scripts/` — fixture generators and demo-track extractors that turn a
  recorded walk into `demo-track.json` for the map/authoring demos.

## Composition

Components 1–6 are the Goal-1 building blocks; 7–10 compose them into the
two app modes (Authoring / Viewing) — see `TASK.md` for the composition
flow. Two test levels are used throughout: plain unit tests for pure logic,
and replay e2e tests that feed real recorded outdoor walks
(`recordings/*.zip`) through `replayRecording` so movement-dependent
components (proximity, map, AR scene, authoring) run deterministically on a
desktop with no phone.
