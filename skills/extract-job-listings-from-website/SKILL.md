---
name: extract-job-listings-from-website
description: Extract role title, location, salary range, requirements, and application details from a public job posting.
---

# Extract Job Listings from Website

Recruiting teams use this recipe to convert public job posts into structured records for matching and reporting. Extract role details, requirements, salary ranges, and application links from career pages.

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
      "url": "https://example.com/careers/senior-platform-engineer"
    },
    "schema": {
        "fields": [
            {
                "name": "role_title",
                "type": "TEXT",
                "description": "Job title"
            },
            {
                "name": "location",
                "type": "TEXT",
                "description": "Job location or remote policy"
            },
            {
                "name": "salary_range",
                "type": "TEXT",
                "description": "Compensation or salary range"
            },
            {
                "name": "requirements",
                "type": "ARRAY",
                "description": "Required qualifications",
                "fields": [
                    {
                        "name": "requirement",
                        "type": "TEXT",
                        "description": "One requirement"
                    }
                ]
            },
            {
                "name": "application_url",
                "type": "TEXT",
                "description": "URL or instructions for applying"
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
    url: "https://example.com/careers/senior-platform-engineer",
  },
  schema: {
    "fields": [
      {
        "name": "role_title",
        "type": "TEXT",
        "description": "Job title"
      },
      {
        "name": "location",
        "type": "TEXT",
        "description": "Job location or remote policy"
      },
      {
        "name": "salary_range",
        "type": "TEXT",
        "description": "Compensation or salary range"
      },
      {
        "name": "requirements",
        "type": "ARRAY",
        "description": "Required qualifications",
        "fields": [
          {
            "name": "requirement",
            "type": "TEXT",
            "description": "One requirement"
          }
        ]
      },
      {
        "name": "application_url",
        "type": "TEXT",
        "description": "URL or instructions for applying"
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
        "url": "https://example.com/careers/senior-platform-engineer",
    },
    schema={
      "fields": [
        {
          "name": "role_title",
          "type": "TEXT",
          "description": "Job title"
        },
        {
          "name": "location",
          "type": "TEXT",
          "description": "Job location or remote policy"
        },
        {
          "name": "salary_range",
          "type": "TEXT",
          "description": "Compensation or salary range"
        },
        {
          "name": "requirements",
          "type": "ARRAY",
          "description": "Required qualifications",
          "fields": [
            {
              "name": "requirement",
              "type": "TEXT",
              "description": "One requirement"
            }
          ]
        },
        {
          "name": "application_url",
          "type": "TEXT",
          "description": "URL or instructions for applying"
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
		File: il.NewWebsiteFromURL("https://example.com/careers/senior-platform-engineer"),
		Schema: il.ExtractionSchema{
			"role_title": il.NewTextFieldConfig("role_title", "Job title"),
			"location": il.NewTextFieldConfig("location", "Job location or remote policy"),
			"salary_range": il.NewTextFieldConfig("salary_range", "Compensation or salary range"),
			"requirements": il.NewArrayFieldConfig(
	"requirements",
	"Required qualifications",
	[]il.FieldConfig{
	il.NewTextFieldConfig("requirement", "One requirement"),
	},
),
			"application_url": il.NewTextFieldConfig("application_url", "URL or instructions for applying"),
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
  "name": "Extract Job Listings from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Job Listings from Website\n\nRecruiting teams use this recipe to convert public job posts into structured records for matching and reporting. Extract role details, requirements, salary ranges, and application links from career pages.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "bfce2742-61e9-5ce0-9b37-cf99740685b3",
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
      "id": "f854d791-1d89-5d0b-8836-106b65106258",
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
      "id": "47b5ee48-e49d-5c8f-ad13-aac100a6329c",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/careers/senior-platform-engineer",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"role_title\",\"type\":\"TEXT\",\"description\":\"Job title\"},{\"name\":\"location\",\"type\":\"TEXT\",\"description\":\"Job location or remote policy\"},{\"name\":\"salary_range\",\"type\":\"TEXT\",\"description\":\"Compensation or salary range\"},{\"name\":\"requirements\",\"type\":\"ARRAY\",\"description\":\"Required qualifications\",\"fields\":[{\"name\":\"requirement\",\"type\":\"TEXT\",\"description\":\"One requirement\"}]},{\"name\":\"application_url\",\"type\":\"TEXT\",\"description\":\"URL or instructions for applying\"}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "966c2c61-d9c3-53a9-94dd-15426091d99f",
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

- role_title (TEXT): Job title
- location (TEXT): Job location or remote policy
- salary_range (TEXT): Compensation or salary range
- requirements (ARRAY): Required qualifications. Each item has requirement (TEXT)
- application_url (TEXT): URL or instructions for applying
```

### Response


```json
{
  "success": true,
  "data": {
    "role_title": {
      "type": "TEXT",
      "value": "Senior Platform Engineer",
      "confidence": 0.98,
      "citations": [
        "Senior Platform Engineer"
      ],
      "source": "website.html"
    },
    "location": {
      "type": "TEXT",
      "value": "Remote, EU time zones",
      "confidence": 0.91,
      "citations": [
        "Remote - EU time zones"
      ],
      "source": "website.html"
    },
    "salary_range": {
      "type": "TEXT",
      "value": "EUR 85,000 - EUR 110,000",
      "confidence": 0.89,
      "citations": [
        "\u20ac85k-\u20ac110k"
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
