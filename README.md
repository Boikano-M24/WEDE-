# Akhona Teens Leadership Foundation — Website

## Project Overview
This is an HTML/CSS prototype website built for **Akhona Teens Leadership Foundation**, a South African youth-development non-profit organisation. The project was completed in two phases:

- **Part 1 — Content & Structure:** basic page structure, content and images for each page (not yet responsive).
- **Part 2 — Designing the Visuals:** an external stylesheet applying layout, typography and colour; a fully responsive design across desktop, tablet and mobile; and improved user experience (readability, navigation, accessibility).

## Organisation Chosen
**Akhona Teens Leadership Foundation** — a youth-development NPO that equips teenagers (aged 13–19) with personal development, leadership, financial literacy and entrepreneurship skills.

## Folder / File Structure
```
AkhonaTeens_Website/
├── index.html              (Homepage)
├── about.html              (About Us)
├── services.html           (Services / Programmes)
├── enquiry.html            (Volunteer / Mentor / Partner / Sponsor enquiry form)
├── contact.html            (Contact details, two location maps, contact form)
├── sitemap.html            (Visual sitemap / supporting page)
├── css/
│   └── style.css           (Single external stylesheet, shared by every page)
├── js/
│   └── script.js           (Footer year + active nav-link highlighting)
├── images/
│   ├── logo-square.jpg
│   ├── logo-wide-banner.jpg
│   ├── hero-banner.jpg (+ -480w / -800w / -1200w responsive variants)
│   ├── team-group-photo.jpg (+ responsive variants)
│   ├── leadership-workshop.jpg (+ responsive variants)
│   ├── community-outreach.jpg
│   ├── volunteers-needed-poster.jpg (+ responsive variants)
│   ├── dignity-pack-supplies.jpg
│   └── screenshots/         (Responsive testing evidence — see Part 2 below)
├── README.md
└── CHANGELOG.md
```

## Sitemap
See `sitemap.html` for a visual representation of the site's hierarchy. In summary:

- **index.html** (Home) links to → about.html, services.html, enquiry.html, contact.html
- All pages share the same navigation menu and footer, so every page is reachable from every other page.
- `sitemap.html` is a supporting page (not part of the main navigation menu) that documents the overall structure.

## Pages and Content

| Page | Purpose |
|---|---|
| `index.html` | Hero image, brief introduction, impact stats, and calls to action |
| `about.html` | Organisation history, founder profile, team members, mission and vision |
| `services.html` | Detailed information on programmes: Personal Development, Leadership Development, Financial Literacy, Teen Entrepreneurship, and community initiatives |
| `enquiry.html` | Enquiry form for volunteering, mentoring, partnering or sponsoring (this NPO's equivalent of a product/service enquiry) |
| `contact.html` | Contact details, two embedded maps (Kempton Park head office and Hluvukani Innovation Hub), and a general contact form |
| `sitemap.html` | Visual overview of the site structure |

## Content Sources
1. **Organisation's own material** — logos, campaign posters and the homepage hero banner were sourced from the `Research` folder supplied for this project (originally gathered from the organisation's public social media presence).
2. **Social media** — photographs of programme activities, the team, and campaign material (e.g. the "Volunteers Needed" poster and dignity-pack campaign photos) were sourced from the organisation's public social media posts, saved in the `Research` folder.
3. **Original content** — page copy (mission and vision statements, programme descriptions, team member profiles, contact details, and campaign summaries) was written specifically for this website prototype based on the organisation's publicly known focus areas and campaigns.

---

## Part 2: Designing the Visuals

### 1. CSS Styling
- A single **external stylesheet** (`css/style.css`) is linked from every page, using a consistent naming convention.
- A **CSS reset** (`*`, `box-sizing`, margin/padding zeroing) establishes consistent cross-browser behaviour before custom styles apply.
- A **base style** sets default font family, font size, colour scheme and spacing via CSS custom properties (design tokens) defined once in `:root`.
- **Typography**: two Google Fonts are used — `Sora` for headings (bold, geometric, echoes the brand wordmark) and `Source Sans 3` for body text — set with a deliberate type scale (`--step-0` to `--step-4`), consistent `font-weight`, `line-height` and `letter-spacing`.
- **Colour palette** is drawn directly from the organisation's own logo and campaign material rather than a generic template palette:

  | Token | Hex | Use |
  |---|---|---|
  | `--color-teal` | `#1C9E8E` | Primary brand colour, links, accents |
  | `--color-purple` | `#4A2E83` | Headings, hero gradient |
  | `--color-amber` | `#F2A93B` | Buttons / calls to action |
  | `--color-ink` | `#211F2B` | Body text, footer background |
  | `--color-paper` | `#FAF8F5` | Page background |

