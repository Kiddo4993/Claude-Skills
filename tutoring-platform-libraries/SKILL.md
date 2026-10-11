---
name: tutoring-platform-libraries
description: Reference of the libraries used by the private-tutoring-platform app (Next.js 16, React 19, Tailwind 4, Neon, NextAuth, Anthropic SDK, etc.). Use when working on or extending that project.
---

# private-tutoring-platform: libraries

Scripts: `npm run dev` (next dev --webpack), `build`, `start`, `lint`.

## Framework / runtime
- next 16.1.6, react / react-dom 19.2.3, typescript ^5
- babel-plugin-react-compiler 1.0.0

## Auth & data
- next-auth ^5.0.0-beta.30
- @neondatabase/serverless ^1.0.2 (Postgres)
- ws ^8.19.0

## AI / media
- @anthropic-ai/sdk ^0.78.0
- replicate ^1.4.0

## UI & styling
- tailwindcss ^4 (@tailwindcss/postcss), tw-animate-css, tailwind-merge, clsx, class-variance-authority
- shadcn ^4.1.2, @base-ui/react ^1.3.0
- lucide-react (icons), geist (font), motion (animation)

## Content rendering
- react-markdown, remark-gfm, remark-math, rehype-katex, rehype-raw, katex
- react-syntax-highlighter (code blocks)
- @monaco-editor/react (code editor)

## Dev tooling
- eslint ^9, eslint-config-next 16.1.6, @types/node, @types/react, @types/react-dom
