# Akhona Teens Leadership Foundation: Website

A multi-page website for **Akhona Teens Leadership Foundation**, a South African youth-development non-profit that equips teenagers (13–19) with leadership, financial-literacy and entrepreneurship skills in Gauteng and Mpumalanga.

| | |
|---|---|

| **Built with** | HTML5, CSS3 (Grid, Flexbox), vanilla JavaScript (no framework), Leaflet 1.9.4 |

The project was built in three parts. This README documents all of them; Part 3 is described in most detail.

| Part | Focus |
|---|---|
| 1 | Content research, page structure, images |
| 2 | CSS styling and responsive design |
| 3 | JavaScript functionality, SEO, validated forms, deployment |

All changes, including corrections made to Part 2, are recorded in [`CHANGELOG.md`](CHANGELOG.md).

---

## Contents
1. [Pages and sitemap](#pages-and-sitemap)
2. [Folder structure](#folder-structure)
3. [Part 3 features](#part-3-features)
4. [Forms](#forms)
5. [SEO](#seo)
6. [Security](#security)
7. [Responsive design and screenshot evidence](#responsive-design-and-screenshot-evidence)
8. [Testing](#testing)
9. [Setup and configuration](#setup-and-configuration)
10. [Deployment](#deployment)
11. [Git workflow](#git-workflow)
12. [Content sources](#content-sources)
13. [References](#references)

---

## Pages and sitemap

```
index.html (Home)
├── about.html      About Us: story, timeline, tabs, team, modals
├── services.html   Services: searchable / sortable programme list
├── events.html     Events: filterable list, add to calendar
├── gallery.html    Gallery: filters + lightbox
├── enquiry.html    Enquiry: adaptive form with instant response, FAQ accordion
├── contact.html    Contact: interactive map, validated message form
├── sitemap.html    Visual sitemap
└── 404.html        Page-not-found page
```

Every page is one click from the home page and shares the same header navigation and footer. The five pages required by the brief are `index`, `about`, `services`, `enquiry` and `contact`; `events`, `gallery`, `sitemap` and `404` are extra pages documented here and in the changelog.

## Folder structure

```
AkhonaTeens_Website/
├── index.html  about.html  services.html  events.html  gallery.html
├── enquiry.html  contact.html  sitemap.html  404.html
├── robots.txt  sitemap.xml  _headers  .nojekyll
├── favicon.ico  favicon.png  apple-touch-icon.png
├── css/
│   ├── style.css              single shared stylesheet (reset, tokens, components, breakpoints)
│   └── leaflet/               Leaflet stylesheet + marker images (BSD-2 licence)
├── js/
│   ├── init.js                adds a "js" class so CSS knows JavaScript is available
│   ├── config.js              form settings (recipient email, AJAX endpoint, sponsorship costs)
│   ├── data.js                programmes, events and locations used by the dynamic pages
│   ├── main.js                nav, scroll reveal, counters, tabs, accordion, modals, share links
│   ├── services.js            programme search / filter / sort
│   ├── events.js              events list, pagination, .ics calendar export
│   ├── gallery.js             gallery filter + lightbox
│   ├── map.js                 Leaflet map, nearest-location finder
│   ├── forms.js               validation engine, AJAX helper, counters
│   ├── enquiry.js             enquiry form logic + personalised responses
│   ├── contact.js             contact form logic + compiled email
│   └── vendor/                leaflet.js (kept local) + licence
├── images/                    descriptive file names, JPEG + WebP at 480/800/1200px
│   └── screenshots/
│       ├── responsive/        every page at desktop / tablet / mobile
│       └── features/          interactive features in action
├── README.md
└── CHANGELOG.md
```

Folders and files use lowercase, hyphenated, descriptive names throughout.

---

## Part 3 features

| Requirement | Where | How it works |
|---|---|---|
| **Accordion** | `enquiry.html` (FAQ) | Buttons with `aria-expanded`; panels animate with CSS grid rows; arrow keys move between questions. |
| **Tabs** | `about.html` | WAI-ARIA tab pattern with arrow keys, Home/End and deep links (`about.html#panel-values`). |
| **Modals** | `about.html`, `services.html`, `events.html` | Native `<dialog>`: focus trapped, Escape closes, focus returns to the trigger. |
| **Interactive map** | `contact.html` | Leaflet + OpenStreetMap; two markers with popups, fly-to cards, "Show both", service-area layer toggle, **Find my nearest** (geolocation + haversine distance). |
| **Animations and transitions** | all pages | Scroll-reveal (IntersectionObserver), animated counters, hover lifts, modal/tab/accordion transitions, sticky-header shadow. Disabled for `prefers-reduced-motion`. |
| **Advanced DOM manipulation** | `services.js`, `events.js`, `forms.js` … | Cards, popups, error messages and responses are created with `createElement`/`textContent`; no `innerHTML`. |
| **Gallery with lightbox** | `gallery.html` | Filter chips; lightbox with buttons, ← → keys, swipe, zoom, counter, preloading, focus return. |
| **Dynamic content** | `services.html`, `events.html`, home page | Rendered from `js/data.js`; "upcoming" is calculated from today's date. |
| **Search, filter and sort** | `services.html`, `events.html` | Multi-word search with highlighting, chips, venue/type filters, 5 and 3 sort orders, result count, empty state, URL state. |
| **Add to calendar** | `events.html` | Generates a standards-compliant `.ics` file in the browser. |

Everything is a progressive enhancement: with JavaScript switched off, content stays readable (tabs and FAQ show all text, `<noscript>` lists the programmes and events, the map area lists the addresses, and the forms remain usable).

### Screenshots of the features

| | | |
|---|---|---|
| ![Search with highlighting](images/screenshots/features/01-services-search-highlight.jpg) | ![Programme details modal](images/screenshots/features/02-programme-details-modal.jpg) | ![About tabs](images/screenshots/features/03-about-tabs.jpg) |
| Services search (highlighted matches) | Details modal | Tabs |
| ![FAQ accordion](images/screenshots/features/04-faq-accordion.jpg) | ![Gallery lightbox](images/screenshots/features/05-gallery-lightbox.jpg) | ![Interactive map](images/screenshots/features/06-interactive-map.jpg) |
| FAQ accordion | Gallery lightbox | Interactive map with popup |
| ![Events search](images/screenshots/features/11-events-search.jpg) | ![Mobile menu](images/screenshots/features/12-mobile-hamburger-menu.jpg) | |
| Events search | Mobile hamburger menu | |

> The map screenshot shows a plain background because the tool used to capture it blocks the OpenStreetMap tile server. On a normal internet connection the standard map tiles load.

---

## Forms

The two forms differ in purpose, as the brief requires.

### `enquiry.html`: enquire about services, volunteering or sponsoring
1. Choose what you want to do: **enrol a teenager, volunteer, become a mentor, sponsor/donate, or partner**. Only the relevant fields appear.
2. Validation (client-side JavaScript on top of HTML5 attributes):
   - required fields, name pattern, email format, **South African phone formats**
   - teenager's **date of birth → age must be 13–19**; guardian name and consent appear and become required only for under-18s
   - cross-field rule: programme and venue must match (the Academy runs in Kempton Park only)
   - start date between today and 12 months ahead; sponsorship amount a whole number from R50 to R1,000,000; character limits
3. Errors appear beside each field (with `aria-invalid` and `aria-describedby`) and in a summary whose links jump to the field.
4. After a valid submit: a "processing" state, an **AJAX** request, then a **personalised response**:

| Enquiry | The response states |
|---|---|
| Enrol | programme **cost** (free), **next start date**, registration deadline, venue, what to bring, guardian consent note |
| Volunteer | matching **time slots**, estimated hours a month, the **next orientation day on or after the chosen start date**, screening timeline, cost |
| Mentor | mentoring commitment, next orientation, matching process |
| Sponsor | **impact of the gift** (e.g. R500/month = 5 dignity packs a month), annual total, sponsor level, Section 18A eligibility, payment timeline |
| Partner | three first-call **availability** slots, response timeline |

Each response has a reference number and a pre-filled "Email this enquiry" button.

| Validation errors | Personalised response | Sponsorship response |
|---|---|---|
| ![Validation](images/screenshots/features/07-enquiry-validation-errors.jpg) | ![Volunteer response](images/screenshots/features/08-enquiry-personalised-response.jpg) | ![Sponsor response](images/screenshots/features/09-sponsorship-response.jpg) |

### `contact.html`: general message
Collects contact details, message type, preferred reply method, best time, subject and message (20–1000 characters, live counter). A phone number becomes required if the visitor asks for a phone or WhatsApp reply. After validation the details are **compiled into an email** (recipient, subject and body) shown for review. The visitor can **open it in their email app** (a `mailto:` link addressed to the recipient in `js/config.js`), **copy the text**, or **edit the message**.

![Compiled email](images/screenshots/features/10-contact-compiled-email.jpg)

### Both forms
- The `<form action>` attribute holds a `mailto:` address as the no-JavaScript fallback.
- **AJAX:** data is sent with `fetch()` as JSON, with a timeout and clear success or failure feedback. The site never gets stuck: if the request fails the visitor is told and can still send the compiled email.
- Honeypot field for bots; submit button disabled while sending; consent (POPIA) checkbox.

> **Important:** a static website cannot send email by itself, so the visitor's own email app performs the final send. The default AJAX endpoint (`jsonplaceholder.typicode.com`) is a free **testing** service that echoes the request and **stores nothing**, so no one receives these submissions. See [Setup and configuration](#setup-and-configuration) to connect a real form service.

---

## SEO

### On-page SEO

| Technique | Implementation |
|---|---|
| **Keyword research** | Primary phrases: *teen leadership programme*, *youth development NPO South Africa*, *Kempton Park*, *Gauteng*, *Mpumalanga*, *teenage entrepreneurship*, *financial literacy for teens*, *volunteer / mentor*, *dignity packs*. Chosen from how parents, volunteers, schools and donors would search, and placed in titles, headings, body text, alt text and links. |
| **Title tags** | Unique on every page, 44–61 characters, keyword first, brand last. |
| **Meta descriptions** | Unique, about 150–160 characters, written as a call to action. |
| **Meta keywords** | Page-specific list on every page (included for completeness; major search engines no longer use them). |
| **Header tags** | Exactly one `<h1>` per page, logical `<h2>`/`<h3>` order (verified automatically). |
| **Image optimisation** | Descriptive file names (`teen-leadership-workshop-gauteng.jpg`), specific alt text, `width`/`height`, WebP + JPEG, `srcset`/`sizes`, lazy loading. |
| **URL structure** | Short, lowercase, hyphenated, descriptive (`services.html`, `events.html`); canonical URL on every page. |
| **Internal linking** | Descriptive anchor text, breadcrumbs, footer links, deep links such as `services.html?q=leadership` and `enquiry.html?type=sponsor`. |
| **Mobile-friendliness** | Responsive layout, viewport meta tag, hamburger menu, full-width buttons on phones, no horizontal scrolling at 375px. |
| **Structured data** | JSON-LD: `NGO` (address, geo, hours, social profiles), `BreadcrumbList`, `FAQPage`. |
| **Social previews** | Open Graph and Twitter Card tags with a page-specific image and alt text. |
| **Accessibility (helps SEO)** | Landmarks, skip link, ARIA patterns, visible focus, colour contrast, `lang="en-ZA"`. |

### Off-page SEO (plan and what is built in)

Off-page work happens outside the site; the site is built to support it.

| Area | Action plan | Supported in the site |
|---|---|---|
| **Backlinks** | Ask partner schools, churches and The Character Company to link to the site; list the NPO on South African non-profit directories, local Kempton Park business and community pages, and volunteer-matching sites; publish event write-ups that local news and school newsletters can link to. | Press-ready events and gallery pages, partner page text, outbound link to a real partner. |
| **Social media** | Post every event and campaign to Facebook, Instagram, LinkedIn and X with a link back to the matching page. | Social profile links, share buttons (WhatsApp, Facebook, X), Open Graph/Twitter cards so shared links show a title, description and image. |
| **Local SEO** | Claim and verify a Google Business Profile for the Kempton Park head office; keep the name, address and phone number (NAP) identical on every listing; ask volunteers and parents for reviews. | Kempton Park and Hluvukani in titles and headings, `NGO` structured data with geo coordinates, NAP identical in the footer, contact page and JSON-LD, interactive map. |

### Additional SEO requests

| Item | File / approach |
|---|---|
| **robots.txt** | `robots.txt`: allows crawling, blocks `404.html` and the vendor folder, links to the sitemap. |
| **sitemap.xml** | `sitemap.xml`: all eight indexable pages with priority, change frequency and last-modified date. |
| **Page speed** | WebP images, `srcset`, lazy loading, `fetchpriority="high"` on the hero, deferred scripts, `preconnect` to fonts, Leaflet only loaded on the contact page, cache headers in `_headers`, no large libraries or frameworks. |
| **Security** | See the next section. |

---

## Security

| Measure | Detail |
|---|---|
| Content-Security-Policy | Meta tag on every page (and in `_headers`): scripts may only come from the site itself; no inline scripts. |
| No third-party script CDN | Leaflet is stored locally (`js/vendor/`), so a compromised CDN cannot inject code. |
| Safe output | All visitor text is inserted with `textContent`, never `innerHTML`, so it cannot inject HTML. Verified with a test that a `<img onerror=…>` name is rejected and nothing runs. |
| Input validation | Formats, lengths and ranges checked in the browser; `maxlength` limits on every field. Server-side validation would be required for any real back end. |
| Bot protection | Hidden honeypot field; submit button disabled while a request is running. |
| External links | `rel="noopener noreferrer"` on every link that opens a new tab. |
| Privacy | Geolocation is requested only when the visitor clicks "Find my nearest" and is never sent anywhere; POPIA consent checkbox on both forms. |
| Headers | `_headers` adds `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy` and HSTS on hosts that read it (Netlify, Cloudflare Pages). GitHub Pages ignores this file, but the CSP and referrer policy still apply through the `<meta>` tags. |
| HTTPS | Provided automatically by GitHub Pages and Netlify. |

---

## Responsive design and screenshot evidence

Layout uses CSS Grid and Flexbox with three ranges: **desktop**, **tablet (≤ 900px)** and **mobile (≤ 600px)**. Navigation collapses into a hamburger menu at 900px; multi-column grids reduce to two then one column; typography scales; images use `srcset`/`sizes` and `<picture>` with WebP.

Pages were captured at desktop (1440×900), tablet (768×1024) and mobile (375×812) in a real browser engine, and an automated check confirmed that **no page scrolls horizontally at any size**.

| Page | Desktop | Tablet | Mobile |
|---|---|---|---|
| Home | [view](images/screenshots/responsive/index-desktop.jpg) | [view](images/screenshots/responsive/index-tablet.jpg) | [view](images/screenshots/responsive/index-mobile.jpg) |
| About | [view](images/screenshots/responsive/about-desktop.jpg) | [view](images/screenshots/responsive/about-tablet.jpg) | [view](images/screenshots/responsive/about-mobile.jpg) |
| Services | [view](images/screenshots/responsive/services-desktop.jpg) | [view](images/screenshots/responsive/services-tablet.jpg) | [view](images/screenshots/responsive/services-mobile.jpg) |
| Events | [view](images/screenshots/responsive/events-desktop.jpg) | [view](images/screenshots/responsive/events-tablet.jpg) | [view](images/screenshots/responsive/events-mobile.jpg) |
| Gallery | [view](images/screenshots/responsive/gallery-desktop.jpg) | [view](images/screenshots/responsive/gallery-tablet.jpg) | [view](images/screenshots/responsive/gallery-mobile.jpg) |
| Enquiry | [view](images/screenshots/responsive/enquiry-desktop.jpg) | [view](images/screenshots/responsive/enquiry-tablet.jpg) | [view](images/screenshots/responsive/enquiry-mobile.jpg) |
| Contact | [view](images/screenshots/responsive/contact-desktop.jpg) | [view](images/screenshots/responsive/contact-tablet.jpg) | [view](images/screenshots/responsive/contact-mobile.jpg) |

| Desktop | Tablet | Mobile |
|---|---|---|
| ![Home desktop](images/screenshots/responsive/index-desktop.jpg) | ![Home tablet](images/screenshots/responsive/index-tablet.jpg) | ![Home mobile](images/screenshots/responsive/index-mobile.jpg) |

> The screenshots were captured in an environment that blocks Google Fonts and map tiles, so text appears in the fallback system font and maps show a plain background. Online, the Sora and Source Sans 3 fonts and the OpenStreetMap tiles load normally.

---

## Testing

| What | Result |
|---|---|
| Functional tests in a headless Chromium browser (navigation, counters, reveal, tabs, accordion, modals, search/filter/sort, URL state, `.ics` download, lightbox, map, geolocation, both forms, validation, AJAX success and failure, XSS attempt, honeypot) | About 140 checks, all passing |
| JavaScript disabled | Content, navigation and forms remain usable |
| Static audit of all pages (titles, descriptions, keywords, canonical, Open Graph, one `<h1>`, heading order, duplicate ids, alt text, image dimensions, inline styles, broken links, external-link safety) | 0 problems |
| Horizontal overflow at 1440 / 768 / 375 px | None on any page |
| Console errors and CSP violations when opened from disk | None |

**Cross-browser note:** the automated tests ran in Chromium (the engine behind Chrome and Edge). The features used (`<dialog>`, CSS Grid, `fetch`, IntersectionObserver, `<picture>`) are supported by current Chrome, Edge, Firefox and Safari. Please also open the site in Firefox and Safari (or on a phone) before submission and note anything unusual in the changelog.

---

## Setup and configuration

**View locally:** unzip the folder and double-click `index.html`. Keep the folders together. An internet connection is needed for fonts and map tiles.

**Form settings: `js/config.js`**

```js
recipientEmail: "info@akhonateens.co.za",   // who compiled emails are addressed to
ajaxEndpoint:   "https://jsonplaceholder.typicode.com/posts",  // test service; "" turns AJAX off
costs: { dignityPack: 85, academySeat: 1200, hubMonth: 3500 }  // used in sponsorship responses
```

- While testing, change `recipientEmail` to **your own address** so test emails do not go to the organisation.
- For real delivery without a server, replace `ajaxEndpoint` with a form service such as `https://formsubmit.co/ajax/your-address@example.com` (it asks you to confirm the address once). The Content-Security-Policy tag in each page already allows `formsubmit.co`; for any other service, add its address to `connect-src` in the `<head>` of `enquiry.html` and `contact.html` (and in `_headers`).

**Content:** programmes, events and locations live in `js/data.js`. Add an object to the `events` array and it appears on the Events page and, when it is among the next three, on the home page.

**Set the live URL:** canonical links, social-sharing tags, `sitemap.xml` and `robots.txt` contain the placeholder `https://YOUR-USERNAME.github.io/akhona-teens-website`. After you know your real address, use *Find and Replace in Files* (VS Code: `Ctrl+Shift+H`) to replace that text with your real URL in all files.

---

## Deployment

### GitHub Pages (free)
1. Push the repository to GitHub (see below).
2. On GitHub open **Settings → Pages**.
3. Under **Build and deployment** choose **Deploy from a branch**, select **main** and folder **/ (root)**, then **Save**.
4. After a minute the site is live at `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`.
5. Replace the placeholder URL (previous section), commit and push.
6. Open every page on the live site to confirm there are no errors.

### Netlify (alternative)
Drag the project folder onto https://app.netlify.com/drop. Netlify reads `_headers` automatically and applies the security headers.

### After deploying
- Submit `/sitemap.xml` in Google Search Console.
- Run the live URL through a page-speed and an accessibility checker.
- Add the live link to the table at the top of this README and submit it with the GitHub repository link.

---

## Git workflow

Commits are small, in logical steps, with messages that describe the change (for example *"Add gallery lightbox: category filter, keyboard/swipe navigation, zoom, focus return"*). View them with `git log --oneline`.

```
git add .
git commit -m "Describe what changed and why"
git push
```

---

## Content sources

1. **Organisation material:** logo, campaign posters, banner and photographs from the supplied `Research` folder (the organisation's public social media).
2. **Public information:** the organisation's public profile and partner information.
3. **Original content:** page copy, programme and event details, team profiles, timeline, sponsorship figures and FAQs were written for this project. Names, schedules, prices and statistics not found in the research are illustrative.
4. **Free resources:** OpenStreetMap map data, Leaflet library, Google Fonts (see references).

---

## References

Agafonkin, V. (2023) *Leaflet: an open-source JavaScript library for mobile-friendly interactive maps* (version 1.9.4). Available at: https://leafletjs.com/ .

Akhona Teens Leadership Foundation (2026) *Logo, campaign posters and photographs* [images supplied in the project's Research folder, from the organisation's public social media].

Google (2026) *Google Fonts: Sora* and *Source Sans 3*. Available at: https://fonts.google.com/ .

Google Search Central (2026) *SEO starter guide*. Available at: https://developers.google.com/search/docs/fundamentals/seo-starter-guide.

JSONPlaceholder (2026) *Free fake and reliable API for testing and prototyping*. Available at: https://jsonplaceholder.typicode.com/.

Mozilla (2026a) *The Dialog element*. MDN Web Docs. Available at: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog.

Mozilla (2026b) *Intersection Observer API*. MDN Web Docs. Available at: https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API (Accessed: 28 October 2026).

Mozilla (2026c) *Fetch API*. MDN Web Docs. Available at: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API.

Mozilla (2026d) *Geolocation API*. MDN Web Docs. Available at: https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API.

Mozilla (2026e) *The Picture element*. MDN Web Docs. Available at: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture.

OpenStreetMap contributors (2026) *OpenStreetMap*. Available at: https://www.openstreetmap.org/copyright.

Republic of South Africa (2013) *Protection of Personal Information Act 4 of 2013*. Pretoria: Government Printer.

Schema.org (2026) *NGO*. Available at: https://schema.org/NGO.

Sinnott, R.W. (1984) 'Virtues of the haversine', *Sky and Telescope*, 68(2), p. 159.

Sitemaps.org (2026) *Sitemaps XML format*. Available at: https://www.sitemaps.org/protocol.html.

The Character Company (n.d.) *Programme partners*. Available at: https://www.thecharactercompany.co.za/program-partner.

W3C (2023) *Web Content Accessibility Guidelines (WCAG) 2.2*. Available at: https://www.w3.org/TR/WCAG22/.

W3C (2026) *WAI-ARIA Authoring Practices Guide: Tabs, Accordion and Dialog (Modal) patterns*. Available at: https://www.w3.org/WAI/ARIA/apg/patterns/ .

Wikipedia (2026) *Hluvukani*. Available at: https://en.wikipedia.org/wiki/Hluvukani 