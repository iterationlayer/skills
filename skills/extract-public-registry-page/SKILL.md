---
name: extract-public-registry-page
description: Extract organization name, registration number, status, registration date, and officers from a public registry page.
---

# Extract Public Registry Page

Compliance teams use this recipe to convert public registry pages into structured facts for onboarding and audit workflows. Extract company identity, registration status, dates, and officers.

## APIs Used

Website Extraction (1 credit per website extraction)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/website-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "url": "https://example.com/registry/acme-ltd"
    },
    "schema": {
        "fields": [
            {
                "name": "organization_name",
                "type": "TEXT",
                "description": "Registered organization name"
            },
            {
                "name": "registration_number",
                "type": "TEXT",
                "description": "Registry identifier"
            },
            {
                "name": "status",
                "type": "TEXT",
                "description": "Current registry status"
            },
            {
                "name": "registration_date",
                "type": "DATE",
                "description": "Registration date"
            },
            {
                "name": "officers",
                "type": "ARRAY",
                "description": "Listed officers or directors",
                "fields": [
                    {
                        "name": "officer",
                        "type": "TEXT",
                        "description": "Officer name and role"
                    }
                ]
            }
        ]
    }
}'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractWebsite({
  file: {
    type: "url",
    url: "https://example.com/registry/acme-ltd",
  },
  schema: {
    "fields": [
      {
        "name": "organization_name",
        "type": "TEXT",
        "description": "Registered organization name"
      },
      {
        "name": "registration_number",
        "type": "TEXT",
        "description": "Registry identifier"
      },
      {
        "name": "status",
        "type": "TEXT",
        "description": "Current registry status"
      },
      {
        "name": "registration_date",
        "type": "DATE",
        "description": "Registration date"
      },
      {
        "name": "officers",
        "type": "ARRAY",
        "description": "Listed officers or directors",
        "fields": [
          {
            "name": "officer",
            "type": "TEXT",
            "description": "Officer name and role"
          }
        ]
      }
    ]
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_website(
    file={
        "type": "url",
        "url": "https://example.com/registry/acme-ltd",
    },
    schema={
      "fields": [
        {
          "name": "organization_name",
          "type": "TEXT",
          "description": "Registered organization name"
        },
        {
          "name": "registration_number",
          "type": "TEXT",
          "description": "Registry identifier"
        },
        {
          "name": "status",
          "type": "TEXT",
          "description": "Current registry status"
        },
        {
          "name": "registration_date",
          "type": "DATE",
          "description": "Registration date"
        },
        {
          "name": "officers",
          "type": "ARRAY",
          "description": "Listed officers or directors",
          "fields": [
            {
              "name": "officer",
              "type": "TEXT",
              "description": "Officer name and role"
            }
          ]
        }
      ]
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractWebsite(il.ExtractWebsiteRequest{
		File: il.NewWebsiteFromURL("https://example.com/registry/acme-ltd"),
		Schema: il.ExtractionSchema{
			"organization_name": il.NewTextFieldConfig("organization_name", "Registered organization name"),
			"registration_number": il.NewTextFieldConfig("registration_number", "Registry identifier"),
			"status": il.NewTextFieldConfig("status", "Current registry status"),
			"registration_date": il.NewDateFieldConfig("registration_date", "Registration date"),
			"officers": il.NewArrayFieldConfig(
	"officers",
	"Listed officers or directors",
	[]il.FieldConfig{
	il.NewTextFieldConfig("officer", "Officer name and role"),
	},
),
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
  "name": "Extract Public Registry Page",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Public Registry Page\n\nCompliance teams use this recipe to convert public registry pages into structured facts for onboarding and audit workflows. Extract company identity, registration status, dates, and officers.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "576db1cb-e2f7-59c7-bc9a-9f50532b2446",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Website Data\nResource: **Website Extraction**\n\nConfigure the public website URL and schema below, then connect your credentials.",
        "height": 160,
        "width": 320,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "5fb7f61e-ded3-59b1-9c95-85ad62f64ace",
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
      "id": "f8f3490f-0e62-5ff9-9ec2-dcc14c4d0494",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/registry/acme-ltd",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"organization_name\",\"type\":\"TEXT\",\"description\":\"Registered organization name\"},{\"name\":\"registration_number\",\"type\":\"TEXT\",\"description\":\"Registry identifier\"},{\"name\":\"status\",\"type\":\"TEXT\",\"description\":\"Current registry status\"},{\"name\":\"registration_date\",\"type\":\"DATE\",\"description\":\"Registration date\"},{\"name\":\"officers\",\"type\":\"ARRAY\",\"description\":\"Listed officers or directors\",\"fields\":[{\"name\":\"officer\",\"type\":\"TEXT\",\"description\":\"Officer name and role\"}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "b0373126-cd56-57b6-a0a5-5e745be85bba",
      "name": "Extract Website Data",
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
            "node": "Extract Website Data",
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
Extract structured data from the public website page at [website URL]. Use the extract_website tool with this schema:

- organization_name (TEXT): Registered organization name
- registration_number (TEXT): Registry identifier
- status (TEXT): Current registry status
- registration_date (DATE): Registration date
- officers (ARRAY): Listed officers or directors. Each item has officer (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "organization_name": {
      "type": "TEXT",
      "value": "Acme Ltd",
      "confidence": 0.97,
      "citations": [
        "Acme Ltd"
      ],
      "source": "website.html"
    },
    "registration_number": {
      "type": "TEXT",
      "value": "HRB 123456",
      "confidence": 0.95,
      "citations": [
        "Registration number HRB 123456"
      ],
      "source": "website.html"
    },
    "status": {
      "type": "TEXT",
      "value": "Active",
      "confidence": 0.93,
      "citations": [
        "Status: Active"
      ],
      "source": "website.html"
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
