---
name: convert-document-for-knowledge-base
description: Convert external documents — specs, contracts, reports — to markdown for knowledge base ingestion.
---

# Convert Document for Knowledge Base

Teams maintaining internal wikis or knowledge bases use this recipe to ingest external documents — vendor specs, partner contracts, agency reports — as clean, searchable markdown.

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
      "name": "vendor-specification.pdf",
      "url": "https://example.com/specs/integration-spec-v2.pdf"
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";

const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.convertDocumentToMarkdown({
  file: {
    type: "url",
    name: "vendor-specification.pdf",
    url: "https://example.com/specs/integration-spec-v2.pdf",
  },
});

// result.markdown — clean markdown for the knowledge base
// result.description — present for image files, useful as search text
```

```python
from iterationlayer import IterationLayer

client = IterationLayer(api_key="YOUR_API_KEY")

result = client.convert_document_to_markdown(
    file={
        "type": "url",
        "name": "vendor-specification.pdf",
        "url": "https://example.com/specs/integration-spec-v2.pdf",
    }
)

# result["markdown"] — clean markdown for the knowledge base
# result["description"] — present for image files, useful as search text
```

```go
import il "github.com/iterationlayer/sdk-go"

client := il.NewClient("YOUR_API_KEY")

result, err := client.ConvertDocumentToMarkdown(il.ConvertDocumentToMarkdownRequest{
    File: il.NewFileFromURL(
        "vendor-specification.pdf",
        "https://example.com/specs/integration-spec-v2.pdf",
    ),
})

// result.Markdown — clean markdown for the knowledge base
// result.Description — present for image files, useful as search text
```

```n8n
{
  "name": "Convert Document for Knowledge Base",
  "nodes": [
    {
      "parameters": {
        "content": "## Convert Document for Knowledge Base\n\nTeams maintaining internal wikis or knowledge bases use this recipe to ingest external documents \u2014 vendor specs, partner contracts, agency reports \u2014 as clean, searchable markdown.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "832c161e-a365-4f7a-aa17-9a2b0295c064",
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
      "id": "dd9efc52-69bc-43d4-91dc-e31e6ae6c616",
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
      "id": "c8a021cd-506e-4768-b4cf-7157115f1cc1",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentToMarkdown",
        "fileInputMode": "url",
        "fileName": "vendor-specification.pdf",
        "fileUrl": "https://example.com/specs/integration-spec-v2.pdf"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "9660e834-f20b-4074-9f83-33a774e23cce",
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
Convert the document at [file URL] to markdown for knowledge base ingestion. Use the convert_document_to_markdown tool with the file URL.
```

### Response


```json
{
  "success": true,
  "data": {
    "name": "vendor-specification.pdf",
    "mime_type": "application/pdf",
    "markdown": "# Integration Specification v2\n\n## Overview\n\nThis document describes the integration protocol between the Acme Payments gateway and partner systems. All communication uses TLS 1.3 over HTTPS.\n\n## Authentication\n\nPartner systems authenticate using mutual TLS (mTLS). Each partner is issued a client certificate signed by the Acme CA.\n\n| Field | Value |\n|---|---|\n| Certificate Authority | Acme Payments Root CA |\n| Key Algorithm | ECDSA P-256 |\n| Certificate Lifetime | 365 days |\n| Renewal Window | 30 days before expiry |\n\n## Webhook Events\n\nThe gateway sends webhook events for the following state transitions:\n\n- **payment.created** — a new payment has been initiated\n- **payment.authorized** — the payment has been authorized by the issuer\n- **payment.captured** — funds have been captured\n- **payment.failed** — the payment attempt failed\n- **payment.refunded** — a refund has been processed\n\n## Rate Limits\n\n| Endpoint | Limit |\n|---|---|\n| POST /payments | 100 req/s |\n| GET /payments/:id | 500 req/s |\n| POST /refunds | 50 req/s |"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
