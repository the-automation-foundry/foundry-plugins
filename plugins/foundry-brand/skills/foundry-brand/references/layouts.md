# Layouts

Signature layouts for Foundry output. Every class below — `.foundry-section`, `.foundry-essence-block`,
`.foundry-quality-card`, `.foundry-principles-list`, etc. — is defined in `assets/foundry-brand.css`.
Load that stylesheet and the classes do the heavy lifting; the snippets here only compose them.

Hold the line on every layout:

- **Calm over dense.** Limestone base, generous padding, short paragraphs. This brand is
  "unhurried" by principle, not just by name.
- **60/30/10 discipline.** Limestone dominates, Current is the workhorse supporting color, Copper
  spends only on the one or two things that deserve warmth (a CTA, a link, an accent word).
- **Literata for gravity, Manrope for clarity.** Don't reach for bold — hierarchy is size/color/
  space, not weight.
- **No stock-photo gloss, no corporate gradients, no forced smiles.** See `visual-vocabulary.md` →
  imagery direction.

---

## HTML is the primary path

HTML is the default output: a single self-contained `.html` file with `assets/foundry-brand.css`
loaded (or inlined for offline use). Fonts arrive via the Google Fonts `@import` already at the
top of that stylesheet, so no build step is required. Unlike a slide-deck brand, Foundry output is
a **scrolling document** by default (proposals, guides, emails, web pages) — don't wrap content in
a fixed-aspect stage; let `.foundry-section` / `.foundry-section-wide` set a comfortable reading
measure and let the page grow.

---

## Signature layouts

### Hero / cover

**When:** the opening moment of any page, guide, or proposal. Eyebrow, one Literata headline, a
lead paragraph, optional meta row. Full lockup logo, usually top nav or a quiet corner mark rather
than a huge centerpiece — the mark is a signature, not the hero.

```
┌──────────────────────────────────────────────┐
│  BRAND GUIDE v1.0                              │
│                                                │
│  The Automation Foundry                        │
│  (Literata H1)                                 │
│                                                │
│  This guide captures who we are, how we look,  │
│  and how we speak. (lead, Literata 300)        │
│                                                │
│  Status · Last Updated · Palette · Typefaces   │
└──────────────────────────────────────────────┘
```

```html
<section class="foundry-surface" style="padding:6rem 3rem 4rem;">
  <p class="foundry-eyebrow">Brand Guide v1.0</p>
  <h1 class="foundry-h1">The Automation Foundry</h1>
  <p class="foundry-lead">This guide captures who we are, how we look, and how we speak — so
    every touchpoint feels like what it is: thoughtful technology craftsmanship.</p>
</section>
```

### Foundation section (essence + qualities)

**When:** establishing who Foundry is before getting into specifics — an about page, a proposal's
opening, a workshop's framing slide. Pairs an essence block with a grid of quality cards.

```
┌──────────────────────────────────────────────┐
│  ┃ Brand Essence (dark essence-block)         │
│  ┃ Helping people become more fully alive…    │
│  ┃                                            │
│  ┌───────────────┐  ┌───────────────┐         │
│  │ Curious,      │  │ Calm,         │         │
│  │ not formal.   │  │ not hectic.   │         │
│  └───────────────┘  └───────────────┘         │
└──────────────────────────────────────────────┘
```

```html
<div class="foundry-section" id="essence">
  <div class="foundry-essence-block">
    <p class="foundry-eyebrow">Brand Essence</p>
    <h2>Helping people become more fully alive through thoughtfully leveraging technology.</h2>
    <p>We exist because technology should serve human flourishing — not the other way around.</p>
  </div>
</div>

<div class="foundry-section">
  <p class="foundry-eyebrow">Brand Qualities</p>
  <h2>This, not that.</h2>
  <div class="foundry-grid-2">
    <div class="foundry-quality-card">
      <div class="foundry-quality-pair">Curious, <span class="foundry-not">not formal.</span></div>
      <p>We lead with genuine interest, not polished corporate distance.</p>
    </div>
    <div class="foundry-quality-card">
      <div class="foundry-quality-pair">Calm, <span class="foundry-not">not hectic.</span></div>
      <p>Our communication, process, and design carry unhurried confidence.</p>
    </div>
  </div>
</div>
```

### Values / principles rail

**When:** a list of 4–6 standalone but related statements (values, writing principles). The
left-rule value-item is the workhorse content layout — denser than the essence block, calmer than
a bordered card grid.

```html
<div class="foundry-section" style="display:flex;flex-direction:column;gap:1.75rem;">
  <div class="foundry-value-item">
    <h3>Authenticity</h3>
    <p>Say what we mean and mean what we say.</p>
  </div>
  <div class="foundry-value-item">
    <h3>Clarity</h3>
    <p>Writing is our superpower. If we can't explain it clearly, we don't understand it well enough.</p>
  </div>
</div>
```

### Manifesto / numbered principles

**When:** a public-facing statement of belief — the 7-principle manifesto, a methodology
explainer, a "how we work" page. See `visual-vocabulary.md` #6.

