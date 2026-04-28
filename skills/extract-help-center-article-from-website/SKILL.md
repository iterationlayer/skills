---
name: extract-help-center-article-from-website
description: Extract title, summary, steps, related links, and last updated date from a public help center article.
---

# Extract Help Center Article from Website

Support teams use this recipe to ingest public help center pages into structured support and knowledge-base workflows. Extract article titles, summaries, steps, related links, and update dates.

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
      "url": "https://example.com/help/reset-password"
    },
    "schema": {
        "fields": [
            {
                "name": "article_title",
                "type": "TEXT",
                "description": "Help article title"
            },
            {
                "name": "summary",
                "type": "TEXTAREA",
                "description": "Short support article summary"
            },
            {
                "name": "steps",
                "type": "ARRAY",
                "description": "Step-by-step instructions",
                "fields": [
                    {
                        "name": "step",
                        "type": "TEXT",
                        "description": "One instruction step"
                    }
                ]
            },
            {
                "name": "related_links",
                "type": "ARRAY",
                "description": "Related article links",
                "fields": [
                    {
                        "name": "link",
                        "type": "TEXT",
                        "description": "Related link label or URL"
                    }
                ]
            },
            {
                "name": "last_updated",
                "type": "DATE",
                "description": "Last updated date"
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
    url: "https://example.com/help/reset-password",
  },
  schema: {
    "fields": [
      {
        "name": "article_title",
        "type": "TEXT",
        "description": "Help article title"
      },
      {
        "name": "summary",
        "type": "TEXTAREA",
        "description": "Short support article summary"
      },
      {
        "name": "steps",
        "type": "ARRAY",
        "description": "Step-by-step instructions",
        "fields": [
          {
            "name": "step",
            "type": "TEXT",
            "description": "One instruction step"
          }
        ]
      },
      {
        "name": "related_links",
        "type": "ARRAY",
        "description": "Related article links",
        "fields": [
          {
            "name": "link",
            "type": "TEXT",
            "description": "Related link label or URL"
          }
        ]
      },
      {
        "name": "last_updated",
        "type": "DATE",
        "description": "Last updated date"
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
        "url": "https://example.com/help/reset-password",
    },
    schema={
      "fields": [
        {
          "name": "article_title",
          "type": "TEXT",
          "description": "Help article title"
        },
        {
          "name": "summary",
          "type": "TEXTAREA",
          "description": "Short support article summary"
        },
        {
          "name": "steps",
          "type": "ARRAY",
          "description": "Step-by-step instructions",
          "fields": [
            {
              "name": "step",
              "type": "TEXT",
              "description": "One instruction step"
            }
          ]
        },
        {
          "name": "related_links",
          "type": "ARRAY",
          "description": "Related article links",
          "fields": [
            {
              "name": "link",
              "type": "TEXT",
              "description": "Related link label or URL"
            }
          ]
        },
        {
          "name": "last_updated",
          "type": "DATE",
          "description": "Last updated date"
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
		File: il.NewWebsiteFromURL("https://example.com/help/reset-password"),
		Schema: il.ExtractionSchema{
			"article_title": il.NewTextFieldConfig("article_title", "Help article title"),
			"summary": il.NewTextareaFieldConfig("summary", "Short support article summary"),
			"steps": il.NewArrayFieldConfig(
	"steps",
	"Step-by-step instructions",
	[]il.FieldConfig{
	il.NewTextFieldConfig("step", "One instruction step"),
	},
),
			"related_links": il.NewArrayFieldConfig(
	"related_links",
	"Related article links",
	[]il.FieldConfig{
	il.NewTextFieldConfig("link", "Related link label or URL"),
	},
),
			"last_updated": il.NewDateFieldConfig("last_updated", "Last updated date"),
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
  "name": "Extract Help Center Article from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Help Center Article from Website\n\nSupport teams use this recipe to ingest public help center pages into structured support and knowledge-base workflows. Extract article titles, summaries, steps, related links, and update dates.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "70b20598-1fe1-5def-b9ce-a7002978b11c",
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
      "id": "90cae498-2bc1-586c-b612-b2e20ae43702",
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
      "id": "c92ba5a0-b1c1-5e10-b1bb-768ed57773fc",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/help/reset-password",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"article_title\",\"type\":\"TEXT\",\"description\":\"Help article title\"},{\"name\":\"summary\",\"type\":\"TEXTAREA\",\"description\":\"Short support article summary\"},{\"name\":\"steps\",\"type\":\"ARRAY\",\"description\":\"Step-by-step instructions\",\"fields\":[{\"name\":\"step\",\"type\":\"TEXT\",\"description\":\"One instruction step\"}]},{\"name\":\"related_links\",\"type\":\"ARRAY\",\"description\":\"Related article links\",\"fields\":[{\"name\":\"link\",\"type\":\"TEXT\",\"description\":\"Related link label or URL\"}]},{\"name\":\"last_updated\",\"type\":\"DATE\",\"description\":\"Last updated date\"}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "1f680089-530b-5f93-b00e-ad61432abb7e",
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

- article_title (TEXT): Help article title
- summary (TEXTAREA): Short support article summary
- steps (ARRAY): Step-by-step instructions. Each item has step (TEXT)
- related_links (ARRAY): Related article links. Each item has link (TEXT)
- last_updated (DATE): Last updated date
```

### Response


```json
{
  "success": true,
  "data": {
    "article_title": {
      "type": "TEXT",
      "value": "Reset your password",
      "confidence": 0.97,
      "citations": [
        "Reset your password"
      ],
      "source": "website.html"
    },
    "steps": {
      "type": "ARRAY",
      "value": [
        "Open Account Settings",
        "Choose Security",
        "Select Reset Password"
      ],
      "confidence": 0.9,
      "citations": [
        "Open Account Settings > Security"
      ],
      "source": "website.html"
    },
    "last_updated": {
      "type": "DATE",
      "value": "2026-03-30",
      "confidence": 0.87,
      "citations": [
        "Updated March 30, 2026"
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
