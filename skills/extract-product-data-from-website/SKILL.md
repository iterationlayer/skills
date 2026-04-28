---
name: extract-product-data-from-website
description: Extract product name, price, availability, description, and specifications from a public product page.
---

# Extract Product Data from Website

E-commerce teams use this recipe to normalize public product pages into catalog records. Pull product names, prices, availability, and specifications without writing selectors for every storefront.

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
      "url": "https://example.com/products/ergonomic-chair"
    },
    "schema": {
        "fields": [
            {
                "name": "product_name",
                "type": "TEXT",
                "description": "Product name shown on the page"
            },
            {
                "name": "price",
                "type": "CURRENCY_AMOUNT",
                "description": "Current product price"
            },
            {
                "name": "availability",
                "type": "TEXT",
                "description": "Stock or availability status"
            },
            {
                "name": "description",
                "type": "TEXTAREA",
                "description": "Product description"
            },
            {
                "name": "specifications",
                "type": "ARRAY",
                "description": "Product specification rows",
                "fields": [
                    {
                        "name": "specification",
                        "type": "TEXT",
                        "description": "One product specification"
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
    url: "https://example.com/products/ergonomic-chair",
  },
  schema: {
    "fields": [
      {
        "name": "product_name",
        "type": "TEXT",
        "description": "Product name shown on the page"
      },
      {
        "name": "price",
        "type": "CURRENCY_AMOUNT",
        "description": "Current product price"
      },
      {
        "name": "availability",
        "type": "TEXT",
        "description": "Stock or availability status"
      },
      {
        "name": "description",
        "type": "TEXTAREA",
        "description": "Product description"
      },
      {
        "name": "specifications",
        "type": "ARRAY",
        "description": "Product specification rows",
        "fields": [
          {
            "name": "specification",
            "type": "TEXT",
            "description": "One product specification"
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
        "url": "https://example.com/products/ergonomic-chair",
    },
    schema={
      "fields": [
        {
          "name": "product_name",
          "type": "TEXT",
          "description": "Product name shown on the page"
        },
        {
          "name": "price",
          "type": "CURRENCY_AMOUNT",
          "description": "Current product price"
        },
        {
          "name": "availability",
          "type": "TEXT",
          "description": "Stock or availability status"
        },
        {
          "name": "description",
          "type": "TEXTAREA",
          "description": "Product description"
        },
        {
          "name": "specifications",
          "type": "ARRAY",
          "description": "Product specification rows",
          "fields": [
            {
              "name": "specification",
              "type": "TEXT",
              "description": "One product specification"
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
		File: il.NewWebsiteFromURL("https://example.com/products/ergonomic-chair"),
		Schema: il.ExtractionSchema{
			"product_name": il.NewTextFieldConfig("product_name", "Product name shown on the page"),
			"price": il.NewCurrencyAmountFieldConfig("price", "Current product price"),
			"availability": il.NewTextFieldConfig("availability", "Stock or availability status"),
			"description": il.NewTextareaFieldConfig("description", "Product description"),
			"specifications": il.NewArrayFieldConfig(
	"specifications",
	"Product specification rows",
	[]il.FieldConfig{
	il.NewTextFieldConfig("specification", "One product specification"),
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
  "name": "Extract Product Data from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Product Data from Website\n\nE-commerce teams use this recipe to normalize public product pages into catalog records. Pull product names, prices, availability, and specifications without writing selectors for every storefront.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "2774c24a-7717-555e-8a84-7f41a077deec",
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
      "id": "03149102-7d1c-52ee-bcdb-42ef2fed5bf6",
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
      "id": "635f46d7-7f37-5134-845b-90924dde6d3c",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/products/ergonomic-chair",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"product_name\",\"type\":\"TEXT\",\"description\":\"Product name shown on the page\"},{\"name\":\"price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Current product price\"},{\"name\":\"availability\",\"type\":\"TEXT\",\"description\":\"Stock or availability status\"},{\"name\":\"description\",\"type\":\"TEXTAREA\",\"description\":\"Product description\"},{\"name\":\"specifications\",\"type\":\"ARRAY\",\"description\":\"Product specification rows\",\"fields\":[{\"name\":\"specification\",\"type\":\"TEXT\",\"description\":\"One product specification\"}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "46217ca0-001a-572a-bfe3-61ec1221d4bb",
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

- product_name (TEXT): Product name shown on the page
- price (CURRENCY_AMOUNT): Current product price
- availability (TEXT): Stock or availability status
- description (TEXTAREA): Product description
- specifications (ARRAY): Product specification rows. Each item has specification (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "product_name": {
      "type": "TEXT",
      "value": "Ergonomic Mesh Chair",
      "confidence": 0.97,
      "citations": [
        "Ergonomic Mesh Chair"
      ],
      "source": "website.html"
    },
    "price": {
      "type": "CURRENCY_AMOUNT",
      "value": 249,
      "confidence": 0.94,
      "citations": [
        "$249"
      ],
      "source": "website.html"
    },
    "availability": {
      "type": "TEXT",
      "value": "In stock",
      "confidence": 0.93,
      "citations": [
        "In stock"
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
