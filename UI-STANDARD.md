# Unitly website: UI standard

The rules every page on the Unitly website follows (`docs/index.html`, `docs/terms.html`, and anything added later).
When something new is built, it should look like it was always there. If a rule here and the code disagree, the code is
wrong or this file is out of date: fix one of them in the same change.

The site is plain HTML and CSS with a little JavaScript. No framework, no CSS library, no build step.

---

## 1. The idea: a report sheet

The product makes inspection reports, so the site reads like one: paper, ink, ruled lines, numbered sections,
tables. It should never look like a generic software template.

| Do | Don't |
|---|---|
| Separate things with rules (1px lines, 2px for section heads) and whitespace | Put content in rounded cards or boxes |
| Square corners on everything the site draws | Pills, rounded badges, tinted icon squares |
| Number sections and list items (01, 1.1) | Small uppercase labels above every heading by habit |
| Put data in tables (pricing, roadmap, spec sheet) | Bento grids, feature cards, icon grids |
| Show the real product: the app mockup and real report pages | Drawn illustrations, stock art, fake screenshots |
| One accent colour, used sparingly | Gradients, glows, a second accent |

**The one exception:** the app mockup in the hero copies the desktop app, which has 8px rounded controls and a
window shadow. That is a picture of the product, so it follows the app's look (section 9), not the site's.

---

## 2. Colour

Every colour is a token in `:root`. Never write a hex value in a component; add a token if one is missing.
Dark mode follows the visitor's system setting (`prefers-color-scheme`) and redefines the same tokens.

| Token | Light | Dark | Use |
|---|---|---|---|
| `--bg` | `#F7F7F4` | `#0C1214` | Page background (paper) |
| `--field` | `#FFFFFF` | `#11191C` | Form inputs |
| `--ink` | `#0E1A1F` | `#E8EEEF` | Text, heavy rules, outlined buttons |
| `--muted` | `#55636A` | `#9AABB1` | Secondary text, labels, hints |
| `--rule` | ink at 16% | ink at 14% | Light dividers |
| `--accent` | `#0F766E` | `#2BB3A3` | The one accent: primary buttons, links, numbers, highlights |
| `--accent-hover` | `#115E59` | `#3CC6B6` | Primary button hover |
| `--accent-ink` | `#FFFFFF` | `#04201D` | Text on the accent |
| `--ok` / `--err` | `#0F766E` / `#B42318` | `#3CC6B6` / `#F97066` | Form messages |

Rules:
- Body text and secondary text meet 4.5:1 contrast on `--bg` in both modes. Check any new pairing.
- Tints of the accent are made with `color-mix(in srgb, var(--accent) N%, transparent)`, e.g. 7% for the best-value
  pricing column and 9% for a selected plan. Don't invent new hues.
- The only full-bleed colour band is the trial section (`--accent` background). Keep it the only one.

---

## 3. Type

| Role | Font | Size | Weight | Tracking | Line height |
|---|---|---|---|---|---|
| Body | Geist (`fonts/geist.woff2`) | 17px | 400 | 0 | 1.6 |
| Hero h1 | Geist | `clamp(40px, min(7.4vw, 11vh), 104px)` | 600 | -0.05em | 0.96 |
| h2 (section titles) | Geist | `clamp(32px, 4.4vw, 56px)` | 600 | -0.035em | 1.02 |
| h3 | Geist | 20px | 600 | -0.015em | 1.25 |
| Lead paragraph (`.lead`) | Geist | 17px, `--muted`, max 52ch | 400 | 0 | 1.6 |
| Label (`.label`, `.tag`) | System monospace (`--mono`) | 12px, uppercase | 500–600 | 0.06em | 1.4 |
| Numbers (prices, big step numbers) | Geist | as designed | 300–600 | -0.04 to -0.05em | 1–1.1 |

Rules:
- One family for everything (Geist). The monospace is only for small labels, section numbers and key caps.
- Headings use `text-wrap: balance`; paragraphs use `text-wrap: pretty`.
- Keep paragraphs to 40–60 characters wide (`max-width` in `ch`).
- Hierarchy comes from size and weight, not colour. Accent-coloured words in a headline: one phrase at most.
- Form inputs are never below 16px, or iPhones zoom in when they're tapped.

---

## 4. Layout and spacing

- Content sits in `.wrap`: max 1240px wide, side gutter `--gut` = `clamp(16px, 4vw, 40px)`.
- Layouts use the 12-column grid `.g` (24px column gap). Typical splits: 4 + 7 columns (heading left, content right),
  or full width.
