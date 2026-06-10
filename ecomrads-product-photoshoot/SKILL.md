---
version: 0.1.0
name: ecomrads-product-photoshoot
description: |
  Generate brand-quality product images and edits via the eComrads MCP
  `edit_product_photo` tool. Entry point for professional product/brand visuals.
  Use when: "product photo", "studio shot", "lifestyle image", "Pinterest pin",
  "hero/banner", "carousel", "ad image", "virtual try-on", "model wearing",
  "person holding product", "closeup with hands", "levitating/floating/splash
  product", "CGI/surreal product", "restyle", "seasonal/aesthetic variation",
  "relight", "swap background", or any product or brand still image.
  Modes: product_shot, lifestyle_scene, closeup_with_person, moodboard_pin,
  hero_banner, conceptual_product, virtual_model_tryout, restyle.
  NOT for: product motion video (use ecomrads-product-video), UGC creator
  video (use ecomrads-ugc-video), static multi-slide ad layouts with copy/CTA
  (use ecomrads-static-ads), analyzing a finished ad (use
  ecomrads-virality-analysis), competitor research (use ecomrads-competitor-spy).
argument-hint: "[--mode] [--count N] [prompt]"
allowed-tools: upload, edit_product_photo, check_generation
---

# Product Photoshoot

Brand-image generation via the eComrads **`edit_product_photo`** tool (`POST /post/edit`). Provide the product image(s) and a strong prompt; the backend runs a Visual-System-informed refine and returns image URLs via a job.

Read [./AGENTS.md](./AGENTS.md) (behavior) and [./references/prompting.md](./references/prompting.md) (prompt craft) first.

## Workflow

1. **Upload** the product image → `upload` → get a public link (see [./references/upload.md](./references/upload.md)). Skip if the user gave a public URL or wants pure text-to-image.
2. **Pick a mode** (table below) from intent, not surface keywords.
3. **Write a Master Prompt** — expand the user's ask into the 6-layer brief.
4. **Call `edit_product_photo`** with the link(s) + prompt + sensible defaults.
5. **Poll `check_generation`** until terminal (see [./references/jobs.md](./references/jobs.md)).
6. **Deliver** the image URLs; offer one refinement.

## Modes

`edit_product_photo` has no literal `mode` field — the mode shapes **how you write the prompt** and which `aspect_ratio` you pick.

| Mode | When the user wants… | Default aspect |
|------|----------------------|----------------|
| `product_shot` | Product on neutral / studio / catalog background | `1:1` |
| `lifestyle_scene` | Product in a real environment — hands, action, atmosphere | `4:5` |
| `closeup_with_person` | Tight crop with hands / partial face — beauty, demonstrating | `4:5` |
| `moodboard_pin` | Vertical Pinterest-native pin, moodboard feel | `3:4` |
| `hero_banner` | Wide website / email / campaign header | `16:9` |
| `conceptual_product` | Surreal / CGI / levitating / splash / sculptural | `1:1` |
| `virtual_model_tryout` | Product worn/used by a model (pass the person as `model_image_url`) | `4:5` |
| `restyle` | Transform an existing image's aesthetic / mood / season | match source |

Tie-breakers: Pinterest wins → `moodboard_pin`; banner format wins → `hero_banner`; person applying/holding → `closeup_with_person`.

## Pre-generation interview

Ask **at most 3–4** short, labeled questions, and skip any whose answer is obvious from context.

- **Uploaded a product photo, vague ask:** How many? `[1 / 3 / 5]` · Style? `[Clean studio / Lifestyle / Conceptual / With a model]` · Where used? `[Shopify / Instagram / Pinterest / Paid ads / Website hero]`
- **Named a use case** (e.g. "hero banner"): mode is obvious — ask only the gaps (offer/mood, what to emphasize).
- **Text only, no photo:** ask them to upload a product photo (much higher fidelity); if none, get category, packaging, color, distinctive features.
- **"Redo / change vibe":** → `restyle` — ask target aesthetic + seasonal context.

## Generation

MCP call shape — fields map to `PostEditRequest`:

```json
{
  "body": {
    "image_urls": ["<url from upload>"],
    "prompt": "<6-layer Master Prompt>",
    "aspect_ratio": "4:5",
    "resolution": "2K"
  }
}
```

- **`prompt`** (required) — your Master Prompt. For text-to-image, omit `image_urls` (or pass `[]`).
- **`image_urls`** — 0–10 product/reference links from `upload`.
- **`aspect_ratio`** — one of `1:1, 3:2, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9` (default `1:1`). Pick by mode/intent.
- **`resolution`** — `1K`, `2K`, `4K` (default `1K`; prefer **`2K`** for product work).
- **`model_image_url`** — for `virtual_model_tryout`: the person to dress; `image_urls` then act as garments.
- **`model`** — leave default for best quality; valid: `nano-banana-2`, `nano-banana-pro`, `gpt-image-2`.
- **`style`** — optional style hint. Leave `auto_refine_prompt` at its default (true).

## Multi-variant

There's no `count` field — to produce N distinct images, call `edit_product_photo` N times, varying the prompt's lighting / angle / palette each time (don't paraphrase the same prompt). Poll all jobs, then deliver the set together.

## Delivering

Short bulleted list of image URLs. No JSON, no IDs, no internal model names, no enhanced-prompt text.

```
3 lifestyle shots ready:
- <url>
- <url>
- <url>
```

## What this skill does NOT do

- No product motion video — use **ecomrads-product-video**.
- No UGC creator video — use **ecomrads-ugc-video**.
- No multi-slide ad layouts with headline/CTA copy — use **ecomrads-static-ads**.
- No ad analysis — use **ecomrads-virality-analysis**.

## Common mistakes

- Sending a one-line prompt instead of a 6-layer brief.
- Asking more than ~4 questions before generating.
- Re-uploading the same image per call instead of reusing the link.
- Picking the wrong aspect ratio for the destination (e.g. `1:1` for a website hero).
- Pasting the prompt back to the user — they want the URLs.
