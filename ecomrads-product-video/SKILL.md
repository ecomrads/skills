---
version: 0.1.0
name: ecomrads-product-video
description: |
  Animate a product still into a short motion clip (image-to-video) via the
  eComrads MCP `product_video` tool. Bring a static product photo to life with
  camera moves and subtle motion — no presenter.
  Use when: "make a video from this image", "animate my product", "image-to-
  video", "product in motion", "add motion to this photo", "cinematic product
  clip", "spinning/rotating product", "camera push-in on product".
  NOT for: creator/presenter videos (use ecomrads-ugc-video), still images
  (use ecomrads-product-photoshoot), static ad slides (use ecomrads-static-ads),
  scoring a video (use ecomrads-virality-analysis).
argument-hint: "[--duration N] [--aspect 9:16|16:9|1:1] [motion prompt]"
allowed-tools: upload, product_video, check_generation
---

# Product Video (animate a still)

Turn a product photo into a short motion clip via the eComrads **`product_video`** tool (`POST /post/animate`). Image-to-video with camera moves and subtle motion — **no spoken presenter**.

Read [./AGENTS.md](./AGENTS.md) and [./references/prompting.md](./references/prompting.md) (esp. the **motion layer**) first.

> Want a creator/presenter talking about the product (testimonial, unboxing, review, how-to)? Use **[ecomrads-ugc-video](../ecomrads-ugc-video)** instead.

## Workflow

1. **Upload** the product image → `upload` → start-frame link (see [./references/upload.md](./references/upload.md)).
2. Write the **motion prompt** — camera move + subject motion + the single beat. One clear action per clip.
3. Call **`product_video`** with the defaults below.
4. **Poll `check_generation`** — video runs longer; 45–60s between checks is fine (see [./references/jobs.md](./references/jobs.md)).
5. **Deliver** the video URL + one-line summary (duration, aspect).

## Call

Fields map to `AnimateRequest`:

```json
{
  "body": {
    "first_frame_url": "<url from upload>",
    "prompt": "slow push-in, soft parallax, gentle light shimmer on the bottle",
    "duration": 10,
    "aspect_ratio": "9:16",
    "resolution": "std",
    "video_model": "kling-3.0"
  }
}
```

- **`first_frame_url`** (required) — start frame (link from `upload`).
- **`prompt`** (required) — the motion description: camera move (push-in, orbit, parallax) + subject motion + the single beat.
- **`duration`** — default 10s.
- **`aspect_ratio`** — `16:9` (default), `9:16` (Reels/TikTok), or `1:1`.
- **`resolution`** — `std` (default) or `pro`.
- **`video_model`** — `kling-3.0` (default), `seedance-2.0`, or `veo-3.1`. Leave default unless asked.
- **Seedance only:** you may pass `image_urls` (1–4 references) and an optional `last_frame_url` for a transition.

## Pre-generation interview

At most 2–3 labeled questions, skipping the obvious:

1. What motion? `[Slow push-in / Orbit around / Parallax reveal / Subtle idle motion]`
2. Aspect? `[9:16 Reels / 1:1 Feed / 16:9 Web]`
3. Length? `[~5s / ~10s]`

## Delivering

```
Your 10s product video is ready (9:16):
<url>
```

## What this skill does NOT do

- No presenter/creator video → **ecomrads-ugc-video**.
- No still images → **ecomrads-product-photoshoot**.
- No static ad slides → **ecomrads-static-ads**.
- No scoring → **ecomrads-virality-analysis**.

## Common mistakes

- Cramming multiple actions into one clip — keep to one beat.
- Forgetting `first_frame_url` (this tool animates a start frame).
- Polling too aggressively on long video jobs.
