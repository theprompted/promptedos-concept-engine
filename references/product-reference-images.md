# Product Reference Images

This file documents how to handle product reference images so the actual product appears in generated ads — not a generic AI render.

## Why Reference Images Matter

Without a reference image, the image generator invents a product. It might be the wrong:
- Shape (rounded when it should be angular)
- Color (silver when it should be matte black)
- Size (too small or too large)
- Branding (missing logos, wrong text)

With a reference image, the generator composites the ACTUAL product into the scene.

## Setting Up Product References

### Step 1: Gather Product Images

Get 1-3 clean product shots. Best options:
1. **Product page images** — usually on white/transparent background
2. **Packshots** — professional studio shots
3. **Lifestyle shots** — product in context (useful for matching lighting)

Save to: `[client-folder]/product-references/`

### Step 2: Document the References

Create `[client-folder]/product-references.md`:

```markdown
# Product Reference Images

## Primary Product
- **Name:** [Product Name]
- **Images:**
  1. `product-references/product-front.png` — front view, white background
  2. `product-references/product-angle.png` — 3/4 angle, white background
  3. `product-references/product-lifestyle.png` — on nightstand, natural lighting

## Packaging (if relevant)
- `product-references/box-front.png`

## Accessories
- `product-references/charging-cable.png`
```

### Step 3: Reference in Prompts

When generating prompts, every concept that shows the product includes:

```json
"product_visibility": "prominent",
"product_reference": {
  "image_index": 1,
  "placement": "description of where in scene",
  "scale": "relative size guidance",
  "lighting_match": "how to match scene lighting"
}
```

## Product Visibility Levels

| Level | When to Use | Example |
|-------|-------------|---------|
| `none` | Product not shown — problem-focused, face testimonials | Delay spray on counter (not our product) |
| `background` | Product visible but not focal | Device charging on nightstand, corner of frame |
| `prominent` | Product clearly visible, part of story | Hand holding device, device on table |
| `hero` | Product is the main element | Product-focused shot, unboxing |

## Model-Specific Handling

### FLUX.2 [pro] Edit (fal-ai/flux-2-pro/edit)
- Pass product images in `image_urls` array
- Reference with `@image1`, `@image2` in prompt
- Native multi-reference compositing

```python
{
  "prompt": "bedroom nightstand scene with @image1 (the device) charging next to coffee mug...",
  "image_urls": [
    "https://storage.../product-front.png"
  ]
}
```

### FLUX.1 Kontext (fal-ai/flux-pro/kontext)
- Single image editing
- Best for: adding product to existing scene
- Pass product image, describe placement

### GPT-4o / GPT Image
- Upload reference image in conversation
- Use "the product from the reference image" in prompt
- Model remembers images in same chat

### Models Without Reference Support
For models that can't composite references:
1. Generate scene with placeholder description
2. Use separate compositing step (Photoshop, GIMP, or another AI tool)
3. OR generate product separately and composite manually

## Folder Structure

```
clients/
└── [client-folder]/
    ├── product-references/
    │   ├── product-front.png
    │   ├── product-angle.png
    │   └── product-lifestyle.png
    ├── product-references.md
    └── ad-concepts-[date]/
        └── concepts.html
```

## Common Mistakes

1. **Using text description instead of reference** — "matte black device" ≠ actual product
2. **Wrong reference for scene** — front view doesn't work for angled placement
3. **Ignoring lighting** — product looks pasted in if lighting doesn't match
4. **Scale mismatch** — product too large/small for scene context
