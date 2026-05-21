---
name: extract-certificate-of-insurance-data
description: Extract structured fields from certificate of insurance documents.
---

# Extract Certificate of Insurance Data

Insurance teams use this recipe to extract Certificate of Insurance details without manually rekeying policy, claim, coverage, and loss information into claims systems.

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
      "name": "certificate-of-insurance.pdf",
      "url": "https://example.com/documents/certificate-of-insurance-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "policyholder_name",
        "type": "TEXT",
        "description": "Policyholder or insured name"
      },
      {
        "name": "policy_number",
        "type": "TEXT",
        "description": "Policy number"
      },
      {
        "name": "claim_or_certificate_number",
        "type": "TEXT",
        "description": "Claim number or certificate number"
      },
      {
        "name": "insurer_name",
        "type": "TEXT",
        "description": "Insurance carrier or administrator"
      },
      {
        "name": "loss_or_effective_date",
        "type": "DATE",
        "description": "Loss date, effective date, or certificate date"
      },
      {
        "name": "coverage_type",
        "type": "TEXT",
        "description": "Coverage or claim type"
      },
      {
        "name": "coverage_or_loss_lines",
        "type": "ARRAY",
        "description": "Coverage limits, loss entries, or claim line items",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Coverage, loss, or claim line description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Line amount, limit, reserve, or paid value"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total claim, paid, reserve, or coverage amount"
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
      "name": "certificate-of-insurance.pdf",
      "url": "https://example.com/documents/certificate-of-insurance-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "policyholder_name",
        "type": "TEXT",
        "description": "Policyholder or insured name"
      },
      {
        "name": "policy_number",
        "type": "TEXT",
        "description": "Policy number"
      },
      {
        "name": "claim_or_certificate_number",
        "type": "TEXT",
        "description": "Claim number or certificate number"
      },
      {
        "name": "insurer_name",
        "type": "TEXT",
        "description": "Insurance carrier or administrator"
      },
      {
        "name": "loss_or_effective_date",
        "type": "DATE",
        "description": "Loss date, effective date, or certificate date"
      },
      {
        "name": "coverage_type",
        "type": "TEXT",
        "description": "Coverage or claim type"
      },
      {
        "name": "coverage_or_loss_lines",
        "type": "ARRAY",
        "description": "Coverage limits, loss entries, or claim line items",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Coverage, loss, or claim line description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Line amount, limit, reserve, or paid value"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total claim, paid, reserve, or coverage amount"
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
      "name": "certificate-of-insurance.pdf",
      "url": "https://example.com/documents/certificate-of-insurance-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "policyholder_name",
        "type": "TEXT",
        "description": "Policyholder or insured name"
      },
      {
        "name": "policy_number",
        "type": "TEXT",
        "description": "Policy number"
      },
      {
        "name": "claim_or_certificate_number",
        "type": "TEXT",
        "description": "Claim number or certificate number"
      },
      {
        "name": "insurer_name",
        "type": "TEXT",
        "description": "Insurance carrier or administrator"
      },
      {
        "name": "loss_or_effective_date",
        "type": "DATE",
        "description": "Loss date, effective date, or certificate date"
      },
      {
        "name": "coverage_type",
        "type": "TEXT",
        "description": "Coverage or claim type"
      },
      {
        "name": "coverage_or_loss_lines",
        "type": "ARRAY",
        "description": "Coverage limits, loss entries, or claim line items",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Coverage, loss, or claim line description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Line amount, limit, reserve, or paid value"
          }
        ]
      },
      {
        "name": "total_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Total claim, paid, reserve, or coverage amount"
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
				Name: "certificate-of-insurance.pdf",
				Url: "https://example.com/documents/certificate-of-insurance-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "policyholder_name",
					Type: "TEXT",
					Description: "Policyholder or insured name",
				},
				il.TextFieldConfig{
					Name: "policy_number",
					Type: "TEXT",
					Description: "Policy number",
				},
				il.TextFieldConfig{
					Name: "claim_or_certificate_number",
					Type: "TEXT",
					Description: "Claim number or certificate number",
				},
				il.TextFieldConfig{
					Name: "insurer_name",
					Type: "TEXT",
					Description: "Insurance carrier or administrator",
				},
				il.DateFieldConfig{
					Name: "loss_or_effective_date",
					Type: "DATE",
					Description: "Loss date, effective date, or certificate date",
				},
				il.TextFieldConfig{
					Name: "coverage_type",
					Type: "TEXT",
					Description: "Coverage or claim type",
				},
				il.ArrayFieldConfig{
					Name: "coverage_or_loss_lines",
					Type: "ARRAY",
					Description: "Coverage limits, loss entries, or claim line items",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Coverage, loss, or claim line description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Line amount, limit, reserve, or paid value",
						},
					},
				},
				il.CurrencyAmountFieldConfig{
					Name: "total_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Total claim, paid, reserve, or coverage amount",
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
  "name": "Extract Certificate of Insurance Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Certificate of Insurance Data\n\nInsurance teams use this recipe to extract Certificate of Insurance details without manually rekeying policy, claim, coverage, and loss information into claims systems.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-certificate-of-insurance-data-overview",
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
      "id": "extract-certificate-of-insurance-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"policyholder_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Policyholder or insured name\"\n    },\n    {\n      \"name\": \"policy_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Policy number\"\n    },\n    {\n      \"name\": \"claim_or_certificate_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Claim number or certificate number\"\n    },\n    {\n      \"name\": \"insurer_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Insurance carrier or administrator\"\n    },\n    {\n      \"name\": \"loss_or_effective_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Loss date, effective date, or certificate date\"\n    },\n    {\n      \"name\": \"coverage_type\",\n      \"type\": \"TEXT\",\n      \"description\": \"Coverage or claim type\"\n    },\n    {\n      \"name\": \"coverage_or_loss_lines\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Coverage limits, loss entries, or claim line items\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Coverage, loss, or claim line description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Line amount, limit, reserve, or paid value\"\n        }\n      ]\n    },\n    {\n      \"name\": \"total_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Total claim, paid, reserve, or coverage amount\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "certificate-of-insurance.pdf",
              "fileUrl": "https://example.com/documents/certificate-of-insurance-sample.pdf"
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
      "id": "extract-certificate-of-insurance-data-extract",
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
Extract certificate of insurance data from the file at [file URL]. Use the extract_document tool with these fields:

- policyholder_name (TEXT): Policyholder or insured name
- policy_number (TEXT): Policy number
- claim_or_certificate_number (TEXT): Claim number or certificate number
- insurer_name (TEXT): Insurance carrier or administrator
- loss_or_effective_date (DATE): Loss date, effective date, or certificate date
- coverage_type (TEXT): Coverage or claim type
- coverage_or_loss_lines (ARRAY): Each with description (TEXT), amount (CURRENCY_AMOUNT)
- total_amount (CURRENCY_AMOUNT): Total claim, paid, reserve, or coverage amount
```

### Response


```json
{
  "success": true,
  "data": {
    "policyholder_name": {
      "value": "Policyholder Name",
      "confidence": 0.97,
      "citations": [
        "POLICYHOLDER NAME"
      ]
    },
    "policy_number": {
      "value": "Policy Number",
      "confidence": 0.97,
      "citations": [
        "POLICY NUMBER"
      ]
    },
    "claim_or_certificate_number": {
      "value": "Claim Or Certificate Number",
      "confidence": 0.97,
      "citations": [
        "CLAIM OR CERTIFICATE NUMBER"
      ]
    },
    "insurer_name": {
      "value": "Insurer Name",
      "confidence": 0.97,
      "citations": [
        "INSURER NAME"
      ]
    },
    "loss_or_effective_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "coverage_type": {
      "value": "Coverage Type",
      "confidence": 0.97,
      "citations": [
        "COVERAGE TYPE"
      ]
    },
    "coverage_or_loss_lines": {
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
              "currency": "USD"
            },
            "confidence": 0.96,
            "citations": [
              "$1,234.56"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "COVERAGE OR LOSS LINES"
      ]
    },
    "total_amount": {
      "value": {
        "amount": "1234.56",
        "currency": "USD"
      },
      "confidence": 0.97,
      "citations": [
        "$1,234.56"
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
