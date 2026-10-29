# Changelog

All notable changes to the Akhona Teens Leadership Foundation website. Newest first.
Each entry says **what** changed, **why**, and **which files** were affected, so the lecturer can follow every edit.

---

## [Part 3] — Functionality, SEO, forms and deployment (28 October 2026)

### A. Corrections to Part 2 work

These are the problems found when re-reading the Part 2 submission against the Part 2 rubric (CSS styling and responsive design). Each was fixed as the first step of Part 3.

| # | Problem found in Part 2 | Fix made in Part 3 | Files |
|---|---|---|---|
| 1 | **Mobile navigation:** below 600px every link was permanently stacked, pushing page content a full screen down. | Replaced with a hamburger "Menu" button (`aria-expanded`, Escape to close, resets when the window is widened). Without JavaScript the links simply stay visible. | `css/style.css`, `js/main.js`, all `.html` |
| 2 | **Inline `style="…"` attributes** were scattered through the HTML (hero eyebrow colours, grid overrides, image rounding), which defeats the external stylesheet. | All removed; replaced by named classes (`.eyebrow--light`, `.media-rounded`, `.grid--steps`, `.d1`–`.d5` …). An automated audit now checks every page for zero inline styles. | `css/style.css`, all `.html` |
| 3 | **Low colour contrast:** white text on the brand teal (`#1C9E8E`, about 3.3:1) in sitemap buttons, and image captions at 65% opacity. | Text and buttons now use the darker teal `#157A6E` (about 5.3:1 on white); captions use a solid `#5C5A66`. Meets WCAG AA for normal text. | `css/style.css` |
| 4 | **Invalid colour token:** `--color-ink-soft: #55536690` (an 8-digit hex that was never used). | Removed and replaced by a valid `--color-muted` token that is used. | `css/style.css` |
| 5 | **Header logo** used the full square logo, so the actual mark rendered tiny. | Cropped logo (`akhona-teens-logo-header.jpg`) at a readable height. | `images/`, all `.html` |
| 6 | **Images had no `width`/`height`** (layout shift while loading) and were not lazy-loaded. | Every `<img>` now has intrinsic dimensions, `loading="lazy"` (hero uses `fetchpriority="high"`), `decoding="async"` and WebP + JPEG sources. | all `.html` |
| 7 | **Footer differed from page to page** (the sitemap page had no links). | One footer template used on every page, with quick links, social links and share links. | all `.html` |
| 8 | **Social media names were plain text, not links.** | Real links with `target="_blank" rel="noopener noreferrer"`. | all `.html` |
| 9 | **Google Maps iframes** showed blank boxes when blocked and were not interactive. | Replaced by a Leaflet map (see Part 3 features below). | `contact.html`, `js/map.js` |
| 10 | **No keyboard shortcut past the menu** and weak focus styles. | Added a "Skip to main content" link and visible `:focus-visible` rings on links, buttons, chips, fields and tabs. | `css/style.css`, all `.html` |
| 11 | **Forms were visual only** (Part 2 styled them but nothing was validated or processed). | Full validation and processing added (see section C). | `js/forms.js`, `js/enquiry.js`, `js/contact.js` |

> If the lecturer's written feedback on Part 2 asked for further changes, add a row to this table with the date, the comment and the change made.

### B. Interactive elements and animation (Functionality 2.1)

