# Look-and-feel audit report (2026-11-13)

Scope: 171 blogs (149 sites with our inner posts + 22 sites reworked this session), home page at 1440px screenshotted and reviewed against `BLOG_CHECKLIST.md`.

## Result of the audit
- Most sites already have a hero + post grid + footer and look acceptable (reworked sites such as mirrortracker.info, kujaswowgoldguide.com, bananaskirt.net, nasl.tv, tekkengamer.com, warcraftgoldstore.com, ab-gaming.com, bullybeware.com, jasonpolan.com are the good reference group).
- Weak or broken (worst first): warriorsrtg.com (lorem ipsum + invented stats), yellingontheinternet.com (fake phone/US address bar, plumbing theme), demossgroup.com (broken images, bare link list), archeagesource.com (black page of raw links), ard-zukunftsdialog.de (broken hero, unreadable title) -> all five reworked in this round.
- Next wave candidates: zaman-online.de (broken card icons), noprophet.com (broken image, empty layout), xenosisgame.com (link-tree look with emoji buttons), stephanierct.com / insidiagame.com / studio71games.com (invented numbers such as "100+", "24/7", "3.2K happy customers"), hypersentinel.com (oversized image on beige), cdautism.org (plain), darknexusarena.com (unreadable overlay text).
- Not reachable during the audit: brellrants.net (Cloudflare 522), dropininc.com, giorgionenyc.com, tankdomination.com, tfbn.org (blank), tekkengamer.com (one blank capture, fine on re-check).
- imgbb "image removed" placeholders were found in 53 posts and replaced with Drive screenshots (see session report).

## The five reworked sites (before -> after)
| Site | Theme | Main problems fixed | Home link kept |
|---|---|---|---|
| warriorsrtg.com | bizento (kept) | lorem ipsum, fake stats, no Start Here/FAQ | WoW gold -> /shop/wow-gold/ |
| yellingontheinternet.com | plumbing-fse (kept) | fake phone/address/stats/testimonials, empty footer, no page titles | WoW Raid Carry -> /shop/wow/buy-wow-pve-raid-boost/ |
| demossgroup.com | media-magazine | broken images, bare list, no pages | Diablo 4 Boosting -> /shop/diablo-4/ |
| archeagesource.com | minimal-blog-nova | black raw-link page, empty title/meta | WoW Raid Boost -> /shop/wow/buy-wow-pve-raid-boost/ |
| ard-zukunftsdialog.de | breezi-blog | broken hero, unreadable text, no German pages | WoW Boost -> /de/shop/wow/ |

All five now have a hero with an own image, three topic cards, a latest-posts grid with working images, an author strip, the single home article in a 720px column, Start Here + FAQ pages, e-mail-only contact, a menu of 4-5 items, a footer without theme credit, and home meta without the site name.

## claude-seo skill test (read-only, archeagesource.com home)
Run with `page_checks.py --file` (the skill refuses the sandbox proxy for live fetches, so the HTML was saved first).
- Before: 0 Critical, 0 High, 3 Medium (title 16 chars, no meta description, first image lazy-loaded), 2 Low (2 H1, no og:image).
- After the rework: 0 Critical, 0 High, 0 Medium, 2 Low (no og:image, one element with >60 children). Title 48 chars, description 145 chars, one H1.
- Verdict: the deterministic page checks are fast and repeatable and agree with what the visual audit found; they do not judge looks. Still open on all sites: og:image.
