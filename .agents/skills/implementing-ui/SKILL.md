---
name: implementing-ui
description: Implements frontend UI using shadcn components. Use when building React/Next.js pages, components, or UI features.
---

# Implementing UI with shadcn

Build UI by searching, selecting, and installing shadcn components.

## When to Use

- Building React/Next.js pages or components
- Adding UI components to project
- Need consistent, accessible UI primitives

Use shadcn MCP tools as the primary interface for component discovery and installation. Avoid other UI libraries unless explicitly requested.

## Workflow

### 1. Understand Requirements

- What UI is needed? (form, table, navigation, etc.)
- What states? (loading, error, empty, success)
- Responsive needs?

For design thinking, load `frontend-design` skill first.

### 2. Search Components

```
mcp__shadcn__search_items_in_registries(["@shadcn"], "button")
mcp__shadcn__search_items_in_registries(["@shadcn"], "form")
```

### 3. View Component Details

```
mcp__shadcn__view_items_in_registries(["@shadcn/button", "@shadcn/card"])
mcp__shadcn__get_item_examples_from_registries(["@shadcn"], "button-demo")
```

### 4. Get Install Command

```
mcp__shadcn__get_add_command_for_items(["@shadcn/button", "@shadcn/card"])
```

Then run:

```bash
npx shadcn@latest add button card
```

### 5. Implement

Use installed components with Tailwind styling.

### 6. Verify

```
mcp__shadcn__get_audit_checklist("verify components working")
```

## Quick Reference

### shadcn MCP Tools

| Tool | Purpose |
|------|---------|
| `search_items_in_registries` | Find components by name |
| `view_items_in_registries` | See component details & code |
| `get_item_examples_from_registries` | Get usage examples |
| `get_add_command_for_items` | Get install command |
| `get_audit_checklist` | Verify implementation |

### Common Components

| Need | Component |
|------|-----------|
| Forms | form, input, select, checkbox, radio-group, textarea |
| Layout | card, separator, tabs, accordion, collapsible |
| Feedback | alert, toast, dialog, tooltip, popover |
| Navigation | navigation-menu, breadcrumb, pagination, menubar |
| Data | table, data-table, calendar, date-picker |
| Actions | button, dropdown-menu, context-menu, command |

### Install Commands

```bash
npx shadcn@latest add [component]  # Add component
npx shadcn@latest add -a           # Add all components
npx shadcn@latest diff [component] # Check for updates
```

## Common Patterns

### Form with Validation

```tsx
import { Form, FormField, FormItem, FormLabel, FormControl, FormMessage } from "@/components/ui/form"
import { Input } from "@/components/ui/input"
import { Button } from "@/components/ui/button"

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
  <Button type="submit">Submit</Button>
</Form>
```

### Card Layout

```tsx
import { Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter } from "@/components/ui/card"

<Card>
  <CardHeader>
    <CardTitle>Title</CardTitle>
    <CardDescription>Description</CardDescription>
  </CardHeader>
  <CardContent>Content here</CardContent>
  <CardFooter>Footer actions</CardFooter>
</Card>
```

### Dialog

```tsx
import { Dialog, DialogTrigger, DialogContent, DialogHeader, DialogTitle, DialogDescription } from "@/components/ui/dialog"
import { Button } from "@/components/ui/button"

<Dialog>
  <DialogTrigger asChild>
    <Button>Open</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Title</DialogTitle>
      <DialogDescription>Description</DialogDescription>
    </DialogHeader>
    {/* Content */}
  </DialogContent>
</Dialog>
```

## Additional Resources

For detailed checklists and patterns, see:
- `reference/accessibility-checklist.md` - WCAG compliance, keyboard nav, ARIA patterns
- `reference/responsive-patterns.md` - Tailwind breakpoints, mobile-first patterns
