# Testing Patterns

**Analysis Date:** 2026-02-03

## Test Framework

**Runner:**
- Not detected - no test framework configured in this codebase

**Assertion Library:**
- Not applicable - no testing infrastructure present

**Run Commands:**
```bash
# No test runner is configured
# This is a static site with no automated testing setup
```

## Test File Organization

**Location:**
- No test files found in repository
- Pattern: Not established

**Naming:**
- Not applicable - no test files present

**Structure:**
- Not applicable - no test suite exists

## Testing Strategy

**Current State:**
This codebase has **zero automated testing** configured. Testing appears to be manual/visual only.

**Testing Gaps:**
The following critical areas lack test coverage:

1. **JavaScript functionality** (`/home/oetiker/checkouts/itis-usa/public/js/custom.js`):
   - Card carousel/scrolling behavior (lines 33-214)
   - Modal state management (lines 216-320)
   - Browser history/URL hash synchronization
   - Pointer event handling (drag, click, wheel)
   - Animation calculations (easing functions)

2. **Hugo content/config**:
   - Front matter parsing and metadata
   - Theme module integration (`z43-cards-theme`)
   - SCSS template rendering with `config.yaml` parameters

3. **Cross-browser compatibility**:
   - No browser compatibility matrix
   - No polyfill strategy documented
   - Pointer events may vary across browsers

## Test Types

**Unit Tests:**
- Scope: Not applicable - no unit testing framework
- Recommendation: Consider testing utility functions like:
  - `isElCompletelyInViewport()` with various viewport configurations
  - `quickScroll()` scroll calculations
  - Pointer event flag management logic

**Integration Tests:**
- Scope: Not applicable - no integration testing framework
- Recommendation: Test card carousel interactions:
  - Modal opening/closing and URL hash updates
  - Navigation arrow visibility based on scroll position
  - Drag gesture coordination with pointer events
  - Wheel event handling and scroll constraints

**E2E Tests:**
- Framework: Not used
- Recommendation: Consider adding E2E tests for:
  - Card carousel navigation (arrow clicks, drag, wheel)
  - Modal open/close with URL hash verification
  - Browser back/forward button behavior
  - Google Analytics event tracking

## Mocking

**Framework:**
- Not applicable - no testing framework present

**What to Mock:**
- `UIkit` modal library interface
- Google Analytics `gtag` function
- DOM methods (getBoundingClientRect, querySelectorAll)
- requestAnimationFrame for animation testing

**What NOT to Mock:**
- DOM manipulation (use real DOM or jsdom)
- Event listeners (test actual event handling)
- CSS measurements (use real or simulated CSS values)

## Fixtures and Factories

**Test Data:**
- Not established - no test data patterns present
- Recommendation: Consider fixtures for:
  - Mock card collections with varying counts (0, 3, 5+ cards)
  - Pointer event objects with various properties
  - Modal element configurations

**Location:**
- Would typically go in: `test/fixtures/` or `test/mocks/`

## Coverage

**Requirements:**
- No coverage requirements currently enforced
- No coverage reports generated

**Targets:**
- Recommendation: Establish coverage targets for critical paths:
  - `/home/oetiker/checkouts/itis-usa/public/js/custom.js` - minimum 70% line coverage
  - Modal state management - 90% coverage (high-risk for UX bugs)

## Manual Testing Checklist

Given the absence of automated tests, the following manual checks should be performed:

**Carousel/Scrolling Behavior:**
- [ ] Arrow buttons appear/disappear correctly based on card count
- [ ] Arrow opacity reflects scroll position (50% at edges, 100% in middle)
- [ ] Clicking left/right arrow scrolls by one card width smoothly
- [ ] Drag gestures work on mouse and touch
- [ ] Wheel scrolling works when carousel is fully in viewport
- [ ] Wheel scrolling does NOT intercept when carousel partially off-screen
- [ ] CSS variable updates on window resize: `--ti-margin-measure-left`, `--ti-margin-measure-right`, `--ti-content-width`

**Modal Behavior:**
- [ ] Clicking card opens correct modal
- [ ] URL hash updates to match modal ID (e.g., `#simulation-insights`)
- [ ] Browser back button closes modal
- [ ] Browser forward button reopens modal
- [ ] Direct URL access with hash opens correct modal
- [ ] Closing modal removes hash from URL
- [ ] Transitioning between modals maintains browser history correctly
- [ ] Google Analytics events fire on modal open/close

**Touch/Pointer Events:**
- [ ] Cursor changes to "grab" when hovering over carousel
- [ ] Cursor changes to "grabbing" when dragging
- [ ] Pointer down on carousel and move horizontally scrolls
- [ ] Pointer events from other pointers are ignored
- [ ] Touch drag behaves like mouse drag

**Edge Cases:**
- [ ] Cards with 3 or fewer items: arrows don't appear
- [ ] Cards with exactly 4 items: arrows appear, full scrollability
- [ ] Window resize triggers CSS variable updates
- [ ] Multiple carousel instances work independently

## Recommended Testing Setup

**Framework Selection:**
- Unit/Integration: `Vitest` (fast, ESM-native) or `Jest`
- E2E: `Playwright` or `Cypress`
- Accessibility: `jest-axe` or `axe-core`

**File Structure:**
```
tests/
├── unit/
│   └── custom.js.test.js
├── integration/
│   └── carousel.test.js
├── e2e/
│   └── modal-navigation.spec.js
└── fixtures/
    └── pointer-events.js
```

**Example Test Pattern (Vitest):**
```javascript
// tests/unit/custom.js.test.js
import { describe, it, expect, beforeEach } from 'vitest';

describe('isElCompletelyInViewport', () => {
  it('returns true when element is fully in viewport', () => {
    // Arrange: create element with known bounding rect
    // Act: call function
    // Assert: expect true
  });

  it('returns false when element partially off-screen', () => {
    // Arrange
    // Act
    // Assert: expect false
  });
});
```

---

*Testing analysis: 2026-02-03*
