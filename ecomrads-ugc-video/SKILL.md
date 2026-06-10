---
version: 0.1.0
name: ecomrads-ugc-video
description: |
  Create UGC-style creator videos via the eComrads MCP `ugc_video` tool —
  authentic, organic-feeling ad content with a presenter/actor talking about a
  product. Testimonials, unboxings, how-to/demos, and vlog-style clips.
  Use when: "UGC video", "UGC ad", "creator video", "influencer-style video",
  "testimonial video", "unboxing video", "product review video", "how-to /
  demo video", "talking presenter with my product", "TikTok/Reels creator ad",
  "spokesperson ad", "user-generated content".
  NOT for: animating a product still with no presenter (use
  ecomrads-product-video), still images (use ecomrads-product-photoshoot),
  static ad slides (use ecomrads-static-ads), scoring a video (use
  ecomrads-virality-analysis).
argument-hint: "[--style testimonial|unboxing|how_to_use|vlog_style] [script]"
allowed-tools: upload, ugc_video, check_generation
---

# UGC Video

Creator-style, organic-feeling ad video via the eComrads **`ugc_video`** tool (`POST /post/ugc`). A presenter/actor speaks a script while showing the product — the format that powers most modern paid social. The backend vision-refines the script (clearer delivery, same intent) and returns a video URL via a job.

Read [./AGENTS.md](./AGENTS.md) and [./references/prompting.md](./references/prompting.md) first.

## When to use this vs product-video

- **Presenter talking / holding / demoing the product** (testimonial, unboxing, review, how-to) → **this skill** (`ugc_video`).
- **Just animate a product still** (camera move, motion, no person speaking) → **ecomrads-product-video** (`product_video`).

## Workflow

1. **Upload** the product image(s) → `upload` → 1–4 product links (see [./references/upload.md](./references/upload.md)). Optionally upload an **actor/presenter** image too.
2. **Write the script** (the spoken line(s)) and pick a **direction** + **video_style**. One clear idea per clip.
3. Call **`ugc_video`** with the defaults below.
4. **Poll `check_generation`** — UGC/video jobs run longer; 45–60s between checks is fine (see [./references/jobs.md](./references/jobs.md)).
5. **Deliver** the video URL + one-line summary (style, duration, aspect).

## Call

Fields map to `UGCRequest`:

```json
{
  "body": {
    "script": "Honestly this is the comfiest hoodie I own — here's why…",
    "image_urls": ["<product url from upload>"],
    "actor_image_url": "<optional presenter url from upload>",
    "direction": "authentic",
    "video_style": "testimonial",
    "duration": 5,
    "aspect_ratio": "9:16",
    "resolution": "std",
    "video_model": "kling-3.0"
  }
}
```

- **`script`** (required) — the spoken line(s). Keep it tight and natural; one idea per clip. The backend refines short scripts unless `auto_refine_prompt` is set false.
- **`image_urls`** (required) — 1–4 product links from `upload`.
- **`actor_image_url`** — optional presenter/avatar link. Omit to let the engine use product references only.
- **`direction`** (required) — one of `authentic`, `high_energy`, `asmr_calm`, `urgent_salesy`.
- **`video_style`** (required) — one of `testimonial`, `unboxing`, `how_to_use`, `vlog_style`.
- **`duration`** — default 5s.
- **`aspect_ratio`** — `9:16` (default — best for Reels/TikTok), `16:9`, or `1:1`.
- **`resolution`** — `std` (default) or `pro`.
- **`video_model`** — `kling-3.0` (default), `seedance-2.0`, or `veo-3.1`. Leave default unless asked.
- **`creative_prompt`** — optional raw art-direction note (injected verbatim for visual direction; not refined).

## Style & direction guide

| video_style | Use it for |
|-------------|------------|
| `testimonial` | Honest first-person endorsement / social proof |
| `unboxing` | Open-and-reveal excitement, first impressions |
| `how_to_use` | Quick demo / tutorial of the product in action |
| `vlog_style` | Casual day-in-the-life, product woven in naturally |

| direction | Tone |
|-----------|------|
| `authentic` | Calm, genuine, believable (default-safe) |
| `high_energy` | Fast, upbeat, hype |
| `asmr_calm` | Soft, slow, sensory |
| `urgent_salesy` | Direct-response, "act now" |

## Pre-generation interview

At most 3–4 labeled questions, skipping anything obvious from context:

1. Style? `[Testimonial / Unboxing / How-to / Vlog]`
2. Vibe? `[Authentic / High energy / ASMR calm / Urgent-salesy]`
3. Aspect? `[9:16 Reels / 1:1 Feed / 16:9 Web]`
4. (Optional) Do you have a presenter image, or should the engine pick one?

If the user gives a hook/offer, draft the `script` yourself from it — don't make them write it.

## Delivering

```
Your UGC testimonial is ready (9:16, 5s):
<url>
```

## What this skill does NOT do

- No presenter-free product animation → **ecomrads-product-video**.
- No still images → **ecomrads-product-photoshoot**.
- No static ad slides → **ecomrads-static-ads**.
- No scoring → **ecomrads-virality-analysis**.

## Common mistakes

- Cramming several messages into one clip — keep to one idea/beat.
- Using an invalid `direction` / `video_style` slug (must match the lists above).
- Forgetting `image_urls` (1–4 required) or passing more than 4.
- Making the user write the script when you can draft it from their hook.
- Polling too aggressively on long video jobs.
