---
name: generate-white-label-report
description: Generate a branded PDF report with custom client logo placeholder, colors, and content sections for white-label agency delivery.
---

# Generate a White-Label PDF Report

Agencies delivering reports under their clients' branding use this recipe to generate white-label PDF reports — with custom headers, color schemes, and content sections that match the client's visual identity.

## APIs Used

Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Monthly Performance Report — March 2026",
        "author": "Prepared by Northbridge Analytics"
      },
      "page": {
        "size": {
          "preset": "A4"
        },
        "margins": {
          "top_in_pt": 54,
          "right_in_pt": 54,
          "bottom_in_pt": 54,
          "left_in_pt": 54
        }
      },
      "content": [
        {
          "type": "headline",
          "level": "h1",
          "text": "Cedarwood Financial Group"
        },
        {
          "type": "headline",
          "level": "h2",
          "text": "Monthly Performance Report — March 2026"
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Portfolio Summary"
        },
        {
          "type": "table",
          "header": {
            "cells": [
              {
                "text": "Metric"
              },
              {
                "text": "Value"
              }
            ]
          },
          "rows": [
            {
              "cells": [
                {
                  "text": "Total AUM"
                },
                {
                  "text": "$14,250,000"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Monthly Return"
                },
                {
                  "text": "+1.8%"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "YTD Return"
                },
                {
                  "text": "+5.4%"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Benchmark (S&P 500 YTD)"
                },
                {
                  "text": "+4.9%"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Alpha"
                },
                {
                  "text": "+0.5%"
                }
              ]
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Asset Allocation"
        },
        {
          "type": "table",
          "header": {
            "cells": [
              {
                "text": "Asset Class"
              },
              {
                "text": "Allocation"
              },
              {
                "text": "Value"
              }
            ]
          },
          "rows": [
            {
              "cells": [
                {
                  "text": "US Equities"
                },
                {
                  "text": "45%"
                },
                {
                  "text": "$6,412,500"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "International Equities"
                },
                {
                  "text": "20%"
                },
                {
                  "text": "$2,850,000"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Fixed Income"
                },
                {
                  "text": "25%"
                },
                {
                  "text": "$3,562,500"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Alternatives"
                },
                {
                  "text": "10%"
                },
                {
                  "text": "$1,425,000"
                }
              ]
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Commentary"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "The portfolio outperformed its benchmark by 50 basis points in March, driven by strong performance in the technology and healthcare sectors. Fixed income allocations provided stability amid rising rate expectations. We recommend maintaining the current allocation through Q2."
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Confidential — Prepared for Cedarwood Financial Group by Northbridge Analytics. Not for redistribution."
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

const report = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: { title: "Monthly Performance Report — March 2026", author: "Prepared by Northbridge Analytics" },
    page: { size: { preset: "A4" }, margins: { top_in_pt: 54, right_in_pt: 54, bottom_in_pt: 54, left_in_pt: 54 } },
    content: [
      { type: "headline", level: "h1", text: "Cedarwood Financial Group" },
      { type: "headline", level: "h2", text: "Monthly Performance Report — March 2026" },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Portfolio Summary" },
      {
        type: "table",
        header: { cells: [{ text: "Metric" }, { text: "Value" }] },
        rows: [
          { cells: [{ text: "Total AUM" }, { text: "$14,250,000" }] },
          { cells: [{ text: "Monthly Return" }, { text: "+1.8%" }] },
          { cells: [{ text: "YTD Return" }, { text: "+5.4%" }] },
          { cells: [{ text: "Benchmark (S&P 500 YTD)" }, { text: "+4.9%" }] },
          { cells: [{ text: "Alpha" }, { text: "+0.5%" }] },
        ],
      },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Asset Allocation" },
      {
        type: "table",
        header: { cells: [{ text: "Asset Class" }, { text: "Allocation" }, { text: "Value" }] },
        rows: [
          { cells: [{ text: "US Equities" }, { text: "45%" }, { text: "$6,412,500" }] },
          { cells: [{ text: "International Equities" }, { text: "20%" }, { text: "$2,850,000" }] },
          { cells: [{ text: "Fixed Income" }, { text: "25%" }, { text: "$3,562,500" }] },
          { cells: [{ text: "Alternatives" }, { text: "10%" }, { text: "$1,425,000" }] },
        ],
      },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Commentary" },
      { type: "paragraph", runs: [{ text: "The portfolio outperformed its benchmark by 50 basis points in March, driven by strong performance in the technology and healthcare sectors. Fixed income allocations provided stability amid rising rate expectations. We recommend maintaining the current allocation through Q2." }] },
      { type: "separator" },
      { type: "paragraph", runs: [{ text: "Confidential — Prepared for Cedarwood Financial Group by Northbridge Analytics." }] },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

report = client.generate_document(
    format="pdf",
    document={
        "metadata": {"title": "Monthly Performance Report — March 2026", "author": "Prepared by Northbridge Analytics"},
        "page": {"size": {"preset": "A4"}, "margins": {"top_in_pt": 54, "right_in_pt": 54, "bottom_in_pt": 54, "left_in_pt": 54}},
        "content": [
            {"type": "headline", "level": "h1", "text": "Cedarwood Financial Group"},
            {"type": "headline", "level": "h2", "text": "Monthly Performance Report — March 2026"},
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Portfolio Summary"},
            {
                "type": "table",
                "header": {"cells": [{"text": "Metric"}, {"text": "Value"}]},
                "rows": [
                    {"cells": [{"text": "Total AUM"}, {"text": "$14,250,000"}]},
                    {"cells": [{"text": "Monthly Return"}, {"text": "+1.8%"}]},
                    {"cells": [{"text": "YTD Return"}, {"text": "+5.4%"}]},
                    {"cells": [{"text": "Benchmark (S&P 500 YTD)"}, {"text": "+4.9%"}]},
                    {"cells": [{"text": "Alpha"}, {"text": "+0.5%"}]},
                ],
            },
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Asset Allocation"},
            {
                "type": "table",
                "header": {"cells": [{"text": "Asset Class"}, {"text": "Allocation"}, {"text": "Value"}]},
                "rows": [
                    {"cells": [{"text": "US Equities"}, {"text": "45%"}, {"text": "$6,412,500"}]},
                    {"cells": [{"text": "International Equities"}, {"text": "20%"}, {"text": "$2,850,000"}]},
                    {"cells": [{"text": "Fixed Income"}, {"text": "25%"}, {"text": "$3,562,500"}]},
                    {"cells": [{"text": "Alternatives"}, {"text": "10%"}, {"text": "$1,425,000"}]},
                ],
            },
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Commentary"},
            {"type": "paragraph", "runs": [{"text": "The portfolio outperformed its benchmark by 50 basis points in March. We recommend maintaining the current allocation through Q2."}]},
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": "Confidential — Prepared for Cedarwood Financial Group by Northbridge Analytics."}]},
        ],
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	_, err := client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{Title: "Monthly Performance Report — March 2026", Author: "Northbridge Analytics"},
			Page: &il.DocumentPage{Size: il.DocPageSize{Preset: "A4"}, Margins: il.DocMargins{TopInPt: 54, RightInPt: 54, BottomInPt: 54, LeftInPt: 54}},
			Content: []il.ContentBlock{
				il.ContentBlock{"type": "headline", "level": "h1", "text": "Cedarwood Financial Group"},
				il.ContentBlock{"type": "headline", "level": "h2", "text": "Monthly Performance Report — March 2026"},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Portfolio Summary"},
				il.ContentBlock{"type": "table", "header": map[string]any{"cells": []map[string]string{{"text": "Metric"}, {"text": "Value"}}}, "rows": []map[string]any{{"cells": []map[string]string{{"text": "Total AUM"}, {"text": "$14,250,000"}}}, {"cells": []map[string]string{{"text": "Monthly Return"}, {"text": "+1.8%"}}}, {"cells": []map[string]string{{"text": "YTD Return"}, {"text": "+5.4%"}}}}},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Confidential — Prepared by Northbridge Analytics."}}},
			},
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Generate white-label PDF report in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate a White-Label PDF Report\n\nAgencies delivering reports under their clients' branding use this recipe to generate white-label PDF reports — with custom headers, color schemes, and content sections.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "d6e7f8a9-b0c1-2345-defa-345678901201",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Report\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "d6e7f8a9-b0c1-2345-defa-345678901202",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "d6e7f8a9-b0c1-2345-defa-345678901203",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": { \"title\": \"Monthly Performance Report — March 2026\", \"author\": \"Northbridge Analytics\" },\n  \"page\": { \"size\": { \"preset\": \"A4\" }, \"margins\": { \"top_in_pt\": 54, \"right_in_pt\": 54, \"bottom_in_pt\": 54, \"left_in_pt\": 54 } },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Cedarwood Financial Group\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"Monthly Performance Report — March 2026\" },\n    { \"type\": \"separator\" },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Portfolio Summary\" },\n    { \"type\": \"table\", \"header\": { \"cells\": [{ \"text\": \"Metric\" }, { \"text\": \"Value\" }] }, \"rows\": [{ \"cells\": [{ \"text\": \"Total AUM\" }, { \"text\": \"$14,250,000\" }] }, { \"cells\": [{ \"text\": \"Monthly Return\" }, { \"text\": \"+1.8%\" }] }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Confidential — Prepared by Northbridge Analytics.\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "d6e7f8a9-b0c1-2345-defa-345678901204",
      "name": "Generate Report",
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
            "node": "Generate Report",
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
Generate a white-label PDF report for [client name]. Use the generate_document tool to create an A4 PDF with:

- Client company name as the main heading
- Report title and period as a subheading
- A portfolio summary table with metrics (AUM, monthly return, YTD return, benchmark, alpha)
- An asset allocation table with asset classes, percentages, and values
- A commentary section with market analysis
- A confidentiality footer with the preparing agency name
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "JVBERi0xLjQ...",
    "mime_type": "application/pdf"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
