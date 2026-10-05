# Emoji → SVG Icon Skill

Replaces Unicode emoji characters used as UI icons with clean, consistent SVG icons from **lucide-react** (or hand-crafted SVG if the project doesn't use lucide). The result looks hand-designed, brand-consistent, and renders identically across all platforms and OS themes.

## When to Invoke

- User says "emojis look AI", "icons look generic", "replace emoji with icons", "make icons more aesthetic"
- You see `<div>🎓</div>` / `<span>⚡</span>` / `{item.icon}` rendering an emoji string in JSX
- String data files (JSON/JS) store emoji characters as icon values

## Why Emojis Fail as UI Icons

| Problem | Effect |
|---------|--------|
| Font-rendered glyphs | Look different on Windows vs macOS vs Android |
| Fixed color | Cannot be themed or recolored with CSS |
| No stroke control | Cannot match surrounding icon stroke weight |
| Platform-dependent style | iOS renders a rainbow heart, Windows renders flat shape |
| Accessibility | Screen readers announce full emoji name, not icon intent |

## Step-by-Step Process

### Step 1 — Audit: Find all visual emoji usage

Search only JSX/TSX/JS/TS files. Ignore emoji inside `alert()`, `console.log()`, and markdown/comment strings — those don't render as UI.

```bash
grep -rn --include="*.jsx" --include="*.tsx" --include="*.js" --include="*.ts" \
  -P "[\x{1F000}-\x{1FFFF}]|[\x{2600}-\x{27FF}]|[\x{2300}-\x{23FF}]" src/
```

Separate into two lists:
- **Data emojis**: emoji stored as string values in objects/arrays that get rendered via `{item.icon}` — these need a name-based system
- **Inline emojis**: emoji hardcoded directly in JSX — these get replaced with component calls

### Step 2 — Create BrandIcon component

If lucide-react is installed (`grep -r lucide package.json`), use it. Otherwise hand-craft SVGs.

**lucide-react version** — create `src/components/BrandIcon.jsx` (or `.tsx`):

```jsx
import {
  GraduationCap, Zap, Star, Trophy, Handshake, Heart, Globe,
  Eye, Gem, Target, FileText, CheckCircle, Clock, Mail, BarChart2,
  BookOpen, Video, HelpCircle, Lock, LogOut, Sun, Moon, User,
  Send, Award, Briefcase, Inbox, CheckSquare, Calendar, Users,
  // add others as needed
} from "lucide-react";

const ICON_MAP = {
  graduation: GraduationCap,
  lightning:  Zap,
  star:       Star,
  trophy:     Trophy,
  handshake:  Handshake,
  community:  Users,
  heart:      Heart,
  globe:      Globe,
  eye:        Eye,
  gem:        Gem,
  target:     Target,
  memo:       FileText,
  document:   FileText,
  check:      CheckCircle,
  done:       CheckSquare,
  clock:      Clock,
  email:      Mail,
  chart:      BarChart2,
  book:       BookOpen,
  video:      Video,
  question:   HelpCircle,
  lock:       Lock,
  logout:     LogOut,
  sun:        Sun,
  moon:       Moon,
  user:       User,
  send:       Send,
  award:      Award,
  briefcase:  Briefcase,
  inbox:      Inbox,
  calendar:   Calendar,
};

export default function BrandIcon({ name, size = 20, className = "", strokeWidth = 1.5, style }) {
  const Icon = ICON_MAP[name];
  if (!Icon) return null;
  return <Icon size={size} strokeWidth={strokeWidth} className={className} style={style} aria-hidden="true" />;
}
```

**Hand-crafted SVG version** (when no icon library) — write each icon as a named export:

```jsx
const ICON_MAP = {
  graduation: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round">
      <path d="M22 10v6M2 10l10-5 10 5-10 5-10-5z"/>
      <path d="M6 12v5c3.333 1.333 8.667 1.333 12 0v-5"/>
    </svg>
  ),
  lightning: (
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.5" strokeLinecap="round" strokeLinejoin="round">
      <polyline points="13 2 13 9 21 9 11 22 11 15 3 15 13 2"/>
    </svg>
  ),
  // ... add others following this pattern
};

export default function BrandIcon({ name, size = 20, className = "", style }) {
  const icon = ICON_MAP[name];
  if (!icon) return null;
  return (
    <span
      role="img"
      aria-hidden="true"
      className={className}
      style={{ display: "inline-flex", width: size, height: size, flexShrink: 0, ...style }}
    >
      {icon}
    </span>
  );
}
```

### Step 3 — Update data files

In data files (JS/JSON) where emojis are icon values, replace with semantic string names:

```js
// Before
{ icon: "🎓", title: "Our Mission" }

// After
{ icon: "graduation", title: "Our Mission" }
```

**Standard emoji → name mapping:**
| Emoji | Name | Lucide Icon |
|-------|------|-------------|
| 🎓 | `graduation` | GraduationCap |
| ⚡ | `lightning` | Zap |
| 🌟 | `star` | Star |
| 🏆 | `trophy` | Trophy |
| 🤝 | `handshake` | Handshake |
| 💝/❤️ | `heart` | Heart |
| 🌍/🌐 | `globe` | Globe |
| 👁️ | `eye` | Eye |
| 💎 | `gem` | Gem |
| 🎯 | `target` | Target |
| 📝 | `memo` | FileText |
| ✅ | `check` | CheckCircle |
| ⏱️/🕐 | `clock` | Clock |
| 📧 | `email` | Mail |
| 📊 | `chart` | BarChart2 |
| 📖/📚 | `book` | BookOpen |
| 🎥 | `video` | Video |
| ❓ | `question` | HelpCircle |
| 🔐/🔒 | `lock` | Lock |
| 🚪 | `logout` | LogOut |
| ☀️ | `sun` | Sun |
| 🌙/🌓 | `moon` | Moon |
| 👤 | `user` | User |
| 👥 | `community` | Users |
| 📨/📤 | `send` | Send |
| 🏅/🥇 | `award` | Award |
| 💼 | `briefcase` | Briefcase |
| 📥 | `inbox` | Inbox |
| 📄 | `document` | FileText |
| 🗓️ | `calendar` | Calendar |

### Step 4 — Replace rendering locations

**Data-driven icon (was `{item.icon}`):**
```jsx
// Before
<div className="card-icon">{item.icon}</div>

// After
<BrandIcon name={item.icon} size={24} />
```

**Inline emoji in JSX:**
```jsx
// Before
<div className="stat-icon">📝</div>

// After
<div className="stat-icon"><BrandIcon name="memo" size={22} /></div>
```

**Inline emoji mixed into text (heading, button):**
```jsx
// Before
<h1>⚡ Welcome, {user.name}!</h1>

// After — use inline-flex with vertical alignment
<h1>
  <BrandIcon name="lightning" size={22} style={{ display: "inline", verticalAlign: "middle", marginRight: "0.4rem" }} />
  Welcome, {user.name}!
</h1>
```

**Numbered step emojis (1️⃣ 2️⃣ 3️⃣):**
Replace with styled index badges — remove from data, use array `index` instead:
```jsx
{steps.map(({ text }, idx) => (
  <div key={idx} style={{ display: "flex", gap: "0.75rem", alignItems: "flex-start" }}>
    <span style={{
      display: "inline-flex", alignItems: "center", justifyContent: "center",
      width: 22, height: 22, minWidth: 22, borderRadius: "50%",
      background: "rgba(99,102,241,0.15)", border: "1px solid rgba(99,102,241,0.35)",
      color: "#818cf8", fontSize: "0.7rem", fontWeight: 700, flexShrink: 0
    }}>
      {idx + 1}
    </span>
    <span>{text}</span>
  </div>
))}
```

### Step 5 — Cleanup

- Remove emoji from `alert()`, `console.log()` strings (optional — these aren't UI)
- Remove emoji from dropdown `<option>` text labels — clean labels read better
- Remove emoji prefixes from card titles (`📝 Your Requests` → `Your Requests`) — the heading communicates itself
- Leave emoji in actual user-generated content (chat messages, comments) — those are intentional

### Step 6 — Verify

```bash
# Check no visual emoji remain in JSX render positions
grep -rn --include="*.jsx" --include="*.tsx" \
  -P "className=\"(card-icon|stat-icon|empty-state-icon|banner-icon)\">[^<]*[\x{1F000}-\x{1FFFF}]" src/
```

## Icon Sizing Guidelines

| Context | Size | strokeWidth |
|---------|------|-------------|
| Card corner accent | 18px | 1.5 |
| Stat card icon | 22px | 1.5 |
| Section / heading inline | 20–22px | 1.5 |
| Popup / modal hero | 32–40px | 1.2 |
| Empty state | 28–32px | 1.2 |
| Button inline | 14–16px | 1.5 |

## Anti-Patterns to Avoid

- Do not use `font-size: 2rem` on an emoji inside a div — replace it, don't style it
- Do not mix emoji and lucide icons at the same hierarchy level
- Do not use `<img src="emoji.png">` — defeats the purpose of SVG
- Do not inline raw SVG paths in every file — use the BrandIcon wrapper
- Do not use `aria-label` on `aria-hidden` icons — they're already hidden from screen readers

## Notes on `about/page.js` Server Components

Next.js server components can import client components. `BrandIcon` (no hooks, no browser APIs) works in both server and client components without needing `"use client"`.
