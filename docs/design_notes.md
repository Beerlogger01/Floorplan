# Design Notes

This document records the design decisions made for the floorplan to help maintain consistency across future changes.

---

## Overall philosophy

- This is **not** a CAD-accurate architectural plan. It is a functional UI.
- **UX is more important than exact geometry.** Rooms must be readable and tappable, not perfectly scaled.
- Prioritize clarity over completeness — show only what is useful in daily use.

---

## Visual style

- Use **soft shadows** (`drop-shadow` filter in CSS) to give rooms depth.
- Use **rounded shapes** (`rx`/`ry` on `<rect>` elements) for a friendly appearance.
- Use **calm, muted colors** — avoid bright or saturated fills for inactive states.
- Use **glow layers** (CSS opacity classes) to indicate light state per room:
  - `glow-low` → 30% opacity
  - `glow-mid` → 60% opacity
  - `glow-high` → 100% opacity
- Warm yellow tint (`#fff5c0`) for rooms where the light is on.

---

## Room layout

Rooms are arranged in a 2×2 grid:

| | Left | Right |
|---|---|---|
| **Top** | Kitchen | Bedroom |
| **Bottom** | Bathroom | Living Room |

---

## Room-by-room notes

| Room | Smart objects | Notes |
|---|---|---|
| Kitchen | Lights | Top-left position |
| Bathroom | None (for now) | No smart objects planned in v0.1–v0.3 |
| Bedroom | Lights | Top-right position |
| Living Room | Lights, scenes | Bottom-right position |
| Corridor / Entry | — | Treated as one functional zone, may be added later |

---

## Roborock integration

- The Roborock vacuum should be shown as a **device/status layer**, not as the main floorplan background.
- Investigation of `image.roborock_qrevo_curv_series_dom_gabelsbergerstr` is planned for v0.4.0.
- The clean SVG floorplan remains the primary UI even after Roborock integration.

---

## What to avoid

- Do not clutter the floorplan with every available Home Assistant entity.
- Do not use the Roborock live map image as the floorplan background.
- Do not hardcode colors or styles inside the SVG file — use `apartment_floorplan.css`.
