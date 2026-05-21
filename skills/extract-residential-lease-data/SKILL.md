---
name: extract-residential-lease-data
description: Extract landlord, tenant, property, lease dates, rent, deposit, utilities, occupants, and pet terms from residential leases.
---

# Extract Residential Lease Data

Property managers use this recipe to extract residential lease terms into structured records for tenant onboarding, rent collection, and compliance workflows.

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
      "name": "residential-lease.pdf",
      "url": "https://example.com/documents/residential-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or property manager name"
      },
      {
        "name": "tenant_names",
        "type": "TEXT",
        "description": "Tenant names"
      },
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Rental property address"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease start date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease end date"
      },
      {
        "name": "monthly_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Monthly rent"
      },
      {
        "name": "rent_due_day",
        "type": "DATE",
        "description": "Rent due date or day of month"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "utilities_responsibility",
        "type": "TEXT",
        "description": "Utility responsibility terms"
      },
      {
        "name": "occupants",
        "type": "TEXT",
        "description": "Authorized occupants"
      },
      {
        "name": "pet_terms",
        "type": "TEXT",
        "description": "Pet policy or pet deposit terms"
      },
      {
        "name": "late_fee_terms",
        "type": "TEXT",
        "description": "Late fee terms"
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
      "name": "residential-lease.pdf",
      "url": "https://example.com/documents/residential-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or property manager name"
      },
      {
        "name": "tenant_names",
        "type": "TEXT",
        "description": "Tenant names"
      },
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Rental property address"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease start date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease end date"
      },
      {
        "name": "monthly_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Monthly rent"
      },
      {
        "name": "rent_due_day",
        "type": "DATE",
        "description": "Rent due date or day of month"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "utilities_responsibility",
        "type": "TEXT",
        "description": "Utility responsibility terms"
      },
      {
        "name": "occupants",
        "type": "TEXT",
        "description": "Authorized occupants"
      },
      {
        "name": "pet_terms",
        "type": "TEXT",
        "description": "Pet policy or pet deposit terms"
      },
      {
        "name": "late_fee_terms",
        "type": "TEXT",
        "description": "Late fee terms"
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
      "name": "residential-lease.pdf",
      "url": "https://example.com/documents/residential-lease-sample.pdf"
    }
  ],
  "schema": {
    "fields": [
      {
        "name": "landlord_name",
        "type": "TEXT",
        "description": "Landlord or property manager name"
      },
      {
        "name": "tenant_names",
        "type": "TEXT",
        "description": "Tenant names"
      },
      {
        "name": "property_address",
        "type": "ADDRESS",
        "description": "Rental property address"
      },
      {
        "name": "lease_start_date",
        "type": "DATE",
        "description": "Lease start date"
      },
      {
        "name": "lease_end_date",
        "type": "DATE",
        "description": "Lease end date"
      },
      {
        "name": "monthly_rent",
        "type": "CURRENCY_AMOUNT",
        "description": "Monthly rent"
      },
      {
        "name": "rent_due_day",
        "type": "DATE",
        "description": "Rent due date or day of month"
      },
      {
        "name": "security_deposit",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit"
      },
      {
        "name": "utilities_responsibility",
        "type": "TEXT",
        "description": "Utility responsibility terms"
      },
      {
        "name": "occupants",
        "type": "TEXT",
        "description": "Authorized occupants"
      },
      {
        "name": "pet_terms",
        "type": "TEXT",
        "description": "Pet policy or pet deposit terms"
      },
      {
        "name": "late_fee_terms",
        "type": "TEXT",
        "description": "Late fee terms"
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
				Name: "residential-lease.pdf",
				Url: "https://example.com/documents/residential-lease-sample.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "landlord_name",
					Type: "TEXT",
					Description: "Landlord or property manager name",
				},
				il.TextFieldConfig{
					Name: "tenant_names",
					Type: "TEXT",
					Description: "Tenant names",
				},
				il.AddressFieldConfig{
					Name: "property_address",
					Type: "ADDRESS",
					Description: "Rental property address",
				},
				il.DateFieldConfig{
					Name: "lease_start_date",
					Type: "DATE",
					Description: "Lease start date",
				},
				il.DateFieldConfig{
					Name: "lease_end_date",
					Type: "DATE",
					Description: "Lease end date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "monthly_rent",
					Type: "CURRENCY_AMOUNT",
					Description: "Monthly rent",
				},
				il.DateFieldConfig{
					Name: "rent_due_day",
					Type: "DATE",
					Description: "Rent due date or day of month",
				},
				il.CurrencyAmountFieldConfig{
					Name: "security_deposit",
					Type: "CURRENCY_AMOUNT",
					Description: "Security deposit",
				},
				il.TextFieldConfig{
					Name: "utilities_responsibility",
					Type: "TEXT",
					Description: "Utility responsibility terms",
				},
				il.TextFieldConfig{
					Name: "occupants",
					Type: "TEXT",
					Description: "Authorized occupants",
				},
				il.TextFieldConfig{
					Name: "pet_terms",
					Type: "TEXT",
					Description: "Pet policy or pet deposit terms",
				},
				il.TextFieldConfig{
					Name: "late_fee_terms",
					Type: "TEXT",
					Description: "Late fee terms",
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
  "name": "Extract Residential Lease Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Residential Lease Data\n\nProperty managers use this recipe to extract residential lease terms into structured records for tenant onboarding, rent collection, and compliance workflows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-residential-lease-data-overview",
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
      "id": "extract-residential-lease-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"landlord_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Landlord or property manager name\"\n    },\n    {\n      \"name\": \"tenant_names\",\n      \"type\": \"TEXT\",\n      \"description\": \"Tenant names\"\n    },\n    {\n      \"name\": \"property_address\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Rental property address\"\n    },\n    {\n      \"name\": \"lease_start_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Lease start date\"\n    },\n    {\n      \"name\": \"lease_end_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Lease end date\"\n    },\n    {\n      \"name\": \"monthly_rent\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Monthly rent\"\n    },\n    {\n      \"name\": \"rent_due_day\",\n      \"type\": \"DATE\",\n      \"description\": \"Rent due date or day of month\"\n    },\n    {\n      \"name\": \"security_deposit\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Security deposit\"\n    },\n    {\n      \"name\": \"utilities_responsibility\",\n      \"type\": \"TEXT\",\n      \"description\": \"Utility responsibility terms\"\n    },\n    {\n      \"name\": \"occupants\",\n      \"type\": \"TEXT\",\n      \"description\": \"Authorized occupants\"\n    },\n    {\n      \"name\": \"pet_terms\",\n      \"type\": \"TEXT\",\n      \"description\": \"Pet policy or pet deposit terms\"\n    },\n    {\n      \"name\": \"late_fee_terms\",\n      \"type\": \"TEXT\",\n      \"description\": \"Late fee terms\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "residential-lease.pdf",
              "fileUrl": "https://example.com/documents/residential-lease-sample.pdf"
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
      "id": "extract-residential-lease-data-extract",
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
Extract residential lease data from the file at [file URL]. Use the extract_document tool with these fields:

- landlord_name (TEXT): Landlord or property manager name
- tenant_names (TEXT): Tenant names
- property_address (ADDRESS): Rental property address
- lease_start_date (DATE): Lease start date
- lease_end_date (DATE): Lease end date
- monthly_rent (CURRENCY_AMOUNT): Monthly rent
- rent_due_day (DATE): Rent due date or day of month
- security_deposit (CURRENCY_AMOUNT): Security deposit
- utilities_responsibility (TEXT): Utility responsibility terms
- occupants (TEXT): Authorized occupants
- pet_terms (TEXT): Pet policy or pet deposit terms
- late_fee_terms (TEXT): Late fee terms
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
    "tenant_names": {
      "value": "Alex Morgan and Jamie Morgan",
      "confidence": 0.97,
      "citations": [
        "TENANT NAMES"
      ]
    },
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
    "monthly_rent": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "rent_due_day": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
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
    "utilities_responsibility": {
      "value": "Utilities Responsibility",
      "confidence": 0.97,
      "citations": [
        "UTILITIES RESPONSIBILITY"
      ]
    },
    "occupants": {
      "value": "Occupants",
      "confidence": 0.97,
      "citations": [
        "OCCUPANTS"
      ]
    },
    "pet_terms": {
      "value": "Pet Terms",
      "confidence": 0.97,
      "citations": [
        "PET TERMS"
      ]
    },
    "late_fee_terms": {
      "value": "Late Fee Terms",
      "confidence": 0.97,
      "citations": [
        "LATE FEE TERMS"
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
