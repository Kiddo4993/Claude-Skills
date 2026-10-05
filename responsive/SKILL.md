---
name: responsive
description: Make any UI layout fully responsive using mobile-first Tailwind breakpoints, fixing overflow, font scaling, touch targets, and grid/flex layouts
trigger: manual
---

# Responsive UI Skill

Transform or audit any UI to be fully responsive across all breakpoints.

## Breakpoints (Tailwind defaults)
| Prefix | Min-width | Target devices |
|--------|-----------|----------------|
| (none) | 0px       | Mobile portrait |
| `sm:`  | 640px     | Mobile landscape / small tablet |
| `md:`  | 768px     | Tablet portrait |
| `lg:`  | 1024px    | Tablet landscape / small laptop |
| `xl:`  | 1280px    | Desktop |
| `2xl:` | 1536px    | Wide desktop |

## Mobile-First Rules
- Start with the mobile layout, then add `sm:` / `md:` / `lg:` overrides
- Never use fixed pixel widths on containers — use `w-full`, `max-w-*`, or `%`
- Replace `flex-row` with `flex-col` on mobile: `flex flex-col md:flex-row`
- Replace multi-column grids with single column: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3`

## Common Fixes

### Typography scaling
```html
<!-- Bad -->
<h1 class="text-6xl">Title</h1>

<!-- Good -->
<h1 class="text-3xl sm:text-4xl lg:text-6xl">Title</h1>
```

### Navigation → Hamburger menu
On mobile: hide nav links, show hamburger button.
On desktop: show full nav, hide hamburger.

### Images
```html
<img class="w-full h-auto object-cover" ... />
```

### Touch targets
All buttons/links: `min-h-[44px] min-w-[44px]`

### Overflow prevention
- Long text: `break-words` or `truncate` with `overflow-hidden`
- Tables: wrap in `overflow-x-auto`
- Code blocks: `overflow-x-auto whitespace-pre`

## When invoked
1. Identify the current layout issues on mobile
2. Apply mobile-first fixes using the patterns above
3. Test at 320px, 768px, and 1280px mentally — describe any remaining edge cases
