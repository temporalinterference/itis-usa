# Codebase Concerns

**Analysis Date:** 2026-02-03

## Critical Build Issues

**Theme Module Version Pinned to Outdated Version:**
- Issue: `go.mod` specifies `v0.0.0` for `github.com/temporalinterference/z43-cards-theme`, but v0.0.5 is available. The project currently uses outdated shortcode implementations (missing `banner-image` shortcode in v0.0.0).
- Files: `/home/oetiker/checkouts/itis-usa/go.mod`, `/home/oetiker/checkouts/itis-usa/go.sum`
- Impact: Build fails immediately with "template for shortcode banner-image not found" error. Site cannot be built or deployed.
- Fix approach: Update `go.mod` to require `v0.0.5` or later: `require github.com/temporalinterference/z43-cards-theme v0.0.5`. Run `hugo mod get -u` to update dependencies.

**UIKit Submodule Not Initialized:**
- Issue: Theme module v0.0.5 requires UIKit git submodule to be initialized, but it's missing.
- Files: Theme module dependency (in Hugo cache), referenced in build warnings
- Impact: Theme fails to load required assets during build, cascading into SCSS compilation failures.
- Fix approach: Initialize UIKit submodule in theme by running the init script provided by theme or `git submodule update --init --recursive` in theme directory.

**SCSS Stylesheet Import Missing:**
- Issue: Build fails with "Can't find stylesheet to import" when compiling SCSS. Root cause is missing UIKit initialization causing cascading asset compilation failures.
- Files: SCSS compilation pipeline (Hugo asset processing)
- Impact: CSS assets fail to compile, blocking all page rendering.
- Fix approach: Initialize UIKit submodule (see above). Also update Dart Sass to address deprecated @import syntax (see Deprecation warnings below).

**Individual Card Page Rendering Failures:**
- Issue: Multiple cards fail to render with "error calling Fingerprint: <nil> can not be transformed" in template execution.
- Files: `/home/oetiker/checkouts/itis-usa/content/cards/jobs/index.md`, `/home/oetiker/checkouts/itis-usa/content/cards/news-20260115-MRI-Filter-Solutions/index.md`, `/home/oetiker/checkouts/itis-usa/content/cards/news-20250710-SPARC-Seminar/index.md`, `/home/oetiker/checkouts/itis-usa/content/cards/news-2025-10-17-SEAWave-Meeting-Lyon/index.md`
- Impact: Individual card pages cannot be rendered. Theme head partial cannot process asset fingerprinting.
- Fix approach: Dependent on fixing UIKit initialization and SCSS compilation. These errors indicate theme template incompatibility with missing assets.

## Missing Content

**Empty Card Directories (No index.md files):**
- Issue: Four card directories exist but contain no content files:
  - `/home/oetiker/checkouts/itis-usa/content/cards/about/` - Referenced in home page but empty
  - `/home/oetiker/checkouts/itis-usa/content/cards/activities/` - Referenced in home page but empty
  - `/home/oetiker/checkouts/itis-usa/content/cards/news/` - Empty stub directory
  - `/home/oetiker/checkouts/itis-usa/content/cards/publications/` - Referenced in home page but empty
- Files: Home page references these at `/home/oetiker/checkouts/itis-usa/content/_index.md`
- Impact: Home page references cards that don't exist. Card grid will render with missing content or broken links. The "About" section shows an empty card holder.
- Fix approach: Either (1) create `index.md` files for each with proper content, or (2) remove from home page references. According to CLAUDE.md task, should "Setup the basic site structure with placeholder text" - suggest creating placeholder `index.md` files for empty directories.

**Placeholder and Stub Content:**
- Issue: Several cards contain minimal or placeholder content:
  - `the-board/index.md`: Only has title "The Board", no content body
  - `staff/index.md`: Only has title "Staff", no content body
  - `news-tbc/index.md`: Contains placeholder text "Month xx, year"
  - `education/index.md`: Minimal one-sentence description
- Files:
  - `/home/oetiker/checkouts/itis-usa/content/cards/the-board/index.md`
  - `/home/oetiker/checkouts/itis-usa/content/cards/staff/index.md`
  - `/home/oetiker/checkouts/itis-usa/content/cards/news-tbc/index.md`
  - `/home/oetiker/checkouts/itis-usa/content/cards/education/index.md`
- Impact: Users see incomplete information. Cards render as nearly empty or with obviously placeholder text.
- Fix approach: Populate with actual content or decide which are intentionally minimal. No modal.md files exist for these either, so detail views are unavailable.

## Architecture Issues

**Inconsistent Card Structure:**
- Issue: Card directories use different file naming conventions. Some cards reference card names that don't match directory names:
  - Home page references `{{< card about >}}` but directory is `./content/cards/about/` (empty)
  - Home page references `{{< card activities >}}` but directory is `./content/cards/activities/` (empty)
  - Some cards reference newsletter/publication items that may need individual card entries
