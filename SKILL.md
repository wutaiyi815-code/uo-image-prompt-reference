---
name: uo-image-prompt-reference
description: Explicit-only reference for writing single-step apparel model image-generation prompts and assigning source images. Use only when the user explicitly invokes `$uo-image-prompt-reference` or explicitly asks to use this prompt reference skill.
---

# UO Image Prompt Reference

Use this skill only when the user explicitly names `$uo-image-prompt-reference` or explicitly requests this prompt reference. Do not invoke it implicitly, do not treat related apparel work as permission to load it, and do not make another skill depend on it by default.

Read [references/prompt-catalog.md](references/prompt-catalog.md) whenever this skill is explicitly invoked.

## Operating Rules

- Treat each catalog entry as one atomic image-generation task.
- Follow the user's instructions for combining or ordering multiple single-step tasks. Do not infer a multi-step sequence from this skill alone.
- Visually inspect the supplied images before selecting an entry or filling variables.
- Preserve the reference-image order defined by the selected entry; Figure 1, Figure 2, and later figures are positional contracts.
- Adapt nouns, colors, view directions, pose details, and fit terms to visible evidence. Do not copy example-specific attributes into an unrelated task.
- Keep the final API prompt task-focused. Do not add SKU identifiers, file paths, API parameters, process notes, or generic restrictions unrelated to the requested edit.
- If no entry fits, say that the catalog has no direct match and write a concise single-step prompt from the same principles only when the user asks you to do so.
