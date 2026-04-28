---
name: convert-resume-to-markdown
description: Convert a resume PDF to clean markdown for LLM parsing or candidate pipelines.
---

# Convert Resume to Markdown

Recruiting platforms and HR automation tools use this recipe to convert candidate resumes to structured markdown before feeding them into LLM screening, comparison, or extraction workflows.

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
      "name": "resume.pdf",
      "url": "https://example.com/candidates/jane-doe-resume.pdf"
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";

const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "resume.pdf",
    url: "https://example.com/candidates/jane-doe-resume.pdf",
  },
});
```

```python
from iterationlayer import IterationLayer

client = IterationLayer(api_key="YOUR_API_KEY")

result = client.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "resume.pdf",
        "url": "https://example.com/candidates/jane-doe-resume.pdf",
    }
)
```

```go
import il "github.com/iterationlayer/sdk-go"

client := il.NewClient("YOUR_API_KEY")

result, err := client.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
  File: il.NewFileFromURL(
    "resume.pdf",
    "https://example.com/candidates/jane-doe-resume.pdf",
  ),
})
```

```n8n
{
  "name": "Convert Resume to Markdown",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Resume to Markdown

Recruiting platforms and HR automation tools use this recipe to convert candidate resumes to structured markdown before feeding them into LLM screening, comparison, or extraction workflows.

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
      "id": "f3eb0ee8-29a6-4e97-9561-e25585c40171",
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
      "id": "281fedea-1917-4786-8f97-e6cb02f31045",
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
      "id": "4abc0d95-5348-4afe-9f0a-7d40078184d2",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "resume.pdf",
        "fileUrl": "https://example.com/candidates/jane-doe-resume.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "bfd7632c-6703-4c1c-87e2-7dfb7a35475b",
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
Convert the resume PDF at [file URL] to markdown. Use the convert_document_to_markdown tool with the file URL.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "resume.pdf",
    "mime_type": "application/pdf",
    "markdown": "# Jane Doe\n\njane.doe@example.com | +49 30 12345678 | Berlin, Germany\n\n## Experience\n\n**Senior Software Engineer** — Acme GmbH (2021–present)\n- Led backend development for a B2B SaaS platform serving 500+ enterprise clients\n- Reduced API response time by 40% through query optimization and caching\n\n**Software Engineer** — Startup AG (2018–2021)\n- Built and maintained REST APIs in Elixir and Go\n- Introduced automated testing, raising code coverage from 30% to 85%\n\n## Education\n\n**M.Sc. Computer Science** — TU Berlin (2018)\n\n## Skills\n\nElixir, Go, TypeScript, PostgreSQL, Redis, Docker, Kubernetes"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
