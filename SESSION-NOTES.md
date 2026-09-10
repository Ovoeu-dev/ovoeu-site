# Ovoeu website — design notes

Single Design Component: `Ovoeu Site.dc.html`. Client-side routing via `state.route`
in the logic class — no URLs/hashes. Open that file to work on the design;
`README.md` covers the technical/deploy side.

## Brand (from Ovoeu-Spec-Sheet_a.pdf)

| Role | Value | Notes |
|---|---|---|
| Dark Blue | `#224C5A` | PANTONE 7477 C. Primary brand surfaces, buttons |
| Deep (derived) | `#15323C` | Darkened dark blue. Footer, hero overlay, book band |
| Light Blue | `#A4BAC2` | PANTONE 7542 C. **Dark grounds only** — fails contrast on paper |
| Light-ground accent | `#2F6072` | Derived tint of dark blue. Eyebrows, numerals, italic lines |
| Paper | `#F4F6F6` | Cool near-white |
| Paper 2 | `#E9EEF0` | Secondary light band |
| Placeholder stripes | `#DFE7EA` / `#D7E1E5` | Image-slot hatching (Terms "copy needed" blocks) |

Body text on paper: `rgba(21,50,60,.84)`. Muted: `.72`. All checked ≥4.5:1.

**Type.** Spec names **Canela** (Commercial Type, licensed — not web-available).
Stand-in is **Instrument Serif** for display, **Jost** for sans/labels.
Buy Canela and swap if the budget allows.

**Logo.** Real artwork in use: `assets/ovoeu-wordmark.png` (header, footer knocked
to white via `filter:brightness(0) invert(1)`), `assets/ovoeu-icon.png` (the
Concierge nav icon). `assets/ovoeu-mark.png` is copied in but unused. The
second header icon (Client Portal) is a hand-drawn inline SVG, not an image file.

## Home page, in order

1. **Hero** — video disabled, see Open items.
2. **Toward the Vow** — full-width illustration (no baked-in text), followed by a
   responsive five-step grid (`.ov-journey-steps`) that is a five-column row on
   desktop and collapses to a single stacked column on mobile. It used to be
   mobile-only text under a banner that had the same five labels baked into the
   artwork; the banner was replaced with a clean version once the grid existed,
   so the labels only live in one place now.
3. **Bespoke** — a coverflow-style gallery (`#bespoke-track`, drag/swipe or arrow
   buttons, click any card to bring it to center) of real finished custom pieces,
   photos or videos, shown with no client names. Data lives in `bespokeRaw` in
   `renderVals()`. Hides entirely (section, nav tab, footer link) if that array
   is empty. Deliberately placed right after Toward the Vow, ahead of the more
   process-oriented Discovery Board and Atelier sections, since real finished
   work is the strongest hook for a jewelry visitor.
4. **The Discovery Board**
5. **Inside the Atelier**
6. **Your Story** — **currently hidden.** This is a real couple-testimonial
   slideshow (auto-advances every 2s, pauses on hover/touch), not a placeholder —
   the code and three real story images are still in the repo and `assets/`, but
   Ovoeu has not gotten consent from those couples to publish their stories, so
   the `stories` array in `renderVals()` is emptied to hide it. Restore by
   repopulating that array; the section/tab/footer link reappear automatically.
7. **Editions preview** — A Life, Collected editorial banner.
8. **An invitation** — booking threshold.
9. **Frequently Asked** (3-item accordion → full page).

Primary nav mirrors this list exactly (minus the invitation/FAQ bands, which are
in-page only) and always matches the footer's "On the home page" column — keep
`tabDefs`, `secIds`, and `footerSections` in sync if you reorder anything.

## Other routes

- **editions** — story-led, not a shop grid. Hero, "Now showing" (A Life,
  Collected; Beloved Isle; The Wall), "In build" (what the shop will do) +
  notify capture, Inquire band.
- **contact** — "Your Concierge — Talk to a real person.", 6 channel rows
  (call / text / email / begin at atelier / begin virtually / visit), atelier
  workspace photo. Deliberately generic, not a photo of one named person —
  see Decisions below.
