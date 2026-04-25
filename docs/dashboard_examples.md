# Dashboard Examples

This guide shows how to use the floorplan in different dashboard configurations for various use cases.

---

## Example 1: Standard Dashboard Card (Default)

A normal Home Assistant dashboard with the floorplan as one card among others.

### Setup

1. Create or edit any dashboard
2. Add a Manual card
3. Paste the contents of `floorplan_card.yaml`

```yaml
# Standard dashboard (mix of cards)
type: grid
cards:
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]
  
  - type: weather-forecast
    entity: weather.home
  
  - type: entities
    entities:
      - light.lampochki_na_kukhne
      - light.smart_ceiling_light
```

### Result

✅ **Pros:**
- Works out-of-the-box
- Mix with other cards (weather, entities, history, etc.)
- Easy to add other dashboards below

❌ **Cons:**
- Floorplan competes for space with other cards
- May look small on desktop
- Scrolling needed on mobile if many cards

### When to use

- **Personal use:** Several dashboards, each with different focus
- **Quick reference:** Want to see weather + lights + floorplan
- **Smart home hub:** Multiple card types on one screen

---

## Example 2: Fullscreen / Panel View ⭐ RECOMMENDED

A dedicated fullscreen dashboard with **only** the floorplan.

Perfect for **wall-mounted tablets** or fullscreen smart displays.

### Setup

**Option A: Create a new Panel view dashboard**

1. Settings → Dashboards
2. Create new dashboard, select view type: **Panel** (not Cards)
3. Add the floorplan card as the only element

```yaml
# Full panel dashboard (floorplan only, fullscreen)
type: custom:floorplan-card
config:
  image: /local/floorplan/apartment_floorplan.svg
  stylesheet: /local/floorplan/apartment_floorplan.css
  rules: [...]
```

### Result

✅ **Pros:**
- Uses **full screen space** (no waste on UI chrome)
- Looks **modern and professional** on wall tablets
- Large, easy-to-tap rooms on mobile
- Minimal distraction

❌ **Cons:**
- Only one dashboard at a time (need to switch for weather, etc.)
- Less suitable if you have many different controls

### When to use

- **Wall-mounted tablet:** Kitchen, living room display
- **Master control:** Single dashboard for all home controls
- **Focus mode:** Want minimal interface
- **Immersive:** Show off the floorplan aesthetic

### Pro Tips

**Set as default dashboard:**
1. Dashboard menu → three dots
2. Select "Make this the default dashboard"
3. Next time you visit Home Assistant, this dashboard loads

**Combine with automations:**
- Set dashboard to fullscreen before guests arrive
- Switch to single room view for "focus mode"
- Use device triggers to auto-select dashboard on time/device

---

## Example 3: Two-Column Layout

Split screen: Left = floorplan, Right = controls/info

### Setup

```yaml
# Two-column layout
type: grid
columns: 2
cards:
  # Left column: floorplan
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]

  # Right column: controls
  - type: vertical-stack
    cards:
      - type: entities
        title: Lights
        entities:
          - light.lampochki_na_kukhne
          - light.smart_ceiling_light
          - light.hall_strip
      
      - type: weather-forecast
        title: Current Conditions
        entity: weather.home
      
      - type: media-control
        entity: media_player.tv
```

### Result

✅ **Pros:**
- Floorplan takes 50% of screen (still readable)
- Other controls visible side-by-side
- Good use of desktop/tablet space
- No scrolling needed (on wide screens)

❌ **Cons:**
- Too cramped on phones (< 600px wide)
- Floorplan gets smaller on larger monitors

### When to use

- **Desktop dashboard:** Computer monitor or large tablet
- **Split view:** Want floorplan + info/controls visible simultaneously
- **Hybrid need:** Some automation + floorplan display

### Media Query Consideration

On mobile (< 600px), stack vertically instead:

```css
@media (max-width: 600px) {
  /* Stack columns vertically */
  /* Cards will automatically reflow */
}
```
(Home Assistant handles this automatically)

---

## Example 4: Room-Focused Views (Advanced)

