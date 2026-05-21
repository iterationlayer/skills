---
name: extract-keihi-seisansho-data
description: Extract employee, department, expense lines, tax, totals, and approval fields from Japanese expense settlement forms.
---

# Extract Keihi Seisansho Data

Finance teams use this recipe to extract keihi seisansho fields into structured reimbursement records for approvals and accounting systems.

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
      "name": "keihi-seisansho.pdf",
      "url": "https://example.com/documents/keihi-seisansho-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee name"
      },
      {
        "name": "employee_number",
        "type": "TEXT",
        "description": "Employee number"
      },
      {
        "name": "department",
        "type": "TEXT",
        "description": "Department"
      },
      {
        "name": "submission_date",
        "type": "DATE",
        "description": "Submission date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense settlement lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or payee"
          },
          {
            "name": "purpose",
            "type": "TEXT",
            "description": "Business purpose"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          },
          {
            "name": "consumption_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Consumption tax amount if shown"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total settlement amount"
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
      "name": "keihi-seisansho.pdf",
      "url": "https://example.com/documents/keihi-seisansho-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee name"
      },
      {
        "name": "employee_number",
        "type": "TEXT",
        "description": "Employee number"
      },
      {
        "name": "department",
        "type": "TEXT",
        "description": "Department"
      },
      {
        "name": "submission_date",
        "type": "DATE",
        "description": "Submission date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense settlement lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or payee"
          },
          {
            "name": "purpose",
            "type": "TEXT",
            "description": "Business purpose"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          },
          {
            "name": "consumption_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Consumption tax amount if shown"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total settlement amount"
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
      "name": "keihi-seisansho.pdf",
      "url": "https://example.com/documents/keihi-seisansho-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "employee_name",
        "type": "TEXT",
        "description": "Employee name"
      },
      {
        "name": "employee_number",
        "type": "TEXT",
        "description": "Employee number"
      },
      {
        "name": "department",
        "type": "TEXT",
        "description": "Department"
      },
      {
        "name": "submission_date",
        "type": "DATE",
        "description": "Submission date"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Expense settlement lines",
        "fields": [
          {
            "name": "expense_date",
            "type": "DATE",
            "description": "Expense date"
          },
          {
            "name": "category",
            "type": "TEXT",
            "description": "Expense category"
          },
          {
            "name": "merchant",
            "type": "TEXT",
            "description": "Merchant or payee"
          },
          {
            "name": "purpose",
            "type": "TEXT",
            "description": "Business purpose"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          },
          {
            "name": "consumption_tax",
            "type": "CURRENCY_AMOUNT",
            "description": "Consumption tax amount if shown"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total settlement amount"
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
				Name: "keihi-seisansho.pdf",
				Url: "https://example.com/documents/keihi-seisansho-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "employee_name",
					Type: "TEXT",
					Description: "Employee name",
				},
				il.TextFieldConfig{
					Name: "employee_number",
					Type: "TEXT",
					Description: "Employee number",
				},
				il.TextFieldConfig{
					Name: "department",
					Type: "TEXT",
					Description: "Department",
				},
				il.DateFieldConfig{
					Name: "submission_date",
					Type: "DATE",
					Description: "Submission date",
				},
				il.ArrayFieldConfig{
					Name: "expense_lines",
					Type: "ARRAY",
					Description: "Expense settlement lines",
					Fields: []any{
						il.DateFieldConfig{
							Name: "expense_date",
							Type: "DATE",
							Description: "Expense date",
						},
						il.TextFieldConfig{
							Name: "category",
							Type: "TEXT",
							Description: "Expense category",
						},
						il.TextFieldConfig{
							Name: "merchant",
							Type: "TEXT",
							Description: "Merchant or payee",
						},
						il.TextFieldConfig{
							Name: "purpose",
							Type: "TEXT",
							Description: "Business purpose",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Expense amount",
						},
						il.CurrencyAmountFieldConfig{
							Name: "consumption_tax",
							Type: "CURRENCY_AMOUNT",
							Description: "Consumption tax amount if shown",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "total_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Total settlement amount",
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
  "name": "Extract Keihi Seisansho Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Keihi Seisansho Data\n\nFinance teams use this recipe to extract keihi seisansho fields into structured reimbursement records for approvals and accounting systems.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-keihi-seisansho-data-overview",
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
      "id": "extract-keihi-seisansho-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"employee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee name\"\n    },\n    {\n      \"name\": \"employee_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee number\"\n    },\n    {\n      \"name\": \"department\",\n      \"type\": \"TEXT\",\n      \"description\": \"Department\"\n    },\n    {\n      \"name\": \"submission_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Submission date\"\n    },\n    {\n      \"name\": \"expense_lines\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Expense settlement lines\",\n      \"fields\": [\n        {\n          \"name\": \"expense_date\",\n          \"type\": \"DATE\",\n          \"description\": \"Expense date\"\n        },\n        {\n          \"name\": \"category\",\n          \"type\": \"TEXT\",\n          \"description\": \"Expense category\"\n        },\n        {\n          \"name\": \"merchant\",\n          \"type\": \"TEXT\",\n          \"description\": \"Merchant or payee\"\n        },\n        {\n          \"name\": \"purpose\",\n          \"type\": \"TEXT\",\n          \"description\": \"Business purpose\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Expense amount\"\n        },\n        {\n          \"name\": \"consumption_tax\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Consumption tax amount if shown\"\n        }\n      ]\n    },\n    {\n      \"name\": \"total_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Total settlement amount\"\n    },\n    {\n      \"name\": \"approval_status\",\n      \"type\": \"TEXT\",\n      \"description\": \"Approval status if shown\"\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "keihi-seisansho.pdf",
              "fileUrl": "https://example.com/documents/keihi-seisansho-sample.pdf"
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
      "id": "extract-keihi-seisansho-data-extract",
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
Extract keihi seisansho data from the file at [file URL]. Use the extract_document tool with these fields:

- employee_name (TEXT): Employee name
- employee_number (TEXT): Employee number
- department (TEXT): Department
- submission_date (DATE): Submission date
- expense_lines (ARRAY): Each with expense_date (DATE), category (TEXT), merchant (TEXT), purpose (TEXT), amount (CURRENCY_AMOUNT), consumption_tax (CURRENCY_AMOUNT)
- total_amount (CURRENCY_AMOUNT): Total settlement amount
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
    "employee_number": {
      "value": "Employee Number",
      "confidence": 0.97,
      "citations": [
        "EMPLOYEE NUMBER"
      ]
    },
    "department": {
      "value": "Department",
      "confidence": 0.97,
      "citations": [
        "DEPARTMENT"
      ]
    },
    "submission_date": {
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
          "purpose": {
            "value": "Purpose",
            "confidence": 0.96,
            "citations": [
              "PURPOSE"
            ]
          },
          "amount": {
            "value": {
              "amount": "1234.56",
              "currency": "EUR"
            },
            "confidence": 0.96,
            "citations": [
              "1,234.56"
            ]
          },
          "consumption_tax": {
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
