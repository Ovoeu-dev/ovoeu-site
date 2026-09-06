# Ovoeu website redesign — session notes

Single Design Component: `Ovoeu Site.dc.html`. Six routes, one shared header + footer.
Open that file to work on the design. Everything below is decisions already made.

## Brand (from Ovoeu-Spec-Sheet_a.pdf)

| Role | Value | Notes |
|---|---|---|
| Dark Blue | `#224C5A` | PANTONE 7477 C. Primary brand surfaces, buttons |
| Deep (derived) | `#15323C` | Darkened dark blue. Footer, hero overlay, book band |
| Light Blue | `#A4BAC2` | PANTONE 7542 C. **Dark grounds only** — fails contrast on paper |
| Light-ground accent | `#2F6072` | Derived tint of dark blue. Eyebrows, numerals, italic lines |
| Paper | `#F4F6F6` | Cool near-white |
| Paper 2 | `#E9EEF0` | Secondary light band |
| Placeholder stripes | `#DFE7EA` / `#D7E1E5` | Image-slot hatching |

Body text on paper: `rgba(21,50,60,.84)`. Muted: `.72`. All checked ≥4.5:1.

**Type.** Spec names **Canela** (Commercial Type, licensed — not web-available).
Stand-in is **Instrument Serif** for display, **Jost** for sans/labels.
Buy Canela and swap if the budget allows.

**Logo.** Real artwork in use: `assets/ovoeu-wordmark.png` (header centered, footer
knocked to white via `filter:brightness(0) invert(1)`), `assets/ovoeu-icon.png`
(the Concierge nav icon). `assets/ovoeu-mark.png` is copied in but unused.

## Structure

Client-side routing via `state.route` in the logic class — no URLs/hashes.

- **home** — tabbed sections, sticky nav with scroll-spy
  1. Hero (existing site video)
  2. `01 · Toward the Vow` — full-width illustration + five moments
  3. `02 · The Ovoeu Difference`
  4. `03 · Inside the Atelier`
  5. `04 · Editions` preview (3 cards, "Shop coming soon")
  6. `05 · Book your Design Session`
  7. `06 · Frequently Asked` (3-item accordion → full page)
- **editions** — story-led, not a shop grid. Hero, "Now showing" (3 editions),
  "In build" (what the shop will do) + notify capture, Inquire band
- **contact** — "Contact Us", 4 channel rows (call / text / email / visit), image slot
- **faq-all** — all 8 questions, grouped in 4 categories, fully expanded
- **privacy** / **terms** / **refunds** — shared layout + legal sub-nav

Tab order follows page order: Toward the Vow · The Difference · The Atelier ·
Editions (SOON) · Book · FAQ · then the Concierge and sign-in icons.

## Decisions worth remembering

- **Every booking CTA routes to Contact.** Zoho Bookings links were removed on request.
- **One CTA per section.** Duplicate CTAs were cut twice (Atelier section, Editions rows).
  The Atelier section owns "Visit the atelier"; section 05 owns booking.
- **No em dashes anywhere.** Prose was rewritten, not just stripped. Label separators
  use `·`. En dashes kept in numeric ranges (60–90 minutes, Mon–Sat).
- **Facts appear once.** Hours live only on the Contact "Visit us" row and the footer.
- Top utility bar removed. Header CTA button removed. "The Lab-Grown Atelier"
  tagline removed from the header.
- Vimeo atelier film removed.
- `Lab-Grown` is wrapped in `white-space:nowrap` inside the hero H1 so it can't
  break on the hyphen.
- Contact form removed — booking happens by call or text.

## Numbers

- Voice: 405.416.5034
- Client texting: 855.772.6935
- hello@ovoeu.com
- 7527 North May Avenue, Oklahoma City · Mon–Sat 10:00AM–6:00PM · by appointment
- Google listing: `maps/place/Ovoeu/data=!4m2!3m1!1s0x0:0x365b8a39c4578de8`
- Client Portal: `ovoeustores.zcrmportals.com/portal/OvoeuStores/crm/login.sas`

## Open items

1. **Photography.** Labeled image slots waiting on real shots:
   atelier interior (wide), bench detail, Discovery Board, 3 home Edition cards,
   3 Editions hero stills, Contact hero (4:5, ring on light-blue ground).
2. **Terms of Service copy.** Three sections marked "copy needed": using this site,
   intellectual property, limitation of liability & governing law. No source copy
   exists on the live site. Privacy and Refund & Returns are verbatim from live.
3. **Editions content is draft.** Bearing / Threshold / Quiet Signet — names, stories,
   and specs are placeholders written about the *designs* (not invented client
   stories). Replace with the real pieces.
4. **Map embed** was removed with the Oklahoma City Atelier section. Can go into
   Contact's right column if wanted.
5. **Sign-in icon** in the nav is CSS-drawn. Swap for a real icon if there is one.
6. **Canela** licensing (see Type above).

## Tweaks (props on the root DC)

`showHeroVideo`, `showEditionsPreview`, `editionsMode` (Email capture / Type only).
