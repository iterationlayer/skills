---
name: extract-shipping-request-data
description: Extract requestor, ship-from, ship-to, service, package, and handling instructions from shipping requests.
---

# Extract Shipping Request Data

Operations teams use this recipe to extract internal shipping request fields into structured records for carrier booking, warehouse fulfillment, and label generation.

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
      "name": "shipping-request.pdf",
      "url": "https://example.com/documents/shipping-request-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "request_number",
        "type": "TEXT",
        "description": "Shipping request number"
      },
      {
        "name": "requestor_name",
        "type": "TEXT",
        "description": "Requestor name"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "ship_from",
        "type": "ADDRESS",
        "description": "Ship-from address"
      },
      {
        "name": "ship_to",
        "type": "ADDRESS",
        "description": "Ship-to address"
      },
      {
        "name": "requested_ship_date",
        "type": "DATE",
        "description": "Requested ship date"
      },
      {
        "name": "service_level",
        "type": "TEXT",
        "description": "Requested carrier service level"
      },
      {
        "name": "packages",
        "type": "ARRAY",
        "description": "Packages requested for shipment",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Package contents or description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Package quantity"
          },
          {
            "name": "weight_kg",
            "type": "DECIMAL",
            "description": "Package weight in kilograms"
          },
          {
            "name": "dimensions",
            "type": "TEXT",
            "description": "Package dimensions"
          }
        ]
      },
      {
        "name": "special_handling",
        "type": "TEXT",
        "description": "Fragile, dangerous goods, refrigeration, insurance, or other handling instructions"
      }
    ]
  }
}'
```

```typescript
import { IterationLayer } from "iterationlayer";

const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractDocument({
  "files": [
    {
      "type": "url",
      "name": "shipping-request.pdf",
      "url": "https://example.com/documents/shipping-request-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "request_number",
        "type": "TEXT",
        "description": "Shipping request number"
      },
      {
        "name": "requestor_name",
        "type": "TEXT",
        "description": "Requestor name"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "ship_from",
        "type": "ADDRESS",
        "description": "Ship-from address"
      },
      {
        "name": "ship_to",
        "type": "ADDRESS",
        "description": "Ship-to address"
      },
      {
        "name": "requested_ship_date",
        "type": "DATE",
        "description": "Requested ship date"
      },
      {
        "name": "service_level",
        "type": "TEXT",
        "description": "Requested carrier service level"
      },
      {
        "name": "packages",
        "type": "ARRAY",
        "description": "Packages requested for shipment",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Package contents or description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Package quantity"
          },
          {
            "name": "weight_kg",
            "type": "DECIMAL",
            "description": "Package weight in kilograms"
          },
          {
            "name": "dimensions",
            "type": "TEXT",
            "description": "Package dimensions"
          }
        ]
      },
      {
        "name": "special_handling",
        "type": "TEXT",
        "description": "Fragile, dangerous goods, refrigeration, insurance, or other handling instructions"
      }
    ]
  }
});
```

```python
from iterationlayer import IterationLayer

