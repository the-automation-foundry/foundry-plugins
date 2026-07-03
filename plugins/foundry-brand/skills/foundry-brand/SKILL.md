---
name: foundry-brand
description: This skill should be used when the user asks to produce or critique communications in The Automation Foundry's brand — web pages, one-pagers, proposals, emails, workshop/deck slides, or other written and visual assets — using Foundry's verified design tokens (Literata + Manrope type; Limestone base with Current/Copper accents), signature components, real logo assets, voice, and manifesto/values. Trigger on phrases like "make this Foundry-branded", "in the Automation Foundry style", "thoughtful technology craftsmanship" tone, "check this against our brand voice", or a request to evaluate a client email/proposal/page against Foundry's brand guide. Outputs HTML by default; DOCX/PPTX on request.
---

# Foundry Brand

Recreate — and critique — The Automation Foundry's communication system. This skill encodes two
layers that always work together:

1. **The look** — Foundry's verified design tokens, signature components, real logo, and voice.
2. **The why** — Foundry's brand foundation: essence, qualities, values, positioning, and the
   7-principle manifesto (`references/principles.md`). This is what makes a Foundry artifact
   *true to the brand*, not just correctly styled.

A great Foundry artifact is on-brand **and** true to the underlying philosophy — calm, plain-
spoken, craft-oriented, never hype-y. Generate for both; evaluate for both.

## When to use

Trigger when the user asks for something "Foundry-branded," "in the Automation Foundry style,"
names The Automation Foundry, or asks to **evaluate/critique** a communication (an email, a
proposal draft, page copy) against Foundry's brand and voice. Don't auto-trigger on a generic
"make me a web page" with no brand context.

## Two modes

**Generate** — produce a page, proposal, email, or Foundry-styled visual asset that embodies the
look and the underlying philosophy.

**Evaluate** — critique an existing artifact (the user's draft, or a client-facing piece already
sent) against `references/principles.md` (the brand qualities and manifesto) and `references/
voice.md` (the tone-check pattern). Name what's off and why, then give the specific fix — be the
person who knows Foundry's brand well enough to say "this reads too corporate" or "this leads with
speed when it should lead with craft," not just a checklist-runner.

## Scope

- **In scope:** web/landing pages, one-pagers, proposals, client emails, workshop or capabilities
  decks, and Foundry-styled visual assets that reuse the same tokens.
- **Out of scope (offer, but flag as outside the formal skill):** full brand-identity work (new
  logo design, new palette), print production files, a from-scratch dark-mode full logo lockup
  (see `assets/logo/README.md` — only the icon has a verified dark variant).

## Output formats

| Format | Use when | Reference |
|---|---|---|
| **HTML** | Default. Highest fidelity, fastest to iterate, portable single file. | `references/layouts.md` |
| **Plain text / email** | Client emails — voice carries almost all the signal; drop card/grid chrome. | `references/layouts.md` → "Email" |
| **DOCX** | A proposal that must go through Track Changes. | `references/layouts.md` → defer to public `docx` skill, fed Foundry's token values |
| **PPTX** | Workshop or capabilities slides needing PowerPoint handoff. | `references/layouts.md` → defer to public `pptx` skill, fed Foundry's token values |

If unspecified, default to HTML and offer the others.

## How to use this skill

1. **Mode + artifact + format.** Generate or evaluate? Page, proposal, email, or deck? Which
   output format?
2. **Load the references.** Always load `references/colors.md`, `references/typography.md`,
   `references/voice.md`, and **`references/principles.md`** (the why — non-negotiable; it drives
   content decisions, not just review). Then `references/visual-vocabulary.md` and `references/
   layouts.md` for composing the output.
3. **Ground content in the principles first.** Before styling: does this lead with craft/clarity
   over speed/scale claims? Does it respect the reader's time (lead with the takeaway)? Is any
   word-list word to avoid slipping in ("leverage," "synergy," "seamless," "game-changing," etc.
   — see `voice.md`)?
4. **Apply the design tokens.** All color/type/spacing comes from `assets/foundry-brand.css`.
   Reference the CSS variables; never hardcode hex. For DOCX/PPTX, use the mirrored token
   *values* from `colors.md`/`typography.md`.
