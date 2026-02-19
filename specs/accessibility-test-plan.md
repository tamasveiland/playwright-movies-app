# Movie App - Critical Accessibility Issues Test Plan

## Application Overview

The Movie App is a React-based movie database application that provides movie discovery, search, and browsing functionality. The application features:

- **Movie Discovery**: Browse popular, top-rated, and upcoming movies
- **Genre Navigation**: Filter movies by genre categories 
- **Search Functionality**: Search for specific movies with autocomplete
- **Theme Switching**: Toggle between light and dark modes
- **Movie Details**: View detailed information, cast, and ratings
- **Responsive Design**: Support for various screen sizes

## Critical Accessibility Issues Identified

Based on interface analysis and code examination, the following critical accessibility areas require comprehensive testing:

## Test Scenarios

### 1. Star Rating System Accessibility

**Critical Issue**: Star ratings may not provide adequate context for screen reader users

#### 1.1 Star Rating Screen Reader Announcement

**File** `tests/accessibility/star-rating-screen-reader.spec.ts`

**Steps:**
1. Navigate to the homepage `/?category=Popular&page=1`
2. Locate the first movie's rating element using `page.getByLabel('rating').first()`
3. Use screen reader simulation to capture the announced text
4. Hover over the rating to trigger tooltip
5. Verify tooltip content is accessible

**Expected Results:**
- Rating should announce as "Rating: 4.5 out of 5 stars" or similar
- Tooltip should provide additional context like "Average rating 4.5 based on 1,234 votes"
- Rating should have proper ARIA label or accessible name
- Focus indicator should be visible when navigating with keyboard

#### 1.2 Rating Keyboard Navigation

**File** `tests/accessibility/star-rating-keyboard.spec.ts`

**Steps:**
1. Navigate to any movie list page
2. Use Tab key to navigate to rating elements
3. Press Enter/Space to interact with rating (if interactive)
4. Verify tooltip appears on focus, not just hover
5. Test Escape key dismisses tooltip

**Expected Results:**
- Rating should be focusable with Tab navigation
- Tooltip should appear on both hover and keyboard focus
- Tooltip should dismiss with Escape key
- Focus should return to rating element after tooltip closes

### 2. Theme Toggle Accessibility

**Critical Issue**: Multiple theme toggle methods need consistent accessibility implementation

#### 2.1 Theme Toggle Multiple Input Methods

**File** `tests/accessibility/theme-toggle-input-methods.spec.ts`

**Steps:**
1. Navigate to homepage
2. Test sun button (☀) with mouse click
3. Test moon button (☾) with mouse click  
4. Test toggle switch with mouse click
5. Repeat all interactions using keyboard only (Tab, Enter, Space)
6. Test with screen reader announcements

**Expected Results:**
- All three methods should work with keyboard only
- Screen reader should announce state change: "Dark mode enabled" / "Light mode enabled"
- Toggle switch should have proper role="switch" and aria-checked state
- Current theme state should be announced when focusing on controls
- Visual focus indicators should be clear and high contrast

#### 2.2 Theme Toggle State Persistence

**File** `tests/accessibility/theme-toggle-state-persistence.spec.ts`

**Steps:**
1. Navigate to homepage 
2. Switch to dark mode using any method
3. Navigate to different page (e.g., movie details)
4. Verify theme persisted and controls reflect current state
5. Test browser refresh maintains state
6. Verify aria-checked and other state attributes are correct

**Expected Results:**
- Theme preference should persist across navigation
- All theme toggle controls should reflect current state consistently
- ARIA attributes should match visual state
- No accessibility announcements should conflict between controls

### 3. Navigation Menu Focus Management

**Critical Issue**: Menu open/close states need proper focus management and keyboard navigation

#### 3.1 Menu Focus Management

**File** `tests/accessibility/menu-focus-management.spec.ts`

**Steps:**
1. Navigate to homepage
2. Use Tab to reach menu button
3. Press Enter to open menu
4. Verify focus moves to first menu item
5. Use arrow keys to navigate menu items
6. Press Escape to close menu
7. Verify focus returns to menu button
8. Test Tab trapping within open menu

**Expected Results:**
- Focus should move to first menu item when opened
- Arrow keys should navigate between menu items
- Tab should be trapped within the menu when open
- Escape should close menu and return focus to trigger
- Menu should have proper ARIA attributes (aria-expanded, aria-haspopup)
- Menu items should have clear focus indicators

#### 3.2 Menu Keyboard Navigation Patterns

**File** `tests/accessibility/menu-keyboard-navigation.spec.ts`

**Steps:**
1. Open navigation menu using keyboard
2. Test Home key moves to first menu item
3. Test End key moves to last menu item
4. Test first letter navigation (e.g., press 'A' for Action)
5. Test Enter activates current menu item
6. Test tab moves out of menu (should close menu)

**Expected Results:**
- Home/End keys should work for quick navigation
- First letter typing should work for genre navigation
- Menu should close when focus moves outside
- Selected menu item should have aria-current or similar state
- Submenu structure should be properly announced

### 4. Search Functionality Accessibility

**Critical Issue**: Search input needs comprehensive accessibility implementation

