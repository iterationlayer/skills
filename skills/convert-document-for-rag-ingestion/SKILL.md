---
name: convert-document-for-rag-ingestion
description: Convert a document to clean markdown suitable for chunking and embedding in a RAG pipeline.
---

# Convert Document for RAG Ingestion

AI teams building retrieval-augmented generation pipelines use this recipe to convert source documents into clean markdown that chunks well and produces high-quality embeddings.

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
      "name": "product-manual.pdf",
      "url": "https://example.com/docs/product-manual-v3.pdf"
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";

const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "product-manual.pdf",
    url: "https://example.com/docs/product-manual-v3.pdf",
  },
});

// Split markdown into chunks at heading boundaries
const chunks = result.markdown.split(/(?=^## )/m);
```

```python
import re
from iterationlayer import IterationLayer

client = IterationLayer(api_key="YOUR_API_KEY")

result = client.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "product-manual.pdf",
        "url": "https://example.com/docs/product-manual-v3.pdf",
    }
)

# Split markdown into chunks at heading boundaries
chunks = re.split(r"(?=^## )", result["markdown"], flags=re.MULTILINE)
```

```go
import (
    "strings"

    il "github.com/iterationlayer/sdk-go"
)

client := il.NewClient("YOUR_API_KEY")

result, err := client.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
    File: il.NewFileFromURL(
        "product-manual.pdf",
        "https://example.com/docs/product-manual-v3.pdf",
    ),
})

// Split markdown into chunks at heading boundaries
chunks := strings.Split(result.Markdown, "\n## ")
```

```n8n
{
  "name": "Convert Document for RAG Ingestion",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Document for RAG Ingestion

AI teams building retrieval-augmented generation pipelines use this recipe to convert source documents into clean markdown that chunks well and produces high-quality embeddings.

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
      "id": "4dce764c-c876-46e6-b345-073db354eff4",
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
      "id": "b8aee214-9501-4c49-b56a-5f00a2bb2f41",
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
      "id": "641f3a90-4201-4e73-8faa-526100f62954",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "product-manual.pdf",
        "fileUrl": "https://example.com/docs/product-manual-v3.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "2da87e7d-179a-453c-b7ab-2175662c07b6",
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
Convert the document at [file URL] to markdown for RAG ingestion. Use the convert_document_to_markdown tool with the file URL.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "product-manual.pdf",
    "mime_type": "application/pdf",
    "markdown": "# Product Manual v3\n\n## Installation\n\nDownload the latest release from the releases page. Unpack the archive and run the installer.\n\n### System Requirements\n\n| Component | Minimum | Recommended |\n|---|---|---|\n| CPU | 2 cores | 4 cores |\n| RAM | 4 GB | 8 GB |\n| Disk | 2 GB | 10 GB |\n\n## Configuration\n\nThe configuration file is located at `/etc/app/config.yaml`. The following options are available:\n\n- **port** — HTTP server port (default: 8080)\n- **log_level** — Logging verbosity: debug, info, warn, error\n- **max_connections** — Maximum concurrent connections (default: 100)\n\n## API Reference\n\n### Authentication\n\nAll API calls require a Bearer token in the Authorization header.\n\n### Endpoints\n\n| Method | Path | Description |\n|---|---|---|\n| GET | /health | Health check |\n| POST | /process | Submit a processing job |\n| GET | /jobs/:id | Get job status |"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
