# Design Notes

This document records the design decisions and philosophy behind the floorplan to help maintain consistency and guide future changes.

---

## Overall Philosophy

### UX Over Accuracy
- This is **not** a CAD-accurate architectural drawing
- **UX clarity and usability are prioritized over geometric perfection**
- Rooms must be **readable and tappable** on phones, not perfectly scaled
- The goal is a functional smart-home UI, not a floor plan for contractors

### Simplicity First
- Show only what is **useful in daily use**
- Avoid clutter and unnecessary complexity
- Users should understand the floorplan at a glance
- One interaction (tap room → toggle light) should work intuitively

---

## Visual Design (v0.2.0)

### Dark Theme

**Why dark?**
- Home Assistant dashboards often run on **wall-mounted tablets** viewed at night
- Dark theme **reduces eye strain** during evening use
- Looks **modern and professional** (matches contemporary smart-home aesthetics)
- Works well with light overlays (glow effects)

**Color palette:**
- Background: `#0f1419` (very dark blue-gray)
- Room floors: `#1a1f27` (dark blue-gray, slightly lighter)
- Borders: `#2a3142` (muted blue-gray)
- Text: `#c0c7d4` (soft gray-blue, readable on dark)
- Light overlay: `#ffd700` (warm yellow, soft glow)

**Transitions:**
- All color changes use `transition: 0.3s-0.4s ease`
- Smooth, not jarring
- Feels responsive but not frenetic

### Soft Shadows & Rounded Shapes

**Shadows:**
- Use CSS `drop-shadow(0 4px 12px rgba(0, 0, 0, 0.4))` for depth
- Creates subtle elevation without looking 3D
- Shadows help distinguish rooms from background

**Rounded Corners:**
- Room rectangles use `rx="16" ry="16"` border-radius
- Creates **friendly, human appearance**
- Avoids harsh "technical schematic" feel
- Still clearly shows room boundaries

**No Harsh Contrasts:**
- Avoid bright borders or neon fills
- Use muted, calm colors throughout
- Aim for modern minimalism, not high-visibility UI

---

## Room Layout (v0.2.0)

### 2×2 Grid + Center Corridor

```
┌────────────────┬────────────────┐
│    Kitchen     │    Bedroom     │  Top
│  (top-left)    │  (top-right)   │
├────────────────┼────────────────┤
│   Bathroom     │  Living Room   │  Bottom
│  (bottom-left) │ (bottom-right) │
└────┬──────────┬┴┬──────────┬────┘
     │ Corridor │ │ Corridor │       Center
     │  (Entry) │ │ (center) │       Corridor Zone
     └─────────────────────────┘
```

**Rationale:**
- Matches real apartment layout (entrance at bottom, left = kitchen+bathroom, right = living+bedroom)
- Center corridor is vertical flow from entry through apartment
- 2×2 simplicity: easy to understand, remember, and navigate
- Corridor acts as central hub for shared systems (lights, motion detection, entry camera)

### Room-by-Room Notes

| Room | Smart Devices | Layout | Design |
|---|---|---|---|
| **Kitchen** | Light (dimmable) | Top-left | Main cooking area, important room |
| **Bathroom** | None (for now) | Bottom-left | Compact, static placeholder |
| **Bedroom** | Light (dimmable), sensors | Top-right | Bedroom light, temp/humidity |
| **Living Room** | Media (TV) | Bottom-right | Main living area, media control |
| **Corridor** | Light, motion, camera | Center | Entry, shared systems, hub |

**Design considerations:**
- Kitchen and bedroom are **priority rooms** (top), larger tap targets
- Bathroom is **compact** (bottom-left), fewer interactions
- Living room is **secondary** (bottom-right), mainly for media display
- Corridor is **always visible** (center), motion/entry context

---

## Light Overlays & Dynamic States

### Architecture

Each room has two layers:
1. **Base floor shape** (`#kitchen-floor`, etc.) — always visible
2. **Light overlay shape** (`#room_kitchen_light`, etc.) — hidden until light is on

