# Foundry logo assets

**Real vector files** extracted unmodified from `.assets/foundry-brand-guide.html` (the source
brand guide, v1.0, February 2026). These are the authentic mark — not a reconstruction.

The mark is **two overlapping circles** representing the intersection of human need and
technological capability. The intersection is filled with a diagonal hatch pattern (rotated 60°)
in **Current** (`#4E6A78`) on light backgrounds, or **Copper** (`#BF7E4A`) on dark/primary
backgrounds — the one deliberate color shift that ties the mark to the full palette. Circle
outlines are Ink on light, Limestone on dark/primary.

## Files

| File | Contents | Use on |
|---|---|---|
| `foundry-logo-lockup.svg` | Full lockup — mark + "AUTOMATION FOUNDRY" wordmark, Ink fill | **light** backgrounds only (only variant provided by the source guide) |
| `foundry-icon-on-light.svg` | Icon only, Current hatch / Ink stroke | light backgrounds |
| `foundry-icon-on-dark.svg` | Icon only, Copper hatch / Limestone stroke | dark (Ink) backgrounds |
| `foundry-icon-on-primary.svg` | Icon only, Copper hatch / Limestone stroke | Current-colored (primary) backgrounds |

**Known gap:** the source guide does not provide a full lockup (icon + wordmark) recolored for
dark backgrounds — only the icon has light/dark/primary variants. Until an official asset lands,
compose a dark-background lockup by pairing `foundry-icon-on-dark.svg` with the wordmark reset in
Limestone type (Literata or Manrope, matching the lockup's proportions) rather than inventing a
new SVG; flag this as an assumption whenever it's used.

## Rules (from the source guide)

- Maintain clear space equal to the radius of one circle on all sides.
- Full lockup minimum size: 180px wide (digital) or 1.5" (print).
- Icon minimum size: 32px (digital) or 0.4" (print).
- Don't stretch, rotate, or recolor outside the approved variants above.
- Don't add shadows, gradients, or outlines beyond what's in the source files.
- Don't place on busy backgrounds without sufficient contrast/clear space.
- Don't separate the two circles from each other, or use the hatch pattern independently of the
  mark.

## In HTML

```html
<!-- light background, full lockup -->
<img src="assets/logo/foundry-logo-lockup.svg" alt="The Automation Foundry" height="64">

<!-- dark (Ink) background, icon only -->
<img src="assets/logo/foundry-icon-on-dark.svg" alt="The Automation Foundry" height="40">
```

SVGs inline cleanly too — paste the markup directly when a single self-contained file with no
external asset references is needed.
