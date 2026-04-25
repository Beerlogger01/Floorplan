# Installation Guide

This guide explains how to set up the Home Assistant **ha-floorplan-gabelsberger** from scratch.

**Estimated time:** 10–15 minutes

---

## Prerequisites

- Home Assistant installation (any recent version)
- Administrator access to Home Assistant
- Network access to your Home Assistant instance (local network)
- Basic familiarity with Home Assistant UI

---

## Step 1 — Install HACS

If you haven't already, install HACS (Home Assistant Community Store):

1. Follow the [official HACS installation guide](https://hacs.xyz/docs/use/download/download/)
2. After installation, HACS will appear in your Home Assistant sidebar
3. Restart Home Assistant when prompted

---

## Step 2 — Install ha-floorplan via HACS Frontend

1. Open **Home Assistant** → **HACS**
2. Click **Frontend**
3. Search for **ha-floorplan**
4. Click the result and then **Download**
5. Confirm the download
6. **Restart Home Assistant** when prompted

---

## Step 3 — Verify the Dashboard Resource

Go to **Settings** → **Dashboards** → **Resources** and check that the following resource is listed:

```
/hacsfiles/ha-floorplan/floorplan.js
```

**Type:** JavaScript module

If it's missing, add it manually:
- Click **Create a resource**
- URL: `/hacsfiles/ha-floorplan/floorplan.js`
- Type: `JavaScript module`
- Save

---

## Step 4 — Copy SVG and CSS Files to Home Assistant

Copy the floorplan files from this repository to your Home Assistant instance:

**From (repository):**
```
floorplan/apartment_floorplan.svg
floorplan/apartment_floorplan.css
```

**To (Home Assistant):**
```
/config/www/floorplan/apartment_floorplan.svg
/config/www/floorplan/apartment_floorplan.css
```

If `/config/www/floorplan/` folder doesn't exist, create it.

### Ways to copy files:

**Option A: Samba Share (easiest)**
- Open network drive: `\\homeassistant.local\config` (or your HA IP)
- Navigate to `www/floorplan/`
- Copy files there

**Option B: SSH + scp**
```bash
scp floorplan/apartment_floorplan.svg homeassistant:/config/www/floorplan/
scp floorplan/apartment_floorplan.css homeassistant:/config/www/floorplan/
```

**Option C: File Editor add-on**
- Install "File editor" add-on in Home Assistant
- Navigate to `/config/www/floorplan/`
- Create files manually or upload them

---

## Step 5 — Verify SVG is Accessible

Test that the SVG file is reachable:

Open this URL in your browser (replace `homeassistant.local` with your actual HA address):

```
http://homeassistant.local:8123/local/floorplan/apartment_floorplan.svg
```

You should see the SVG apartment floorplan rendered. If you get a **404 error**, check:
- File path: is it in `/config/www/floorplan/`?
- File permissions: can Home Assistant read it?

---

## Step 6 — Add the Lovelace Manual Card

1. Go to your dashboard (or create a new dashboard)
2. Click **Edit dashboard** (pencil icon)
3. Click **Add card** → **Manual**
4. Copy and paste the contents of `floorplan/floorplan_card.yaml` from this repository

**Important:** Review the entity IDs in the YAML and update them to match your Home Assistant instance:

```yaml
# Example: Update these entity IDs
- entity: light.lampochki_na_kukhne      ← Check this exists in your HA!
  element: room-kitchen
```

To find your real entity IDs:
- Go to **Settings** → **Devices and Services** → **Entities**
- Search for "light" or your device name
- Copy the entity ID you find

5. Click **Save** and close edit mode
6. **Hard-refresh browser:** Ctrl+Shift+R (Windows/Linux) or Cmd+Shift+R (Mac)

---

## Step 7 — Customize Entity IDs (Important!)

The `floorplan_card.yaml` file contains example entity IDs. You **must** update them to match your real Home Assistant entities:

### Kitchen
Find these in your HA instance and update:
- `light.lampochki_na_kukhne` (main kitchen light)
- `switch.w601_switch_1` (if you have this device)
- `switch.teapot_start` (if you have this device)

### Bedroom
- `light.smart_ceiling_light`
- `sensor.datchik_temperatury_temperature`
- `sensor.datchik_temperatury_humidity`

### Corridor / Entry
- `light.hall_strip`
- `binary_sensor.datchik_dvizheniia_motion`
- `camera.intellektualnyi_dvernoi_zvonok`

### Living Room
- `media_player.tv`

### Vacuum
- `vacuum.roborock_qrevo_curv_series`

**How to update YAML:**
1. Edit the card (click **Edit** on the card itself or edit in dashboard)
2. Find each entity ID
3. Replace with your real entity ID (from Settings → Entities)
4. Save and test

---

## Step 8 — Test the Floorplan

1. Click on a room (e.g., Kitchen) — the light should toggle on/off
2. Check the room color changes when light is on (should show a warm glow)
3. Try clicking other rooms
4. Try tapping sensor icons for more-info modals
5. Check browser console (F12) for any errors

---

## Step 9 — Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| SVG returns 404 | File not in `/config/www/floorplan/` | Copy files to correct path |
| `Custom element doesn't exist` error | ha-floorplan resource not added | Add `/hacsfiles/ha-floorplan/floorplan.js` as a resource |
| Card shows blank / no SVG | Path wrong in YAML (`image:` field) | Verify `/local/floorplan/apartment_floorplan.svg` in YAML |
| Clicking room does nothing | Entity ID doesn't exist in HA | Check entity ID exists in Settings → Entities |
| Lights don't toggle | YAML syntax error or entity mismatch | Validate YAML indentation, check entity IDs |
| Old version still showing | Browser cache | Hard-refresh (Ctrl+Shift+R), clear cache |

### Getting help

- Check [docs/development.md](development.md) for debugging tips
- Check [docs/design_notes.md](design_notes.md) for design philosophy
- Review browser console (F12) for JavaScript errors
- Verify all entity IDs exist: Settings → Devices and Services → Entities

---

## Step 10 — (Optional) Fullscreen Dashboard

For a cleaner look (e.g., on a wall-mounted tablet):

1. Go to Dashboards
2. Create a new dashboard (or edit existing)
3. Change to **Panel** view (not cards view)
4. Add only this floorplan card
5. Save and enjoy fullscreen display

See [docs/dashboard_examples.md](dashboard_examples.md) for more examples.

---

## Next Steps

- **Customize appearance:** Edit `apartment_floorplan.css` to change colors, shadows, fonts
- **Add more devices:** Update `floorplan_card.yaml` to bind more entities
- **Test lighting:** Try different light colors/brightness to see how overlays respond
- **Read more:** See [README.md](../README.md) for feature overview

---

## Support & Contributing

- Questions? Check [design_notes.md](design_notes.md)
- Bug found? Open an issue on GitHub
- Want to improve? PRs welcome! Follow [development.md](development.md)

---

**Last updated:** April 2026 | Version: 0.2.0

