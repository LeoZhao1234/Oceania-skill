---
name: marketplace-image-rework
description: Analyze a batch of Amazon/AMZ product photos and turn them into a compliant cross-marketplace image rework plan for TUMU/Temu or other marketplaces. Use when Codex receives a group of product photos, image URLs, or an image folder and must classify each source image, decide which marketplace asset it should become, produce image-edit or image-generation prompts, create SKU-level deliverables, run image inventory checks, or define QA criteria. Do not use for evading duplicate-detection systems, bypassing marketplace enforcement, copying competitor imagery, removing ownership marks without authorization, or guaranteeing that detection will be avoided.
---

# Marketplace Image Rework

## Purpose

Convert a group of AMZ product photos into legitimate, platform-ready creative briefs and deliverables by identifying what each source photo is, deciding how it should be re-created for TUMU/Temu, and producing actionable edit/generation prompts. Treat this as a re-creation workflow, not as an anti-detection or concealment workflow.

## Guardrails

- Do not promise to avoid, bypass, defeat, or test against marketplace duplicate-detection, enforcement, moderation, watermark, fingerprinting, or ranking systems.
- Do not provide stealth transformations whose main purpose is to hide that an image was reused.
- Require that the user owns or is licensed to use the input photos, product renders, brand marks, and packaging.
- Treat the input photos as the user's own active Amazon product listing photos when the user states they are selling the product on Amazon.
- Prefer genuinely new assets: new photography, new 3D renders, new scene layouts, fresh lighting, revised props, corrected packaging, and updated copy.
- Remove Amazon-specific UI, badges, review stars, marketplace labels, ASIN text, watermarks, or seller-only marks only when the user has rights to the underlying product image and the removal is for clean platform adaptation.
- Do not include children, kids, teenagers, minors, or any underage person in generated imagery or replacement text.

## Mandatory Edit Requirements

Apply these requirements whenever editing Amazon product photos for TUMU/Temu:

- Quality: produce ultra-high-definition, realistic, sharp, natural-light images with true color, natural shadows, no distortion, clearly visible accessories, and a polished Temu-style commercial atmosphere.
- Product protection: edit only the intended mask/background/empty area and editable text areas. Do not cover, redraw, deform, recolor, replace, crop away, or alter the product body itself. Preserve product structure, colors, textures, shapes, proportions, included accessories, brand logos, model numbers, certification marks, compliance labels, and mandatory safety labels.
- Background: replace or improve the background to highlight the product in a believable real-world scene. Background objects must stay behind or around the product and must not hide product details.
- People: if the source image contains people, replace the person with a different adult model and change pose, position, and camera angle while keeping the product accurate. If no person appears, ignore this requirement.
- Text: source marketing text must not remain unchanged. If the source image contains editable marketing text, feature copy, infographic copy, benefit claims, captions, callouts, banners, badges, comparison labels, or non-mandatory explanatory text, remove the original text and create new rewritten text while preserving the original meaning. Change more than 60% of the wording, sentence structure, word order, line breaks, typography, and layout expression compared with the source text. Use local-market wording and keep claims factual. If reliable rewritten text cannot be rendered, remove the editable source text entirely rather than leaving it unchanged. Do not change brand logos, model numbers, certification marks, compliance labels, legally required warnings, or mandatory safety labels.
- Do not treat Amazon listing overlay text, infographic headings, feature bullets, promotional badges, comparison labels, or benefit callouts as product labels. Those are editable marketing text and must be replaced or removed.
- Measurement standards: use local-market units and conventions for the target marketplace and region. Convert dimensions, weight, capacity, temperature, voltage, and other units when needed.
- Prohibited content: image and text must not mention or depict children, kids, teenagers, minors, or other underage people.

## Workflow

1. Accept the photo batch:
   - Inputs can be attached images, image URLs, a local folder, a CSV manifest, or an n8n JSON payload.
   - If the product/SKU name is missing, infer a temporary product label from filenames and visible product attributes.
   - If a folder is provided, run `scripts/inventory_images.py <image-folder> --out <manifest.csv>`.
   - Do not require the user to provide a SKU table before doing useful work.

2. Classify every source photo:
   - `hero-main`: product-only or primary AMZ main image.
   - `angle-detail`: side/back/top/detail/texture/connector/finish closeup.
   - `lifestyle`: product shown in use or in a room/outdoor/context scene.
   - `infographic`: feature, comparison, benefit, dimension, or callout image.
   - `package-contents`: packaging, bundle, included accessories, unboxing, kit layout.
   - `variant`: color, size, quantity, style, or model variation.
   - `low-quality/unknown`: blurry, cropped, watermarked, too small, irrelevant, or unclear.

3. Clarify the target marketplace and deliverables:
   - Target: TUMU/Temu or another marketplace.
   - Output types: main image, gallery images, lifestyle images, size chart, feature infographic, variant swatches.
   - Constraints: aspect ratio, minimum pixel size, background rules, text/logo rules, category restrictions.

