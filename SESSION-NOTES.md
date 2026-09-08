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
  1. Hero (video disabled, see Open items)
  2. `Toward the Vow` — full-width illustration; the five moments (01-05)
     are labeled directly in the artwork, with a mobile-only accessible text version
  3. `The Discovery Board`
  4. `Inside the Atelier`
  5. `Your Story` — anonymized tablet/iterations editorial
  6. `Editions` preview — A Life, Collected editorial banner
  7. `An invitation` — booking threshold
  8. `Frequently Asked` (3-item accordion → full page)
- **editions** — story-led, not a shop grid. Hero, "Now showing" (A Life,
  Collected; Beloved Isle; The Wall),
  "In build" (what the shop will do) + notify capture, Inquire band
- **contact** — "Contact Us", 4 channel rows (call / text / email / visit), image slot
- **faq-all** — all 8 questions, grouped in 4 categories, fully expanded
- **privacy** / **terms** / **refunds** — shared layout + legal sub-nav

Primary navigation is editorial and intentionally selective: Home · The Atelier ·
Editions · Virtual Design Sessions · The Discovery Board. FAQ, Contact, and the
Client Portal remain in the footer and the mobile utility row.

## Decisions worth remembering

- **Booking is now an invitation threshold.** “Begin Your Design Session” opens a
  ceremonial side sheet with two clear choices: At the Atelier or Virtual. Each
  choice continues to its corresponding Zoho booking calendar.
- **One CTA per section.** Duplicate CTAs were cut twice (Atelier section, Editions rows).
  The Atelier section owns "Visit the atelier"; section 05 owns booking.
- **No em dashes anywhere.** Prose was rewritten, not just stripped. Label separators
  use `·`. En dashes kept in numeric ranges (60–90 minutes, Mon–Sat).
- **Facts appear once.** Hours live only on the Contact "Visit us" row and the footer.
- Top utility bar removed. A single invitation-style header CTA was restored after
  approval of the “digital wedding invitation” direction. "The Lab-Grown Atelier"
  tagline remains removed from the header.
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

1. **Hero video is broken and disabled.** `showHeroVideo` now defaults to `false`.
   The hotlinked MP4 (`vid.cdn-website.com/.../Untitled+design-v.mp4`) is corrupted —
   confirmed with Playwright that it fails to decode in Chromium even when
   downloaded and played locally with no network involved at all (`readyState`
   stays `0`, `networkState` stays `NETWORK_NO_SOURCE`). Needs a fresh export from
   whoever produced it, hosted from `assets/` instead of the third-party CDN, then
   flip `showHeroVideo` back to `true`. The `autoPlay`/`playsInline`/`muted`
   attributes on the `<video>` tag are already fixed and ready for that file.
2. **Photography.** Real atelier wide, felt-table, and founder-at-bench photography
   are now in the Home and Contact layouts. Editions uses the approved editorial
   campaign artwork. Replace any of these later only with an intentional final crop.
3. **Terms of Service copy.** Three sections marked "copy needed": using this site,
   intellectual property, limitation of liability & governing law. No source copy
   exists on the live site. Privacy and Refund & Returns are verbatim from live.
4. **Editions content.** The earlier placeholder names and specs were replaced with
   the approved A Life, Collected and Beloved Isle concepts. Confirm production
   materials and commission language before enabling online ordering.
5. **Map embed** was removed with the Oklahoma City Atelier section. Can go into
   Contact's right column if wanted.
6. **Sign-in icon** in the nav is CSS-drawn. Swap for a real icon if there is one.
7. **Canela** licensing (see Type above).

## Mobile/responsive audit (2026-09-06)

Full desktop + mobile pass with Playwright against the live Pages URL. Fixed:

- **Nav overflow.** `<nav>` combined `justify-content:center` with
  `overflow-x:auto` and `flex-wrap:nowrap`; below the overflow threshold the
  centered overflow hid the first tab and the concierge/account icons on load,
  with no scrollbar to reveal them. Now `justify-content:flex-start` by default
  (first tab always visible) and centered again only at `min-width:1024px`
  (`.site-nav` class), where it actually fits — same look as before on desktop.
  Threshold was re-measured and this breakpoint bumped from 900px after the
  nav-spacing pass below.
