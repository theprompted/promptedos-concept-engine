---
name: ConceptEngine
description: This skill should be used when the user asks to "generate ad concepts", "make ads for [product]", "create ad prompts", "ad concepts from funnel", or provides a funnel/landing page and wants ready-to-paste image generation prompts with copy.
version: 0.4.0
---

# ConceptEngine

Generate end-to-end ad concepts from a funnel or landing page. Output: ready-to-paste image generation prompts in an HTML file with copy buttons.

**CRITICAL CONCEPT: Each reference image IS a template.** The swipe file contains specific ads with specific layouts — circular insets, POV angles, masthead positions, text card placements. You are not writing "native advertorial style" prompts. You are modeling the EXACT structure of each reference image, then swapping in the client's copy and product.

## Process Overview

1. **FIRST: Read `references/copy-methodology.md` completely** — contains critical nuance on why copy fails
2. Read the funnel/landing page
3. Check for existing consumer mapping (or create one)
4. Select 5 diverse ad formats from swipe file
5. Generate 3 variations per format (15 total)
6. Output HTML with copy buttons

**CRITICAL:** Do not write ANY copy until `references/copy-methodology.md` has been read. The copy methodology contains the full nuanced back-and-forth on what makes copy fail vs succeed — summaries are not sufficient.

## Step 1: Get Input

Read the funnel/landing page HTML or URL provided.

### Product Reference Images (Required)

Before generating any prompts, locate or request product reference images:

1. **Search for existing product images** in the client folder:
   - `*product*.{png,jpg,jpeg,webp}`
   - `*reference*.{png,jpg,jpeg,webp}`
   - `assets/` or `images/` subdirectories

2. **If no product images found**, ask the user:
   > "I need 1-3 product reference images (clean shots on white/transparent background work best). These will be composited into ads that show the product. Where can I find them, or can you provide them?"

3. **Save product image paths** for use in Step 4. Store as:
   ```
   product_references:
     - path: /path/to/product-front.png
       description: "front view, white background"
     - path: /path/to/product-angle.png
       description: "3/4 angle, lifestyle context"
   ```

**Why this matters:** Without reference images, the image generator invents a generic product. With reference images, it composites the ACTUAL product into the scene.

## Step 2: Consumer Mapping

Search for `*consumer-mapping*.md` or `*hell-heaven*.md` in the client folder.

**If exists:** Load it, proceed to Step 3.

**If missing:** Create it using the question sequence in `references/consumer-mapping-questions.md`. Save to `[client-folder]/[product]-consumer-mapping.md` for reuse.

### When Consumer Mapping Can Be Skipped

Consumer mapping is critical for **transformation products** (supplements, courses, services) where the ad copy needs to speak to pain points and outcomes.

**You can skip consumer mapping for:**
- **Novelty/gift products** — The product IS the message (e.g., funny t-shirts, gag gifts)
- **Self-explanatory products** — Value is obvious from the product itself (e.g., "Size Matters" bullet caliber shirt)
- **Impulse purchases** — Low consideration, the visual + copy does the work

**How to decide:** Ask "Does the consumer need to be convinced this solves a problem, or do they just need to see it and want it?"
- If problem/solution → consumer mapping required
- If see-and-want → skip mapping, focus on lifestyle/vibe copy

**When skipping, still extract:**
- Target demographic (age, gender, context)
- Occasion/use case (gift, friend group, event)
- Tone (humor level, edginess, warmth)

Document this lightweight context in 3-5 bullets rather than full Hell/Heaven mapping.

## Step 3: Select Reference Images (NOT Format Types)

**CRITICAL: You are selecting SPECIFIC IMAGES to model, not format categories.**

1. Read `swipe-file/ad-formats.json` (relative to this skill folder)
2. Select 5-15 specific images (by `id` and `filename`) that fit this consumer
3. For EACH selected image, you will create ONE prompt that models that exact image's structure

**Wrong approach:** "I'll use 3 native-advertorial formats" → writes generic native ad concepts
**Right approach:** "I'll model bqhj.jpeg, bqjq.jpeg, bqsv.jpeg" → reads each schema, adapts exact layout

Selection criteria:
- Different `format_type` values for diversity
- Match product category via `vibe.would_work_for`
- Cover different `primary_mechanic` (social proof, authority, transformation)

### Schema ID Validation (Required)

**Before using any schema ID, verify it exists:**

```bash
# Check if a specific ID exists
cat swipe-file/ad-formats.json | jq '.[] | select(.id == "002") | .id'
# Should output: "002" — if empty, that ID doesn't exist

# List all valid IDs
cat swipe-file/ad-formats.json | jq -r '.[].id' | sort
```

**If a schema ID doesn't exist:**
- Do NOT invent prompts without a schema — this defeats the purpose
- Pick a different valid ID from the list
- If you need a specific format type, search by format: `jq '.[] | select(.format_type == "ugc-honest-review")'`

