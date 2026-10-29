# Domain playbook (EBN WoW / gaming blog network)

Load this file first in a new session. It holds the goal, the user's hard rules, the working method and the current state. It contains NO credentials.

## 1. Goal
SEO / link-building across the user's EasyBlogNetworks (EBN) blogs. Each blog must look like its own real site (own theme look, own voice, no shared text or structure = "no footprints"), with exactly the backlinks to https://wowvendor.com that the user ordered.

## 2. The 17 "WoW Forever" homepage domains
Each home carries ONE article before the footer with exactly ONE wowvendor link (capitalised anchor).

| Domain | Anchor | URL |
|---|---|---|
| afairshot.org, cwalac.org, psnation.org, terrabattle2.com | WoW Forever Boost | https://wowvendor.com/shop/wow-forever/ |
| emergefestival.co.uk, whensallymetsally.co.uk, supremeleagueofpatriots.com | WoW Forever Boosting | https://wowvendor.com/shop/wow-forever/ |
| aimiaagophotographyprize.com, starr-art.com, cm-online.com, povertyproofing.co.uk, vodafonebusinessshop.co.uk | WoW Forever Gold | https://wowvendor.com/shop/wow-gold/product/wow-forever-gold/ |
| daymearocena.com, hyderabad.org.uk, jai-alaibooks.com, nbfcenter.com, savoringrhodeisland.com | Buy WoW Forever Gold | https://wowvendor.com/shop/wow-gold/product/wow-forever-gold/ |

The 9 first-listed Destiny-era sites (afairshot, cwalac, psnation, terrabattle2, emergefestival, whensallymetsally, supremeleague, aimiaago, starr-art) were redesigned as multi-gaming blogs. The other 8 (cm-online, povertyproofing, vodafonebusinessshop, jai-alaibooks, savoringrhodeisland, nbfcenter, daymearocena, hyderabad) are WoW diaries and MUST stay as they are.

## 3. Hard rules from the user
- Anchors: exact text as ordered, capitalised ("WoW Forever Boost"); exact URL; ONE link per home; inner pages carry NO wowvendor links (posts with older commercial links are untouched).
- Never rebuild a theme's layout by hand. Keep the theme's native design; edit only text/images/links inside its own blocks and its own patterns / page templates. "Write nothing from scratch."
- Variety between pages comes only from the theme's own patterns (`wp/v2/block-patterns/patterns`) and page templates; every site gets a different combination. No theme demo images on pages (use the site's own media); no avatars/stock.
- No footprints: distinct copy, headings, CTAs, menu labels, page names, section order per site; check with 7-gram overlap (`b7_fp.py`) across the sites and vs the diary sites.
- No social links/icons, no `#` links, no lorem / example.com / fake address / phone / placeholders. Contact = e-mail only.
- Exactly one H1 per page, no skipped heading levels (site-title level 0 when another H1 exists).
- Do NOT touch alt texts, plugins (soft-404 redirect plugins stay, only reported), or posts. Brand = domain name (site title stays domain), new multi-game tagline.
- Never use `?limited=` one-time login links. Credentials: fetch from the EBN API per use into a scratchpad file, delete it (and cookie jars) afterwards; remind the user to rotate the EBN key and blog admin passwords. Never commit secrets.
- Do not create PRs / commits unless asked. Report plain lists when the user says "просто список ссылок".
- A destructive/unclear step must not be worked around (a blocked `rm` of a glob was not bypassed).

