# Coding Conventions

**Analysis Date:** 2026-02-03

## Naming Patterns

**Files:**
- Hugo content files: `index.md` for card index (brief description), `modal.md` for modal content (detailed description)
- Card directories: kebab-case with semantic prefix pattern: `news-YYYY-MM-DD-Description` for news cards, `topic-name` for permanent cards
- Example: `/home/oetiker/checkouts/itis-usa/content/cards/news-20260115-MRI-Filter-Solutions/index.md`
- SCSS files: `custom.scss` for site-specific overrides
- JavaScript files: `custom.js` for site-specific behavior

**Functions:**
- camelCase for JavaScript functions
- Example: `isElCompletelyInViewport()`, `updateArrows()`, `quickScroll()`, `handleModalHash()`, `setupModalHandlers()`
- Verb-first pattern: `handle*`, `setup*`, `update*`, `is*`, `remove*`

**Variables:**
- camelCase for JavaScript variables
- UPPER_SNAKE_CASE for configuration constants at module level
- Example constants in `/home/oetiker/checkouts/itis-usa/public/js/custom.js`:
  - `SCROLL_MULTIPLIER = 0.5`
  - `EDGE_THRESHOLD = 20`
  - `MODAL_PREFIXES = ['modal-', 'popup-']`
- Descriptive names with clear intent: `isPointerDown`, `activePointerId`, `maxScroll`, `currentScroll`, `cardWidth`, `scrollAmount`

**Types:**
- No explicit TypeScript usage in codebase; JavaScript uses JSDoc-style comments for type hints
- HTML IDs follow patterns: `ti-*` prefix for measurement divs, `modal-*` or `popup-*` prefix for modal elements
- CSS classes: `ti-card`, `ti-card-holder`, `ti-card-holder-wrapper`, `ti-nav-left`, `ti-nav-right` (using `ti-` namespace)
- Hugo/YAML frontmatter: snake_case for metadata fields (`build`, `publishResources`, `navigation`, etc.)

## Code Style

**Formatting:**
- No explicit formatter detected (no .prettierrc)
- Inferred style from code analysis:
  - 4-space indentation in JavaScript
  - Consistent spacing around operators and after control keywords
  - Opening braces on same line as function/control structure
  - Single-line comments for explanation, multi-line for docstrings
  - String concatenation uses template literals with backticks

**Linting:**
- No explicit linter detected (no .eslintrc configuration)
- Code follows JavaScript best practices:
  - Consistent variable declarations with `const` preferred over `let` and `var`
  - Event listener patterns standardized
  - Error handling with try/catch not heavily used (mostly event-driven code)

## Import Organization

**Order:**
Not applicable - this is a Hugo static site without module imports in content files. JavaScript files are monolithic.

**Path Aliases:**
Not detected. Static files organized by type: `/assets/` for sources, `/public/` for built output.

## Error Handling

**Patterns:**
- Graceful degradation: Functions check for element existence before operating
  - Example: `if (!needsNavigation || !leftArrow || !rightArrow) return;`
  - Example: `const modal = UIkit.modal('#${hash}'); if (modal) { modal.show(); }`
- Optional chaining used for safe property access: `wrapper?.querySelector('.ti-nav-left')`
- Type checking: `typeof gtag !== 'undefined'` before calling Google Analytics
- Pointer event tracking with flag-based state management: `isPointerDown`, `isProgrammaticModalOperation`, `isModalTransition`

## Logging

**Framework:** `console` (minimal usage in `/home/oetiker/checkouts/itis-usa/public/js/custom.js`)

**Patterns:**
- Commented-out debug logging detected: `// console.log(e);` (line 173)
- Production code does not use console logging
- Event tracking via Google Analytics: `gtag('event', 'modal_open', {...})` and `gtag('event', 'modal_close', {...})`

## Comments

**When to Comment:**
- Section headers for major functional blocks with `/* Comment */` syntax
- Example: `/* Handle scrolling for each card holder */`
- Example: `/* measure margins and set css variables */`
- Short inline comments for non-obvious logic: `// Ease out cubic for smooth deceleration`
- Commented code preserved for future reference (e.g., `// console.log(e);`)

**JSDoc/TSDoc:**
- No JSDoc patterns detected
- Minimal documentation; code intent is mostly clear from function names

## Function Design

**Size:** Generally small, focused functions
- Example: `isElCompletelyInViewport()` - 3 lines
- Example: `removeHash()` - 8 lines
- Exception: `cardHolders.forEach()` callback spans ~180 lines but maintains single responsibility (card holder management)

**Parameters:**
- Minimal parameters; mostly work with DOM elements through closures
- Example: `quickScroll(direction)` takes single parameter (1 or -1)
- Example: `setupModalHandlers(element)` takes single DOM element

**Return Values:**
- Functions mostly perform side effects (DOM manipulation, event handling)
- `isElCompletelyInViewport()` returns boolean
- Modal handler setup functions return void

## Module Design

**Exports:**
- No explicit exports; code runs in global scope within `<script>` tags
- Global configuration exposed: `UIkit` (from theme), `gtag` (Google Analytics)

**Barrel Files:**
- Not applicable; single monolithic `custom.js` file handles all custom functionality

## SCSS/CSS Conventions

**File Organization:**
- Located at `/home/oetiker/checkouts/itis-usa/assets/scss/custom.scss`
- Uses Hugo templating within SCSS: `{{ .Site.Params.colors.emphasis }}`
- Font loading with `resources.Get()` Hugo function

**Naming:**
- CSS classes use `ti-` namespace (e.g., `.ti-title`, `.ti-claim`, `.uk-*` from UIKit framework)
- BEM-like patterns observed: `.ti-card-holder-wrapper`

**Color Usage:**
- Colors defined in `config.yaml` under `params.colors`
- Referenced via Hugo template syntax in SCSS
- Example colors: `#BF0A30` (link/emphasis red), `#002868` (navy/emphasis), `#ffffff` (white)

---

*Convention analysis: 2026-02-03*