### Behavior

**Light OFF:**
- Overlay opacity: `0` (invisible)
- Room floor stays at base color: `#1a1f27`
- Appearance: dark, quiet, "off"

**Light ON (static):**
- Overlay opacity: `0.25` (soft glow)
- Warm yellow fill: `#ffd700`
- SVG `mix-blend-mode: screen` adds glow effect
- Appearance: warm light, cozy, "on"

**Light ON (future brightness support):**
- Low brightness (0–85): opacity `0.1` (barely visible)
- Medium brightness (85–170): opacity `0.2` (moderate glow)
- High brightness (170–255): opacity `0.3` (full glow)
- Smoothly transitions as brightness slider moves

### Future: RGB & Color Temperature

**RGB support:**
- Overlay fill could change based on light color attribute
- Purple light → `#b366ff` fill
- Blue light → `#66b3ff` fill
- Red light → `#ff6666` fill

**Color temperature:**
- Warm white (>3000K) → `#ffa060` (amber)
- Cool white (<4000K) → `#a0d8ff` (blue)
- Mixed/tuned → dynamic based on mired value

**Not yet implemented** but CSS architecture supports it.

---

## Roborock Vacuum Integration

### Philosophy

**Why NOT use Roborock map as background?**
1. **Complexity:** Live map updates add complexity and performance overhead
2. **Visual distraction:** Changing map can obscure room boundaries
3. **Use case mismatch:** Room controls ≠ robot vacuum tracking
4. **Simplicity:** Clean SVG floorplan as main UI, separate status layer for robot

**Why investigate live position?**
- Shows where robot is cleaning → useful context
- Robot position on rooms could highlight "busy" areas
- Not essential for MVP, but nice enhancement

### Current Implementation (v0.2.0)

- Roborock device shown in **center corridor zone** as placeholder
- Visual indicator (circle with pulse animation) marks robot position
- Status text shows robot state: "docked", "cleaning", "returning", etc.
- No live position extraction yet (v0.4.0 planned)

### Future Investigation (v0.4.0)

**Question:** Can we extract live robot position from `image.roborock_qrevo_curv_series_dom_gabelsbergerstr`?

**Investigation plan:**
1. Check if Roborock entity contains coordinate data
2. Research ha-floorplan Roborock map capabilities
3. Try to overlay position indicator in real-time
4. Fallback: static status only if live position not feasible

**Success criteria:**
- Robot position updates smoothly on floorplan
- No performance degradation
- Status remains visible even without live position

---

## Layout Direction & Vertical Optimization

### Mobile-First Design

**Why vertical (600×1200) instead of horizontal (1200×800)?**

1. **Most users view on phones:** Home Assistant companion app is portrait-first
2. **Tablet optimization:** Most mounted tablets are portrait-oriented
3. **Full-screen use:** Vertical layout uses more of the visible screen space
4. **No horizontal scrolling:** Rooms visible without panning

### ViewBox & Scaling

**SVG ViewBox:** `600 1200`
- Units are abstract (not pixels or cm)
- Scales to fit container on desktop, phone, tablet
- Maintains aspect ratio automatically

**Margins & Spacing:**
- Top rooms (kitchen/bedroom): y=30–310
- Corridor zone: y=950–1150
- Bottom rooms (bathroom/living): y=360–660
- Horizontal padding: 30px on each side (30–570 width range)

**Room sizes:**
- All rooms: ~240px width, 260–300px height
- Large enough to tap comfortably (48px+ on phone screen)
- Readable text at various zoom levels

### Responsive Behavior

**Desktop (1200px wide):** Floorplan large, lots of space
**Tablet (800px wide):** Floorplan medium, still readable
**Phone (375px wide):** Floorplan small but usable, text still readable

SVG scales automatically — no media queries needed for basic display. CSS media queries only for text size tweaks.

---

## What to Avoid (Anti-Patterns)

### ❌ Don't Do This

