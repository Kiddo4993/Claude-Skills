---
name: page-transitions
description: Implement fluid page and route transitions in React/Next.js — shared element transitions, crossfades, slide-ins, and View Transitions API used by top-tier web products
trigger: manual
---

# Page Transitions Skill — Product-Grade Route Animations

Page transitions are what make a web app feel like a native app. The wrong transition breaks immersion instantly. This skill implements them at the level of Vercel, Linear, and Notion.

## Principles
- **Transitions carry context**: a slide-left/right implies navigation depth; a crossfade implies a peer swap
- **Consistent direction**: deeper pages slide in from the right; going back slides right → left
- **Never block content**: the transition should complete in ≤ 500ms
- **Shared elements** (same element morphing between routes) are the most premium technique

---

## Next.js App Router: Route transitions with Framer Motion

```tsx
// app/template.tsx  (template.tsx re-mounts on every route change, layout.tsx does not)
'use client'
import { motion } from 'framer-motion'

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 12 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -8 }}
      transition={{ type: 'spring', stiffness: 300, damping: 30, duration: 0.3 }}
    >
      {children}
    </motion.div>
  )
}
```

---

## Directional slide (mobile app feel)
```tsx
// Track navigation direction in a context or URL param
type Direction = 'forward' | 'back'

const slideVariants = (direction: Direction) => ({
  initial: { x: direction === 'forward' ? '100%' : '-100%', opacity: 0 },
  enter:   { x: 0, opacity: 1 },
  exit:    { x: direction === 'forward' ? '-20%' : '100%', opacity: 0 }
})

<motion.div
  variants={slideVariants(direction)}
  initial="initial"
  animate="enter"
  exit="exit"
  transition={{ type: 'spring', stiffness: 300, damping: 35 }}
>
  {children}
</motion.div>
```

---

## View Transitions API (native browser, zero JS animation cost)
```tsx
// app/link.tsx — wrapper around Next <Link> that triggers View Transition
'use client'
import NextLink from 'next/link'
import { useRouter } from 'next/navigation'

export function Link({ href, children, ...props }) {
  const router = useRouter()

  function handleClick(e: React.MouseEvent) {
    e.preventDefault()
    if (!document.startViewTransition) {
      router.push(href)
      return
    }
    document.startViewTransition(() => {
      router.push(href)
    })
  }

  return <NextLink href={href} onClick={handleClick} {...props}>{children}</NextLink>
}
```

```css
/* globals.css */
@keyframes fade-in  { from { opacity: 0; transform: translateY(12px); } }
@keyframes fade-out { to   { opacity: 0; transform: translateY(-8px); } }

::view-transition-old(root) {
  animation: 200ms ease-in fade-out;
}
::view-transition-new(root) {
  animation: 300ms ease-out fade-in;
}
```

---

## Shared element transition (card → detail view)
```tsx
// List page — card
<motion.div layoutId={`card-${item.id}`} className="rounded-xl bg-white p-6">
  <motion.h2 layoutId={`title-${item.id}`}>{item.title}</motion.h2>
</motion.div>

// Detail page — expanded card (same layoutId = Framer morphs between them)
<motion.div layoutId={`card-${item.id}`} className="fixed inset-0 bg-white p-12">
  <motion.h2 layoutId={`title-${item.id}`} className="text-4xl">{item.title}</motion.h2>
</motion.div>
```
Wrap both pages in `<AnimatePresence>` — Framer handles the morph automatically.

---

## Modal / Sheet / Drawer transitions

### Bottom sheet (mobile)
```tsx
<AnimatePresence>
  {open && (
    <>
      <motion.div
        className="fixed inset-0 bg-black/40 z-40"
        initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }}
        onClick={onClose}
      />
      <motion.div
        className="fixed bottom-0 left-0 right-0 bg-white rounded-t-2xl z-50 p-6"
        initial={{ y: '100%' }}
        animate={{ y: 0 }}
        exit={{ y: '100%' }}
        transition={{ type: 'spring', stiffness: 350, damping: 35 }}
      >
        {children}
      </motion.div>
    </>
  )}
</AnimatePresence>
```

### Center modal with scale
```tsx
<motion.div
  initial={{ opacity: 0, scale: 0.95, y: 8 }}
  animate={{ opacity: 1, scale: 1, y: 0 }}
  exit={{ opacity: 0, scale: 0.95, y: 4 }}
  transition={{ type: 'spring', stiffness: 400, damping: 28 }}
  className="fixed inset-0 m-auto w-[480px] h-fit bg-white rounded-2xl shadow-2xl z-50 p-8"
>
  {children}
</motion.div>
```

---

## Common mistakes
- Wrapping `<AnimatePresence>` inside the component being animated (it must be a parent)
- Using `layout` prop without `LayoutGroup` when multiple shared-element components exist simultaneously
- Page transitions that run on the initial load (use `initial={false}` on `<AnimatePresence>` to skip first render)
- Exit animations that outlive their container (z-index, position absolute)
