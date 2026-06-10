---
version: 0.1.0
name: ecomrads-static-ads
description: |
  Create static ad creatives via the eComrads MCP tools. `static_product_ad`
  produces static / carousel ad slides (with headline & CTA treatment chosen by
  the art director); `recreate_similar_ad` recreates a competitor-style ad using
  your product + a reference ad.
  Use when: "make an ad", "static ad", "ad creative", "carousel ad", "slides",
  "Meta/Facebook/Instagram ad", "promo graphic", "recreate this ad", "ad like
  this competitor", "model this winning ad with my product".
  NOT for: plain product photos (use ecomrads-product-photoshoot), video ads
  (use ecomrads-product-video / ecomrads-ugc-video), analyzing an ad (use
  ecomrads-virality-analysis), finding competitor ads (use
  ecomrads-competitor-spy).
argument-hint: "[--recreate] [--slides N] [brief]"
allowed-tools: upload, static_product_ad, recreate_similar_ad, check_generation
---

# Static Ads

Two tools:

- **`static_product_ad`** (`POST /post/static`) — static / carousel ad creative; the art director chooses headline & CTA treatment from your brief.
- **`recreate_similar_ad`** (`POST /post/genanalysis`) — recreate a competitor-style ad using your product and a reference ad.

Read [./AGENTS.md](./AGENTS.md) and [./references/prompting.md](./references/prompting.md) first. Pair with **ecomrads-competitor-spy** to source reference ads.

## Workflow

1. **Upload** the product image → `upload` → `product_image_url`.
2. Write a tight **creative brief** (offer, tone, angle, season, what to avoid). Treat ad copy intent as part of the brief.
3. Call the tool with defaults below.
4. **Poll `check_generation`** until terminal.
5. Deliver the slide URLs.

## `static_product_ad` — ad creative

Fields map to `StaticAdRequest`:

```json
{
  "body": {
    "product_image_url": "<url from upload>",
    "instructions": "Black Friday angle, quiet-luxury tone, emphasize 40% off, avoid clutter",
    "ad_format": "4:5",
    "num_slides": 3,
    "resolution": "2K"
  }
}
```

- **`product_image_url`** (required) — link from `upload`.
- **`ad_format`** (required) — one of `1:1`, `9:16`, `4:5`, `3:4`, `16:9`. Pick by placement (`4:5`/`1:1` feed, `9:16` story, `16:9` web).
- **`instructions`** — optional creative brief. When omitted, the art director runs autonomous. CTA wording/treatment is chosen by the LLM unless you specify an exact phrase in `instructions`.
- **`num_slides`** — 1–5 (default 1). The visual system is locked across slides automatically.
- **`resolution`** — `1K`, `2K`, `4K` (default `2K`). **`model`** — leave default (`nano-banana-pro`).

## `recreate_similar_ad` — model a winning ad

Fields map to `GenAnalysisRequest`:

```json
{
  "body": {
    "product_image_url": "<your product url from upload>",
    "ad_media_url": "<reference ad image/video url>",
    "media_type": "image"
  }
}
```

- **`product_image_url`** (required) — your product (from `upload`).
- **`ad_media_url`** (required) — the reference ad (often from **ecomrads-competitor-spy** results, or a link the user gives).
- **`media_type`** — `image` (default) or `video`.

## Pre-generation interview

At most 3–4 labeled questions, skipping the obvious:

- Path? `[New ad from my product / Recreate a specific ad]`
- Placement / format? `[1:1 / 4:5 / 9:16 / 16:9]`
- How many slides? `[1 / 3 / 5]`
- The offer / hook? (one line)

## Delivering

```
Your 3-slide ad creative is ready (4:5):
- <url>
- <url>
- <url>
```

## What this skill does NOT do

- No plain product photography (use **ecomrads-product-photoshoot**), no video (use **ecomrads-product-video** / **ecomrads-ugc-video**), no ad analysis (use **ecomrads-virality-analysis**), no Meta Library search (use **ecomrads-competitor-spy**).

## Common mistakes

- Omitting the required `ad_format`.
- Requesting more than 5 slides (max is 5).
- For `recreate_similar_ad`, forgetting the reference `ad_media_url`.
- Hard-coding CTA copy when the art director would do better — only force a phrase when the user demands exact wording.
