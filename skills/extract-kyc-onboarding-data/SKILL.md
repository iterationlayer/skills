---
name: extract-kyc-onboarding-data
description: Extract company information, submitted identity-document references, and beneficial ownership data from onboarding packets.
---

# Extract KYC Onboarding Data

Financial service teams use this recipe to organize onboarding packets by extracting company registrations, beneficial ownership details, and submitted identity-document references for internal review queues.

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
        "name": "kyc-packet-meridian-holdings.pdf",
        "url": "https://example.com/kyc/meridian-holdings-onboarding.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "company_name",
          "type": "TEXT",
          "description": "Legal company name"
        },
        {
          "name": "registration_number",
          "type": "TEXT",
          "description": "Company registration or incorporation number"
        },
        {
          "name": "country_of_incorporation",
          "type": "COUNTRY",
          "description": "Country where the company is incorporated"
        },
        {
          "name": "incorporation_date",
          "type": "DATE",
          "description": "Date of incorporation"
        },
        {
          "name": "registered_address",
          "type": "ADDRESS",
          "description": "Registered business address"
        },
        {
          "name": "tax_id",
          "type": "TEXT",
          "description": "Tax identification number"
        },
        {
          "name": "business_type",
          "type": "TEXT",
          "description": "Type of business or industry"
        },
        {
          "name": "contact_person",
          "type": "TEXT",
          "description": "Primary contact person name"
        },
        {
          "name": "contact_email",
          "type": "EMAIL",
          "description": "Contact email address"
        },
        {
          "name": "beneficial_owners",
          "type": "ARRAY",
          "description": "Beneficial owners with 25% or more ownership",
          "fields": [
            {
              "name": "owner_name",
              "type": "TEXT",
              "description": "Full name of the beneficial owner"
            },
            {
              "name": "nationality",
              "type": "COUNTRY",
              "description": "Nationality"
            },
            {
              "name": "ownership_percentage",
              "type": "DECIMAL",
              "description": "Ownership percentage"
            },
            {
              "name": "date_of_birth",
              "type": "DATE",
              "description": "Date of birth"
            }
          ]
        },
        {
          "name": "id_document_type",
          "type": "TEXT",
          "description": "Type of identity document referenced in the onboarding packet"
        },
        {
          "name": "id_document_number",
          "type": "TEXT",
          "description": "Identity document number"
        }
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "kyc-packet-meridian-holdings.pdf",
      url: "https://example.com/kyc/meridian-holdings-onboarding.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "company_name", type: "TEXT", description: "Legal company name" },
      { name: "registration_number", type: "TEXT", description: "Company registration or incorporation number" },
      { name: "country_of_incorporation", type: "COUNTRY", description: "Country where the company is incorporated" },
      { name: "incorporation_date", type: "DATE", description: "Date of incorporation" },
      { name: "registered_address", type: "ADDRESS", description: "Registered business address" },
      { name: "tax_id", type: "TEXT", description: "Tax identification number" },
      { name: "business_type", type: "TEXT", description: "Type of business or industry" },
      { name: "contact_person", type: "TEXT", description: "Primary contact person name" },
      { name: "contact_email", type: "EMAIL", description: "Contact email address" },
      {
        name: "beneficial_owners",
        type: "ARRAY",
        description: "Beneficial owners with 25% or more ownership",
        fields: [
          { name: "owner_name", type: "TEXT", description: "Full name of the beneficial owner" },
          { name: "nationality", type: "COUNTRY", description: "Nationality" },
          { name: "ownership_percentage", type: "DECIMAL", description: "Ownership percentage" },
          { name: "date_of_birth", type: "DATE", description: "Date of birth" },
        ],
      },
      { name: "id_document_type", type: "TEXT", description: "Type of identity document referenced in the onboarding packet" },
      { name: "id_document_number", type: "TEXT", description: "Identity document number" },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "kyc-packet-meridian-holdings.pdf",
            "url": "https://example.com/kyc/meridian-holdings-onboarding.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "company_name", "type": "TEXT", "description": "Legal company name"},
            {"name": "registration_number", "type": "TEXT", "description": "Company registration or incorporation number"},
            {"name": "country_of_incorporation", "type": "COUNTRY", "description": "Country where the company is incorporated"},
            {"name": "incorporation_date", "type": "DATE", "description": "Date of incorporation"},
            {"name": "registered_address", "type": "ADDRESS", "description": "Registered business address"},
            {"name": "tax_id", "type": "TEXT", "description": "Tax identification number"},
            {"name": "business_type", "type": "TEXT", "description": "Type of business or industry"},
            {"name": "contact_person", "type": "TEXT", "description": "Primary contact person name"},
            {"name": "contact_email", "type": "EMAIL", "description": "Contact email address"},
            {
                "name": "beneficial_owners",
                "type": "ARRAY",
                "description": "Beneficial owners with 25% or more ownership",
                "fields": [
                    {"name": "owner_name", "type": "TEXT", "description": "Full name of the beneficial owner"},
                    {"name": "nationality", "type": "COUNTRY", "description": "Nationality"},
                    {"name": "ownership_percentage", "type": "DECIMAL", "description": "Ownership percentage"},
                    {"name": "date_of_birth", "type": "DATE", "description": "Date of birth"},
                ],
            },
            {"name": "id_document_type", "type": "TEXT", "description": "Type of identity document provided"},
            {"name": "id_document_number", "type": "TEXT", "description": "Identity document number"},
        ]
    },
)
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
				Name: "kyc-packet-meridian-holdings.pdf",
				Url: "https://example.com/kyc/meridian-holdings-onboarding.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "company_name",
					Type: "TEXT",
					Description: "Legal company name",
				},
				il.TextFieldConfig{
					Name: "registration_number",
					Type: "TEXT",
					Description: "Company registration or incorporation number",
				},
				il.CountryFieldConfig{
					Name: "country_of_incorporation",
					Type: "COUNTRY",
					Description: "Country where the company is incorporated",
				},
				il.DateFieldConfig{
					Name: "incorporation_date",
					Type: "DATE",
					Description: "Date of incorporation",
				},
				il.AddressFieldConfig{
					Name: "registered_address",
					Type: "ADDRESS",
					Description: "Registered business address",
				},
				il.TextFieldConfig{
					Name: "tax_id",
					Type: "TEXT",
					Description: "Tax identification number",
				},
				il.TextFieldConfig{
					Name: "business_type",
					Type: "TEXT",
					Description: "Type of business or industry",
				},
				il.TextFieldConfig{
					Name: "contact_person",
					Type: "TEXT",
					Description: "Primary contact person name",
				},
				il.EmailFieldConfig{
					Name: "contact_email",
					Type: "EMAIL",
					Description: "Contact email address",
				},
				il.ArrayFieldConfig{
					Name: "beneficial_owners",
					Type: "ARRAY",
					Description: "Beneficial owners with 25% or more ownership",
					Fields: []any{
						il.TextFieldConfig{
							Name: "owner_name",
							Type: "TEXT",
							Description: "Full name of the beneficial owner",
						},
						il.CountryFieldConfig{
							Name: "nationality",
							Type: "COUNTRY",
							Description: "Nationality",
						},
						il.DecimalFieldConfig{
							Name: "ownership_percentage",
							Type: "DECIMAL",
							Description: "Ownership percentage",
						},
						il.DateFieldConfig{
							Name: "date_of_birth",
							Type: "DATE",
							Description: "Date of birth",
						},
					},
				},
				il.TextFieldConfig{
					Name: "id_document_type",
					Type: "TEXT",
					Description: "Type of identity document referenced in the onboarding packet",
				},
				il.TextFieldConfig{
					Name: "id_document_number",
					Type: "TEXT",
					Description: "Identity document number",
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
  "name": "Extract KYC onboarding data in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract KYC Onboarding Data\n\nFinancial service teams use this recipe to organize onboarding packets by extracting company registrations, beneficial ownership details, and submitted identity-document references for internal review queues.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "c9d0e1f2-a3b4-5678-cdef-678901234501",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract KYC Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "c9d0e1f2-a3b4-5678-cdef-678901234502",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "c9d0e1f2-a3b4-5678-cdef-678901234503",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"company_name\",\"type\":\"TEXT\",\"description\":\"Legal company name\"},{\"name\":\"registration_number\",\"type\":\"TEXT\",\"description\":\"Company registration number\"},{\"name\":\"country_of_incorporation\",\"type\":\"COUNTRY\",\"description\":\"Country of incorporation\"},{\"name\":\"incorporation_date\",\"type\":\"DATE\",\"description\":\"Date of incorporation\"},{\"name\":\"registered_address\",\"type\":\"ADDRESS\",\"description\":\"Registered business address\"},{\"name\":\"tax_id\",\"type\":\"TEXT\",\"description\":\"Tax identification number\"},{\"name\":\"business_type\",\"type\":\"TEXT\",\"description\":\"Type of business\"},{\"name\":\"contact_person\",\"type\":\"TEXT\",\"description\":\"Primary contact person\"},{\"name\":\"contact_email\",\"type\":\"EMAIL\",\"description\":\"Contact email\"},{\"name\":\"beneficial_owners\",\"type\":\"ARRAY\",\"description\":\"Beneficial owners\",\"fields\":[{\"name\":\"owner_name\",\"type\":\"TEXT\",\"description\":\"Owner name\"},{\"name\":\"nationality\",\"type\":\"COUNTRY\",\"description\":\"Nationality\"},{\"name\":\"ownership_percentage\",\"type\":\"DECIMAL\",\"description\":\"Ownership percentage\"},{\"name\":\"date_of_birth\",\"type\":\"DATE\",\"description\":\"Date of birth\"}]},{\"name\":\"id_document_type\",\"type\":\"TEXT\",\"description\":\"ID document type\"},{\"name\":\"id_document_number\",\"type\":\"TEXT\",\"description\":\"ID document number\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "kyc-packet-meridian-holdings.pdf",
              "fileUrl": "https://example.com/kyc/meridian-holdings-onboarding.pdf"
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
      "id": "c9d0e1f2-a3b4-5678-cdef-678901234504",
      "name": "Extract KYC Data",
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
            "node": "Extract KYC Data",
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
Extract KYC onboarding data from the file at [file URL]. Use the extract_document tool with these fields:

- company_name (TEXT): Legal company name
- registration_number (TEXT): Company registration or incorporation number
- country_of_incorporation (COUNTRY): Country where the company is incorporated
- incorporation_date (DATE): Date of incorporation
- registered_address (ADDRESS): Registered business address
- tax_id (TEXT): Tax identification number
- business_type (TEXT): Type of business or industry
- contact_person (TEXT): Primary contact person name
- contact_email (EMAIL): Contact email address
- beneficial_owners (ARRAY): Each with owner_name (TEXT), nationality (COUNTRY), ownership_percentage (DECIMAL), date_of_birth (DATE)
- id_document_type (TEXT): Type of identity document provided
- id_document_number (TEXT): Identity document number
```

### Response


```json
{
  "success": true,
  "data": {
    "company_name": {
      "value": "Meridian Holdings B.V.",
      "confidence": 0.99,
      "citations": [
        "Meridian Holdings B.V."
      ]
    },
    "registration_number": {
      "value": "KVK 82947361",
      "confidence": 0.98,
      "citations": [
        "KvK-nummer: 82947361"
      ]
    },
    "country_of_incorporation": {
      "value": "NL",
      "confidence": 0.99,
      "citations": [
        "Geregistreerd in Nederland"
      ]
    },
    "incorporation_date": {
      "value": "2019-04-02",
      "confidence": 0.97,
      "citations": [
        "Opgericht: 02-04-2019"
      ]
    },
    "registered_address": {
      "value": {
        "street": "Herengracht 412",
        "city": "Amsterdam",
        "postal_code": "1017 BZ",
        "country": "NL"
      },
      "confidence": 0.96,
      "citations": [
        "Herengracht 412, 1017 BZ Amsterdam"
      ]
    },
    "tax_id": {
      "value": "NL862947361B01",
      "confidence": 0.97,
      "citations": [
        "BTW-nummer: NL862947361B01"
      ]
    },
    "business_type": {
      "value": "Investment holding company",
      "confidence": 0.94,
      "citations": [
        "Activiteit: Beleggingsmaatschappij"
      ]
    },
    "contact_person": {
      "value": "Jan de Vries",
      "confidence": 0.97,
      "citations": [
        "Contactpersoon: Jan de Vries"
      ]
    },
    "contact_email": {
      "value": "j.devries@meridian-holdings.nl",
      "confidence": 0.98,
      "citations": [
        "E-mail: j.devries@meridian-holdings.nl"
      ]
    },
    "beneficial_owners": {
      "value": [
        {
          "owner_name": {
            "value": "Jan de Vries",
            "confidence": 0.98,
            "citations": [
              "Jan de Vries"
            ]
          },
          "nationality": {
            "value": "NL",
            "confidence": 0.97,
            "citations": [
              "Nederlandse"
            ]
          },
          "ownership_percentage": {
            "value": 60.0,
            "confidence": 0.96,
            "citations": [
              "60%"
            ]
          },
          "date_of_birth": {
            "value": "1978-09-14",
            "confidence": 0.95,
            "citations": [
              "14-09-1978"
            ]
          }
        },
        {
          "owner_name": {
            "value": "Eva Lindström",
            "confidence": 0.97,
            "citations": [
              "Eva Lindström"
            ]
          },
          "nationality": {
            "value": "SE",
            "confidence": 0.96,
            "citations": [
              "Zweedse"
            ]
          },
          "ownership_percentage": {
            "value": 40.0,
            "confidence": 0.96,
            "citations": [
              "40%"
            ]
          },
          "date_of_birth": {
            "value": "1982-03-22",
            "confidence": 0.95,
            "citations": [
              "22-03-1982"
            ]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    },
    "id_document_type": {
      "value": "Passport",
      "confidence": 0.98,
      "citations": [
        "Identiteitsbewijs: Paspoort"
      ]
    },
    "id_document_number": {
      "value": "NW8K42P17",
      "confidence": 0.97,
      "citations": [
        "Documentnummer: NW8K42P17"
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
