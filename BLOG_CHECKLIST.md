# Blog quality checklist (what a good-looking WoW blog in the EBN network must be)

Load together with `DOMAIN_PLAYBOOK.md`. Written after studying the reference https://downwardgame.com and our own reworked sites.
Goal: a visitor lands on a blog that looks like a real, cared-for personal gaming site. No site may look abandoned, empty, template-demo-like or broken.

## 0. Hard limits (from the user)
- Do NOT copy a reference design. Take only the structure (sections, pages, order).
- NEVER write a theme from scratch and NEVER edit a theme's code/CSS/files. Native block themes only; change content, blocks, patterns and page templates inside the theme (DB template rows are fine, theme files are not).
- No fake stats ("20+ years", "18k clients", "24/7"), no testimonials, no social links, no `#` links, no lorem, no example.com, no demo images, no newsletter forms, no fake addresses/phones.
- Anchors/links on the home are untouched when we only fix looks (one link, same anchor, same URL).

## 1. What the reference does well (structure only)
downwardgame.com (WoW class/raid guides, block theme `blockskit-*`):
1. Slim header: small logo + site name left, 3-4 plain menu items. No search clutter, no social icons.
2. Full-width hero: ONE strong H1 promise, one supporting sentence, an eyebrow label, own artwork behind it (a map), light dark overlay for contrast.
3. Three "pillars" directly under the H1 (numbered 01/02/03, one short sentence each): tells a visitor what the blog covers in 5 seconds.
4. Proof/summary strip (4 tiles). We keep the idea but with TRUE, non-numeric or countable-from-the-site content (e.g. "Raids", "Dungeons", "Classes", "Professions"), never invented numbers.
5. "Latest guides" row: 2-3 cards with real featured images, a small category label, date, author, 2-line excerpt + one intro card (heading + sentence + button) beside them.
6. The long article (our link) sits below the cards in a centred reading column.
7. Quiet dark footer with © and the site name.
What NOT to copy / weak points of the reference: oversized H2s inside the article (looks shouty), no About/Contact link in the footer, giant text wall with no images, a fake-looking stats block, nothing between the article and footer.

## 2. Page set every blog must have (menu <= 5, footer carries the rest)
| Page | Purpose | Minimum content |
|---|---|---|
| Home | 5-second explanation + latest posts + the one home article | hero H1/sub/2 buttons, 3 topic blocks, latest posts grid with images, author strip, article, footer |
| About / The Author | gives the blog a human owner | name, why they play, what the blog covers, 1 own image, link to contact; byline of every post links here |
| Contact | e-mail only | one e-mail address from the old contact page, short "what to write about" text |
| Privacy Policy | legal hygiene | sensible text, no fake date/address |
| Start Here / Guide hub | navigation for new readers | posts grouped by topic with real links, 300-500 words |
| FAQ or Glossary (optional, strongly recommended) | depth + internal links | 8-12 Q&A or terms, plain language |
| 404 | no dead end | friendly text + links to hub/home (never a second H1) |
Footer links: Privacy, About, Contact (+ Terms/Disclosure if the site had them). Menu labels must differ between sites (no footprints).

