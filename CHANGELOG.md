# Changelog
All notable changes to the Akhona Teens Leadership Foundation website are documented in this file.

## [Part 2] — CSS Styling & Responsive Design
### Added
- External stylesheet `css/style.css` linked from every page, with a CSS reset and `:root` design-token system for colour, type and spacing.
- Google Fonts integration (Sora for headings, Source Sans 3 for body text).
- Header/navigation built with Flexbox; active-page nav state added.
- Gradient page-hero band for interior pages (About, Services, Enquiry, Contact, Sitemap).
- CSS Grid card layouts for programmes, team members and community initiatives.
- Two-column, then responsive, map grid on the Contact page.
- Button, link, form-field and card interactive states using `:hover`, `:focus-visible` and `:active`.
- Responsive image variants (480w / 800w / 1200w) generated for the hero banner, leadership workshop, team photo and volunteers poster, wired up with `<picture>`/`srcset`/`sizes`.
- Two responsive breakpoints (900px tablet, 600px mobile) adjusting layout, typography scale and navigation.
- `js/script.js` extended to mark the current page's nav link as active.
- Screenshot evidence of desktop/tablet/mobile rendering added to `images/screenshots/` and documented in `README.md`.

### Changed
- Rebuilt all six HTML pages (`index`, `about`, `services`, `enquiry`, `contact`, `sitemap`) to use the new CSS class structure (`.wrap`, `.section`, `.card`, `.grid`, `.split`, `.btn`, etc.) in place of Part 1's unstyled markup.
- Replaced Part 1's plain HTML tables/lists for team members and programmes with Grid-based card components.

### Feedback Addressed
- N/A for this submission — this is the first graded milestone for Part 2. Any tutor/marker feedback received on Part 1 will be logged here in a future entry, together with the specific changes made in response.

## [Part 1] — Content, Structure and Basic HTML
### Added
- Initial project folder structure: root HTML files plus `css/`, `js/` and `images/` folders.
- Five core pages created: `index.html`, `about.html`, `services.html`, `enquiry.html`, `contact.html`, plus a supporting `sitemap.html`.
- Basic semantic HTML structure on every page (`header`, `nav`, `main`, `section`/`article`, `footer`).
- Organisation content researched and written for every page (history, mission, vision, team, programmes, community initiatives, contact details).
- All eight images from the `Research` folder copied into `images/` with clear, descriptive filenames.
- Enquiry form built for an NPO context (volunteer / mentor / partner / sponsor options).
- Contact page built with two location entries (Kempton Park head office, Hluvukani Innovation Hub) ready for embedded maps.
- Basic navigation menu linking all pages, repeated consistently in the footer.
