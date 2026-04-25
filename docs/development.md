# Development Guidelines

Follow these rules when working on this project to keep the codebase clean, maintainable, and stable.

---

## Core Rules

### SVG Element IDs are Sacred

🔴 **DO NOT** change SVG element IDs without updating YAML!

Once an `id` attribute is referenced in `floorplan_card.yaml`, it becomes a stable contract:

**Example:**
```svg
<!-- SVG -->
<rect id="kitchen-floor" ... />
```

```yaml
# YAML
- entity: light.lampochki_na_kukhne
  element: room-kitchen        ← references the <g> parent
  ...
```

**If you must rename:**
1. Update SVG `id` attribute
2. Update YAML `element` reference
3. Update CSS selectors
4. Update all documentation
5. Test thoroughly before committing

**Why?** Changing IDs breaks YAML bindings, making the floorplan non-functional.

---

### No Hardcoded Styles in SVG

All visual styling belongs in `apartment_floorplan.css`, not in SVG `<style>` blocks or inline `style=""` attributes.

**❌ Wrong:**
```svg
<style>
  #kitchen-floor { fill: #1a1f27; stroke: #2a3142; }
</style>
```

**✅ Right:**
```css
/* apartment_floorplan.css */
#kitchen-floor {
  fill: #1a1f27;
  stroke: #2a3142;
}
```

**Why?** Separation of concerns makes theming and maintenance easier.

---

### Entity IDs Must Exist

Every entity referenced in `floorplan_card.yaml` must exist in your Home Assistant instance.

**Before committing YAML changes:**
1. Go to Settings → Devices and Services → Entities
2. Search for each entity ID
3. Verify it exists and is the right device
4. If not, find the correct ID or mark as TODO

**Example check:**
```bash
# Does this entity exist?
light.lampochki_na_kukhne
↓
Check in HA: Settings → Entities → Search "lampochki"
↓
✅ Found, matches real kitchen light
```

---

### No Secrets in Code

Do **not** hardcode passwords, tokens, or credentials in SVG, CSS, or YAML files.

**Allowed:** `entity_id` (e.g., `light.lampochki_na_kukhne`)
**Forbidden:** API keys, passwords, IP addresses, phone numbers

Use Home Assistant's `secrets.yaml` for sensitive info, never in the floorplan files.

---

## File Structure Rules

### Only Edit in `/config/www/floorplan/`

On your Home Assistant instance, only modify files in:
```
/config/www/floorplan/
  ├── apartment_floorplan.svg
  ├── apartment_floorplan.css
  └── floorplan_card.yaml
```

**Never edit:**
```
❌ /config/www/community/           (HACS-managed, will be overwritten)
❌ /config/automations/floorplan*   (should be in main automations)
❌ /config/custom_components/       (not needed for floorplan)
```

### Keep Repository & HA in Sync

1. Edit files locally in the repository (`floorplan/` folder)
2. Copy to Home Assistant (`/config/www/floorplan/`)
3. Test in Home Assistant
4. Commit changes to repository

---

## SVG Development Rules

### Keep SVG Simple

- Use basic shapes: `<rect>`, `<circle>`, `<g>`, `<text>`
- Avoid overly complex paths or nested groups
- One SVG element = one distinct "thing" (room, device, label)

### Naming Convention

- Room groups: `id="room-{name}"` (e.g., `room-kitchen`, `room-bedroom`)
- Room floors: `id="{name}-floor"` (e.g., `kitchen-floor`, `bedroom-floor`)
- Room labels: `id="{name}-label"` (e.g., `kitchen-label`, `bedroom-label`)
- Light overlays: `id="room_{name}_light"` (e.g., `room_kitchen_light`)
- Special elements: `id="{function}-{detail}"` (e.g., `entrance-marker`, `robot-position`)

**Why?** Consistent naming makes it easy to find elements and predict structure.

### Testing SVG Changes

After editing SVG:

1. **Syntax check:** Make sure it's valid XML (no mismatched tags)
2. **View in browser:** Go to `/local/floorplan/apartment_floorplan.svg`
3. **Test in Lovelace:** Does it render in the floorplan card?
4. **Check console:** F12 → Console → any errors?

