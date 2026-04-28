---
name: extract-real-estate-listing
description: Extract property address, price, room count, and features from a listing document into structured JSON for MLS and property platforms.
---

# Extract Real Estate Listing

Real estate agencies and property platforms use this recipe to digitize a listing document. Upload a property listing PDF and receive structured JSON with address, price, and property features — ready for your MLS integration or property database.

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
        "name": "listing.pdf",
        "url": "https://example.com/listings/listing.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "address",
          "type": "ADDRESS",
          "description": "Full property address"
        },
        {
          "name": "listing_price",
          "type": "CURRENCY_AMOUNT",
          "description": "Listed sale price"
        },
        {
          "name": "bedrooms",
          "type": "INTEGER",
          "description": "Number of bedrooms"
        },
        {
          "name": "bathrooms",
          "type": "INTEGER",
          "description": "Number of bathrooms"
        },
        {
          "name": "square_footage",
          "type": "INTEGER",
          "description": "Total square footage"
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
      name: "listing.pdf",
      url: "https://example.com/listings/listing.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "address",
        type: "ADDRESS",
        description: "Full property address",
      },
      {
        name: "listing_price",
        type: "CURRENCY_AMOUNT",
        description: "Listed sale price",
      },
      {
        name: "bedrooms",
        type: "INTEGER",
        description: "Number of bedrooms",
      },
      {
        name: "bathrooms",
        type: "INTEGER",
        description: "Number of bathrooms",
      },
      {
        name: "square_footage",
        type: "INTEGER",
        description: "Total square footage",
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
            "name": "listing.pdf",
            "url": "https://example.com/listings/listing.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "address",
                "type": "ADDRESS",
                "description": "Full property address",
            },
            {
                "name": "listing_price",
                "type": "CURRENCY_AMOUNT",
                "description": "Listed sale price",
            },
            {
                "name": "bedrooms",
                "type": "INTEGER",
                "description": "Number of bedrooms",
            },
            {
                "name": "bathrooms",
                "type": "INTEGER",
                "description": "Number of bathrooms",
            },
            {
                "name": "square_footage",
                "type": "INTEGER",
                "description": "Total square footage",
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
			il.NewFileFromURL("listing.pdf", "https://example.com/listings/listing.pdf"),
		},
		Schema: il.ExtractionSchema{
			"address": il.NewAddressFieldConfig(
				"address", "Full property address",
			),
			"listing_price": il.NewCurrencyAmountFieldConfig(
				"listing_price", "Listed sale price",
			),
			"bedrooms": il.NewIntegerFieldConfig(
				"bedrooms", "Number of bedrooms",
			),
			"bathrooms": il.NewIntegerFieldConfig(
				"bathrooms", "Number of bathrooms",
			),
			"square_footage": il.NewIntegerFieldConfig(
				"square_footage", "Total square footage",
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
  "name": "Extract Real Estate Listing",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Real Estate Listing\n\nReal estate agencies and property platforms use this recipe to digitize a listing document. Upload a property listing PDF and receive structured JSON with address, price, and property features \u2014 ready for your MLS integration or property database.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "87bfaa5a-7dfd-4b13-a506-fc47c2a1c137",
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
      "id": "17bf5410-88ba-4288-bdf0-c7d81249f3df",
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
      "id": "a9b0c1d2-e3f4-5678-abcd-789012345abc",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"address\",\"type\":\"ADDRESS\",\"description\":\"Full property address\"},{\"name\":\"listing_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Listed sale price\"},{\"name\":\"bedrooms\",\"type\":\"INTEGER\",\"description\":\"Number of bedrooms\"},{\"name\":\"bathrooms\",\"type\":\"INTEGER\",\"description\":\"Number of bathrooms\"},{\"name\":\"square_footage\",\"type\":\"INTEGER\",\"description\":\"Total square footage\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "listing.pdf",
              "fileUrl": "https://example.com/listings/listing.pdf"
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
      "id": "b0c1d2e3-f4a5-6789-bcde-890123456bcd",
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
Extract real estate listing data from the file at [file URL]. Use the extract_document tool with these fields:

- address (ADDRESS): Full property address
- listing_price (CURRENCY_AMOUNT): Listed sale price
- bedrooms (INTEGER): Number of bedrooms
- bathrooms (INTEGER): Number of bathrooms
- square_footage (INTEGER): Total square footage
```

### Response


```json
{
  "success": true,
  "data": {
    "address": {
      "value": {
        "street": "1847 Maple Drive",
        "city": "Austin",
        "region": "TX",
        "postal_code": "78704",
        "country": "US"
      },
      "confidence": 0.97,
      "citations": ["1847 Maple Drive, Austin, TX 78704"]
    },
    "listing_price": {
      "value": {
        "amount": "549,000.00",
        "currency": "USD"
      },
      "confidence": 0.98,
      "citations": ["Listed at $549,000"]
    },
    "bedrooms": {
      "value": 3,
      "confidence": 0.99,
      "citations": ["3 Bed"]
    },
    "bathrooms": {
      "value": 2,
      "confidence": 0.99,
      "citations": ["2 Bath"]
    },
    "square_footage": {
      "value": 1850,
      "confidence": 0.96,
      "citations": ["1,850 sq ft"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
