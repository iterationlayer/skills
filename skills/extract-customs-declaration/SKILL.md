---
name: extract-customs-declaration
description: Merge a commercial invoice, packing list, and bill of lading into a unified customs declaration.
---

# Extract Customs Declaration

Freight forwarders and customs brokers use this recipe to automate declaration processing. Upload a commercial invoice, packing list, and bill of lading in one API call — receive a unified customs record with shipper, consignee, goods description, HS codes, weights, and declared values.

## APIs Used

Document Extraction (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer $ITERATION_LAYER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "commercial-invoice.pdf",
        "url": "https://example.com/docs/commercial-invoice.pdf"
      },
      {
        "type": "url",
        "name": "packing-list.pdf",
        "url": "https://example.com/docs/packing-list.pdf"
      },
      {
        "type": "url",
        "name": "bill-of-lading.pdf",
        "url": "https://example.com/docs/bill-of-lading.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "shipper",
          "type": "ARRAY",
          "description": "Shipper details",
          "fields": [
              {
                "name": "name",
                "type": "TEXT",
                "description": "Shipper company name"
              },
              {
                "name": "address",
                "type": "TEXT",
                "description": "Shipper address"
              },
              {
                "name": "country",
                "type": "TEXT",
                "description": "Shipper country"
              }
            ]
        },
        {
          "name": "consignee",
          "type": "ARRAY",
          "description": "Consignee details",
          "fields": [
              {
                "name": "name",
                "type": "TEXT",
                "description": "Consignee company name"
              },
              {
                "name": "address",
                "type": "TEXT",
                "description": "Consignee address"
              },
              {
                "name": "country",
                "type": "TEXT",
                "description": "Consignee country"
              }
            ]
        },
        {
          "name": "goods",
          "type": "ARRAY",
          "description": "List of goods in the shipment",
          "fields": [
              {
                "name": "description",
                "type": "TEXT",
                "description": "Description of the goods"
              },
              {
                "name": "hs_code",
                "type": "TEXT",
                "description": "Harmonized System code"
              },
              {
                "name": "quantity",
                "type": "INTEGER",
                "description": "Number of units"
              },
              {
                "name": "weight_in_kg",
                "type": "DECIMAL",
                "description": "Weight in kilograms"
              },
              {
                "name": "declared_value",
                "type": "CURRENCY_AMOUNT",
                "description": "Declared customs value"
              }
            ]
        },
        {
          "name": "total_weight_in_kg",
          "type": "DECIMAL",
          "description": "Total shipment weight in kilograms"
        },
        {
          "name": "total_declared_value",
          "type": "CURRENCY_AMOUNT",
          "description": "Total declared customs value"
        },
        {
          "name": "currency",
          "type": "TEXT",
          "description": "Currency code for declared values"
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
      name: "commercial-invoice.pdf",
      url: "https://example.com/docs/commercial-invoice.pdf",
    },
    {
      type: "url",
      name: "packing-list.pdf",
      url: "https://example.com/docs/packing-list.pdf",
    },
    {
      type: "url",
      name: "bill-of-lading.pdf",
      url: "https://example.com/docs/bill-of-lading.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "shipper",
        type: "ARRAY",
        description: "Shipper details",
        fields: [
            {
              name: "name",
              type: "TEXT",
              description: "Shipper company name",
            },
            {
              name: "address",
              type: "TEXT",
              description: "Shipper address",
            },
            {
              name: "country",
              type: "TEXT",
              description: "Shipper country",
            },
          ],
      },
      {
        name: "consignee",
        type: "ARRAY",
        description: "Consignee details",
        fields: [
            {
              name: "name",
              type: "TEXT",
              description: "Consignee company name",
            },
            {
              name: "address",
              type: "TEXT",
              description: "Consignee address",
            },
            {
              name: "country",
              type: "TEXT",
              description: "Consignee country",
            },
          ],
      },
      {
        name: "goods",
        type: "ARRAY",
        description: "List of goods in the shipment",
        fields: [
            {
              name: "description",
              type: "TEXT",
              description: "Description of the goods",
            },
            {
              name: "hs_code",
              type: "TEXT",
              description: "Harmonized System code",
            },
            {
              name: "quantity",
              type: "INTEGER",
              description: "Number of units",
            },
            {
              name: "weight_in_kg",
              type: "DECIMAL",
              description: "Weight in kilograms",
            },
            {
              name: "declared_value",
              type: "CURRENCY_AMOUNT",
              description: "Declared customs value",
            },
          ],
      },
      {
        name: "total_weight_in_kg",
        type: "DECIMAL",
        description: "Total shipment weight in kilograms",
      },
      {
        name: "total_declared_value",
        type: "CURRENCY_AMOUNT",
        description: "Total declared customs value",
      },
      {
        name: "currency",
        type: "TEXT",
        description: "Currency code for declared values",
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
            "name": "commercial-invoice.pdf",
            "url": "https://example.com/docs/commercial-invoice.pdf",
        },
        {
            "type": "url",
            "name": "packing-list.pdf",
            "url": "https://example.com/docs/packing-list.pdf",
        },
        {
            "type": "url",
            "name": "bill-of-lading.pdf",
            "url": "https://example.com/docs/bill-of-lading.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "shipper",
                "type": "ARRAY",
                "description": "Shipper details",
                "fields": [
                    {
                        "name": "name",
                        "type": "TEXT",
                        "description": "Shipper company name",
                    },
                    {
                        "name": "address",
                        "type": "TEXT",
                        "description": "Shipper address",
                    },
                    {
                        "name": "country",
                        "type": "TEXT",
                        "description": "Shipper country",
                    },
                ],
            },
            {
                "name": "consignee",
                "type": "ARRAY",
                "description": "Consignee details",
                "fields": [
                    {
                        "name": "name",
                        "type": "TEXT",
                        "description": "Consignee company name",
                    },
                    {
                        "name": "address",
                        "type": "TEXT",
                        "description": "Consignee address",
                    },
                    {
                        "name": "country",
                        "type": "TEXT",
                        "description": "Consignee country",
                    },
                ],
            },
            {
                "name": "goods",
                "type": "ARRAY",
                "description": "List of goods in the shipment",
                "fields": [
                    {
                        "name": "description",
                        "type": "TEXT",
                        "description": "Description of the goods",
                    },
                    {
                        "name": "hs_code",
                        "type": "TEXT",
                        "description": "Harmonized System code",
                    },
                    {
                        "name": "quantity",
                        "type": "INTEGER",
                        "description": "Number of units",
                    },
                    {
                        "name": "weight_in_kg",
                        "type": "DECIMAL",
                        "description": "Weight in kilograms",
                    },
                    {
                        "name": "declared_value",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Declared customs value",
                    },
                ],
            },
            {
                "name": "total_weight_in_kg",
                "type": "DECIMAL",
                "description": "Total shipment weight in kilograms",
            },
            {
                "name": "total_declared_value",
                "type": "CURRENCY_AMOUNT",
                "description": "Total declared customs value",
            },
            {
                "name": "currency",
                "type": "TEXT",
                "description": "Currency code for declared values",
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
				"commercial-invoice.pdf",
				"https://example.com/docs/commercial-invoice.pdf",
			),
			il.NewFileFromURL(
				"packing-list.pdf",
				"https://example.com/docs/packing-list.pdf",
			),
			il.NewFileFromURL(
				"bill-of-lading.pdf",
				"https://example.com/docs/bill-of-lading.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"shipper": il.NewArrayFieldConfig(
				"shipper",
				"Shipper details",
				[]il.FieldConfig{
					il.NewTextFieldConfig(
						"name",
						"Shipper company name",
					),
					il.NewTextFieldConfig(
						"address",
						"Shipper address",
					),
					il.NewTextFieldConfig(
						"country",
						"Shipper country",
					),
				},
			),
			"consignee": il.NewArrayFieldConfig(
				"consignee",
				"Consignee details",
				[]il.FieldConfig{
					il.NewTextFieldConfig(
						"name",
						"Consignee company name",
					),
					il.NewTextFieldConfig(
						"address",
						"Consignee address",
					),
					il.NewTextFieldConfig(
						"country",
						"Consignee country",
					),
				},
			),
			"goods": il.NewArrayFieldConfig(
				"goods",
				"List of goods in the shipment",
				[]il.FieldConfig{
					il.NewTextFieldConfig(
						"description",
						"Description of the goods",
					),
					il.NewTextFieldConfig(
						"hs_code",
						"Harmonized System code",
					),
					il.NewIntegerFieldConfig(
						"quantity",
						"Number of units",
					),
					il.NewDecimalFieldConfig(
						"weight_in_kg",
						"Weight in kilograms",
					),
					il.NewCurrencyAmountFieldConfig(
						"declared_value",
						"Declared customs value",
					),
				},
			),
			"total_weight_in_kg": il.NewDecimalFieldConfig(
				"total_weight_in_kg",
				"Total shipment weight in kilograms",
			),
			"total_declared_value": il.NewCurrencyAmountFieldConfig(
				"total_declared_value",
				"Total declared customs value",
			),
			"currency": il.NewTextFieldConfig(
				"currency",
				"Currency code for declared values",
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
  "name": "Extract Customs Declaration",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Customs Declaration\n\nFreight forwarders and customs brokers use this recipe to automate declaration processing. Upload a commercial invoice, packing list, and bill of lading in one API call \u2014 receive a unified customs record with shipper, consignee, goods description, HS codes, weights, and declared values.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "c4414550-c43e-4423-b40a-d0ea01b504b4",
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
      "id": "6c3d22ba-8551-48ca-8924-df2cb20bd73e",
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
      "id": "a7b8c9d0-e1f2-3456-abcd-567890123456",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"shipper\",\"type\":\"ARRAY\",\"description\":\"Shipper details\",\"fields\":[{\"name\":\"name\",\"type\":\"TEXT\",\"description\":\"Shipper company name\"},{\"name\":\"address\",\"type\":\"TEXT\",\"description\":\"Shipper address\"},{\"name\":\"country\",\"type\":\"TEXT\",\"description\":\"Shipper country\"}]},{\"name\":\"consignee\",\"type\":\"ARRAY\",\"description\":\"Consignee details\",\"fields\":[{\"name\":\"name\",\"type\":\"TEXT\",\"description\":\"Consignee company name\"},{\"name\":\"address\",\"type\":\"TEXT\",\"description\":\"Consignee address\"},{\"name\":\"country\",\"type\":\"TEXT\",\"description\":\"Consignee country\"}]},{\"name\":\"goods\",\"type\":\"ARRAY\",\"description\":\"List of goods in the shipment\",\"fields\":[{\"name\":\"description\",\"type\":\"TEXT\",\"description\":\"Description of the goods\"},{\"name\":\"hs_code\",\"type\":\"TEXT\",\"description\":\"Harmonized System code\"},{\"name\":\"quantity\",\"type\":\"INTEGER\",\"description\":\"Number of units\"},{\"name\":\"weight_in_kg\",\"type\":\"DECIMAL\",\"description\":\"Weight in kilograms\"},{\"name\":\"declared_value\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Declared customs value\"}]},{\"name\":\"total_weight_in_kg\",\"type\":\"DECIMAL\",\"description\":\"Total shipment weight in kilograms\"},{\"name\":\"total_declared_value\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total declared customs value\"},{\"name\":\"currency\",\"type\":\"TEXT\",\"description\":\"Currency code for declared values\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "commercial-invoice.pdf",
              "fileUrl": "https://example.com/docs/commercial-invoice.pdf"
            },
            {
              "fileInputMode": "url",
              "fileName": "packing-list.pdf",
              "fileUrl": "https://example.com/docs/packing-list.pdf"
            },
            {
              "fileInputMode": "url",
              "fileName": "bill-of-lading.pdf",
              "fileUrl": "https://example.com/docs/bill-of-lading.pdf"
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
      "id": "b8c9d0e1-f2a3-4567-bcde-678901234567",
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
Extract a customs declaration from the files at [commercial invoice URL], [packing list URL], and [bill of lading URL]. Use the extract_document tool with these fields:

- shipper (ARRAY): With name (TEXT), address (TEXT), country (TEXT)
- consignee (ARRAY): With name (TEXT), address (TEXT), country (TEXT)
- goods (ARRAY): Each with description (TEXT), hs_code (TEXT), quantity (INTEGER), weight_in_kg (DECIMAL), declared_value (CURRENCY_AMOUNT)
- total_weight_in_kg (DECIMAL): Total shipment weight in kilograms
- total_declared_value (CURRENCY_AMOUNT): Total declared customs value
- currency (TEXT): Currency code for declared values
```

### Response


```json
{
  "success": true,
  "data": {
    "shipper": {
      "value": {
        "name": {
          "value": "Shenzhen Electronics Co.",
          "confidence": 0.97,
          "citations": ["Shenzhen Electronics Co., Ltd."]
        },
        "address": {
          "value": "88 Nanshan Road, Shenzhen",
          "confidence": 0.94,
          "citations": ["88 Nanshan Road, Shenzhen 518000"]
        },
        "country": {
          "value": "CN",
          "confidence": 0.99,
          "citations": ["China"]
        }
      },
      "confidence": 0.96,
      "citations": []
    },
    "consignee": {
      "value": {
        "name": {
          "value": "EuroTech Imports GmbH",
          "confidence": 0.98,
          "citations": ["EuroTech Imports GmbH"]
        },
        "address": {
          "value": "Industriestr. 12, Hamburg",
          "confidence": 0.95,
          "citations": ["Industriestr. 12, 20095 Hamburg"]
        },
        "country": {
          "value": "DE",
          "confidence": 0.99,
          "citations": ["Germany"]
        }
      },
      "confidence": 0.96,
      "citations": []
    },
    "goods": {
      "value": [
        {
          "description": {
            "value": "Wireless Bluetooth Earbuds",
            "confidence": 0.96,
            "citations": ["Wireless Bluetooth Earbuds"]
          },
          "hs_code": {
            "value": "8518.30.20",
            "confidence": 0.93,
            "citations": ["HS 8518.30.20"]
          },
          "quantity": {
            "value": 500,
            "confidence": 0.98,
            "citations": ["Qty: 500 pcs"]
          },
          "weight_in_kg": {
            "value": 45.0,
            "confidence": 0.95,
            "citations": ["Net Weight: 45.0 kg"]
          },
          "declared_value": {
            "value": {
              "amount": "7,500.00",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["USD 7,500.00"]
          }
        }
      ],
      "confidence": 0.95,
      "citations": []
    },
    "total_weight_in_kg": {
      "value": 52.5,
      "confidence": 0.96,
      "citations": ["Gross Weight: 52.5 kg"]
    },
    "total_declared_value": {
      "value": {
        "amount": "7,500.00",
        "currency": "USD"
      },
      "confidence": 0.97,
      "citations": ["Total Declared Value: USD 7,500.00"]
    },
    "currency": {
      "value": "USD",
      "confidence": 0.99,
      "citations": ["Currency: USD"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
