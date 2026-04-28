---
name: extract-supplier-catalog-from-website
description: Extract SKUs, product names, unit prices, availability, and minimum order quantities from a supplier catalog page.
---

# Extract Supplier Catalog from Website

Procurement teams use this recipe to turn public supplier catalog pages into structured records. Extract SKUs, unit prices, stock status, and minimum order quantities for sourcing workflows.

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
      "url": "https://example.com/catalog/industrial-fasteners"
    },
    "schema": {
        "fields": [
            {
                "name": "catalog_items",
                "type": "ARRAY",
                "description": "Supplier catalog items",
                "fields": [
                    {
                        "name": "sku",
                        "type": "TEXT",
                        "description": "Supplier SKU"
                    },
                    {
                        "name": "product_name",
                        "type": "TEXT",
                        "description": "Product name"
                    },
                    {
                        "name": "unit_price",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Unit price"
                    },
                    {
                        "name": "availability",
                        "type": "TEXT",
                        "description": "Availability status"
                    },
                    {
                        "name": "minimum_order_quantity",
                        "type": "INTEGER",
                        "description": "Minimum order quantity"
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
    url: "https://example.com/catalog/industrial-fasteners",
  },
  schema: {
    "fields": [
      {
        "name": "catalog_items",
        "type": "ARRAY",
        "description": "Supplier catalog items",
        "fields": [
          {
            "name": "sku",
            "type": "TEXT",
            "description": "Supplier SKU"
          },
          {
            "name": "product_name",
            "type": "TEXT",
            "description": "Product name"
          },
          {
            "name": "unit_price",
            "type": "CURRENCY_AMOUNT",
            "description": "Unit price"
          },
          {
            "name": "availability",
            "type": "TEXT",
            "description": "Availability status"
          },
          {
            "name": "minimum_order_quantity",
            "type": "INTEGER",
            "description": "Minimum order quantity"
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
        "url": "https://example.com/catalog/industrial-fasteners",
    },
    schema={
      "fields": [
        {
          "name": "catalog_items",
          "type": "ARRAY",
          "description": "Supplier catalog items",
          "fields": [
            {
              "name": "sku",
              "type": "TEXT",
              "description": "Supplier SKU"
            },
            {
              "name": "product_name",
              "type": "TEXT",
              "description": "Product name"
            },
            {
              "name": "unit_price",
              "type": "CURRENCY_AMOUNT",
              "description": "Unit price"
            },
            {
              "name": "availability",
              "type": "TEXT",
              "description": "Availability status"
            },
            {
              "name": "minimum_order_quantity",
              "type": "INTEGER",
              "description": "Minimum order quantity"
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
		File: il.NewWebsiteFromURL("https://example.com/catalog/industrial-fasteners"),
		Schema: il.ExtractionSchema{
			"catalog_items": il.NewArrayFieldConfig(
	"catalog_items",
	"Supplier catalog items",
	[]il.FieldConfig{
	il.NewTextFieldConfig("sku", "Supplier SKU"),
	il.NewTextFieldConfig("product_name", "Product name"),
	il.NewCurrencyAmountFieldConfig("unit_price", "Unit price"),
	il.NewTextFieldConfig("availability", "Availability status"),
	il.NewIntegerFieldConfig("minimum_order_quantity", "Minimum order quantity"),
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
  "name": "Extract Supplier Catalog from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Supplier Catalog from Website\n\nProcurement teams use this recipe to turn public supplier catalog pages into structured records. Extract SKUs, unit prices, stock status, and minimum order quantities for sourcing workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "b223f26d-5e5b-55b7-8533-2224bd81f7d2",
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
      "id": "97a3f12c-71c8-5662-bbe4-c2da7f216991",
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
      "id": "fbd31168-1ff3-5c5c-bca7-beb0d940b9f2",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/catalog/industrial-fasteners",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"catalog_items\",\"type\":\"ARRAY\",\"description\":\"Supplier catalog items\",\"fields\":[{\"name\":\"sku\",\"type\":\"TEXT\",\"description\":\"Supplier SKU\"},{\"name\":\"product_name\",\"type\":\"TEXT\",\"description\":\"Product name\"},{\"name\":\"unit_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Unit price\"},{\"name\":\"availability\",\"type\":\"TEXT\",\"description\":\"Availability status\"},{\"name\":\"minimum_order_quantity\",\"type\":\"INTEGER\",\"description\":\"Minimum order quantity\"}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "e0867029-09ea-535a-87bd-85324e11a6b6",
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

- catalog_items (ARRAY): Supplier catalog items. Each item has sku (TEXT), product_name (TEXT), unit_price (CURRENCY_AMOUNT), availability (TEXT), minimum_order_quantity (INTEGER)
```

### Response


```json
{
  "success": true,
  "data": {
    "catalog_items": {
      "type": "ARRAY",
      "value": [
        {
          "sku": "FAST- M8-40",
          "product_name": "M8 stainless hex bolt",
          "unit_price": 0.18,
          "availability": "Ships in 2 days",
          "minimum_order_quantity": 500
        }
      ],
      "confidence": 0.9,
      "citations": [
        "M8 stainless hex bolt SKU FAST-M8-40 MOQ 500"
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
