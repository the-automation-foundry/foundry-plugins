# Visual Vocabulary

These are the recurring component moves that make a piece feel like Foundry. Use them as building
blocks — a page typically uses two or three, not all of them stacked on top of each other.

All classes below are defined in `assets/foundry-brand.css`. Load that stylesheet and reference
the classes; don't hand-roll equivalents inline.

Two typefaces carry everything: **Literata** (serif) for headings/display, **Manrope** (sans) for
body/UI/eyebrows. Color comes from the 60/30/10 system — Limestone base, Current for structural
elements, Copper spent sparingly for warmth/CTAs. See `colors.md` and `typography.md` for the full
token set.

## 1. The eyebrow

A small uppercase Manrope label above a heading — the quiet "chapter marker." Colored Current;
dims to ~70% opacity on dark or primary surfaces.

**When to use:** above nearly every heading, as a section/context label ("Brand Guide v1.0",
"Part I", "Voice Pillars").

```html
<p class="foundry-eyebrow">Part I</p>
<h1 class="foundry-h1">Brand Foundation</h1>
```

## 2. The essence block

A dark (Ink) card that states the single most important idea in a section — brand essence, a
key finding, a thesis statement. Limestone headline, dimmed (`--foundry-dim`) supporting text.

**When to use:** once per major section, for the one thing that must land even on a skim. Don't
overuse — if every section has one, none of them read as the important one.

```html
<div class="foundry-essence-block">
  <p class="foundry-eyebrow">Brand Essence</p>
  <h2>Helping people become more fully alive through thoughtfully leveraging technology.</h2>
  <p>We exist because technology should serve human flourishing — not the other way around.</p>
</div>
```

## 3. The quality card ("this, not that")

A paired statement — what Foundry is, and the adjacent territory it deliberately avoids — on a
lifted paper (pure white) surface against the Limestone base.

**When to use:** describing brand personality, positioning against a competitor default, or any
"we do X, not Y" contrast.

```html
<div class="foundry-quality-card">
  <div class="foundry-quality-pair">Curious, <span class="foundry-not">not formal.</span></div>
  <p>We lead with genuine interest, not polished corporate distance.</p>
</div>
```

**Rules:** two to four per grid (`foundry-grid-2`), never one alone — the contrast across cards
is part of the effect. Keep the "not" clause short (2–4 words); it's a foil, not a second thesis.

## 4. The value item (left-rule)

A left-bordered statement in Current, used for values lists and writing principles — a quieter,
denser alternative to the essence block for a longer list of related points.

**When to use:** values, writing principles, any 4–6 item list of related standalone statements.

```html
<div class="foundry-value-item">
  <h3>Clarity</h3>
  <p>Writing is our superpower. If we can't explain it clearly, we don't understand it well enough.</p>
</div>
```

## 5. The positioning box

A tinted callout panel (Current-faint wash) holding a single Literata paragraph — the one-sentence
positioning statement or a similarly load-bearing single claim.

**When to use:** sparingly — once per document, for the sentence that most needs to be quoted back
verbatim. Never for routine body copy; the tint loses meaning if it's everywhere.

```html
<div class="foundry-positioning-box">
  <p>The Automation Foundry is a craft-oriented technology consultancy for values-driven business owners.</p>
</div>
```

## 6. The numbered principles list

A manifesto/framework device: a decorative leading-zero numeral in Mist, a Literata statement,
and a short explanation. Reads as a public, quotable list of beliefs.

**When to use:** manifestos, methodology explainers, "how we work" pages — anywhere a small
numbered set of durable beliefs or steps needs to stand on its own.

```html
<div class="foundry-principles-list">
  <div class="foundry-principle">
    <h3>Clarity is kindness.</h3>
    <p>If we can't explain it simply, we don't understand it well enough.</p>
  </div>
</div>
```

**Rules:** 5–7 items is the sweet spot (the source manifesto uses 7). More than ~8 and it stops
reading as a distilled set of beliefs and starts reading as a checklist.

## 7. The quick reference (dark summary block)

Same dark-surface family as the essence block, but grid-based — a "brand/idea in 30 seconds"
recap that closes out a longer document.

**When to use:** once, at the end of a brand-adjacent document, or anywhere a reader needs the
condensed version after the full explanation.

