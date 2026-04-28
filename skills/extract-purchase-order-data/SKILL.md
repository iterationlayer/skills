---
name: extract-purchase-order-data
description: Extract line items, quantities, unit prices, delivery dates, and supplier details from purchase order documents.
---

# Extract Purchase Order Data

B2B suppliers, procurement teams, and warehouse operators use this recipe to extract purchase order details at scale — pulling out line items, quantities, pricing, and delivery dates for ERP ingestion or order fulfillment workflows.

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
        "name": "po-northstar-retail.pdf",
        "url": "https://example.com/purchase-orders/po-northstar-retail.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "customer_name",
          "type": "TEXT",
          "description": "Name of the customer or buying company"
        },
        {
          "name": "po_number",
          "type": "TEXT",
          "description": "Purchase order number"
        },
        {
          "name": "po_date",
          "type": "DATE",
          "description": "Date the PO was issued"
        },
        {
          "name": "requested_delivery_date",
          "type": "DATE",
          "description": "Requested delivery date"
        },
        {
          "name": "supplier_name",
          "type": "TEXT",
          "description": "Name of the supplier or vendor"
        },
        {
          "name": "billing_address",
          "type": "TEXT",
          "description": "Customer billing address"
        },
        {
          "name": "shipping_address",
          "type": "TEXT",
          "description": "Delivery address"
        },
        {
          "name": "line_items",
          "type": "ARRAY",
          "description": "Individual line items on the purchase order",
          "fields": [
            {
              "name": "product_sku",
              "type": "TEXT",
              "description": "Product SKU or item code"
            },
            {
              "name": "product_name",
              "type": "TEXT",
              "description": "Product name or description"
            },
            {
              "name": "quantity",
              "type": "INTEGER",
              "description": "Quantity ordered"
            },
            {
              "name": "unit_price",
              "type": "CURRENCY_AMOUNT",
              "description": "Price per unit"
            },
            {
              "name": "line_total",
              "type": "CURRENCY_AMOUNT",
              "description": "Total for this line item"
            }
          ]
        },
        {
          "name": "currency",
          "type": "CURRENCY_CODE",
          "description": "Order currency"
        },
        {
          "name": "po_total",
          "type": "CURRENCY_AMOUNT",
          "description": "Total order value"
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
      name: "po-northstar-retail.pdf",
      url: "https://example.com/purchase-orders/po-northstar-retail.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "customer_name", type: "TEXT", description: "Name of the customer or buying company" },
      { name: "po_number", type: "TEXT", description: "Purchase order number" },
      { name: "po_date", type: "DATE", description: "Date the PO was issued" },
      { name: "requested_delivery_date", type: "DATE", description: "Requested delivery date" },
      { name: "supplier_name", type: "TEXT", description: "Name of the supplier or vendor" },
      { name: "billing_address", type: "TEXT", description: "Customer billing address" },
      { name: "shipping_address", type: "TEXT", description: "Delivery address" },
      {
        name: "line_items",
        type: "ARRAY",
        description: "Individual line items on the purchase order",
        fields: [
          { name: "product_sku", type: "TEXT", description: "Product SKU or item code" },
          { name: "product_name", type: "TEXT", description: "Product name or description" },
          { name: "quantity", type: "INTEGER", description: "Quantity ordered" },
          { name: "unit_price", type: "CURRENCY_AMOUNT", description: "Price per unit" },
          { name: "line_total", type: "CURRENCY_AMOUNT", description: "Total for this line item" },
        ],
      },
      { name: "currency", type: "CURRENCY_CODE", description: "Order currency" },
      { name: "po_total", type: "CURRENCY_AMOUNT", description: "Total order value" },
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
            "name": "po-northstar-retail.pdf",
            "url": "https://example.com/purchase-orders/po-northstar-retail.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "customer_name", "type": "TEXT", "description": "Name of the customer or buying company"},
            {"name": "po_number", "type": "TEXT", "description": "Purchase order number"},
            {"name": "po_date", "type": "DATE", "description": "Date the PO was issued"},
            {"name": "requested_delivery_date", "type": "DATE", "description": "Requested delivery date"},
            {"name": "supplier_name", "type": "TEXT", "description": "Name of the supplier or vendor"},
            {"name": "billing_address", "type": "TEXT", "description": "Customer billing address"},
            {"name": "shipping_address", "type": "TEXT", "description": "Delivery address"},
            {
                "name": "line_items",
                "type": "ARRAY",
                "description": "Individual line items on the purchase order",
                "fields": [
                    {"name": "product_sku", "type": "TEXT", "description": "Product SKU or item code"},
                    {"name": "product_name", "type": "TEXT", "description": "Product name or description"},
                    {"name": "quantity", "type": "INTEGER", "description": "Quantity ordered"},
                    {"name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Price per unit"},
                    {"name": "line_total", "type": "CURRENCY_AMOUNT", "description": "Total for this line item"},
                ],
            },
            {"name": "currency", "type": "CURRENCY_CODE", "description": "Order currency"},
            {"name": "po_total", "type": "CURRENCY_AMOUNT", "description": "Total order value"},
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
			il.NewFileFromURL("po-northstar-retail.pdf", "https://example.com/purchase-orders/po-northstar-retail.pdf"),
		},
		Schema: il.ExtractionSchema{
			"customer_name":           il.NewTextFieldConfig("customer_name", "Name of the customer or buying company"),
			"po_number":               il.NewTextFieldConfig("po_number", "Purchase order number"),
			"po_date":                 il.NewDateFieldConfig("po_date", "Date the PO was issued"),
			"requested_delivery_date": il.NewDateFieldConfig("requested_delivery_date", "Requested delivery date"),
			"supplier_name":           il.NewTextFieldConfig("supplier_name", "Name of the supplier or vendor"),
			"billing_address":         il.NewTextFieldConfig("billing_address", "Customer billing address"),
			"shipping_address":        il.NewTextFieldConfig("shipping_address", "Delivery address"),
			"line_items": il.NewArrayFieldConfig(
				"line_items",
				"Individual line items on the purchase order",
				[]il.FieldConfig{
					il.NewTextFieldConfig("product_sku", "Product SKU or item code"),
					il.NewTextFieldConfig("product_name", "Product name or description"),
					il.NewIntegerFieldConfig("quantity", "Quantity ordered"),
					il.NewCurrencyAmountFieldConfig("unit_price", "Price per unit"),
					il.NewCurrencyAmountFieldConfig("line_total", "Total for this line item"),
				},
			),
			"currency": il.NewCurrencyCodeFieldConfig("currency", "Order currency"),
			"po_total": il.NewCurrencyAmountFieldConfig("po_total", "Total order value"),
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
  "name": "Extract purchase order data in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Purchase Order Data\n\nB2B suppliers, procurement teams, and warehouse operators use this recipe to extract purchase order details at scale — pulling out line items, quantities, pricing, and delivery dates for ERP ingestion or order fulfillment workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "f3a4b5c6-d7e8-9012-fabc-789012345601",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Purchase Order Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "f3a4b5c6-d7e8-9012-fabc-789012345602",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "f3a4b5c6-d7e8-9012-fabc-789012345603",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"customer_name\",\"type\":\"TEXT\",\"description\":\"Name of the customer or buying company\"},{\"name\":\"po_number\",\"type\":\"TEXT\",\"description\":\"Purchase order number\"},{\"name\":\"po_date\",\"type\":\"DATE\",\"description\":\"Date the PO was issued\"},{\"name\":\"requested_delivery_date\",\"type\":\"DATE\",\"description\":\"Requested delivery date\"},{\"name\":\"supplier_name\",\"type\":\"TEXT\",\"description\":\"Name of the supplier or vendor\"},{\"name\":\"billing_address\",\"type\":\"TEXT\",\"description\":\"Customer billing address\"},{\"name\":\"shipping_address\",\"type\":\"TEXT\",\"description\":\"Delivery address\"},{\"name\":\"line_items\",\"type\":\"ARRAY\",\"description\":\"Individual line items on the purchase order\",\"fields\":[{\"name\":\"product_sku\",\"type\":\"TEXT\",\"description\":\"Product SKU or item code\"},{\"name\":\"product_name\",\"type\":\"TEXT\",\"description\":\"Product name or description\"},{\"name\":\"quantity\",\"type\":\"INTEGER\",\"description\":\"Quantity ordered\"},{\"name\":\"unit_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Price per unit\"},{\"name\":\"line_total\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total for this line item\"}]},{\"name\":\"currency\",\"type\":\"CURRENCY_CODE\",\"description\":\"Order currency\"},{\"name\":\"po_total\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total order value\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "po-northstar-retail.pdf",
              "fileUrl": "https://example.com/purchase-orders/po-northstar-retail.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "f3a4b5c6-d7e8-9012-fabc-789012345604",
      "name": "Extract Purchase Order Data",
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
            "node": "Extract Purchase Order Data",
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
Extract purchase order data from the file at [file URL]. Use the extract_document tool with these fields:

- customer_name (TEXT): Name of the customer or buying company
- po_number (TEXT): Purchase order number
- po_date (DATE): Date the PO was issued
- requested_delivery_date (DATE): Requested delivery date
- supplier_name (TEXT): Name of the supplier or vendor
- billing_address (TEXT): Customer billing address
- shipping_address (TEXT): Delivery address
- line_items (ARRAY): Each with product_sku (TEXT), product_name (TEXT), quantity (INTEGER), unit_price (CURRENCY_AMOUNT), line_total (CURRENCY_AMOUNT)
- currency (CURRENCY_CODE): Order currency
- po_total (CURRENCY_AMOUNT): Total order value
```

### Response


```json
{
  "success": true,
  "data": {
    "customer_name": {
      "value": "Northstar Retail Group",
      "confidence": 0.99,
      "citations": ["Bill To: Northstar Retail Group"]
    },
    "po_number": {
      "value": "PO-2026-0847",
      "confidence": 0.99,
      "citations": ["Purchase Order No: PO-2026-0847"]
    },
    "po_date": {
      "value": "2026-03-18",
      "confidence": 0.98,
      "citations": ["Date: March 18, 2026"]
    },
    "requested_delivery_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": ["Requested Delivery: April 15, 2026"]
    },
    "supplier_name": {
      "value": "Meridian Manufacturing Co.",
      "confidence": 0.99,
      "citations": ["Vendor: Meridian Manufacturing Co."]
    },
    "billing_address": {
      "value": "450 Commerce Drive, Suite 200, Chicago, IL 60601",
      "confidence": 0.96,
      "citations": ["Billing Address: 450 Commerce Drive, Suite 200, Chicago, IL 60601"]
    },
    "shipping_address": {
      "value": "1200 Warehouse Lane, Dock 4, Indianapolis, IN 46204",
      "confidence": 0.96,
      "citations": ["Ship To: 1200 Warehouse Lane, Dock 4, Indianapolis, IN 46204"]
    },
    "line_items": {
      "value": [
        {
          "product_sku": {
            "value": "SFT-GL-200",
            "confidence": 0.98,
            "citations": ["SFT-GL-200"]
          },
          "product_name": {
            "value": "Industrial Safety Gloves",
            "confidence": 0.98,
            "citations": ["Industrial Safety Gloves"]
          },
          "quantity": {
            "value": 200,
            "confidence": 0.99,
            "citations": ["Qty: 200"]
          },
          "unit_price": {
            "value": {
              "amount": "4.50",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$4.50"]
          },
          "line_total": {
            "value": {
              "amount": "900.00",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$900.00"]
          }
        },
        {
          "product_sku": {
            "value": "SFT-VS-150",
            "confidence": 0.98,
            "citations": ["SFT-VS-150"]
          },
          "product_name": {
            "value": "High-Vis Safety Vests",
            "confidence": 0.98,
            "citations": ["High-Vis Safety Vests"]
          },
          "quantity": {
            "value": 150,
            "confidence": 0.99,
            "citations": ["Qty: 150"]
          },
          "unit_price": {
            "value": {
              "amount": "12.00",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$12.00"]
          },
          "line_total": {
            "value": {
              "amount": "1800.00",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$1,800.00"]
          }
        },
        {
          "product_sku": {
            "value": "SFT-BT-080",
            "confidence": 0.98,
            "citations": ["SFT-BT-080"]
          },
          "product_name": {
            "value": "Steel-Toe Work Boots",
            "confidence": 0.97,
            "citations": ["Steel-Toe Work Boots"]
          },
          "quantity": {
            "value": 80,
            "confidence": 0.99,
            "citations": ["Qty: 80"]
          },
          "unit_price": {
            "value": {
              "amount": "67.50",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$67.50"]
          },
          "line_total": {
            "value": {
              "amount": "5400.00",
              "currency": "USD"
            },
            "confidence": 0.98,
            "citations": ["$5,400.00"]
          }
        }
      ],
      "confidence": 0.97,
      "citations": ["Item table with 3 line items"]
    },
    "currency": {
      "value": "USD",
      "confidence": 0.99,
      "citations": ["Currency: USD"]
    },
    "po_total": {
      "value": {
        "amount": "8100.00",
        "currency": "USD"
      },
      "confidence": 0.99,
      "citations": ["Total: $8,100.00"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
