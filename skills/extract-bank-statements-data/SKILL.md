---
name: extract-bank-statements-data
description: Extract structured fields from bank statements documents.
---

# Extract Bank Statements Data

Finance teams use this recipe to extract Bank Statements details and transaction tables from PDFs instead of manually rekeying statement activity.

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
      "name": "bank-statements.pdf",
      "url": "https://example.com/documents/bank-statements-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "account_holder",
        "type": "TEXT",
        "description": "Account holder name"
      },
      {
        "name": "institution_name",
        "type": "TEXT",
        "description": "Bank, card issuer, or financial institution name"
      },
      {
        "name": "account_number",
        "type": "TEXT",
        "description": "Masked account or card number"
      },
      {
        "name": "statement_start_date",
        "type": "DATE",
        "description": "Statement period start date"
      },
      {
        "name": "statement_end_date",
        "type": "DATE",
        "description": "Statement period end date"
      },
      {
        "name": "opening_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Opening balance"
      },
      {
        "name": "closing_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Closing balance"
      },
      {
        "name": "transactions",
        "type": "ARRAY",
        "description": "Statement transaction lines",
        "fields": [
          {
            "name": "transaction_date",
            "type": "DATE",
            "description": "Transaction date"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Transaction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Transaction amount"
          }
        ]
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
      "name": "bank-statements.pdf",
      "url": "https://example.com/documents/bank-statements-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "account_holder",
        "type": "TEXT",
        "description": "Account holder name"
      },
      {
        "name": "institution_name",
        "type": "TEXT",
        "description": "Bank, card issuer, or financial institution name"
      },
      {
        "name": "account_number",
        "type": "TEXT",
        "description": "Masked account or card number"
      },
      {
        "name": "statement_start_date",
        "type": "DATE",
        "description": "Statement period start date"
      },
      {
        "name": "statement_end_date",
        "type": "DATE",
        "description": "Statement period end date"
      },
      {
        "name": "opening_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Opening balance"
      },
      {
        "name": "closing_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Closing balance"
      },
      {
        "name": "transactions",
        "type": "ARRAY",
        "description": "Statement transaction lines",
        "fields": [
          {
            "name": "transaction_date",
            "type": "DATE",
            "description": "Transaction date"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Transaction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Transaction amount"
          }
        ]
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
      "name": "bank-statements.pdf",
      "url": "https://example.com/documents/bank-statements-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "account_holder",
        "type": "TEXT",
        "description": "Account holder name"
      },
      {
        "name": "institution_name",
        "type": "TEXT",
        "description": "Bank, card issuer, or financial institution name"
      },
      {
        "name": "account_number",
        "type": "TEXT",
        "description": "Masked account or card number"
      },
      {
        "name": "statement_start_date",
        "type": "DATE",
        "description": "Statement period start date"
      },
      {
        "name": "statement_end_date",
        "type": "DATE",
        "description": "Statement period end date"
      },
      {
        "name": "opening_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Opening balance"
      },
      {
        "name": "closing_balance",
        "type": "CURRENCY_AMOUNT",
        "description": "Closing balance"
      },
      {
        "name": "transactions",
        "type": "ARRAY",
        "description": "Statement transaction lines",
        "fields": [
          {
            "name": "transaction_date",
            "type": "DATE",
            "description": "Transaction date"
          },
          {
            "name": "description",
            "type": "TEXT",
            "description": "Transaction description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Transaction amount"
          }
        ]
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
				Name: "bank-statements.pdf",
				Url: "https://example.com/documents/bank-statements-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "account_holder",
					Type: "TEXT",
					Description: "Account holder name",
				},
				il.TextFieldConfig{
					Name: "institution_name",
					Type: "TEXT",
					Description: "Bank, card issuer, or financial institution name",
				},
				il.TextFieldConfig{
					Name: "account_number",
					Type: "TEXT",
					Description: "Masked account or card number",
				},
				il.DateFieldConfig{
					Name: "statement_start_date",
					Type: "DATE",
					Description: "Statement period start date",
				},
				il.DateFieldConfig{
					Name: "statement_end_date",
					Type: "DATE",
					Description: "Statement period end date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "opening_balance",
					Type: "CURRENCY_AMOUNT",
					Description: "Opening balance",
				},
				il.CurrencyAmountFieldConfig{
					Name: "closing_balance",
					Type: "CURRENCY_AMOUNT",
					Description: "Closing balance",
				},
				il.ArrayFieldConfig{
					Name: "transactions",
					Type: "ARRAY",
					Description: "Statement transaction lines",
					Fields: []any{
						il.DateFieldConfig{
							Name: "transaction_date",
							Type: "DATE",
							Description: "Transaction date",
						},
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Transaction description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Transaction amount",
						},
					},
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
  "name": "Extract Bank Statements Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Bank Statements Data\n\nFinance teams use this recipe to extract Bank Statements details and transaction tables from PDFs instead of manually rekeying statement activity.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-bank-statements-data-overview",
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
      "id": "extract-bank-statements-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"account_holder\",\n      \"type\": \"TEXT\",\n      \"description\": \"Account holder name\"\n    },\n    {\n      \"name\": \"institution_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Bank, card issuer, or financial institution name\"\n    },\n    {\n      \"name\": \"account_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Masked account or card number\"\n    },\n    {\n      \"name\": \"statement_start_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Statement period start date\"\n    },\n    {\n      \"name\": \"statement_end_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Statement period end date\"\n    },\n    {\n      \"name\": \"opening_balance\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Opening balance\"\n    },\n    {\n      \"name\": \"closing_balance\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Closing balance\"\n    },\n    {\n      \"name\": \"transactions\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Statement transaction lines\",\n      \"fields\": [\n        {\n          \"name\": \"transaction_date\",\n          \"type\": \"DATE\",\n          \"description\": \"Transaction date\"\n        },\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Transaction description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Transaction amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"currency\",\n      \"type\": \"CURRENCY_CODE\",\n      \"description\": \"Currency code\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "bank-statements.pdf",
              "fileUrl": "https://example.com/documents/bank-statements-sample.pdf"
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
      "id": "extract-bank-statements-data-extract",
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
Extract bank statements data from the file at [file URL]. Use the extract_document tool with these fields:

