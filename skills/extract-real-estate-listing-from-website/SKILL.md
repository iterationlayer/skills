---
name: extract-real-estate-listing-from-website
description: Extract address, listing price, bedrooms, bathrooms, amenities, and agent contact details from a public listing page.
---

# Extract Real Estate Listing from Website

Real estate teams use this recipe to normalize public property listings into structured JSON for CRM and listing operations. Extract price, address, bedrooms, amenities, and agent details.

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
      "url": "https://example.com/listings/123-main-street"
    },
    "schema": {
        "fields": [
            {
                "name": "address",
                "type": "ADDRESS",
                "description": "Property address"
            },
            {
                "name": "listing_price",
                "type": "CURRENCY_AMOUNT",
                "description": "Advertised listing price"
            },
            {
                "name": "bedrooms",
                "type": "INTEGER",
                "description": "Number of bedrooms"
            },
            {
                "name": "bathrooms",
                "type": "DECIMAL",
                "description": "Number of bathrooms"
            },
            {
                "name": "amenities",
                "type": "ARRAY",
                "description": "Listed property amenities",
                "fields": [
                    {
                        "name": "amenity",
                        "type": "TEXT",
                        "description": "One amenity"
                    }
                ]
            },
            {
                "name": "agent_name",
                "type": "TEXT",
                "description": "Listing agent name"
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
    url: "https://example.com/listings/123-main-street",
  },
  schema: {
    "fields": [
      {
        "name": "address",
        "type": "ADDRESS",
        "description": "Property address"
      },
      {
        "name": "listing_price",
        "type": "CURRENCY_AMOUNT",
        "description": "Advertised listing price"
      },
      {
        "name": "bedrooms",
        "type": "INTEGER",
        "description": "Number of bedrooms"
      },
      {
        "name": "bathrooms",
        "type": "DECIMAL",
        "description": "Number of bathrooms"
      },
      {
        "name": "amenities",
        "type": "ARRAY",
        "description": "Listed property amenities",
        "fields": [
          {
            "name": "amenity",
            "type": "TEXT",
            "description": "One amenity"
          }
        ]
      },
      {
        "name": "agent_name",
        "type": "TEXT",
        "description": "Listing agent name"
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
        "url": "https://example.com/listings/123-main-street",
    },
    schema={
      "fields": [
        {
          "name": "address",
          "type": "ADDRESS",
          "description": "Property address"
        },
        {
          "name": "listing_price",
          "type": "CURRENCY_AMOUNT",
          "description": "Advertised listing price"
        },
        {
          "name": "bedrooms",
          "type": "INTEGER",
          "description": "Number of bedrooms"
        },
        {
          "name": "bathrooms",
          "type": "DECIMAL",
          "description": "Number of bathrooms"
        },
        {
          "name": "amenities",
          "type": "ARRAY",
          "description": "Listed property amenities",
          "fields": [
            {
              "name": "amenity",
              "type": "TEXT",
              "description": "One amenity"
            }
          ]
        },
        {
          "name": "agent_name",
          "type": "TEXT",
          "description": "Listing agent name"
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
		File: il.NewWebsiteFromURL("https://example.com/listings/123-main-street"),
		Schema: il.ExtractionSchema{
			"address": il.NewAddressFieldConfig("address", "Property address"),
			"listing_price": il.NewCurrencyAmountFieldConfig("listing_price", "Advertised listing price"),
			"bedrooms": il.NewIntegerFieldConfig("bedrooms", "Number of bedrooms"),
			"bathrooms": il.NewDecimalFieldConfig("bathrooms", "Number of bathrooms"),
			"amenities": il.NewArrayFieldConfig(
	"amenities",
	"Listed property amenities",
	[]il.FieldConfig{
	il.NewTextFieldConfig("amenity", "One amenity"),
	},
),
			"agent_name": il.NewTextFieldConfig("agent_name", "Listing agent name"),
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
  "name": "Extract Real Estate Listing from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Real Estate Listing from Website\n\nReal estate teams use this recipe to normalize public property listings into structured JSON for CRM and listing operations. Extract price, address, bedrooms, amenities, and agent details.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "38115878-a4dd-5005-8e9f-fedebf187caa",
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
      "id": "45cbbfc1-4872-5df8-bf1f-10f30fd09473",
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
      "id": "4eb4e182-3f61-5ea4-a79c-d7bff59d3c24",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/listings/123-main-street",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"address\",\"type\":\"ADDRESS\",\"description\":\"Property address\"},{\"name\":\"listing_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Advertised listing price\"},{\"name\":\"bedrooms\",\"type\":\"INTEGER\",\"description\":\"Number of bedrooms\"},{\"name\":\"bathrooms\",\"type\":\"DECIMAL\",\"description\":\"Number of bathrooms\"},{\"name\":\"amenities\",\"type\":\"ARRAY\",\"description\":\"Listed property amenities\",\"fields\":[{\"name\":\"amenity\",\"type\":\"TEXT\",\"description\":\"One amenity\"}]},{\"name\":\"agent_name\",\"type\":\"TEXT\",\"description\":\"Listing agent name\"}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "f75d51a1-4ef2-57a3-963b-9dc93dd64966",
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

- address (ADDRESS): Property address
- listing_price (CURRENCY_AMOUNT): Advertised listing price
- bedrooms (INTEGER): Number of bedrooms
- bathrooms (DECIMAL): Number of bathrooms
- amenities (ARRAY): Listed property amenities. Each item has amenity (TEXT)
- agent_name (TEXT): Listing agent name
```

### Response


```json
{
  "success": true,
  "data": {
    "address": {
      "type": "ADDRESS",
      "value": "123 Main Street, Berlin, Germany",
      "confidence": 0.96,
      "citations": [
        "123 Main Street"
      ],
      "source": "website.html"
    },
    "listing_price": {
      "type": "CURRENCY_AMOUNT",
      "value": 685000,
      "confidence": 0.94,
      "citations": [
        "\u20ac685,000"
      ],
      "source": "website.html"
    },
    "bedrooms": {
      "type": "INTEGER",
      "value": 3,
      "confidence": 0.95,
      "citations": [
        "3 bedrooms"
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