- Files: `/home/oetiker/checkouts/itis-usa/content/_index.md`, card directories
- Impact: Content structure is unclear. Card naming is inconsistent with directory structure. Future maintainers will struggle to add new cards.
- Fix approach: Clarify whether "about" and "activities" should be section headers or actual card items. Review card naming convention and ensure consistency.

**Missing Modal Content:**
- Issue: Many cards exist but lack corresponding `modal.md` files for detail views:
  - `/home/oetiker/checkouts/itis-usa/content/cards/about/` (directory is empty)
  - `/home/oetiker/checkouts/itis-usa/content/cards/activities/` (directory is empty)
  - `/home/oetiker/checkouts/itis-usa/content/cards/research/` (has index.md but no modal.md)
  - `/home/oetiker/checkouts/itis-usa/content/cards/standards/` (has index.md but no modal.md)
  - `/home/oetiker/checkouts/itis-usa/content/cards/the-board/` (has index.md but no modal.md)
  - `/home/oetiker/checkouts/itis-usa/content/cards/staff/` (has index.md but no modal.md)
  - `/home/oetiker/checkouts/itis-usa/content/cards/jobs/` (has index.md but no modal.md)
  - `/home/oetiker/checkouts/itis-usa/content/cards/education/` (has index.md but no modal.md)
- Files: Multiple card directories in `/home/oetiker/checkouts/itis-usa/content/cards/`
- Impact: Card detail modals are unavailable for these cards. Users cannot see expanded content. Theme templates expect modal.md files.
- Fix approach: Either (1) create modal.md files with detailed content for each card, or (2) configure theme to handle cards without modals gracefully.

## Dependency and Maintenance Issues

**Dependency Version Mismatch in go.sum:**
- Issue: `go.sum` contains checksums for three different versions of z43-cards-theme (v0.0.0, v0.0.2, v0.0.5), but only v0.0.0 is required in go.mod. This indicates historical changes and confusion about which version to use.
- Files: `/home/oetiker/checkouts/itis-usa/go.sum`
- Impact: Unclear which version is canonical. Maintenance burden and potential for accidental downgrades.
- Fix approach: Clean up go.sum to contain only the checksums for the required version. After updating go.mod to v0.0.5, remove old v0.0.0 and v0.0.2 entries from go.sum.

**Backup Files in Version Control:**
- Issue: Multiple backup files are committed to the repository:
  - `/home/oetiker/checkouts/itis-usa/CLAUDE.md~` (vim backup)
  - `/home/oetiker/checkouts/itis-usa/go.mod~` (vim backup)
- Files: CLAUDE.md~, go.mod~
- Impact: Repository contains unnecessary files. Pollutes version history. Creates confusion about which file is current.
- Fix approach: Add `*~` pattern to `.gitignore` and remove these backup files from version control using `git rm --cached`.

## Deprecation Warnings

**Dart Sass @import Deprecation:**
- Issue: Build warnings show "Sass @import rules are deprecated and will be removed in Dart Sass 3.0.0"
- Files: SCSS stylesheets being compiled (from theme)
- Impact: Build produces warnings. Will break in Dart Sass 3.0.0 when @import is removed.
- Fix approach: Theme needs to migrate from @import to @use statements. This is likely a theme issue (z43-cards-theme) but monitor for when they update. May need to update to newer theme version that has migrated to @use.

## Configuration Issues

**Navigation Link Error:**
- Issue: In `/home/oetiker/checkouts/itis-usa/content/navigation.md`, both "Publications" and "About" navigation items, but "About" link points to `#about` while "Contact" link also points to `#about`. Should either point to different sections or "Contact" should point to its own section.
- Files: `/home/oetiker/checkouts/itis-usa/content/navigation.md` (lines 16-18)
- Impact: "Contact" navigation item goes to same location as "About", creating confusion.
- Fix approach: Verify intended navigation structure. Either create a dedicated `#contact` section in home page or remove "Contact" from navigation.

## Test Coverage Gaps

**No Testing Infrastructure:**
- Issue: No test files, test framework configuration, or automated testing for:
  - Content structure validation (cards have required files)
  - Build correctness (site builds without errors)
  - Link validation (cards referenced in home page exist)
  - Content completeness (required fields present)
- Files: None (missing entirely)
- Impact: Build breaks (as currently observed) could have been caught by pre-push validation. No automated safeguards against incomplete content.
- Fix approach: Consider adding pre-commit or pre-push hooks to validate card directory structure. Could use simple bash script to check:
  - All cards referenced in _index.md have corresponding directories
  - All card directories have index.md
  - Hugo builds successfully before commit

## Summary of Blocking Issues

**Immediately blocking site build (in priority order):**
1. Update `go.mod` to use theme v0.0.5 (currently using v0.0.0 which lacks required shortcodes)
2. Initialize UIKit submodule in theme module (blocking SCSS compilation)
3. Create missing `index.md` files for empty card directories: about, activities, news, publications
4. Populate placeholder/stub content in cards with minimal content

---

*Concerns audit: 2026-02-03*