- Sections: `.sec` has `clamp(80px, 10vw, 136px)` top padding. No coloured section backgrounds (except the trial band).
- **Section head (`.sh`):** a 2px `--ink` rule on top, then the section number in accent and the section name, in the
  label style. Numbers run in page order: 01 Features, 02 How it works, 03 Roadmap, 04 Pricing, 05 Buy,
  06 Free trial, 07 FAQ. Renumber when sections move.
- Lists of things are ruled: 1px `--ink` line on top, 1px `--rule` line under each row (`.rows`, `.road`, `.ptable`).
- Spacing steps used across the site: 6, 8, 12, 16, 20, 24, 28, 36, 48, 56px. Pick from these.

### Breakpoints

| Width | What changes |
|---|---|
| ≤ 900px | Multi-column layouts stack; nav links hide (the trial button stays); spec sheet 2 columns |
| ≤ 720px | Pricing table: each row's label sits above its three values |
| ≤ 560px | Forms go single column; plan picker stacks; spec sheet 1 column; hero buttons go full width |

Every page must work at 375px wide with no sideways scroll.

---

## 5. Components

### Buttons (`.btn`)
- Square corners, 48px tall (40px in the header), 16px semibold text, an optional icon after the label.
- `.btn-primary`: accent background, `--accent-ink` text. One primary action per view.
- `.btn-ghost`: transparent with a 1px `--ink` border. Fills with ink on hover.
- Inside the trial band: `.btn-invert` (primary) and `.btn-line` (secondary).
- Pressing scales to 0.97. Disabled: 60% opacity and the `progress` cursor while a form sends.

### Links
`--accent`, underlined, `text-underline-offset: .18em`.

### Forms
- Label above the field (`label.l`, 14px semibold); optional fields say "(optional)" in `.hint`.
- Inputs: 48px tall, square, 1px border at 28% ink, `--field` background; focus is a 2px accent outline.
- Help text goes under the field in `.hint` (13px muted). Errors go under it in `.err` (13px semibold `--err`), and
  the input gets `aria-invalid="true"`.
- After sending, a `.msg` line with `role="status"` says what happened, in plain words.
- Checkboxes use `accent-color: var(--accent)`.

### Plan picker (`.choose`)
Three options sharing one border, each with a small square marker. The selected one gets a 2px accent inset
outline and a 9% accent tint. "Best value" is a `.tag` in accent, not a badge.

### Tables (`.ptable`)
2px ink rule under the header row, 1px rules between rows, row labels in `--muted`. Ticks and dashes are icons
with screen-reader text ("Included" / "Not included"). The highlighted column gets a 4px accent bar on top and a
7% tint.

### Checkout stepper (`.stepper`)
Four steps with monospace numbers (01–04). The current step has a 3px accent underline; done steps keep their
number in accent.

### FAQ (`details.q`)
Ruled rows. The marker is a + that turns 45° into ×. Answers expand smoothly where the browser supports it and snap
open where it doesn't.

### Key caps (`.kbd`)
Monospace, 1px ink border with a 3px bottom edge, so they look like a key.

### Status markers
Roadmap status is a small square: filled accent for "Available now", outlined for later.

---

## 6. Icons

- Website icons are **Phosphor (regular)**, inlined once in the SVG sprite at the top of `index.html` and used with
  `<svg class="ic"><use href="#i-name"/></svg>`. Only the icons in use are in the sprite; remove any that stop
  being used.
- Icons sit inline with text at 1.1em and take `currentColor`.
- Icons that mean something get screen-reader text next to them (`.sr`). Decorative icons get `aria-hidden="true"`.
- No emoji, ever.
- The app mockup uses its own `m-*` symbols, drawn to match the app's Windows icon font (section 9).

---

## 7. Logo

Masters and every export live in the app repo: `Unitly-Code/brand/` (`python brand/export.py ../Unitly/docs`
regenerates all website sizes).

| File | What | Where |
|---|---|---|
| `img/lockup-light.webp` | House mark + "Unitly", teal | Header and footer on light pages, app mockup (light) |
| `img/lockup-dark.webp` | House mark + "Unitly", pale | Header and footer on dark pages, app mockup (dark), the teal colour wipe |
| `icon-light.png` / `icon-dark.png` | House mark only | Favicons (light/dark browser themes), terms page back link, mockup title bar |
| `logo.png` | Teal house on white, 512px | Home-screen icon (`apple-touch-icon`) and the email header |
| `og.png` | 1200×630 link-preview card | Social shares (`og:image`, `twitter:image`) |

