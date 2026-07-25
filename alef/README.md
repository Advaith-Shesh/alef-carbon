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
- **Palette.** Cool graphite neutrals, dark and light. See below.
- **The accent.** Exactly one: ochre `#d5aa66`. It marks predicted values and
  the primary action. Nothing else on the surface is coloured. Every `--cds-*`
  interactive token is remapped to it, which is what keeps IBM blue off the
  screen.
- **Register.** 0 radius, no shadows, no gradients. Hairline rules rather than
  boxes. Alef should read as a precision instrument, not a SaaS dashboard —
  these rules do more for that than the palette does.
- **Epistemic marks.** ● observed · ◇ predicted · ⊘ withheld · ✓ confirmed.

## The palette is cool, and that is a hard rule

The neutrals were warm brown-graphite (`#171513` base, `#2b2723` borders) and
were rejected for warmth. They are now cool graphite, and the rule that keeps
them that way is mechanical rather than a matter of taste:

> For every neutral token, in both themes, the **red channel must be ≤ the blue
> channel**. Check it before you commit a new neutral.

The old ramp broke this by +4 to +29 per token. The current ramp runs −5 to −21
on dark and −5 to −19 on light, with `#ffffff` at exactly 0.

### Measured contrast

Floors are held on **every** surface, not just the base — the binding case is
always text on `--color-surface-overlay`.

| foreground | floor | dark (worst surface) | light (worst surface) |
|---|---|---|---|
| `--color-text-primary` | ≥ 12:1 | **12.74** | **14.00** |
| `--color-text-secondary` | ≥ 7:1 | **8.71** | **7.09** |
| `--color-text-tertiary` | ≥ 4.5:1 | **5.36** | **4.79** |
| `--color-action-ink` | ≥ 4.5:1 | **6.36** | **5.04** |
| status critical / positive / caution | ≥ 4.5:1 | 5.01 / 7.38 / 5.73 | 4.93 / 5.01 / 4.79 |

Primary text on the base surface measures 18.25:1 dark and 17.66:1 light.

### Layer separation

Surface steps are sized in **CIE L\***, not hex distance — hex distance is
meaningless near black. Adjacent layers are ≥ 4.3 L\* apart on dark; the warm
ramp managed ~3.0, which is why it read as one field rather than a stack.

### Two accent tokens, not one

`--color-action-primary` is the ochre **fill** (button backgrounds, chips);
`--color-action-ink` is the accent used as **ink on a surface** (links, the
`◇ predicted` mark). On dark they are the same value. On light they cannot be:
`#d5aa66` as text on a light surface measures **1.95:1**, so the previous
single-token palette made the predicted mark the least legible thing on the
page. The ink variant holds the ochre's hue (36.8°) and saturation (57%) and
only drops lightness to 29%.

Carbon tokens are mapped accordingly: `--cds-link-primary`, `--cds-focus`,
`--cds-button-tertiary` and `--cds-support-info` take the ink;
`--cds-button-primary` takes the fill. Getting that backwards is what produced
the unreadable links.

### Caution and the accent still overlap — unresolved

With ochre as the single accent, the amber warning slot is **occupied**. The
old `caution` was `#e2b562`, which sits **2.1° of hue and ΔE00 4.0** from the
accent — perceptually the same colour, so "at risk" and "predicted" were
indistinguishable. `caution` has moved to an orange (`#f0904f` dark,
`#8f4a14` light), raising separation to ΔE00 **14.0** and **10.5**.

That is a 3.5× improvement but it is **not a clean separation**, and it cannot
be made one by picking a different amber: every candidate that pulls away from
the accent collides with `critical`. Until this is ruled on, **never encode
caution by colour alone** — always pair it with the word or a glyph.

## The epistemic marks are not decoration

The product's central claim is that it distinguishes what it *observed* from
what it *predicted* from what it *deliberately did not retrieve*. Those four
marks carry that claim.

Do not restyle them into ornament, and do not collapse them to a single colour.
`◇ predicted` is ochre specifically so a modelled value can never be mistaken
for a measured one, and a predicted value should always appear with its
confidence. It must track `--color-action-ink`, not `--color-action-primary`.
`⊘ withheld` means the system could have retrieved something and chose not to —
it is a governance statement, not an empty state.

## Typography floor

Labels are set at an 11px floor and must not go below it at any viewport.
Metadata, source attributions and epistemic labels are the *argument*, not
chrome; they are not the thing that shrinks when space is tight.

## Relationship to upstream Carbon

IBM source under `packages/` is unmodified and stays that way, so this fork can
still take upstream changes. If you find yourself editing `packages/`, the
change probably belongs here instead.

## Where the palette lives now

`alef-theme.css` **is** the source of truth. It is no longer a copy of
`alef-app/app/globals.css` — the product's warm ramp was rejected and this file
carries the replacement. The marketing site's de-warmed ramp (`#141517` base)
was a step in this direction and is now superseded by it. If the product or the
site is re-skinned, they follow this file.

## Radius is 0 and hairlines stay 1px

Both are deliberate, and both were changes from the first version of this file.

**Radius `2px` → `0`.** A 2px radius on a 1px hairline is not a soft corner; at
normal density it is two or three blended pixels, so it reads as an
anti-aliasing artefact rather than a decision, and it softens exactly the
corners that make a dense layout look measured. Carbon's own default is 0. With
the layer steps now separating planes, rounding has no job left. `--alef-radius`
still exists so it can be reintroduced in one place.

**Hairlines stay 1px.** They were hard to see because they were low-contrast —
`border-subtle` measured 1.23:1 against the base — not because they were thin.
That is fixed in the palette: it now measures 1.63:1 on dark and 1.45:1 on
light. Thickening them instead would have produced SaaS dividers. Fix rule
presence with contrast, not weight. Two weights are provided: `--alef-hairline`
(subtle, for rows inside a dense table) and `--alef-rule` (default, for
separating regions).
