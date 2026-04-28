---
name: preprocess-document-for-llm
description: Convert a document to markdown and classify it with an LLM in a single pipeline.
---

# Preprocess Document for LLM Classification

Backend teams building document automation pipelines use this recipe to normalize incoming files — PDFs, scans, Word docs — into clean markdown before classifying or routing them with an LLM.

## APIs Used

Document to Markdown (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Convert document to markdown
MARKDOWN=$(curl -s -X POST https://api.iterationlayer.com/document-to-markdown/v1/convert \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "incoming-document.pdf",
      "url": "https://example.com/uploads/incoming-document.pdf"
    }
  }' | jq -r '.data.markdown')

# Step 2: Classify with an LLM
curl -s https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"claude-sonnet-4-20250514\",
    \"max_tokens\": 256,
    \"messages\": [{
      \"role\": \"user\",
      \"content\": \"Classify this document as one of: invoice, contract, receipt, report, other. Return only the category.\n\n$MARKDOWN\"
    }]
  }"
```

```typescript
import { IterationLayer } from "iterationlayer";
import Anthropic from "@anthropic-ai/sdk";

const il = new IterationLayer({ apiKey: "YOUR_API_KEY" });
const anthropic = new Anthropic();

const { markdown } = await il.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "incoming-document.pdf",
    url: "https://example.com/uploads/incoming-document.pdf",
  },
});

const response = await anthropic.messages.create({
  model: "claude-sonnet-4-20250514",
  max_tokens: 256,
  messages: [
    {
      role: "user",
      content: `Classify this document as one of: invoice, contract, receipt, report, other. Return only the category.\n\n${markdown}`,
    },
  ],
});
```

```python
import anthropic
from iterationlayer import IterationLayer

il = IterationLayer(api_key="YOUR_API_KEY")
claude = anthropic.Anthropic()

result = il.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "incoming-document.pdf",
        "url": "https://example.com/uploads/incoming-document.pdf",
    }
)

response = claude.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=256,
    messages=[
        {
            "role": "user",
            "content": f"Classify this document as one of: invoice, contract, receipt, report, other. Return only the category.\n\n{result['markdown']}",
        }
    ],
)
```

```go
import (
    "context"
    "fmt"
    "os"

    il "github.com/iterationlayer/sdk-go"
    "github.com/anthropics/anthropic-sdk-go"
)

ilClient := il.NewClient("YOUR_API_KEY")
claude := anthropic.NewClient()

result, err := ilClient.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
    File: il.NewFileFromURL(
        "incoming-document.pdf",
        "https://example.com/uploads/incoming-document.pdf",
    ),
})

response, err := claude.Messages.New(context.Background(), anthropic.MessageNewParams{
    Model:     "claude-sonnet-4-20250514",
    MaxTokens: 256,
    Messages: []anthropic.MessageParam{
        anthropic.NewUserMessage(
            anthropic.NewTextBlock(
                fmt.Sprintf("Classify this document as one of: invoice, contract, receipt, report, other. Return only the category.\n\n%s", result.Markdown),
            ),
        ),
    },
})
```

```n8n
{
  "name": "Preprocess Document for LLM Classification",
  "nodes": [
    {
      "parameters": {
        "content": "## Preprocess Document for LLM Classification\n\nBackend teams building document automation pipelines use this recipe to normalize incoming files \u2014 PDFs, scans, Word docs \u2014 into clean markdown before classifying or routing them with an LLM.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "c038ca04-e7d0-493d-86fc-a762def63940",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Convert Document to Markdown\nResource: **Document to Markdown**\n\nConfigure the Document to Markdown parameters below, then connect your credentials.",
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
      "id": "c3c4154d-1370-4d06-a281-b9c15b3f216c",
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
      "id": "3364a568-b08e-4f5f-a382-c90933bfea9f",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "incoming-document.pdf",
        "fileUrl": "https://example.com/uploads/incoming-document.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "c9006bc5-f42e-48f0-b64b-1d82b40b6b5b",
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
Convert the document at [file URL] to markdown for LLM processing. Use the convert_document_to_markdown tool with the file URL. The resulting markdown can then be passed to an LLM for classification, summarization, or analysis.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "incoming-document.pdf",
    "mime_type": "application/pdf",
    "markdown": "# Invoice\n\n**Invoice Number:** INV-2026-047\n**Date:** 2026-03-20\n\n**From:**\nNordic Design Studio AB\nStorgatan 15\n11123 Stockholm, Sweden\n\n| Service | Hours | Rate | Amount |\n|---|---|---|---|\n| UX Research | 24 | €120.00 | €2,880.00 |\n| Interface Design | 40 | €140.00 | €5,600.00 |\n\n**Total:** €8,480.00"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
