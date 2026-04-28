---
name: document-to-markdown-pipeline
description: Convert PDF, DOCX, HTML, or image documents to clean, structured Markdown.
---

# Convert Document to Markdown

Development teams and content platforms use this recipe to convert documents for RAG ingestion, knowledge base import, or content migration. Upload any supported document and receive clean Markdown with preserved structure.

## APIs Used

Document to Markdown (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-to-markdown/v1/convert \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "quarterly-report.pdf",
      "url": "https://example.com/reports/quarterly-report-q1-2026.pdf"
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "quarterly-report.pdf",
    url: "https://example.com/reports/quarterly-report-q1-2026.pdf",
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "quarterly-report.pdf",
        "url": "https://example.com/reports/quarterly-report-q1-2026.pdf",
    }
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
		File: il.NewFileFromURL(
			"quarterly-report.pdf",
			"https://example.com/reports/quarterly-report-q1-2026.pdf",
		),
	})
	if err != nil {
		panic(err)
	}
	_ = result
}
```

```n8n
{
  "name": "Convert Document to Markdown",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Document to Markdown

Development teams and content platforms use this recipe to convert documents for RAG ingestion, knowledge base import, or content migration. Upload any supported document and receive clean Markdown with preserved structure.

**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "d902e1d3-3586-4e18-a7d4-924ce672fb85",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Convert Document to Markdown
Resource: **Document to Markdown**

Configure the Document to Markdown parameters below, then connect your credentials.",
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
      "id": "97bf1b2f-4c80-4cc5-8f57-f14bbc06babe",
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
      "id": "2b7f6978-fcf3-4080-a209-ed3a7b1e211b",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "quarterly-report.pdf",
        "fileUrl": "https://example.com/reports/quarterly-report-q1-2026.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "f3d36e4a-b4eb-46f1-8e01-1b0552552064",
      "name": "Convert Document to Markdown",
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
            "node": "Convert Document to Markdown",
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
Convert the document at [file URL] to markdown. Use the convert_document_to_markdown tool with the file URL.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "quarterly-report.pdf",
    "mime_type": "application/pdf",
    "markdown": "## Quarterly Sales Report\n\n**Period:** January 1 – March 31, 2026\n\n### Revenue Summary\n\nRevenue grew 12% quarter-over-quarter, driven by strong enterprise adoption in EMEA and APAC regions.\n\n| Region | Q4 2025 | Q1 2026 | Growth |\n|--------|---------|---------|--------|\n| North America | $4.2M | $4.5M | 7.1% |\n| EMEA | $2.8M | $3.3M | 17.8% |\n| APAC | $1.1M | $1.4M | 27.3% |\n\n### Key Highlights\n\n- **New enterprise contracts:** 14 signed, up from 9 in Q4\n- **Net revenue retention:** 118%\n- **Average deal size:** $84,200, up 22% YoY\n\n### Outlook\n\nQ2 pipeline is $12.4M with 62% weighted probability. Product launches scheduled for April are expected to accelerate mid-market adoption."
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
