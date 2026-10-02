---
name: image-prompt-craft
description: Analyze a supplied image and produce a detailed, parameterized image-generation prompt with explicit visual measurements and controlled character or style edits.
metadata:
  short-description: Quantitative image analysis and controlled prompt rewriting
---

# Image Prompt Craft

Use this skill when a user supplies an image and wants a prompt that recreates it, or asks to replace a character or change a visual style while preserving the rest. Inspect the actual image before writing. If the image is unavailable, ask the user to attach it; do not invent its contents.

## Workflow

1. **Read the image as evidence.** Record what is visible, estimate what can be inferred, and mark what cannot be known from pixels alone. Infer the rendering medium from visible cues (for example, photographic skin and lens blur, 2.5D shading, 3D lighting, cel edges, or painterly strokes); do not apply a stock style label before inspecting the image.
2. **Build a compact quantitative visual specification.** Follow [the visual analysis and prompt specification](references/visual-analysis-and-prompt-spec.md). Use normalized image coordinates and bounded estimates. Separate direct measurements, image-space estimates, and uncertain physical inferences; never claim hidden EXIF, exact lens data, lighting setup, or unseen anatomy.
3. **Resolve the requested edit by attribute.** Make an explicit keep/change ledger before composing the prompt. Exact requested values and changes take priority. A general “keep everything else” or “only change the character” instruction locks every unrequested attribute. A broad style request changes only the smallest set of rendering attributes needed for that style; it does not authorize stock assumptions about pose, camera, proportions, costume construction, or framing. When a named style request explicitly includes a parameter (such as “85 mm”), change that parameter and preserve the rest. If two equally specific instructions conflict, briefly identify the conflict and ask which value to use.
4. **For a character replacement,** update only identity-linked appearance: face identity cues, hair shape/color, eye appearance, signature costume motifs or accessories, and their identifying palette. Keep the original garment cuts and construction, pose, gesture, body proportions, shot size, subject placement, camera angle/perspective, background, light, materials, and finish unless the user also asks to change them. Fit new signature elements into the existing costume structure rather than redesigning the outfit.
5. **Write a fluent Chinese prompt with inline mathematical parameters.** Respond in the user's language. The prompt must read as natural connected prose, with each important visual detail followed immediately by its parameter in compact mathematical notation (for example, `画幅 R=9:16`, `主体框 B=(x,y,w,h)=(...)`, `Pitch≈+8°`, `光比≈1.7:1`, `粗糙度 r∈[0.5,0.7]`). Do not turn the prompt into a table, a stack of `key=value` fields, or a long parameter header. Keep the parameter close to the detail it describes. The copy-ready prompt itself—not just an analysis before it—must contain the detailed applicable measurements, estimated ranges/confidence, and explicit unknowns. Follow [the visual analysis and prompt specification](references/visual-analysis-and-prompt-spec.md). End the same copy-ready prompt with a concise separate negative-constraint line. For edits, weave the requested change and preserved attributes naturally into the prompt rather than adding another parameter inventory.

## Quality rules

- Keep the observed medium, palette, framing, gesture, anatomy, lighting, material response, background, and post-processing close to the source unless the user names a change.
- Preserve relative relationships, not just labels: subject scale and center, horizon and vanishing lines, hand-to-face distance, silhouette, light-to-shadow balance, and focus falloff.
- Give ranges and confidence where exact values are not measurable. Do not turn low-confidence guesses into precise camera metadata or factual claims.
- Put quantitative values inside the fluent copy-ready prompt itself, in readable inline mathematical notation beside their descriptions. Do not deliver a richly parameterized analysis followed by an unparameterized prose prompt, and do not make the prompt look like a raw data dump.
- Avoid contradictory prompt clauses. If an exact requested change conflicts with a locked source value, follow the more specific request and disclose the one changed field in the ledger.
- Keep negative constraints tied to the source and the user's request: protect locked attributes and prevent likely generation failures without piling on generic exclusions.