Multiple dashboards, each focusing on a different room.

### Setup

Create separate dashboard pages:

**Dashboard: "Kitchen"**
```yaml
type: grid
cards:
  # Floorplan (show all rooms for context)
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]

  # Kitchen-specific controls
  - type: entities
    title: Kitchen Appliances
    entities:
      - light.lampochki_na_kukhne
      - switch.w601_switch_1
      - switch.teapot_start
      - sensor.teapot_current_temperature
```

**Dashboard: "Bedroom"**
```yaml
type: grid
cards:
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]

  - type: entities
    title: Bedroom
    entities:
      - light.smart_ceiling_light
      - sensor.datchik_temperatury_temperature
      - sensor.datchik_temperatury_humidity
```

### Navigation

Add dashboard tabs:
1. Settings → Dashboards → select dashboard
2. Click three dots → "Manage dashboard" → select "Show in sidebar"
3. Reorder tabs as needed

### Result

✅ **Pros:**
- Organized by room/context
- Quick switch between different views
- Each page shows floorplan + room-specific controls

❌ **Cons:**
- More dashboards to manage
- Repetition (floorplan appears on each page)

### When to use

- **Large homes:** Many rooms, can't fit on one dashboard
- **Detailed control:** Per-room automations, scenes, history
- **Different user roles:** Parents vs. guests see different stuff

---

## Example 5: Mobile-Optimized Dashboard

Designed for smartphone (portrait) viewing.

### Setup

```yaml
# Mobile-first dashboard
type: vertical-stack
cards:
  # Header
  - type: markdown
    content: |
      # 🏠 Home
      April 25, 2026

  # Main: Floorplan (full width)
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]

  # Quick controls below (vertical stack)
  - type: entities
    title: Quick Access
    entities:
      - light.lampochki_na_kukhne
      - light.smart_ceiling_light
      - vacuum.roborock_qrevo_curv_series
```

### Mobile CSS

Ensure responsive floorplan:

```css
/* Mobile (default) */
#floorplan {
  width: 100%;
  height: auto;
  aspect-ratio: 1 / 2;  /* Maintains 600×1200 ratio */
}

@media (min-width: 600px) {
  /* Tablet / larger */
  #floorplan {
    max-width: 600px;
  }
}
```

### Result

✅ **Pros:**
- Full-width on mobile
- Text readable on small screen
- Single-column layout (no horizontal scroll)
- Floorplan naturally sized at 600×1200 aspect ratio

❌ **Cons:**
- Tall on desktop (may need scrolling)
- Limited controls visible at once

### When to use

- **Companion app:** Primary interface on phone
- **Mobile-first:** Most users on phones
- **Minimal chrome:** Want clean, focused dashboard

---

## Example 6: Scene Selection Dashboard

Dashboard with scene/automation quick buttons above floorplan.

### Setup

```yaml
# Scenes + floorplan
type: vertical-stack
cards:
  # Scene buttons (quick access)
  - type: horizontal-stack
    cards:
      - type: custom:button-card
        entity: scene.evening_mode
        name: Evening
        tap_action:
          action: call-service
          service: scene.turn_on
          service_data:
            entity_id: scene.evening_mode
      
      - type: custom:button-card
        entity: scene.watching_movie
        name: Movie
      
      - type: custom:button-card
        entity: scene.good_night
        name: Goodnight

  # Floorplan (main display)
  - type: custom:floorplan-card
    config:
      image: /local/floorplan/apartment_floorplan.svg
      stylesheet: /local/floorplan/apartment_floorplan.css
      rules: [...]

  # Status info
  - type: entities
    entities:
      - vacuum.roborock_qrevo_curv_series
```

### Result

✅ **Pros:**
- Quick scene access (one tap)
- Floorplan shows current state after scene activates
- Professional, organized layout

❌ **Cons:**
- Requires custom button card (HACS)
- More complex YAML config

### When to use

- **Automation enthusiast:** Have scenes configured
- **Tablet display:** Quick access to common modes
- **Complex workflows:** Multiple grouped actions

---

