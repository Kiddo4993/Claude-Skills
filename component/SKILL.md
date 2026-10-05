---
name: component
description: Scaffold a production-ready React + Tailwind component with props, TypeScript types, variants, and usage example
trigger: manual
---

# Component Generator Skill

Generate a production-ready React component. Follow these standards:

## Stack
- React (functional components + hooks)
- TypeScript (strict types, no `any`)
- Tailwind CSS v3+ for styling
- shadcn/ui primitives where appropriate (Radix UI under the hood)

## Component Structure
Every component should include:

```tsx
// 1. TypeScript interface for props
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost' | 'destructive'
  size?: 'sm' | 'md' | 'lg'
  disabled?: boolean
  loading?: boolean
  onClick?: () => void
  children: React.ReactNode
}

// 2. The component with defaults
export function Button({ variant = 'primary', size = 'md', ...props }: ButtonProps) { ... }

// 3. Usage example in a comment or separate snippet
```

## Variants Pattern
Use `class-variance-authority` (cva) for multi-variant components:
```tsx
import { cva, type VariantProps } from 'class-variance-authority'
const buttonVariants = cva('base classes', {
  variants: { variant: { primary: '...', secondary: '...' } }
})
```

## Rules
- No hardcoded colors — use Tailwind semantic tokens or CSS variables
- Loading states should disable interaction and show a spinner
- Error states should be visually distinct but not panic-inducing
- Always export both the component AND its props type
- Include a JSDoc comment only if the component has non-obvious behavior

## When invoked
Ask the user: "What component do you need? Give me a name and describe what it does." Then generate it following the above standards.
