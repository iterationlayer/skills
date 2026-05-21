---
name: extract-delivery-receipt-data
description: Extract proof-of-delivery number, recipient, delivery time, items, exceptions, and signature status from delivery receipts.
---

# Extract Delivery Receipt Data

Logistics and operations teams use this recipe to extract delivery receipt fields into structured proof-of-delivery records for customer service, billing, and exception handling.

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
      "name": "delivery-receipt.pdf",
      "url": "https://example.com/documents/delivery-receipt-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_receipt_number",
        "type": "TEXT",
        "description": "Delivery receipt or proof-of-delivery number"
      },
      {
        "name": "tracking_number",
        "type": "TEXT",
        "description": "Tracking or shipment number"
      },
      {
        "name": "carrier_name",
        "type": "TEXT",
        "description": "Carrier name"
      },
      {
        "name": "recipient_name",
        "type": "TEXT",
        "description": "Recipient name"
      },
      {
        "name": "delivery_address",
        "type": "ADDRESS",
        "description": "Delivery address"
      },
      {
        "name": "delivery_date",
        "type": "DATE",
        "description": "Delivery date"
      },
      {
        "name": "delivery_time",
        "type": "TEXT",
        "description": "Delivery time"
      },
      {
        "name": "delivered_items",
        "type": "ARRAY",
        "description": "Delivered item or package lines",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Item or package description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Delivered quantity"
          },
          {
            "name": "condition",
            "type": "TEXT",
            "description": "Delivered condition or exception"
          }
        ]
      },
      {
        "name": "signature_present",
        "type": "BOOLEAN",
        "description": "Whether a recipient signature is present"
      },
      {
        "name": "exception_notes",
        "type": "TEXT",
        "description": "Shortage, damage, refusal, or other delivery exception notes"
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
      "name": "delivery-receipt.pdf",
      "url": "https://example.com/documents/delivery-receipt-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_receipt_number",
        "type": "TEXT",
        "description": "Delivery receipt or proof-of-delivery number"
      },
      {
        "name": "tracking_number",
        "type": "TEXT",
        "description": "Tracking or shipment number"
      },
      {
        "name": "carrier_name",
        "type": "TEXT",
        "description": "Carrier name"
      },
      {
        "name": "recipient_name",
        "type": "TEXT",
        "description": "Recipient name"
      },
      {
        "name": "delivery_address",
        "type": "ADDRESS",
        "description": "Delivery address"
      },
      {
        "name": "delivery_date",
        "type": "DATE",
        "description": "Delivery date"
      },
      {
        "name": "delivery_time",
        "type": "TEXT",
        "description": "Delivery time"
      },
      {
        "name": "delivered_items",
        "type": "ARRAY",
        "description": "Delivered item or package lines",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Item or package description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Delivered quantity"
          },
          {
            "name": "condition",
            "type": "TEXT",
            "description": "Delivered condition or exception"
          }
        ]
      },
      {
        "name": "signature_present",
        "type": "BOOLEAN",
        "description": "Whether a recipient signature is present"
      },
      {
        "name": "exception_notes",
        "type": "TEXT",
        "description": "Shortage, damage, refusal, or other delivery exception notes"
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
      "name": "delivery-receipt.pdf",
      "url": "https://example.com/documents/delivery-receipt-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "delivery_receipt_number",
        "type": "TEXT",
        "description": "Delivery receipt or proof-of-delivery number"
      },
      {
        "name": "tracking_number",
        "type": "TEXT",
        "description": "Tracking or shipment number"
      },
      {
        "name": "carrier_name",
        "type": "TEXT",
        "description": "Carrier name"
      },
      {
        "name": "recipient_name",
        "type": "TEXT",
        "description": "Recipient name"
      },
      {
        "name": "delivery_address",
        "type": "ADDRESS",
        "description": "Delivery address"
      },
      {
        "name": "delivery_date",
        "type": "DATE",
        "description": "Delivery date"
      },
      {
        "name": "delivery_time",
        "type": "TEXT",
        "description": "Delivery time"
      },
      {
        "name": "delivered_items",
        "type": "ARRAY",
        "description": "Delivered item or package lines",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Item or package description"
          },
          {
            "name": "quantity",
            "type": "DECIMAL",
            "description": "Delivered quantity"
          },
          {
            "name": "condition",
            "type": "TEXT",
            "description": "Delivered condition or exception"
          }
        ]
      },
      {
        "name": "signature_present",
        "type": "BOOLEAN",
        "description": "Whether a recipient signature is present"
      },
      {
        "name": "exception_notes",
        "type": "TEXT",
        "description": "Shortage, damage, refusal, or other delivery exception notes"
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
				Name: "delivery-receipt.pdf",
				Url: "https://example.com/documents/delivery-receipt-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "delivery_receipt_number",
					Type: "TEXT",
					Description: "Delivery receipt or proof-of-delivery number",
				},
				il.TextFieldConfig{
					Name: "tracking_number",
					Type: "TEXT",
					Description: "Tracking or shipment number",
				},
				il.TextFieldConfig{
					Name: "carrier_name",
					Type: "TEXT",
					Description: "Carrier name",
				},
				il.TextFieldConfig{
					Name: "recipient_name",
					Type: "TEXT",
					Description: "Recipient name",
				},
				il.AddressFieldConfig{
					Name: "delivery_address",
					Type: "ADDRESS",
					Description: "Delivery address",
				},
				il.DateFieldConfig{
					Name: "delivery_date",
					Type: "DATE",
					Description: "Delivery date",
				},
				il.TextFieldConfig{
					Name: "delivery_time",
					Type: "TEXT",
					Description: "Delivery time",
				},
				il.ArrayFieldConfig{
					Name: "delivered_items",
					Type: "ARRAY",
					Description: "Delivered item or package lines",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Item or package description",
						},
						il.DecimalFieldConfig{
							Name: "quantity",
							Type: "DECIMAL",
							Description: "Delivered quantity",
						},
						il.TextFieldConfig{
							Name: "condition",
							Type: "TEXT",
							Description: "Delivered condition or exception",
						},
					},
				},
				il.BooleanFieldConfig{
					Name: "signature_present",
					Type: "BOOLEAN",
					Description: "Whether a recipient signature is present",
				},
				il.TextFieldConfig{
					Name: "exception_notes",
					Type: "TEXT",
					Description: "Shortage, damage, refusal, or other delivery exception notes",
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
  "name": "Extract Delivery Receipt Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Delivery Receipt Data\n\nLogistics and operations teams use this recipe to extract delivery receipt fields into structured proof-of-delivery records for customer service, billing, and exception handling.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-delivery-receipt-data-overview",
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
      "id": "extract-delivery-receipt-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"delivery_receipt_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Delivery receipt or proof-of-delivery number\"\n    },\n    {\n      \"name\": \"tracking_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Tracking or shipment number\"\n    },\n    {\n      \"name\": \"carrier_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Carrier name\"\n    },\n    {\n      \"name\": \"recipient_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Recipient name\"\n    },\n    {\n      \"name\": \"delivery_address\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Delivery address\"\n    },\n    {\n      \"name\": \"delivery_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Delivery date\"\n    },\n    {\n      \"name\": \"delivery_time\",\n      \"type\": \"TEXT\",\n      \"description\": \"Delivery time\"\n    },\n    {\n      \"name\": \"delivered_items\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Delivered item or package lines\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Item or package description\"\n        },\n        {\n          \"name\": \"quantity\",\n          \"type\": \"DECIMAL\",\n          \"description\": \"Delivered quantity\"\n        },\n        {\n          \"name\": \"condition\",\n          \"type\": \"TEXT\",\n          \"description\": \"Delivered condition or exception\"\n        }\n      ]\n    },\n    {\n      \"name\": \"signature_present\",\n      \"type\": \"BOOLEAN\",\n      \"description\": \"Whether a recipient signature is present\"\n    },\n    {\n      \"name\": \"exception_notes\",\n      \"type\": \"TEXT\",\n      \"description\": \"Shortage, damage, refusal, or other delivery exception notes\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "delivery-receipt.pdf",
              "fileUrl": "https://example.com/documents/delivery-receipt-sample.pdf"
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
      "id": "extract-delivery-receipt-data-extract",
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
Extract delivery receipt data from the file at [file URL]. Use the extract_document tool with these fields:

- delivery_receipt_number (TEXT): Delivery receipt or proof-of-delivery number
- tracking_number (TEXT): Tracking or shipment number
- carrier_name (TEXT): Carrier name
- recipient_name (TEXT): Recipient name
- delivery_address (ADDRESS): Delivery address
- delivery_date (DATE): Delivery date
- delivery_time (TEXT): Delivery time
- delivered_items (ARRAY): Each with description (TEXT), quantity (DECIMAL), condition (TEXT)
- signature_present (BOOLEAN): Whether a recipient signature is present
- exception_notes (TEXT): Shortage, damage, refusal, or other delivery exception notes
```

### Response


```json
{
  "success": true,
  "data": {
    "delivery_receipt_number": {
      "value": "Delivery Receipt Number",
      "confidence": 0.97,
      "citations": [
        "DELIVERY RECEIPT NUMBER"
      ]
    },
    "tracking_number": {
      "value": "Tracking Number",
      "confidence": 0.97,
      "citations": [
        "TRACKING NUMBER"
      ]
    },
    "carrier_name": {
      "value": "DHL Freight",
      "confidence": 0.97,
      "citations": [
        "CARRIER NAME"
      ]
    },
    "recipient_name": {
      "value": "Recipient Name",
      "confidence": 0.97,
      "citations": [
        "RECIPIENT NAME"
      ]
    },
    "delivery_address": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "DELIVERY ADDRESS"
      ]
    },
    "delivery_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "delivery_time": {
      "value": "Delivery Time",
      "confidence": 0.97,
      "citations": [
        "DELIVERY TIME"
      ]
    },
    "delivered_items": {
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
          "condition": {
            "value": "Condition",
            "confidence": 0.96,
            "citations": [
              "CONDITION"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "DELIVERED ITEMS"
      ]
    },
    "signature_present": {
      "value": true,
      "confidence": 0.97,
      "citations": [
        "SIGNATURE PRESENT"
      ]
    },
    "exception_notes": {
      "value": "Exception Notes",
      "confidence": 0.97,
      "citations": [
        "EXCEPTION NOTES"
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