5. **Use signature components.** Pick named components from `visual-vocabulary.md` where they
   fit (essence block, quality card, value item, numbered principles, quick-ref closer). Two or
   three per page, not all of them. Hold the 60/30/10 color discipline — Copper spends sparingly.
6. **Use the real logo.** From `assets/logo/` — never a CSS fake. See `assets/logo/README.md`
   (note: only the icon has a verified dark-background variant; flag the gap if a full dark
   lockup is needed).
7. **Match the voice.** Plainspoken, calm, warm-but-not-arrogant, honest about complexity.
   `references/voice.md` — including the word-calibration lists and the tone-check pattern for
   dialing tone by context (onboarding vs. delivering vs. explaining vs. educating).
8. **Self-check before delivering.** Run the evaluation lens on your own output: does it read
   unhurried, or does it feel rushed/hyped? Is Copper used sparingly? Would a Foundry person say
   this across the table to someone they respect?
9. **Output.** Save the file and present it. For HTML, a single self-contained `.html` that
   imports (or inlines) `foundry-brand.css` and references the logo SVGs.

## The Foundry look in one paragraph

A calm Limestone canvas (60%) carries Literata headlines — sentence case, never bold, quiet
authority — paired with Manrope body copy at Light weight. Current (a muted blue-teal) does the
structural work: navigation, headings, card borders, eyebrows. Copper, a warm terracotta, appears
only where it's earned — a link, a CTA, a single accent — never as a fill. Signature components
(the dark essence block, the "this-not-that" quality card, the left-ruled value item, the
numbered manifesto list) do the organizing; generous negative space and short paragraphs do the
rest. Voice is plainspoken, warm, and unhurried — never hype-y, never stiff.

## Anti-patterns (don't)

- **Don't spend Copper like a primary color.** It's 10% by design — one CTA, one link, one accent
  word. Filling a card or a large surface with it breaks the ratio that makes the palette work.
- **Don't reach for bold.** Hierarchy comes from size, color, and space — not font-weight. This
  system tops out at Manrope 600 for one outlier UI label; body/heading text stays 300–500.
- **Don't write hype.** No "cutting-edge," "game-changing," "revolutionary," "seamless,"
  "leverage" (verb), "synergy." See the full avoid-list in `voice.md`.
- **Don't rush the reader.** Short paragraphs, one idea each, lead with the takeaway. A wall of
  text or a breathless pace is the opposite of "unhurried."
- **Don't fake brand assets.** Use the real logo SVGs in `assets/logo/`; never reconstruct the
  mark or invent a missing variant without flagging it as an assumption.
- **Don't lead with speed/scale over craft/partnership.** Check `references/principles.md` →
  positioning. An artifact that reads like a growth-hacking pitch is off-brand even with correct
  colors.
- **No purple gradients, gradient orbs, stock-photo gloss, or emoji bullets** — the generic-AI
  tells that this brand's calm, handmade imagery direction explicitly avoids.

## Future-proofing (when the brand guide is revised)

Every value here is **[VERIFIED]** from the source brand guide (v1.0, February 2026), kept at
`.assets/foundry-brand-guide.html` in the `foundry-plugins` marketplace repo — a repo-maintainer
reference, not a file shipped inside this plugin package. When that guide is updated:

1. Update `assets/foundry-brand.css` `:root` — most tokens flow from there.
2. Mirror value changes into `references/colors.md` / `typography.md`.
3. Replace logo SVGs in `assets/logo/` if official files differ, and re-check the dark-lockup gap
   noted in `assets/logo/README.md`.
4. `layouts.md` / `visual-vocabulary.md` shouldn't need changes unless the design language itself
   shifts.

## Reference index

- `references/colors.md` — verified 60/30/10 palette, tokens, usage rules, accessibility
- `references/typography.md` — Literata + Manrope, type scale, setting rules
- `references/voice.md` — voice pillars, tone-by-context, writing principles, word lists
- `references/principles.md` — **the why**: brand essence, qualities, values, positioning, the
  7-principle manifesto
- `references/visual-vocabulary.md` — the 10 signature component moves (eyebrow, essence block,
  quality card, etc.) + imagery direction
- `references/layouts.md` — signature HTML layouts for pages/proposals/email; DOCX/PPTX mapping
- `assets/foundry-brand.css` — single source of truth for design tokens + components
- `assets/logo/` — real Foundry logo SVGs (+ `README.md` provenance and the known dark-lockup gap)