4. Build a source-to-target map:
   - Use the image inventory and visual classification to map each input photo to one target asset.
   - Flag missing assets. Example: no clean hero image, no scale image, no back angle, no dimensions, no package contents.
   - Flag low-resolution, compressed, blurry, mismatched, watermarked, marketplace-specific, or text-heavy photos for manual review.

5. Create an originality-first rework plan:
   - Main image: show the actual product clearly, use clean lighting, correct color, accurate proportions, and a marketplace-appropriate background.
   - Gallery: add angles that were not present in the source set, such as back, side, scale-in-hand, packaging, detail closeups, and in-use context.
   - Infographics: rebuild layout from scratch with concise claims, measured dimensions, compliant icons, and verified feature text.
   - Lifestyle: create or brief a new scene that matches the buyer use case and does not misrepresent size, quantity, materials, or included accessories.
   - Variants: produce consistent angles and lighting across colors/sizes.

6. Write edit or generation prompts:
   - Describe the product, materials, shape, colors, camera angle, lighting, surface, and intended buyer context.
   - State factual constraints: exact quantity, included accessories, label text, color accuracy, packaging version.
   - Include the mandatory edit requirements, especially product protection and no-minors constraints.
   - Avoid instructions like "make it pass detection", "change enough pixels", "avoid Amazon matching", or "hide reuse".
   - For AI image generation, ask for a new product render or scene while preserving factual product identity.

7. QA every output:
   - Product accuracy: no altered structure, wrong quantity, false material, fake accessories, or changed branding.
   - Product preservation: no product body pixels, product edges, textures, colors, accessories, proportions, brand logos, model numbers, certification marks, compliance labels, or mandatory safety labels are changed unless the user explicitly supplied a product mask that allows it.
   - Marketplace compliance: background, file size, dimensions, prohibited text, claim substantiation, category-specific requirements.
   - Visual quality: sharp edges, natural shadows, no distorted labels, no generated text artifacts, clean masks.
   - Minor-safety: no children, kids, teenagers, minors, school-age people, childlike bodies, or underage references appear in image or text.
   - Commercial readiness: thumbnail legibility, consistent gallery order, coherent style across SKU family.

## Deliverable Format

When producing a plan from a photo batch, use this compact table:

| Input photo | Detected role | Target TUMU asset | Rework direction | Prompt needed | Required checks | Status |
| --- | --- | --- | --- | --- | --- | --- |

For each photo batch, include:

- A short product/SKU summary inferred from the photos and filenames.
- A source-photo classification table.
- A one-line main-image brief.
- A gallery shot list.
- A prompt set for each target asset.
- QA notes and any missing inputs.

## Prompt Template

Use this template for each source photo that becomes a target asset:

```text
Input photo: [filename or URL].
Detected source role: [hero-main/angle-detail/lifestyle/infographic/package-contents/variant/unknown].
Create a new marketplace-ready product image for [SKU/product name] on [target platform].
Product facts: [materials, dimensions, colors, quantity, included accessories].
Target asset: [main image/gallery/lifestyle/infographic].
Composition: [camera angle, crop, background, props, lighting].
Preserve accuracy: [brand/logo/model/certification/package/color constraints].
Edit boundary: modify only [mask/background/empty area/editable text area]; do not alter the product body, texture, color, shape, accessories, logo, model number, certification mark, compliance label, or mandatory safety label.
People: if people appear, replace only with adult models and change pose/position/angle; if no people appear, ignore.
Text: editable marketing/feature/infographic/explanatory text must not remain unchanged. Remove the original editable text and replace it with rewritten text preserving meaning and local-market units; change more than 60% of wording, sentence structure, word order, line breaks, typography, and layout expression. If rewritten text cannot be rendered reliably, remove editable source text rather than keeping it. Preserve brand logos, model numbers, certification marks, compliance labels, legally required warnings, and mandatory safety labels.
Avoid: misleading scale, fake features, extra accessories, unreadable text, marketplace badges, review stars, watermarks, children, kids, teenagers, minors, or underage references.
Output requirements: [aspect ratio, pixel size, file type, transparent/white/background style].
```

## n8n Input Shape

When called from n8n, expect one of these payload styles:

```json
{
  "target_platform": "TUMU/Temu",
  "category": "product category",
  "image_requirements": "ratio, size, file type, background rules",
  "photos": [
    {
      "file_name": "sku-main-01.jpg",
      "url": "https://example.com/image.jpg",
      "sku": "optional-sku",
      "notes": "optional product facts"
    }
  ]
}
```

If only file paths or URLs are provided, classify the images first and then ask only for missing facts that materially affect accuracy, such as dimensions, included accessories, color variants, or claims.

## References

- Read `references/rework-standards.md` when preparing a detailed operating checklist or QA rubric.
