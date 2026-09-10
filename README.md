# BellaFina Outdoors — Outdoor Pizza Ovens

Single-page static landing site for **Outdoor Pizza Ovens**, deployed on Vercel at
**pizzaovens.bellafinaoutdoors.com**.

Built-in and freestanding, gas and wood-fired. Sits at the intersection of pizza oven
product searches and broader outdoor-kitchen planning, creating a second route into
the outdoor cooking category.

## Structure

```
index.html      the whole page — content, nav, forms, JSON-LD
assets/
  site.css      all styles
  site.js       lazy split-section video, UTM passthrough, hero lead form, quote popup
  logo.webp     brand logo and favicons
vercel.json     cleanUrls + no trailing slash
robots.txt      allow-all + sitemap reference
sitemap.xml     the single URL
```

Everything is at the repository root, so Vercel deploys it with no Root Directory
setting and no build step.

## Deploying

1. vercel.com → **Add New → Project** → import this repository
2. **Framework Preset → Other**. Leave Build Command and Output Directory empty —
   these are plain static files, there is nothing to build
3. Deploy
4. **Settings → Domains** → add `pizzaovens.bellafinaoutdoors.com`, then add the
   `CNAME` record Vercel shows to your DNS (`cname.vercel-dns.com`)

Production deploys from `main`. Every push redeploys automatically.

## This site is standalone

It does not link to the other BellaFina landing sites (fire bowls, fire pits and outdoor kitchens).
The header nav is on-page anchors only:

`#top` Pizza Ovens · `#fuel` Gas vs Wood · `#types` Oven Types · `#how` How It Works ·
`#specs` How to Choose · `#areas` Service Area · `#faq` FAQ

The header logo is an anchor to `#top`, not an outbound link — nothing in the nav
leaves the page.

Every outbound link goes to **bellafinaoutdoors.com**, deep-linked to the matching
page rather than the homepage, and carries UTM tags:

- `utm_source=pizza-ovens-lp`
- `utm_medium=landing_page`
- `utm_campaign=pizza-ovens`
- `utm_content=<placement>` — e.g. `split_gas_ovens`, `card_built_in_ovens`,
  `showroom_directions`, `footer_contact`

Adding a nav item means adding an `id` to the section it points at.

## One primary CTA

The page has a single primary call to action — **Request My Free Quote**, which opens
the quote modal. It appears in the header, the hero, the product grid, the showroom
band, the trade band and the final CTA, and `.btn-primary` is reserved for it.
Outbound links to bellafinaoutdoors.com are always secondary (`.btn-navy`,
`.btn-outline` or a text link), so no off-site link competes with the quote.

## Leads

The hero form and the quote popup both use the GoHighLevel form
`iD7GLxxCdv51i6umUCJF`. `<body data-lead-source="Outdoor pizza ovens page">`
is what marks the CRM record as coming from this site, so keep it if you copy this
page anywhere.

Until `LEAD_WEBHOOK_URL` in `assets/site.js` is set, the hero form opens the hosted
version of that form in a new tab with the fields prefilled, rather than posting
directly. Set the webhook URL to capture leads without the extra step.

The popup opens by itself 5 seconds after load, once per session, and on
desktop exit intent.

## Design

The page follows the BellaFina Outdoors system, and every value in it comes from a
token in `assets/site.css` — no page or rule should carry a raw hex.

| Token | Value | Use |
| --- | --- | --- |
| `--navy` | `#0d1b2a` | headings, dark bands, footer |
| `--cream` | `#fdf5ec` | page ground |
| `--orange` | `#e5672a` | the single accent — primary CTA, bullets, links |
| `--gold` | `#c8a25b` | eyebrows and hairlines |
| `--radius` | `10px` | every card, button, field and panel |

Type: **Cormorant Garamond** for `h1`/`h2` (`--font-display`), **Montserrat** for
sub-headings, nav, buttons and eyebrows (`--font-ui`), **Roboto** for body copy
(`--font-body`). All three load from Google Fonts in `<head>`.

Section order, top to bottom:

hero → trust strip → design center → lede → product splits → product grid →
how it works → why BellaFina → specs → FAQ → service areas → showroom → trade →
final CTA → footer

This site has no photography of its own yet, so the hero uses a brand gradient
(`.hero-oven` in `assets/site.css`) and the split sections use gradient panels with an
SVG mark (`.visual-fire`, `.visual-water`). To use a real photo: drop it into
`assets/`, swap the gradient class on the hero `<section>` for `hero-photo`, and
set `style="--hero-image:url('/assets/your-photo.webp')"`. Use a root-relative
path there — a relative `url()` inside a custom property resolves against
`site.css`, not the page.

## Geographic coverage

Rather than a thin page per city, the service-area section carries the whole
Southern California footprint at once, grouped into clusters by county (Orange,
Los Angeles, San Diego), with copy on what actually differs by area. The same city
list feeds `areaServed` in the page's JSON-LD.

To extend the footprint, edit the `.areas` section — don't spawn new URLs.

## Before launch

- Point `pizzaovens.bellafinaoutdoors.com` at the Vercel project and confirm HTTPS
- Add real photography (see **Design**)
- Add this site as its own property in Search Console and submit `sitemap.xml` —
  subdomains are separate properties
