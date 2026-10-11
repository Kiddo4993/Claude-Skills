---
name: replicate-media
description: Run AI image/video/audio models from Next.js with the Replicate SDK (replicate npm package). Use when generating or transforming media via Replicate, handling predictions, webhooks, or streaming output.
---

# Replicate media generation

Package: `replicate` (^1.4). Server-side only; keep `REPLICATE_API_TOKEN` out of client code.

```ts
import Replicate from "replicate";
const replicate = new Replicate({ auth: process.env.REPLICATE_API_TOKEN });

// Simple: waits for completion
const output = await replicate.run("owner/model:version", { input: { prompt } });

// Long jobs: create + poll/webhook
const prediction = await replicate.predictions.create({
  model: "owner/model",
  input: { prompt },
  webhook: `${process.env.APP_URL}/api/replicate/webhook`,
  webhook_events_filter: ["completed"],
});
```

## Guidelines
- Call from a Next.js Route Handler (`app/api/.../route.ts`) or Server Action, never the browser.
- Output is a URL (or array of URLs / `FileOutput` streams). Persist files you need; Replicate URLs expire.
- For anything over a few seconds use `predictions.create` + webhook or polling (`replicate.wait`) to avoid serverless timeouts (see `vercel.json` maxDuration).
- Validate and length-limit user prompts; handle `prediction.status === "failed"` and surface `prediction.error`.
- Pair with `@anthropic-ai/sdk` (see the `claude-api` skill) when Claude writes the prompt and Replicate renders it.
