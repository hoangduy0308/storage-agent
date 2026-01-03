# Responsive Design Patterns

## Breakpoints (Tailwind defaults)

| Breakpoint | Min Width | Typical Device |
|------------|-----------|----------------|
| (default)  | 0px       | Mobile         |
| sm         | 640px     | Large phone    |
| md         | 768px     | Tablet         |
| lg         | 1024px    | Laptop         |
| xl         | 1280px    | Desktop        |
| 2xl        | 1536px    | Large desktop  |

## Common Patterns

### Mobile-First Grid

```tsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
```

### Responsive Typography

```tsx
<h1 className="text-2xl sm:text-3xl lg:text-4xl xl:text-5xl">
```

### Hide/Show Elements

```tsx
<nav className="hidden md:flex">Desktop nav</nav>
<button className="md:hidden">Mobile menu</button>
```

### Responsive Padding/Margin

```tsx
<section className="px-4 sm:px-6 lg:px-8 py-8 sm:py-12 lg:py-16">
```

### Stack to Row

```tsx
<div className="flex flex-col md:flex-row gap-4">
```

### Responsive Images

```tsx
<Image
  src="/hero.jpg"
  alt="Hero"
  className="w-full h-48 sm:h-64 lg:h-80 object-cover"
/>
```

## Testing Breakpoints

With Playwright:

```typescript
// Test multiple viewports
const viewports = [
  { width: 375, height: 667, name: 'mobile' },
  { width: 768, height: 1024, name: 'tablet' },
  { width: 1280, height: 800, name: 'desktop' },
];

for (const vp of viewports) {
  await page.setViewportSize({ width: vp.width, height: vp.height });
  await page.screenshot({ path: `screenshots/${vp.name}.png` });
}
```