#### 4.1 Search Input Labeling and Instructions

**File** `tests/accessibility/search-input-labeling.spec.ts`

**Steps:**
1. Navigate to homepage
2. Locate search input field
3. Verify proper labeling (aria-label or associated label element)
4. Test screen reader announcement of purpose and instructions
5. Test placeholder text accessibility
6. Test search suggestions/autocomplete accessibility

**Expected Results:**
- Search input should have descriptive accessible name
- Purpose should be clear: "Search for movies"
- Instructions should be available: "Type to search movies, results will appear below"
- Placeholder text should not be the only label
- Search suggestions should be properly associated with input

#### 4.2 Search Results and Error States

**File** `tests/accessibility/search-results-accessibility.spec.ts`

**Steps:**
1. Navigate to homepage
2. Perform successful search for "Twisters"
3. Verify results are announced to screen readers
4. Test "No results" search (e.g., "xyz123nonexistent")
5. Verify error message is accessible and properly announced
6. Test search results keyboard navigation

**Expected Results:**
- Search results should be in a labeled region: "Search results for Twisters"
- Results count should be announced: "12 movies found for Twisters"
- "No results" message should be clear and helpful
- Error messages should have proper ARIA live regions
- Results should be navigable with keyboard

### 5. Movie Card Information Architecture

**Critical Issue**: Movie cards need comprehensive accessible names combining multiple information elements

#### 5.1 Movie Card Accessible Names

**File** `tests/accessibility/movie-card-accessible-names.spec.ts`

**Steps:**
1. Navigate to movie list page
2. Locate first movie card/link
3. Test screen reader announcement of complete card information
4. Verify all essential information is included in accessible name
5. Test with longer movie titles that might be truncated

**Expected Results:**
- Full accessible name should include: movie title, rating, and year if available
- Example: "Deadpool & Wolverine, rated 4.5 out of 5 stars, 2024"
- Truncated titles should have full text in accessible name
- Poster image alt text should complement, not duplicate link text
- Rating should be meaningful: "4.5 out of 5 stars" not "★ ★ ★ ★ ★"

#### 5.2 Movie Card Information Hierarchy

**File** `tests/accessibility/movie-card-information-structure.spec.ts`

**Steps:**
1. Navigate to movie list
2. Test heading structure within movie cards
3. Verify poster images have descriptive alt text
4. Test card focus indicators and boundaries
5. Verify card grouping and list semantics

**Expected Results:**
- Movie titles should use appropriate heading levels (h2 or h3)
- List structure should be semantic with proper roles
- Each movie should be contained in a list item
- Poster alt text should describe poster content, not just title
- Group/list labels should provide context: "Popular movies list"

### 6. Dynamic Content and Loading States

**Critical Issue**: Loading states and dynamic content updates need proper accessibility announcements

#### 6.1 Page Loading and Content Updates

**File** `tests/accessibility/loading-states-accessibility.spec.ts`

**Steps:**
1. Navigate to slow-loading page (e.g., large movie list)
2. Monitor screen reader announcements during loading
3. Test loading spinner/indicator accessibility
4. Test pagination loading states
5. Verify completion announcements

**Expected Results:**
- Loading states should be announced: "Loading movies..."
- Progress should be communicated if determinable
- Loading indicators should have proper labels and roles
- Completion should be announced: "Movies loaded, 20 items displayed"
- Focus should be managed appropriately during updates

### 7. Color Contrast and Visual Accessibility

**Critical Issue**: Ensure sufficient color contrast in both light and dark themes

#### 7.1 Color Contrast Compliance Testing

**File** `tests/accessibility/color-contrast-compliance.spec.ts`

**Steps:**
1. Navigate to homepage in light mode
2. Test color contrast ratios for all text elements
3. Switch to dark mode
4. Repeat contrast testing for dark theme
5. Test focus indicators visibility in both themes
6. Test interactive element state colors (hover, active, disabled)

**Expected Results:**
- Body text should meet WCAG AA contrast ratio (4.5:1)
- Large text should meet WCAG AA contrast ratio (3:1)
- Focus indicators should meet contrast requirements
- Interactive elements should maintain contrast in all states
- Color should not be the only means of conveying information

## Implementation Priority

1. **High Priority**: Star rating accessibility, theme toggle keyboard support, menu focus management
2. **Medium Priority**: Search functionality, movie card information architecture
3. **Lower Priority**: Color contrast validation, loading states

## Testing Tools Recommended

- **aXe-core** for automated accessibility scanning
- **Playwright's built-in accessibility testing** for ARIA snapshots
- **Screen reader simulation** for user experience testing
- **Keyboard-only navigation** testing
- **Color contrast analyzers** for visual accessibility

## Success Criteria

Each test should verify:
- ✅ Keyboard accessibility (all functionality available via keyboard)
- ✅ Screen reader compatibility (proper announcements and context)
- ✅ ARIA compliance (correct roles, properties, and states)
- ✅ Focus management (clear indicators and logical flow)
- ✅ Error handling (accessible error messages and recovery)
- ✅ Color accessibility (sufficient contrast, not color-dependent)