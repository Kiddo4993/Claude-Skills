---
name: accessibility
description: Audit UI code for WCAG 2.2 AA accessibility violations and output fixed code with explanations
trigger: manual
---

# Accessibility Audit Skill

Perform a WCAG 2.2 Level AA accessibility audit on the provided code. Fix every violation found.

## What to Check

### Perceivable
- **Color contrast**: Body text ≥ 4.5:1, large text (18pt+ or 14pt bold+) ≥ 3:1, UI components ≥ 3:1
- **Non-text content**: All `<img>`, `<svg>`, `<canvas>` have meaningful `alt` or `aria-label`, or `alt=""` if decorative
- **Info not conveyed by color alone**: Don't use only red/green to indicate error/success — add icons or text
- **Captions/transcripts**: Video/audio content needs captions

### Operable
- **Keyboard navigation**: Every interactive element reachable by Tab, activated by Enter/Space
- **Focus management**: Visible focus ring on all focusable elements (never `outline: none` without replacement)
- **No keyboard traps**: User can always Tab out of modals/dropdowns
- **Skip navigation**: Long pages need a "Skip to main content" link
- **Target size**: Interactive elements ≥ 44×44 CSS pixels

### Understandable
- **Language declared**: `<html lang="en">` (or appropriate language code)
- **Labels on inputs**: Every `<input>`, `<select>`, `<textarea>` has a `<label>` or `aria-label`
- **Error identification**: Errors clearly identify which field and what to fix
- **Autocomplete on personal data fields**: `autocomplete` attribute set appropriately

### Robust
- **Valid ARIA**: Roles, states, properties used correctly; no invalid combos
- **Name, Role, Value**: Custom widgets expose role/state to assistive tech
- **No ARIA on non-interactive elements**: Don't add `role="button"` to a `<div>` — use `<button>`

## Output Format
For each violation found:
```
❌ [WCAG criterion, e.g. 1.4.3 Contrast]
   Problem: <what's wrong>
   Before: <offending code>
   After: <fixed code>
   Why: <one-sentence explanation>
```

Then provide the complete fixed file at the end.