```html
<div class="foundry-section" id="manifesto">
  <p class="foundry-eyebrow">The Automation Foundry Manifesto</p>
  <h2>What we believe about technology and being human.</h2>
  <div class="foundry-principles-list">
    <div class="foundry-principle">
      <h3>Technology should make you more fully alive — not more fully busy.</h3>
      <p>The measure of a good system isn't how much it can do. It's how much space it creates
        for the things that matter.</p>
    </div>
    <div class="foundry-principle">
      <h3>Clarity is kindness.</h3>
      <p>If we can't explain it simply, we don't understand it well enough.</p>
    </div>
  </div>
</div>
```

### Positioning statement

**When:** the single sentence that most needs to be quoted back verbatim — a proposal's opening
claim, an about page's thesis. Use once per document; the tint loses meaning if repeated.

```html
<div class="foundry-positioning-box">
  <p>The Automation Foundry is a craft-oriented technology consultancy for values-driven
    business owners who know that investing in the right systems is wise — and that hiring a
    specialist to build them is wiser.</p>
</div>
```

### Proposal / one-pager

**When:** a client-facing proposal or capabilities one-pager. Same tokens, document rhythm rather
than slide rhythm — more like the source guide itself. Signature section order:

1. **Hero** — eyebrow + Literata headline + lead, the engagement or company name.
2. **Positioning** — the one-sentence "why us" in a `.foundry-positioning-box`.
3. **What we do** — a `foundry-grid-2`/`foundry-grid-3` of quality-cards or value-items describing
   services.
4. **How we work** — 3–5 numbered principles (a scoped-down manifesto: process phases or working
   principles specific to this engagement).
5. **Quick reference / next step** — a `.foundry-quick-ref` dark closer with the concrete next
   action, not a generic "thank you."

### Tone-check / voice review panel

**When:** evaluating a draft (email, proposal, page copy) against the brand voice. Three-column
comparison, `visual-vocabulary.md` #9.

```html
<div class="foundry-grid-3">
  <div class="foundry-tone-col foundry-tone-no">
    <div class="foundry-tone-label">✕ Too corporate</div>
    "We are pleased to inform you of significant progress across all deliverables."
  </div>
  <div class="foundry-tone-col foundry-tone-mid">
    <div class="foundry-tone-label">✕ Too casual</div>
    "Hey! Tons of awesome stuff happened this week!! 🎉"
  </div>
  <div class="foundry-tone-col foundry-tone-yes">
    <div class="foundry-tone-label">✓ On brand</div>
    "Good progress this week. We completed the integration and ran initial tests."
  </div>
</div>
```

### Quick-reference closer

**When:** the last thing on a longer document — the condensed version after the full explanation.
See `visual-vocabulary.md` #7.

```html
<div class="foundry-section">
  <div class="foundry-quick-ref">
    <h2>The Automation Foundry</h2>
    <div class="foundry-quick-ref-grid">
      <div class="foundry-quick-ref-item">
        <h4>Tagline</h4>
        <p>Thoughtful technology craftsmanship.</p>
      </div>
      <div class="foundry-quick-ref-item">
        <h4>Voice</h4>
        <p>Plainspoken expertise. Thoughtful intentionality. Warm confidence. Honest partnership.</p>
      </div>
    </div>
  </div>
</div>
```

---

## Email

Foundry's most frequent real-world touchpoint. Drop all card/grid chrome — email clients don't
render custom fonts or CSS grids reliably. Keep:

- The **voice** (writing principles + word calibration from `voice.md`) — this carries almost all
  of the brand signal in an email.
- Plain-text-safe structure: lead with the takeaway, short paragraphs, one clear next step.
- A plain-text signature; the wordmark/lockup image only if the client's email setup reliably
  renders inline images, and only as a small corner mark, never a hero banner.

## Other formats (DOCX / PPTX)

For a proposal that must go through Track Changes, or slides for a workshop, defer to the public
`docx` / `pptx` Claude skills for file generation, but feed them Foundry's token **values**
explicitly so output doesn't drift generic:

- **Type:** Literata (serif) for headings, Regular/Medium weight, sentence case; Manrope (sans)
  for body, Light weight, contractions encouraged.
- **Ground:** Limestone `#F0EFEB` (60%), with Current `#4E6A78` (30%) for structural elements and
  Copper `#BF7E4A` (10%) spent sparingly for warmth/CTAs; Ink `#262B2E` for dark surfaces/anchor
  text.
- **Moves:** eyebrow labels, left-rule value items, numbered principles, a closing quick-reference
  — see `visual-vocabulary.md`.

Two things drift most in DOCX/PPTX — pin them explicitly:

1. **Font embedding.** Literata and Manrope aren't system fonts; embed them or the brand silently
   falls back to Georgia/Calibri. State this requirement to the skill generating the file.
2. **The exact hexes.** Generic "teal"/"tan" round to the wrong swatch — hand over the literal
   values from `colors.md`, don't let a theme palette pick nearest-match colors.
