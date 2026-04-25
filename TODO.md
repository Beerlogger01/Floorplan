# TODO

This document tracks planned features and improvements for future releases.

---

## v0.2.0 ✅ COMPLETED

- ✅ Correct room layout (kitchen/bathroom left, bedroom/living room right)
- ✅ Add corridor/entry zone
- ✅ Improved visual design (dark theme, soft shadows, rounded shapes)
- ✅ Vertical/fullscreen-friendly layout (viewBox 600x1200)
- ✅ Dynamic room lighting overlay architecture (base prepared)
- ✅ Real entity bindings (lampochki_na_kukhne, smart_ceiling_light, hall_strip, etc.)
- ✅ Comprehensive documentation and examples
- ✅ Roborock placeholder / status zone

---

## v0.3.0 📋 PLANNED

### Brightness Support for Room Lighting
- [ ] Implement brightness calculation based on `entity.attributes.brightness`
- [ ] Update CSS to scale brightness levels (low 0-85, medium 85-170, high 170-255)
- [ ] Test with real lights in Home Assistant
- [ ] Add smooth transitions when brightness changes

### RGB / Color Temperature Support
- [ ] Research SVG color matrix filters for RGB blending
- [ ] Prepare CSS classes for warm/cool color overlays
- [ ] Create color mapping logic in floorplan_card.yaml
- [ ] Test with RGB-capable lights (if available)

### Living Room Light Entity
- [ ] Verify if `light.audiosistema_outlet` should remain as placeholder
- [ ] Identify real living room light entity (if it exists)
- [ ] Update YAML and SVG accordingly
- [ ] Document the decision in design_notes.md

### Enhanced Sensor Labels
- [ ] Add temperature/humidity display in corridor/bedroom
- [ ] Show motion sensor status (motion / no motion)
- [ ] Display door/window sensor states
- [ ] Format numbers with units (°C, %)

---

## v0.4.0 📋 PLANNED

### Roborock Live Position Investigation

#### Phase 1: Data Exploration
- [ ] Verify if `image.roborock_qrevo_curv_series_dom_gabelsbergerstr` contains coordinate data
- [ ] Check Roborock API documentation for position extraction methods
- [ ] Research ha-floorplan Roborock map element capabilities
- [ ] Document findings in design_notes.md

#### Phase 2: Live Position (if feasible)
- [ ] Extract robot coordinates from live map entity
- [ ] Overlay position indicator on floorplan
- [ ] Add trajectory/history visualization (optional)
- [ ] Add status labels (cleaning, docked, returning, paused)

#### Fallback: Manual Status
- [ ] If live position not possible, use status sensor only
- [ ] Show vacuum state with visual indicator
- [ ] Keep status always visible in center zone

### Roborock Controls (Optional)
- [ ] Add hold_action or button to start vacuum
- [ ] Add stop/pause buttons (if safe)
- [ ] Document vacuum control flow

---

## v0.5.0 📋 PLANNED

### Animations & Motion
- [ ] Add subtle entrance animations on page load
- [ ] Add hover animations for room tap targets
- [ ] Add state change animations (light turning on/off)
- [ ] Add pulse animation for active/cleaning state

### Scene & Automation Buttons
- [ ] Add scene buttons (e.g., "Good Night" → turn off lights, close blinds)
- [ ] Add automation triggers (e.g., "Movie Mode")
- [ ] Create custom button styling
- [ ] Wire to Home Assistant automation service calls

### Improved Responsive Design
- [ ] Test on various phone sizes (320px - 480px - 768px)
- [ ] Test on tablets and large displays
- [ ] Add media queries for extreme sizes
- [ ] Ensure text remains readable
- [ ] Ensure tap targets remain >44px (accessibility)

### Accessibility Improvements
- [ ] Add ARIA labels to all interactive elements
- [ ] Add keyboard navigation support
- [ ] Test with screen readers
- [ ] Add high contrast mode option (optional)

### Additional Sensor Displays
- [ ] Add weather widget or external info panel
- [ ] Show current time and date
- [ ] Add battery levels for wireless sensors
- [ ] Create expandable sensor drawer (future UI enhancement)

---

## v1.0.0 📋 FUTURE

### Stable Release
- [ ] Complete all v0.3.0 - v0.5.0 tasks
- [ ] Comprehensive testing on real Home Assistant instances
- [ ] User feedback collection and fixes
- [ ] Performance optimization
- [ ] Full documentation (README, development guide, troubleshooting)

### Nice-to-Have (Post v1.0.0)
- [ ] Multi-floor support (switch between floors)
- [ ] Customizable room colors and layouts
- [ ] Theme selector (light / dark / high contrast)
- [ ] Local language support (i18n)
- [ ] Vacation modes and automations display

---

## Known Investigations

These are items that need research or decision-making:

### 1. Roborock Live Map Integration
**Question:** Can we extract live robot position from `image.roborock_qrevo_curv_series_dom_gabelsbergerstr`?

**Current Status:** Unknown. The entity exists, but data format needs investigation.

**Action:** v0.4.0 will focus on exploring this.

**Reference:** ha-floorplan may have built-in support for live map overlay, or we may need custom integration.

### 2. Living Room Light Entity
**Question:** Is `light.audiosistema_outlet` the main living room light or just an audio system?

**Current Status:** Uncertain. It's marked as static in current YAML.

**Action:** Confirm with user or HA instance. If it's not the main light, find the real entity.

**Reference:** See floorplan_card.yaml comments (line ~112).

### 3. Multi-Room Audio System
**Question:** Should we add media player controls for the audio system?

**Current Status:** Not in scope for v0.2.0. Future enhancement.

**Action:** Defer to post-v1.0.0.

### 4. Bathroom Smart Devices
**Question:** Are there any smart devices planned for the bathroom?

**Current Status:** Currently static. No smart entities defined.

**Action:** Once a device is added (e.g., smart fan, heater), update SVG and YAML.

---

## Developer Guidelines

When working on TODO items:

1. **Test locally** before committing changes
2. **Hard-refresh browser** after updating CSS/SVG
3. **Check entity IDs** match your Home Assistant instance
4. **Document new features** in README and design_notes.md
5. **One feature per commit** (keep commits small and reviewable)
6. **Update CHANGELOG.md** when releasing a new version

---

## Version Release Checklist

Before releasing a new version:

- [ ] All tasks in the version are completed
- [ ] Code peer-reviewed (or self-reviewed thoroughly)
- [ ] All entity IDs match current HA instance
- [ ] No secrets/credentials in files
- [ ] Documentation updated (README, CHANGELOG, design notes)
- [ ] Hard-tested in Home Assistant (visual + functional)
- [ ] Screenshots/video recorded for release notes (optional)
- [ ] Version number updated in CHANGELOG and code comments
- [ ] Git tag created: `git tag v0.3.0 && git push --tags`

---

**Last updated:** April 2026

