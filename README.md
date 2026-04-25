# ha-floorplan-gabelsberger

Interactive Home Assistant floorplan for an apartment, built with ha-floorplan, SVG, CSS and Lovelace YAML.

---

## 1. Project Overview

This project provides a custom interactive floorplan for [Home Assistant](https://www.home-assistant.io/) using the [ha-floorplan](https://github.com/ExperienceLovelace/ha-floorplan) frontend component installed via [HACS](https://hacs.xyz/).

The floorplan is drawn as an SVG file, styled with a separate CSS file, and wired to Home Assistant entities via a Lovelace Manual card defined in YAML.

---

## 2. Features

- SVG-based apartment map with named, clickable rooms
- CSS-driven visual design with soft shadows, rounded shapes and calm colors
- Glow layers for light on/off state per room
- Brightness-aware glow opacity
- Lovelace Manual card integration via `floorplan_card.yaml`
- Clean separation between SVG, CSS and YAML

---

## 3. Requirements

- Home Assistant (any recent version)
- HACS (Home Assistant Community Store)
- `ha-floorplan` installed via HACS → Frontend
- Files hosted at `/config/www/floorplan/` inside Home Assistant

---

## 4. Installation

See [docs/installation.md](docs/installation.md) for the full step-by-step guide.

Quick summary:

1. Install HACS
2. Install `ha-floorplan` from HACS → Frontend
3. Copy `apartment_floorplan.svg` and `apartment_floorplan.css` to `/config/www/floorplan/`
4. Add a Manual card in Lovelace using the contents of `floorplan/floorplan_card.yaml`

---

## 5. File Structure

```
/
├── README.md
├── CHANGELOG.md
├── TODO.md
├── LICENSE
├── .gitignore
├── floorplan/
│   ├── apartment_floorplan.svg   ← SVG floorplan map
│   ├── apartment_floorplan.css   ← Visual styling & animations
│   └── floorplan_card.yaml       ← Lovelace Manual card config
├── docs/
│   ├── installation.md           ← Setup instructions
│   ├── development.md            ← Development guidelines
│   └── design_notes.md           ← Design decisions
└── preview/
    └── .gitkeep                  ← Placeholder for screenshot previews
```

---

## 6. Home Assistant Paths

| Purpose | Path |
|---|---|
| SVG file on disk | `/config/www/floorplan/apartment_floorplan.svg` |
| CSS file on disk | `/config/www/floorplan/apartment_floorplan.css` |
| SVG URL in browser | `/local/floorplan/apartment_floorplan.svg` |
| CSS URL in browser | `/local/floorplan/apartment_floorplan.css` |
| ha-floorplan resource | `/hacsfiles/ha-floorplan/floorplan.js` |

---

## 7. Development Workflow

1. Edit SVG and CSS files locally in the `floorplan/` directory of this repository.
2. Copy updated files to `/config/www/floorplan/` on your Home Assistant instance.
3. Hard-refresh the Lovelace dashboard (Ctrl+Shift+R or Cmd+Shift+R).
4. Test entity interactions and visual states in Home Assistant.
5. Commit small, tested changes.

See [docs/development.md](docs/development.md) for full guidelines.

---

## 8. Roadmap

| Version | Goal |
|---|---|
| v0.1.0 | Initial structure, SVG, CSS, YAML placeholder |
| v0.2.0 | Correct room layout, improved visuals, mobile-friendly |
| v0.3.0 | Dynamic lighting via HA entities, glow effects |
| v0.4.0 | Roborock live map investigation |
| v0.5.0 | Motion animations, sensor labels, scene buttons |

See [TODO.md](TODO.md) for detailed task list.

---

## 9. Notes

- The SVG is **not** a CAD-accurate architectural drawing. UX clarity is the priority.
- All secrets and entity names must be kept out of this repository. Use Home Assistant's `secrets.yaml` for credentials.
- Do **not** edit files in `/config/www/community/` (managed by HACS).
- The `preview/` folder is reserved for dashboard screenshots.
