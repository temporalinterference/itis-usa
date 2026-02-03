# Architecture

**Analysis Date:** 2026-02-03

## Pattern Overview

**Overall:** Static Site Generation (SSG) with card-based content architecture

**Key Characteristics:**
- Hugo-based static site generator with module system
- Content organized as discrete "cards" representing individual pages/sections
- Dual-file pattern for each card: `index.md` (card metadata/teaser) and `modal.md` (expanded content)
- Theme imported as a Hugo module (`z43-cards-theme`)
- Asset pipeline for SCSS compilation and image optimization
- No database or server-side processing required

## Layers

**Content Layer:**
- Purpose: Manages site content organized by card
- Location: `/home/oetiker/checkouts/itis-usa/content/cards/`
- Contains: Markdown files defining individual cards and their expanded modal content
- Depends on: Hugo content schema, z43-cards-theme module
- Used by: Hugo renderer, theme layouts

**Presentation Layer:**
- Purpose: Handles theming, styling, and UI framework integration
- Location: Imported via module `github.com/temporalinterference/z43-cards-theme`
- Contains: HTML layouts, templates, JavaScript, and UIKit CSS framework
- Depends on: UIKit framework, custom SCSS overrides
- Used by: Hugo template engine to render content

**Asset Layer:**
- Purpose: Manages site-specific styling, fonts, and images
- Location: `/home/oetiker/checkouts/itis-usa/assets/`
- Contains: Custom SCSS, font files, images (logo, banner, favicons)
- Depends on: Hugo asset pipeline, Dart Sass compiler
- Used by: SCSS processor, static build output

**Configuration Layer:**
- Purpose: Defines site metadata, theme parameters, and build settings
- Location: `/home/oetiker/checkouts/itis-usa/config.yaml`
- Contains: Base URL, language, site title, color scheme, card dimensions, font definitions
- Depends on: Hugo configuration schema
- Used by: Hugo build process, theme parameterization

## Data Flow

**Build Process:**

1. Hugo reads site configuration from `config.yaml`
2. Hugo loads module dependencies (z43-cards-theme) via Go module system
3. Hugo processes content from `content/cards/` - each card directory contains `index.md` and `modal.md`
4. Theme layouts render cards using parameters from `config.yaml` (colors, fonts, card dimensions)
5. Asset pipeline processes `assets/scss/custom.scss` with Dart Sass compiler
6. Hugo generates static HTML files to `public/` directory

**Content Rendering:**

- Each card directory maps to a URL route under `/cards/`
- `index.md` provides card title and teaser text (displayed on grid view)
- `modal.md` provides expanded content with id field for modal identification
- Modal images are referenced inline (e.g., `{{< modal-image filename.jpg >}}`)
- Markdown supports Hugo shortcodes for dynamic content (modal-image, etc.)

**State Management:**
- No client-side state - fully static HTML
- Navigation and card selection handled by browser navigation to URLs

## Key Abstractions

**Card:**
- Purpose: Represents a single piece of content (page, news item, publication, etc.)
- Examples: `content/cards/history/`, `content/cards/news-20250710-SPARC-Seminar/`, `content/cards/publication-1/`
- Pattern: Each card is a directory containing `index.md` (teaser), `modal.md` (expanded), and optional assets

**News Card:**
- Purpose: Specialized card for time-sensitive announcements
- Examples: `content/cards/news-20250710-SPARC-Seminar/`, `content/cards/news-20250820-FDA-Approval/`
- Pattern: Prefixed with `news-` followed by date or event identifier; includes image assets for teasers

**Modal Pattern:**
- Purpose: Displays expanded content in an overlay without page navigation
- Pattern: `modal.md` file in each card directory with `id: <card-id>` front matter
- Content: Structured markdown with headings, lists, and Hugo shortcodes

## Entry Points

**Site Home:**
- Location: Root URL `/` or `index.html`
- Triggers: Browser navigation to site URL
- Responsibilities: Displays grid of all cards (via theme layout)

**Card Pages:**
- Location: `/cards/{card-id}/` (e.g., `/cards/history/`, `/cards/publication-1/`)
- Triggers: Browser navigation to card URL or card selection from grid
- Responsibilities: Renders card page with teaser and modal overlay functionality

**Static Assets:**
- Location: `/css/`, `/js/`, `/fonts/`, `/images/`, `/uikit/`
- Triggers: Browser requests for stylesheets, scripts, fonts
- Responsibilities: Serve compiled CSS from SCSS, UIKit framework files, fonts, images

## Error Handling

**Strategy:** Graceful degradation - static HTML with minimal client-side dependencies

**Patterns:**
- Missing content: Empty card directories handled by Hugo (generates minimal output)
- Image loading failures: HTML `<img>` tags render with fallback to alt text
- CSS parsing errors: Browser renders without custom styles, falls back to base theme
- No JavaScript errors possible in build output (static HTML generation)

## Cross-Cutting Concerns

**Styling:**
- Global color scheme defined in `config.yaml` under `params.colors`
- Custom brand colors: emphasis (#002868), link red (#BF0A30), background (#f5f5f5)
- Applied via CSS variables and theme parameter interpolation in `assets/scss/custom.scss`
- Navbar gradient: white to red linear gradient using CSS custom properties

**Typography:**
- Primary font: Source Sans 3 (defined in `config.yaml` under `params.fonts.body` and `params.fonts.heading`)
- Font weights: 300, 400, 500 with italic variants
- Font files loaded from `assets/fonts/` with WOFF2 format
- Base font size: 16px with 1.5 line height

**Theming:**
- Centralized through `config.yaml` parameterization
- z43-cards-theme reads parameters for colors, fonts, card dimensions
- Custom SCSS overrides in `assets/scss/custom.scss` for gradient effects and spacing adjustments

**Image Handling:**
- Banner image: `assets/images/banner.jpg` (800KB) - displayed in header
- Logo: `assets/images/logo.svg` (2.7KB) - primary navigation/branding
- Alternate logo: `assets/images/logo-white.svg` - inverse color variant
- Favicon: `assets/images/favicon.svg` (2.7KB)
- Card images: Stored alongside `index.md` and `modal.md` in card directories

---

*Architecture analysis: 2026-02-03*
