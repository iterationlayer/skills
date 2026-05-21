---
name: extract-fiche-de-paie-data
description: Extract employee, employer, gross pay, cotisations, tax, and net pay from French payslips.
---

# Extract Fiche de Paie Data

Payroll and HR teams use this recipe to extract French fiche de paie fields into structured JSON for employee records, payroll checks, and accounting reconciliation.

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
      "name": "fiche-de-paie.pdf",
      "url": "https://example.com/documents/fiche-de-paie-sample.pdf"
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
        "name": "employee_social_security_number",
        "type": "TEXT",
        "description": "Employee social security number if shown"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "employer_siret",
        "type": "TEXT",
        "description": "Employer SIRET number"
      },
      {
        "name": "collective_agreement",
        "type": "TEXT",
        "description": "Convention collective if shown"
      },
      {
        "name": "pay_period_start",
        "type": "DATE",
        "description": "Pay period start date"
      },
      {
        "name": "pay_period_end",
        "type": "DATE",
        "description": "Pay period end date"
      },
      {
        "name": "gross_salary",
        "type": "CURRENCY_AMOUNT",
        "description": "Salaire brut"
      },
      {
        "name": "cotisations",
        "type": "ARRAY",
        "description": "Employee and employer social contributions",
        "fields": [
          {
            "name": "label",
            "type": "TEXT",
            "description": "Contribution label"
          },
          {
            "name": "employee_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employee contribution amount"
          },
          {
            "name": "employer_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employer contribution amount"
          }
        ]
      },
      {
        "name": "income_tax_withheld",
        "type": "CURRENCY_AMOUNT",
        "description": "Prelevement a la source amount"
      },
      {
        "name": "net_pay_before_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Net a payer avant impot"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Net amount paid"
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
      "name": "fiche-de-paie.pdf",
      "url": "https://example.com/documents/fiche-de-paie-sample.pdf"
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
        "name": "employee_social_security_number",
        "type": "TEXT",
        "description": "Employee social security number if shown"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "employer_siret",
        "type": "TEXT",
        "description": "Employer SIRET number"
      },
      {
        "name": "collective_agreement",
        "type": "TEXT",
        "description": "Convention collective if shown"
      },
      {
        "name": "pay_period_start",
        "type": "DATE",
        "description": "Pay period start date"
      },
      {
        "name": "pay_period_end",
        "type": "DATE",
        "description": "Pay period end date"
      },
      {
        "name": "gross_salary",
        "type": "CURRENCY_AMOUNT",
        "description": "Salaire brut"
      },
      {
        "name": "cotisations",
        "type": "ARRAY",
        "description": "Employee and employer social contributions",
        "fields": [
          {
            "name": "label",
            "type": "TEXT",
            "description": "Contribution label"
          },
          {
            "name": "employee_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employee contribution amount"
          },
          {
            "name": "employer_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employer contribution amount"
          }
        ]
      },
      {
        "name": "income_tax_withheld",
        "type": "CURRENCY_AMOUNT",
        "description": "Prelevement a la source amount"
      },
      {
        "name": "net_pay_before_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Net a payer avant impot"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Net amount paid"
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
      "name": "fiche-de-paie.pdf",
      "url": "https://example.com/documents/fiche-de-paie-sample.pdf"
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
        "name": "employee_social_security_number",
        "type": "TEXT",
        "description": "Employee social security number if shown"
      },
      {
        "name": "employer_name",
        "type": "TEXT",
        "description": "Employer name"
      },
      {
        "name": "employer_siret",
        "type": "TEXT",
        "description": "Employer SIRET number"
      },
      {
        "name": "collective_agreement",
        "type": "TEXT",
        "description": "Convention collective if shown"
      },
      {
        "name": "pay_period_start",
        "type": "DATE",
        "description": "Pay period start date"
      },
      {
        "name": "pay_period_end",
        "type": "DATE",
        "description": "Pay period end date"
      },
      {
        "name": "gross_salary",
        "type": "CURRENCY_AMOUNT",
        "description": "Salaire brut"
      },
      {
        "name": "cotisations",
        "type": "ARRAY",
        "description": "Employee and employer social contributions",
        "fields": [
          {
            "name": "label",
            "type": "TEXT",
            "description": "Contribution label"
          },
          {
            "name": "employee_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employee contribution amount"
          },
          {
            "name": "employer_amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Employer contribution amount"
          }
        ]
      },
      {
        "name": "income_tax_withheld",
        "type": "CURRENCY_AMOUNT",
        "description": "Prelevement a la source amount"
      },
      {
        "name": "net_pay_before_tax",
        "type": "CURRENCY_AMOUNT",
        "description": "Net a payer avant impot"
      },
      {
        "name": "net_pay",
        "type": "CURRENCY_AMOUNT",
        "description": "Net amount paid"
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
				Name: "fiche-de-paie.pdf",
				Url: "https://example.com/documents/fiche-de-paie-sample.pdf",
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
					Name: "employee_social_security_number",
					Type: "TEXT",
					Description: "Employee social security number if shown",
				},
				il.TextFieldConfig{
					Name: "employer_name",
					Type: "TEXT",
					Description: "Employer name",
				},
				il.TextFieldConfig{
					Name: "employer_siret",
					Type: "TEXT",
					Description: "Employer SIRET number",
				},
				il.TextFieldConfig{
					Name: "collective_agreement",
					Type: "TEXT",
					Description: "Convention collective if shown",
				},
				il.DateFieldConfig{
					Name: "pay_period_start",
					Type: "DATE",
					Description: "Pay period start date",
				},
				il.DateFieldConfig{
					Name: "pay_period_end",
					Type: "DATE",
					Description: "Pay period end date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "gross_salary",
					Type: "CURRENCY_AMOUNT",
					Description: "Salaire brut",
				},
				il.ArrayFieldConfig{
					Name: "cotisations",
					Type: "ARRAY",
					Description: "Employee and employer social contributions",
					Fields: []any{
						il.TextFieldConfig{
							Name: "label",
							Type: "TEXT",
							Description: "Contribution label",
						},
						il.CurrencyAmountFieldConfig{
							Name: "employee_amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Employee contribution amount",
						},
						il.CurrencyAmountFieldConfig{
							Name: "employer_amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Employer contribution amount",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "income_tax_withheld",
					Type: "CURRENCY_AMOUNT",
					Description: "Prelevement a la source amount",
				},
				il.CurrencyAmountFieldConfig{
					Name: "net_pay_before_tax",
					Type: "CURRENCY_AMOUNT",
					Description: "Net a payer avant impot",
				},
				il.CurrencyAmountFieldConfig{
					Name: "net_pay",
					Type: "CURRENCY_AMOUNT",
					Description: "Net amount paid",
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
  "name": "Extract Fiche de Paie Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Fiche de Paie Data\n\nPayroll and HR teams use this recipe to extract French fiche de paie fields into structured JSON for employee records, payroll checks, and accounting reconciliation.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-fiche-de-paie-data-overview",
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
      "id": "extract-fiche-de-paie-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"employee_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee name\"\n    },\n    {\n      \"name\": \"employee_social_security_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employee social security number if shown\"\n    },\n    {\n      \"name\": \"employer_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employer name\"\n    },\n    {\n      \"name\": \"employer_siret\",\n      \"type\": \"TEXT\",\n      \"description\": \"Employer SIRET number\"\n    },\n    {\n      \"name\": \"collective_agreement\",\n      \"type\": \"TEXT\",\n      \"description\": \"Convention collective if shown\"\n    },\n    {\n      \"name\": \"pay_period_start\",\n      \"type\": \"DATE\",\n      \"description\": \"Pay period start date\"\n    },\n    {\n      \"name\": \"pay_period_end\",\n      \"type\": \"DATE\",\n      \"description\": \"Pay period end date\"\n    },\n    {\n      \"name\": \"gross_salary\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Salaire brut\"\n    },\n    {\n      \"name\": \"cotisations\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Employee and employer social contributions\",\n      \"fields\": [\n        {\n          \"name\": \"label\",\n          \"type\": \"TEXT\",\n          \"description\": \"Contribution label\"\n        },\n        {\n          \"name\": \"employee_amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Employee contribution amount\"\n        },\n        {\n          \"name\": \"employer_amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Employer contribution amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"income_tax_withheld\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Prelevement a la source amount\"\n    },\n    {\n      \"name\": \"net_pay_before_tax\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Net a payer avant impot\"\n    },\n    {\n      \"name\": \"net_pay\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Net amount paid\"\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "fiche-de-paie.pdf",
              "fileUrl": "https://example.com/documents/fiche-de-paie-sample.pdf"
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
      "id": "extract-fiche-de-paie-data-extract",
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
Extract fiche de paie data from the file at [file URL]. Use the extract_document tool with these fields:

- employee_name (TEXT): Employee name
- employee_social_security_number (TEXT): Employee social security number if shown
- employer_name (TEXT): Employer name
- employer_siret (TEXT): Employer SIRET number
- collective_agreement (TEXT): Convention collective if shown
- pay_period_start (DATE): Pay period start date
- pay_period_end (DATE): Pay period end date
- gross_salary (CURRENCY_AMOUNT): Salaire brut
- cotisations (ARRAY): Each with label (TEXT), employee_amount (CURRENCY_AMOUNT), employer_amount (CURRENCY_AMOUNT)
- income_tax_withheld (CURRENCY_AMOUNT): Prelevement a la source amount
- net_pay_before_tax (CURRENCY_AMOUNT): Net a payer avant impot
- net_pay (CURRENCY_AMOUNT): Net amount paid
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
    "employee_social_security_number": {
      "value": "Employee Social Security Number",
      "confidence": 0.97,
      "citations": [
        "EMPLOYEE SOCIAL SECURITY NUMBER"
      ]
    },
    "employer_name": {
      "value": "Northwind GmbH",
      "confidence": 0.97,
      "citations": [
        "EMPLOYER NAME"
      ]
    },
    "employer_siret": {
      "value": "Employer Siret",
      "confidence": 0.97,
      "citations": [
        "EMPLOYER SIRET"
      ]
    },
    "collective_agreement": {
      "value": "Collective Agreement",
      "confidence": 0.97,
      "citations": [
        "COLLECTIVE AGREEMENT"
      ]
    },
    "pay_period_start": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "pay_period_end": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "gross_salary": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "cotisations": {
      "value": [
        {
          "label": {
            "value": "Label",
            "confidence": 0.96,
            "citations": [
              "LABEL"
            ]
          },
          "employee_amount": {
            "value": {
              "amount": "1234.56",
              "currency": "EUR"
            },
            "confidence": 0.96,
            "citations": [
              "1,234.56"
            ]
          },
          "employer_amount": {
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
        "COTISATIONS"
      ]
    },
    "income_tax_withheld": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "net_pay_before_tax": {
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
