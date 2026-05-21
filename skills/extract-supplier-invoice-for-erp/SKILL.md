---
name: extract-supplier-invoice-for-erp
description: Extract supplier invoice details structured for direct import into ERP systems like SAP, Oracle, or Microsoft Dynamics.
---

# Extract Supplier Invoice Data for ERP Import

Finance agencies and outsourced AP teams use this recipe to process supplier invoices into structured data that maps directly to ERP import fields — vendor ID, GL codes, tax breakdowns, and payment terms — eliminating manual data entry.

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
        "name": "supplier-invoice-SI-8842.pdf",
        "url": "https://example.com/invoices/supplier-invoice-SI-8842.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "vendor_name",
          "type": "TEXT",
          "description": "Supplier or vendor company name"
        },
        {
          "name": "vendor_tax_id",
          "type": "TEXT",
          "description": "Vendor VAT or tax identification number"
        },
        {
          "name": "vendor_iban",
          "type": "IBAN",
          "description": "Vendor bank account IBAN"
        },
        {
          "name": "invoice_number",
          "type": "TEXT",
          "description": "Supplier invoice number"
        },
        {
          "name": "invoice_date",
          "type": "DATE",
          "description": "Invoice issue date"
        },
        {
          "name": "due_date",
          "type": "DATE",
          "description": "Payment due date"
        },
        {
          "name": "payment_terms",
          "type": "TEXT",
          "description": "Payment terms, e.g. Net 30"
        },
        {
          "name": "currency",
          "type": "CURRENCY_CODE",
          "description": "Invoice currency"
        },
        {
          "name": "line_items",
          "type": "ARRAY",
          "description": "Invoice line items",
          "fields": [
            {
              "name": "description",
              "type": "TEXT",
              "description": "Item or service description"
            },
            {
              "name": "gl_code",
              "type": "TEXT",
              "description": "General ledger account code if present"
            },
            {
              "name": "quantity",
              "type": "DECIMAL",
              "description": "Quantity"
            },
            {
              "name": "unit_price",
              "type": "CURRENCY_AMOUNT",
              "description": "Unit price"
            },
            {
              "name": "line_total",
              "type": "CURRENCY_AMOUNT",
              "description": "Line item total"
            }
          ]
        },
        {
          "name": "subtotal",
          "type": "CURRENCY_AMOUNT",
          "description": "Subtotal before tax"
        },
        {
          "name": "tax_amount",
          "type": "CURRENCY_AMOUNT",
          "description": "Total tax amount"
        },
        {
          "name": "tax_rate",
          "type": "DECIMAL",
          "description": "Tax rate as a percentage"
        },
        {
          "name": "total_amount",
          "type": "CURRENCY_AMOUNT",
          "description": "Total amount due"
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
      name: "supplier-invoice-SI-8842.pdf",
      url: "https://example.com/invoices/supplier-invoice-SI-8842.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "vendor_name", type: "TEXT", description: "Supplier or vendor company name" },
      { name: "vendor_tax_id", type: "TEXT", description: "Vendor VAT or tax identification number" },
      { name: "vendor_iban", type: "IBAN", description: "Vendor bank account IBAN" },
      { name: "invoice_number", type: "TEXT", description: "Supplier invoice number" },
      { name: "invoice_date", type: "DATE", description: "Invoice issue date" },
      { name: "due_date", type: "DATE", description: "Payment due date" },
      { name: "payment_terms", type: "TEXT", description: "Payment terms, e.g. Net 30" },
      { name: "currency", type: "CURRENCY_CODE", description: "Invoice currency" },
      {
        name: "line_items",
        type: "ARRAY",
        description: "Invoice line items",
        fields: [
          { name: "description", type: "TEXT", description: "Item or service description" },
          { name: "gl_code", type: "TEXT", description: "General ledger account code if present" },
          { name: "quantity", type: "DECIMAL", description: "Quantity" },
          { name: "unit_price", type: "CURRENCY_AMOUNT", description: "Unit price" },
          { name: "line_total", type: "CURRENCY_AMOUNT", description: "Line item total" },
        ],
      },
      { name: "subtotal", type: "CURRENCY_AMOUNT", description: "Subtotal before tax" },
      { name: "tax_amount", type: "CURRENCY_AMOUNT", description: "Total tax amount" },
      { name: "tax_rate", type: "DECIMAL", description: "Tax rate as a percentage" },
      { name: "total_amount", type: "CURRENCY_AMOUNT", description: "Total amount due" },
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
            "name": "supplier-invoice-SI-8842.pdf",
            "url": "https://example.com/invoices/supplier-invoice-SI-8842.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "vendor_name", "type": "TEXT", "description": "Supplier or vendor company name"},
            {"name": "vendor_tax_id", "type": "TEXT", "description": "Vendor VAT or tax identification number"},
            {"name": "vendor_iban", "type": "IBAN", "description": "Vendor bank account IBAN"},
            {"name": "invoice_number", "type": "TEXT", "description": "Supplier invoice number"},
            {"name": "invoice_date", "type": "DATE", "description": "Invoice issue date"},
            {"name": "due_date", "type": "DATE", "description": "Payment due date"},
            {"name": "payment_terms", "type": "TEXT", "description": "Payment terms, e.g. Net 30"},
            {"name": "currency", "type": "CURRENCY_CODE", "description": "Invoice currency"},
            {
                "name": "line_items",
                "type": "ARRAY",
                "description": "Invoice line items",
                "fields": [
                    {"name": "description", "type": "TEXT", "description": "Item or service description"},
                    {"name": "gl_code", "type": "TEXT", "description": "General ledger account code if present"},
                    {"name": "quantity", "type": "DECIMAL", "description": "Quantity"},
                    {"name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Unit price"},
                    {"name": "line_total", "type": "CURRENCY_AMOUNT", "description": "Line item total"},
                ],
            },
            {"name": "subtotal", "type": "CURRENCY_AMOUNT", "description": "Subtotal before tax"},
            {"name": "tax_amount", "type": "CURRENCY_AMOUNT", "description": "Total tax amount"},
            {"name": "tax_rate", "type": "DECIMAL", "description": "Tax rate as a percentage"},
            {"name": "total_amount", "type": "CURRENCY_AMOUNT", "description": "Total amount due"},
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
			il.FileInput{
				Type: "url",
				Name: "supplier-invoice-SI-8842.pdf",
				Url: "https://example.com/invoices/supplier-invoice-SI-8842.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "vendor_name",
					Type: "TEXT",
					Description: "Supplier or vendor company name",
				},
				il.TextFieldConfig{
					Name: "vendor_tax_id",
					Type: "TEXT",
					Description: "Vendor VAT or tax identification number",
				},
				il.IbanFieldConfig{
					Name: "vendor_iban",
					Type: "IBAN",
					Description: "Vendor bank account IBAN",
				},
				il.TextFieldConfig{
					Name: "invoice_number",
					Type: "TEXT",
					Description: "Supplier invoice number",
				},
				il.DateFieldConfig{
					Name: "invoice_date",
					Type: "DATE",
					Description: "Invoice issue date",
				},
				il.DateFieldConfig{
					Name: "due_date",
					Type: "DATE",
					Description: "Payment due date",
				},
				il.TextFieldConfig{
					Name: "payment_terms",
					Type: "TEXT",
					Description: "Payment terms, e.g. Net 30",
				},
				il.CurrencyCodeFieldConfig{
					Name: "currency",
					Type: "CURRENCY_CODE",
					Description: "Invoice currency",
				},
				il.ArrayFieldConfig{
					Name: "line_items",
					Type: "ARRAY",
					Description: "Invoice line items",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Item or service description",
						},
						il.TextFieldConfig{
							Name: "gl_code",
							Type: "TEXT",
							Description: "General ledger account code if present",
						},
						il.DecimalFieldConfig{
							Name: "quantity",
							Type: "DECIMAL",
							Description: "Quantity",
						},
						il.CurrencyAmountFieldConfig{
							Name: "unit_price",
							Type: "CURRENCY_AMOUNT",
							Description: "Unit price",
						},
						il.CurrencyAmountFieldConfig{
							Name: "line_total",
							Type: "CURRENCY_AMOUNT",
							Description: "Line item total",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "subtotal",
					Type: "CURRENCY_AMOUNT",
					Description: "Subtotal before tax",
				},
				il.CurrencyAmountFieldConfig{
					Name: "tax_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Total tax amount",
				},
				il.DecimalFieldConfig{
					Name: "tax_rate",
					Type: "DECIMAL",
					Description: "Tax rate as a percentage",
				},
				il.CurrencyAmountFieldConfig{
					Name: "total_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Total amount due",
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
  "name": "Extract supplier invoice data for ERP import in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Supplier Invoice Data for ERP Import\n\nFinance agencies and outsourced AP teams use this recipe to process supplier invoices into structured data that maps directly to ERP import fields — vendor ID, GL codes, tax breakdowns, and payment terms — eliminating manual data entry.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "a7b8c9d0-e1f2-3456-abcd-456789012301",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Invoice Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "a7b8c9d0-e1f2-3456-abcd-456789012302",
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
      "id": "a7b8c9d0-e1f2-3456-abcd-456789012303",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"vendor_name\",\"type\":\"TEXT\",\"description\":\"Supplier or vendor company name\"},{\"name\":\"vendor_tax_id\",\"type\":\"TEXT\",\"description\":\"Vendor VAT or tax identification number\"},{\"name\":\"vendor_iban\",\"type\":\"IBAN\",\"description\":\"Vendor bank account IBAN\"},{\"name\":\"invoice_number\",\"type\":\"TEXT\",\"description\":\"Supplier invoice number\"},{\"name\":\"invoice_date\",\"type\":\"DATE\",\"description\":\"Invoice issue date\"},{\"name\":\"due_date\",\"type\":\"DATE\",\"description\":\"Payment due date\"},{\"name\":\"payment_terms\",\"type\":\"TEXT\",\"description\":\"Payment terms\"},{\"name\":\"currency\",\"type\":\"CURRENCY_CODE\",\"description\":\"Invoice currency\"},{\"name\":\"line_items\",\"type\":\"ARRAY\",\"description\":\"Invoice line items\",\"fields\":[{\"name\":\"description\",\"type\":\"TEXT\",\"description\":\"Item or service description\"},{\"name\":\"gl_code\",\"type\":\"TEXT\",\"description\":\"General ledger account code\"},{\"name\":\"quantity\",\"type\":\"DECIMAL\",\"description\":\"Quantity\"},{\"name\":\"unit_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Unit price\"},{\"name\":\"line_total\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Line item total\"}]},{\"name\":\"subtotal\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Subtotal before tax\"},{\"name\":\"tax_amount\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total tax amount\"},{\"name\":\"tax_rate\",\"type\":\"DECIMAL\",\"description\":\"Tax rate as a percentage\"},{\"name\":\"total_amount\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total amount due\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "supplier-invoice-SI-8842.pdf",
              "fileUrl": "https://example.com/invoices/supplier-invoice-SI-8842.pdf"
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
      "id": "a7b8c9d0-e1f2-3456-abcd-456789012304",
      "name": "Extract Invoice Data",
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
            "node": "Extract Invoice Data",
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
Extract supplier invoice data for ERP import from the file at [file URL]. Use the extract_document tool with these fields:

- vendor_name (TEXT): Supplier or vendor company name
- vendor_tax_id (TEXT): Vendor VAT or tax identification number
- vendor_iban (IBAN): Vendor bank account IBAN
- invoice_number (TEXT): Supplier invoice number
- invoice_date (DATE): Invoice issue date
- due_date (DATE): Payment due date
- payment_terms (TEXT): Payment terms, e.g. Net 30
- currency (CURRENCY_CODE): Invoice currency
- line_items (ARRAY): Each with description (TEXT), gl_code (TEXT), quantity (DECIMAL), unit_price (CURRENCY_AMOUNT), line_total (CURRENCY_AMOUNT)
- subtotal (CURRENCY_AMOUNT): Subtotal before tax
- tax_amount (CURRENCY_AMOUNT): Total tax amount
- tax_rate (DECIMAL): Tax rate as a percentage
- total_amount (CURRENCY_AMOUNT): Total amount due
```

### Response


```json
{
  "success": true,
  "data": {
    "vendor_name": {
      "value": "Brenner Industrial Supply GmbH",
      "confidence": 0.98,
      "citations": [
        "Brenner Industrial Supply GmbH"
      ]
    },
    "vendor_tax_id": {
      "value": "DE298374561",
      "confidence": 0.97,
      "citations": [
        "USt-IdNr.: DE298374561"
      ]
    },
    "vendor_iban": {
      "value": "DE89370400440532013000",
      "confidence": 0.96,
      "citations": [
        "IBAN: DE89 3704 0044 0532 0130 00"
      ]
    },
    "invoice_number": {
      "value": "SI-8842",
      "confidence": 0.99,
      "citations": [
        "Rechnung Nr. SI-8842"
      ]
    },
    "invoice_date": {
      "value": "2026-03-20",
      "confidence": 0.98,
      "citations": [
        "Datum: 20.03.2026"
      ]
    },
    "due_date": {
      "value": "2026-04-19",
      "confidence": 0.97,
      "citations": [
        "Zahlungsziel: 19.04.2026"
      ]
    },
    "payment_terms": {
      "value": "Net 30",
      "confidence": 0.95,
      "citations": [
        "Zahlungsbedingungen: 30 Tage netto"
      ]
    },
    "currency": {
      "value": "EUR",
      "confidence": 0.99,
      "citations": [
        "EUR"
      ]
    },
    "line_items": {
      "value": [
        {
          "description": {
            "value": "Hydraulic press maintenance kit",
            "confidence": 0.96,
            "citations": [
              "Hydraulic press maintenance kit"
            ]
          },
          "gl_code": {
            "value": "6200",
            "confidence": 0.88,
            "citations": [
              "Konto 6200"
            ]
          },
          "quantity": {
            "value": 2.0,
            "confidence": 0.98,
            "citations": [
              "Menge: 2"
            ]
          },
          "unit_price": {
            "value": {
              "amount": "340.00",
              "currency": "EUR"
            },
            "confidence": 0.97,
            "citations": [
              "340,00 EUR"
            ]
          },
          "line_total": {
            "value": {
              "amount": "680.00",
              "currency": "EUR"
            },
            "confidence": 0.97,
            "citations": [
              "680,00 EUR"
            ]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    },
    "subtotal": {
      "value": {
        "amount": "680.00",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "Nettobetrag: 680,00 EUR"
      ]
    },
    "tax_amount": {
      "value": {
        "amount": "129.20",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "MwSt. 19%: 129,20 EUR"
      ]
    },
    "tax_rate": {
      "value": 19.0,
      "confidence": 0.98,
      "citations": [
        "19%"
      ]
    },
    "total_amount": {
      "value": {
        "amount": "809.20",
        "currency": "EUR"
      },
      "confidence": 0.98,
      "citations": [
        "Gesamtbetrag: 809,20 EUR"
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