## Example 7: Minimalist Dashboard (Less is More)

Only the floorplan, no other cards or text.

### Setup

```yaml
# Minimal: floorplan only
type: custom:floorplan-card
config:
  image: /local/floorplan/apartment_floorplan.svg
  stylesheet: /local/floorplan/apartment_floorplan.css
  rules: [...]
```

### Styling

Remove Home Assistant UI chrome:
1. Edit dashboard (pencil icon)
2. Three dots → "Edit dashboard"
3. Toggle:
   - **Title:** Off
   - **Background:** Custom color (dark)
   - **Show toolbar:** Off (in some themes)

### Result

✅ **Pros:**
- Ultra-clean, minimalist aesthetic
- Maximum focus on floorplan
- Looks like a custom smart home app

❌ **Cons:**
- No fallback to other controls
- Can't access other Home Assistant features from this view

### When to use

- **Wall art:** Display as ambient dashboard
- **Framed tablet:** Dedicated device for floorplan
- **Aesthetic priority:** Form over function

---

## Mobile / Responsive Best Practices

### Phone (375–425px wide)

✅ **Do:**
- Use fullscreen/panel view (floorplan uses all available width)
- Stack controls vertically below floorplan
- Use large touch targets (48px+ buttons)
- Keep text readable (14px+ font)

❌ **Don't:**
- Use 2+ column layouts (too cramped)
- Resize floorplan manually (let SVG scale)
- Rely on hover states (mobile has no hover)

### Tablet (768–1024px wide)

✅ **Do:**
- Use two-column layout (floorplan + sidebar)
- Optimize for portrait AND landscape
- Use medium text (12–16px)

❌ **Don't:**
- Hard-code fixed widths
- Create huge tap targets (wastes space)

### Desktop (1200px+)

✅ **Do:**
- Use large floorplan (takes up more of screen)
- Multiple columns for controls/info
- Add additional dashboards for more features

❌ **Don't:**
- Make floorplan tiny (use responsive sizing)
- Ignore other card types (mix useful controls)

---

## Performance Tips

### For Wall-Mounted Tablet

1. **Fullscreen/Panel view** → saves battery (no UI updates)
2. **Set auto-reload:** Edit dashboard → three dots → Reload dashboard every 30 min
3. **Reduce motion:** Disable animations if needed
4. **Brightness:** Set to a comfortable level for extended viewing

### For Mobile Companion App

1. **Optimized dashboard:** Minimize number of cards
2. **Connection:** Use local WiFi (faster than internet)
3. **Cache:** Home Assistant caches CSS/SVG automatically
4. **Update frequency:** Floorplan updates realtime with light changes

---

## Troubleshooting Dashboard Issues

| Issue | Cause | Fix |
|---|---|---|
| Floorplan too small | Using cards grid with many items | Switch to panel view or reduce cards |
| Text unreadable on mobile | Floorplan scaled down too much | Use fullscreen view, check responsive CSS |
| Colors not updating | CSS not loading | Hard-refresh browser (Ctrl+Shift+R) |
| Tap not working | Entity ID wrong | Check Settings → Entities, update YAML |
| Dashboard loads slowly | Too many cards | Reduce number of cards, use simpler controls |
| Old version showing | Cache issue | Clear browser cache completely |

---

## Summary

| Use Case | Recommended View | Why |
|---|---|---|
| **Wall tablet (fullscreen)** | Panel view | Uses full space, minimal UI |
| **Smart phone** | Fullscreen or vertical-stack | Mobile-optimized layout |
| **Desktop monitor** | Two-column grid | Floorplan + controls visible |
| **Quick info** | Standard dashboard mix | Combine with weather, time, etc. |
| **Room-focused** | Multiple dashboards | One per room with specific controls |
| **Aesthetic display** | Minimalist | Floorplan only, clean look |

---

## Next Steps

- Pick a layout from above that fits your use case
- Test on your devices (phone, tablet, desktop)
- Customize colors and controls as needed
- Refer to [../README.md](../README.md) for more tips

---

**Last updated:** April 2026 | Version: 0.2.0