## 3. Home anatomy (top to bottom)
1. Header (logo + name + <=5 items). Logo is the site's own; missing logo -> text title, never a broken box.
2. Hero (own media image, readable overlay, one H1, one sentence, two buttons to real pages).
3. Three topic cards (WoW only: raids / Mythic+ / classes etc. matching the posts the site really has).
4. Latest posts grid: 3-6 cards, every card has a working image, category, date; no duplicated posts across several blocks.
5. Author strip (portrait from the site's media + 2 sentences + link to About).
6. The single home article: centred column, max ~720px wide, heading sizes at theme default or smaller, at most one image, ONE link.
7. Footer: own text, 2 link columns max, ©, e-mail. No theme credit, no social.

## 4. Post page
- Title is the only H1 (theme may render it as H2 on a few themes: then no other H1).
- Featured image shown ONCE (never as cover background + featured block + first inline image).
- Byline: author name linking to About, date. No avatar if it is a stock/demo avatar.
- Header + footer present (custom templates must reference parts of the ACTIVE theme slug).
- Sidebar (if any): search, latest posts, about box. No tag cloud with 4 odd tags, no demo ad, no social.
- No image wider than the column (fix: width:100% style on figure/img), no broken/placeholder images.

## 5. Visual quality gates (score each 0 = broken, 1 = weak, 2 = fine, 3 = good)
1. Header: logo/name/menu aligned, nothing cut off, menu <= 5 items, no raw demo labels.
2. Hero: real image, contrast OK, H1 readable at 390px.
3. Imagery: no broken images, no 0-byte defaults, no imgbb "image removed" placeholders (180x180 blue), no same picture 3x in a row.
4. Typography: heading scale sane (H1 > H2 > H3, article H2 not larger than ~36px), body 16-18px, line length 60-80 characters, contrast >= 4.5:1.
5. Spacing & rhythm: consistent section padding, no giant empty areas, no overlapping blocks, footer not glued to content.
6. Colour: one accent colour, dark/light consistent; no white box on dark page.
7. Mobile (390px): no horizontal scroll, menu reachable, images fit, text not tiny.
8. Content integrity: no demo text/images, no `#`, no social, no placeholders, no empty sections.
9. Completeness: About, Contact, Privacy, Start Here exist and are linked.
10. Look-and-feel ("would I trust this blog?"): overall impression.
Site passes when no criterion is 0 and the total is >= 20/30. Anything <= 14/30 gets a theme swap + rework (BRIEF.md / BRIEF2.md method).

## 6. Symptom -> fix table (learned in this project)
| Symptom | Cause | Fix |
|---|---|---|
| Header/footer missing on posts | custom `single` template references parts of the parent theme slug (child theme active) | change `"theme":"<parent>"` to the active theme slug in the template (done on global-ethic.org) |
| Same picture 3x at top of a post | custom single template: cover with useFeaturedImage + featured-image block + first inline image | swap to a native theme with simple `single` (done on marchforourlivespetition.com) |
| Broken featured image (empty box) | featured media is a 0-byte `default-image.jpg` | set each post's featured image to its own first inline image (done on gramohorn.com) |
| 180x180 blue "imgbb.com" picture in posts | ibb album scrape returned the host's "image removed" placeholder | replace images from the Drive folders (in progress, see report) |
| Tagline empty after SEO save | Yoast `options.php` post resets blogdescription | set tagline again via `wp/v2/settings` after any Yoast write |
| og tags vanish | posted `wpseo_social[...]` without `opengraph=1`,`twitter=1` | always repost both flags |
| og:title = domain | Yoast frontpage social title empty | set `wpseo_titles[open_graph_frontpage_title]` + `wpseo_social[og_frontpage_title]` |
| Home shows page 2 with the article again | home template reused on `/page/2/` | set posts_per_page high or hide the article via a query-loop condition (half-waif.com used 40) |
| Soft-404 plugin redirects old slugs to home | plugin | accept; do not rely on WP old-slug redirects there |

## 7. Theme selection guide (block themes installed so far)
Prefer themes whose NATIVE home already has: hero, post grid with images, author/about strip, simple footer, logo support. Reject themes whose native home is e-commerce/SaaS/portfolio heavy unless every demo block can be deleted without leaving a hole.
Good native results seen in the sheets: pressline-news, magazine-fse, the-entertainer, the-streamer, chronicle-journal, newsgrove, masthead, prism-magazine, nox-blog, dark-realm.
Plain/weak (needs extra care): postfolio-blocks, spiel (empty feel), the-minimal-blogger, inkora-blog (sidebar-box menu), hello-insurance (child theme, template pitfalls).
Already used (do not reuse on another site if avoidable): see `b19/themes_used.txt` in the scratchpad plus streamer, spectral-fse, fse-gamer, newspaper-builder, midnight-blogger, linea-blog, postblog, daily-news-blog, bedrock, mmo-realm, pc-gaming, the-free-blogger, voyago, vw-audio-podcast, travel-mates, dreambuild-fse.

## 8. Audit procedure for any site
1. Fetch home + one post + About at 1440 and 390 (Chrome UA, `waitUntil: load`, 3s wait, scroll once). Look at the screenshots.
2. Score the 10 gates; list symptoms; map them with section 6.
3. Fix with native means only; re-check live (cache purge) and re-screenshot.
4. Record result in the report table: site, theme, score before/after, what changed.
5. Delete creds/jars, remind the user to rotate keys.