```html
<div class="foundry-quick-ref">
  <h2>The Automation Foundry</h2>
  <div class="foundry-quick-ref-grid">
    <div class="foundry-quick-ref-item">
      <h4>Tagline</h4>
      <p>Thoughtful technology craftsmanship.</p>
    </div>
  </div>
</div>
```

## 8. Imagery tier cards

Paper cards with a colored tier badge (primary/secondary/tertiary), used when documenting or
choosing among Foundry's three imagery layers.

**When to use:** art-direction briefs, imagery selection for a page, explaining the imagery
system itself. See `references/principles.md` is not the right file for this — imagery direction
detail lives below.

```html
<div class="foundry-imagery-tier foundry-tier-primary">
  <span class="foundry-tier-badge">Primary — Identity</span>
  <h3>Watercolor washes</h3>
  <p>Abstract watercolor textures in palette colors — the organic, handmade quality that makes Foundry visually unmistakable.</p>
</div>
```

### The three imagery layers

- **Primary — Identity: watercolor washes.** Abstract watercolor textures in palette colors
  (muted blues, warm coppers, soft limestone). Hero backgrounds, section dividers, accent
  elements. This is what makes Foundry visually unmistakable — organic and handmade, not another
  tech-company gradient.
- **Secondary — Clarity: abstract line art.** Minimal, geometric line illustrations for diagrams,
  process flows, icons. Fine strokes in Current or Ink on light backgrounds, Limestone or Copper
  on dark. Structured enough to feel precise, hand-drawn enough to feel human.
- **Tertiary — Proof: monochromatic photography.** High-contrast, warm-toned black and white.
  Used sparingly for case studies, team imagery, workspace shots. Always candid, never posed. Warm
  highlights, lifted shadows — shows the real: people, spaces, work in progress.

**Art direction notes:** natural light, warm tones, shallow depth of field. Show hands at work.
Calm, focused expressions. Clean but lived-in environments. **Avoid:** stock photography,
corporate settings, forced smiles, people pointing at screens.

## 9. Tone-check examples (voice calibration only)

A three-column comparison (too corporate / too casual / on brand) using soft supporting tints —
NOT part of the core 5-color palette. Reserved for teaching/evaluating voice, never as general UI
decoration.

```html
<div class="foundry-grid-3">
  <div class="foundry-tone-col foundry-tone-no"><div class="foundry-tone-label">✕ Too corporate</div>…</div>
  <div class="foundry-tone-col foundry-tone-mid"><div class="foundry-tone-label">✕ Too casual</div>…</div>
  <div class="foundry-tone-col foundry-tone-yes"><div class="foundry-tone-label">✓ On brand</div>…</div>
</div>
```

## 10. Logo placement

The Foundry mark is **two overlapping circles**, real SVG assets in `assets/logo/` (see
`assets/logo/README.md` for full provenance) — never a CSS reconstruction.

- `foundry-logo-lockup.svg` — full lockup (mark + wordmark), for **light** backgrounds. This is
  the only full-lockup variant provided by the source guide.
- `foundry-icon-on-light.svg` / `-on-dark.svg` / `-on-primary.svg` — icon-only mark, matched to
  the three ground colors it can sit on.

**Rules:**
- The intersection hatch is Current on light backgrounds, Copper on dark/primary — never any
  other color, and never a solid fill instead of the diagonal hatch.
- Maintain clear space equal to one circle's radius on all sides.
- Minimum sizes: full lockup 180px digital / 1.5" print; icon 32px digital / 0.4" print.
- Don't stretch, rotate, recolor outside the approved variants, or separate the two circles.

## Anti-patterns

- **All vocabulary at once.** Pick two or three moves per page. Essence block + quick-ref + every
  quality card + a positioning box + tone examples on one page is noise, not brand.
- **Copper as a fill color.** It's a 10% accent; using it as a large background or a fill for
  multiple cards inflates it past its role.
- **Bold type for hierarchy.** This system does hierarchy with size/color/space, not font-weight
  — reaching for bold instead of one of the components above is a tell that the wrong tool was
  picked.
- **Tone-check tints used as general UI color.** The soft red/green/tan pairs exist only to teach
  or evaluate voice — never use them to color unrelated UI.
- **Symmetry for its own sake.** The source guide's layouts are calm, but calm isn't the same as
  centered-and-boxed; keep generous, asymmetric white space rather than boxing everything in
  identical bordered cards.
