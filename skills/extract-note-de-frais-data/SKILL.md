---
name: extract-note-de-frais-data
description: Extract employee, trip, expense lines, VAT, totals, and approvals from French expense claims.
---

# Extract Note de Frais Data

Finance teams use this recipe to extract note de frais fields into structured expense records for reimbursement, approval, and accounting workflows.

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
      "name": "note-de-frais.pdf",
      "url": "https://example.com/documents/note-de-frais-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee or claimant name"
      },
      {
        "name": "employee_id",
        "type": "TEXT",
        "description": "Employee identifier if shown"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "claim_period_start",
        "type": "DATE",
        "description": "Expense claim period start date"
      },
      {
        "name": "claim_period_end",
        "type": "DATE",
        "description": "Expense claim period end date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category such as meal, hotel, mileage, taxi, train, or parking"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or provider"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount_including_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Amount including tax"
          },
          {
            "name": "vat_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "VAT amount"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
      },
      {
        "name": "approval_status",
        "type": "TEXT",
        "description": "Approval status if shown"
      },
      {
        "name": "currency",
        "type": "CURRENCY_CODE",
        "description": "Currency code"
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
      "name": "note-de-frais.pdf",
      "url": "https://example.com/documents/note-de-frais-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee or claimant name"
      },
      {
        "name": "employee_id",
        "type": "TEXT",
        "description": "Employee identifier if shown"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "claim_period_start",
        "type": "DATE",
        "description": "Expense claim period start date"
      },
      {
        "name": "claim_period_end",
        "type": "DATE",
        "description": "Expense claim period end date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category such as meal, hotel, mileage, taxi, train, or parking"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or provider"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount_including_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Amount including tax"
          },
          {
            "name": "vat_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "VAT amount"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
      },
      {
        "name": "approval_status",
        "type": "TEXT",
        "description": "Approval status if shown"
      },
      {
        "name": "currency",
        "type": "CURRENCY_CODE",
        "description": "Currency code"
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
      "name": "note-de-frais.pdf",
      "url": "https://example.com/documents/note-de-frais-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee or claimant name"
      },
      {
        "name": "employee_id",
        "type": "TEXT",
        "description": "Employee identifier if shown"
      },
      {
        "name": "department_or_cost_center",
        "type": "TEXT",
        "description": "Department, project, or cost center"
      },
      {
        "name": "claim_period_start",
        "type": "DATE",
        "description": "Expense claim period start date"
      },
      {
        "name": "claim_period_end",
        "type": "DATE",
        "description": "Expense claim period end date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category such as meal, hotel, mileage, taxi, train, or parking"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or provider"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount_including_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Amount including tax"
          },
          {
            "name": "vat_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "VAT amount"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
      },
      {
        "name": "approval_status",
        "type": "TEXT",
        "description": "Approval status if shown"
      },
      {
        "name": "currency",
        "type": "CURRENCY_CODE",
        "description": "Currency code"
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
				Name: "note-de-frais.pdf",
				Url: "https://example.com/documents/note-de-frais-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "employee_name",
					Type: "TEXT",
					Description: "Employee or claimant name",
				},
				il.TextFieldConfig{
					Name: "employee_id",
					Type: "TEXT",
					Description: "Employee identifier if shown",
				},
				il.TextFieldConfig{
					Name: "department_or_cost_center",
					Type: "TEXT",
					Description: "Department, project, or cost center",
				},
				il.DateFieldConfig{
					Name: "claim_period_start",
					Type: "DATE",
					Description: "Expense claim period start date",
				},
				il.DateFieldConfig{
					Name: "claim_period_end",
					Type: "DATE",
					Description: "Expense claim period end date",
				},
				il.ArrayFieldConfig{
					Name: "expense_lines",
					Type: "ARRAY",
					Description: "Expense lines",
					Fields: []any{
						il.DateFieldConfig{
							Name: "expense_date",
							Type: "DATE",
							Description: "Expense date",
						},
						il.TextFieldConfig{
							Name: "category",
							Type: "TEXT",
							Description: "Expense category such as meal, hotel, mileage, taxi, train, or parking",
						},
						il.TextFieldConfig{
							Name: "merchant",
							Type: "TEXT",
							Description: "Merchant or provider",
						},
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Expense description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount_including_tax",
							Type: "CURRENCY_AMOUNT",
							Description: "Amount including tax",
						},
						il.CurrencyAmountFieldConfig{
							Name: "vat_amount",
							Type: "CURRENCY_AMOUNT",
							Description: "VAT amount",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "total_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Total reimbursement amount",
				},
				il.TextFieldConfig{
					Name: "approval_status",
					Type: "TEXT",
					Description: "Approval status if shown",
				},
				il.CurrencyCodeFieldConfig{
					Name: "currency",
					Type: "CURRENCY_CODE",
					Description: "Currency code",
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
  "name": "Extract Note de Frais Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Note de Frais Data\n\nFinance teams use this recipe to extract note de frais fields into structured expense records for reimbursement, approval, and accounting workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-note-de-frais-data-overview",
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
      "id": "extract-note-de-frais-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"employee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee or claimant name\"\n    },\n    {\n      \"name\": \"employee_id\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee identifier if shown\"\n    },\n    {\n      \"name\": \"department_or_cost_center\",\n      \"type\": \"TEXT\",\n      \"description\": \"Department, project, or cost center\"\n    },\n    {\n      \"name\": \"claim_period_start\",\n      \"type\": \"DATE\",\n      \"description\": \"Expense claim period start date\"\n    },\n    {\n      \"name\": \"claim_period_end\",\n      \"type\": \"DATE\",\n      \"description\": \"Expense claim period end date\"\n    },\n    {\n      \"name\": \"expense_lines\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Expense lines\",\n      \"fields\": [\n        {\n          \"name\": \"expense_date\",\n          \"type\": \"DATE\",\n          \"description\": \"Expense date\"\n        },\n        {\n          \"name\": \"category\",\n          \"type\": \"TEXT\",\n          \"description\": \"Expense category such as meal, hotel, mileage, taxi, train, or parking\"\n        },\n        {\n          \"name\": \"merchant\",\n          \"type\": \"TEXT\",\n          \"description\": \"Merchant or provider\"\n        },\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Expense description\"\n        },\n        {\n          \"name\": \"amount_including_tax\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Amount including tax\"\n        },\n        {\n          \"name\": \"vat_amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"VAT amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"total_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Total reimbursement amount\"\n    },\n    {\n      \"name\": \"approval_status\",\n      \"type\": \"TEXT\",\n      \"description\": \"Approval status if shown\"\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "note-de-frais.pdf",
              "fileUrl": "https://example.com/documents/note-de-frais-sample.pdf"
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
      "id": "extract-note-de-frais-data-extract",
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
Extract note de frais data from the file at [file URL]. Use the extract_document tool with these fields:

- employee_name (TEXT): Employee or claimant name
- employee_id (TEXT): Employee identifier if shown
- department_or_cost_center (TEXT): Department, project, or cost center
- claim_period_start (DATE): Expense claim period start date
- claim_period_end (DATE): Expense claim period end date
- expense_lines (ARRAY): Each with expense_date (DATE), category (TEXT), merchant (TEXT), description (TEXT), amount_including_tax (CURRENCY_AMOUNT), vat_amount (CURRENCY_AMOUNT)
- total_amount (CURRENCY_AMOUNT): Total reimbursement amount
- approval_status (TEXT): Approval status if shown
- currency (CURRENCY_CODE): Currency code
```

### Response


```json
{
  "success": true,
  "data": {
    "employee_name": {
      "value": "Maria Keller",
      "confidence": 0.97,
      "citations": [
        "EMPLOYEE NAME"
      ]
    },
    "employee_id": {
      "value": "EMP-1042",
      "confidence": 0.97,
      "citations": [
        "EMPLOYEE ID"
      ]
    },
    "department_or_cost_center": {
      "value": "Department Or Cost Center",
      "confidence": 0.97,
      "citations": [
        "DEPARTMENT OR COST CENTER"
      ]
    },
    "claim_period_start": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "claim_period_end": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "expense_lines": {
      "value": [
        {
          "expense_date": {
            "value": "2026-04-15",
            "confidence": 0.96,
            "citations": [
              "15 Apr 2026"
            ]
          },
          "category": {
            "value": "Category",
            "confidence": 0.96,
            "citations": [
              "CATEGORY"
            ]
          },
          "merchant": {
            "value": "Merchant",
            "confidence": 0.96,
            "citations": [
              "MERCHANT"
            ]
          },
          "description": {
            "value": "Description",
            "confidence": 0.96,
            "citations": [
              "DESCRIPTION"
            ]
          },
          "amount_including_tax": {
            "value": {
              "amount": "1234.56",
              "currency": "EUR"
            },
            "confidence": 0.96,
            "citations": [
              "1,234.56"
            ]
          },
          "vat_amount": {
            "value": {
              "amount": "1234.56",
              "currency": "EUR"
            },
            "confidence": 0.96,
            "citations": [
              "1,234.56"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "EXPENSE LINES"
      ]
    },
    "total_amount": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "approval_status": {
      "value": "Approval Status",
      "confidence": 0.97,
      "citations": [
        "APPROVAL STATUS"
      ]
    },
    "currency": {
      "value": "EUR",
      "confidence": 0.97,
      "citations": [
        "CURRENCY"
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
