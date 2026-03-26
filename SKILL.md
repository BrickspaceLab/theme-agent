---
name: shopify-theme-json
description: >-
  Guides creation and editing of Shopify Online Store 2.0 JSON templates (templates/*.json).
  Teaches agents to resolve section and block types from the theme's sections and schemas,
  align settings with {% schema %}, and output valid sections/order/block_order structures.
  Use when building or changing theme templates, theme editor layouts, JSON template files,
  or when adding, removing, or reordering sections and blocks in a Shopify theme.
---

# Shopify theme JSON templates (Online Store 2.0)

## When to use this skill

Use when the user (or task) involves **JSON templates** under `templates/` — e.g. `product.json`, `index.json`, alternate templates like `page.contact.json`, or customer account JSON templates under `templates/customers/` if present.

**Do not assume** this skill alone covers:

- `config/settings_schema.json` or `config/settings_data.json` (theme settings UI and values)
- Checkout branding or Checkout UI extensions
- Theme app extension code under `extensions/`
- Replacing **Liquid** `.liquid` templates where the theme has not adopted JSON for that route

If the theme uses a **build step** that generates `sections/` from source files, read that theme’s README and follow its source-of-truth paths before editing compiled output.

## Quick start

1. **Understand requirements** — Which template file? Which sections to add/remove/reorder? Any new blocks or settings?
2. **Discover types in this theme** — Section `type` strings come from `sections/*.liquid` (filename stem or schema `name`). Block `type` strings come from that section’s `{% schema %}` (`blocks` definitions), not from guesswork.
3. **Read schemas** — For every section and block you touch, read `{% schema %}`: allowed nested blocks, setting IDs, types, defaults, and enums.
4. **Compile JSON** — Build the `sections` object, top-level `order`, and each section’s `blocks` + `block_order` (and nested `blocks` / `block_order` where the schema allows).
5. **Validate** — Run through the checklist below before finishing.

## JSON shape (canonical)

Shopify JSON templates use an object of **sections**, each keyed by a **unique instance id** (string). Each section has at least `type`. Optional: `settings`, `blocks`, `block_order`. Nested blocks use the same pattern: object keyed by block instance id, optional nested `blocks` and `block_order`.

Top-level `order` is an **array of section instance ids** and defines render order.

```json
{
  "sections": {
    "<sectionInstanceId>": {
      "type": "<sectionType>",
      "settings": {},
      "blocks": {
        "<blockInstanceId>": {
          "type": "<blockType>",
          "name": "Optional display label or translation key",
          "settings": {},
          "blocks": {},
          "block_order": []
        }
      },
      "block_order": ["<blockInstanceId>"]
    }
  },
  "order": ["<sectionInstanceId>"]
}
```

**Important:** In real themes, `blocks` is an **object** keyed by id, not an array. `block_order` lists those keys in sequence. The same applies to nested blocks when the schema supports nesting.

## Workflow

### Step 1 — Requirements

From the user prompt, determine:

- Target file (e.g. `templates/product.json`)
- Sections to include, remove, or duplicate (instance ids vs `type`)
- Blocks to add/remove/reorder inside sections
- Any setting values; prefer schema defaults when unspecified

### Step 2 — Resolve section and block types

1. List `sections/` in the theme. The section **`type`** in JSON is usually the Liquid file **basename** without `.liquid` (e.g. `sections/main-product.liquid` → `"main-product"`). If unsure, open the file and read `{% schema %}` → `"name"` and surrounding docs.
2. For each section, open `{% schema %}` and find:
   - `settings` — valid `id` keys and allowed values
   - `blocks` — valid block `type` strings and which block types may nest under which
3. Some themes define blocks in **snippets** or shared files; follow includes back to the schema that defines the block.
4. **App blocks** and **theme** blocks may appear in schema as `@app` or patterns like `@theme`; use only what that theme’s schema allows for that section or block.

Never invent a `type` string that does not exist in the theme’s sections/schemas for that template context.

### Step 3 — Instance IDs and ordering

- **Section instance ids** (keys under `sections`) must be unique within the template. Use descriptive slugs: `header`, `main`, `footer`, `custom_liquid_banner`, etc. Avoid collisions when duplicating a section.
- **Block instance ids** must be unique **within** that section’s block tree (and consistent with nested scope rules).
- **`order`** must list every top-level section key you want rendered, in order.
- **`block_order`** must list every sibling block id under that parent, in order, and match the keys present in `blocks` at that level.

### Step 4 — Settings

- Only use setting keys that appear in the schema for that section or block.
- Use JSON types that match the schema: booleans as `true`/`false`, numbers as numbers, strings as strings, `select` values exactly as in schema options.
- If the schema defines a default and the user has no preference, omit the key or set the default explicitly — be consistent with existing theme files.
- Optional **`name`** on blocks may be a plain string or a translation reference (themes vary); follow existing patterns in sibling templates.

### Step 5 — Alternate templates

Alternate JSON templates follow the naming pattern `\<template\>.\<suffix\>.json` (e.g. `product.gift-card.json`). Structure is the same; only the filename and assignment in Admin differ.

## Validation checklist

Before finalizing:

- [ ] JSON is valid (parseable)
- [ ] Root has `sections`; top-level `order` exists and lists every section key to render
- [ ] Every section has a valid `type` for this theme
- [ ] Section and block **instance ids** are unique within their scope
- [ ] Every `block_order` matches the set of keys in the sibling `blocks` object at that level
- [ ] Each block `type` is allowed by its parent’s schema
- [ ] No unknown setting keys; values respect schema types and enums
- [ ] For nested blocks, both `blocks` and `block_order` are consistent at each level

## Error handling

| Problem | What to do |
|--------|------------|
| Unknown `type` | Search `sections/` and schema `blocks`; list close matches from the theme |
| Invalid nesting | Re-read parent `{% schema %}` blocks array; only use allowed `type` values |
| Schema unreadable / large | Prefer duplicating patterns from an existing working template in the same theme |
| Ambiguous user request | Ask which template file and which section instance should change |
| Theme uses generated `sections/` | Edit source per theme docs, then run the theme build |

## Further reading in this package

- [reference/shopify-json.md](reference/shopify-json.md) — official docs links and template filenames
- [examples/minimal-templates.md](examples/minimal-templates.md) — illustrative JSON (types are examples; always verify against the target theme)
