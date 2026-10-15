# Production UI Components

Use this reference for reusable components, UI architecture, and frontend quality work.

## Component boundary

Separate:

- **primitives:** visual and interaction foundations such as button, field, dialog, and spinner;
- **composites:** reusable combinations such as search box, data table, and form section;
- **features:** product behavior, fetching, permissions, and orchestration;
- **pages/routes:** layout and feature composition.

Keep remote data and business rules in feature or application code. A primitive should not know product-specific endpoints or global stores.

## Props and state

- Make the common case simple and the controlled/uncontrolled behavior unambiguous.
- Prefer semantic variants over raw styling flags.
- Use discriminated unions when only certain prop combinations are valid.
- Forward the correct element attributes and refs when composition requires them.
- Define event semantics, disabled behavior, async behavior, and error ownership.
- Avoid mirroring derived props in state. Keep state as local as practical.

Document the public contract with types and one realistic usage example. Do not expose internal implementation choices as props without a real customization need.

## Required states

Design the full state matrix:

| Dimension | States to consider |
| --- | --- |
| Data | initial, loading, success, empty, stale, error, retrying |
| Interaction | idle, hover, focus-visible, active, disabled, busy |
| Permission | allowed, read-only, hidden, denied |
| Content | short, long, localized, missing, malformed |
| Layout | narrow, wide, zoomed, reduced motion, high contrast |

Do not replace existing content with a spinner during background refresh. Prevent duplicate async submissions and make completion or failure perceivable.

## Accessibility

- Prefer native elements and semantics before ARIA.
- Ensure keyboard operation, logical focus order, visible focus, and predictable focus restoration.
- Provide programmatic labels, descriptions, errors, and live announcements only where needed.
- Associate validation messages with fields and move focus to a useful error summary for long forms.
- Maintain sufficient contrast and non-color indicators.
- Respect reduced-motion settings and avoid essential information conveyed only through animation.
- Test at common zoom levels and with long localized content.

Dialogs, menus, comboboxes, tabs, and grids have complex interaction contracts. Prefer a well-maintained accessible primitive already present in the codebase before implementing them from scratch.

## Responsive and performance behavior

Use content-driven breakpoints and resilient layouts. Preserve touch targets, safe areas, scroll ownership, and table/list usability on narrow screens.

Measure before adding memoization. Stabilize expensive inputs, virtualize only genuinely large collections, lazy-load meaningful boundaries, and avoid layout reads interleaved with writes. Reserve space for media to prevent layout shift.

## Verification

At minimum, verify:

- semantic roles, accessible name, keyboard path, focus movement, and disabled/busy behavior;
- loading, empty, error, retry, long-content, and narrow-screen states;
- no obvious console errors or duplicate requests;
- targeted component tests and one integrated usage path;
- visual inspection at representative widths when layout matters.

Deliver the component architecture, typed prop contract, implementation, tests, and usage example. Mention any dependency or browser constraints.
