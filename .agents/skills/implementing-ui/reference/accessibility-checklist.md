# Accessibility Checklist

## Keyboard Navigation
- [ ] All interactive elements focusable with Tab
- [ ] Focus order follows visual order
- [ ] Focus indicator visible (outline or ring)
- [ ] Escape closes modals/dropdowns
- [ ] Enter/Space activates buttons

## Screen Readers
- [ ] Images have alt text
- [ ] Form inputs have labels
- [ ] Buttons have descriptive text
- [ ] Landmarks used (main, nav, header, footer)
- [ ] Headings follow hierarchy (h1 → h2 → h3)

## Visual
- [ ] Color contrast ratio ≥ 4.5:1 (text)
- [ ] Color contrast ratio ≥ 3:1 (large text, UI)
- [ ] Information not conveyed by color alone
- [ ] Text resizable to 200% without breaking layout

## Forms
- [ ] Error messages linked to inputs (aria-describedby)
- [ ] Required fields marked (aria-required)
- [ ] Validation errors announced
- [ ] Form has submit button (not just Enter)

## ARIA Patterns
- [ ] Dialogs have role="dialog" and aria-modal="true"
- [ ] Tabs use role="tablist", role="tab", role="tabpanel"
- [ ] Expandable sections use aria-expanded
- [ ] Loading states use aria-busy

## Testing Commands

```bash
# Lighthouse accessibility audit
npx lighthouse URL --only-categories=accessibility

# axe-core with Playwright
npx playwright test --grep accessibility
```
