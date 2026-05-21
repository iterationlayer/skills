---
name: extract-lohnabrechnung-data
description: Extract employee, employer, gross wage, tax deductions, social insurance, and net wage from German payslips.
---

# Extract Lohnabrechnung Data

Payroll and HR teams use this recipe to extract German Lohnabrechnung fields into structured JSON for payroll verification and accounting workflows.

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
      "name": "lohnabrechnung.pdf",
      "url": "https://example.com/documents/lohnabrechnung-sample.pdf"
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
        "name": "personnel_number",
        "type": "TEXT",
        "description": "Personalnummer or payroll employee number"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "pay_period",
        "type": "DATE",
        "description": "Payroll period date"
      },
      {
        "name": "tax_class",
        "type": "TEXT",
        "description": "Steuerklasse"
      },
      {
        "name": "tax_id",
        "type": "TEXT",
        "description": "Steuer-ID if shown"
      },
      {
        "name": "gross_wage",
        "type": "CURRENCY_AMOUNT",
        "description": "Gesamtbrutto or gross wage"
      },
      {
        "name": "earnings",
        "type": "ARRAY",
        "description": "Earnings and wage components",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Earning description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Earning amount"
          }
        ]
      },
      {
        "name": "deductions",
        "type": "ARRAY",
        "description": "Tax and social insurance deductions",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Deduction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Deduction amount"
          }
        ]
      },
      {
        "name": "wage_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Lohnsteuer"
      },
      {
        "name": "church_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Kirchensteuer if shown"
      },
      {
        "name": "solidarity_surcharge",
        "type": "CURRENCY_AMOUNT",
        "description": "Solidaritaetszuschlag if shown"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Auszahlungsbetrag or net pay"
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
      "name": "lohnabrechnung.pdf",
      "url": "https://example.com/documents/lohnabrechnung-sample.pdf"
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
        "name": "personnel_number",
        "type": "TEXT",
        "description": "Personalnummer or payroll employee number"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "pay_period",
        "type": "DATE",
        "description": "Payroll period date"
      },
      {
        "name": "tax_class",
        "type": "TEXT",
        "description": "Steuerklasse"
      },
      {
        "name": "tax_id",
        "type": "TEXT",
        "description": "Steuer-ID if shown"
      },
      {
        "name": "gross_wage",
        "type": "CURRENCY_AMOUNT",
        "description": "Gesamtbrutto or gross wage"
      },
      {
        "name": "earnings",
        "type": "ARRAY",
        "description": "Earnings and wage components",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Earning description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Earning amount"
          }
        ]
      },
      {
        "name": "deductions",
        "type": "ARRAY",
        "description": "Tax and social insurance deductions",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Deduction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Deduction amount"
          }
        ]
      },
      {
        "name": "wage_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Lohnsteuer"
      },
      {
        "name": "church_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Kirchensteuer if shown"
      },
      {
        "name": "solidarity_surcharge",
        "type": "CURRENCY_AMOUNT",
        "description": "Solidaritaetszuschlag if shown"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Auszahlungsbetrag or net pay"
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
      "name": "lohnabrechnung.pdf",
      "url": "https://example.com/documents/lohnabrechnung-sample.pdf"
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
        "name": "personnel_number",
        "type": "TEXT",
        "description": "Personalnummer or payroll employee number"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "pay_period",
        "type": "DATE",
        "description": "Payroll period date"
      },
      {
        "name": "tax_class",
        "type": "TEXT",
        "description": "Steuerklasse"
      },
      {
        "name": "tax_id",
        "type": "TEXT",
        "description": "Steuer-ID if shown"
      },
      {
        "name": "gross_wage",
        "type": "CURRENCY_AMOUNT",
        "description": "Gesamtbrutto or gross wage"
      },
      {
        "name": "earnings",
        "type": "ARRAY",
        "description": "Earnings and wage components",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Earning description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Earning amount"
          }
        ]
      },
      {
        "name": "deductions",
        "type": "ARRAY",
        "description": "Tax and social insurance deductions",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Deduction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Deduction amount"
          }
        ]
      },
      {
        "name": "wage_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Lohnsteuer"
      },
      {
        "name": "church_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Kirchensteuer if shown"
      },
      {
        "name": "solidarity_surcharge",
        "type": "CURRENCY_AMOUNT",
        "description": "Solidaritaetszuschlag if shown"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Auszahlungsbetrag or net pay"
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
				Name: "lohnabrechnung.pdf",
				Url: "https://example.com/documents/lohnabrechnung-sample.pdf",
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
					Name: "personnel_number",
					Type: "TEXT",
					Description: "Personalnummer or payroll employee number",
				},
				il.TextFieldConfig{
					Name: "employer_name",
					Type: "TEXT",
					Description: "Employer name",
				},
				il.DateFieldConfig{
					Name: "pay_period",
					Type: "DATE",
					Description: "Payroll period date",
				},
				il.TextFieldConfig{
					Name: "tax_class",
					Type: "TEXT",
					Description: "Steuerklasse",
				},
				il.TextFieldConfig{
					Name: "tax_id",
					Type: "TEXT",
					Description: "Steuer-ID if shown",
				},
				il.CurrencyAmountFieldConfig{
					Name: "gross_wage",
					Type: "CURRENCY_AMOUNT",
					Description: "Gesamtbrutto or gross wage",
				},
				il.ArrayFieldConfig{
					Name: "earnings",
					Type: "ARRAY",
					Description: "Earnings and wage components",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Earning description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Earning amount",
						},
					},
				},
				il.ArrayFieldConfig{
					Name: "deductions",
					Type: "ARRAY",
					Description: "Tax and social insurance deductions",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Deduction description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Deduction amount",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "wage_tax",
					Type: "CURRENCY_AMOUNT",
					Description: "Lohnsteuer",
				},
				il.CurrencyAmountFieldConfig{
					Name: "church_tax",
					Type: "CURRENCY_AMOUNT",
					Description: "Kirchensteuer if shown",
				},
				il.CurrencyAmountFieldConfig{
					Name: "solidarity_surcharge",
					Type: "CURRENCY_AMOUNT",
					Description: "Solidaritaetszuschlag if shown",
				},
				il.CurrencyAmountFieldConfig{
					Name: "net_pay",
					Type: "CURRENCY_AMOUNT",
					Description: "Auszahlungsbetrag or net pay",
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
  "name": "Extract Lohnabrechnung Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Lohnabrechnung Data\n\nPayroll and HR teams use this recipe to extract German Lohnabrechnung fields into structured JSON for payroll verification and accounting workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-lohnabrechnung-data-overview",
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
      "id": "extract-lohnabrechnung-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"employee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee name\"\n    },\n    {\n      \"name\": \"personnel_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Personalnummer or payroll employee number\"\n    },\n    {\n      \"name\": \"employer_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employer name\"\n    },\n    {\n      \"name\": \"pay_period\",\n      \"type\": \"DATE\",\n      \"description\": \"Payroll period date\"\n    },\n    {\n      \"name\": \"tax_class\",\n      \"type\": \"TEXT\",\n      \"description\": \"Steuerklasse\"\n    },\n    {\n      \"name\": \"tax_id\",\n      \"type\": \"TEXT\",\n      \"description\": \"Steuer-ID if shown\"\n    },\n    {\n      \"name\": \"gross_wage\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Gesamtbrutto or gross wage\"\n    },\n    {\n      \"name\": \"earnings\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Earnings and wage components\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Earning description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Earning amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"deductions\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Tax and social insurance deductions\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Deduction description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Deduction amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"wage_tax\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Lohnsteuer\"\n    },\n    {\n      \"name\": \"church_tax\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Kirchensteuer if shown\"\n    },\n    {\n      \"name\": \"solidarity_surcharge\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Solidaritaetszuschlag if shown\"\n    },\n    {\n      \"name\": \"net_pay\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Auszahlungsbetrag or net pay\"\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "lohnabrechnung.pdf",
              "fileUrl": "https://example.com/documents/lohnabrechnung-sample.pdf"
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
      "id": "extract-lohnabrechnung-data-extract",
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
Extract lohnabrechnung data from the file at [file URL]. Use the extract_document tool with these fields:

- employee_name (TEXT): Employee name
- personnel_number (TEXT): Personalnummer or payroll employee number
- employer_name (TEXT): Employer name
- pay_period (DATE): Payroll period date
- tax_class (TEXT): Steuerklasse
- tax_id (TEXT): Steuer-ID if shown
- gross_wage (CURRENCY_AMOUNT): Gesamtbrutto or gross wage
- earnings (ARRAY): Each with description (TEXT), amount (CURRENCY_AMOUNT)
- deductions (ARRAY): Each with description (TEXT), amount (CURRENCY_AMOUNT)
- wage_tax (CURRENCY_AMOUNT): Lohnsteuer
- church_tax (CURRENCY_AMOUNT): Kirchensteuer if shown
- solidarity_surcharge (CURRENCY_AMOUNT): Solidaritaetszuschlag if shown
- net_pay (CURRENCY_AMOUNT): Auszahlungsbetrag or net pay
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
    "personnel_number": {
      "value": "Personnel Number",
      "confidence": 0.97,
      "citations": [
        "PERSONNEL NUMBER"
      ]
    },
    "employer_name": {
      "value": "Northwind GmbH",
      "confidence": 0.97,
      "citations": [
        "EMPLOYER NAME"
      ]
    },
    "pay_period": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "tax_class": {
      "value": "Tax Class",
      "confidence": 0.97,
      "citations": [
        "TAX CLASS"
      ]
    },
    "tax_id": {
      "value": "Tax Id",
      "confidence": 0.97,
      "citations": [
        "TAX ID"
      ]
    },
    "gross_wage": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "earnings": {
      "value": [
        {
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
        "EARNINGS"
      ]
    },
    "deductions": {
      "value": [
        {
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
        "DEDUCTIONS"
      ]
    },
    "wage_tax": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "church_tax": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "solidarity_surcharge": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "net_pay": {
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
