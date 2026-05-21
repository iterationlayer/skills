---
name: extract-reisekostenabrechnung-data
description: Extract employee, trip, mileage, per diem, expense lines, VAT, and totals from German travel expense claims.
---

# Extract Reisekostenabrechnung Data

Finance teams use this recipe to extract Reisekostenabrechnung fields into structured records for travel reimbursement and accounting workflows.

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
      "name": "reisekostenabrechnung.pdf",
      "url": "https://example.com/documents/reisekostenabrechnung-sample.pdf"
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
        "name": "cost_center",
        "type": "TEXT",
        "description": "Kostenstelle or cost center"
      },
      {
        "name": "trip_purpose",
        "type": "TEXT",
        "description": "Purpose of trip"
      },
      {
        "name": "destination",
        "type": "TEXT",
        "description": "Travel destination"
      },
      {
        "name": "trip_start_date",
        "type": "DATE",
        "description": "Trip start date"
      },
      {
        "name": "trip_end_date",
        "type": "DATE",
        "description": "Trip end date"
      },
      {
        "name": "business_kilometers",
        "type": "DECIMAL",
        "description": "Business kilometers claimed"
      },
      {
        "name": "mileage_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Mileage reimbursement amount"
      },
      {
        "name": "per_diem_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Per diem or Verpflegungsmehraufwand amount"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Travel expense lines",
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
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          }
        ]
      },
      {
        "name": "total_reimbursement",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
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
      "name": "reisekostenabrechnung.pdf",
      "url": "https://example.com/documents/reisekostenabrechnung-sample.pdf"
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
        "name": "cost_center",
        "type": "TEXT",
        "description": "Kostenstelle or cost center"
      },
      {
        "name": "trip_purpose",
        "type": "TEXT",
        "description": "Purpose of trip"
      },
      {
        "name": "destination",
        "type": "TEXT",
        "description": "Travel destination"
      },
      {
        "name": "trip_start_date",
        "type": "DATE",
        "description": "Trip start date"
      },
      {
        "name": "trip_end_date",
        "type": "DATE",
        "description": "Trip end date"
      },
      {
        "name": "business_kilometers",
        "type": "DECIMAL",
        "description": "Business kilometers claimed"
      },
      {
        "name": "mileage_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Mileage reimbursement amount"
      },
      {
        "name": "per_diem_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Per diem or Verpflegungsmehraufwand amount"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Travel expense lines",
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
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          }
        ]
      },
      {
        "name": "total_reimbursement",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
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
      "name": "reisekostenabrechnung.pdf",
      "url": "https://example.com/documents/reisekostenabrechnung-sample.pdf"
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
        "name": "cost_center",
        "type": "TEXT",
        "description": "Kostenstelle or cost center"
      },
      {
        "name": "trip_purpose",
        "type": "TEXT",
        "description": "Purpose of trip"
      },
      {
        "name": "destination",
        "type": "TEXT",
        "description": "Travel destination"
      },
      {
        "name": "trip_start_date",
        "type": "DATE",
        "description": "Trip start date"
      },
      {
        "name": "trip_end_date",
        "type": "DATE",
        "description": "Trip end date"
      },
      {
        "name": "business_kilometers",
        "type": "DECIMAL",
        "description": "Business kilometers claimed"
      },
      {
        "name": "mileage_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Mileage reimbursement amount"
      },
      {
        "name": "per_diem_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Per diem or Verpflegungsmehraufwand amount"
      },
      {
        "name": "expense_lines",
        "type": "ARRAY",
        "description": "Travel expense lines",
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
            "name": "description",
            "type": "TEXT",
            "description": "Expense description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Expense amount"
          }
        ]
      },
      {
        "name": "total_reimbursement",
        "type": "CURRENCY_AMOUNT",
        "description": "Total reimbursement amount"
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
				Name: "reisekostenabrechnung.pdf",
				Url: "https://example.com/documents/reisekostenabrechnung-sample.pdf",
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
					Name: "cost_center",
					Type: "TEXT",
					Description: "Kostenstelle or cost center",
				},
				il.TextFieldConfig{
					Name: "trip_purpose",
					Type: "TEXT",
					Description: "Purpose of trip",
				},
				il.TextFieldConfig{
					Name: "destination",
					Type: "TEXT",
					Description: "Travel destination",
				},
				il.DateFieldConfig{
					Name: "trip_start_date",
					Type: "DATE",
					Description: "Trip start date",
				},
				il.DateFieldConfig{
					Name: "trip_end_date",
					Type: "DATE",
					Description: "Trip end date",
				},
				il.DecimalFieldConfig{
					Name: "business_kilometers",
					Type: "DECIMAL",
					Description: "Business kilometers claimed",
				},
				il.CurrencyAmountFieldConfig{
					Name: "mileage_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Mileage reimbursement amount",
				},
				il.CurrencyAmountFieldConfig{
					Name: "per_diem_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Per diem or Verpflegungsmehraufwand amount",
				},
				il.ArrayFieldConfig{
					Name: "expense_lines",
					Type: "ARRAY",
					Description: "Travel expense lines",
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
							Name: "description",
							Type: "TEXT",
							Description: "Expense description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Expense amount",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "total_reimbursement",
					Type: "CURRENCY_AMOUNT",
					Description: "Total reimbursement amount",
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
  "name": "Extract Reisekostenabrechnung Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Reisekostenabrechnung Data\n\nFinance teams use this recipe to extract Reisekostenabrechnung fields into structured records for travel reimbursement and accounting workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-reisekostenabrechnung-data-overview",
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
      "id": "extract-reisekostenabrechnung-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"employee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee or claimant name\"\n    },\n    {\n      \"name\": \"cost_center\",\n      \"type\": \"TEXT\",\n      \"description\": \"Kostenstelle or cost center\"\n    },\n    {\n      \"name\": \"trip_purpose\",\n      \"type\": \"TEXT\",\n      \"description\": \"Purpose of trip\"\n    },\n    {\n      \"name\": \"destination\",\n      \"type\": \"TEXT\",\n      \"description\": \"Travel destination\"\n    },\n    {\n      \"name\": \"trip_start_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Trip start date\"\n    },\n    {\n      \"name\": \"trip_end_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Trip end date\"\n    },\n    {\n      \"name\": \"business_kilometers\",\n      \"type\": \"DECIMAL\",\n      \"description\": \"Business kilometers claimed\"\n    },\n    {\n      \"name\": \"mileage_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Mileage reimbursement amount\"\n    },\n    {\n      \"name\": \"per_diem_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Per diem or Verpflegungsmehraufwand amount\"\n    },\n    {\n      \"name\": \"expense_lines\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Travel expense lines\",\n      \"fields\": [\n        {\n          \"name\": \"expense_date\",\n          \"type\": \"DATE\",\n          \"description\": \"Expense date\"\n        },\n        {\n          \"name\": \"category\",\n          \"type\": \"TEXT\",\n          \"description\": \"Expense category\"\n        },\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Expense description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Expense amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"total_reimbursement\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Total reimbursement amount\"\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "reisekostenabrechnung.pdf",
              "fileUrl": "https://example.com/documents/reisekostenabrechnung-sample.pdf"
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
      "id": "extract-reisekostenabrechnung-data-extract",
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
Extract reisekostenabrechnung data from the file at [file URL]. Use the extract_document tool with these fields:

- employee_name (TEXT): Employee or claimant name
- cost_center (TEXT): Kostenstelle or cost center
- trip_purpose (TEXT): Purpose of trip
- destination (TEXT): Travel destination
- trip_start_date (DATE): Trip start date
- trip_end_date (DATE): Trip end date
- business_kilometers (DECIMAL): Business kilometers claimed
- mileage_amount (CURRENCY_AMOUNT): Mileage reimbursement amount
- per_diem_amount (CURRENCY_AMOUNT): Per diem or Verpflegungsmehraufwand amount
- expense_lines (ARRAY): Each with expense_date (DATE), category (TEXT), description (TEXT), amount (CURRENCY_AMOUNT)
- total_reimbursement (CURRENCY_AMOUNT): Total reimbursement amount
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
    "cost_center": {
      "value": "Cost Center",
      "confidence": 0.97,
      "citations": [
        "COST CENTER"
      ]
    },
    "trip_purpose": {
      "value": "Trip Purpose",
      "confidence": 0.97,
      "citations": [
        "TRIP PURPOSE"
      ]
    },
    "destination": {
      "value": "Destination",
      "confidence": 0.97,
      "citations": [
        "DESTINATION"
      ]
    },
    "trip_start_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "trip_end_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "business_kilometers": {
      "value": 12.5,
      "confidence": 0.97,
      "citations": [
        "BUSINESS KILOMETERS"
      ]
    },
    "mileage_amount": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "per_diem_amount": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
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
          "description": {
            "value": "Description",
            "confidence": 0.96,
            "citations": [
              "DESCRIPTION"
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
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "EXPENSE LINES"
      ]
    },
    "total_reimbursement": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
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
