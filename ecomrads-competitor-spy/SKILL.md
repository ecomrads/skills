---
version: 0.1.0
name: ecomrads-competitor-spy
description: |
  Research live and historical ads in the Meta Ads Library via the eComrads MCP
  `competitor_spy_meta_library` tool. Returns competitor ad metadata you can use
  for inspiration or to feed into ad recreation.
  Use when: "spy on competitors", "find competitor ads", "what ads is X
  running", "Meta ads library", "Facebook/Instagram ads for brand X", "ad
  research", "what's working in my niche", "find a winning ad to model".
  NOT for: generating creatives (use ecomrads-product-photoshoot /
  ecomrads-static-ads / ecomrads-product-video / ecomrads-ugc-video), analyzing
  an ad (use ecomrads-virality-analysis).
argument-hint: "[query] [--country XX]"
allowed-tools: competitor_spy_meta_library
---

# Competitor Spy (Meta Ads Library)

Research competitor ads with the eComrads **`competitor_spy_meta_library`** tool (`POST /spy/`). Use it to find live/historical ads, spot what's working, and source references for **ecomrads-static-ads** (`recreate_similar_ad`).

Read [./AGENTS.md](./AGENTS.md) first. Note: this may consume credits per workspace policy — don't pre-warn about cost unless the user asks.

## Call

```json
{
  "body": {
    "query": "minimalist skincare serum",
    "country": "US"
  }
}
```

- **`query`** (required) — a brand name or search term for the Meta Ads Library.
- **`country`** — optional ISO-style country token; defaults if omitted.

This returns results directly (not a long-running job) — no polling needed in most cases.

## Workflow

1. Clarify the **query** if vague (brand vs niche keyword) — one short question max.
2. Call `competitor_spy_meta_library`.
3. **Summarize** the findings: who's running what, recurring hooks/angles, formats, and a couple of standout creatives.
4. **Offer the next step:** recreate a standout with the user's product via **ecomrads-static-ads** (`recreate_similar_ad`), passing a reference `ad_media_url` from these results.

## Delivering

Plain-language summary, not raw JSON. Highlight patterns and 2–3 noteworthy ads with their angle. If the user wants to act on one, hand the reference link to the static-ads skill.

```
Found active ads for "minimalist skincare serum" (US):
- Brand A — benefit-led hook, 4:5 lifestyle, "glass skin in 14 days"
- Brand B — UGC testimonial, 9:16, before/after
- Brand C — bold typographic offer, 1:1

Want me to recreate one of these with your product?
```

## What this skill does NOT do

- Does not generate or analyze creatives — chain to the generation or virality-analysis skills.

## Common mistakes

- Searching with an empty/too-broad query.
- Dumping raw JSON instead of an insight summary.
- Treating it like a long job (results come back directly).