- **faq-all** — all questions, grouped by category, fully expanded.
- **privacy** / **terms** / **refunds** — shared layout + legal sub-nav. Terms
  still has three "copy needed" placeholder blocks (see Open items).

## Decisions worth remembering

- **"Concierge," not "Founder & Jeweler."** Ovoeu plans to have more than one
  concierge in the future, so Contact and the FAQ answers about Design Sessions
  were rewritten to talk about "your Concierge" generically, and the Contact
  photo was changed from a photo of the founder to a generic atelier shot.
  Don't reintroduce founder-specific copy or imagery without checking this is
  still the plan.
- **No client names, ever, on Bespoke or Your Story.** Bespoke pieces are shown
  as finished work with no names attached. Your Story is real testimonials but
  is hidden until each couple has actually consented to appear (see above).
- **Bespoke vs. Editions vs. Your Story are three different things.** Bespoke =
  real one-off custom pieces (video/photo gallery, no names). Editions = designs
  already made, released with a story, and repeatable/commissionable again.
  Your Story = couple testimonials. Don't merge or rename these casually.
- **No em dashes anywhere.** Prose was rewritten, not just stripped. Label
  separators use `·`. En dashes are kept in numeric ranges (60–90 minutes,
  Mon–Sat).
- **One CTA per section.** Don't duplicate calls to action within a section.
- **Header and content share one alignment formula.** The header used to have
  its own `max-width`/padding combo that drifted out of alignment with the
  page content above ~1390px wide screens. It's now built the same way every
  content section is: a full-width element with `padding: 0 clamp(20px,5vw,56px)`
  containing a `max-width:1280px;margin:0 auto` inner box. If you touch header
  layout, keep that pattern or the two will drift apart again.
- **Image pipeline.** New photography/illustrations are converted to WebP at
  quality 95 and checked with a mean-pixel-diff script before the original is
  deleted (~1.3–1.9/255 diff is the "imperceptible" bar used throughout). Don't
  ship raw PNG/JPG exports if a WebP conversion is easy to do instead.

## Numbers

- Voice: 405.416.5034
- Client texting: 855.772.6935
- hello@ovoeu.com
- 7527 North May Avenue, Oklahoma City · Mon–Sat 10:00AM–6:00PM · by appointment
- Google listing: `maps/place/Ovoeu/data=!4m2!3m1!1s0x0:0x365b8a39c4578de8`
- Client Portal: `ovoeustores.zcrmportals.com/portal/OvoeuStores/crm/login.sas`

## Open items

1. **Hero video is broken and disabled.** `showHeroVideo` defaults to `false`.
   The hotlinked MP4 (`vid.cdn-website.com/.../Untitled+design-v.mp4`) is
   corrupted — confirmed with Playwright, using real Chrome (not just
   Playwright's bundled Chromium, which has no H.264 support at all and will
   give a false negative), that it fails to decode even downloaded and played
   fully offline. Needs a fresh export, hosted from `assets/` instead of a
   third-party CDN, then flip `showHeroVideo` back to `true`. The `autoPlay`/
   `playsInline`/`muted` attributes are already fixed and ready for that file
   (they must be camelCase in this template, not lowercase, or the browser
   won't apply them).
2. **Your Story is hidden pending consent** — see Home page section 6 above.
3. **Bespoke only has two pieces so far.** The coverflow gallery is built to
   scale to more; add entries to `bespokeRaw` as more finished pieces are
   cleared to show. No names, ever.
4. **Terms of Service copy.** Three sections still marked "copy needed": using
   this site, intellectual property, limitation of liability & governing law.
   Privacy and Refund & Returns are verbatim from the live site.
5. **Canela licensing** — see Type above.
6. **Known harmless quirk:** on first paint, the runtime (`support.js`) briefly
   requests the literal unrendered `{{ ... }}` string for a couple of image/video
   bindings (Bespoke, Your Story) before the real template value renders. Shows
   up as one or two harmless 404s in the network tab on page load; no visible
   effect, nothing to fix.
