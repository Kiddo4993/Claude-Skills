---
name: micro-interactions
description: Add premium micro-interactions to UI elements — hover states, click feedback, focus rings, toggle switches, button presses, and form field animations that feel expensive
trigger: manual
---

# Micro-Interactions Skill — The Details That Cost $10k

Micro-interactions are what separate a $500 website from a $50,000 one. Users feel them subconsciously. Every interactive element should respond to input as if it has mass and resistance.

## Core Principle
**Every action deserves a reaction.** Click → compress. Hover → lift. Error → shake. Success → bloom. Load → pulse.

---

## Button Interactions

### Press effect (physical depth)
```tsx
<motion.button
  whileHover={{ scale: 1.02, boxShadow: '0 8px 30px rgba(0,0,0,0.12)' }}
  whileTap={{ scale: 0.97, boxShadow: '0 2px 8px rgba(0,0,0,0.08)' }}
  transition={{ type: 'spring', stiffness: 500, damping: 30 }}
>
  Click me
</motion.button>
```

### Loading → success state
```tsx
const [state, setState] = useState<'idle' | 'loading' | 'success'>('idle')

<AnimatePresence mode="wait">
  {state === 'idle' && (
    <motion.span key="idle" initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0, y: -8 }}>
      Submit
    </motion.span>
  )}
  {state === 'loading' && (
    <motion.span key="loading" initial={{ opacity: 0, y: 8 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0 }}>
      <Spinner />
    </motion.span>
  )}
  {state === 'success' && (
    <motion.span key="success" initial={{ scale: 0 }} animate={{ scale: 1 }} transition={{ type: 'spring', stiffness: 400, damping: 15 }}>
      ✓
    </motion.span>
  )}
</AnimatePresence>
```

---

## Toggle / Switch (feels mechanical)
```tsx
function Toggle({ checked, onChange }: { checked: boolean; onChange: () => void }) {
  return (
    <motion.button
      onClick={onChange}
      className={`relative w-12 h-6 rounded-full ${checked ? 'bg-blue-500' : 'bg-gray-200'}`}
      animate={{ backgroundColor: checked ? '#3b82f6' : '#e5e7eb' }}
      transition={{ duration: 0.2 }}
    >
      <motion.div
        className="absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow-md"
        animate={{ x: checked ? 24 : 0 }}
        transition={{ type: 'spring', stiffness: 500, damping: 35 }}
      />
    </motion.button>
  )
}
```

---

## Input Field Focus (Stripe-style)
```tsx
const [focused, setFocused] = useState(false)
const [hasValue, setHasValue] = useState(false)

<div className="relative">
  <motion.label
    className="absolute left-3 pointer-events-none text-gray-400"
    animate={{
      top: focused || hasValue ? 6 : 14,
      fontSize: focused || hasValue ? '11px' : '14px',
      color: focused ? '#3b82f6' : '#9ca3af'
    }}
    transition={{ type: 'spring', stiffness: 400, damping: 30 }}
  >
    Email address
  </motion.label>
  <motion.div
    className="absolute bottom-0 left-0 h-[2px] bg-blue-500"
    animate={{ scaleX: focused ? 1 : 0 }}
    style={{ transformOrigin: 'left' }}
    transition={{ type: 'spring', stiffness: 300, damping: 25 }}
  />
  <input
    className="w-full pt-6 pb-2 px-3 border border-gray-200 rounded-lg outline-none"
    onFocus={() => setFocused(true)}
    onBlur={() => setFocused(false)}
    onChange={e => setHasValue(e.target.value.length > 0)}
  />
</div>
```

---

## Error Shake
```tsx
const shakeVariants = {
  shake: {
    x: [0, -8, 8, -6, 6, -3, 3, 0],
    transition: { duration: 0.5, ease: 'easeInOut' }
  }
}

const [hasError, setHasError] = useState(false)
const [shakeKey, setShakeKey] = useState(0)

function triggerError() {
  setHasError(true)
  setShakeKey(k => k + 1) // re-trigger animation
}

<motion.div key={shakeKey} variants={shakeVariants} animate={hasError ? 'shake' : ''}>
  <input className={`border ${hasError ? 'border-red-500' : 'border-gray-200'}`} />
</motion.div>
```

---

## Hover Card Lift (Linear-style)
```tsx
<motion.div
  className="rounded-xl border border-gray-100 bg-white p-6 cursor-pointer"
  whileHover={{
    y: -4,
    boxShadow: '0 20px 40px rgba(0,0,0,0.08), 0 4px 12px rgba(0,0,0,0.04)',
    borderColor: 'rgba(0,0,0,0.1)'
  }}
  transition={{ type: 'spring', stiffness: 400, damping: 25 }}
>
  {children}
</motion.div>
```

---

## Tooltip with scale origin
```tsx
<motion.div
  initial={{ opacity: 0, scale: 0.85, y: 4 }}
  animate={{ opacity: 1, scale: 1, y: 0 }}
  exit={{ opacity: 0, scale: 0.85, y: 4 }}
  transition={{ type: 'spring', stiffness: 400, damping: 25 }}
  style={{ transformOrigin: 'bottom center' }}
  className="absolute bottom-full mb-2 px-3 py-1.5 bg-gray-900 text-white text-xs rounded-lg"
>
  {label}
</motion.div>
```

---

## Checklist before shipping any micro-interaction
- [ ] Spring physics used (not linear/ease)
- [ ] Exit animation is faster/simpler than entry
- [ ] `prefers-reduced-motion` respected
- [ ] No layout thrashing (only transform/opacity animated)
- [ ] Works on mobile touch (test `whileTap` not just `whileHover`)