---

## CSS Development Rules

### Selectors Must Match SVG IDs

Every CSS selector should target an SVG element that actually exists.

**✅ Good:**
```css
#kitchen-floor { fill: #1a1f27; }
```
(Matches `<rect id="kitchen-floor" ... />`)

**❌ Bad:**
```css
#kitchen_main_floor { fill: #1a1f27; }
```
(No matching SVG element!)

### Use Classes for Dynamic States

Home Assistant applies CSS classes dynamically. Use classes for state-based styling.

**✅ Right:**
```css
.light-on #kitchen-floor { fill: #fff5c0; }
.light-off #kitchen-floor { fill: #1a1f27; }
```

**❌ Wrong:**
```css
#kitchen-floor { fill: light ? '#fff5c0' : '#1a1f27'; }
```
(CSS doesn't support JavaScript-like logic)

### Avoid `!important`

Use CSS specificity instead of `!important`.

**✅ Better:**
```css
.light-on #kitchen-floor { fill: #fff5c0; }
```

**❌ Avoid:**
```css
#kitchen-floor { fill: #1a1f27 !important; }
```

---

## YAML Development Rules

### Indentation Matters

YAML uses indentation for structure. Must be consistent (spaces, not tabs).

```yaml
# ✅ Correct (2-space indentation)
type: custom:floorplan-card
config:
  image: /local/floorplan/apartment_floorplan.svg
  stylesheet: /local/floorplan/apartment_floorplan.css
  rules:
    - entity: light.lampochki_na_kukhne
      element: room-kitchen
```

```yaml
# ❌ Wrong (inconsistent indentation)
type: custom:floorplan-card
config:
 image: /local/floorplan/apartment_floorplan.svg
  stylesheet: /local/floorplan/apartment_floorplan.css
```

### Validate YAML Syntax

Use an online YAML validator or Home Assistant's built-in editor validation:
1. Edit dashboard card manually
2. Home Assistant will show syntax errors immediately
3. Fix and save

### Class Template Logic

The `class_template` field contains JavaScript that determines which CSS classes apply.

**✅ Simple & clear:**
```yaml
class_template: |
  if (entity.state === 'on') return 'light-on';
  return 'light-off';
```

**✅ Brightness-aware:**
```yaml
class_template: |
  if (entity.state === 'on') {
    const brightness = entity.attributes.brightness || 0;
    if (brightness < 85) return 'is-on brightness-low';
    if (brightness < 170) return 'is-on brightness-medium';
    return 'is-on brightness-high';
  }
  return 'is-off';
```

**❌ Too complex:**
```yaml
class_template: |
  return entity.state === 'on' && entity.attributes.brightness > 128 
    && entity.attributes.color_temp < 4000 ? 'on-warm-bright' 
    : entity.state === 'off' && entity.last_changed < 300 ? 'recently-off' 
    : 'default';
```
(Hard to understand, maintain, and test)

---

## Git Workflow

### Commit Guidelines

**One feature/fix per commit:** Don't lump multiple changes together.

**Good commit messages:**
- `fix: correct kitchen room position`
- `feat: add dynamic light overlays`
- `docs: update installation guide with entity ID examples`
- `style: improve dark theme contrast`

**Bad commit messages:**
- `update files`
- `fix bugs`
- `changes`
- `final update!!!`

### Commit Scope

Keep commits **focused and testable:**

**✅ Good scope:**
```
fix: update living room light entity ID in YAML
- Changed light.living_room → media_player.tv (no smart light)
- Added comment explaining temporary state
- Updated CHANGELOG
```

**❌ Too broad:**
```
refactor: complete overhaul of everything
- Changed colors, layouts, entity ids, documentation
- Added new features and removed old ones
- Broke and fixed multiple things
```

### Before Committing

1. **Hard-test in Home Assistant** — visual and functional
2. **Review your changes** — try to break it yourself
3. **Check for secrets** — no passwords, tokens, keys
4. **Update docs** — README, CHANGELOG if needed
5. **Keep it small** — can someone review this in 5 minutes?

---

## Testing Checklist

After each SVG, CSS, or YAML change:

- [ ] **Syntax valid:** No XML/YAML parse errors
- [ ] **SVG renders:** `/local/floorplan/apartment_floorplan.svg` loads in browser
- [ ] **Card displays:** Floorplan card shows up in dashboard
- [ ] **Interactions work:** Tapping rooms triggers correct actions
- [ ] **Lights toggle:** Clicking light room toggles the light
- [ ] **Light colors respond:** Overlay shows when light is on
- [ ] **No console errors:** F12 → Console tab → no red errors
- [ ] **Mobile view:** Test on phone or mobile viewport (375px)
- [ ] **Tablet view:** Test on iPad or tablet viewport (800px)
- [ ] **Hard-refresh:** Ctrl+Shift+R (not just F5)

---

## Debugging Tips

### Card not showing up

1. Check resource: Settings → Dashboards → Resources → `/hacsfiles/ha-floorplan/floorplan.js`?
2. Restart Home Assistant
3. Clear browser cache
4. Check browser console (F12) for errors

### SVG shows 404 error

1. File exists at `/config/www/floorplan/apartment_floorplan.svg`?
2. Can you access it directly: `/local/floorplan/apartment_floorplan.svg`?
3. Permissions correct? (`chmod 644` or similar)
4. No special characters in filename

### Tapping room does nothing

1. Entity ID exists? Settings → Entities → search
2. Entity ID matches YAML exactly (copy/paste to be sure)
3. YAML indentation correct?
4. CSS class applied? F12 → Inspector → click room → check classes
5. No console JavaScript errors

### Light colors not changing

1. Light entity exists and is dimmable
2. CSS class applied correctly? F12 → Inspector
3. Brightness attribute populated? Look at entity details in HA
4. CSS rules match class names

### Styling looks wrong

1. Hard-refresh browser: Ctrl+Shift+R
2. Clear browser cache entirely
3. Check CSS file permissions on HA instance
4. Verify CSS path in YAML: `/local/floorplan/apartment_floorplan.css`

---

## Performance Optimization

### SVG File Size

Keep under 50KB (currently ~2KB). Check with:
```bash
ls -lh floorplan/apartment_floorplan.svg
```

If growing:
- Remove unused shapes
- Use simpler paths
- Compress with `svgo` if needed

### CSS & Animations

- Use `transition` for state changes (efficient)
- Avoid complex filters on every element
- Use GPU-friendly properties: `opacity`, `transform`
- Not recommended: expensive filters like `blur` on many elements

### YAML Complexity

- Keep class templates simple and fast
- Avoid expensive calculations in `class_template`
- Test with 20+ entities — still responsive?

---

## Security Considerations

### Entity ID Validation

Before committing YAML:
- Verify entity IDs don't leak personal info
- No phone numbers in device names
- No home address in entity labels

### File Permissions

- SVG/CSS must be readable by
 Home Assistant (`644` permissions)
- No world-writable backup files in `/config/`

---

## Code Review Checklist

Before accepting a PR or committing your own changes:

- [ ] SVG is valid XML
- [ ] CSS selectors match SVG IDs
- [ ] YAML syntax is correct
- [ ] No hardcoded secrets/credentials
- [ ] Entity IDs exist in HA
- [ ] Changes tested in Home Assistant
- [ ] Documentation updated (README, CHANGELOG, docs)
- [ ] No unnecessary files committed (backup, temp, etc.)
- [ ] One focused change per commit
- [ ] Commit message is clear

---

## Useful Resources

- [SVG Tutorial](https://developer.mozilla.org/en-US/docs/Web/SVG)
- [CSS Guide](https://developer.mozilla.org/en-US/docs/Learn/CSS)
- [YAML Spec](https://yaml.org/spec/)
- [ha-floorplan GitHub](https://github.com/ExperienceLovelace/ha-floorplan)
- [Home Assistant Dev Docs](https://developers.home-assistant.io/)

---

## Need Help?

- See [design_notes.md](design_notes.md) for design philosophy
- See [installation.md](installation.md) for setup help
- Check [../README.md](../README.md) for overview
- Open an issue on GitHub with details

---

**Last updated:** April 2026 | Version: 0.2.0

