---
name: scroll-animations
description: Implement scroll-driven animations — parallax, reveal-on-scroll, progress indicators, sticky headers, and GSAP ScrollTrigger sequences used in agency and product sites
trigger: manual
---

# Scroll Animations Skill — Agency / Product Quality

Scroll animations are how high-budget sites earn their budget. Done right they feel cinematic. Done wrong they cause motion sickness. This skill implements them correctly.

## Two approaches
| Use case | Tool |
|---|---|
| React components, single elements | Framer Motion `useScroll` + `useTransform` |
| Complex timelines, pinned sequences, scrubbing | GSAP `ScrollTrigger` |

---

## Framer Motion: Fade + Slide on scroll entry

```tsx
import { motion, useInView } from 'framer-motion'
import { useRef } from 'react'

function RevealOnScroll({ children, delay = 0 }: { children: React.ReactNode; delay?: number }) {
  const ref = useRef(null)
  const isInView = useInView(ref, { once: true, margin: '-80px 0px' })

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, y: 32, filter: 'blur(6px)' }}
      animate={isInView ? { opacity: 1, y: 0, filter: 'blur(0px)' } : {}}
      transition={{ type: 'spring', stiffness: 200, damping: 24, delay }}
    >
      {children}
    </motion.div>
  )
}
```

---

## Framer Motion: Scroll progress bar
```tsx
import { useScroll, useSpring, motion } from 'framer-motion'

function ScrollProgressBar() {
  const { scrollYProgress } = useScroll()
  const scaleX = useSpring(scrollYProgress, { stiffness: 100, damping: 30 })

  return (
    <motion.div
      className="fixed top-0 left-0 right-0 h-[3px] bg-blue-500 origin-left z-50"
      style={{ scaleX }}
    />
  )
}
```

---

## Framer Motion: Parallax hero image
```tsx
import { useScroll, useTransform, motion } from 'framer-motion'
import { useRef } from 'react'

function ParallaxHero() {
  const ref = useRef(null)
  const { scrollYProgress } = useScroll({ target: ref, offset: ['start start', 'end start'] })
  const y = useTransform(scrollYProgress, [0, 1], ['0%', '30%'])
  const opacity = useTransform(scrollYProgress, [0, 0.6], [1, 0])

  return (
    <section ref={ref} className="relative h-screen overflow-hidden">
      <motion.div className="absolute inset-0" style={{ y }}>
        <img src="/hero.jpg" className="w-full h-full object-cover" alt="" />
      </motion.div>
      <motion.div className="relative z-10 flex items-center justify-center h-full" style={{ opacity }}>
        <h1 className="text-white text-7xl font-bold">Hero Text</h1>
      </motion.div>
    </section>
  )
}
```

---

## GSAP ScrollTrigger: Pinned horizontal scroll (Awwwards-style)
```tsx
import { useEffect, useRef } from 'react'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

function HorizontalScrollSection() {
  const sectionRef = useRef(null)
  const trackRef = useRef(null)

  useEffect(() => {
    const track = trackRef.current
    const totalWidth = track.scrollWidth - window.innerWidth

    gsap.to(track, {
      x: -totalWidth,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionRef.current,
        start: 'top top',
        end: `+=${totalWidth}`,
        scrub: 1,
        pin: true,
        anticipatePin: 1,
      }
    })

    return () => ScrollTrigger.getAll().forEach(t => t.kill())
  }, [])

  return (
    <section ref={sectionRef} className="overflow-hidden">
      <div ref={trackRef} className="flex w-max gap-8 p-16">
        {panels.map((panel, i) => (
          <div key={i} className="w-[80vw] h-screen flex-shrink-0 bg-gray-100 rounded-2xl" />
        ))}
      </div>
    </section>
  )
}
```

---

## GSAP ScrollTrigger: Stagger text reveal (line by line)
```tsx
useEffect(() => {
  const lines = gsap.utils.toArray('.reveal-line')

  lines.forEach((line, i) => {
    gsap.fromTo(line,
      { y: '100%', opacity: 0 },
      {
        y: '0%',
        opacity: 1,
        duration: 0.8,
        ease: 'power3.out',
        delay: i * 0.1,
        scrollTrigger: {
          trigger: line,
          start: 'top 90%',
          toggleActions: 'play none none none'
        }
      }
    )
  })

  return () => ScrollTrigger.getAll().forEach(t => t.kill())
}, [])
```

---

## Performance rules for scroll animations
- Only animate `transform` and `opacity` — never `top`, `left`, `width`, `height`
- Add `will-change: transform` to elements that scroll-animate continuously
- Always cleanup ScrollTrigger instances in `useEffect` return
- Use `scrub: 1` (not `scrub: true`) for smoothed scrubbing
- Test on a low-end Android device — if it drops frames there, it ships broken
- `margin: '-80px 0px'` in `useInView` prevents animations from firing too early
