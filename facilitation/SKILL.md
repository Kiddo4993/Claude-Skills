---
name: facilitation
description: Surveys every skill and plugin available in this Claude Code install and picks whichever one — or combination of several, run together or in sequence — best builds a stated final product. Use when the user describes what they want to end up with (a deliverable, a feature, a file, an app) and it's unclear which of the many installed skills/plugins/agents should be used, or when the user explicitly asks "which skill should I use for X", "what's the right tool for this", or "recommend a skill/plugin for this".
---

# facilitation

Acts as a router: given a description of the final product the user wants, decide which installed skill(s) are the best fit — a single skill, or a combination working together — explain why in one or two sentences, and hand off to it. A combination is not a fallback for when no single skill is a clean match; it's the right answer whenever the product genuinely has multiple parts (e.g. a design pass plus a review pass, or a doc skill plus a deployment skill). Never force a multi-part deliverable into one skill just to keep the answer simple, and never chain skills together when one already covers the whole thing.

## When to use this

- The user describes a deliverable ("I need X") and there are multiple candidate skills/plugins that could plausibly apply.
- The user directly asks which skill/tool to use.
- Before kicking off a nontrivial build where picking the wrong skill would mean redone work (e.g. picking between `frontend-design`, `ui-theme-designer`, `component`, and `web-artifacts-builder` for a UI task).

Skip this skill for simple, unambiguous requests — if there's one obvious skill and no real ambiguity, just use it directly instead of routing through here.

## Steps

1. **Restate the final product** in one line: what artifact/outcome does the user want (a file type, a running app, a document, a design, a review, etc.)? If it's genuinely unclear, ask one clarifying question before proceeding rather than guessing.

2. **Enumerate the candidates.** Pull from every source of installed capability, not just the always-visible skill list:
   - The skill listing already provided in context for this session (the `<system-reminder>` block enumerating available skills).
   - Installed plugins and their commands/agents/skills: `claude plugin list`, then inspect each plugin's manifest under `~/.claude/plugins/cache/**/.claude-plugin/plugin.json` and its `commands/`, `agents/`, `skills/` subfolders.
   - Custom skills-dir skills under `~/.claude/skills/*/SKILL.md`.
   - Do not limit the search to whichever skills happen to already be summarized in context if the task is high-stakes — read the actual `SKILL.md`/`plugin.json` `description` fields for close calls, since one-line summaries can be lossy.

3. **Decide single skill vs. combination.** Break the requested final product into its actual parts first (e.g. "a themed portfolio site with a deployed live link" = design/build + deploy; "a reviewed PR with a written summary doc" = review + docs). Then for each part, and for the whole:
   - Does one installed skill's stated purpose cover the *entire* deliverable end to end? If so, that's the answer — don't chain skills it doesn't need.
   - If the deliverable has genuinely separate parts, match each part to its best-fit skill independently, and note the order they'd run in (sequential handoff vs. independent/parallel).
   - Within each part, is the candidate the more specific tool for the domain, or a generic fallback? Prefer the specific one.
   - Does its stated purpose match the requested output format/medium exactly (e.g. `.pptx` deck vs `slides` artifact vs `docx`)?
   - Does it require credentials/setup the user doesn't have configured (e.g. an MCP connector needing an API key)? Note this as a caveat even if it's still the best fit.

4. **Report the recommendation before invoking anything**, in this shape:
   - Recommended skill(s), in the order they'd be used, with a one-line note on why each is there (which part of the product it covers).
   - If it's a combination: one line stating how they fit together (sequential steps vs. independent pieces of the same deliverable).
   - Any runner-up that was close, and why it lost.
   - Any setup/credential caveat.

5. **Hand off.** Once the user confirms (or if the choice was unambiguous and low-stakes), invoke the chosen skill(s) via the `Skill` tool — in sequence if the product needs them in order — rather than reimplementing their behavior inline.

## Notes

- This skill never fabricates a skill name — every recommendation must trace back to a real, currently-installed skill/plugin/agent found in step 2. If nothing installed actually fits, say so plainly and suggest what class of plugin (from the marketplace) would need to be installed instead.
- Keep the output short — a routing decision, not an essay. The user wants an answer, not a survey of the whole catalog.