1. **Clutter the floorplan with every entity**
   - Not every sensor needs to be shown
   - Pick the most useful ones (lights, motion, etc.)
   - Keep it clean

2. **Hardcode colors or styles in SVG**
   - All styling goes in `apartment_floorplan.css`
   - SVG should only have structure (`<rect>`, `<g>`, `id` attributes)
   - Easier to theme and maintain

3. **Embed large inline `<style>` in SVG**
   - Use external CSS file instead
   - Separates structure from presentation
   - Consistent with web design best practices

4. **Use overly complex class names**
   - Keep it simple: `light-on`, `light-off`, `brightness-low`
   - Avoid: `kitchen_light_state_bright_and_warm_dimmable_mode_active`

5. **Change SVG element IDs**
   - Once an `id` is in YAML, it's stable
   - Changing `id="kitchen-floor"` to `id="kitchen_main_floor"` breaks YAML
   - If you must rename, update YAML too

---

## Entity ID Philosophy

### Preserve Existing IDs

**Why?**
- IDs created by user's devices (sensors, lights, etc.)
- Changing them requires updating automations, scripts, etc.
- Breaking change with no benefit

**Rule:** Only change entity IDs if there's a compelling reason
- Not: "I like short names better"
- Is okay: "Entity no longer exists, replaced by newer device"

### Meaningful IDs

**Good:**
- `light.lampochki_na_kukhne` (kitchen lights, Russian name)
- `sensor.datchik_temperatury_temperature` (temperature sensor)
- `vacuum.roborock_qrevo_curv_series` (vacuum model name)

**Bad:**
- `light.light_1` (not descriptive)
- `sensor.s1` (unclear what it measures)
- `switch.relay_kitchen_new` (confusing versioning)

### No Hardcoding

Never put entity IDs directly in SVG or CSS. They belong in:
- YAML: `floorplan_card.yaml` (where they're configured)
- CSS: via class names applied by ha-floorplan logic

---

## Accessibility & Inclusive Design

### Touch Targets

- All rooms should be **≥44px** in any dimension on a mobile screen at reasonable zoom
- Text should be **readable at 14px+** font size
- Labels should have **good contrast** against background

### Keyboard Navigation

- Future goal (v0.5.0): Tab between rooms, Enter/Space to activate
- When implemented, avoid relying only on color to indicate state

### Screen Readers

- Each interactive room should have `aria-label` attribute
- Elements should have semantic HTML structure where possible
- Alternative text descriptions for icons/symbols

---

## Performance Considerations

### SVG Optimization

- Keep SVG file size < 50KB (currently ~2KB)
- Use simple shapes (`<rect>`, `<circle>`) not complex paths
- Avoid filters on every element (use strategically for shadows only)

### CSS Complexity

- Keep class names and selectors simple
- Avoid deep nesting or overly specific selectors
- Use CSS animations not full JavaScript where possible

### Browser Rendering

- CSS `transition` properties preferred over full redraws
- `mix-blend-mode: screen` for light overlays (efficient)
- Animations should use GPU-friendly properties (opacity, transform)

---

## Version History & Evolution

### v0.1.0 (Initial)
- Basic 4-room layout, placeholder styling

### v0.2.0 (Current) ✅
- Correct room positioning
- Dark theme & modern aesthetics
- Added corridor/entry zone
- Mobile-first vertical layout
- Comprehensive documentation

### v0.3.0 (Planned)
- Brightness-aware lighting
- RGB/color temperature support

### v0.4.0 (Planned)
- Roborock live position investigation
- Vacuum status display improvements

### v0.5.0 (Planned)
- Animations, sensor labels, scene buttons
- Improved accessibility

---

## Summary

The floorplan prioritizes **clear, friendly, intuitive UX** over architectural accuracy. Dark theme, soft shapes, smooth interactions, and thoughtful layout create a modern smart-home dashboard that works well on phones and wall-mounted tablets. Simple geometry (2×2 grid) makes the floorplan easy to understand and extend with new devices.

---

**Last updated:** April 2026 | Version: 0.2.0