- **Accordion**: FAQ on `enquiry.html`. Keyboard operable (Arrow keys, Home, End), `aria-expanded`, animated height. (`js/main.js`)
- **Tabs**: Mission / Vision / Values / Approach on `about.html`, following the WAI-ARIA tabs pattern (arrow keys, Home/End, deep links like `about.html#panel-values`). (`js/main.js`)
- **Modals**: team profiles on `about.html`, programme details on `services.html`, event details on `events.html`. Built on the native `<dialog>` element so focus is trapped, Escape closes it and focus returns to the button that opened it. (`js/main.js` → `ATLF.openModal`)
- **Interactive map** (Leaflet + OpenStreetMap) on `contact.html`: two markers with popups (address, hours, phone, directions), location cards that fly the map to each site, "Show both locations", a layers control that toggles shaded service areas, and **"Find my nearest"**, which uses the browser's geolocation (only after the visitor clicks) and the haversine formula to name the closest site and its distance. Scroll-wheel zoom stays off until the map is clicked so it never traps page scrolling. Leaflet is stored locally in `js/vendor/`. (`js/map.js`)
- **Gallery with lightbox** (`gallery.html`): category filters, a lightbox with previous/next buttons, ← → keys, swipe on touch screens, click-to-zoom, photo counter, preloading of neighbouring photos, and focus returning to the thumbnail. Only the photos in the current filter are in the sequence. (`js/gallery.js`)
- **Animations and transitions**: scroll-reveal fade-ups (IntersectionObserver), animated impact counters, hero text entrance, card hover lifts, accordion/tab/modal/lightbox transitions, header shadow on scroll, back-to-top button. All motion is switched off for visitors who prefer reduced motion.
- **DOM manipulation**: all dynamic lists, cards, modals, popups, error messages and form responses are built with `createElement`/`textContent` (never `innerHTML`), so typed text cannot inject markup.

### C. Dynamic content and search (Functionality 2.2)

- **Services page** (`services.html`): programmes are loaded from `js/data.js` and rendered by `js/services.js`. Search (multi-word, matches title, summary, tags, venue), category chips, venue filter, five sort orders, highlighted matches, a result count announced to screen readers, an empty state, and the search kept in the URL (`?q=money&category=core`) so it can be shared. A `<noscript>` list keeps the content available without JavaScript.
- **Events page** (`events.html`): upcoming/past chips, search, type filter, three sort orders, "Load more" pagination and a **downloadable `.ics` "Add to calendar"** file generated in the browser. "Upcoming" is worked out from today's date, so the list stays current.
- **Home page**: the next three upcoming events are loaded dynamically.

### D. Forms (Form Functionality)

Two forms with clearly different purposes, as the brief requires.

**`enquiry.html`: enrol, volunteer, mentor, sponsor or partner**
- Fields adapt to the chosen type (text, email, phone, date, number, select, radio groups, checkbox groups, textarea with live counter, consent checkboxes).
- Validation: required fields; name pattern; email format; South African phone formats; date of birth with **age check 13–19**; guardian name and consent appear and become required only when the teenager is under 18; programme/venue cross-check (the Academy runs at Kempton Park only); start date within the next 12 months; whole-rand amounts R50–R1,000,000; character limits.
- Errors appear next to each field (`aria-invalid`, `aria-describedby`) and in a summary whose links focus the field.
- **Processing and response:** after a valid submit the visitor sees a "processing" state, then a personalised response calculated from their answers: programme **cost**, next **start date** and registration deadline (enrol); matching **time slots**, hours per month and the **next orientation day after their start date** (volunteer/mentor); the **impact of the gift** (dignity packs, Academy places, hub costs), annual total, sponsor level and Section 18A eligibility (sponsor); first-call **availability** slots (partner). Includes a reference number and a pre-filled "Email this enquiry" button.

**`contact.html`: general message**
- Fields: name, email, phone, message type, preferred reply method, best time, subject, message (20–1000 characters with counter), consent.
- Cross-field rule: a phone number becomes required if the visitor asks for a phone or WhatsApp reply.
- After validation the information is **compiled into an email** (recipient, subject and body) shown for review, with **Open in my email app** (`mailto:` addressed to the recipient set in `js/config.js`), **Copy email text** and **Edit my message**.

**Both forms:** the `<form action>` attribute points at a `mailto:` address as a no-JavaScript fallback; client-side validation; **AJAX** submission with `fetch()` (JSON body, timeout, clear success and failure messages, never blocks the visitor); honeypot field against bots; the Submit button is disabled while sending to prevent double submission.

