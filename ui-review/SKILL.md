---
name: ui-review
description: Review existing UI/frontend code for design quality, accessibility, responsiveness, performance, and best practices — outputs a prioritized fix list
trigger: manual
---

# UI Review Skill

Conduct a thorough review of the provided UI code. Evaluate across these dimensions:

## Review Checklist

### 1. Accessibility (highest priority)
- [ ] Color contrast ratio ≥ 4.5:1 for normal text, ≥ 3:1 for large text
- [ ] All interactive elements have visible focus rings
- [ ] Images have descriptive `alt` text (or `alt=""` for decorative)
- [ ] Form inputs have associated `<label>` elements
- [ ] ARIA roles/labels used where semantic HTML isn't enough
- [ ] Keyboard navigation works without a mouse
- [ ] Screen reader flow makes logical sense

### 2. Responsiveness
- [ ] Layout works on mobile (320px), tablet (768px), desktop (1280px+)
- [ ] No fixed pixel widths that break on small screens
- [ ] Touch targets are at least 44x44px
- [ ] Text doesn't overflow containers

### 3. Design Quality
- [ ] Consistent spacing scale (not random pixel values)
- [ ] Typography hierarchy is clear (heading > subheading > body > caption)
- [ ] Color palette is cohesive and not overloaded
- [ ] No visual noise — every element earns its place

### 4. Performance
- [ ] Images use modern formats (WebP, AVIF) and have `width`/`height`
- [ ] No layout shifts from late-loading content
- [ ] No unnecessary re-renders (React: check memo, callbacks)

### 5. Code Quality
- [ ] No inline styles that belong in CSS/Tailwind
- [ ] Components are reasonably small and single-purpose
- [ ] No hardcoded colors/sizes that should be design tokens

## Output Format
Return a **prioritized fix list** grouped by severity:
- 🔴 **Critical** (accessibility blockers, broken layouts)
- 🟡 **Important** (design inconsistencies, responsiveness gaps)
- 🟢 **Nice to have** (polish, performance micro-optimizations)

For each issue: describe the problem, show the offending code snippet, and provide the fix.
