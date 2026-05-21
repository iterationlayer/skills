---
name: extract-commercial-lease-data
description: Extract parties, premises, lease term, rent, escalation, deposit, options, and expense obligations from commercial leases.
---

# Extract Commercial Lease Data

Real estate teams use this recipe to extract commercial lease business terms into structured records for lease administration, diligence, and portfolio systems.

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
      "name": "commercial-lease.pdf",
      "url": "https://example.com/documents/commercial-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or lessor name"
      },
      {
        "name": "tenant_name",
        "type": "TEXT",
        "description": "Tenant or lessee name"
      },
      {
        "name": "premises_address",
        "type": "ADDRESS",
        "description": "Leased premises address"
      },
      {
        "name": "premises_description",
        "type": "TEXT",
        "description": "Suite, unit, rentable area, or premises description"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease commencement date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease expiration date"
      },
      {
        "name": "base_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Base rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency such as monthly or annually"
      },
      {
        "name": "rent_escalation",
        "type": "TEXT",
        "description": "Rent escalation or review terms"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "permitted_use",
        "type": "TEXT",
        "description": "Permitted use"
      },
      {
        "name": "renewal_options",
        "type": "TEXT",
        "description": "Renewal option terms"
      },
      {
        "name": "expense_obligations",
        "type": "TEXT",
        "description": "CAM, tax, insurance, or service charge obligations"
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
      "name": "commercial-lease.pdf",
      "url": "https://example.com/documents/commercial-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or lessor name"
      },
      {
        "name": "tenant_name",
        "type": "TEXT",
        "description": "Tenant or lessee name"
      },
      {
        "name": "premises_address",
        "type": "ADDRESS",
        "description": "Leased premises address"
      },
      {
        "name": "premises_description",
        "type": "TEXT",
        "description": "Suite, unit, rentable area, or premises description"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease commencement date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease expiration date"
      },
      {
        "name": "base_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Base rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency such as monthly or annually"
      },
      {
        "name": "rent_escalation",
        "type": "TEXT",
        "description": "Rent escalation or review terms"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "permitted_use",
        "type": "TEXT",
        "description": "Permitted use"
      },
      {
        "name": "renewal_options",
        "type": "TEXT",
        "description": "Renewal option terms"
      },
      {
        "name": "expense_obligations",
        "type": "TEXT",
        "description": "CAM, tax, insurance, or service charge obligations"
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
      "name": "commercial-lease.pdf",
      "url": "https://example.com/documents/commercial-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or lessor name"
      },
      {
        "name": "tenant_name",
        "type": "TEXT",
        "description": "Tenant or lessee name"
      },
      {
        "name": "premises_address",
        "type": "ADDRESS",
        "description": "Leased premises address"
      },
      {
        "name": "premises_description",
        "type": "TEXT",
        "description": "Suite, unit, rentable area, or premises description"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease commencement date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease expiration date"
      },
      {
        "name": "base_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Base rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency such as monthly or annually"
      },
      {
        "name": "rent_escalation",
        "type": "TEXT",
        "description": "Rent escalation or review terms"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "permitted_use",
        "type": "TEXT",
        "description": "Permitted use"
      },
      {
        "name": "renewal_options",
        "type": "TEXT",
        "description": "Renewal option terms"
      },
      {
        "name": "expense_obligations",
        "type": "TEXT",
        "description": "CAM, tax, insurance, or service charge obligations"
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
				Name: "commercial-lease.pdf",
				Url: "https://example.com/documents/commercial-lease-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "landlord_name",
					Type: "TEXT",
					Description: "Landlord or lessor name",
				},
				il.TextFieldConfig{
					Name: "tenant_name",
					Type: "TEXT",
					Description: "Tenant or lessee name",
				},
				il.AddressFieldConfig{
					Name: "premises_address",
					Type: "ADDRESS",
					Description: "Leased premises address",
				},
				il.TextFieldConfig{
					Name: "premises_description",
					Type: "TEXT",
					Description: "Suite, unit, rentable area, or premises description",
				},
				il.DateFieldConfig{
					Name: "lease_start_date",
					Type: "DATE",
					Description: "Lease commencement date",
				},
				il.DateFieldConfig{
					Name: "lease_end_date",
					Type: "DATE",
					Description: "Lease expiration date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "base_rent",
					Type: "CURRENCY_AMOUNT",
					Description: "Base rent amount",
				},
				il.TextFieldConfig{
					Name: "rent_frequency",
					Type: "TEXT",
					Description: "Rent frequency such as monthly or annually",
				},
				il.TextFieldConfig{
					Name: "rent_escalation",
					Type: "TEXT",
					Description: "Rent escalation or review terms",
				},
				il.CurrencyAmountFieldConfig{
					Name: "security_deposit",
					Type: "CURRENCY_AMOUNT",
					Description: "Security deposit",
				},
				il.TextFieldConfig{
					Name: "permitted_use",
					Type: "TEXT",
					Description: "Permitted use",
				},
				il.TextFieldConfig{
					Name: "renewal_options",
					Type: "TEXT",
					Description: "Renewal option terms",
				},
				il.TextFieldConfig{
					Name: "expense_obligations",
					Type: "TEXT",
					Description: "CAM, tax, insurance, or service charge obligations",
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
  "name": "Extract Commercial Lease Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Commercial Lease Data\n\nReal estate teams use this recipe to extract commercial lease business terms into structured records for lease administration, diligence, and portfolio systems.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-commercial-lease-data-overview",
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
      "id": "extract-commercial-lease-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"landlord_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Landlord or lessor name\"\n    },\n    {\n      \"name\": \"tenant_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Tenant or lessee name\"\n    },\n    {\n      \"name\": \"premises_address\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Leased premises address\"\n    },\n    {\n      \"name\": \"premises_description\",\n      \"type\": \"TEXT\",\n      \"description\": \"Suite, unit, rentable area, or premises description\"\n    },\n    {\n      \"name\": \"lease_start_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Lease commencement date\"\n    },\n    {\n      \"name\": \"lease_end_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Lease expiration date\"\n    },\n    {\n      \"name\": \"base_rent\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Base rent amount\"\n    },\n    {\n      \"name\": \"rent_frequency\",\n      \"type\": \"TEXT\",\n      \"description\": \"Rent frequency such as monthly or annually\"\n    },\n    {\n      \"name\": \"rent_escalation\",\n      \"type\": \"TEXT\",\n      \"description\": \"Rent escalation or review terms\"\n    },\n    {\n      \"name\": \"security_deposit\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Security deposit\"\n    },\n    {\n      \"name\": \"permitted_use\",\n      \"type\": \"TEXT\",\n      \"description\": \"Permitted use\"\n    },\n    {\n      \"name\": \"renewal_options\",\n      \"type\": \"TEXT\",\n      \"description\": \"Renewal option terms\"\n    },\n    {\n      \"name\": \"expense_obligations\",\n      \"type\": \"TEXT\",\n      \"description\": \"CAM, tax, insurance, or service charge obligations\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "commercial-lease.pdf",
              "fileUrl": "https://example.com/documents/commercial-lease-sample.pdf"
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
      "id": "extract-commercial-lease-data-extract",
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
Extract commercial lease data from the file at [file URL]. Use the extract_document tool with these fields:

- landlord_name (TEXT): Landlord or lessor name
- tenant_name (TEXT): Tenant or lessee name
- premises_address (ADDRESS): Leased premises address
- premises_description (TEXT): Suite, unit, rentable area, or premises description
- lease_start_date (DATE): Lease commencement date
- lease_end_date (DATE): Lease expiration date
- base_rent (CURRENCY_AMOUNT): Base rent amount
- rent_frequency (TEXT): Rent frequency such as monthly or annually
- rent_escalation (TEXT): Rent escalation or review terms
- security_deposit (CURRENCY_AMOUNT): Security deposit
- permitted_use (TEXT): Permitted use
- renewal_options (TEXT): Renewal option terms
- expense_obligations (TEXT): CAM, tax, insurance, or service charge obligations
```

### Response


```json
{
  "success": true,
  "data": {
    "landlord_name": {
      "value": "Harbor Property Management LLC",
      "confidence": 0.97,
      "citations": [
        "LANDLORD NAME"
      ]
    },
    "tenant_name": {
      "value": "Tenant Name",
      "confidence": 0.97,
      "citations": [
        "TENANT NAME"
      ]
    },
    "premises_address": {
      "value": {
        "street": "100 Market Street",
        "city": "Berlin",
        "postal_code": "10115",
        "country": "DE"
      },
      "confidence": 0.97,
      "citations": [
        "PREMISES ADDRESS"
      ]
    },
    "premises_description": {
      "value": "Premises Description",
      "confidence": 0.97,
      "citations": [
        "PREMISES DESCRIPTION"
      ]
    },
    "lease_start_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "lease_end_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "base_rent": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "rent_frequency": {
      "value": "Rent Frequency",
      "confidence": 0.97,
      "citations": [
        "RENT FREQUENCY"
      ]
    },
    "rent_escalation": {
      "value": "Rent Escalation",
      "confidence": 0.97,
      "citations": [
        "RENT ESCALATION"
      ]
    },
    "security_deposit": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "permitted_use": {
      "value": "Permitted Use",
      "confidence": 0.97,
      "citations": [
        "PERMITTED USE"
      ]
    },
    "renewal_options": {
      "value": "Renewal Options",
      "confidence": 0.97,
      "citations": [
        "RENEWAL OPTIONS"
      ]
    },
    "expense_obligations": {
      "value": "Expense Obligations",
      "confidence": 0.97,
      "citations": [
        "EXPENSE OBLIGATIONS"
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
