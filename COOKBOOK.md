# Cookbook — eComrads MCP Skills

Copy-paste recipes. Every creative tool takes a single `body` object and returns a job → poll `check_generation`. Image links come from `upload`. See [AGENTS.md](./AGENTS.md) and [references/prompting.md](./references/prompting.md).

## 1. Lifestyle product photo

```
upload → { "body": { "url": "https://.../candle.jpg" } }
edit_product_photo → { "body": {
  "image_urls": ["<upload url>"],
  "prompt": "<6-layer lifestyle Master Prompt>",
  "aspect_ratio": "4:5", "resolution": "2K"
} }
check_generation → { "job_id": "<id>" }   # poll ~30s
```

## 2. Website hero banner

```
edit_product_photo → { "body": {
  "image_urls": ["<upload url>"],
  "prompt": "<hero Master Prompt, wide negative space for headline>",
  "aspect_ratio": "16:9", "resolution": "2K"
} }
```

## 3. Animate a product (Reels)

```
upload → product image
product_video → { "body": {
  "first_frame_url": "<upload url>",
  "prompt": "slow push-in, soft parallax, subtle light shimmer",
  "duration": 10, "aspect_ratio": "9:16"
} }
check_generation → poll ~45–60s (video is slower)
```

## 4. UGC unboxing video (ecomrads-ugc-video)

```
upload → product image (+ optional presenter image)
ugc_video → { "body": {
  "script": "Okay let's open this — first impressions…",
  "image_urls": ["<product upload url>"],
  "actor_image_url": "<optional presenter upload url>",
  "direction": "high_energy", "video_style": "unboxing",
  "duration": 5, "aspect_ratio": "9:16"
} }
check_generation → poll ~45–60s
```

UGC styles: `testimonial`, `unboxing`, `how_to_use`, `vlog_style`. Directions: `authentic`, `high_energy`, `asmr_calm`, `urgent_salesy`.

## 4b. Multi-angle storyboard (ecomrads-storyboard)

```
upload → product image
multi_angles → { "body": {
  "image_urls": ["<upload url>"],
  "aspect_ratio": "4:5", "resolution": "2K",
  "prompt": "consistent studio lighting across all angles"
} }
check_generation → poll until COMPLETED   # produces 9 angle shots
```

`image_urls` accepts 1–2 references. Outputs 9 still angle shots (not video).

## 5. Static ad carousel (3 slides)

```
static_product_ad → { "body": {
  "product_image_url": "<upload url>",
  "instructions": "Black Friday, quiet-luxury tone, emphasize 40% off",
  "ad_format": "4:5", "num_slides": 3, "resolution": "2K"
} }
```

## 6. Spy → recreate a winning competitor ad

```
competitor_spy_meta_library → { "body": { "query": "skincare serum", "country": "US" } }
# pick a reference ad URL from the results, then:
recreate_similar_ad → { "body": {
  "product_image_url": "<your product upload url>",
  "ad_media_url": "<reference ad url>",
  "media_type": "image"
} }
```

## 7. Virality Analysis of a finished ad (ecomrads-virality-analysis)

```
upload → ad creative
analyze_ad → { "body": {
  "media_url": "<upload url>",
  "media_type": "image",
  "context": "DTC skincare, IG feed, women 25–34"
} }
check_generation → poll, then deliver the plain-language Virality Analysis
```

> The feature is **Virality Analysis**; the MCP tool is still `analyze_ad` (route `/post/ad-score`).

## Chaining

- **photoshoot → product-video / ugc-video:** generate a hero still, then animate it or use it as a UGC product reference.
- **storyboard:** `multi_angles` → 9 angle shots of one product (stills).
- **spy → static-ads:** use a spied ad as `ad_media_url` for `recreate_similar_ad`.
- **any creative → virality-analysis:** validate the output before the user ships it.
