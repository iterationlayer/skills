---
name: extract-delivery-note-data
description: Extract shipment details, item quantities, and delivery confirmation data from warehouse delivery notes and goods received notes.
---

# Extract Delivery Note Data

Logistics agencies and warehouse operators use this recipe to digitize delivery notes — extracting shipment references, item quantities, and receiving details for inventory reconciliation and goods-received workflows.

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
        "name": "delivery-note-DN-2026-1147.pdf",
        "url": "https://example.com/warehouse/delivery-note-DN-2026-1147.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "delivery_note_number",
          "type": "TEXT",
          "description": "Delivery note or consignment number"
        },
        {
          "name": "delivery_date",
          "type": "DATE",
          "description": "Date of delivery"
        },
        {
          "name": "supplier_name",
          "type": "TEXT",
          "description": "Supplier or sender name"
        },
        {
          "name": "purchase_order_ref",
          "type": "TEXT",
          "description": "Related purchase order reference"
        },
        {
          "name": "carrier",
          "type": "TEXT",
          "description": "Carrier or transport company"
        },
        {
          "name": "delivery_address",
          "type": "ADDRESS",
          "description": "Delivery destination address"
        },
        {
          "name": "items",
          "type": "ARRAY",
          "description": "Delivered items",
          "fields": [
            {
              "name": "item_code",
              "type": "TEXT",
              "description": "Item or SKU code"
            },
            {
              "name": "description",
              "type": "TEXT",
              "description": "Item description"
            },
            {
              "name": "quantity_ordered",
              "type": "INTEGER",
              "description": "Quantity originally ordered"
            },
            {
              "name": "quantity_delivered",
              "type": "INTEGER",
              "description": "Quantity actually delivered"
            },
            {
              "name": "unit",
              "type": "TEXT",
              "description": "Unit of measure, e.g. pcs, kg, pallets"
            }
          ]
        },
        {
          "name": "total_packages",
          "type": "INTEGER",
          "description": "Total number of packages or pallets"
        },
        {
          "name": "receiver_name",
          "type": "TEXT",
          "description": "Name of the person who received the delivery"
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
      name: "delivery-note-DN-2026-1147.pdf",
      url: "https://example.com/warehouse/delivery-note-DN-2026-1147.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "delivery_note_number", type: "TEXT", description: "Delivery note or consignment number" },
      { name: "delivery_date", type: "DATE", description: "Date of delivery" },
      { name: "supplier_name", type: "TEXT", description: "Supplier or sender name" },
      { name: "purchase_order_ref", type: "TEXT", description: "Related purchase order reference" },
      { name: "carrier", type: "TEXT", description: "Carrier or transport company" },
      { name: "delivery_address", type: "ADDRESS", description: "Delivery destination address" },
      {
        name: "items",
        type: "ARRAY",
        description: "Delivered items",
        fields: [
          { name: "item_code", type: "TEXT", description: "Item or SKU code" },
          { name: "description", type: "TEXT", description: "Item description" },
          { name: "quantity_ordered", type: "INTEGER", description: "Quantity originally ordered" },
          { name: "quantity_delivered", type: "INTEGER", description: "Quantity actually delivered" },
          { name: "unit", type: "TEXT", description: "Unit of measure, e.g. pcs, kg, pallets" },
        ],
      },
      { name: "total_packages", type: "INTEGER", description: "Total number of packages or pallets" },
      { name: "receiver_name", type: "TEXT", description: "Name of the person who received the delivery" },
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
            "name": "delivery-note-DN-2026-1147.pdf",
            "url": "https://example.com/warehouse/delivery-note-DN-2026-1147.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "delivery_note_number", "type": "TEXT", "description": "Delivery note or consignment number"},
            {"name": "delivery_date", "type": "DATE", "description": "Date of delivery"},
            {"name": "supplier_name", "type": "TEXT", "description": "Supplier or sender name"},
            {"name": "purchase_order_ref", "type": "TEXT", "description": "Related purchase order reference"},
            {"name": "carrier", "type": "TEXT", "description": "Carrier or transport company"},
            {"name": "delivery_address", "type": "ADDRESS", "description": "Delivery destination address"},
            {
                "name": "items",
                "type": "ARRAY",
                "description": "Delivered items",
                "fields": [
                    {"name": "item_code", "type": "TEXT", "description": "Item or SKU code"},
                    {"name": "description", "type": "TEXT", "description": "Item description"},
                    {"name": "quantity_ordered", "type": "INTEGER", "description": "Quantity originally ordered"},
                    {"name": "quantity_delivered", "type": "INTEGER", "description": "Quantity actually delivered"},
                    {"name": "unit", "type": "TEXT", "description": "Unit of measure"},
                ],
            },
            {"name": "total_packages", "type": "INTEGER", "description": "Total number of packages or pallets"},
            {"name": "receiver_name", "type": "TEXT", "description": "Name of the person who received the delivery"},
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
			il.NewFileFromURL("delivery-note-DN-2026-1147.pdf", "https://example.com/warehouse/delivery-note-DN-2026-1147.pdf"),
		},
		Schema: il.ExtractionSchema{
			"delivery_note_number": il.NewTextFieldConfig("delivery_note_number", "Delivery note or consignment number"),
			"delivery_date":       il.NewDateFieldConfig("delivery_date", "Date of delivery"),
			"supplier_name":       il.NewTextFieldConfig("supplier_name", "Supplier or sender name"),
			"purchase_order_ref":  il.NewTextFieldConfig("purchase_order_ref", "Related purchase order reference"),
			"carrier":             il.NewTextFieldConfig("carrier", "Carrier or transport company"),
			"delivery_address":    il.NewAddressFieldConfig("delivery_address", "Delivery destination address"),
			"items": il.NewArrayFieldConfig("items", "Delivered items", []il.FieldConfig{
				il.NewTextFieldConfig("item_code", "Item or SKU code"),
				il.NewTextFieldConfig("description", "Item description"),
				il.NewIntegerFieldConfig("quantity_ordered", "Quantity originally ordered"),
				il.NewIntegerFieldConfig("quantity_delivered", "Quantity actually delivered"),
				il.NewTextFieldConfig("unit", "Unit of measure"),
			}),
			"total_packages": il.NewIntegerFieldConfig("total_packages", "Total number of packages or pallets"),
			"receiver_name":  il.NewTextFieldConfig("receiver_name", "Name of the person who received the delivery"),
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
  "name": "Extract delivery note data in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Delivery Note Data\n\nLogistics agencies and warehouse operators use this recipe to digitize delivery notes — extracting shipment references, item quantities, and receiving details for inventory reconciliation and goods-received workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "d0e1f2a3-b4c5-6789-defa-789012345601",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Delivery Note Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "d0e1f2a3-b4c5-6789-defa-789012345602",
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
      "id": "d0e1f2a3-b4c5-6789-defa-789012345603",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"delivery_note_number\",\"type\":\"TEXT\",\"description\":\"Delivery note or consignment number\"},{\"name\":\"delivery_date\",\"type\":\"DATE\",\"description\":\"Date of delivery\"},{\"name\":\"supplier_name\",\"type\":\"TEXT\",\"description\":\"Supplier or sender name\"},{\"name\":\"purchase_order_ref\",\"type\":\"TEXT\",\"description\":\"Related purchase order reference\"},{\"name\":\"carrier\",\"type\":\"TEXT\",\"description\":\"Carrier or transport company\"},{\"name\":\"delivery_address\",\"type\":\"ADDRESS\",\"description\":\"Delivery destination address\"},{\"name\":\"items\",\"type\":\"ARRAY\",\"description\":\"Delivered items\",\"fields\":[{\"name\":\"item_code\",\"type\":\"TEXT\",\"description\":\"Item or SKU code\"},{\"name\":\"description\",\"type\":\"TEXT\",\"description\":\"Item description\"},{\"name\":\"quantity_ordered\",\"type\":\"INTEGER\",\"description\":\"Quantity ordered\"},{\"name\":\"quantity_delivered\",\"type\":\"INTEGER\",\"description\":\"Quantity delivered\"},{\"name\":\"unit\",\"type\":\"TEXT\",\"description\":\"Unit of measure\"}]},{\"name\":\"total_packages\",\"type\":\"INTEGER\",\"description\":\"Total packages\"},{\"name\":\"receiver_name\",\"type\":\"TEXT\",\"description\":\"Receiver name\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "delivery-note-DN-2026-1147.pdf",
              "fileUrl": "https://example.com/warehouse/delivery-note-DN-2026-1147.pdf"
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
      "id": "d0e1f2a3-b4c5-6789-defa-789012345604",
      "name": "Extract Delivery Note Data",
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
            "node": "Extract Delivery Note Data",
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
Extract delivery note data from the file at [file URL]. Use the extract_document tool with these fields:

- delivery_note_number (TEXT): Delivery note or consignment number
- delivery_date (DATE): Date of delivery
- supplier_name (TEXT): Supplier or sender name
- purchase_order_ref (TEXT): Related purchase order reference
- carrier (TEXT): Carrier or transport company
- delivery_address (ADDRESS): Delivery destination address
- items (ARRAY): Each with item_code (TEXT), description (TEXT), quantity_ordered (INTEGER), quantity_delivered (INTEGER), unit (TEXT)
- total_packages (INTEGER): Total number of packages or pallets
- receiver_name (TEXT): Name of the person who received the delivery
```

### Response


```json
{
  "success": true,
  "data": {
    "delivery_note_number": {
      "value": "DN-2026-1147",
      "confidence": 0.99,
      "citations": [
        "Lieferschein Nr. DN-2026-1147"
      ]
    },
    "delivery_date": {
      "value": "2026-04-02",
      "confidence": 0.98,
      "citations": [
        "Lieferdatum: 02.04.2026"
      ]
    },
    "supplier_name": {
      "value": "Rheinland Packaging GmbH",
      "confidence": 0.98,
      "citations": [
        "Absender: Rheinland Packaging GmbH"
      ]
    },
    "purchase_order_ref": {
      "value": "PO-2026-0394",
      "confidence": 0.97,
      "citations": [
        "Bestellnummer: PO-2026-0394"
      ]
    },
    "carrier": {
      "value": "DHL Freight",
      "confidence": 0.96,
      "citations": [
        "Spediteur: DHL Freight"
      ]
    },
    "delivery_address": {
      "value": {
        "street": "Industriestr. 28",
        "city": "Dortmund",
        "postal_code": "44147",
        "country": "DE"
      },
      "confidence": 0.95,
      "citations": [
        "Lieferadresse: Industriestr. 28, 44147 Dortmund"
      ]
    },
    "items": {
      "value": [
        {
          "item_code": {
            "value": "PKG-CB-400",
            "confidence": 0.97,
            "citations": [
              "PKG-CB-400"
            ]
          },
          "description": {
            "value": "Corrugated boxes 400x300x200mm",
            "confidence": 0.96,
            "citations": [
              "Wellpappkartons 400x300x200mm"
            ]
          },
          "quantity_ordered": {
            "value": 500,
            "confidence": 0.98,
            "citations": [
              "Bestellt: 500"
            ]
          },
          "quantity_delivered": {
            "value": 500,
            "confidence": 0.98,
            "citations": [
              "Geliefert: 500"
            ]
          },
          "unit": {
            "value": "pcs",
            "confidence": 0.97,
            "citations": [
              "Stk."
            ]
          }
        },
        {
          "item_code": {
            "value": "PKG-BW-50",
            "confidence": 0.97,
            "citations": [
              "PKG-BW-50"
            ]
          },
          "description": {
            "value": "Bubble wrap rolls 50m",
            "confidence": 0.95,
            "citations": [
              "Luftpolsterfolie 50m Rollen"
            ]
          },
          "quantity_ordered": {
            "value": 20,
            "confidence": 0.98,
            "citations": [
              "Bestellt: 20"
            ]
          },
          "quantity_delivered": {
            "value": 18,
            "confidence": 0.97,
            "citations": [
              "Geliefert: 18"
            ]
          },
          "unit": {
            "value": "rolls",
            "confidence": 0.96,
            "citations": [
              "Rollen"
            ]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    },
    "total_packages": {
      "value": 12,
      "confidence": 0.97,
      "citations": [
        "Packstücke gesamt: 12"
      ]
    },
    "receiver_name": {
      "value": "Thomas Becker",
      "confidence": 0.95,
      "citations": [
        "Empfänger: Thomas Becker"
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
