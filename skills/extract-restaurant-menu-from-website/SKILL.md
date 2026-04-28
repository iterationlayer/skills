---
name: extract-restaurant-menu-from-website
description: Extract menu item names, prices, descriptions, and dietary notes from a public restaurant menu page.
---

# Extract Restaurant Menu from Website

Restaurant operators and delivery platforms use this recipe to convert public menu pages into structured menu data. Extract dishes, prices, descriptions, categories, and dietary notes from one page.

## APIs Used

Website Extraction (1 credit per website extraction)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/website-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "url": "https://example.com/menu"
    },
    "schema": {
        "fields": [
            {
                "name": "menu_items",
                "type": "ARRAY",
                "description": "Menu items listed on the page",
                "fields": [
                    {
                        "name": "item_name",
                        "type": "TEXT",
                        "description": "Menu item name"
                    },
                    {
                        "name": "category",
                        "type": "TEXT",
                        "description": "Menu category"
                    },
                    {
                        "name": "price",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Menu item price"
                    },
                    {
                        "name": "description",
                        "type": "TEXTAREA",
                        "description": "Menu item description"
                    },
                    {
                        "name": "dietary_notes",
                        "type": "TEXT",
                        "description": "Dietary labels such as vegan or gluten-free"
                    }
                ]
            }
        ]
    }
}'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractWebsite({
  file: {
    type: "url",
    url: "https://example.com/menu",
  },
  schema: {
    "fields": [
      {
        "name": "menu_items",
        "type": "ARRAY",
        "description": "Menu items listed on the page",
        "fields": [
          {
            "name": "item_name",
            "type": "TEXT",
            "description": "Menu item name"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Menu category"
          },
          {
            "name": "price",
            "type": "CURRENCY_AMOUNT",
            "description": "Menu item price"
          },
          {
            "name": "description",
            "type": "TEXTAREA",
            "description": "Menu item description"
          },
          {
            "name": "dietary_notes",
            "type": "TEXT",
            "description": "Dietary labels such as vegan or gluten-free"
          }
        ]
      }
    ]
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_website(
    file={
        "type": "url",
        "url": "https://example.com/menu",
    },
    schema={
      "fields": [
        {
          "name": "menu_items",
          "type": "ARRAY",
          "description": "Menu items listed on the page",
          "fields": [
            {
              "name": "item_name",
              "type": "TEXT",
              "description": "Menu item name"
            },
            {
              "name": "category",
              "type": "TEXT",
              "description": "Menu category"
            },
            {
              "name": "price",
              "type": "CURRENCY_AMOUNT",
              "description": "Menu item price"
            },
            {
              "name": "description",
              "type": "TEXTAREA",
              "description": "Menu item description"
            },
            {
              "name": "dietary_notes",
              "type": "TEXT",
              "description": "Dietary labels such as vegan or gluten-free"
            }
          ]
        }
      ]
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractWebsite(il.ExtractWebsiteRequest{
		File: il.NewWebsiteFromURL("https://example.com/menu"),
		Schema: il.ExtractionSchema{
			"menu_items": il.NewArrayFieldConfig(
	"menu_items",
	"Menu items listed on the page",
	[]il.FieldConfig{
	il.NewTextFieldConfig("item_name", "Menu item name"),
	il.NewTextFieldConfig("category", "Menu category"),
	il.NewCurrencyAmountFieldConfig("price", "Menu item price"),
	il.NewTextareaFieldConfig("description", "Menu item description"),
	il.NewTextFieldConfig("dietary_notes", "Dietary labels such as vegan or gluten-free"),
	},
),
		},
	})
	if err != nil {
		panic(err)
	}

	_ = result
}
```

```n8n
{
  "name": "Extract Restaurant Menu from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Restaurant Menu from Website\n\nRestaurant operators and delivery platforms use this recipe to convert public menu pages into structured menu data. Extract dishes, prices, descriptions, categories, and dietary notes from one page.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        200,
        40
      ],
      "id": "fc2184f4-0e68-5159-9780-77c40c7b98ef",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Website Data\nResource: **Website Extraction**\n\nConfigure the public website URL and schema below, then connect your credentials.",
        "height": 160,
        "width": 320,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "ee1432e8-a3b8-558f-8fbb-6e780a05ec9d",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "a52c9351-cb97-5642-9a0e-1bf92f5d4826",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/menu",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"menu_items\",\"type\":\"ARRAY\",\"description\":\"Menu items listed on the page\",\"fields\":[{\"name\":\"item_name\",\"type\":\"TEXT\",\"description\":\"Menu item name\"},{\"name\":\"category\",\"type\":\"TEXT\",\"description\":\"Menu category\"},{\"name\":\"price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Menu item price\"},{\"name\":\"description\",\"type\":\"TEXTAREA\",\"description\":\"Menu item description\"},{\"name\":\"dietary_notes\",\"type\":\"TEXT\",\"description\":\"Dietary labels such as vegan or gluten-free\"}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "2552c7e6-c739-5ca5-851d-fe90c2c4ac95",
      "name": "Extract Website Data",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    }
  ],
  "connections": {
    "Manual Trigger": {
      "main": [
        [
          {
            "node": "Extract Website Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "settings": {
    "executionOrder": "v1"
  }
}
```

```prompt
Extract structured data from the public website page at [website URL]. Use the extract_website tool with this schema:

- menu_items (ARRAY): Menu items listed on the page. Each item has item_name (TEXT), category (TEXT), price (CURRENCY_AMOUNT), description (TEXTAREA), dietary_notes (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "menu_items": {
      "type": "ARRAY",
      "value": [
        {
          "item_name": "Roasted Vegetable Bowl",
          "category": "Mains",
          "price": 14.5,
          "description": "Seasonal vegetables with lemon tahini",
          "dietary_notes": "Vegan"
        }
      ],
      "confidence": 0.92,
      "citations": [
        "Roasted Vegetable Bowl - Vegan - 14.50"
      ],
      "source": "website.html"
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
