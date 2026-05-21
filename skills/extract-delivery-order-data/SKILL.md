---
name: extract-delivery-order-data
description: Extract release order, carrier, consignee, pickup location, goods, and validity fields from delivery orders.
---

# Extract Delivery Order Data

Freight forwarders and warehouses use this recipe to extract delivery order fields into structured release records for pickup authorization and cargo handoff workflows.

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
      "name": "delivery-order.pdf",
      "url": "https://example.com/documents/delivery-order-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_order_number",
        "type": "TEXT",
        "description": "Delivery order or release number"
      },
      {
        "name": "bill_of_lading_number",
        "type": "TEXT",
        "description": "Related bill of lading or shipment number"
      },
      {
        "name": "carrier_or_agent",
        "type": "TEXT",
        "description": "Carrier, freight forwarder, or issuing agent"
      },
      {
        "name": "consignee_name",
        "type": "TEXT",
        "description": "Consignee or authorized recipient"
      },
      {
        "name": "pickup_location",
        "type": "ADDRESS",
        "description": "Pickup or release location"
      },
      {
        "name": "issue_date",
        "type": "DATE",
        "description": "Issue date"
      },
      {
        "name": "valid_until",
        "type": "DATE",
        "description": "Expiration or last free date if shown"
      },
      {
        "name": "goods",
        "type": "ARRAY",
        "description": "Goods authorized for release",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Goods description"
          },
          {
            "name": "container_or_package_number",
            "type": "TEXT",
            "description": "Container, package, or seal number"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Quantity or package count"
          },
          {
            "name": "gross_weight_kg",
            "type": "DECIMAL",
            "description": "Gross weight in kilograms"
          }
        ]
      },
      {
        "name": "release_instructions",
        "type": "TEXT",
        "description": "Release instructions or conditions"
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
      "name": "delivery-order.pdf",
      "url": "https://example.com/documents/delivery-order-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_order_number",
        "type": "TEXT",
        "description": "Delivery order or release number"
      },
      {
        "name": "bill_of_lading_number",
        "type": "TEXT",
        "description": "Related bill of lading or shipment number"
      },
      {
        "name": "carrier_or_agent",
        "type": "TEXT",
        "description": "Carrier, freight forwarder, or issuing agent"
      },
      {
        "name": "consignee_name",
        "type": "TEXT",
        "description": "Consignee or authorized recipient"
      },
      {
        "name": "pickup_location",
        "type": "ADDRESS",
        "description": "Pickup or release location"
      },
      {
        "name": "issue_date",
        "type": "DATE",
        "description": "Issue date"
      },
      {
        "name": "valid_until",
        "type": "DATE",
        "description": "Expiration or last free date if shown"
      },
      {
        "name": "goods",
        "type": "ARRAY",
        "description": "Goods authorized for release",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Goods description"
          },
          {
            "name": "container_or_package_number",
            "type": "TEXT",
            "description": "Container, package, or seal number"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Quantity or package count"
          },
          {
            "name": "gross_weight_kg",
            "type": "DECIMAL",
            "description": "Gross weight in kilograms"
          }
        ]
      },
      {
        "name": "release_instructions",
        "type": "TEXT",
        "description": "Release instructions or conditions"
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
      "name": "delivery-order.pdf",
      "url": "https://example.com/documents/delivery-order-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_order_number",
        "type": "TEXT",
        "description": "Delivery order or release number"
      },
      {
        "name": "bill_of_lading_number",
        "type": "TEXT",
        "description": "Related bill of lading or shipment number"
      },
      {
        "name": "carrier_or_agent",
        "type": "TEXT",
        "description": "Carrier, freight forwarder, or issuing agent"
      },
      {
        "name": "consignee_name",
        "type": "TEXT",
        "description": "Consignee or authorized recipient"
      },
      {
        "name": "pickup_location",
        "type": "ADDRESS",
        "description": "Pickup or release location"
      },
      {
        "name": "issue_date",
        "type": "DATE",
        "description": "Issue date"
      },
      {
        "name": "valid_until",
        "type": "DATE",
        "description": "Expiration or last free date if shown"
      },
      {
        "name": "goods",
        "type": "ARRAY",
        "description": "Goods authorized for release",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Goods description"
          },
          {
            "name": "container_or_package_number",
            "type": "TEXT",
            "description": "Container, package, or seal number"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Quantity or package count"
          },
          {
            "name": "gross_weight_kg",
            "type": "DECIMAL",
            "description": "Gross weight in kilograms"
          }
        ]
      },
      {
        "name": "release_instructions",
        "type": "TEXT",
        "description": "Release instructions or conditions"
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
				Name: "delivery-order.pdf",
				Url: "https://example.com/documents/delivery-order-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "delivery_order_number",
					Type: "TEXT",
					Description: "Delivery order or release number",
				},
				il.TextFieldConfig{
					Name: "bill_of_lading_number",
					Type: "TEXT",
					Description: "Related bill of lading or shipment number",
				},
				il.TextFieldConfig{
					Name: "carrier_or_agent",
					Type: "TEXT",
					Description: "Carrier, freight forwarder, or issuing agent",
				},
				il.TextFieldConfig{
					Name: "consignee_name",
					Type: "TEXT",
					Description: "Consignee or authorized recipient",
				},
				il.AddressFieldConfig{
					Name: "pickup_location",
					Type: "ADDRESS",
					Description: "Pickup or release location",
				},
				il.DateFieldConfig{
					Name: "issue_date",
					Type: "DATE",
					Description: "Issue date",
				},
				il.DateFieldConfig{
					Name: "valid_until",
					Type: "DATE",
					Description: "Expiration or last free date if shown",
				},
				il.ArrayFieldConfig{
					Name: "goods",
					Type: "ARRAY",
					Description: "Goods authorized for release",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Goods description",
						},
						il.TextFieldConfig{
							Name: "container_or_package_number",
							Type: "TEXT",
							Description: "Container, package, or seal number",
						},
						il.DecimalFieldConfig{
							Name: "quantity",
							Type: "DECIMAL",
							Description: "Quantity or package count",
						},
						il.DecimalFieldConfig{
							Name: "gross_weight_kg",
							Type: "DECIMAL",
							Description: "Gross weight in kilograms",
						},
					},
				},
				il.TextFieldConfig{
					Name: "release_instructions",
					Type: "TEXT",
					Description: "Release instructions or conditions",
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
  "name": "Extract Delivery Order Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Delivery Order Data\n\nFreight forwarders and warehouses use this recipe to extract delivery order fields into structured release records for pickup authorization and cargo handoff workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-delivery-order-data-overview",
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
      "id": "extract-delivery-order-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"delivery_order_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Delivery order or release number\"\n    },\n    {\n      \"name\": \"bill_of_lading_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Related bill of lading or shipment number\"\n    },\n    {\n      \"name\": \"carrier_or_agent\",\n      \"type\": \"TEXT\",\n      \"description\": \"Carrier, freight forwarder, or issuing agent\"\n    },\n    {\n      \"name\": \"consignee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Consignee or authorized recipient\"\n    },\n    {\n      \"name\": \"pickup_location\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Pickup or release location\"\n    },\n    {\n      \"name\": \"issue_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Issue date\"\n    },\n    {\n      \"name\": \"valid_until\",\n      \"type\": \"DATE\",\n      \"description\": \"Expiration or last free date if shown\"\n    },\n    {\n      \"name\": \"goods\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Goods authorized for release\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Goods description\"\n        },\n        {\n          \"name\": \"container_or_package_number\",\n          \"type\": \"TEXT\",\n          \"description\": \"Container, package, or seal number\"\n        },\n        {\n          \"name\": \"quantity\",\n          \"type\": \"DECIMAL\",\n          \"description\": \"Quantity or package count\"\n        },\n        {\n          \"name\": \"gross_weight_kg\",\n          \"type\": \"DECIMAL\",\n          \"description\": \"Gross weight in kilograms\"\n        }\n      ]\n    },\n    {\n      \"name\": \"release_instructions\",\n      \"type\": \"TEXT\",\n      \"description\": \"Release instructions or conditions\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "delivery-order.pdf",
              "fileUrl": "https://example.com/documents/delivery-order-sample.pdf"
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
      "id": "extract-delivery-order-data-extract",
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
Extract delivery order data from the file at [file URL]. Use the extract_document tool with these fields:

- delivery_order_number (TEXT): Delivery order or release number
- bill_of_lading_number (TEXT): Related bill of lading or shipment number
- carrier_or_agent (TEXT): Carrier, freight forwarder, or issuing agent
- consignee_name (TEXT): Consignee or authorized recipient
- pickup_location (ADDRESS): Pickup or release location
- issue_date (DATE): Issue date
- valid_until (DATE): Expiration or last free date if shown
- goods (ARRAY): Each with description (TEXT), container_or_package_number (TEXT), quantity (DECIMAL), gross_weight_kg (DECIMAL)
- release_instructions (TEXT): Release instructions or conditions
```

### Response


```json
{
  "success": true,
  "data": {
    "delivery_order_number": {
      "value": "Delivery Order Number",
      "confidence": 0.97,
      "citations": [
        "DELIVERY ORDER NUMBER"
      ]
    },
    "bill_of_lading_number": {
      "value": "Bill Of Lading Number",
      "confidence": 0.97,
      "citations": [
        "BILL OF LADING NUMBER"
      ]
    },
    "carrier_or_agent": {
      "value": "Carrier Or Agent",
      "confidence": 0.97,
      "citations": [
        "CARRIER OR AGENT"
      ]
    },
    "consignee_name": {
      "value": "Acme Distribution Inc.",
      "confidence": 0.97,
      "citations": [
        "CONSIGNEE NAME"
      ]
    },
    "pickup_location": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "PICKUP LOCATION"
      ]
    },
    "issue_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "valid_until": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "goods": {
      "value": [
        {
          "description": {
            "value": "Description",
            "confidence": 0.96,
            "citations": [
              "DESCRIPTION"
            ]
          },
          "container_or_package_number": {
            "value": "Container Or Package Number",
            "confidence": 0.96,
            "citations": [
              "CONTAINER OR PACKAGE NUMBER"
            ]
          },
          "quantity": {
            "value": 12.5,
            "confidence": 0.96,
            "citations": [
              "QUANTITY"
            ]
          },
          "gross_weight_kg": {
            "value": 12.5,
            "confidence": 0.96,
            "citations": [
              "GROSS WEIGHT KG"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "GOODS"
      ]
    },
    "release_instructions": {
      "value": "Release Instructions",
      "confidence": 0.97,
      "citations": [
        "RELEASE INSTRUCTIONS"
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
