---
version: 0.2.0
name: ecomrads-storyboard
description: |
  Generate a multi-angle product storyboard via the eComrads MCP `multi_angles`
  tool — a 9-shot set of consistent angle views from one product image.
  Use when: "storyboard", "multi-angle", "multiple angles of my product", "9
  shots", "turn my product into different angles", "angle pack", "shot
  sequence", "product turntable set", "show it from every side".
  NOT for: animating a still into video (use ecomrads-product-video), UGC
  creator video (use ecomrads-ugc-video), still photoshoots (use
  ecomrads-product-photoshoot), static ad slides (use ecomrads-static-ads),
  analyzing an ad (use ecomrads-virality-analysis).
argument-hint: "[--aspect 4:5] [--resolution 2K] [product image]"
allowed-tools: upload, multi_angles, check_generation
---

# Storyboard (multi-angle, 9 shots)

Generate a **9-shot angle storyboard** from a product image via the eComrads MCP **`multi_angles`** tool (`POST /post/multi-angles`).

Read [../AGENTS.md](../AGENTS.md) and [../references/prompting.md](../references/prompting.md) first.

> Want a single animated clip from one still? Use **[ecomrads-product-video](../ecomrads-product-video)**. Want a creator/presenter video? Use **[ecomrads-ugc-video](../ecomrads-ugc-video)**.

## Workflow

1. **Upload** the product image → `upload` → 1–2 links (see [../references/upload.md](../references/upload.md)).
2. **`multi_angles`** → returns a `job_id`. **Poll `check_generation`** until `completed`.
3. **Deliver** the 9 storyboard image URLs.

## Call — `multi_angles`

Fields map to `MultiAnglesRequest`:

```json
{
  "body": {
    "image_urls": ["<url from upload>"],
    "aspect_ratio": "4:5",
    "resolution": "2K",
    "prompt": "premium matte skincare bottle, consistent studio lighting across angles"
  }
}
```

- **`image_urls`** (required) — **1–2** links from `upload`.
- **`aspect_ratio`** (required) — one of `1:1, 3:2, 3:4, 4:3, 4:5, 5:4, 9:16, 16:9`.
- **`resolution`** — `1K`, `2K` (recommended), `4K`. Default `1K`.
- **`prompt`** — optional direction to keep the 9 angles consistent (lighting, mood, surface).
- **`model`** — leave default (`nano-banana-pro`); valid: `nano-banana-2`, `nano-banana-pro`, `gpt-image-2`.

Produces **9 shots** in a single job.

## Pre-generation interview

At most 1–2 labeled questions, skipping the obvious:

1. Aspect? `[1:1 / 4:5 / 9:16 / 16:9]`
2. Resolution? `[1K / 2K / 4K]`

## Delivering

```
9-shot storyboard ready:
- <url> … (×9)
```

Embed the images if the client supports it.

## What this skill does NOT do

- No animation / video → **ecomrads-product-video** (single still) or **ecomrads-ugc-video** (presenter).
- No still photoshoot → **ecomrads-product-photoshoot**.
- No static ad slides → **ecomrads-static-ads**.
- No analysis → **ecomrads-virality-analysis**.

## Common mistakes

- Passing `image_urls` with more than 2 entries (max 2).
- Using an invalid `aspect_ratio` (must be one of the eight listed).
- Treating this as a video tool — it outputs **9 still angle shots**, not motion.
