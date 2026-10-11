---
name: markdown-math-rendering
description: Render Markdown with GFM tables, LaTeX math (KaTeX) and syntax-highlighted code in React using react-markdown, remark-gfm, remark-math, rehype-katex, rehype-raw and react-syntax-highlighter. Use for lesson content, AI chat output, or any rich-text display.
---

# Markdown + math + code in React

Packages: `react-markdown`, `remark-gfm`, `remark-math`, `rehype-katex`, `rehype-raw`, `katex`, `react-syntax-highlighter`.

```tsx
"use client";
import ReactMarkdown from "react-markdown";
import remarkGfm from "remark-gfm";
import remarkMath from "remark-math";
import rehypeKatex from "rehype-katex";
import { Prism as SyntaxHighlighter } from "react-syntax-highlighter";
import { oneDark } from "react-syntax-highlighter/dist/esm/styles/prism";
import "katex/dist/katex.min.css"; // required or math renders unstyled

export function Md({ children }: { children: string }) {
  return (
    <ReactMarkdown
      remarkPlugins={[remarkGfm, remarkMath]}
      rehypePlugins={[rehypeKatex]}
      components={{
        code({ className, children, ...props }) {
          const lang = /language-(\w+)/.exec(className || "")?.[1];
          return lang
            ? <SyntaxHighlighter style={oneDark} language={lang} PreTag="div">{String(children).replace(/\n$/, "")}</SyntaxHighlighter>
            : <code className={className} {...props}>{children}</code>;
        },
      }}
    >{children}</ReactMarkdown>
  );
}
```

## Notes
- Math: `$inline$` and `$$block$$`. LLMs often emit `\( \)` / `\[ \]`; convert to `$` / `$$` before rendering.
- `rehype-raw` allows raw HTML in markdown: only add it for trusted content (or pair with `rehype-sanitize`), since it opens XSS for user/AI text.
- Render streamed AI output by re-rendering with the growing string; memoize the component.
- Import highlighter styles from `/dist/esm/styles/prism` to keep bundles tree-shakeable.
