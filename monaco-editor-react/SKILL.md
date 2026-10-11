---
name: monaco-editor-react
description: Embed the Monaco (VS Code) code editor in React/Next.js with @monaco-editor/react. Use for coding exercises, in-browser editors, or diff views.
---

# Monaco editor in React

Package: `@monaco-editor/react`.

```tsx
"use client";
import Editor from "@monaco-editor/react";

<Editor
  height="400px"
  language="python"
  theme="vs-dark"
  value={code}
  onChange={(v) => setCode(v ?? "")}
  options={{ minimap: { enabled: false }, fontSize: 14, automaticLayout: true, scrollBeyondLastLine: false }}
/>
```

## Guidelines
- Client component only (`"use client"`); in Next.js lazy-load with `next/dynamic` if bundle size matters.
- By default it loads Monaco from a CDN; for offline/CSP use `loader.config({ monaco })` with the `monaco-editor` package.
- Use `onMount(editor, monaco)` to grab refs (e.g. `editor.getValue()`, add keybindings like Ctrl+Enter to run).
- `automaticLayout: true` handles resizable containers; give the parent an explicit height.
- Match theme to app dark mode (`vs` / `vs-dark`). `DiffEditor` is available for before/after comparisons.
- Running user code needs a sandbox (server runner or iframe/worker), never `eval` in the page.