- **Five-step row overflow.** Same class of issue: the 5-card row needs ~810px
  and never wraps, so most phones only showed 2 of 5 steps. It already scrolled
  correctly (starts at card 01), it just had zero affordance — hidden scrollbar,
  no hint. Both this row and the nav shared a `.hscroll` class that fades the
  right edge via `mask-image` when content can overflow. This row itself was
  later removed entirely (see Content updates) — the class/pattern is now only
  on the nav, but kept generic in case another scroller needs it.
- Hero video — see Open items above.

No other layout issues found; Editions/FAQ/Contact/Legal already reflow cleanly
to single-column on mobile.

**Nav spacing (same day, follow-up).** Tab gap widened 4px → 16px and button
padding 10px → 14px — `EDITIONS [SOON]` and `BOOK` were reading as crammed
together. This increases the nav's natural width, which is why the
flex-start/center breakpoint above moved to 1024px.

## Content updates (2026-09-06)

- Removed the "Five considered moments." heading, intro paragraph, and the
  five step cards from Home — the new banner (below) has 01-05 labeled
  directly in the artwork, so the text below it was fully redundant. Also
  dropped the now-unused `beats` data array.
- Replaced `assets/toward-the-vow.png` with the final high-res illustration,
  saved as `assets/toward-the-vow.webp` (quality 95 — mean pixel diff vs the
  source PNG is 1.6/255, visually indistinguishable) at 576KB instead of the
  2.6MB source export. Update the `<img src>` in `Ovoeu Site.dc.html` if this
  ever gets replaced again with a plain `.png`.
- Capitalized "For those who vow." (hero, footer, `og:title`) — was lowercase
  `for`.
- **Known minor quirk:** even with `showHeroVideo: false`, one aborted fetch to
  the hero video URL still fires on page load (confirmed no `<video>` element
  ever mounts — `document.querySelectorAll('video').length` is `0` throughout).
  Looks like the runtime (`support.js`) preloads media `src`s it finds in the
  template independent of the surrounding conditional. Harmless — no visible
  effect and not the full file — but not fixable from the template; would need
  tracing through the minified runtime to actually stop it.

## Tweaks (props on the root DC)

`showHeroVideo` (default `false` — see Open items), `showEditionsPreview`,
`editionsMode` (Email capture / Type only).

## Navigation and invitation update (2026-09-06)

- Rebuilt the header as one 80px editorial row on desktop: wordmark, five focused
  destinations, and one persistent “Begin Your Design Session” invitation.
- Replaced the mobile horizontal scroller with Menu · OVOEU · Begin and a full-height
  ivory navigation drawer.
- Added a keyboard-accessible invitation sheet with focus trapping, Escape/backdrop
  close, background inert state, scroll locking, and focus restoration.
- Retitled section 02 “The Discovery Board” and clarified how personal questions,
  stones, sketches, and iterations become the couple’s shared design language.
- Removed “SOON” from the primary navigation. Shop timing stays inside Editions.

## Founder, story, and imagery refinement (2026-09-06)

- Reframed the hero around the emotional promise: “A ring only your story could
  make.” Lab-grown, Oklahoma City, and virtual availability remain explicit below.
- Reserved 01–05 exclusively for the five Toward the Vow moments. Home section
  numbering was removed so it no longer conflicts with Black Glove Delivery.
- Added a mobile-only text rendering of the five moments; the words baked into the
  wide artwork were too small to carry the experience on a phone.
- Made the founder-led model explicit throughout Home, Contact, and FAQs. “Concierge”
  no longer hides the fact that guests work directly with the founder and jeweler.
- Added the Your Story editorial section and an anonymized tablet asset. The tablet
  header reads “ITERATIONS”; no client names or testimonials were fabricated.
- Replaced every development image slot in Home, Editions, and Contact with approved
  atelier photography or editorial artwork.
- Standardized customer-facing language to Technical Drawing (CAD on first mention),
  clarified the $250 prototype deposit, and changed “Nothing sold” to “Nothing pushed.”
