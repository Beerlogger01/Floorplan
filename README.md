# ha-floorplan-gabelsberger

Interactive Home Assistant floorplan for an apartment, built with [ha-floorplan](https://github.com/ExperienceLovelace/ha-floorplan), SVG, CSS and Lovelace YAML.

A modern, dark-themed smart home dashboard showing live lighting, device status, and room interactions.

---

## 1. Project Overview

This project provides a custom interactive floorplan for [Home Assistant](https://www.home-assistant.io/) using the [ha-floorplan](https://github.com/ExperienceLovelace/ha-floorplan) frontend component installed via [HACS](https://hacs.xyz/).

The floorplan is drawn as an SVG file, styled with a separate CSS file, and wired to Home Assistant entities via a Lovelace Manual card defined in YAML.

**Key philosophy:** UX clarity and modern aesthetics are prioritized over architectural CAD accuracy.

---

## 2. Current Features (v0.2.0)

- ✅ SVG-based apartment map with 5 named, clickable rooms (kitchen, bathroom, bedroom, living room, corridor)
- ✅ Correct room layout matching real apartment geometry
- ✅ Modern dark theme with soft shadows and rounded shapes
- ✅ Mobile-friendly and fullscreen-ready layout (vertical-optimized viewBox)
- ✅ Dynamic room lighting overlays (prepared architecture)
- ✅ Light on/off state visualization with brightness-aware opacity
- ✅ Tap actions for room lights (toggle)
- ✅ More-info actions for sensors and devices
- ✅ Roborock vacuum status display (placeholder for live map integration)
- ✅ Lovelace Manual card integration via `floorplan_card.yaml`
- ✅ Clean separation between SVG, CSS and YAML
- ✅ Comprehensive documentation and development guides

---

## 3. Requirements

- **Home Assistant** (any recent version)
- **HACS** (Home Assistant Community Store)
- **ha-floorplan** installed via HACS → Frontend
- **Files** hosted at `/config/www/floorplan/` inside Home Assistant

---

## 4. Installation

### Quick Start (5 minutes)

1. **Install HACS** (if not already done):
   - Go to Home Assistant → Settings → Devices & Services → Custom Integrations
   - Search "HACS", install and restart Home Assistant

2. **Install ha-floorplan**:
   - Open HACS → Frontend
   - Search "ha-floorplan", install, restart Home Assistant

3. **Add the resource** to Home Assistant:
   - Settings → Dashboards → Resources
   - Create new resource: `/hacsfiles/ha-floorplan/floorplan.js` (type: JavaScript Module)

4. **Copy files to Home Assistant**:
   ```
   From repository         To Home Assistant
   floorplan/apartment_floorplan.svg    →    /config/www/floorplan/apartment_floorplan.svg
   floorplan/apartment_floorplan.css    →    /config/www/floorplan/apartment_floorplan.css
   ```

5. **Add the Lovelace card**:
   - Go to your dashboard → Edit dashboard
   - Add Card → Manual
   - Paste contents of `floorplan/floorplan_card.yaml`
   - Save and reload dashboard (Ctrl+Shift+R or Cmd+Shift+R)

Full step-by-step guide: [docs/installation.md](docs/installation.md)

---

## 5. File Structure

```
/
├── README.md                          ← You are here
├── CHANGELOG.md                       ← Version history
├── TODO.md                            ← Roadmap
├── LICENSE                            ← MIT license
├── .gitignore                         ← Git ignore rules
│
├── floorplan/                         ← Main floorplan files
│   ├── apartment_floorplan.svg        ← SVG apartment map (600x1200, vertical layout)
│   ├── apartment_floorplan.css        ← Dark theme styling + animations
│   └── floorplan_card.yaml            ← Home Assistant Lovelace card config
│
├── docs/                              ← Documentation
│   ├── installation.md                ← Setup instructions
│   ├── development.md                 ← Development guidelines
│   ├── design_notes.md                ← Design decisions & philosophy
│   └── dashboard_examples.md          ← Dashboard view examples
│
└── preview/                           ← Screenshots folder (reserved)
    └── .gitkeep
```

---

## 6. Home Assistant Paths

| Purpose | Path |
|---|---|
| **SVG file on disk** | `/config/www/floorplan/apartment_floorplan.svg` |
| **CSS file on disk** | `/config/www/floorplan/apartment_floorplan.css` |
| **SVG URL in browser** | `/local/floorplan/apartment_floorplan.svg` |
| **CSS URL in browser** | `/local/floorplan/apartment_floorplan.css` |
| **ha-floorplan resource** | `/hacsfiles/ha-floorplan/floorplan.js` |
| **YAML card config** | `floorplan/floorplan_card.yaml` |

---

## 7. Room Layout & Entities

### Kitchen (top-left)
- **Light:** `light.lampochki_na_kukhne`
- **Devices:** teapot, switches
- **Interaction:** Tap to toggle light

### Bathroom (bottom-left)
- **Status:** Static (no smart devices yet)
- **Interaction:** More info / future extension

### Corridor + Entry (center)
- **Light:** `light.hall_strip`
- **Devices:** Motion sensor, luminance sensor, smart doorbell camera
- **Interaction:** Tap to toggle light

### Bedroom (top-right)
- **Light:** `light.smart_ceiling_light`
- **Devices:** Temperature/humidity sensor, scene button
- **Interaction:** Tap to toggle light

### Living Room (bottom-right)
- **Devices:** TV (media player)
- **Light:** Static (pending real light entity, see TODO.md)
- **Interaction:** More info

### Roborock Vacuum (center)
- **Device:** `vacuum.roborock_qrevo_curv_series`
- **Status sensor:** `sensor.roborock_qrevo_curv_series_status`
- **Interaction:** More info

---

## 8. How It Works

### Dynamic Lighting

Each room has a base floor shape and a light overlay shape:
- **Light ON** → overlay shows with warm glow (opacity ~0.25)
- **Light OFF** → overlay hidden (opacity 0)
- **Brightness** → affects opacity (low/medium/high)

CSS handles animations and transitions. The YAML `class_template` updates classes based on Home Assistant entity state.

### Tap Interactions

- **Rooms with lights** → toggle light on/off
- **Rooms/devices without controls** → show more info modal
- **Future:** hold actions for device-specific controls

### Roborock Integration

Currently shows vacuum status and device info. Future work:
- Extract live robot position from `image.roborock_qrevo_curv_series_dom_gabelsbergerstr`
- Overlay position indicator on floorplan
- See [docs/design_notes.md](docs/design_notes.md) for details

---

## 9. Development Workflow

1. Edit SVG and CSS files in the `floorplan/` directory
2. Copy updated files to `/config/www/floorplan/` on your Home Assistant instance
3. Hard-refresh Lovelace dashboard (Ctrl+Shift+R or Cmd+Shift+R)
4. Test entity interactions and visual states
5. Commit tested changes

See [docs/development.md](docs/development.md) for full guidelines.

---

## 10. Limitations & Known Issues

| Item | Status | Notes |
|---|---|---|
| **Room proportions** | ✅ Correct | Layout matches real apartment geometry |
| **Mobile layout** | ✅ Optimized | Vertical viewBox (600x1200) works well on phones |
| **Roborock live position** | ⚠️ Investigated | Data source identified but integration pending |
| **Living room light** | ⚠️ Temporary | `light.audiosistema_outlet` is not ideal; waiting for real entity |
| **RGB/color temperature** | 📋 Planned | Architecture prepared, not yet implemented |
| **Sensor value labels** | 📋 Planned | Temperature, humidity, motion status display |
| **Scenes / automations** | 📋 Planned | Scene buttons and automation triggers |

---

## 11. Design Notes

### Why Dark Theme?

Home Assistant dashboards often run on wall-mounted tablets or are viewed at night. A dark theme:
- Reduces eye strain
- Looks modern and professional
- Fits the smart home aesthetic

### Why No Roborock Map as Background?

The live Roborock map is useful for cleanup planning but:
- Adds complexity and performance overhead
- Makes room shapes hard to distinguish
- Can distract from other controls

We keep the clean SVG floorplan as the main UI and investigate Roborock integration as a separate feature.

### Why Vertical Layout?

Most smart home dashboards are viewed on phones and tablets in portrait mode. A 600x1200 viewBox:
- Uses the full screen height
- Avoids wasting horizontal space
- Makes rooms large enough to tap comfortably

See [docs/design_notes.md](docs/design_notes.md) for more design decisions.

---

## 12. Roadmap

| Version | Target | Goal |
|---|---|---|
| **v0.1.0** | ✅ Done | Initial structure, placeholders |
| **v0.2.0** | ✅ Current | Correct layout, dark theme, mobile-friendly |
| **v0.3.0** | 📋 Planned | Brightness-aware lighting, RGB support |
| **v0.4.0** | 📋 Planned | Roborock live map investigation |
| **v0.5.0** | 📋 Planned | Animations, sensor labels, scene buttons |

See [TODO.md](TODO.md) for detailed task list.

---

## 13. Tips & Tricks

### Fullscreen Dashboard View

For a cleaner look on wall-mounted tablets:
1. Settings → Dashboards → Create new [Panel] view
2. Add only this floorplan card
3. Set as default or bookmark

This gives you maximum space and minimal UI clutter.

### Mobile Phone Optimization

The vertical layout (600x1200) works great on phones:
- Install companion app on Android/iOS
- Add this dashboard to the home screen
- Works in portrait mode without horizontal scrolling

### Updating Files

After editing SVG/CSS:
1. Copy new files to `/config/www/floorplan/`
2. Hard-refresh browser (Ctrl+Shift+R)
3. If still showing old version, clear browser cache and restart Home Assistant

---

## 14. Contributing & Support

- **Questions?** Check [docs/development.md](docs/development.md) and [docs/design_notes.md](docs/design_notes.md)
- **Bug reports?** Open an issue on GitHub
- **Improvements?** PRs welcome! Follow guidelines in [docs/development.md](docs/development.md)

---

## 15. License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

**Last updated:** April 2026 | Version: 0.2.0

