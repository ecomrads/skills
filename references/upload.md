# Upload — the only upload step you need

Every creative tool needs a **public image link**. The `upload` tool turns a user's image (or a URL) into one. Run it first, then pass the returned link downstream.

## How to call

```json
{ "body": { "url": "https://cdn.example.com/product.jpg" } }
```

or paste/attach the image bytes (the tool accepts an image the user shared in chat).

## Using the result

The tool returns a public URL. Use that exact link as:

- `image_urls[]` → `edit_product_photo`, `ugc_video`, `multi_angles` (1–2)
- `first_frame_url` → `product_video`
- `product_image_url` → `static_product_ad`, `recreate_similar_ad`
- `media_url` → `analyze_ad`

## Rules

- **One upload per image.** Don't re-upload the same image for each tool — reuse the link.
- **Sandbox paths don't work.** Links like `/mnt/data/photo.jpg` are not fetchable. If that's all you have, ask the user for a public `https://` link or to re-attach the image.
- **Don't paste the raw link into chat** unless the user asks — just use it in the next tool call.
- **Multiple products?** Upload each once and pass the links together (e.g. `image_urls` accepts up to the tool's limit).