- account_holder (TEXT): Account holder name
- institution_name (TEXT): Bank, card issuer, or financial institution name
- account_number (TEXT): Masked account or card number
- statement_start_date (DATE): Statement period start date
- statement_end_date (DATE): Statement period end date
- opening_balance (CURRENCY_AMOUNT): Opening balance
- closing_balance (CURRENCY_AMOUNT): Closing balance
- transactions (ARRAY): Each with transaction_date (DATE), description (TEXT), amount (CURRENCY_AMOUNT)
- currency (CURRENCY_CODE): Currency code
```

### Response


```json
{
  "success": true,
  "data": {
    "account_holder": {
      "value": "Account Holder",
      "confidence": 0.97,
      "citations": [
        "ACCOUNT HOLDER"
      ]
    },
    "institution_name": {
      "value": "Institution Name",
      "confidence": 0.97,
      "citations": [
        "INSTITUTION NAME"
      ]
    },
    "account_number": {
      "value": "Account Number",
      "confidence": 0.97,
      "citations": [
        "ACCOUNT NUMBER"
      ]
    },
    "statement_start_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "statement_end_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "opening_balance": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "EUR 1,234.56"
      ]
    },
    "closing_balance": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "EUR 1,234.56"
      ]
    },
    "transactions": {
      "value": [
        {
          "transaction_date": {
            "value": "2026-04-15",
            "confidence": 0.96,
            "citations": [
              "15 Apr 2026"
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
              "EUR 1,234.56"
            ]
          }
        }
      ],
      "confidence": 0.97,
      "citations": [
        "TRANSACTIONS"
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
