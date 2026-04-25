# Development Guidelines

Follow these rules when working on this project to keep the codebase clean and stable.

---

## File locations

- **Only edit files in `/config/www/floorplan/`** on your Home Assistant instance.
- **Never edit files in `/config/www/community/`** — this directory is managed by HACS and will be overwritten on updates.
- Keep the repository (`floorplan/`) in sync with what is deployed on the Home Assistant instance.

---

## SVG rules

- Keep SVG element `id` attributes **stable** — changing an `id` will break YAML bindings.
- Every clickable or entity-bound SVG element must have a **clear, descriptive `id`**.
- Group related elements inside `<g id="room-*">` groups.
- Do not embed large inline styles in the SVG — use the external CSS file instead.

---

## CSS rules

- Keep all visual styling in `apartment_floorplan.css`.
- Do not duplicate styles between the SVG `<style>` block and the external CSS.
- Use class names (e.g. `.light-on`, `.glow-low`) for dynamic states applied by ha-floorplan.

---

## YAML rules

- Every Home Assistant entity referenced in `floorplan_card.yaml` must exist in your HA instance.
- Do **not** hardcode secrets, tokens or passwords in YAML files.
- Keep CSS class logic in the `class_template` fields, not in inline styles.

---

## Git workflow

- Commit **small, focused changes** (one feature or fix per commit).
- Write clear commit messages (e.g. `fix: correct kitchen room position`).
- **Test every change** in Home Assistant before committing.
- Use the `preview/` folder to store dashboard screenshots when useful.

---

## Testing checklist

After each change, verify the following in Home Assistant:

- [ ] SVG renders correctly in the Lovelace card
- [ ] Room labels are readable
- [ ] Clicking a room triggers the correct entity action
- [ ] Light on/off state changes the room appearance
- [ ] No browser console errors related to ha-floorplan
