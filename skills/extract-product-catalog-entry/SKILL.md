---
name: extract-product-catalog-entry
description: Extract product name, SKU, price, and specifications from a catalog document into structured JSON for e-commerce workflows.
---

# Extract Product Catalog Entry

E-commerce teams and marketplace operators use this recipe to digitize a supplier catalog entry. Upload a catalog PDF and receive structured JSON with product name, SKU, price, and description — ready for import into your product database or storefront.

## APIs Used

Document Extraction (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "catalog.pdf",
        "url": "https://example.com/catalogs/supplier-catalog.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "products",
          "type": "ARRAY",
          "description": "List of products in the catalog",
          "fields": [
            {
              "name": "name",
              "type": "TEXT",
              "description": "Product name"
            },
            {
              "name": "sku",
              "type": "TEXT",
              "description": "Stock keeping unit identifier"
            },
            {
              "name": "price",
              "type": "CURRENCY_AMOUNT",
              "description": "Product price"
            },
            {
              "name": "description",
              "type": "TEXTAREA",
              "description": "Product description"
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

const result = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "catalog.pdf",
      url: "https://example.com/catalogs/supplier-catalog.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "products",
        type: "ARRAY",
        description: "List of products in the catalog",
        fields: [
          {
            name: "name",
            type: "TEXT",
            description: "Product name",
          },
          {
            name: "sku",
            type: "TEXT",
            description: "Stock keeping unit identifier",
          },
          {
            name: "price",
            type: "CURRENCY_AMOUNT",
            description: "Product price",
          },
          {
            name: "description",
            type: "TEXTAREA",
            description: "Product description",
          },
        ],
      },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "catalog.pdf",
            "url": "https://example.com/catalogs/supplier-catalog.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "products",
                "type": "ARRAY",
                "description": "List of products in the catalog",
                "fields": [
                    {
                        "name": "name",
                        "type": "TEXT",
                        "description": "Product name",
                    },
                    {
                        "name": "sku",
                        "type": "TEXT",
                        "description": "Stock keeping unit identifier",
                    },
                    {
                        "name": "price",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Product price",
                    },
                    {
                        "name": "description",
                        "type": "TEXTAREA",
                        "description": "Product description",
                    },
                ],
            },
        ]
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL(
				"catalog.pdf",
				"https://example.com/catalogs/supplier-catalog.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"products": il.NewArrayFieldConfig(
				"products",
				"List of products in the catalog",
				il.ExtractionSchema{
					"name": il.NewTextFieldConfig(
						"name", "Product name",
					),
					"sku": il.NewTextFieldConfig(
						"sku", "Stock keeping unit identifier",
					),
					"price": il.NewCurrencyAmountFieldConfig(
						"price", "Product price",
					),
					"description": il.NewTextFieldConfig(
						"description", "Product description",
					),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

}
```

```n8n
{
  "name": "Extract Product Catalog Entry",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Product Catalog Entry\n\nE-commerce teams and marketplace operators use this recipe to digitize a supplier catalog entry. Upload a catalog PDF and receive structured JSON with product name, SKU, price, and description \u2014 ready for import into your product database or storefront.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "6be80463-1d4d-4c67-a929-9c7f382a599c",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "c23ebb20-4aef-4e8a-93d8-a12acaf9de39",
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
      "id": "c5d6e7f8-a9b0-1234-cdef-345678901cde",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"products\",\"type\":\"ARRAY\",\"description\":\"List of products in the catalog\",\"fields\":[{\"name\":\"name\",\"type\":\"TEXT\",\"description\":\"Product name\"},{\"name\":\"sku\",\"type\":\"TEXT\",\"description\":\"Stock keeping unit identifier\"},{\"name\":\"price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Product price\"},{\"name\":\"description\",\"type\":\"TEXTAREA\",\"description\":\"Product description\"}]}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "catalog.pdf",
              "fileUrl": "https://example.com/catalogs/supplier-catalog.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "d6e7f8a9-b0c1-2345-defa-456789012def",
      "name": "Extract Data",
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
            "node": "Extract Data",
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
Extract product catalog entries from the file at [file URL]. Use the extract_document tool with these fields:

- products (ARRAY): Each with:
  - name (TEXT): Product name
  - sku (TEXT): Stock keeping unit identifier
  - price (CURRENCY_AMOUNT): Product price
  - description (TEXTAREA): Product description
```

### Response


```json
{
  "success": true,
  "data": {
    "products": {
      "value": [
        {
          "name": {
            "value": "ProGrip Wireless Mouse",
            "confidence": 0.97,
            "citations": ["ProGrip Wireless Mouse"]
          },
          "sku": {
            "value": "PG-WM-2026",
            "confidence": 0.99,
            "citations": ["SKU: PG-WM-2026"]
          },
          "price": {
            "value": {
              "amount": "34.99",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$34.99"]
          },
          "description": {
            "value": "Ergonomic wireless mouse with 2.4GHz connectivity, 1600 DPI sensor, and rechargeable battery lasting up to 90 days.",
            "confidence": 0.94,
            "citations": ["Ergonomic wireless mouse with 2.4GHz connectivity"]
          }
        },
        {
          "name": {
            "value": "UltraType Mechanical Keyboard",
            "confidence": 0.96,
            "citations": ["UltraType Mechanical Keyboard"]
          },
          "sku": {
            "value": "UT-MK-1050",
            "confidence": 0.99,
            "citations": ["SKU: UT-MK-1050"]
          },
          "price": {
            "value": {
              "amount": "89.00",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["$89.00"]
          },
          "description": {
            "value": "Full-size mechanical keyboard with Cherry MX Brown switches, RGB backlighting, and detachable USB-C cable.",
            "confidence": 0.93,
            "citations": [
              "Full-size mechanical keyboard with Cherry MX Brown switches"
            ]
          }
        }
      ],
      "confidence": 0.95,
      "citations": []
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
