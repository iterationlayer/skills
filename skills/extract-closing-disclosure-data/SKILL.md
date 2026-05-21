---
name: extract-closing-disclosure-data
description: Extract structured fields from closing disclosure documents.
---

# Extract Closing Disclosure Data

Real estate and lending teams use this recipe to extract Closing Disclosure details into structured records for loan files, title review, lease administration, or portfolio systems.

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
      "name": "closing-disclosure.pdf",
      "url": "https://example.com/documents/closing-disclosure-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Property address"
      },
      {
        "name": "document_number",
        "type": "TEXT",
        "description": "Loan, lease, title, closing, or report number"
      },
      {
        "name": "primary_party",
        "type": "TEXT",
        "description": "Borrower, tenant, owner, buyer, or insured party"
      },
      {
        "name": "counterparty",
        "type": "TEXT",
        "description": "Lender, landlord, seller, title company, or inspector"
      },
      {
        "name": "document_date",
        "type": "DATE",
        "description": "Document date"
      },
      {
        "name": "primary_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Rent, mortgage balance, closing amount, premium, or stated value"
      },
      {
        "name": "line_items",
        "type": "ARRAY",
        "description": "Charges, terms, findings, exceptions, or property details",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Line item, term, finding, or exception description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Associated amount"
          }
        ]
      },
      {
        "name": "status_or_summary",
        "type": "TEXT",
        "description": "Status, conclusion, renewal terms, or summary"
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
      "name": "closing-disclosure.pdf",
      "url": "https://example.com/documents/closing-disclosure-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Property address"
      },
      {
        "name": "document_number",
        "type": "TEXT",
        "description": "Loan, lease, title, closing, or report number"
      },
      {
        "name": "primary_party",
        "type": "TEXT",
        "description": "Borrower, tenant, owner, buyer, or insured party"
      },
      {
        "name": "counterparty",
        "type": "TEXT",
        "description": "Lender, landlord, seller, title company, or inspector"
      },
      {
        "name": "document_date",
        "type": "DATE",
        "description": "Document date"
      },
      {
        "name": "primary_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Rent, mortgage balance, closing amount, premium, or stated value"
      },
      {
        "name": "line_items",
        "type": "ARRAY",
        "description": "Charges, terms, findings, exceptions, or property details",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Line item, term, finding, or exception description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Associated amount"
          }
        ]
      },
      {
        "name": "status_or_summary",
        "type": "TEXT",
        "description": "Status, conclusion, renewal terms, or summary"
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
      "name": "closing-disclosure.pdf",
      "url": "https://example.com/documents/closing-disclosure-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Property address"
      },
      {
        "name": "document_number",
        "type": "TEXT",
        "description": "Loan, lease, title, closing, or report number"
      },
      {
        "name": "primary_party",
        "type": "TEXT",
        "description": "Borrower, tenant, owner, buyer, or insured party"
      },
      {
        "name": "counterparty",
        "type": "TEXT",
        "description": "Lender, landlord, seller, title company, or inspector"
      },
      {
        "name": "document_date",
        "type": "DATE",
        "description": "Document date"
      },
      {
        "name": "primary_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "Rent, mortgage balance, closing amount, premium, or stated value"
      },
      {
        "name": "line_items",
        "type": "ARRAY",
        "description": "Charges, terms, findings, exceptions, or property details",
        "fields": [
          {
            "name": "description",
            "type": "TEXT",
            "description": "Line item, term, finding, or exception description"
          },
          {
            "name": "amount",
            "type": "CURRENCY_AMOUNT",
            "description": "Associated amount"
          }
        ]
      },
      {
        "name": "status_or_summary",
        "type": "TEXT",
        "description": "Status, conclusion, renewal terms, or summary"
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
				Name: "closing-disclosure.pdf",
				Url: "https://example.com/documents/closing-disclosure-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.AddressFieldConfig{
					Name: "property_address",
					Type: "ADDRESS",
					Description: "Property address",
				},
				il.TextFieldConfig{
					Name: "document_number",
					Type: "TEXT",
					Description: "Loan, lease, title, closing, or report number",
				},
				il.TextFieldConfig{
					Name: "primary_party",
					Type: "TEXT",
					Description: "Borrower, tenant, owner, buyer, or insured party",
				},
				il.TextFieldConfig{
					Name: "counterparty",
					Type: "TEXT",
					Description: "Lender, landlord, seller, title company, or inspector",
				},
				il.DateFieldConfig{
					Name: "document_date",
					Type: "DATE",
					Description: "Document date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "primary_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "Rent, mortgage balance, closing amount, premium, or stated value",
				},
				il.ArrayFieldConfig{
					Name: "line_items",
					Type: "ARRAY",
					Description: "Charges, terms, findings, exceptions, or property details",
					Fields: []any{
						il.TextFieldConfig{
							Name: "description",
							Type: "TEXT",
							Description: "Line item, term, finding, or exception description",
						},
						il.CurrencyAmountFieldConfig{
							Name: "amount",
							Type: "CURRENCY_AMOUNT",
							Description: "Associated amount",
						},
					},
				},
				il.TextFieldConfig{
					Name: "status_or_summary",
					Type: "TEXT",
					Description: "Status, conclusion, renewal terms, or summary",
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
  "name": "Extract Closing Disclosure Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Closing Disclosure Data\n\nReal estate and lending teams use this recipe to extract Closing Disclosure details into structured records for loan files, title review, lease administration, or portfolio systems.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-closing-disclosure-data-overview",
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
      "id": "extract-closing-disclosure-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"property_address\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Property address\"\n    },\n    {\n      \"name\": \"document_number\",\n      \"type\": \"TEXT\",\n      \"description\": \"Loan, lease, title, closing, or report number\"\n    },\n    {\n      \"name\": \"primary_party\",\n      \"type\": \"TEXT\",\n      \"description\": \"Borrower, tenant, owner, buyer, or insured party\"\n    },\n    {\n      \"name\": \"counterparty\",\n      \"type\": \"TEXT\",\n      \"description\": \"Lender, landlord, seller, title company, or inspector\"\n    },\n    {\n      \"name\": \"document_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Document date\"\n    },\n    {\n      \"name\": \"primary_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Rent, mortgage balance, closing amount, premium, or stated value\"\n    },\n    {\n      \"name\": \"line_items\",\n      \"type\": \"ARRAY\",\n      \"description\": \"Charges, terms, findings, exceptions, or property details\",\n      \"fields\": [\n        {\n          \"name\": \"description\",\n          \"type\": \"TEXT\",\n          \"description\": \"Line item, term, finding, or exception description\"\n        },\n        {\n          \"name\": \"amount\",\n          \"type\": \"CURRENCY_AMOUNT\",\n          \"description\": \"Associated amount\"\n        }\n      ]\n    },\n    {\n      \"name\": \"status_or_summary\",\n      \"type\": \"TEXT\",\n      \"description\": \"Status, conclusion, renewal terms, or summary\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "closing-disclosure.pdf",
              "fileUrl": "https://example.com/documents/closing-disclosure-sample.pdf"
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
      "id": "extract-closing-disclosure-data-extract",
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
Extract closing disclosure data from the file at [file URL]. Use the extract_document tool with these fields:

- property_address (ADDRESS): Property address
- document_number (TEXT): Loan, lease, title, closing, or report number
- primary_party (TEXT): Borrower, tenant, owner, buyer, or insured party
- counterparty (TEXT): Lender, landlord, seller, title company, or inspector
- document_date (DATE): Document date
- primary_amount (CURRENCY_AMOUNT): Rent, mortgage balance, closing amount, premium, or stated value
- line_items (ARRAY): Each with description (TEXT), amount (CURRENCY_AMOUNT)
- status_or_summary (TEXT): Status, conclusion, renewal terms, or summary
```

### Response


```json
{
  "success": true,
  "data": {
    "property_address": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "PROPERTY ADDRESS"
      ]
    },
    "document_number": {
      "value": "BL-2026-00418",
      "confidence": 0.97,
      "citations": [
        "DOCUMENT NUMBER"
      ]
    },
    "primary_party": {
      "value": "Primary Party",
      "confidence": 0.97,
      "citations": [
        "PRIMARY PARTY"
      ]
    },
    "counterparty": {
      "value": "Counterparty",
      "confidence": 0.97,
      "citations": [
        "COUNTERPARTY"
      ]
    },
    "document_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "primary_amount": {
      "value": {
        "amount": "1234.56",
        "currency": "USD"
      },
      "confidence": 0.97,
      "citations": [
        "$1,234.56"
      ]
    },
    "line_items": {
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
        "LINE ITEMS"
      ]
    },
    "status_or_summary": {
      "value": "Status Or Summary",
      "confidence": 0.97,
      "citations": [
        "STATUS OR SUMMARY"
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
