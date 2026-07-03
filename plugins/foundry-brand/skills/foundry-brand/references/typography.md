# Typography

**[VERIFIED]** — Literata + Manrope, extracted from `.assets/foundry-brand-guide.html`.
Tokens live in `assets/foundry-brand.css`.

## The two faces

### Literata — headings, display, quotes
A humanist reading serif. Brings warmth, craft, and quiet authority to anything that needs
gravity: headlines, section titles, pull quotes, the numbered principles, the tagline.

- **Weights used:** 300 (Light), 400 (Regular — the default for headings), 500 (Medium — H3 /
  subheads only).
- **Load:** `@import url('https://fonts.googleapis.com/css2?family=Literata:ital,opsz,wght@0,7..72,300;0,7..72,400;0,7..72,500;0,7..72,600;1,7..72,300;1,7..72,400&display=swap');`
- **CSS:** `--foundry-serif:'Literata',Georgia,'Times New Roman',serif;`
- **Fallback chain:** Literata → Georgia → "Times New Roman" → serif.

### Manrope — body, UI, eyebrows
A geometric sans. Brings clarity and modern legibility to body copy, navigation, labels, and
eyebrows.

- **Weights used:** 300 (Light — default body weight), 400 (Regular — small text, UI), 500
  (Medium — eyebrows, nav, UI labels).
- **Load:** `@import url('https://fonts.googleapis.com/css2?family=Manrope:wght@300;400;500;600&display=swap');`
- **CSS:** `--foundry-sans:'Manrope','Helvetica Neue',Helvetica,Arial,sans-serif;`
- **Fallback chain:** Manrope → "Helvetica Neue" → Helvetica → Arial → sans-serif.

**Avoid bold (700) entirely.** Confidence in this system comes from hierarchy — size, color,
spacing — not heaviness. The guide caps out at 600 (used only for the nav-brand mark) and treats
even that as an outlier; the working range for body/heading text is 300–500.

## Type scale

Built on a documented 1.25 ratio (Major Third) from a 16px base, with line-heights aligned to a
4px grid. This is the canonical, source-of-truth scale — reproduce these numbers even if a given
container's live CSS uses a slightly different responsive `clamp()`.

| Token | Size / line-height | Face + weight | Use |
|---|---|---|---|
| H1 | 48px / 56px | Literata 400 | Cover / top-level page headline |
| H2 | 32px / 40px | Literata 400 | Section titles |
| H3 | 24px / 32px | Literata 500 | Subheads |
| Body | 16px / 24px | Manrope 300 | Default paragraph text |
| Small | 14px / 20px | Manrope 400 | Captions, secondary text |
| Eyebrow | 11px / 16px, caps | Manrope 500 | Chapter-marker labels above headings |

`assets/foundry-brand.css` implements these as `clamp()`-based tokens (`--fs-h1` etc.) so they
scale gracefully on narrow viewports while topping out at the numbers above on desktop.

For a **hero/cover** moment specifically, the source guide allows headline sizes to run larger
(up to ~3.8rem / 61px) — treat that as a deliberate one-time exception for the single biggest
moment of a piece, not a second H1 scale.

## Setting rules

**Headings (Literata)**
- Weight 400 by default; 500 only for H3-level subheads.
- Tight-ish tracking on larger sizes: `-0.02em` at H1, `-0.01em` at H2, none below that.
- Sentence case, not uppercase — Literata's warmth is the point; all-caps fights it.
- Don't italicize for emphasis; the italic cut exists (300/400 italic are loaded) but is reserved
  for the tagline / a single deliberate editorial aside, not routine emphasis.

**Body (Manrope)**
- Weight 300 (Light) as the default paragraph weight; 400 for UI/small text; 500 sparingly for
  inline emphasis.
- Line-height 1.6–1.7 for paragraphs. Left-aligned, never justified.
- Cap measure at roughly 60–65 characters (`max-width` in the ~36–38em range) — the guide
  consistently constrains paragraph width even in wide containers.

**Eyebrows**
- Manrope 500, uppercase, letter-spacing 0.08–0.12em, 11px. A quiet chapter-marker above a
  heading or section, colored `--foundry-primary` (Current) — or dimmed to ~70% opacity on dark
  surfaces.

## Don't

- Don't use bold (700) weights — hierarchy comes from size/color/space, not weight.
- Don't set Literata in all-caps — that's a Manrope-eyebrow move, not a heading move.
- Don't justify body copy.
- Don't widen the body measure much past ~65 characters — the calm, unhurried read depends on
  restrained line length.
