# Claude Skills

My Claude Code skills (from `~/.claude/skills`).

## Install

```bash
git clone --recurse-submodules git@github.com:Kiddo4993/Claude-Skills.git
cp -R Claude-Skills/<skill-name> ~/.claude/skills/
```

## Skills

| Skill | What it does |
|---|---|
| `accessibility` | Audit UI code for WCAG 2.2 AA accessibility violations and output fixed code with explanations |
| `algorithmic-art` | Creating algorithmic art using p5.js with seeded randomness and interactive parameter exploration. Use this when users request creating art using code, gener… |
| `animation-design` | Design and implement enterprise-grade UI animations using Framer Motion and GSAP — physics-based, purposeful, brand-quality motion that Fortune 500 design sy… |
| `banner-design` | Design banners for social media, ads, website heroes, creative assets, and print. Multiple art direction options with AI-generated visuals. Actions: design,… |
| `brand` | Brand voice, visual identity, messaging frameworks, asset management, brand consistency. Activate for branded content, tone of voice, marketing assets, brand… |
| `brand-guidelines` | Applies Anthropic's official brand colors and typography to any sort of artifact that may benefit from having Anthropic's look-and-feel. Use it when brand co… |
| `canvas-design` | Create beautiful visual art in .png and .pdf documents using design philosophy. You should use this skill when the user asks to create a poster, piece of art… |
| `claude-api` | Build, debug, and optimize Claude API / Anthropic SDK apps. Apps built with this skill should include prompt caching. Also handles migrating existing Claude… |
| `claude-commands` | Reference of Claude Code's built-in special commands, CLI flags, and shortcuts — sessions (resume, rename, rewind, clear, compact, export), parallelism (back… |
| `component` | Scaffold a production-ready React + Tailwind component with props, TypeScript types, variants, and usage example |
| `design` | Comprehensive design skill: brand identity, design tokens, UI styling, logo generation (55 styles, Gemini AI), corporate identity program (50 deliverables, C… |
| `design-system` | Token architecture, component specifications, and slide generation. Three-layer tokens (primitive→semantic→component), CSS variables, spacing/typography scal… |
| `doc-coauthoring` | Guide users through a structured workflow for co-authoring documentation. Use when user wants to write documentation, proposals, technical specs, decision do… |
| `emoji-to-svg` |  |
| `facilitation` | Surveys every skill and plugin available in this Claude Code install and picks whichever one — or combination of several, run together or in sequence — best… |
| `favicon` | Designs a premium, hand-crafted-looking favicon and full icon set (SVG source, PNG sizes, apple-touch-icon, .ico) through several genuinely distinct concept… |
| `fork-session` | Fork or branch the current Claude Code conversation — copy everything discussed so far into a separate line of work so an alternative approach can be tried w… |
| `frontend-design` | Generate polished, distinctive frontend UI — avoids generic AI aesthetics, uses strong typography, purposeful color, and real visual hierarchy |
| `internal-comms` | A set of resources to help me write all kinds of internal communications, using the formats that my company likes to use. Claude should use this skill whenev… |
| `loading-states` | Design professional loading animations — skeleton screens, shimmer effects, spinners, progress bars, and optimistic UI patterns used by Stripe, Linear, and G… |
| `mcp-builder` | Guide for creating high-quality MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. Use whe… |
| `micro-interactions` | Add premium micro-interactions to UI elements — hover states, click feedback, focus rings, toggle switches, button presses, and form field animations that fe… |
| `page-transitions` | Implement fluid page and route transitions in React/Next.js — shared element transitions, crossfades, slide-ins, and View Transitions API used by top-tier we… |
| `responsive` | Make any UI layout fully responsive using mobile-first Tailwind breakpoints, fixing overflow, font scaling, touch targets, and grid/flex layouts |
| `scroll-animations` | Implement scroll-driven animations — parallax, reveal-on-scroll, progress indicators, sticky headers, and GSAP ScrollTrigger sequences used in agency and pro… |
| `skill` | Emil Kowalski's design-engineering skill (git submodule → [emilkowalski/skill](https://github.com/emilkowalski/skill)) |
| `skill-creator` | Create new skills, modify and improve existing skills, and measure skill performance. Use when users want to create a skill from scratch, edit, or optimize a… |
| `slack-gif-creator` | Knowledge and utilities for creating animated GIFs optimized for Slack. Provides constraints, validation tools, and animation concepts. Use when users reques… |
| `slides` | Create strategic HTML presentations with Chart.js, design tokens, responsive layouts, copywriting formulas, and contextual slide strategies. |
| `theme-factory` | Toolkit for styling artifacts with a theme. These artifacts can be slides, docs, reportings, HTML landing pages, etc. There are 10 pre-set themes with colors… |
| `typography` | Actively researches the live web (font foundries, Google Fonts, Fontshare, design roundups) to find and recommend a typography system genuinely fitting a spe… |
| `ui-review` | Review existing UI/frontend code for design quality, accessibility, responsiveness, performance, and best practices — outputs a prioritized fix list |
| `ui-styling` | Create beautiful, accessible user interfaces with shadcn/ui components (built on Radix UI + Tailwind), Tailwind CSS utility-first styling, and canvas-based v… |
| `ui-ux-pro-max` | UI/UX design intelligence for web and mobile. Includes 50+ styles, 161 color palettes, 57 font pairings, 161 product types, 99 UX guidelines, and 25 chart ty… |
| `watch` | Watch a video (URL or local path). Downloads with yt-dlp, extracts auto-scaled frames with ffmpeg, pulls the transcript from captions (or Whisper API fallbac… |
| `web-artifacts-builder` | Suite of tools for creating elaborate, multi-component claude.ai HTML artifacts using modern frontend web technologies (React, Tailwind CSS, shadcn/ui). Use… |
| `webapp-testing` | Toolkit for interacting with and testing local web applications using Playwright. Supports verifying frontend functionality, debugging UI behavior, capturing… |
| `worktree` | Create, enter, switch between, and clean up git worktrees so work happens on an isolated branch/checkout without touching the main one. Use when the user say… |

## Notes

- Skills that include a `LICENSE.txt` keep their original license (mostly Apache 2.0 from Anthropic's public skills).
- `watch` comes from [bradautomates/claude-video](https://github.com/bradautomates/claude-video) (MIT).
- Not included: Anthropic's `pdf` skill and claude.ai-synced skills, which may not be redistributed.
