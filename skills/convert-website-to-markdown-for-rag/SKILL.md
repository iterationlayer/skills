---
name: convert-website-to-markdown-for-rag
description: Extract clean title, summary, markdown sections, and source metadata from a public documentation page for RAG ingestion.
---

# Convert Website to Markdown for RAG

AI teams use this recipe to prepare public documentation pages for RAG ingestion. Extract a clean title, summary, markdown sections, canonical URL, and update metadata before indexing.

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
      "url": "https://example.com/docs/api-reference"
    },
    "schema": {
        "fields": [
            {
                "name": "page_title",
                "type": "TEXT",
                "description": "Documentation page title"
            },
            {
                "name": "canonical_url",
                "type": "TEXT",
                "description": "Canonical URL for the page"
            },
            {
                "name": "summary",
                "type": "TEXTAREA",
                "description": "Short page summary"
            },
            {
                "name": "sections",
                "type": "ARRAY",
                "description": "Clean markdown sections from the page",
                "fields": [
                    {
                        "name": "heading",
                        "type": "TEXT",
                        "description": "Section heading"
                    },
                    {
                        "name": "markdown",
                        "type": "TEXTAREA",
                        "description": "Section content as markdown"
                    }
                ]
            },
            {
                "name": "last_updated",
                "type": "DATE",
                "description": "Last updated date if shown"
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
    url: "https://example.com/docs/api-reference",
  },
  schema: {
    "fields": [
      {
        "name": "page_title",
        "type": "TEXT",
        "description": "Documentation page title"
      },
      {
        "name": "canonical_url",
        "type": "TEXT",
        "description": "Canonical URL for the page"
      },
      {
        "name": "summary",
        "type": "TEXTAREA",
        "description": "Short page summary"
      },
      {
        "name": "sections",
        "type": "ARRAY",
        "description": "Clean markdown sections from the page",
        "fields": [
          {
            "name": "heading",
            "type": "TEXT",
            "description": "Section heading"
          },
          {
            "name": "markdown",
            "type": "TEXTAREA",
            "description": "Section content as markdown"
          }
        ]
      },
      {
        "name": "last_updated",
        "type": "DATE",
        "description": "Last updated date if shown"
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
        "url": "https://example.com/docs/api-reference",
    },
    schema={
      "fields": [
        {
          "name": "page_title",
          "type": "TEXT",
          "description": "Documentation page title"
        },
        {
          "name": "canonical_url",
          "type": "TEXT",
          "description": "Canonical URL for the page"
        },
        {
          "name": "summary",
          "type": "TEXTAREA",
          "description": "Short page summary"
        },
        {
          "name": "sections",
          "type": "ARRAY",
          "description": "Clean markdown sections from the page",
          "fields": [
            {
              "name": "heading",
              "type": "TEXT",
              "description": "Section heading"
            },
            {
              "name": "markdown",
              "type": "TEXTAREA",
              "description": "Section content as markdown"
            }
          ]
        },
        {
          "name": "last_updated",
          "type": "DATE",
          "description": "Last updated date if shown"
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
		File: il.NewWebsiteFromURL("https://example.com/docs/api-reference"),
		Schema: il.ExtractionSchema{
			"page_title": il.NewTextFieldConfig("page_title", "Documentation page title"),
			"canonical_url": il.NewTextFieldConfig("canonical_url", "Canonical URL for the page"),
			"summary": il.NewTextareaFieldConfig("summary", "Short page summary"),
			"sections": il.NewArrayFieldConfig(
	"sections",
	"Clean markdown sections from the page",
	[]il.FieldConfig{
	il.NewTextFieldConfig("heading", "Section heading"),
	il.NewTextareaFieldConfig("markdown", "Section content as markdown"),
	},
),
			"last_updated": il.NewDateFieldConfig("last_updated", "Last updated date if shown"),
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
  "name": "Convert Website to Markdown for RAG",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Website to Markdown for RAG\n\nAI teams use this recipe to prepare public documentation pages for RAG ingestion. Extract a clean title, summary, markdown sections, canonical URL, and update metadata before indexing.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "8489dbea-5cb7-5f09-9b79-e6a44f53269d",
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
      "id": "3cc8b04a-f31d-5e80-8e40-21caf2d0d2e7",
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
      "id": "06fbaa26-af7e-5549-a5a1-cbf38e0ad77e",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/docs/api-reference",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"page_title\",\"type\":\"TEXT\",\"description\":\"Documentation page title\"},{\"name\":\"canonical_url\",\"type\":\"TEXT\",\"description\":\"Canonical URL for the page\"},{\"name\":\"summary\",\"type\":\"TEXTAREA\",\"description\":\"Short page summary\"},{\"name\":\"sections\",\"type\":\"ARRAY\",\"description\":\"Clean markdown sections from the page\",\"fields\":[{\"name\":\"heading\",\"type\":\"TEXT\",\"description\":\"Section heading\"},{\"name\":\"markdown\",\"type\":\"TEXTAREA\",\"description\":\"Section content as markdown\"}]},{\"name\":\"last_updated\",\"type\":\"DATE\",\"description\":\"Last updated date if shown\"}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "18411b48-26e9-52ec-a857-abe04c01b0ea",
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

- page_title (TEXT): Documentation page title
- canonical_url (TEXT): Canonical URL for the page
- summary (TEXTAREA): Short page summary
- sections (ARRAY): Clean markdown sections from the page. Each item has heading (TEXT), markdown (TEXTAREA)
- last_updated (DATE): Last updated date if shown
```

### Response


```json
{
  "success": true,
  "data": {
    "page_title": {
      "type": "TEXT",
      "value": "API Reference",
      "confidence": 0.98,
      "citations": [
        "API Reference"
      ],
      "source": "website.html"
    },
    "summary": {
      "type": "TEXTAREA",
      "value": "Reference documentation for authentication, requests, responses, and errors.",
      "confidence": 0.91,
      "citations": [
        "Authentication, requests, responses, and errors"
      ],
      "source": "website.html"
    },
    "sections": {
      "type": "ARRAY",
      "value": [
        {
          "heading": "Authentication",
          "markdown": "Use a bearer token in the Authorization header."
        }
      ],
      "confidence": 0.88,
      "citations": [
        "## Authentication"
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
