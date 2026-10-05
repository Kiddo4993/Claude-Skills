---
name: typography
description: Actively researches the live web (font foundries, Google Fonts, Fontshare, design roundups) to find and recommend a typography system genuinely fitting a specific website or project's brand and content — instead of defaulting to the same memorized "safe" pairings (Inter+Playfair, Montserrat+Merriweather) every time. Use when the user asks to pick, choose, research, or recommend fonts, typefaces, or a type pairing for a site/brand/project, or says an existing font choice feels generic/wrong.
---

# typography

The failure mode this skill exists to avoid: reaching for the same 5 memorized font pairings regardless of project. The point of "soaring the internet" is to actually look — current foundry catalogs, recent design writeups, and the project's own content — rather than recall.

## Process

1. **Read the actual project before searching.** Pull the brand personality from what's really there: `package.json`/README/site copy, existing CSS custom properties or design tokens, the tone of actual headline/body text, target audience, industry. If there's nothing to read yet, ask for 2–3 adjectives and any competitor/reference sites rather than guessing a generic "modern clean" brief.

2. **Write a short typographic brief** before searching: 2–4 adjectives grounded in what you read (e.g. "confident, editorial, slightly irreverent" vs. "quiet, technical, trustworthy"), plus hard requirements — language/character-set needs, weight range needed for the hierarchy planned, whether italics are needed, variable-font preference, self-hosting vs. Google Fonts CDN.

3. **Actually search, more than once, from different angles.** Use WebSearch/WebFetch — don't rely on training-data recall for what's "current." Useful angles:
   - `"[industry/vibe] font pairing 2026"` / recent design-blog roundups
   - the foundry catalogs directly: Google Fonts, Fontshare, and any other open catalogs relevant to the brief
   - searches aimed at *avoiding* the overused defaults: `"distinctive alternative to [overused font]"`, `"underrated variable fonts [year]"`
   - if a specific typeface looks promising, fetch its actual foundry/specimen page to confirm weight range, italics, license terms, and whether it's actually free for commercial use — don't assume from the name alone.

4. **Produce several real candidate pairings, not one.** For each: heading face + body face (+ mono if the project needs code/data display), with the license and hosting method confirmed, not assumed.

5. **Show it, don't just name it.** Build a quick visual comparison (an HTML/artifact mockup, or inline in the project if there's a live page to edit) rendering the *project's actual headline and body copy* in each candidate pairing side by side, at real UI sizes — not lorem ipsum, not oversized specimen type. A pairing that looks great at 72px display size can fall apart at 14px body text; check both.

6. **Recommend one**, with reasoning tied back to the brief (not just "it looks nice") — legibility at the sizes actually used, weight range covering the real hierarchy, license fit, and load-performance fit (variable font / subsettable / self-hostable if that matters for the project's stack).

7. **Hand off an implementation snippet** matching the project's actual setup: Google Fonts `<link>`/`@import`, a self-hosted `@font-face` block if self-hosting fits better, and CSS custom properties (`--font-heading`, `--font-body`) if the project already uses design tokens.

## Guardrails

- Don't recommend a typeface you haven't verified is actually available under the license the project needs (free commercial use, or the user has confirmed they'll license it). Fetch the license/pricing page — don't assume from a font's popularity.
- Don't stop at the first plausible pairing found. The brief in step 2 is the filter — a font can be beautiful and still be the wrong choice for this specific project.
- If the safest, most overused pairing genuinely *is* the best fit (e.g. the brand explicitly wants to look conventional/trustworthy over distinctive), it's fine to land there — but say that's a deliberate choice made after comparison, not the default because searching didn't happen.
