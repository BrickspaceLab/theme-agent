# Slab

This file holds bundled knowledge for the Slab Shopify theme. 

## Block reference


| Role              | Typical block types                                                                                                                                  |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Layout**        | `layout__grid` (children: `_g__grid-item`), `layout__flex` (`_g__flex-item`), `layout__slider` (`_g__slider-item`), `layout__inline`, `g__container` |
| **Content**       | `image`, `g__button`, `richtext`, `g__product-card`, `g__article-card`, `g__collection-card`                                                         |
| **Item wrappers** | `_g__grid-item`, `_g__flex-item`, `_g__slider-item`                                                                                                  |


## Examples

### 3-column grid with image and button per column

**Structure:**

```
layout__grid (row_desktop: 3)
  └── _g__grid-item (×3)
      └── image
      └── g__button
```

**Illustrative nested block JSON** (verify types/settings in the target theme):

```json
{
  "type": "layout__grid",
  "settings": {
    "row_desktop": 3,
    "row_mobile": 1,
    "gap_size": "default",
    "enable_x_padding": false
  },
  "blocks": {
    "grid_item_1": {
      "type": "_g__grid-item",
      "blocks": {
        "image_1": {
          "type": "image",
          "settings": {
            "enable_x_padding": false,
            "enable_t_padding": false,
            "enable_b_padding": false
          }
        },
        "button_1": {
          "type": "g__button",
          "settings": {
            "url": "/collections/all",
            "enable_x_padding": false
          }
        }
      },
      "block_order": ["image_1", "button_1"]
    }
  },
  "block_order": ["grid_item_1", "grid_item_2", "grid_item_3"]
}
```

### Flex row with multiple items

**Structure:**

```
layout__flex (direction: flex-row)
  └── _g__flex-item (×3)
      └── image
      └── richtext
```

**Illustrative nested block JSON** (verify types/settings in the target theme):

```json
{
  "type": "layout__flex",
  "settings": {
    "direction": "flex-row",
    "gap_size": "default",
    "enable_x_padding": false
  },
  "blocks": {
    "flex_item_1": {
      "type": "_g__flex-item",
      "settings": {
        "width_desktop": "1/3",
        "width_mobile": "1/1"
      },
      "blocks": {
        "image_1": { "type": "image", "settings": {} },
        "richtext_1": { "type": "richtext", "settings": {} }
      },
      "block_order": ["image_1", "richtext_1"]
    }
  },
  "block_order": ["flex_item_1", "flex_item_2", "flex_item_3"]
}
```

### Nested containers

**Structure:**

```
_g__grid-item
  └── g__container (optional)
      └── image
      └── richtext
      └── g__button
```

**Illustrative nested block JSON** (verify types/settings in the target theme):

```json
{
  "type": "_g__grid-item",
  "blocks": {
    "container_1": {
      "type": "g__container",
      "blocks": {
        "image_1": { "type": "image", "settings": {} },
        "richtext_1": { "type": "richtext", "settings": {} },
        "button_1": {
          "type": "g__button",
          "settings": { "url": "/collections/all" }
        }
      },
      "block_order": ["image_1", "richtext_1", "button_1"]
    }
  },
  "block_order": ["container_1"]
}
```

## Sources

Block type patterns and schema structure in this file are derived from theme-specific inspection. For the underlying Shopify platform concepts, see:

- [Theme blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks) — how `blocks/*.liquid` files work, `@theme` reference, nesting rules
- [Section schema](https://shopify.dev/docs/storefronts/themes/architecture/sections/section-schema) — settings types, `blocks` array, `presets`, limits

