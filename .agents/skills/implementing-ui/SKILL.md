---
name: implementing-ui
description: Implements frontend UI from designs or specs using shadcn, Figma MCP, and validates with Playwright. Use when building React/Next.js pages, components, or UI features.
---

# Implementing UI: Design → Code → Validate

End-to-end workflow: interpret UX goals → extract design specs → implement with shadcn → validate with Playwright.

## When to Use

- Building React/Next.js pages or components
- Implementing UI from Figma designs
- Creating UI from textual specifications
- Adding shadcn components to project

## MCP Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| **shadcn** | Component registry | Installing/searching UI components |
| **Figma MCP** | Design extraction | When Figma file available |
| **Playwright MCP** | Browser automation | Visual testing, UX validation |
| **Browser Tools** | Audits | Lighthouse, console monitoring |
| **Context7** | Framework docs | Verify React/Next.js/Tailwind APIs |

## Workflow

### Phase 1: Clarify Requirements

Before coding, understand:
- Target audience and use case
- Key user flows (3-5 max)
- Technical constraints (framework, responsive needs)
- Design source: Figma link or textual spec

For design thinking, load `frontend-design` skill first.

### Phase 2: Extract Design (if Figma available)

```
mcp__figma__get_design_context(file_key, node_id)
```

Extract:
- Color tokens, typography scales
- Spacing and layout patterns
- Component variants (buttons, inputs, cards)
- Responsive breakpoints

### Phase 3: Plan Components

Map UI to components:

```markdown
## Component Map

| UI Element | Source | File Path |
|------------|--------|-----------|
| Hero section | shadcn Card + custom | components/hero.tsx |
| Navigation | shadcn NavigationMenu | components/nav.tsx |
| Form | shadcn Form + Input | components/contact-form.tsx |
```

### Phase 4: Select & Install Components

Search shadcn registry:

```
mcp__shadcn__search_items_in_registries(["@shadcn"], "button")
mcp__shadcn__view_items_in_registries(["@shadcn/button", "@shadcn/card"])
mcp__shadcn__get_add_command_for_items(["@shadcn/button"])
```

Install needed components:

```bash
npx shadcn@latest add button card form input
```

### Phase 5: Implement

Guidelines:
- Use Tailwind classes, not hardcoded styles
- Implement all states: loading, error, empty, success
- Add accessibility: aria-*, alt text, keyboard nav
- Follow existing project conventions

Verify framework APIs with Context7:

```
mcp__context7__resolve_library_id("next.js")
mcp__context7__get_library_docs(library_id, "server components")
```

### Phase 6: Validate with Playwright

Run visual tests:

```
mcp__playwright__browser_navigate(url)
mcp__playwright__browser_screenshot()
mcp__playwright__browser_click(selector)
```

Smoke test key flows:
1. Navigation works
2. Forms submit correctly
3. Error states display properly
4. Responsive layouts render correctly

### Phase 7: Audit & Iterate

Run Lighthouse via Browser Tools:

```
mcp__browser_tools__runLighthouse(url)
mcp__browser_tools__getConsoleLogs()
```

Check:
- [ ] Performance score > 90
- [ ] Accessibility score > 90
- [ ] No console errors
- [ ] No layout shifts

## Quick Reference

### shadcn Commands

```bash
npx shadcn@latest add [component]  # Add component
npx shadcn@latest diff [component] # Check for updates
```

### Common Components

| Need | shadcn Component |
|------|-----------------|
| Forms | form, input, select, checkbox, radio-group |
| Layout | card, separator, tabs, accordion |
| Feedback | alert, toast, dialog, tooltip |
| Navigation | navigation-menu, breadcrumb, pagination |
| Data | table, data-table, calendar |

### Figma to Code Mapping

| Figma | Tailwind |
|-------|----------|
| Auto Layout | flex, gap-* |
| Fill Container | w-full, h-full |
| Fixed Width | w-[px] |
| Padding | p-*, px-*, py-* |
| Corner Radius | rounded-* |

## Integration with Other Skills

- **frontend-design**: Load first for design thinking and UX principles
- **planning**: Use when UI is part of larger feature epic
- **reviewing-beads**: Create issues for UI tasks

## Common Patterns

### Responsive Layout

```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  {items.map(item => <Card key={item.id} {...item} />)}
</div>
```

### Form with Validation

```tsx
<Form {...form}>
  <FormField
    control={form.control}
    name="email"
    render={({ field }) => (
      <FormItem>
        <FormLabel>Email</FormLabel>
        <FormControl>
          <Input placeholder="email@example.com" {...field} />
        </FormControl>
        <FormMessage />
      </FormItem>
    )}
  />
</Form>
```

### Loading State

```tsx
{isLoading ? (
  <Skeleton className="h-12 w-full" />
) : (
  <Content data={data} />
)}
```
