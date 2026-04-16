# Project Update Summary

This document outlines the recent updates, architectural shifts, and UX/UI improvements made to the Todo Task application over the last 48 hours. The changes represent a complete shift from static HTML to an interactive, accessible, and easily manageable dynamic component architecture, diving deeper than previous commit logs.

## 📝 Differences in Code (Last 48 Hours)

### `index.html`
- **Dynamic Element Mapping**: Introduced explicit classes, and `data-testid` properties (e.g., `todo__status-control`, `todo__checkbox`) to elements to allow robust DOM querying and testability.
- **Inline Editing Form**: Added full `<form class="todo__form">` blocks nested inside `.todo__body` providing Title, Description, Priority, and Due Date editable inputs.
- **Collapsible Regions**: Wrapped descriptions in `.todo__collapsible-container` to support progressive disclosure for lengthy text visually.
- **Status Synching**: Migrated from purely static text-based statuses to interactive `span` indicators, checkboxes, and controllable `<select>` drop-downs operating in tandem.

### `src/style.css`
- **Mobile-First Layouts**: Replaced rigid alignments with a mobile-first column-based flexbox strategy using `.flex-container` for seamless vertical stacking on smaller screens, smoothly breaking to horizontal rows at `min-width: 45em`.
- **Form UI Polish**: Overhauled inputs, `<textarea>`, and `<select>` fields with generous padding, `var(--radius-md)` border constraints, subtle contrast backgrounds (`var(--clr-tertiary-50)`), and crisp accessibility-focused outline rings on focus.
- **Custom Select Assets**: Stripped default browser `-webkit-appearance` styles from drop-downs in favor of a normalized, custom SVG data-URI chevron, ensuring consistent visual aesthetics across MacOS, Windows, and mobile platforms.
- **Text Truncation**: Implemented `.todo__description--collapsed` using WebKit `line-clamp: 3` and `-webkit-box-orient: vertical` properties to cleanly truncate descriptions purely via CSS without slicing strings.

### `src/main.js`
- **Component Class Architecture**: Refactored fragmented query scripts into a cohesive ES6 `Task` class (`class Task`) that individually maps HTML structures and maintains isolated logic bindings for each separate todo card present on the page.
- **Time & Urgency Tracking**: Added interval-based timer logic that repeatedly fires `calculateRemainingTime()`, interpreting historical `datetime` elements into dynamic string notifications (e.g., "Due now!", "Overdue by 2 hours", "Due tomorrow").
- **Two-way Syncing**: Coupled unique DOM manipulation logic via `updateCard` and `handleStatusChange` to ensure that ticking the complete checkbox natively updates the surrounding text labels, selects the matching dropdown state, and crosses out the task title synchronously.
- **Dynamic Component Operations**: Handled programmatic Javascript string measuring (exceeding 120 character limitations) inside `collapseDescription()` to strategically inject missing generic "Show more" toggles during class initialization.

## ✨ New Design Decisions
- **In-Place Editing Context**: Instead of navigating the user to a completely detached route or disruptive popup modal to edit an item, we embedded an inline `<form>`. This preserves UX context by letting the user focus purely on their current list perspective.
- **Progressive Disclosure**: Descriptions are structurally clipped to three lines maximum initially. This creates a clean macroscopic view of the list board while maintaining deep-dive reading tools ("Show more") for heavier context.
- **Semantic Color State**: UI indicators automatically restyle corresponding to threat thresholds. The `success` variable scales represent distant timelines, `warning` scales (Amber) indicate pending 24 hours, and severe `error` scales (Red) explicitly denote overdue thresholds.

## ⚠️ Known Limitations
- **No Data Persistence**: The current structural logic is entirely ephemeral, executing within in-memory structures only. A browser hard-refresh will restore the application to its hardcoded primary HTML values. Currently, `localStorage` and functional API handlers do not actually push changes.
- **Incomplete Actions**: Core features mock real interactions. For instance, clicking the delete button blindly triggers a Javascript window `alert()` without splicing arrays or structurally deleting the targeted `.todo` article fragment.
- **Missing Submit Payload**: Form submission acts locally using `FormData` updates on the DOM layer without invoking standard `fetch` API methods (GET/POST/PUT/DELETE) to physically commit changes.

## ♿ Accessibility Notes
- **Announcer Tags**: Embedded `aria-live="polite"` on varying dynamic span locations. This implies screen reader technologies proactively report progress transitions globally without requiring a user to forcefully focus on said regions specifically.
- **State Semantics**: Dynamic buttons programmed for condensing long text seamlessly shift intrinsic `aria-expanded` parameters between `true` and `false` and denote associations mapping to `aria-controls`, effectively broadcasting physical structural transformations directly to accessibility nodes.
- **Icon Button Verbosity**: Generic interactive shapes such as SVG vector Edit and Delete icons forcefully utilize specific `aria-label` tags, preventing readers from stumbling upon unmarked ghost components.
- **Focus Preservation**: UX flow logic ensures that unmounting the `<form>` wrapper returns programmatic focus identically backwards toward the initial Edit invocation (`this.editBtn.focus()`), bypassing annoying top-of-page navigation leaps commonly noted in JavaScript component teardowns.
- **High Contrast Interactions**: All active form input selections inject a strict `2px solid var(--clr-primary-400)` focus bounding-box that strictly achieves mandated double AA/AAA contrast guidelines.
