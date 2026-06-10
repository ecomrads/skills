---
version: 0.1.0
name: ecomrads-virality-analysis
description: |
  Run Virality Analysis on a finished ad (image or video) via the eComrads MCP
  `analyze_ad` tool. Returns a creative breakdown — strengths, weaknesses, and a
  virality/performance score — as a text report, not a generated asset.
  Use when: "virality analysis", "analyze virality", "will this go viral",
  "score this ad", "analyze my ad", "rate this creative", "is this ad good",
  "evaluate the hook", "ad feedback", "how will this perform", "critique this
  creative".
  NOT for: generating images (use ecomrads-product-photoshoot), product video
  (use ecomrads-product-video), UGC video (use ecomrads-ugc-video), static ads
  (use ecomrads-static-ads), competitor research (use ecomrads-competitor-spy).
argument-hint: "[--video] [image-or-video]"
allowed-tools: upload, analyze_ad, check_generation
---

# Virality Analysis

Analyze a finished ad and predict its performance with the eComrads **`analyze_ad`** tool (`POST /post/ad-score`). This is **analysis in → text report out** — not a generated media asset.

> Naming: the customer-facing feature is **Virality Analysis**. The underlying MCP tool is still named **`analyze_ad`** (route `/post/ad-score`) — call that, but present results as "Virality Analysis."

Read [./AGENTS.md](./AGENTS.md) first.

## Workflow

1. **Upload** the ad creative → `upload` → public link (or use a link the user gives). Sandbox paths don't work.
2. Call **`analyze_ad`** with the link.
3. **Poll `check_generation`** until terminal.
4. **Deliver the summary** in plain language as a Virality Analysis.

## Call

Fields map to `AdScoreRequest`:

```json
{
  "body": {
    "media_url": "<url from upload>",
    "media_type": "image",
    "context": "DTC skincare, target 25–34 women, IG feed"
  }
}
```

- **`media_url`** (required) — the creative to analyze (link from `upload`). This tool takes **only** a URL — never base64 or a chat image directly.
- **`media_type`** — `image` (default) or `video`.
- **`context`** — optional: audience, platform, offer, goal. Improves the analysis.

## Delivering

Report the result from the **completed** job as a short, plain-language Virality Analysis — headline score, the strongest elements, the biggest weaknesses, and 1–3 concrete fixes. Never fabricate numbers; use only what the job returns.

```
Virality Analysis: 72/100

Strong: clear hero product, high contrast, legible offer.
Weak: hook is generic, CTA competes with the logo.
Fixes: lead with the benefit in the first line; isolate the CTA; warm the color grade.
```

If the job fails, state the reason plainly and offer to retry.

## What this skill does NOT do

- Does not generate or edit creatives — route generation to the photoshoot / video / UGC / static-ads skills.
- Does not invent a score before the job completes.

## Common mistakes

- Passing a chat image or base64 instead of a `media_url` from `upload`.
- Wrong `media_type` (use `video` for video ads).
- Reporting a score before the job reaches `completed`.
- Dumping raw JSON instead of a plain-language summary.
- Calling a tool named `virality_analysis` — the actual MCP tool is `analyze_ad`.
