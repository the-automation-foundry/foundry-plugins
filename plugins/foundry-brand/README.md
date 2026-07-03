# foundry-brand

A Claude plugin that produces **and critiques** communications in The Automation Foundry's
brand. Two layers that always work together:

- **The look** — verified design tokens (Literata + Manrope type; Limestone base with muted
  Current/Copper accents), 10 signature components, and the real Foundry logo as vector assets.
- **The why** — Foundry's brand foundation: essence, qualities, values, positioning, and the
  7-principle manifesto, grounded in `.assets/foundry-brand-guide.html`.

Outputs **HTML** by default; **DOCX/PPTX** on request. Works in **generate** or **evaluate** mode.

## Triggers

Asking for something "Foundry-branded," "in the Automation Foundry style," or to **critique** a
communication (email, proposal, page copy) against Foundry's brand voice.

## Layout

```
skills/foundry-brand/
├── SKILL.md                 the brain — modes, workflow, anti-patterns
├── references/              colors · typography · voice · principles (the why)
│                            · visual-vocabulary · layouts
├── assets/
│   ├── foundry-brand.css    token + component backbone (single source of truth)
│   └── logo/                real Foundry logo SVGs (+ provenance README)
```

## Install

See the [marketplace README](../../README.md). Quick version:
```
/plugin marketplace add the-automation-foundry/foundry-plugins
/plugin install foundry-brand@foundry-plugins
```

## Provenance

All tokens, voice, and principles are **[VERIFIED]** — extracted directly from the source brand
guide (v1.0, February 2026), kept at `.assets/foundry-brand-guide.html` in the `foundry-plugins`
marketplace repo root (a repo-maintainer reference — not shipped inside this plugin package).
Logo SVGs are the real vector paths from that guide, not reconstructed. When the guide is
revised, update `assets/foundry-brand.css` first — see SKILL.md → "Future-proofing."