## 4. Method
1. Access: `GET https://app.easyblognetworks.com/api/v1/blogs/{id}/` -> admin login; WP cookie login + REST nonce (`ebnwp.py` in scratchpad: `login`, `get`, `post`). Retry on proxy flakiness.
2. Text work: clean native markup (`b7_clean.py`: drop social-links, trigger-text groups, placeholders, theme images -> site media, `#` -> real pages, external links -> `/` except wowvendor), node-index replacement `replace_nodes` (`b7_nodes.py`), patterns filled from `b7/patcopy/<domain>.json` and applied by `b7_pages.py`.
3. Template customizations are DB rows: revert a theme to native with `DELETE wp/v2/templates|template-parts/<theme>//<slug>?force=true`. Navigation = `wp_navigation` entities (ids in `b7/nav_<domain>.json`).
4. Cache: `curl -X PURGE https://<domain>/`; Cloudflare-fronted sites (terrabattle2) may take ~1 h. Verify with a cache-busted URL.
5. SEO write paths (home title/description live in plugin settings, not page meta):
   - Yoast: POST `wp-admin/options.php` with `option_page=wpseo_page_settings`, `_wpnonce` (from the settings page JSON `"endpoint":".../options.php","nonce"`), `wpseo_titles[title-home-wpseo]`, `wpseo_titles[metadesc-home-wpseo]` (partial update works).
   - AIOSEO: `POST aioseo/v1/options {"options":{"searchAppearance":{"global":{"siteTitle","metaDescription"}}}}`.
   - SEOPress: `POST seopress/v1/options/titles-settings` REPLACES THE WHOLE OPTION. Always read the full option first and post all keys back (this bit us once; restored from a snapshot).
   - Squirrly (aimiaago) and no plugin (supreme, starr-art): hand the text over (`b7/meta_handoff.md`).
   - Targets: title 35-58 chars ending with the domain, description 120-156.

## 5. The 9 multi-gaming sites (theme = native, customized text only)
afairshot.org = streamer, cwalac.org = spectral-fse, psnation.org = fse-gamer, terrabattle2.com = newspaper-builder, emergefestival.co.uk = midnight-blogger, whensallymetsally.co.uk = linea-blog, supremeleagueofpatriots.com = postblog, aimiaagophotographyprize.com = daily-news-blog, starr-art.com = bedrock.

Done: native themes with rewritten texts, Forever article, bios/contact/privacy/listing pages, new nav (<=5 items), bylines linking to the bio page, footer copyright fixed, home title/description set (Yoast x4, AIOSEO, SEOPress), pattern blocks added to cwalac / terrabattle2 / emergefestival / aimiaago pages, QA green (one H1, one wowvendor link on home, no `#`/social/theme images).

## 6. Open items
- Hand-over meta for aimiaago (Squirrly), supreme, starr-art homes and aimiaago page descriptions (see `b7/meta_handoff.md`).
- whensallymetsally page meta descriptions are saved but not output.
- Not done: card wrap around the Forever article; page templates for afairshot / psnation / whensally; empty white box at the bottom of supreme posts.
- Not touched by decision: soft-404 plugins, og:image, afairshot plugin author box, empty thumbnail alts.
- Rotate the EBN API key and blog admin passwords.

## 7. Scripts (scratchpad `.../scratchpad/sites/`, session-bound; copy anything needed)
`ebnwp.py`, `b7_clean.py`, `b7_nodes.py`, `b7_native.py`, `b7_apply2.py`, `b7_pat.py`, `b7_pages.py`, `b7_patchk.py`, `b7_copychk.py`, `b7_fp.py`, `b7_qa.py`, `b7_heads.py`, `b7_audit.py`, `b7_meta.py`, `b7_home_yoast.py`, shot script `b7_shot2.js` (Playwright, executablePath `/opt/pw-browsers/chromium`). Data: `b7/` (spec, native, patcopy, pat2, ids_*, nav_*, bak), `b6/art/<domain>.json` (Forever articles).

## 8. Standard checklist for any new domain
Recon -> backup -> native theme text via nodes/patterns -> images from own media -> nav (<=5) -> footer (no glue, no duplicate columns) -> byline linked to bio page -> headings -> meta (per plugin) -> footprint gate -> PURGE -> QA + screenshots 1440/390 -> delete creds -> short report / link list.
