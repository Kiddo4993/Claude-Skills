---
name: loading-states
description: Design professional loading animations — skeleton screens, shimmer effects, spinners, progress bars, and optimistic UI patterns used by Stripe, Linear, and GitHub
trigger: manual
---

# Loading States Skill — No More Spinning Circles

Amateur UIs show a spinner and block everything. Professional UIs keep users oriented, optimistic, and never feel frozen. This skill implements the full spectrum of loading patterns.

## Hierarchy of loading patterns (best to worst UX)
1. **Optimistic UI** — show the result immediately, confirm in background
2. **Skeleton screens** — show the shape of what's coming
3. **Progress indicator** — show how much is left
4. **Shimmer/pulse** — signal that content is loading in-place
5. **Spinner** — last resort, only for indeterminate short waits

---

## Skeleton Screen (Tailwind + CSS)
```tsx
function SkeletonCard() {
  return (
    <div className="rounded-xl border border-gray-100 p-6 space-y-4">
      <div className="flex items-center gap-3">
        <div className="w-10 h-10 rounded-full bg-gray-100 animate-pulse" />
        <div className="space-y-2 flex-1">
          <div className="h-3 bg-gray-100 rounded-full w-1/3 animate-pulse" />
          <div className="h-3 bg-gray-100 rounded-full w-1/4 animate-pulse" />
        </div>
      </div>
      <div className="space-y-2">
        <div className="h-3 bg-gray-100 rounded-full w-full animate-pulse" />
        <div className="h-3 bg-gray-100 rounded-full w-5/6 animate-pulse" />
        <div className="h-3 bg-gray-100 rounded-full w-4/6 animate-pulse" />
      </div>
    </div>
  )
}
```

---

## Shimmer effect (premium feel — used by LinkedIn, Facebook)
```tsx
// Add to globals.css
// @keyframes shimmer { from { background-position: -200% 0 } to { background-position: 200% 0 } }
// .shimmer { background: linear-gradient(90deg, #f0f0f0 25%, #e8e8e8 50%, #f0f0f0 75%); background-size: 200% 100%; animation: shimmer 1.6s infinite; }

function ShimmerBlock({ className }: { className?: string }) {
  return (
    <div
      className={`rounded ${className}`}
      style={{
        background: 'linear-gradient(90deg, #f0f0f0 25%, #e8e8e8 50%, #f0f0f0 75%)',
        backgroundSize: '200% 100%',
        animation: 'shimmer 1.6s infinite',
      }}
    />
  )
}
```

---

## Framer Motion skeleton with stagger
```tsx
import { motion } from 'framer-motion'

const skeletonVariants = {
  pulse: {
    opacity: [1, 0.4, 1],
    transition: { duration: 1.5, repeat: Infinity, ease: 'easeInOut' }
  }
}

function SkeletonLine({ width = 'full', delay = 0 }: { width?: string; delay?: number }) {
  return (
    <motion.div
      variants={skeletonVariants}
      animate="pulse"
      transition={{ delay }}
      className={`h-3 rounded-full bg-gray-100 w-${width}`}
    />
  )
}
```

---

## Determinate progress bar (file upload, multi-step form)
```tsx
import { motion } from 'framer-motion'

function ProgressBar({ value }: { value: number }) { // value: 0–100
  return (
    <div className="w-full h-1.5 bg-gray-100 rounded-full overflow-hidden">
      <motion.div
        className="h-full bg-blue-500 rounded-full origin-left"
        initial={{ scaleX: 0 }}
        animate={{ scaleX: value / 100 }}
        transition={{ type: 'spring', stiffness: 80, damping: 20 }}
      />
    </div>
  )
}
```

---

## Premium spinner (not the default border trick)
```tsx
function Spinner({ size = 20, color = '#3b82f6' }: { size?: number; color?: string }) {
  return (
    <svg width={size} height={size} viewBox="0 0 24 24" fill="none">
      <motion.circle
        cx="12" cy="12" r="10"
        stroke={color}
        strokeWidth="2.5"
        strokeLinecap="round"
        strokeDasharray="32"
        strokeDashoffset="32"
        animate={{ strokeDashoffset: [32, 8, 32], rotate: [0, 270, 360] }}
        transition={{ duration: 1.4, repeat: Infinity, ease: 'easeInOut' }}
        style={{ transformOrigin: 'center' }}
      />
    </svg>
  )
}
```

---

## Optimistic UI pattern
```tsx
function CommentList() {
  const [comments, setComments] = useState(initialComments)

  async function addComment(text: string) {
    // 1. Show the result immediately with a temp ID
    const optimistic = { id: `temp-${Date.now()}`, text, pending: true }
    setComments(prev => [...prev, optimistic])

    try {
      // 2. Confirm with server
      const saved = await api.createComment(text)
      setComments(prev => prev.map(c => c.id === optimistic.id ? saved : c))
    } catch {
      // 3. Rollback on failure
      setComments(prev => prev.filter(c => c.id !== optimistic.id))
      toast.error('Failed to post comment')
    }
  }

  return (
    <AnimatePresence>
      {comments.map(comment => (
        <motion.div
          key={comment.id}
          initial={{ opacity: 0, y: 8 }}
          animate={{ opacity: comment.pending ? 0.6 : 1, y: 0 }}
          exit={{ opacity: 0, height: 0 }}
          transition={{ type: 'spring', stiffness: 400, damping: 30 }}
        >
          {comment.text}
        </motion.div>
      ))}
    </AnimatePresence>
  )
}
```

---

## Content-aware skeleton matching
When generating a skeleton for any given component, match:
- The exact number of text lines
- Relative widths (last line shorter = feels natural)
- Avatar/image placeholder dimensions
- Button placeholder width and height

Never use a generic full-width rectangle skeleton for a complex layout — it breaks the spatial promise to the user.
