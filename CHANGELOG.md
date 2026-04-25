# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [Unreleased]

(No unreleased changes at this time. Next proposed version: v0.3.0)

---

## [0.2.0] - 2026-04-25

### Added
- Added corridor / entry zone to floorplan (center vertical zone)
- Added dynamic room lighting overlay system (architecture prepared for future brightness/RGB support)
- Added room light overlay elements with CSS classes: `.room-light-overlay`, `.is-on`, `.is-off`, `.brightness-low`, `.brightness-medium`, `.brightness-high`
- Added Roborock vacuum visual indicator and status display zone
- Added comprehensive room-to-entity mapping in YAML configuration
- Added fullscreen/panel dashboard view recommendations in README and docs

### Changed
- **Refactored SVG layout:** Changed viewBox from 800x600 to 600x1200 for vertical/mobile-first design
- **Updated room positions:** Confirmed correct layout (kitchen top-left, bathroom bottom-left, bedroom top-right, living room bottom-right)
- **Redesigned visual theme:** Changed from light theme to modern dark theme (#0f1419 background)
- **Improved room styling:** Added rounded corners (rx/ry=16), soft shadows, and modern aesthetics
- **Updated entity bindings:** Replaced placeholder entities with real Home Assistant entity IDs:
  - Kitchen: `light.lampochki_na_kukhne`
  - Bedroom: `light.smart_ceiling_light`
  - Corridor: `light.hall_strip`
  - Roborock: `vacuum.roborock_qrevo_curv_series`
- **Refactored Lovelace card:** Updated YAML with real entity IDs, proper tap/more-info actions
- **Enhanced documentation:** Expanded README.md with installation guide, room descriptions, tips & tricks

### Fixed
- Fixed incorrect entity_id references (was using generic `light.kitchen`, `light.bedroom`, etc.)
- Fixed room positioning in SVG (confirmed correct layout geometry)
- Fixed CSS color scheme to match modern dark-theme aesthetic

### Security
- Added security note about secrets in documentation
- Added guideline to keep entity_id changes out of repository unless necessary

### Documentation
- Updated README.md with complete installation guide, feature list, and usage examples
- Updated docs/installation.md with step-by-step setup instructions
- Updated docs/development.md with SVG/CSS editing guidelines
- Updated docs/design_notes.md with philosophy behind design choices
- Created docs/dashboard_examples.md with panel view and mobile optimization tips
- Updated TODO.md with version-specific tasks

---

## [0.1.0] - Initial version

### Added
- Added first SVG floorplan (basic 4-room layout)
- Added base CSS styling
- Added initial Lovelace card config
- Added documentation structure (README, docs/, etc.)
- Added .gitignore and LICENSE

---

### Version Numbering

- **v0.x.x** = Pre-release versions. Breaking changes, major refactors may occur.
- **v1.0.0** = Stable release when core features are complete and tested.

### Future Versions (Planned)

| Version | Target | Key Goals |
|---|---|---|
| **v0.3.0** | Q2 2026 | Brightness-aware glow, RGB/color temperature support |
| **v0.4.0** | Q3 2026 | Roborock live position investigation & integration |
| **v0.5.0** | Q3 2026 | Animations, sensor labels, scene buttons |
| **v1.0.0** | Q4 2026 | Stable release with comprehensive documentation |

