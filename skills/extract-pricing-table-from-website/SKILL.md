---
name: extract-pricing-table-from-website
description: Extract plan names, prices, billing periods, and feature lists from a public pricing page.
---

# Extract Pricing Table from Website

SaaS teams and agencies use this recipe to turn competitor pricing pages into structured JSON. Extract plan names, prices, billing periods, and feature lists for monitoring, quoting, and sales enablement.

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
      "url": "https://example.com/pricing"
    },
    "schema": {
        "fields": [
            {
                "name": "plans",
                "type": "ARRAY",
                "description": "Pricing plans shown on the page",
                "fields": [
                    {
                        "name": "plan_name",
                        "type": "TEXT",
                        "description": "Name of the pricing plan"
                    },
                    {
                        "name": "monthly_price",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Advertised monthly price"
                    },
                    {
                        "name": "billing_period",
                        "type": "TEXT",
                        "description": "Billing period such as monthly or annual"
                    },
                    {
                        "name": "features",
                        "type": "ARRAY",
                        "description": "Included plan features",
                        "fields": [
                            {
                                "name": "feature",
                                "type": "TEXT",
                                "description": "Feature label"
                            }
                        ]
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
    url: "https://example.com/pricing",
  },
  schema: {
    "fields": [
      {
        "name": "plans",
        "type": "ARRAY",
        "description": "Pricing plans shown on the page",
        "fields": [
          {
            "name": "plan_name",
            "type": "TEXT",
            "description": "Name of the pricing plan"
          },
          {
            "name": "monthly_price",
            "type": "CURRENCY_AMOUNT",
            "description": "Advertised monthly price"
          },
          {
            "name": "billing_period",
            "type": "TEXT",
            "description": "Billing period such as monthly or annual"
          },
          {
            "name": "features",
            "type": "ARRAY",
            "description": "Included plan features",
            "fields": [
              {
                "name": "feature",
                "type": "TEXT",
                "description": "Feature label"
              }
            ]
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
        "url": "https://example.com/pricing",
    },
    schema={
      "fields": [
        {
          "name": "plans",
          "type": "ARRAY",
          "description": "Pricing plans shown on the page",
          "fields": [
            {
              "name": "plan_name",
              "type": "TEXT",
              "description": "Name of the pricing plan"
            },
            {
              "name": "monthly_price",
              "type": "CURRENCY_AMOUNT",
              "description": "Advertised monthly price"
            },
            {
              "name": "billing_period",
              "type": "TEXT",
              "description": "Billing period such as monthly or annual"
            },
            {
              "name": "features",
              "type": "ARRAY",
              "description": "Included plan features",
              "fields": [
                {
                  "name": "feature",
                  "type": "TEXT",
                  "description": "Feature label"
                }
              ]
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
		File: il.NewWebsiteFromURL("https://example.com/pricing"),
		Schema: il.ExtractionSchema{
			"plans": il.NewArrayFieldConfig(
	"plans",
	"Pricing plans shown on the page",
	[]il.FieldConfig{
	il.NewTextFieldConfig("plan_name", "Name of the pricing plan"),
	il.NewCurrencyAmountFieldConfig("monthly_price", "Advertised monthly price"),
	il.NewTextFieldConfig("billing_period", "Billing period such as monthly or annual"),
	il.NewArrayFieldConfig(
		"features",
		"Included plan features",
		[]il.FieldConfig{
		il.NewTextFieldConfig("feature", "Feature label"),
		},
	),
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
  "name": "Extract Pricing Table from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Pricing Table from Website\n\nSaaS teams and agencies use this recipe to turn competitor pricing pages into structured JSON. Extract plan names, prices, billing periods, and feature lists for monitoring, quoting, and sales enablement.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "88888234-d21f-5db9-9db7-98c92a323806",
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
      "id": "589850c3-62b4-5307-9fa3-b2d1c64ee4b9",
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
      "id": "2bfec09e-e8cb-5c7f-92b3-a9723e465d98",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/pricing",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"plans\",\"type\":\"ARRAY\",\"description\":\"Pricing plans shown on the page\",\"fields\":[{\"name\":\"plan_name\",\"type\":\"TEXT\",\"description\":\"Name of the pricing plan\"},{\"name\":\"monthly_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Advertised monthly price\"},{\"name\":\"billing_period\",\"type\":\"TEXT\",\"description\":\"Billing period such as monthly or annual\"},{\"name\":\"features\",\"type\":\"ARRAY\",\"description\":\"Included plan features\",\"fields\":[{\"name\":\"feature\",\"type\":\"TEXT\",\"description\":\"Feature label\"}]}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "81db5364-c171-568b-9351-e2aa9e82ba98",
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

- plans (ARRAY): Pricing plans shown on the page. Each item has plan_name (TEXT), monthly_price (CURRENCY_AMOUNT), billing_period (TEXT), features (ARRAY)
```

### Response


```json
{
  "success": true,
  "data": {
    "plans": {
      "type": "ARRAY",
      "value": [
        {
          "plan_name": "Startup",
          "monthly_price": 119,
          "billing_period": "monthly",
          "features": [
            "10,000 pages",
            "Team workspace",
            "Webhook exports"
          ]
        }
      ],
      "confidence": 0.94,
      "citations": [
        "Startup $119/mo includes 10,000 pages"
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
