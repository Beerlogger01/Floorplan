# Installation Guide

This guide explains how to set up the Home Assistant floorplan from scratch.

---

## Step 1 — Install HACS

Follow the official [HACS installation guide](https://hacs.xyz/docs/use/download/download/).

After installation, HACS will appear in your Home Assistant sidebar.

---

## Step 2 — Install ha-floorplan via HACS Frontend

1. Open Home Assistant → **HACS** → **Frontend**.
2. Search for **ha-floorplan**.
3. Click **Download** and confirm.
4. Reload Home Assistant when prompted.

---

## Step 3 — Confirm the dashboard resource exists

Go to **Settings → Dashboards → Resources** and verify that the following resource is listed:

```
/hacsfiles/ha-floorplan/floorplan.js
```

Type: **JavaScript module**

If it is missing, add it manually.

---

## Step 4 — Copy SVG and CSS files to Home Assistant

Copy the two floorplan files to your Home Assistant config directory:

```
/config/www/floorplan/apartment_floorplan.svg
/config/www/floorplan/apartment_floorplan.css
```

Create the `/config/www/floorplan/` folder if it does not exist.

You can do this via:
- **Samba share** (network drive)
- **SSH** and `scp`
- **File Editor** add-on in Home Assistant

---

## Step 5 — Verify the SVG is accessible

Open the following URL in your browser (replace `homeassistant.local` with your HA address):

```
http://homeassistant.local:8123/local/floorplan/apartment_floorplan.svg
```

You should see the SVG rendered. If you get a 404 error, check the file path.

---

## Step 6 — Add the Lovelace Manual card

1. Go to the dashboard where you want the floorplan.
2. Click **Edit dashboard** → **Add card** → **Manual**.
3. Copy the contents of `floorplan/floorplan_card.yaml` from this repository.
4. Paste it into the Manual card editor.
5. Update entity IDs (`light.kitchen`, etc.) to match your real Home Assistant entities.
6. Click **Save**.

---

## Step 7 — Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| `404` error on SVG or CSS | Wrong file path | Check `/config/www/floorplan/` structure |
| `custom element doesn't exist` | ha-floorplan resource missing | Add `/hacsfiles/ha-floorplan/floorplan.js` as a resource |
| Blank card / no floorplan rendered | SVG or CSS URL is wrong in YAML | Verify paths in `floorplan_card.yaml` |
| Entity not responding to tap | Entity ID mismatch | Check entity IDs in `floorplan_card.yaml` match HA |
