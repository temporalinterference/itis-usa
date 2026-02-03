# Technology Stack

**Analysis Date:** 2026-02-03

## Languages

**Primary:**
- YAML - Site configuration and frontmatter
- Markdown - Content files and templating
- SCSS - Custom styling and fonts
- HTML - Generated output

**Secondary:**
- Bash - Build scripts and CI/CD automation
- Go - Hugo module management (go.mod)

## Runtime

**Environment:**
- Hugo (Static Site Generator)
- Go 1.25.3 (module system backend)

**Package Manager:**
- Hugo module system (Go-based)
- Lockfile: `go.sum` (present)

## Frameworks

**Core:**
- Hugo 0.152.2 (hugo-extended) - Static site generator and module system
  - Location: Defined in `mise.toml` via Aqua
  - Purpose: Page generation, template processing, asset bundling

**CSS/UI Framework:**
- UIKit - CSS/JavaScript UI framework
  - Imported as: GitHub module `github.com/temporalinterference/z43-cards-theme`
  - Integrated via: Hugo module system in `config.yaml`
  - Purpose: Component library and responsive design

**Styling:**
- Dart Sass 1.93.2 - SCSS compilation
  - Location: Specified in `mise.toml`
  - Purpose: SCSS to CSS compilation with full language support

## Key Dependencies

**Critical:**
- `github.com/temporalinterference/z43-cards-theme` (v0.0.0 in go.mod) - Custom Hugo theme
  - Purpose: Provides card-based layout system, templates, and UIKit integration
  - Configuration: `config.yaml` imports via `module.imports`
  - Theme initialization: Runs `init-theme.sh` script to setup UIKit

**Infrastructure:**
- Hugo module system - Manages theme and dependencies
- GitHub Actions - CI/CD and deployment pipeline

## Configuration

**Environment:**
- `mise.toml` - Tool version management
  - Hugo: 0.152.2 (via aqua:gohugoio/hugo/hugo-extended)
  - Dart Sass: 1.93.2 (via aqua:sass/dart-sass)
  - Go: 1.25.3
- `config.yaml` - Hugo site configuration
  - Base URL: https://itis-usa.org
  - Language: en-us
  - Theme module import configured
  - Custom color palette defined (emphasis: #002868, link: #BF0A30)
  - Font settings: Source Sans 3 for body and headings

**Build:**
- `go.mod` - Go module definition for Hugo module system
- `go.sum` - Dependency checksums
- GitHub Actions workflow: `.github/workflows/deploy-hugo.yaml`

## Platform Requirements

**Development:**
- Go 1.25.3 (for module system)
- Hugo 0.152.2 (extended version with SCSS support)
- Dart Sass 1.93.2 (for SCSS compilation)
- Bash shell (for build scripts)
- Git (for submodule and module management)
- mise (tool version manager, optional but recommended)

**Production:**
- GitHub Pages - Static site hosting
- Automatic deployment via GitHub Actions on push to main branch and all PRs
- Support for branch-specific preview deployments

## Asset Management

**Fonts:**
- Source Sans 3 (self-hosted WOFF2 format)
  - Weights: 300, 300italic, 400, 400italic, 500, 500italic
  - Location: `assets/fonts/`
  - Delivery: Minified and cached with long-lived expiration via Hugo

**Images:**
- Favicon (favicon.svg): `assets/images/favicon.svg`
- Logo (white): `assets/images/logo-white.svg`
- Logo: `assets/images/logo.svg`
- Banner: `assets/images/banner.jpg`
- Card teasers: Stored in content directories with card content

**Generated Assets:**
- `resources/_gen/` - Hugo-generated minified and optimized assets
- Ignored from version control (see `.gitignore`)

## Build Configuration

**Hugo Build:**
- Hugo extended required (for SCSS compilation)
- Minification enabled: `hugo --minify`
- Module initialization: `hugo mod get -u` + theme init script
- Base URL: Dynamic based on GitHub Pages deployment context (main vs branch previews)

**SCSS Processing:**
- Custom SCSS in `assets/scss/custom.scss`
- Compiled by Hugo's asset pipeline with Dart Sass
- Includes font-face definitions and custom theming

---

*Stack analysis: 2026-02-03*