Rules:
- Swap light/dark with `<picture><source srcset="…-dark…" media="(prefers-color-scheme: dark)"><img src="…-light…"></picture>`.
- The header logo is 36px tall. Always set `width` and `height` on the `<img>` so the page doesn't jump while it loads.
- On the accent colour, use the pale lockup.
- Don't recolour, stretch, outline, add shadows to, or put the logo in a box.

---

## 8. Motion

Motion explains the product or confirms an action. It is never decoration on things people see many times.

| Thing | Motion |
|---|---|
| Button press | `scale(.97)`, 160ms, `--ease-out` |
| Hover (colour changes) | 150ms; only on real mouse pointers |
| Scroll reveal (`.reveal`) | Opacity + 12px rise, 600ms `--ease-out`, staggered with `--d` |
| Checkout step change | Slide 16px from the direction of travel + fade, 300ms |
| FAQ open | Height + fade, 280ms |
| Hero reel | Scroll-driven ad (see below) |

- Easing: `--ease-out` = `cubic-bezier(.23, 1, .32, 1)` for UI; `cubic-bezier(.77, 0, .175, 1)` for on-screen movement
  in the reel. Don't add new curves.
- Animate only `transform` and `opacity` (`clip-path` is fine for reveals). Never animate width, height, margin or top.
- Every hover style lives inside `@media (hover: hover) and (pointer: fine)`, so taps on phones don't leave it stuck.
- **Reduced motion:** with `prefers-reduced-motion: reduce`, movement goes away but colour and opacity feedback stay.
  The hero reel doesn't pin; it shows one still of the app under the headline.
- **No JavaScript:** the page still reads. Reveals start visible, and the hero shows its headline without the reel.

### The hero reel
The hero is a pinned, scroll-scrubbed ad: kinetic intro lines, a teal colour wipe with the logo, the app mockup at
work (cursor, callouts, typed captions, Generate), the report pages printing out, and an end card with the trial
button. The timeline lives in the reel script (`render(p)`). Rules for changing it:
- Keep the mockup faithful to the real app: same labels, layout, colours and order of rooms.
- Kinetic words rise through masks (`.m > span`); blocks wipe in from the left with `clip-path`.
- Text on screen must stay up long enough to read at a normal scroll speed.
- Phones get a camera push-in on the caption box so typing is readable; check any change at 375px.

---

## 9. The app mockup

The mockup in the hero is a picture of the desktop app, so it follows the app's design (from `Unitly-Code/gui/simple_app.py`),
not the site's:
- Its own tokens on `.app` (`--a-surface`, `--a-side`, `--a-line`, `--a-primary` …): the app's light palette, and in
  dark mode the app's warm greys (`#262624` surface, `#1F1E1D` sidebar) with the aqua accent `#2BB3A3`.
- 8px radius on controls and the photo viewer, 4px on thumbnails and key caps; nothing else rounded.
- Layout: title bar, header (logo, folder, theme, template, Generate), summary strip, tinted photo list (number,
  thumbnail, defect, check on the right), photo viewer, zoom info box, defect box, Previous / Save and next, keyboard
  info box with key caps.
- When the app's UI changes, update the mockup to match in the same week.

---

## 10. Accessibility and mobile basics

- One `<h1>` per page; headings in order.
- A "Skip to content" link first in the page.
- Visible focus: 2px accent outline, 3px offset, on every interactive element.
- Touch targets at least 44px tall.
- Images have real `alt` text; decorative ones have `alt=""`.
- `theme-color` meta tags match the page background in each mode.
- No tap-highlight flash, no 300ms tap delay (`touch-action: manipulation` on controls), button labels can't be
  long-press selected.

---

## 11. Copy

- Plain, short, specific. Say what happens ("Your licence key and download link are emailed once we confirm the payment").
- Sentence case for headings and buttons.
- Prices as `RM 800` (space after RM).
- British/Malaysian spelling: licence (noun), colour, cancelled.
- No made-up numbers, testimonials or "trusted by" logos. Only claims the product can back up.

---

## 12. Checklist before pushing a website change

- [ ] Looks right in light and dark mode.
- [ ] Works at 375px, 768px and desktop widths, with no sideways scroll.
- [ ] No new hex colours in components; tokens only.
- [ ] No new rounded boxes, pills or shadows outside the app mockup.
- [ ] Hover styles are inside the hover media query; reduced motion is handled.
- [ ] No errors in the browser console; forms still send.
- [ ] Section numbers still run in order.
