---
name: photo-to-gouache
description: This skill should be used when the user uploads a photo and asks to convert it into a gouache painting / storybook picture-book illustration style, while preserving the original photo's mood and atmosphere.
agent_created: true
---

# photo-to-gouache

## Overview

Convert user-uploaded photos into a **gouache storybook illustration** style. The style is learned from the reference image (bold color blocks, visible brush strokes, simplified shapes) but is **decoupled from the reference's specific scene**. The skill preserves the original photo's mood, lighting, and atmosphere — it only changes the painting treatment.

**This skill is interactive.** After the user uploads a photo, you MUST let the user choose an output size, then ask for confirmation, BEFORE calling ImageGen. Do not generate immediately.

## When to use

Trigger this skill when the user:
- Uploads an image and asks to "turn into illustration", "转成插画", "水粉风", "绘本风", "storybook style", "gouache style", etc.
- Wants a photo converted into the painting style shown in the reference images stored in `assets/refs/`.

## Workflow

### Step 0 — Receive the photo and read its dimensions
1. Get the uploaded photo path from the conversation or the user's explicit input.
2. Read the image's pixel dimensions so you can offer a "match original ratio" size later.
3. Briefly note the photo's mood/atmosphere (e.g., sunny, dusk, indoor, rainy) so you can append it to the prompt if needed.

### Step 0.5 — Detect multiple photos (batch mode)
If the user uploaded **two or more** photos in one message:
- Treat it as a **batch**. Let the user pick ONE shared output size via `AskUserQuestion` (same options as Step 1). Also offer:
  - `统一用原图比例` (recommended) — each photo keeps its own ratio, no cropping
  - `统一用 3:4 竖图` → `1024x1536`
  - `统一用 1:1 方形` → `1024x1024`
  - `统一用 4:3 横图` → `1536x1024`
  - `分别单独选择` — fall back to per-image Step 1 for each photo
- In the confirmation (Step 2), restate that N images will be generated and the total credit cost (≈5–10 credits × N).
- When generating, process photos **one at a time (serial)**, NOT in parallel. Generate the next only after the previous finishes. This avoids the ImageGen file-name collision bug (same-second names overwrite each other).
- **Make each photo's prompt unique**: prefix the preset prompt with one sentence describing that photo's subject (e.g., "A city greening worker..."). This both preserves mood and guarantees distinct output filenames.
- If ImageGen returns `RequestLimitExceeded.JobNumExceed` (task limit reached), wait ~30s and retry that photo; do not abort the batch.

### Step 1 — Let the user pick the output size (AskUserQuestion, single-photo mode)
Use the `AskUserQuestion` tool with one question:

- **Question — Size (`header: "尺寸"`, `multiSelect: false`)**
  - `原图比例` (recommended) — keep the photo's aspect ratio, no cropping. Compute the closest supported size:
    - square/near-square → `1024x1024`
    - portrait (taller) → `1024x1536`
    - landscape (wider) → `1536x1024`
  - `1:1 方形` → `1024x1024`
  - `3:4 竖图` → `1024x1536`
  - `4:3 横图` → `1536x1024`
  - Description per option: dimensions + use case.

### Step 2 — Confirm before generating
Summarize the plan in a short message AND ask for confirmation. Use `AskUserQuestion` with one question (`header: "确认"`, `multiSelect: false`):
- Options: `生成` (go ahead) / `重新选` (change size).
- In the confirmation message, restate: fixed style (水粉绘本插画), chosen size, the photo path, and the credit cost (≈5–10 credits per image).

Only call ImageGen after the user picks `生成`.

### Step 3 — Generate
Call `ImageGen` with image-to-image mode:
- **Single photo** (Step 1 path): one `ImageGen` call with `image` = the user's photo.
- **Batch** (Step 0.5 path): loop over photos serially. For each photo, prefix the preset prompt with one sentence naming its subject (e.g., "A young couple crossing an urban street..."), then call `ImageGen` with `image` = that photo. Wait for each result before the next.
- `image`: the user's photo path
- `prompt`: the single preset prompt from `references/style-presets.md`, **optionally appending a one-sentence mood/scene description** if the photo has a strong atmosphere (e.g., "a quiet dusk road lined with trees, a person standing with arms outstretched"). Do NOT add Mediterranean architectural elements.
- `input_fidelity`: `high` to keep the original composition and subjects
- `quality`: `high`
- `size`: the confirmed size from Step 1

### Step 4 — Return the result
Return the generated image path to the user. If the output looks too realistic, append `no photorealistic details, painted texture only` on a retry; if it strays from the original, re-run with `input_fidelity: high` and the original size.

## ImageGen parameters

| Parameter | Recommended value | Why |
|-----------|-------------------|-----|
| `image` | user's photo path | Required to trigger image-to-image editing. |
| `prompt` | `references/style-presets.md` preset | Encodes the transferable gouache storybook style. |
| `input_fidelity` | `high` | Keeps composition and main subjects intact while changing style. |
| `quality` | `high` | Better texture and color fidelity. |
| `size` | chosen in Step 1 | User-controlled; "原图比例" prevents unwanted cropping. |

## Style preset

The only preset is `gouache-storybook`. Load `references/style-presets.md` for the full prompt and style analysis.

## Style references

Reference images stored in `assets/refs/` show the target illustration style. Use them for visual guidance when refining prompts, but note that `ImageGen` treats every image in the `image` array as an input to be edited. The current skill relies on prompt-based style transfer rather than multi-image style reference, because prompt-level control better preserves the user's original mood.

## Credits reminder

ImageGen consumes roughly 5–10 credits per image. Always state the cost in the Step 2 confirmation so the user knows before committing. Inform the user before generating multiple variants.
