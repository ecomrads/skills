# Prompting system — get great outputs

> "The AI User types 'cool sports car' and prays. The AI Director engineers the result." Direct the model; don't gamble.

A short user request is a **brief**, not a prompt. Before calling any generation tool, expand it into a **Master Prompt**. eComrads refines prompts server-side, but a specific, layered prompt always produces better, more on-brand work.

*(Framework distilled from "The Visual System" — Andreis Ohneisser: intentional direction, the Alpha→Master prompt workflow, the 3 pillars, layered realism.)*

## The 6-layer creative brief

Build every image/edit prompt from these layers. Fill each one; omit nothing important.

1. **Subject & intent** — exactly what the hero is and the goal. *"a matte-black soy candle as a luxury gifting hero shot."*
2. **Composition & framing** — angle, crop, perspective, rule of thirds, negative space. *"three-quarter angle, low hero perspective, product fills ~60% of frame."*
3. **Lighting** — setup, direction, quality, mood. *"soft key from camera-left, subtle rim light, warm practical glow, gentle falloff."*
4. **Environment & styling** — surface, background, props, season/context. *"polished travertine, blurred warm interior, eucalyptus sprig."*
5. **Materials & detail** — textures, finishes, micro-detail, realism cues. *"soft wax sheen, crisp embossed label, faint condensation, visible grain."*
6. **Technical & style** — camera/lens feel, color grade, resolution, render quality. *"85mm look, shallow depth of field, editorial color grade, ultra-detailed, photoreal."*

## The 3 pillars (quality gate)

Before sending, confirm the prompt delivers:

- **Intention** — every choice serves the goal; nothing is generic.
- **Technical precision** — concrete lighting, lens, angle, material language (not vague adjectives).
- **Consistency** — reuses the brand's recurring look (same lighting / grade / mood across a set) so outputs feel like one family.

## Director's vocabulary (specifics beat adjectives)

- **Camera / lens:** ultra-wide 14–24mm (drama), 35mm (context), 50–85mm (natural/portrait), macro (texture); shallow vs deep depth of field; eye-level vs low hero vs top-down.
- **Lighting:** soft key, hard key, rim/back light, three-point, golden hour, overcast softbox, rear-curtain flash, high-key vs low-key, directional shadows.
- **Composition:** rule of thirds, leading lines, negative space, symmetry, foreground framing, hero scale.
- **Grade / finish:** editorial, cinematic, muted/desaturated, punchy contrast, filmic grain, clean commercial.

## Iterate the right way

When the user wants changes, **rewrite the full prompt** with the change folded in — never "just adjust this image." Example: *"rewrite the full prompt with more motion blur and softer shadows,"* keeping the rest of the brief intact. This preserves the look and yields a reusable Master Prompt.

## Motion layer (video)

For `product_video` / `ugc_video`, add motion intent on top of the 6 layers: camera move (slow push-in, orbit, parallax), subject/physics motion, pacing, and the **single beat** the clip should land. Short clips can't tell long stories — one clear action per clip.

## Aspect ratio by intent

| Use | Ratio |
|-----|-------|
| Feed post (square) | `1:1` |
| Portrait feed | `4:5` |
| Story / Reels / TikTok | `9:16` |
| Web hero / banner / YouTube | `16:9` |
| Pinterest-style tall | `3:4` |

## Anti-patterns

- ❌ One-line prompt straight to a tool → expand into the 6-layer brief.
- ❌ Vague adjectives ("nice", "premium") with no technical specifics.
- ❌ Re-prompting with "adjust this image" → rewrite the full Master Prompt.
- ❌ Mixing five ideas into one clip → one beat per video.