client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(**{
  "files": [
    {
      "type": "url",
      "name": "shipping-request.pdf",
      "url": "https://example.com/documents/shipping-request-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "request_number",
        "type": "TEXT",
        "description": "Shipping request number"
      },
      {
        "name": "requestor_name",
        "type": "TEXT",
        "description": "Requestor name"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "ship_from",
        "type": "ADDRESS",
        "description": "Ship-from address"
      },
      {
        "name": "ship_to",
        "type": "ADDRESS",
        "description": "Ship-to address"
      },
      {
        "name": "requested_ship_date",
        "type": "DATE",
        "description": "Requested ship date"
      },
      {
        "name": "service_level",
        "type": "TEXT",
        "description": "Requested carrier service level"
      },
      {
        "name": "packages",
        "type": "ARRAY",
        "description": "Packages requested for shipment",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Package contents or description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Package quantity"
          },
          {
            "name": "weight_kg",
            "type": "DECIMAL",
            "description": "Package weight in kilograms"
          },
          {
            "name": "dimensions",
            "type": "TEXT",
            "description": "Package dimensions"
          }
        ]
      },
      {
        "name": "special_handling",
        "type": "TEXT",
        "description": "Fragile, dangerous goods, refrigeration, insurance, or other handling instructions"
      }
    ]
  }
})
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.FileInput{
				Type: "url",
				Name: "shipping-request.pdf",
				Url: "https://example.com/documents/shipping-request-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "request_number",
					Type: "TEXT",
					Description: "Shipping request number",
				},
				il.TextFieldConfig{
					Name: "requestor_name",
					Type: "TEXT",
					Description: "Requestor name",
				},
				il.TextFieldConfig{
					Name: "department_or_cost_center",
					Type: "TEXT",
					Description: "Department, project, or cost center",
				},
				il.AddressFieldConfig{
					Name: "ship_from",
					Type: "ADDRESS",
					Description: "Ship-from address",
				},
				il.AddressFieldConfig{
					Name: "ship_to",
					Type: "ADDRESS",
					Description: "Ship-to address",
				},
				il.DateFieldConfig{
					Name: "requested_ship_date",
					Type: "DATE",
					Description: "Requested ship date",
				},
				il.TextFieldConfig{
					Name: "service_level",
					Type: "TEXT",
					Description: "Requested carrier service level",
				},
				il.ArrayFieldConfig{
					Name: "packages",
					Type: "ARRAY",
					Description: "Packages requested for shipment",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Package contents or description",
						},
						il.DecimalFieldConfig{
							Name: "quantity",
							Type: "DECIMAL",
							Description: "Package quantity",
						},
						il.DecimalFieldConfig{
							Name: "weight_kg",
							Type: "DECIMAL",
							Description: "Package weight in kilograms",
						},
						il.TextFieldConfig{
							Name: "dimensions",
							Type: "TEXT",
							Description: "Package dimensions",
						},
					},
				},
				il.TextFieldConfig{
					Name: "special_handling",
					Type: "TEXT",
					Description: "Fragile, dangerous goods, refrigeration, insurance, or other handling instructions",
				},
			},
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
  "name": "Extract Shipping Request Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Shipping Request Data\n\nOperations teams use this recipe to extract internal shipping request fields into structured records for carrier booking, warehouse fulfillment, and label generation.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-shipping-request-data-overview",
      "name": "Overview"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "extract-shipping-request-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"request_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Shipping request number\"\n    },\n    {\n      \"name\": \"requestor_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Requestor name\"\n    },\n    {\n      \"name\": \"department_or_cost_center\",\n      \"type\": \"TEXT\",\n      \"description\": \"Department, project, or cost center\"\n    },\n    {\n      \"name\": \"ship_from\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Ship-from address\"\n    },\n    {\n      \"name\": \"ship_to\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Ship-to address\"\n    },\n    {\n      \"name\": \"requested_ship_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Requested ship date\"\n    },\n    {\n      \"name\": \"service_level\",\n      \"type\": \"TEXT\",\n      \"description\": \"Requested carrier service level\"\n    },\n    {\n      \"name\": \"packages\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Packages requested for shipment\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Package contents or description\"\n        },\n        {\n          \"name\": \"quantity\",\n          \"type\": \"DECIMAL\",\n          \"description\": \"Package quantity\"\n        },\n        {\n          \"name\": \"weight_kg\",\n          \"type\": \"DECIMAL\",\n          \"description\": \"Package weight in kilograms\"\n        },\n        {\n          \"name\": \"dimensions\",\n          \"type\": \"TEXT\",\n          \"description\": \"Package dimensions\"\n        }\n      ]\n    },\n    {\n      \"name\": \"special_handling\",\n      \"type\": \"TEXT\",\n      \"description\": \"Fragile, dangerous goods, refrigeration, insurance, or other handling instructions\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "shipping-request.pdf",
              "fileUrl": "https://example.com/documents/shipping-request-sample.pdf"
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
      "id": "extract-shipping-request-data-extract",
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
Extract shipping request data from the file at [file URL]. Use the extract_document tool with these fields:

- request_number (TEXT): Shipping request number
- requestor_name (TEXT): Requestor name
- department_or_cost_center (TEXT): Department, project, or cost center
- ship_from (ADDRESS): Ship-from address
- ship_to (ADDRESS): Ship-to address
- requested_ship_date (DATE): Requested ship date
- service_level (TEXT): Requested carrier service level
- packages (ARRAY): Each with description (TEXT), quantity (DECIMAL), weight_kg (DECIMAL), dimensions (TEXT)
- special_handling (TEXT): Fragile, dangerous goods, refrigeration, insurance, or other handling instructions
```

### Response


```json
{
  "success": true,
  "data": {
    "request_number": {
      "value": "Request Number",
      "confidence": 0.97,
      "citations": [
        "REQUEST NUMBER"
      ]
    },
    "requestor_name": {
      "value": "Requestor Name",
      "confidence": 0.97,
      "citations": [
        "REQUESTOR NAME"
      ]
    },
    "department_or_cost_center": {
      "value": "Department Or Cost Center",
      "confidence": 0.97,
      "citations": [
        "DEPARTMENT OR COST CENTER"
      ]
    },
    "ship_from": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "SHIP FROM"
      ]
    },
    "ship_to": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "SHIP TO"
      ]
    },
    "requested_ship_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "service_level": {
      "value": "Service Level",
      "confidence": 0.97,
      "citations": [
        "SERVICE LEVEL"
      ]
    },
    "packages": {
      "value": [
        {
          "description": {
            "value": "Description",
            "confidence": 0.96,
            "citations": [
              "DESCRIPTION"
            ]
          },
          "quantity": {
            "value": 12.5,
            "confidence": 0.96,
            "citations": [
              "QUANTITY"
            ]
          },
          "weight_kg": {
            "value": 12.5,
            "confidence": 0.96,
            "citations": [
              "WEIGHT KG"
            ]
          },
          "dimensions": {
            "value": "Dimensions",
            "confidence": 0.96,
            "citations": [
              "DIMENSIONS"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "PACKAGES"
      ]
    },
    "special_handling": {
      "value": "Special Handling",
      "confidence": 0.97,
      "citations": [
        "SPECIAL HANDLING"
      ]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
