---
name: extract-legal-invoice-data
description: Extract timekeeper entries, disbursements, matter references, and billing summaries from law firm invoices.
---

# Extract Legal Invoice Data

Corporate legal departments and legal ops agencies use this recipe to process outside counsel invoices — extracting timekeeper rates, hours, disbursements, and matter codes for e-billing systems and cost analysis.

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
        "name": "legal-invoice-HW-2026-0341.pdf",
        "url": "https://example.com/legal-billing/invoice-HW-2026-0341.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "law_firm",
          "type": "TEXT",
          "description": "Name of the law firm"
        },
        {
          "name": "invoice_number",
          "type": "TEXT",
          "description": "Invoice number"
        },
        {
          "name": "invoice_date",
          "type": "DATE",
          "description": "Invoice date"
        },
        {
          "name": "client_name",
          "type": "TEXT",
          "description": "Client name"
        },
        {
          "name": "matter_number",
          "type": "TEXT",
          "description": "Matter or case reference number"
        },
        {
          "name": "matter_description",
          "type": "TEXT",
          "description": "Description of the matter"
        },
        {
          "name": "billing_period",
          "type": "TEXT",
          "description": "Billing period covered"
        },
        {
          "name": "time_entries",
          "type": "ARRAY",
          "description": "Individual time entries",
          "fields": [
            {
              "name": "timekeeper",
              "type": "TEXT",
              "description": "Name and title of the timekeeper"
            },
            {
              "name": "date",
              "type": "DATE",
              "description": "Date of the time entry"
            },
            {
              "name": "description",
              "type": "TEXT",
              "description": "Description of work performed"
            },
            {
              "name": "hours",
              "type": "DECIMAL",
              "description": "Hours billed"
            },
            {
              "name": "rate",
              "type": "CURRENCY_AMOUNT",
              "description": "Hourly rate"
            },
            {
              "name": "amount",
              "type": "CURRENCY_AMOUNT",
              "description": "Total amount for this entry"
            }
          ]
        },
        {
          "name": "total_fees",
          "type": "CURRENCY_AMOUNT",
          "description": "Total professional fees"
        },
        {
          "name": "total_disbursements",
          "type": "CURRENCY_AMOUNT",
          "description": "Total disbursements and expenses"
        },
        {
          "name": "total_amount",
          "type": "CURRENCY_AMOUNT",
          "description": "Total invoice amount"
        },
        {
          "name": "payment_terms",
          "type": "TEXT",
          "description": "Payment terms"
        }
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractDocument({
  files: [{ type: "url", name: "legal-invoice-HW-2026-0341.pdf", url: "https://example.com/legal-billing/invoice-HW-2026-0341.pdf" }],
  schema: {
    fields: [
      { name: "law_firm", type: "TEXT", description: "Name of the law firm" },
      { name: "invoice_number", type: "TEXT", description: "Invoice number" },
      { name: "invoice_date", type: "DATE", description: "Invoice date" },
      { name: "client_name", type: "TEXT", description: "Client name" },
      { name: "matter_number", type: "TEXT", description: "Matter or case reference number" },
      { name: "matter_description", type: "TEXT", description: "Description of the matter" },
      { name: "billing_period", type: "TEXT", description: "Billing period covered" },
      {
        name: "time_entries", type: "ARRAY", description: "Individual time entries",
        fields: [
          { name: "timekeeper", type: "TEXT", description: "Name and title of the timekeeper" },
          { name: "date", type: "DATE", description: "Date of the time entry" },
          { name: "description", type: "TEXT", description: "Description of work performed" },
          { name: "hours", type: "DECIMAL", description: "Hours billed" },
          { name: "rate", type: "CURRENCY_AMOUNT", description: "Hourly rate" },
          { name: "amount", type: "CURRENCY_AMOUNT", description: "Total amount for this entry" },
        ],
      },
      { name: "total_fees", type: "CURRENCY_AMOUNT", description: "Total professional fees" },
      { name: "total_disbursements", type: "CURRENCY_AMOUNT", description: "Total disbursements and expenses" },
      { name: "total_amount", type: "CURRENCY_AMOUNT", description: "Total invoice amount" },
      { name: "payment_terms", type: "TEXT", description: "Payment terms" },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(
    files=[{"type": "url", "name": "legal-invoice-HW-2026-0341.pdf", "url": "https://example.com/legal-billing/invoice-HW-2026-0341.pdf"}],
    schema={
        "fields": [
            {"name": "law_firm", "type": "TEXT", "description": "Name of the law firm"},
            {"name": "invoice_number", "type": "TEXT", "description": "Invoice number"},
            {"name": "invoice_date", "type": "DATE", "description": "Invoice date"},
            {"name": "client_name", "type": "TEXT", "description": "Client name"},
            {"name": "matter_number", "type": "TEXT", "description": "Matter or case reference number"},
            {"name": "matter_description", "type": "TEXT", "description": "Description of the matter"},
            {"name": "billing_period", "type": "TEXT", "description": "Billing period covered"},
            {
                "name": "time_entries", "type": "ARRAY", "description": "Individual time entries",
                "fields": [
                    {"name": "timekeeper", "type": "TEXT", "description": "Name and title of the timekeeper"},
                    {"name": "date", "type": "DATE", "description": "Date of the time entry"},
                    {"name": "description", "type": "TEXT", "description": "Description of work performed"},
                    {"name": "hours", "type": "DECIMAL", "description": "Hours billed"},
                    {"name": "rate", "type": "CURRENCY_AMOUNT", "description": "Hourly rate"},
                    {"name": "amount", "type": "CURRENCY_AMOUNT", "description": "Total amount for this entry"},
                ],
            },
            {"name": "total_fees", "type": "CURRENCY_AMOUNT", "description": "Total professional fees"},
            {"name": "total_disbursements", "type": "CURRENCY_AMOUNT", "description": "Total disbursements and expenses"},
            {"name": "total_amount", "type": "CURRENCY_AMOUNT", "description": "Total invoice amount"},
            {"name": "payment_terms", "type": "TEXT", "description": "Payment terms"},
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
			il.NewFileFromURL("legal-invoice-HW-2026-0341.pdf", "https://example.com/legal-billing/invoice-HW-2026-0341.pdf"),
		},
		Schema: il.ExtractionSchema{
			"law_firm":             il.NewTextFieldConfig("law_firm", "Name of the law firm"),
			"invoice_number":      il.NewTextFieldConfig("invoice_number", "Invoice number"),
			"invoice_date":        il.NewDateFieldConfig("invoice_date", "Invoice date"),
			"client_name":         il.NewTextFieldConfig("client_name", "Client name"),
			"matter_number":       il.NewTextFieldConfig("matter_number", "Matter or case reference number"),
			"matter_description":  il.NewTextFieldConfig("matter_description", "Description of the matter"),
			"billing_period":      il.NewTextFieldConfig("billing_period", "Billing period covered"),
			"time_entries": il.NewArrayFieldConfig("time_entries", "Individual time entries", []il.FieldConfig{
				il.NewTextFieldConfig("timekeeper", "Name and title of the timekeeper"),
				il.NewDateFieldConfig("date", "Date of the time entry"),
				il.NewTextFieldConfig("description", "Description of work performed"),
				il.NewDecimalFieldConfig("hours", "Hours billed"),
				il.NewCurrencyAmountFieldConfig("rate", "Hourly rate"),
				il.NewCurrencyAmountFieldConfig("amount", "Total amount for this entry"),
			}),
			"total_fees":          il.NewCurrencyAmountFieldConfig("total_fees", "Total professional fees"),
			"total_disbursements": il.NewCurrencyAmountFieldConfig("total_disbursements", "Total disbursements"),
			"total_amount":        il.NewCurrencyAmountFieldConfig("total_amount", "Total invoice amount"),
			"payment_terms":       il.NewTextFieldConfig("payment_terms", "Payment terms"),
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
  "name": "Extract legal invoice data in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Legal Invoice Data\n\nCorporate legal departments and legal ops agencies use this recipe to process outside counsel invoices — extracting timekeeper rates, hours, disbursements, and matter codes for e-billing systems and cost analysis.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "c5d6e7f8-a9b0-1234-cdef-234567890101",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Legal Invoice\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "c5d6e7f8-a9b0-1234-cdef-234567890102",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "c5d6e7f8-a9b0-1234-cdef-234567890103",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"law_firm\",\"type\":\"TEXT\",\"description\":\"Law firm name\"},{\"name\":\"invoice_number\",\"type\":\"TEXT\",\"description\":\"Invoice number\"},{\"name\":\"invoice_date\",\"type\":\"DATE\",\"description\":\"Invoice date\"},{\"name\":\"client_name\",\"type\":\"TEXT\",\"description\":\"Client name\"},{\"name\":\"matter_number\",\"type\":\"TEXT\",\"description\":\"Matter number\"},{\"name\":\"matter_description\",\"type\":\"TEXT\",\"description\":\"Matter description\"},{\"name\":\"billing_period\",\"type\":\"TEXT\",\"description\":\"Billing period\"},{\"name\":\"time_entries\",\"type\":\"ARRAY\",\"description\":\"Time entries\",\"fields\":[{\"name\":\"timekeeper\",\"type\":\"TEXT\",\"description\":\"Timekeeper\"},{\"name\":\"date\",\"type\":\"DATE\",\"description\":\"Date\"},{\"name\":\"description\",\"type\":\"TEXT\",\"description\":\"Work description\"},{\"name\":\"hours\",\"type\":\"DECIMAL\",\"description\":\"Hours\"},{\"name\":\"rate\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Rate\"},{\"name\":\"amount\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Amount\"}]},{\"name\":\"total_fees\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total fees\"},{\"name\":\"total_disbursements\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Disbursements\"},{\"name\":\"total_amount\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total amount\"},{\"name\":\"payment_terms\",\"type\":\"TEXT\",\"description\":\"Payment terms\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "legal-invoice-HW-2026-0341.pdf",
              "fileUrl": "https://example.com/legal-billing/invoice-HW-2026-0341.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "c5d6e7f8-a9b0-1234-cdef-234567890104",
      "name": "Extract Legal Invoice",
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
            "node": "Extract Legal Invoice",
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
Extract legal invoice data from the file at [file URL]. Use the extract_document tool with these fields:

- law_firm (TEXT): Name of the law firm
- invoice_number (TEXT): Invoice number
- invoice_date (DATE): Invoice date
- client_name (TEXT): Client name
- matter_number (TEXT): Matter or case reference number
- matter_description (TEXT): Description of the matter
- billing_period (TEXT): Billing period covered
- time_entries (ARRAY): Each with timekeeper (TEXT), date (DATE), description (TEXT), hours (DECIMAL), rate (CURRENCY_AMOUNT), amount (CURRENCY_AMOUNT)
- total_fees (CURRENCY_AMOUNT): Total professional fees
- total_disbursements (CURRENCY_AMOUNT): Total disbursements and expenses
- total_amount (CURRENCY_AMOUNT): Total invoice amount
- payment_terms (TEXT): Payment terms
```

### Response


```json
{
  "success": true,
  "data": {
    "law_firm": {
      "value": "Harrison & Whitfield LLP",
      "confidence": 0.99,
      "citations": ["Harrison & Whitfield LLP"]
    },
    "invoice_number": {
      "value": "HW-2026-0341",
      "confidence": 0.99,
      "citations": ["Invoice No. HW-2026-0341"]
    },
    "invoice_date": {
      "value": "2026-03-31",
      "confidence": 0.98,
      "citations": ["Date: March 31, 2026"]
    },
    "client_name": {
      "value": "Pinnacle Industries Inc.",
      "confidence": 0.98,
      "citations": ["Client: Pinnacle Industries Inc."]
    },
    "matter_number": {
      "value": "PIN-2025-0047",
      "confidence": 0.97,
      "citations": ["Matter No. PIN-2025-0047"]
    },
    "matter_description": {
      "value": "Employment litigation — wrongful termination defense",
      "confidence": 0.95,
      "citations": ["Re: Employment Litigation — Wrongful Termination Defense"]
    },
    "billing_period": {
      "value": "March 1–31, 2026",
      "confidence": 0.97,
      "citations": ["For services rendered: March 1–31, 2026"]
    },
    "time_entries": {
      "value": [
        {
          "timekeeper": {
            "value": "Sarah Chen, Partner",
            "confidence": 0.97,
            "citations": ["Sarah Chen, Partner"]
          },
          "date": {
            "value": "2026-03-05",
            "confidence": 0.96,
            "citations": ["03/05/2026"]
          },
          "description": {
            "value": "Review amended complaint; conference with client re discovery strategy",
            "confidence": 0.94,
            "citations": ["Review amended complaint; conf. w/ client re discovery strategy"]
          },
          "hours": {
            "value": 3.5,
            "confidence": 0.98,
            "citations": ["3.5"]
          },
          "rate": {
            "value": {
              "amount": "475.00",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["$475.00/hr"]
          },
          "amount": {
            "value": {
              "amount": "1662.50",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["$1,662.50"]
          }
        },
        {
          "timekeeper": {
            "value": "David Park, Associate",
            "confidence": 0.97,
            "citations": ["David Park, Associate"]
          },
          "date": {
            "value": "2026-03-12",
            "confidence": 0.96,
            "citations": ["03/12/2026"]
          },
          "description": {
            "value": "Draft responses to interrogatories; document review (142 documents)",
            "confidence": 0.93,
            "citations": ["Draft responses to interrogatories; doc review (142 docs)"]
          },
          "hours": {
            "value": 6.2,
            "confidence": 0.97,
            "citations": ["6.2"]
          },
          "rate": {
            "value": {
              "amount": "325.00",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["$325.00/hr"]
          },
          "amount": {
            "value": {
              "amount": "2015.00",
              "currency": "USD"
            },
            "confidence": 0.97,
            "citations": ["$2,015.00"]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    },
    "total_fees": {
      "value": {
        "amount": "3677.50",
        "currency": "USD"
      },
      "confidence": 0.98,
      "citations": ["Total Professional Fees: $3,677.50"]
    },
    "total_disbursements": {
      "value": {
        "amount": "247.80",
        "currency": "USD"
      },
      "confidence": 0.96,
      "citations": ["Disbursements: $247.80"]
    },
    "total_amount": {
      "value": {
        "amount": "3925.30",
        "currency": "USD"
      },
      "confidence": 0.98,
      "citations": ["Total Due: $3,925.30"]
    },
    "payment_terms": {
      "value": "Net 30",
      "confidence": 0.95,
      "citations": ["Payment Terms: Net 30"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