### E. Search engine optimisation

- **Unique `<title>` (44–61 characters) and `<meta name="description">` (about 150–160 characters) and `<meta name="keywords">` on every page**, each written around that page's real keywords.
- One `<h1>` per page with a logical `h2`/`h3` hierarchy (checked automatically).
- **Image SEO:** files renamed to descriptive keyword names (for example `teen-leadership-workshop-gauteng.jpg`), specific alt text on every image, explicit dimensions, WebP versions and `srcset`.
- **URLs and internal linking:** short descriptive file names; breadcrumbs; keyword-rich internal links (for example Home → `services.html?q=leadership`); sitemap page.
- **Structured data (JSON-LD):** `NGO` organisation (address, geo coordinates, opening hours, social profiles), `BreadcrumbList` and `FAQPage`.
- **Social sharing:** Open Graph and Twitter Card tags; share links (WhatsApp, Facebook, X) in the footer.
- **Local SEO:** Kempton Park and Hluvukani in titles, headings and structured data; NAP (name, address, phone) kept identical everywhere.
- **Files:** `robots.txt`, `sitemap.xml`, canonical URLs, favicon and touch icon, a custom `404.html`.

### F. Page speed and security

- Page speed: WebP images with responsive `srcset`, `loading="lazy"`, `fetchpriority="high"` on the hero, `defer` on all scripts, font `preconnect`/`display=swap`, Leaflet loaded only on the contact page, and long-term caching rules in `_headers`.
- Security: Content-Security-Policy meta tag (scripts allowed only from the site itself), referrer policy, `rel="noopener noreferrer"` on every external link, all user text inserted with `textContent`, input validation and length limits, honeypot, no secrets in the code, and Leaflet kept as a local file (no third-party script CDN). `_headers` adds X-Frame-Options, X-Content-Type-Options, HSTS and Permissions-Policy for hosts that support it.

### G. Repository and documentation

- Work committed in descriptive stages (see `git log`).
- `README.md` rewritten for Part 3 (features, SEO, security, deployment, testing, references).
- References updated.

### H. Testing carried out

- About 140 automated checks in a real headless browser (Chromium) covering navigation, counters, tabs, accordion, modals, search/filter/sort, calendar download, lightbox, map, both forms and their validation, AJAX success/failure, and behaviour with JavaScript switched off.
- Static audit of every page: title and description lengths, one `<h1>`, heading order, duplicate ids, alt text, image dimensions, inline styles, broken internal links, external-link safety.
- Horizontal-overflow check at desktop (1440), tablet (768) and mobile (375) widths on every page.
- Bugs found and fixed during testing: search icon overlapping placeholder text; programme photos giving cards different heights; lightbox caption clipped under tall photos; load-more button not centred; green "valid" border showing on empty optional fields.

---

## [Part 2] — CSS styling and responsive design

### Added
- External stylesheet `css/style.css` linked from every page, with a CSS reset and `:root` design tokens for colour, type and spacing.
- Brand colour palette taken from the Akhona Teens logo and posters; Sora (headings) and Source Sans 3 (body) typography with a type scale.
- Layout with CSS Grid (card grids, map grid) and Flexbox (header, nav, stats, footer).
- `:hover`, `:focus-visible` and `:active` states on links, buttons, cards and form fields.
- Two breakpoints (900px tablet, 600px mobile), relative units, `<picture>`/`srcset` hero and content images.
- Screenshot evidence of desktop, tablet and mobile layouts.

---

## [Part 1] — Content, structure and basic HTML

### Added
- Project folder structure (`css/`, `js/`, `images/`) and the five required pages (`index`, `about`, `services`, `enquiry`, `contact`) plus a sitemap page.
- Semantic HTML5 structure (`header`, `nav`, `main`, `section`, `article`, `footer`).
- Researched content for every page and the eight images from the `Research` folder.
- Enquiry form for volunteers, mentors, partners and sponsors; contact page with two locations.
- Navigation menu and README.
