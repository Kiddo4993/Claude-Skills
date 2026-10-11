---
name: styling-primitives
description: Utility styling stack for Tailwind 4 React apps: clsx, tailwind-merge, class-variance-authority (cva), tw-animate-css, @base-ui/react, lucide-react icons, geist font and motion. Use when building variant-based components or wiring these helpers together.
---

# Styling primitives (Tailwind 4 stack)

Complements `ui-styling` (shadcn/Tailwind) and `animation-design` (Framer Motion/GSAP).

## `cn` helper (`lib/utils.ts`)
```ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";
export const cn = (...i: ClassValue[]) => twMerge(clsx(i));
```
`clsx` joins conditionals; `twMerge` resolves conflicting Tailwind classes (last wins).

## Variants with cva
```ts
import { cva, type VariantProps } from "class-variance-authority";
export const button = cva("inline-flex items-center rounded-md font-medium transition", {
  variants: {
    variant: { default: "bg-primary text-primary-foreground", ghost: "hover:bg-muted" },
    size: { sm: "h-8 px-3 text-sm", md: "h-10 px-4" },
  },
  defaultVariants: { variant: "default", size: "md" },
});
type Props = VariantProps<typeof button>;
```

## Other pieces
- **Tailwind 4**: CSS-first config (`@import "tailwindcss";` + `@theme`), PostCSS via `@tailwindcss/postcss`.
- **tw-animate-css**: `@import "tw-animate-css";` gives `animate-in`, `fade-in`, `slide-in-from-*` utilities (shadcn's replacement for tailwindcss-animate).
- **@base-ui/react**: unstyled accessible primitives (Dialog, Popover, Select, Tabs...) that shadcn v4 can build on; style with `className` / data attributes like `data-[open]`.
- **lucide-react**: `import { Check } from "lucide-react"`; size with `className="size-4"`.
- **geist**: `import { GeistSans } from "geist/font/sans"`; apply `GeistSans.variable` on `<html>`.
- **motion**: `import { motion } from "motion/react"`.
