---
name: extract-rental-application
description: Extract applicant details, employment history, income, and references from a rental application form into structured JSON for tenant screening.
---

# Extract Rental Application

Property managers and landlords use this recipe to digitize a tenant application. Upload a rental application PDF and receive structured JSON with applicant info, employment history, and references — ready for your tenant screening workflow.

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
        "name": "rental-application.pdf",
        "url": "https://example.com/applications/rental-application.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "applicant_name",
          "type": "TEXT",
          "description": "Full name of the applicant"
        },
        {
          "name": "current_address",
          "type": "ADDRESS",
          "description": "Applicant current address"
        },
        {
          "name": "employer",
          "type": "TEXT",
          "description": "Current employer name"
        },
        {
          "name": "monthly_income",
          "type": "CURRENCY_AMOUNT",
          "description": "Gross monthly income"
        },
        {
          "name": "move_in_date",
          "type": "DATE",
          "description": "Requested move-in date"
        },
        {
          "name": "previous_landlord",
          "type": "TEXT",
          "description": "Previous landlord name and contact"
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
      name: "rental-application.pdf",
      url: "https://example.com/applications/rental-application.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "applicant_name",
        type: "TEXT",
        description: "Full name of the applicant",
      },
      {
        name: "current_address",
        type: "ADDRESS",
        description: "Applicant current address",
      },
      {
        name: "employer",
        type: "TEXT",
        description: "Current employer name",
      },
      {
        name: "monthly_income",
        type: "CURRENCY_AMOUNT",
        description: "Gross monthly income",
      },
      {
        name: "move_in_date",
        type: "DATE",
        description: "Requested move-in date",
      },
      {
        name: "previous_landlord",
        type: "TEXT",
        description: "Previous landlord name and contact",
      },
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
            "name": "rental-application.pdf",
            "url": "https://example.com/applications/rental-application.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "applicant_name",
                "type": "TEXT",
                "description": "Full name of the applicant",
            },
            {
                "name": "current_address",
                "type": "ADDRESS",
                "description": "Applicant current address",
            },
            {
                "name": "employer",
                "type": "TEXT",
                "description": "Current employer name",
            },
            {
                "name": "monthly_income",
                "type": "CURRENCY_AMOUNT",
                "description": "Gross monthly income",
            },
            {
                "name": "move_in_date",
                "type": "DATE",
                "description": "Requested move-in date",
            },
            {
                "name": "previous_landlord",
                "type": "TEXT",
                "description": "Previous landlord name and contact",
            },
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
			il.NewFileFromURL(
				"rental-application.pdf",
				"https://example.com/applications/rental-application.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"applicant_name": il.NewTextFieldConfig(
				"applicant_name",
				"Full name of the applicant",
			),
			"current_address": il.NewAddressFieldConfig(
				"current_address",
				"Applicant current address",
			),
			"employer": il.NewTextFieldConfig(
				"employer", "Current employer name",
			),
			"monthly_income": il.NewCurrencyAmountFieldConfig(
				"monthly_income",
				"Gross monthly income",
			),
			"move_in_date": il.NewDateFieldConfig(
				"move_in_date",
				"Requested move-in date",
			),
			"previous_landlord": il.NewTextFieldConfig(
				"previous_landlord",
				"Previous landlord name and contact",
			),
		},
	})
	if err != nil {
		panic(err)
	}

}
```

```n8n
{
  "name": "Extract Rental Application",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Rental Application\n\nProperty managers and landlords use this recipe to digitize a tenant application. Upload a rental application PDF and receive structured JSON with applicant info, employment history, and references \u2014 ready for your tenant screening workflow.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "cbaaf4a9-f942-493c-a2f9-2a9242af4b68",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "f8da1feb-bd3c-4cbe-8148-38d28174831e",
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
      "id": "e3f4a5b6-c7d8-9012-efab-123456789efa",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"applicant_name\",\"type\":\"TEXT\",\"description\":\"Full name of the applicant\"},{\"name\":\"current_address\",\"type\":\"ADDRESS\",\"description\":\"Applicant current address\"},{\"name\":\"employer\",\"type\":\"TEXT\",\"description\":\"Current employer name\"},{\"name\":\"monthly_income\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Gross monthly income\"},{\"name\":\"move_in_date\",\"type\":\"DATE\",\"description\":\"Requested move-in date\"},{\"name\":\"previous_landlord\",\"type\":\"TEXT\",\"description\":\"Previous landlord name and contact\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "rental-application.pdf",
              "fileUrl": "https://example.com/applications/rental-application.pdf"
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
      "id": "f4a5b6c7-d8e9-0123-fabc-234567890fab",
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
Extract rental application data from the file at [file URL]. Use the extract_document tool with these fields:

- applicant_name (TEXT): Full name of the applicant
- current_address (ADDRESS): Applicant current address
- employer (TEXT): Current employer name
- monthly_income (CURRENCY_AMOUNT): Gross monthly income
- move_in_date (DATE): Requested move-in date
- previous_landlord (TEXT): Previous landlord name and contact
```

### Response


```json
{
  "success": true,
  "data": {
    "applicant_name": {
      "value": "David Kim",
      "confidence": 0.99,
      "citations": ["Applicant: David Kim"]
    },
    "current_address": {
      "value": {
        "street": "742 Evergreen Terrace",
        "city": "Portland",
        "region": "OR",
        "postal_code": "97201",
        "country": "US"
      },
      "confidence": 0.95,
      "citations": ["742 Evergreen Terrace, Portland, OR 97201"]
    },
    "employer": {
      "value": "Cascade Health Systems",
      "confidence": 0.97,
      "citations": ["Employer: Cascade Health Systems"]
    },
    "monthly_income": {
      "value": {
        "amount": "6,200.00",
        "currency": "USD"
      },
      "confidence": 0.96,
      "citations": ["Gross Monthly Income: $6,200"]
    },
    "move_in_date": {
      "value": "2026-05-01",
      "confidence": 0.98,
      "citations": ["Desired Move-In: May 1, 2026"]
    },
    "previous_landlord": {
      "value": "Janet Morales, (503) 555-0147",
      "confidence": 0.94,
      "citations": ["Previous Landlord: Janet Morales (503) 555-0147"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
