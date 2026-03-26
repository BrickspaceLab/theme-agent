# Minimal JSON template examples

These examples use **illustrative** section and block types (similar to common Dawn-style names). **Replace** `type` values and settings with those defined in your theme’s `sections/` and `{% schema %}` before use.

## Product template

```json
{
  "sections": {
    "header": {
      "type": "header"
    },
    "main": {
      "type": "main-product",
      "settings": {
        "show_vendor": true,
        "media_size": "medium"
      },
      "blocks": {
        "title": {
          "type": "title",
          "settings": {}
        },
        "price": {
          "type": "price",
          "settings": {
            "show_compare_at": true
          }
        },
        "variant_picker": {
          "type": "variant_picker",
          "settings": {
            "picker_type": "dropdown"
          }
        }
      },
      "block_order": ["title", "price", "variant_picker"]
    },
    "footer": {
      "type": "footer"
    }
  },
  "order": ["header", "main", "footer"]
}
```

## Collection template

```json
{
  "sections": {
    "header": {
      "type": "header"
    },
    "banner": {
      "type": "collection-banner",
      "settings": {
        "show_collection_description": true,
        "show_collection_image": false
      }
    },
    "main": {
      "type": "main-collection-product-grid",
      "settings": {
        "products_per_page": 24,
        "columns_desktop": 4,
        "columns_mobile": "2"
      }
    },
    "footer": {
      "type": "footer"
    }
  },
  "order": ["header", "banner", "main", "footer"]
}
```

## Static page template

```json
{
  "sections": {
    "header": {
      "type": "header"
    },
    "main": {
      "type": "main-page",
      "settings": {
        "padding_top": 28,
        "padding_bottom": 28
      }
    },
    "footer": {
      "type": "footer"
    }
  },
  "order": ["header", "main", "footer"]
}
```

## Notes

- Some themes use different section `type` strings (e.g. `main-product` vs custom names).
- `columns_mobile` may be a number or string depending on schema; match the theme.
- Omit `settings` or keys when defaults apply; the examples set a few fields for clarity.
