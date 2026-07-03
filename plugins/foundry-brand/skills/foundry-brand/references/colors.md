# Colors — "Deep Current"

All values are **[VERIFIED]** — extracted directly from `.assets/foundry-brand-guide.html`
(Brand Guide v1.0, February 2026). The canonical copy lives in `assets/foundry-brand.css`
`:root`. Edit there first; mirror value changes here.

## Core palette

| Token | Hex | RGB | Role |
|---|---|---|---|
| `--foundry-limestone` | `#F0EFEB` | 240, 239, 235 | **60% — Base.** Primary light ground. Dominant background canvas. |
| `--foundry-mist` | `#D2D4D0` | 210, 212, 208 | **Neutral.** Borders, dividers, structural lines. |
| `--foundry-current` | `#4E6A78` | 78, 106, 120 | **30% — Primary.** Muted blue-teal. Navigation, headings, cards, supporting elements. Carries authority without coldness. |
| `--foundry-copper` | `#BF7E4A` | 191, 126, 74 | **10% — Accent.** Warm. CTAs, links, moments of warmth. Used *sparingly* — it's the one thing that keeps the palette from feeling corporate. |
| `--foundry-ink` | `#262B2E` | 38, 43, 46 | **Anchor.** Dark ground / near-black text. |

Supporting text tones (not part of the 5-swatch palette, but load-bearing for body copy):

| Token | Hex | Role |
|---|---|---|
| `--foundry-body` | `#555859` | Default body-copy color on light grounds. |
| `--foundry-body-light` | `#7A7D7F` | Secondary/muted body text, italic details. |
| `--foundry-dim` | `#9EA2A4` | Muted text on **dark** (Ink) surfaces. |

Faint tints (panel washes only — never body text):

| Token | Value | Role |
|---|---|---|
| `--foundry-current-faint` | `rgba(78,106,120,0.08)` | Positioning-box / callout-panel background. |
| `--foundry-copper-faint` | `rgba(191,126,74,0.10)` | Tertiary-tier badge background. |

## The 60/30/10 rule

This is the whole color system — internalize the ratio before anything else:

- **60% Limestone** — the dominant background canvas. Foundry compositions read as calm and
  spacious, not stark white or corporate navy.
- **30% Current** — navigation, headings, cards, supporting UI. The "authority" color.
- **10% Copper** — CTAs, links, moments of warmth. Deliberately restrained. If Copper is
  showing up everywhere, pull it back — its power comes from scarcity.
- **Mist and Ink** are structural, not part of the ratio: Mist for borders/dividers, Ink for
  anchor text and dark surfaces.

## Usage rules

**Do**
- Lead with Limestone as the base canvas for anything not explicitly a dark/hero surface.
- Use Current for structural/supporting elements — it's the "everywhere" color within the 30%.
- Spend Copper deliberately — one CTA, one link color, one warm accent per composition. Never
  as a fill for large surfaces.
- Use `--foundry-paper` (`#FFFFFF`, pure white) for "lifted" card surfaces that need to sit
  slightly above the Limestone base — the guide's quality cards and imagery tiers do this.

**Don't**
- Don't treat Copper as a general accent color available for backgrounds/large fills — it's a
  10% color by design.
- Don't use the tone-example / word-list supporting tints (the soft red/green/tan pairs) as
  general UI color — they exist only for voice-calibration callouts (see `voice.md`).
- Don't invert Limestone/Ink directly for a naive "dark mode." Adjust for context; see below.

## Dark mode

In dark mode, **Ink becomes the background surface**. Body text shifts to Limestone at roughly
87% opacity (`--foundry-dim` approximates this on `.foundry-surface--dark` in the CSS). Current
and Copper stay consistent across modes — they're the thread of identity connecting light and
dark. Never invert colors mechanically; adjust for the context while preserving the personality.

## Accessibility

Contrast ratios verified against Limestone:

| Pairing | Ratio | Guidance |
|---|---|---|
| Ink on Limestone | 12.4:1 | Passes WCAG AAA. Safe for all body text. |
| Current on Limestone | 4.6:1 | Passes AA for large text and UI components. |
| Copper on Limestone | 3.6:1 | Use at 18px+ only, or pair with Ink for smaller text. |

Always test pairings directly rather than assuming — these numbers are specific to the exact
hexes above, not the general "teal"/"orange" families they belong to.