- **Layout structure** uses both **CSS Grid** (card grids for programmes/team/community initiatives, the two-column map grid) and **Flexbox** (header bar, stat row, nav menu, footer links) — see `display: grid`, `grid-template-columns`, `display: flex`, `justify-content`, `align-items` throughout `style.css`.
- **Visual styling**: `border-radius`, `box-shadow`, gradients (`linear-gradient`) on the page hero, and `border-left` accent stripes on stat cards.
- **Pseudo-classes**: `:hover`, `:focus-visible` and `:active` are used on nav links, buttons, cards and form fields to create interactive, accessible states (e.g. buttons lift on hover, form fields get a visible teal focus ring, nav links get an amber underline on hover).

### 2. Responsive Design
- **Breakpoints**: two media queries — `max-width: 900px` (tablet) and `max-width: 600px` (mobile) — adapt the layout, typography scale and navigation.
- **Layout adjustments**: two-column "split" sections (hero text/image, campaign features) collapse to a single column on tablet; three-column card grids (`grid--3`) become two columns on tablet and one column on mobile; the two-location map grid stacks vertically below 900px.
- **Navigation adjustments**: the horizontal nav menu becomes a full-width, stacked vertical menu on mobile screens.
- **Typography adjustments**: heading sizes (`--step-3`, `--step-4`, etc.) scale down at each breakpoint so large headlines don't overwhelm small screens.
- **Relative units**: spacing and type use `rem`/`em`, grid columns use `fr` and `%`-based `minmax()`, so the layout is not pinned to fixed pixel widths.
- **Responsive images**: the homepage hero image and several content photos use `<picture>`/`srcset`/`sizes` with three generated resolutions (480w / 800w / 1200w) so smaller screens download smaller files instead of the full-size original.

### 3. Test and Iterate — Screenshot Evidence
The website was tested at three screen sizes using real browser rendering (a headless Chromium instance) rather than only resizing a window by eye, to get pixel-accurate evidence:

| Breakpoint | Viewport used |
|---|---|
| Desktop | 1440 × 900 |
| Tablet | 768 × 1024 |
| Mobile | 375 × 812 |

Screenshots for the homepage, Services page and Contact page are saved in `images/screenshots/`:

**Homepage (`index.html`)**
- Desktop: `screenshots/index-desktop.png`
- Tablet: `screenshots/index-tablet.png`
- Mobile: `screenshots/index-mobile.png`

**Services (`services.html`)** — demonstrates the 3-column → 2-column → 1-column card grid
- Desktop: `screenshots/services-desktop.png`
- Tablet: `screenshots/services-tablet.png`
- Mobile: `screenshots/services-mobile.png`

**Contact (`contact.html`)** — demonstrates the two-location map grid and form collapsing
- Desktop: `screenshots/contact-desktop.png`
- Tablet: `screenshots/contact-tablet.png`
- Mobile: `screenshots/contact-mobile.png`

> **Note on the screenshots:** these were captured in a sandboxed development environment with restricted internet access. As a result, two things don't render in the screenshots but **will render normally** when the site is opened with a normal internet connection: (1) the Google Fonts (Sora / Source Sans 3) fall back to the system sans-serif font, and (2) the two embedded Google Maps `iframe`s show a network message instead of the live map tiles. The HTML/CSS itself is unaffected — layout, grid/flexbox behaviour, and breakpoints all render and reflow correctly, which is what this testing step is verifying.

Across all three breakpoints, the header/navigation, hero, stat row, card grids, image/text split sections, map grid, forms and footer all reflow correctly with no horizontal scrollbars or overlapping content.

## Technologies Used
- HTML5 (semantic elements: `header`, `nav`, `main`, `section`, `article`, `footer`)
- CSS3 (Grid, Flexbox, custom properties, media queries, pseudo-classes, gradients)
- Google Fonts (Sora, Source Sans 3)
- Minimal JavaScript (footer year, active nav-link highlighting)

## How to View the Website
Open `index.html` in any modern web browser (with an internet connection, so Google Fonts and Google Maps load). All internal links use relative paths, so the whole `AkhonaTeens_Website` folder should be kept together.

## References
- Akhona Teens Leadership Foundation — publicly available organisational information and campaign material (logos, posters and photographs supplied via the project's `Research` folder).
- Google Fonts: Sora — https://fonts.google.com/specimen/Sora
- Google Fonts: Source Sans 3 — https://fonts.google.com/specimen/Source+Sans+3
- Google Maps Embed (no API key) — https://www.google.com/maps

See `CHANGELOG.md` for a record of development steps.
