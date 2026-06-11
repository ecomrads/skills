# Install eComrads Skills

Seven skills ship in this repo:

- **`ecomrads-product-photoshoot`** — brand-quality product images & edits (studio, lifestyle, hero, try-on, restyle)
- **`ecomrads-storyboard`** — multi-angle 9-shot product storyboard (consistent angle views from one image)
- **`ecomrads-product-video`** — animate a product still into motion (image-to-video, no presenter)
- **`ecomrads-ugc-video`** — UGC-style creator video (testimonial, unboxing, how-to, vlog)
- **`ecomrads-static-ads`** — static / carousel ad creatives + recreate a competitor ad
- **`ecomrads-virality-analysis`** — analyze a finished ad (image or video) and predict performance
- **`ecomrads-competitor-spy`** — Meta Ads Library research

They chain: **competitor-spy** → feed a reference into **static-ads** (`recreate_similar_ad`); **product-photoshoot** → use the still as a start frame for **product-video** / **ugc-video**; **storyboard** generates 9 angle views of one product; any creative → **virality-analysis** to validate.

## Prerequisite — connect the eComrads MCP server

The skills are instructions that call eComrads MCP tools. **Installing the skills does not connect the server** — do that once in your agent first:

1. Add a custom MCP server / connector pointing at:
   ```
   https://mcp.ecomrads.com/mcp
   ```
2. Authorize in the browser when prompted (your eComrads account).

Need an account? Sign up at [ecomrads.com](https://ecomrads.com).

## Option 1 — `npx skills` (recommended, cross-agent)

Works with Claude Code, Cursor, Codex, and any agent that loads `SKILL.md` skills. Requires Node.js.

```bash
npx skills add ecomrads/skills
```

Installs all seven skills; the `skills` CLI auto-detects the host agent and writes each skill to the right path.

## Option 2 — `gh skill install`

GitHub CLI v2.90+:

```bash
gh skill install ecomrads/skills
```

## Option 3 — Claude Code marketplace

Inside Claude Code:

```
/plugin marketplace add ecomrads/skills
/plugin install ecomrads@ecomrads
```

Registers all seven skills as `/ecomrads:product-photoshoot`, `/ecomrads:storyboard`, `/ecomrads:product-video`, `/ecomrads:ugc-video`, `/ecomrads:static-ads`, `/ecomrads:virality-analysis`, `/ecomrads:competitor-spy`.

## Option 4 — Setup script

Universal fallback. Clones the repo and symlinks each skill into the agent's skills directory.

```bash
git clone --depth 1 https://github.com/ecomrads/skills.git
cd skills
./setup            # or: ./setup --host cursor|claude|codex
```

Idempotent — safe to re-run.

## Verify

In your agent, with the MCP server connected, ask:

> "Make a clean studio product photo from this image." (attach a product photo)

The agent should invoke `ecomrads-product-photoshoot`, call `upload` then `edit_product_photo`, poll `check_generation`, and deliver the image URL.

## Updating

| Method | Update command |
|--------|----------------|
| `npx skills` | re-run `npx skills add ecomrads/skills` |
| `gh skill install` | `gh skill update ecomrads/skills` |
| Claude marketplace | `/plugin update ecomrads@ecomrads` |
| Setup script | `cd skills && git pull && ./setup` |

> Replace `ecomrads/skills` with your actual `github-owner/repo` if you publish under a different name.
