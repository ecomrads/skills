# Shared behavior — eComrads MCP Skills

These rules apply to **every** eComrads skill. (Also valid as `CLAUDE.md`.)

## UX rules

1. **Be concise.** No raw job IDs, no JSON dumps in chat. For generated media, print the **URL** (embed it if the client renders images/video). For Virality Analysis, print the **text summary**.
2. **No internal jargon.** Don't narrate "uploading base64", "calling the API", "enqueuing", or "polling job 4f2a…". Show progress and the result.
3. **Detect the user's language** from their first message and reply in it. Keep technical args in English (`aspect_ratio: "16:9"`, `media_type: "image"`, mode/style slugs).
4. **Don't batch-ask.** Pick sane defaults and ask **at most one** thing at a time, only when a required input is genuinely missing (the product image, or the core idea).
5. **Don't pre-estimate credits** or push cheaper/faster models unless the user asks. Default to **quality** first.
6. **Drive jobs to completion.** After starting a generation, poll `check_generation` on a calm cadence (see [references/jobs.md](./references/jobs.md)) and surface only the final result. Never make the user run a manual "generate → then check" step.
7. **Uploads are invisible.** Take the user's image (or URL), run `upload`, and use the returned link downstream. Don't paste raw upload URLs or base64 into chat unless asked.
8. **Never invent results or URLs.** Scores, links, and outputs come only from completed jobs. If a job fails, report the reason plainly.

## MCP call shape

Every eComrads POST tool takes a single top-level `body` object matching the API:

```json
{ "body": { "prompt": "…", "aspect_ratio": "4:5" } }
```

Never double-wrap (`{ "body": { "body": {…} } }`). The image links you pass to creative tools (`image_urls`, `media_url`, `product_image_url`, `first_frame_url`) must be links returned by `upload`.

## The universal loop

> **upload** → write a layered **Master Prompt** ([references/prompting.md](./references/prompting.md)) → call the **skill tool** with good defaults → poll **check_generation** calmly → **deliver** the URL → offer one tasteful refinement.

## Prompt craft (always)

A short request ("make my candle look premium") is a brief, not a prompt. Expand it into a 6-layer Master Prompt before generating — see [references/prompting.md](./references/prompting.md). eComrads also refines prompts server-side (`auto_refine_prompt` defaults to true), but a specific, well-structured prompt always wins.
