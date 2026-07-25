# Alef layer

This fork of IBM Carbon exists for one reason: Alef uses **Carbon's structure**
and **its own identity**. Carbon supplies the grid, spacing, component anatomy
and interaction states. Everything visual that makes Alef recognisable lives in
this directory and nowhere else.

## What's here

| File | Purpose |
|---|---|
| `alef-theme.css` | The complete Alef skin — typefaces, palette, Carbon token remap, register rules, epistemic marks. |

## Usage

Load it after Carbon:

```css
@import '@carbon/styles/css/styles.css';
@import './alef/alef-theme.css';
```

Dark is the default. Light is `[data-theme="light"]` on the root element.

## Why this layer is not optional

A plain Carbon fork renders **IBM Plex Sans on IBM blue (`#0f62fe`)**. That is
IBM's identity. Without this file, every screen generated from this repo — by a
person or by a tool consuming it — looks like an IBM product.

Concretely, this layer changes:

- **Typefaces.** Newsreader for headlines, Instrument Sans for body, IBM Plex
  Mono for labels and metadata only. Mono is never used for reading text.
- **Palette.** Warm brown-graphite neutrals, dark and light.
- **The accent.** Exactly one: ochre `#d5aa66`. It marks predicted or at-risk
  values and the primary action. Nothing else on the surface is coloured. Every
  `--cds-*` interactive token is remapped to it, which is what keeps IBM blue
  off the screen.
- **Register.** 2px radius, no shadows, no gradients. Hairline rules rather than
  boxes. Alef should read as a precision instrument, not a SaaS dashboard —
  these rules do more for that than the palette does.
- **Epistemic marks.** ● observed · ◇ predicted · ⊘ withheld · ✓ confirmed.

## The epistemic marks are not decoration

The product's central claim is that it distinguishes what it *observed* from
what it *predicted* from what it *deliberately did not retrieve*. Those four
marks carry that claim.

Do not restyle them into ornament, and do not collapse them to a single colour.
`◇ predicted` is ochre specifically so a modelled value can never be mistaken
for a measured one, and a predicted value should always appear with its
confidence. `⊘ withheld` means the system could have retrieved something and
chose not to — it is a governance statement, not an empty state.

## Typography floor

Labels are set at an 11px floor and must not go below it at any viewport.
Metadata, source attributions and epistemic labels are the *argument*, not
chrome; they are not the thing that shrinks when space is tight.

## Relationship to upstream Carbon

IBM source under `packages/` is unmodified and stays that way, so this fork can
still take upstream changes. If you find yourself editing `packages/`, the
change probably belongs here instead.

Palette source of truth is `alef-app/app/globals.css` in the product repo. The
marketing site deliberately runs a cooler neutral ramp at the same accent — if
those two are ever reconciled, this file follows the product.
