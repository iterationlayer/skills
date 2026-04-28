---
name: extract-news-article-metadata-from-website
description: Extract headline, author, publication date, summary, category, and named entities from a public news article.
---

# Extract News Article Metadata from Website

Media monitoring teams use this recipe to turn public article pages into structured metadata for research and alerts. Extract headline, author, date, summary, category, and named entities.

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
      "url": "https://example.com/news/market-update"
    },
    "schema": {
        "fields": [
            {
                "name": "headline",
                "type": "TEXT",
                "description": "Article headline"
            },
            {
                "name": "author",
                "type": "TEXT",
                "description": "Article author"
            },
            {
                "name": "published_date",
                "type": "DATE",
                "description": "Publication date"
            },
            {
                "name": "summary",
                "type": "TEXTAREA",
                "description": "Short article summary"
            },
            {
                "name": "entities",
                "type": "ARRAY",
                "description": "Named entities mentioned in the article",
                "fields": [
                    {
                        "name": "entity",
                        "type": "TEXT",
                        "description": "One named entity"
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
    url: "https://example.com/news/market-update",
  },
  schema: {
    "fields": [
      {
        "name": "headline",
        "type": "TEXT",
        "description": "Article headline"
      },
      {
        "name": "author",
        "type": "TEXT",
        "description": "Article author"
      },
      {
        "name": "published_date",
        "type": "DATE",
        "description": "Publication date"
      },
      {
        "name": "summary",
        "type": "TEXTAREA",
        "description": "Short article summary"
      },
      {
        "name": "entities",
        "type": "ARRAY",
        "description": "Named entities mentioned in the article",
        "fields": [
          {
            "name": "entity",
            "type": "TEXT",
            "description": "One named entity"
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
        "url": "https://example.com/news/market-update",
    },
    schema={
      "fields": [
        {
          "name": "headline",
          "type": "TEXT",
          "description": "Article headline"
        },
        {
          "name": "author",
          "type": "TEXT",
          "description": "Article author"
        },
        {
          "name": "published_date",
          "type": "DATE",
          "description": "Publication date"
        },
        {
          "name": "summary",
          "type": "TEXTAREA",
          "description": "Short article summary"
        },
        {
          "name": "entities",
          "type": "ARRAY",
          "description": "Named entities mentioned in the article",
          "fields": [
            {
              "name": "entity",
              "type": "TEXT",
              "description": "One named entity"
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
		File: il.NewWebsiteFromURL("https://example.com/news/market-update"),
		Schema: il.ExtractionSchema{
			"headline": il.NewTextFieldConfig("headline", "Article headline"),
			"author": il.NewTextFieldConfig("author", "Article author"),
			"published_date": il.NewDateFieldConfig("published_date", "Publication date"),
			"summary": il.NewTextareaFieldConfig("summary", "Short article summary"),
			"entities": il.NewArrayFieldConfig(
	"entities",
	"Named entities mentioned in the article",
	[]il.FieldConfig{
	il.NewTextFieldConfig("entity", "One named entity"),
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
  "name": "Extract News Article Metadata from Website",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract News Article Metadata from Website\n\nMedia monitoring teams use this recipe to turn public article pages into structured metadata for research and alerts. Extract headline, author, date, summary, category, and named entities.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes before importing. Self-hosted n8n only.",
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
      "id": "5766ba18-e695-5f62-aaae-932013b93399",
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
      "id": "eda4ec81-04b9-5775-abfb-6c49de96d208",
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
      "id": "5be9ccc8-fefc-5654-af78-bff133a24904",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "websiteExtraction",
        "fileUrl": "https://example.com/news/market-update",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"headline\",\"type\":\"TEXT\",\"description\":\"Article headline\"},{\"name\":\"author\",\"type\":\"TEXT\",\"description\":\"Article author\"},{\"name\":\"published_date\",\"type\":\"DATE\",\"description\":\"Publication date\"},{\"name\":\"summary\",\"type\":\"TEXTAREA\",\"description\":\"Short article summary\"},{\"name\":\"entities\",\"type\":\"ARRAY\",\"description\":\"Named entities mentioned in the article\",\"fields\":[{\"name\":\"entity\",\"type\":\"TEXT\",\"description\":\"One named entity\"}]}]}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        520,
        300
      ],
      "id": "8e5eb38c-9e78-59a2-ac0d-8b7231907bf8",
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

- headline (TEXT): Article headline
- author (TEXT): Article author
- published_date (DATE): Publication date
- summary (TEXTAREA): Short article summary
- entities (ARRAY): Named entities mentioned in the article. Each item has entity (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "headline": {
      "type": "TEXT",
      "value": "European SaaS Spending Rises in Q1",
      "confidence": 0.98,
      "citations": [
        "European SaaS Spending Rises in Q1"
      ],
      "source": "website.html"
    },
    "author": {
      "type": "TEXT",
      "value": "Mara Klein",
      "confidence": 0.92,
      "citations": [
        "By Mara Klein"
      ],
      "source": "website.html"
    },
    "published_date": {
      "type": "DATE",
      "value": "2026-04-12",
      "confidence": 0.9,
      "citations": [
        "April 12, 2026"
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