**Common mistake:** Assuming IDs are sequential (001, 002, 003...). They're not — always verify.

## Step 4: Generate Prompts (One Image = One Prompt)

**STOP. Before writing any copy, confirm `references/copy-methodology.md` has been read this session.**

### Using Subagents for Batch Generation

When generating more than 5 prompts, use parallel subagents. **Proven pattern: 10 agents × 3 prompts = 30 total.**

#### Step 1: Extract schemas into temp files

```bash
# For 30 prompts, split 30 schema IDs into 10 groups of 3
cd /path/to/concept-engine
cat swipe-file/ad-formats.json | jq '[.[] | select(.id == "002" or .id == "007" or .id == "008")]' > /tmp/agent1-schemas.json
cat swipe-file/ad-formats.json | jq '[.[] | select(.id == "016" or .id == "023" or .id == "032")]' > /tmp/agent2-schemas.json
# ... repeat for all 10 groups
```

#### Step 2: Launch 10 parallel agents

Each agent gets:
- Path to their schema file (`/tmp/agent1-schemas.json`)
- Path to consumer mapping (they READ it, don't paste)
- Product details
- Explicit JSON output instruction

#### Step 3: Subagent prompt template

```
Generate 3 ad prompts for [PRODUCT NAME].

**READ THESE FILES FIRST:**
1. /path/to/consumer-mapping.md — Hell/Heaven Island copy
2. /tmp/agentN-schemas.json — Your 3 schemas to model

**PRODUCT:**
[Brief product description]

**OUTPUT FORMAT — CRITICAL:**
Output each prompt as STRUCTURED JSON with nested objects.
NOT flat text descriptions. NOT paragraphs.

Example structure:
{
  "reference_ad_id": "002",
  "format_type": "ugc-honest-review",
  "composition": { "layout": "...", "visual_hierarchy": "..." },
  "subject": { "description": "...", "face": { ... } },
  "product": { "type": "...", "position": { ... } },
  "text_overlay": { "content": "...", "style": { ... } },
  "color_story": { "primary": "...", "accent": "..." },
  "constraints": { "must_keep": [...], "avoid": [...] },
  "negative_prompt": [...]
}

For EACH schema:
1. Read its full structure
2. Preserve exact layout/composition
3. Swap in product + consumer copy
4. Output as JSON
```

**Common subagent failure:** Producing text like "Bird's eye flatlay showing..." instead of JSON. Prevent by showing the JSON example and saying "NOT text descriptions."

#### Step 4: Validate Subagent Outputs Before HTML Compilation

**CRITICAL: Do not compile HTML until all outputs are validated.**

After subagents complete, verify each output:

```bash
# For each agent output, test if it's valid JSON
cat /tmp/agent1-output.json | jq '.' > /dev/null 2>&1 && echo "✓ Valid" || echo "✗ Invalid JSON"
```

**Validation checklist (run for each output):**
1. **Is it valid JSON?** — `jq '.'` must succeed without errors
2. **Does it have required fields?** — `reference_ad_id`, `format_type`, `composition`, `constraints`
3. **Is it NOT a text description?** — If output starts with prose like "A bird's eye view..." it's wrong

**If validation fails:**
- Do NOT proceed to HTML compilation
- Re-run that specific subagent with stronger JSON instruction
- Show the failed output to diagnose: was it text? malformed JSON? missing fields?

**Quick validation script:**
```bash
for f in /tmp/agent*-output.json; do
  echo -n "$f: "
  if jq -e '.[0].reference_ad_id' "$f" > /dev/null 2>&1; then
    echo "✓ Valid (has reference_ad_id)"
  else
    echo "✗ INVALID — check output format"
  fi
done
```

**ENFORCEMENT GATE: For EACH prompt, you MUST:**
1. Read the FULL schema for that specific reference image from ad-formats.json
2. Identify the exact structural elements (circular insets, POV angles, text card positions, masthead style)
3. Write a prompt that preserves that EXACT structure while swapping in client copy/product

**One reference image = one prompt.** Do not write "variations" that deviate from the reference structure. If you want 3 prompts, select 3 different reference images.

**What you're adapting:**
- Copy/headlines → client's message using copy methodology
- Product shown → client's product with reference image handling
- Human subjects → appropriate for client's consumer demographic

**What you're preserving:**
- Layout structure (where elements are positioned)
- Visual hierarchy (what's prominent, what's background)
- Compositional elements (circular insets, POV angles, split panels)
- Typography style and positioning
- Constraints and negative prompts from the original schema

### Copy Requirements (Non-Negotiable)

Full methodology with examples in `references/copy-methodology.md`. Quick reference:

| Requirement | Bad Example | Good Example |
|-------------|-------------|--------------|
| Name problem explicitly | "what he struggles with" | "finishing in under 2 minutes" |
| Specific transformation | "she notices" | "lasted so long I asked if he took something" |
| Include mechanism | (none) | "trains the muscle that controls it" |
| No assumptions | mid-conversation copy | standalone, works for first-time viewer |
| Sound real | "crying writing this" | "I'm still in shock" |

### Product Reference Handling

**Two approaches depending on product type:**

**1. Wearables (shirts, jewelry, hats)** — No special handling. Describe in `product` object:
```json
"product": {
  "type": "Grunt Style Size Matters black crew neck t-shirt",
  "details": { "color": "black", "graphic": "SIZE MATTERS text with bullet caliber comparison (.50 CAL to .22 LR)" }
}
```

**2. Compositing products (bottles, devices, boxes)** — Use `product_visibility` + inline `product_instruction`.

Set prominence:
```json
"product_visibility": "none" | "background" | "prominent" | "hero"
```

Embed instruction where the product appears:
```json
"circular_inset_images": {
  "image_1": {
    "subject": "doctor demonstrating FlexPro device",
    "product_instruction": "USE ATTACHED PRODUCT IMAGE - show actual device in doctor's hands",
    "action": "holding device, displaying it to camera"
  }
}
```

Or for product-in-use:
```json
"product_in_use": {
  "item": "FlexPro device",
  "product_instruction": "USE ATTACHED PRODUCT IMAGE - device held naturally as if mid-session",
  "positioning": "center of POV frame, held in right hand"
}
```

**Why this works:** Natural language is clearer for image generators than technical fields. Battle-tested with good results.

### Layout Requirements (Always Include)

```json
"layout_constraints": "all elements must fit fully within frame with no cutoff or truncation, leave 20px padding from all edges"
```

Negative prompt must include: `"elements cut off, text truncated, text extending beyond frame edges"`

### Visual Detail Standard

Match `swipe-file/schema-examples-gold-standard.md`:
- Precise positioning for every element
- Exhaustive physical details (skin texture, fabric, lighting)
- Nested specificity (eyes: look, energy, direction)
- Constraints and negative prompts

## Step 5: Output HTML (MANDATORY — DO NOT DEVIATE)

Create `[client-folder]/ad-concepts-[date]/concepts.html`

### Pre-Write Verification (Required)

Before writing any HTML, run these checks:

```bash
# 1. Verify image folder exists and get sample filename
ls /.claude/skills/concept-engine/swipe-file/images/ | head -3

# 2. Confirm path format
# CORRECT: /.claude/skills/concept-engine/swipe-file/images/SCR-20260313-bhwx.jpeg
# WRONG: /reference-images/, ../../../, relative paths
```

### Use the Template (Do Not Write From Scratch)

**Copy the structure from `references/output-template-example.html` exactly.** This template contains:
- Working copy button script (uses `execCommand` method that works everywhere)
- Correct CSS styling
- Proper HTML structure with reference images above each prompt

**What to copy from template:**
- Entire `<style>` block
- Entire `<script>` block (DO NOT MODIFY — this is the working copy function)
- HTML structure for prompt cards

**What to replace:**
- `[Product Name]` → actual product name
- `[Date]` → generation date
- Prompt content and reference image paths

### Critical Rules

| Rule | Why |
|------|-----|
| Image paths start with `/` | Root-relative paths work in file browser |
| Use `/.claude/skills/concept-engine/swipe-file/images/` | This is the actual folder location |
| JSON goes directly in `<div class="prompt-text">` | No HTML escaping — raw JSON |
| Copy exact `<script>` block from template | Uses `execCommand` + textarea fallback that works everywhere |
| Each prompt-text div has unique ID | `prompt-1`, `prompt-2`, etc. for copy function |

### Common Mistakes (Do Not Repeat)

| Mistake | Fix |
|---------|-----|
| Using `navigator.clipboard` API | Use `execCommand('copy')` with textarea |
| HTML-escaping JSON (`&quot;`) | Write raw JSON, no escaping |
| Wrong image folder (`/reference-images/`) | Verify with `ls` first |
| Relative paths (`../../../`) | Always root-relative starting with `/` |
| Writing HTML from memory | Copy from `references/output-template-example.html` |

Provide file browser URL: `http://100.109.172.85:8080/[path]`

## Reference Files

- **`references/copy-methodology.md`** — Full copy requirements with examples
- **`references/consumer-mapping-questions.md`** — 7-question sequence for Hell/Heaven Island
- **`references/product-reference-images.md`** — How to handle product images for compositing
- **`references/html-template.md`** — HTML template for output
- **`examples/good-prompt.json`** — Example of properly structured prompt with product reference

## External References

- Consumer mapping example: See `references/example-consumer-mapping.md` in this skill folder
- Visual standard: `swipe-file/schema-examples-gold-standard.md`
- Ad formats: `swipe-file/ad-formats.json`
- Good prompts example: See `examples/good-prompt.json` in this skill folder
