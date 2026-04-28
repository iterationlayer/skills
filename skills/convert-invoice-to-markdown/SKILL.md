---
name: convert-invoice-to-markdown
description: Convert a PDF invoice to clean markdown for LLM processing or document pipelines.
---

# Convert Invoice to Markdown

Finance teams and automation platforms use this recipe to convert PDF invoices to clean markdown before feeding them into LLM workflows, archiving pipelines, or custom extraction scripts.

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
      "name": "invoice.pdf",
      "url": "https://example.com/invoices/2026-001.pdf"
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";

const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "invoice.pdf",
    url: "https://example.com/invoices/2026-001.pdf",
  },
});
```

```python
from iterationlayer import IterationLayer

client = IterationLayer(api_key="YOUR_API_KEY")

result = client.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "invoice.pdf",
        "url": "https://example.com/invoices/2026-001.pdf",
    }
)
```

```go
import il "github.com/iterationlayer/sdk-go"

client := il.NewClient("YOUR_API_KEY")

result, err := client.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
  File: il.NewFileFromURL(
    "invoice.pdf",
    "https://example.com/invoices/2026-001.pdf",
  ),
})
```

```n8n
{
  "name": "Convert Invoice to Markdown",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Invoice to Markdown

Finance teams and automation platforms use this recipe to convert PDF invoices to clean markdown before feeding them into LLM workflows, archiving pipelines, or custom extraction scripts.

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
      "id": "c4b38344-4a6e-4ebb-b65e-a6118abb7899",
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
      "id": "2c5678bc-d6c6-47a3-bc3d-0ba493ba94e0",
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
      "id": "85f3117c-db42-4e82-8399-219775030e5b",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "invoice.pdf",
        "fileUrl": "https://example.com/invoices/2026-001.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "7c9a6489-8cb0-42fe-9712-b3deaa0e2755",
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
Convert the invoice PDF at [file URL] to markdown. Use the convert_document_to_markdown tool with the file URL.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "invoice.pdf",
    "mime_type": "application/pdf",
    "markdown": "# Invoice\n\n**Invoice Number:** INV-2026-001\n**Date:** 2026-03-15\n**Due Date:** 2026-04-15\n\n**From:**\nAcme Consulting GmbH\nMusterstraße 12\n10115 Berlin, Germany\n\n**To:**\nContoso Ltd.\n123 Main Street\nLondon, UK\n\n| Description | Qty | Unit Price | Total |\n|---|---|---|---|\n| Software Development | 40h | €150.00 | €6,000.00 |\n| Code Review | 8h | €150.00 | €1,200.00 |\n| Project Management | 4h | €120.00 | €480.00 |\n\n**Subtotal:** €7,680.00\n**VAT (19%):** €1,459.20\n**Total:** €9,139.20\n\n**Payment:** IBAN DE89 3704 0044 0532 0130 00"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
