---
name: extract-academic-paper-metadata
description: Extract title, authors, abstract, and citation info from academic papers.
---

# Extract Academic Paper Metadata

Research teams and academic platforms use this recipe to extract metadata from a paper. Upload a PDF paper and receive structured JSON with title, authors, abstract, publication date, and keywords — ready for indexing, citation analysis, or a literature review tool.

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
        "name": "paper.pdf",
        "url": "https://example.com/papers/research-paper.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "title",
          "type": "TEXT",
          "description": "Title of the academic paper"
        },
        {
          "name": "authors",
          "type": "ARRAY",
          "description": "List of paper authors",
          "fields": [
            {
              "name": "name",
              "type": "TEXT",
              "description": "Full name of the author"
            }
          ]
        },
        {
          "name": "abstract",
          "type": "TEXTAREA",
          "description": "Paper abstract"
        },
        {
          "name": "published_date",
          "type": "DATE",
          "description": "Publication date of the paper"
        },
        {
          "name": "keywords",
          "type": "ARRAY",
          "description": "Subject keywords or tags",
          "fields": [
            {
              "name": "keyword",
              "type": "TEXT",
              "description": "A keyword or topic tag"
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

const result = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "paper.pdf",
      url: "https://example.com/papers/research-paper.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "title",
        type: "TEXT",
        description: "Title of the academic paper",
      },
      {
        name: "authors",
        type: "ARRAY",
        description: "List of paper authors",
        fields: [
          {
            name: "name",
            type: "TEXT",
            description: "Full name of the author",
          },
        ],
      },
      {
        name: "abstract",
        type: "TEXTAREA",
        description: "Paper abstract",
      },
      {
        name: "published_date",
        type: "DATE",
        description: "Publication date of the paper",
      },
      {
        name: "keywords",
        type: "ARRAY",
        description: "Subject keywords or tags",
        fields: [
          {
            name: "keyword",
            type: "TEXT",
            description: "A keyword or topic tag",
          },
        ],
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
            "name": "paper.pdf",
            "url": "https://example.com/papers/research-paper.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "title",
                "type": "TEXT",
                "description": "Title of the academic paper",
            },
            {
                "name": "authors",
                "type": "ARRAY",
                "description": "List of paper authors",
                "fields": [
                    {
                        "name": "name",
                        "type": "TEXT",
                        "description": "Full name of the author",
                    },
                ],
            },
            {
                "name": "abstract",
                "type": "TEXTAREA",
                "description": "Paper abstract",
            },
            {
                "name": "published_date",
                "type": "DATE",
                "description": "Publication date of the paper",
            },
            {
                "name": "keywords",
                "type": "ARRAY",
                "description": "Subject keywords or tags",
                "fields": [
                    {
                        "name": "keyword",
                        "type": "TEXT",
                        "description": "A keyword or topic tag",
                    },
                ],
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
			il.NewFileFromURL("paper.pdf", "https://example.com/papers/research-paper.pdf"),
		},
		Schema: il.ExtractionSchema{
			"title": il.NewTextFieldConfig(
				"title",
				"Title of the academic paper",
			),
			"authors": il.NewArrayFieldConfig(
				"authors",
				"List of paper authors",
				il.ExtractionSchema{
					"name": il.NewTextFieldConfig(
						"name",
						"Full name of the author",
					),
				},
			),
			"abstract": il.NewTextFieldConfig(
				"abstract", "Paper abstract",
			),
			"published_date": il.NewDateFieldConfig(
				"published_date",
				"Publication date of the paper",
			),
			"keywords": il.NewArrayFieldConfig(
				"keywords",
				"Subject keywords or tags",
				il.ExtractionSchema{
					"keyword": il.NewTextFieldConfig(
						"keyword",
						"A keyword or topic tag",
					),
				},
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
  "name": "Extract Academic Paper Metadata",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Academic Paper Metadata\n\nResearch teams and academic platforms use this recipe to extract metadata from a paper. Upload a PDF paper and receive structured JSON with title, authors, abstract, publication date, and keywords \u2014 ready for indexing, citation analysis, or a literature review tool.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "2e5849ad-7140-4898-9adc-b818ae2d3de8",
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
      "id": "0895d89d-fcc2-409b-a989-35d4344b4b0d",
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
      "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"title\",\"type\":\"TEXT\",\"description\":\"Title of the academic paper\"},{\"name\":\"authors\",\"type\":\"ARRAY\",\"description\":\"List of paper authors\",\"fields\":[{\"name\":\"name\",\"type\":\"TEXT\",\"description\":\"Full name of the author\"}]},{\"name\":\"abstract\",\"type\":\"TEXTAREA\",\"description\":\"Paper abstract\"},{\"name\":\"published_date\",\"type\":\"DATE\",\"description\":\"Publication date of the paper\"},{\"name\":\"keywords\",\"type\":\"ARRAY\",\"description\":\"Subject keywords or tags\",\"fields\":[{\"name\":\"keyword\",\"type\":\"TEXT\",\"description\":\"A keyword or topic tag\"}]}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "paper.pdf",
              "fileUrl": "https://example.com/papers/research-paper.pdf"
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f12345678901",
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
Extract metadata from the academic paper at [file URL]. Use the extract_document tool with these fields:

- title (TEXT): Title of the academic paper
- authors (ARRAY): Each with name (TEXT)
- abstract (TEXTAREA): Paper abstract
- published_date (DATE): Publication date of the paper
- keywords (ARRAY): Each with keyword (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "title": {
      "value": "Attention Is All You Need",
      "confidence": 0.99,
      "citations": ["Attention Is All You Need"]
    },
    "authors": {
      "value": [
        {
          "name": {
            "value": "Ashish Vaswani",
            "confidence": 0.98,
            "citations": ["Ashish Vaswani"]
          }
        },
        {
          "name": {
            "value": "Noam Shazeer",
            "confidence": 0.97,
            "citations": ["Noam Shazeer"]
          }
        }
      ],
      "confidence": 0.97,
      "citations": []
    },
    "abstract": {
      "value": "The dominant sequence transduction models are based on complex recurrent or convolutional neural networks. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms.",
      "confidence": 0.95,
      "citations": [
        "The dominant sequence transduction models are based on complex recurrent or convolutional neural networks"
      ]
    },
    "published_date": {
      "value": "2017-06-12",
      "confidence": 0.94,
      "citations": ["12 Jun 2017"]
    },
    "keywords": {
      "value": [
        {
          "keyword": {
            "value": "transformer",
            "confidence": 0.96,
            "citations": ["Transformer"]
          }
        },
        {
          "keyword": {
            "value": "attention mechanism",
            "confidence": 0.95,
            "citations": ["attention mechanisms"]
          }
        }
      ],
      "confidence": 0.95,
      "citations": []
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
