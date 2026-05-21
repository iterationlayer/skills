---
name: extract-lease-renewal-data
description: Extract tenant, property, renewal term, new rent, deposit changes, and signature fields from lease renewal notices or agreements.
---

# Extract Lease Renewal Data

Property managers use this recipe to extract lease renewal terms into structured records for rent updates, tenant communications, and lease administration.

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
      "name": "lease-renewal.pdf",
      "url": "https://example.com/documents/lease-renewal-sample.pdf"
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
        "description": "Property address"
      },
      {
        "name": "original_lease_reference",
        "type": "TEXT",
        "description": "Original lease date or reference"
      },
      {
        "name": "renewal_start_date",
        "type": "DATE",
        "description": "Renewal term start date"
      },
      {
        "name": "renewal_end_date",
        "type": "DATE",
        "description": "Renewal term end date"
      },
      {
        "name": "new_rent_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "New rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency"
      },
      {
        "name": "deposit_change",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit increase or change if shown"
      },
      {
        "name": "changed_terms",
        "type": "TEXT",
        "description": "Summary of changed lease terms"
      },
      {
        "name": "tenant_signature_present",
        "type": "BOOLEAN",
        "description": "Whether tenant signature is present"
      },
      {
        "name": "landlord_signature_present",
        "type": "BOOLEAN",
        "description": "Whether landlord signature is present"
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
      "name": "lease-renewal.pdf",
      "url": "https://example.com/documents/lease-renewal-sample.pdf"
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
        "description": "Property address"
      },
      {
        "name": "original_lease_reference",
        "type": "TEXT",
        "description": "Original lease date or reference"
      },
      {
        "name": "renewal_start_date",
        "type": "DATE",
        "description": "Renewal term start date"
      },
      {
        "name": "renewal_end_date",
        "type": "DATE",
        "description": "Renewal term end date"
      },
      {
        "name": "new_rent_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "New rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency"
      },
      {
        "name": "deposit_change",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit increase or change if shown"
      },
      {
        "name": "changed_terms",
        "type": "TEXT",
        "description": "Summary of changed lease terms"
      },
      {
        "name": "tenant_signature_present",
        "type": "BOOLEAN",
        "description": "Whether tenant signature is present"
      },
      {
        "name": "landlord_signature_present",
        "type": "BOOLEAN",
        "description": "Whether landlord signature is present"
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
      "name": "lease-renewal.pdf",
      "url": "https://example.com/documents/lease-renewal-sample.pdf"
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
        "description": "Property address"
      },
      {
        "name": "original_lease_reference",
        "type": "TEXT",
        "description": "Original lease date or reference"
      },
      {
        "name": "renewal_start_date",
        "type": "DATE",
        "description": "Renewal term start date"
      },
      {
        "name": "renewal_end_date",
        "type": "DATE",
        "description": "Renewal term end date"
      },
      {
        "name": "new_rent_amount",
        "type": "CURRENCY_AMOUNT",
        "description": "New rent amount"
      },
      {
        "name": "rent_frequency",
        "type": "TEXT",
        "description": "Rent frequency"
      },
      {
        "name": "deposit_change",
        "type": "CURRENCY_AMOUNT",
        "description": "Security deposit increase or change if shown"
      },
      {
        "name": "changed_terms",
        "type": "TEXT",
        "description": "Summary of changed lease terms"
      },
      {
        "name": "tenant_signature_present",
        "type": "BOOLEAN",
        "description": "Whether tenant signature is present"
      },
      {
        "name": "landlord_signature_present",
        "type": "BOOLEAN",
        "description": "Whether landlord signature is present"
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
				Name: "lease-renewal.pdf",
				Url: "https://example.com/documents/lease-renewal-sample.pdf",
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
					Description: "Property address",
				},
				il.TextFieldConfig{
					Name: "original_lease_reference",
					Type: "TEXT",
					Description: "Original lease date or reference",
				},
				il.DateFieldConfig{
					Name: "renewal_start_date",
					Type: "DATE",
					Description: "Renewal term start date",
				},
				il.DateFieldConfig{
					Name: "renewal_end_date",
					Type: "DATE",
					Description: "Renewal term end date",
				},
				il.CurrencyAmountFieldConfig{
					Name: "new_rent_amount",
					Type: "CURRENCY_AMOUNT",
					Description: "New rent amount",
				},
				il.TextFieldConfig{
					Name: "rent_frequency",
					Type: "TEXT",
					Description: "Rent frequency",
				},
				il.CurrencyAmountFieldConfig{
					Name: "deposit_change",
					Type: "CURRENCY_AMOUNT",
					Description: "Security deposit increase or change if shown",
				},
				il.TextFieldConfig{
					Name: "changed_terms",
					Type: "TEXT",
					Description: "Summary of changed lease terms",
				},
				il.BooleanFieldConfig{
					Name: "tenant_signature_present",
					Type: "BOOLEAN",
					Description: "Whether tenant signature is present",
				},
				il.BooleanFieldConfig{
					Name: "landlord_signature_present",
					Type: "BOOLEAN",
					Description: "Whether landlord signature is present",
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
  "name": "Extract Lease Renewal Data",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Lease Renewal Data\n\nProperty managers use this recipe to extract lease renewal terms into structured records for rent updates, tenant communications, and lease administration.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "extract-lease-renewal-data-overview",
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
      "id": "extract-lease-renewal-data-trigger",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\n  \"fields\": [\n    {\n      \"name\": \"landlord_name\",\n      \"type\": \"TEXT\",\n      \"description\": \"Landlord or property manager name\"\n    },\n    {\n      \"name\": \"tenant_names\",\n      \"type\": \"TEXT\",\n      \"description\": \"Tenant names\"\n    },\n    {\n      \"name\": \"property_address\",\n      \"type\": \"ADDRESS\",\n      \"description\": \"Property address\"\n    },\n    {\n      \"name\": \"original_lease_reference\",\n      \"type\": \"TEXT\",\n      \"description\": \"Original lease date or reference\"\n    },\n    {\n      \"name\": \"renewal_start_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Renewal term start date\"\n    },\n    {\n      \"name\": \"renewal_end_date\",\n      \"type\": \"DATE\",\n      \"description\": \"Renewal term end date\"\n    },\n    {\n      \"name\": \"new_rent_amount\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"New rent amount\"\n    },\n    {\n      \"name\": \"rent_frequency\",\n      \"type\": \"TEXT\",\n      \"description\": \"Rent frequency\"\n    },\n    {\n      \"name\": \"deposit_change\",\n      \"type\": \"CURRENCY_AMOUNT\",\n      \"description\": \"Security deposit increase or change if shown\"\n    },\n    {\n      \"name\": \"changed_terms\",\n      \"type\": \"TEXT\",\n      \"description\": \"Summary of changed lease terms\"\n    },\n    {\n      \"name\": \"tenant_signature_present\",\n      \"type\": \"BOOLEAN\",\n      \"description\": \"Whether tenant signature is present\"\n    },\n    {\n      \"name\": \"landlord_signature_present\",\n      \"type\": \"BOOLEAN\",\n      \"description\": \"Whether landlord signature is present\"\n    }\n  ]\n}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "lease-renewal.pdf",
              "fileUrl": "https://example.com/documents/lease-renewal-sample.pdf"
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
      "id": "extract-lease-renewal-data-extract",
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
Extract lease renewal data from the file at [file URL]. Use the extract_document tool with these fields:

- landlord_name (TEXT): Landlord or property manager name
- tenant_names (TEXT): Tenant names
- property_address (ADDRESS): Property address
- original_lease_reference (TEXT): Original lease date or reference
- renewal_start_date (DATE): Renewal term start date
- renewal_end_date (DATE): Renewal term end date
- new_rent_amount (CURRENCY_AMOUNT): New rent amount
- rent_frequency (TEXT): Rent frequency
- deposit_change (CURRENCY_AMOUNT): Security deposit increase or change if shown
- changed_terms (TEXT): Summary of changed lease terms
- tenant_signature_present (BOOLEAN): Whether tenant signature is present
- landlord_signature_present (BOOLEAN): Whether landlord signature is present
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
    "original_lease_reference": {
      "value": "Original Lease Reference",
      "confidence": 0.97,
      "citations": [
        "ORIGINAL LEASE REFERENCE"
      ]
    },
    "renewal_start_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "renewal_end_date": {
      "value": "2026-04-15",
      "confidence": 0.97,
      "citations": [
        "15 Apr 2026"
      ]
    },
    "new_rent_amount": {
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
    "deposit_change": {
      "value": {
        "amount": "1234.56",
        "currency": "EUR"
      },
      "confidence": 0.97,
      "citations": [
        "1,234.56"
      ]
    },
    "changed_terms": {
      "value": "Changed Terms",
      "confidence": 0.97,
      "citations": [
        "CHANGED TERMS"
      ]
    },
    "tenant_signature_present": {
      "value": true,
      "confidence": 0.97,
      "citations": [
        "TENANT SIGNATURE PRESENT"
      ]
    },
    "landlord_signature_present": {
      "value": true,
      "confidence": 0.97,
      "citations": [
        "LANDLORD SIGNATURE PRESENT"
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
