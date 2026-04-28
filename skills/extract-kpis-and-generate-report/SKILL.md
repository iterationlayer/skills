---
name: extract-kpis-and-generate-report
description: Extract key performance indicators from client documents, then generate a branded PDF report summarizing the results.
---

# Extract KPIs and Generate a Client Report

Marketing and consulting agencies use this pipeline to automate monthly client reporting — extracting performance metrics from analytics exports or campaign summaries and generating branded PDF reports ready for delivery.

## APIs Used

Document Extraction (1 credit per page), Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract KPIs from client document
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "campaign-summary-q1.pdf",
        "url": "https://example.com/reports/campaign-summary-q1-2026.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "client_name",
          "type": "TEXT",
          "description": "Client or company name"
        },
        {
          "name": "report_period",
          "type": "TEXT",
          "description": "Reporting period, e.g. Q1 2026"
        },
        {
          "name": "total_impressions",
          "type": "INTEGER",
          "description": "Total ad impressions"
        },
        {
          "name": "total_clicks",
          "type": "INTEGER",
          "description": "Total ad clicks"
        },
        {
          "name": "click_through_rate",
          "type": "DECIMAL",
          "description": "Click-through rate as a percentage"
        },
        {
          "name": "total_conversions",
          "type": "INTEGER",
          "description": "Total conversions or leads generated"
        },
        {
          "name": "cost_per_conversion",
          "type": "CURRENCY_AMOUNT",
          "description": "Average cost per conversion"
        },
        {
          "name": "total_spend",
          "type": "CURRENCY_AMOUNT",
          "description": "Total advertising spend"
        },
        {
          "name": "return_on_ad_spend",
          "type": "DECIMAL",
          "description": "Return on ad spend ratio"
        }
      ]
    }
  }')

# Step 2: Generate a branded PDF report
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Q1 2026 Campaign Performance — Greenfield Organics",
        "author": "Spark Digital Agency"
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
          "text": "Spark Digital Agency"
        },
        {
          "type": "headline",
          "level": "h2",
          "text": "Campaign Performance Report — Q1 2026"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Prepared for: ",
              "font_weight": "bold"
            },
            {
              "text": "Greenfield Organics"
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Key Performance Indicators"
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
                  "text": "Total Impressions"
                },
                {
                  "text": "1,245,800"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Total Clicks"
                },
                {
                  "text": "37,420"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Click-Through Rate"
                },
                {
                  "text": "3.00%"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Total Conversions"
                },
                {
                  "text": "1,870"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Cost per Conversion"
                },
                {
                  "text": "$12.45"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Total Spend"
                },
                {
                  "text": "$23,281.50"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Return on Ad Spend"
                },
                {
                  "text": "4.2x"
                }
              ]
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
              "text": "Report generated by Spark Digital Agency. For questions, contact reporting@sparkdigital.io"
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

// Step 1: Extract KPIs from client document
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "campaign-summary-q1.pdf",
      url: "https://example.com/reports/campaign-summary-q1-2026.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "client_name", type: "TEXT", description: "Client or company name" },
      { name: "report_period", type: "TEXT", description: "Reporting period, e.g. Q1 2026" },
      { name: "total_impressions", type: "INTEGER", description: "Total ad impressions" },
      { name: "total_clicks", type: "INTEGER", description: "Total ad clicks" },
      { name: "click_through_rate", type: "DECIMAL", description: "Click-through rate as a percentage" },
      { name: "total_conversions", type: "INTEGER", description: "Total conversions or leads generated" },
      { name: "cost_per_conversion", type: "CURRENCY_AMOUNT", description: "Average cost per conversion" },
      { name: "total_spend", type: "CURRENCY_AMOUNT", description: "Total advertising spend" },
      { name: "return_on_ad_spend", type: "DECIMAL", description: "Return on ad spend ratio" },
    ],
  },
});

const clientName = String(extraction["client_name"].value);
const reportPeriod = String(extraction["report_period"].value);
const totalImpressions = String(extraction["total_impressions"].value);
const totalClicks = String(extraction["total_clicks"].value);
const clickThroughRate = String(extraction["click_through_rate"].value);
const totalConversions = String(extraction["total_conversions"].value);
const costPerConversion = String(extraction["cost_per_conversion"].value);
const totalSpend = String(extraction["total_spend"].value);
const returnOnAdSpend = String(extraction["return_on_ad_spend"].value);

// Step 2: Generate a branded PDF report
const report = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: `${reportPeriod} Campaign Performance — ${clientName}`,
      author: "Spark Digital Agency",
    },
    page: {
      size: { preset: "A4" },
      margins: { top_in_pt: 54, right_in_pt: 54, bottom_in_pt: 54, left_in_pt: 54 },
    },
    content: [
      { type: "headline", level: "h1", text: "Spark Digital Agency" },
      { type: "headline", level: "h2", text: `Campaign Performance Report — ${reportPeriod}` },
      {
        type: "paragraph",
        runs: [
          { text: "Prepared for: ", font_weight: "bold" },
          { text: clientName },
        ],
      },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Key Performance Indicators" },
      {
        type: "table",
        header: { cells: [{ text: "Metric" }, { text: "Value" }] },
        rows: [
          { cells: [{ text: "Total Impressions" }, { text: totalImpressions }] },
          { cells: [{ text: "Total Clicks" }, { text: totalClicks }] },
          { cells: [{ text: "Click-Through Rate" }, { text: `${clickThroughRate}%` }] },
          { cells: [{ text: "Total Conversions" }, { text: totalConversions }] },
          { cells: [{ text: "Cost per Conversion" }, { text: costPerConversion }] },
          { cells: [{ text: "Total Spend" }, { text: totalSpend }] },
          { cells: [{ text: "Return on Ad Spend" }, { text: `${returnOnAdSpend}x` }] },
        ],
      },
      { type: "separator" },
      {
        type: "paragraph",
        runs: [{ text: "Report generated by Spark Digital Agency. For questions, contact reporting@sparkdigital.io" }],
      },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract KPIs from client document
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "campaign-summary-q1.pdf",
            "url": "https://example.com/reports/campaign-summary-q1-2026.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "client_name", "type": "TEXT", "description": "Client or company name"},
            {"name": "report_period", "type": "TEXT", "description": "Reporting period, e.g. Q1 2026"},
            {"name": "total_impressions", "type": "INTEGER", "description": "Total ad impressions"},
            {"name": "total_clicks", "type": "INTEGER", "description": "Total ad clicks"},
            {"name": "click_through_rate", "type": "DECIMAL", "description": "Click-through rate as a percentage"},
            {"name": "total_conversions", "type": "INTEGER", "description": "Total conversions or leads generated"},
            {"name": "cost_per_conversion", "type": "CURRENCY_AMOUNT", "description": "Average cost per conversion"},
            {"name": "total_spend", "type": "CURRENCY_AMOUNT", "description": "Total advertising spend"},
            {"name": "return_on_ad_spend", "type": "DECIMAL", "description": "Return on ad spend ratio"},
        ]
    },
)

client_name = extraction["client_name"]["value"]
report_period = extraction["report_period"]["value"]
total_impressions = extraction["total_impressions"]["value"]
total_clicks = extraction["total_clicks"]["value"]
click_through_rate = extraction["click_through_rate"]["value"]
total_conversions = extraction["total_conversions"]["value"]
cost_per_conversion = extraction["cost_per_conversion"]["value"]
total_spend = extraction["total_spend"]["value"]
return_on_ad_spend = extraction["return_on_ad_spend"]["value"]

# Step 2: Generate a branded PDF report
report = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": f"{report_period} Campaign Performance — {client_name}",
            "author": "Spark Digital Agency",
        },
        "page": {
            "size": {"preset": "A4"},
            "margins": {"top_in_pt": 54, "right_in_pt": 54, "bottom_in_pt": 54, "left_in_pt": 54},
        },
        "content": [
            {"type": "headline", "level": "h1", "text": "Spark Digital Agency"},
            {"type": "headline", "level": "h2", "text": f"Campaign Performance Report — {report_period}"},
            {
                "type": "paragraph",
                "runs": [
                    {"text": "Prepared for: ", "font_weight": "bold"},
                    {"text": client_name},
                ],
            },
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Key Performance Indicators"},
            {
                "type": "table",
                "header": {"cells": [{"text": "Metric"}, {"text": "Value"}]},
                "rows": [
                    {"cells": [{"text": "Total Impressions"}, {"text": str(total_impressions)}]},
                    {"cells": [{"text": "Total Clicks"}, {"text": str(total_clicks)}]},
                    {"cells": [{"text": "Click-Through Rate"}, {"text": f"{click_through_rate}%"}]},
                    {"cells": [{"text": "Total Conversions"}, {"text": str(total_conversions)}]},
                    {"cells": [{"text": "Cost per Conversion"}, {"text": str(cost_per_conversion)}]},
                    {"cells": [{"text": "Total Spend"}, {"text": str(total_spend)}]},
                    {"cells": [{"text": "Return on Ad Spend"}, {"text": f"{return_on_ad_spend}x"}]},
                ],
            },
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": "Report generated by Spark Digital Agency. For questions, contact reporting@sparkdigital.io"}]},
        ],
    },
)
```

```go
package main

import (
	"fmt"

	il "github.com/iterationlayer/sdk-go"
)

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Extract KPIs from client document
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("campaign-summary-q1.pdf", "https://example.com/reports/campaign-summary-q1-2026.pdf"),
		},
		Schema: il.ExtractionSchema{
			"client_name":         il.NewTextFieldConfig("client_name", "Client or company name"),
			"report_period":       il.NewTextFieldConfig("report_period", "Reporting period, e.g. Q1 2026"),
			"total_impressions":   il.NewIntegerFieldConfig("total_impressions", "Total ad impressions"),
			"total_clicks":        il.NewIntegerFieldConfig("total_clicks", "Total ad clicks"),
			"click_through_rate":  il.NewDecimalFieldConfig("click_through_rate", "Click-through rate as a percentage"),
			"total_conversions":   il.NewIntegerFieldConfig("total_conversions", "Total conversions or leads generated"),
			"cost_per_conversion": il.NewCurrencyAmountFieldConfig("cost_per_conversion", "Average cost per conversion"),
			"total_spend":         il.NewCurrencyAmountFieldConfig("total_spend", "Total advertising spend"),
			"return_on_ad_spend":  il.NewDecimalFieldConfig("return_on_ad_spend", "Return on ad spend ratio"),
		},
	})
	if err != nil {
		panic(err)
	}

	clientName := fmt.Sprintf("%v", (*extraction)["client_name"].Value)
	reportPeriod := fmt.Sprintf("%v", (*extraction)["report_period"].Value)

	// Step 2: Generate a branded PDF report
	_, err = client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{
				Title:  reportPeriod + " Campaign Performance — " + clientName,
				Author: "Spark Digital Agency",
			},
			Page: &il.DocumentPage{
				Size:    il.DocPageSize{Preset: "A4"},
				Margins: il.DocMargins{TopInPt: 54, RightInPt: 54, BottomInPt: 54, LeftInPt: 54},
			},
			Content: []il.ContentBlock{
				il.ContentBlock{"type": "headline", "level": "h1", "text": "Spark Digital Agency"},
				il.ContentBlock{"type": "headline", "level": "h2", "text": "Campaign Performance Report — " + reportPeriod},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Prepared for: ", "font_weight": "bold"}, {"text": clientName}}},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Key Performance Indicators"},
				il.ContentBlock{
					"type":   "table",
					"header": map[string]any{"cells": []map[string]string{{"text": "Metric"}, {"text": "Value"}}},
					"rows": []map[string]any{
						{"cells": []map[string]string{{"text": "Total Impressions"}, {"text": "1,245,800"}}},
						{"cells": []map[string]string{{"text": "Total Clicks"}, {"text": "37,420"}}},
						{"cells": []map[string]string{{"text": "Click-Through Rate"}, {"text": "3.00%"}}},
						{"cells": []map[string]string{{"text": "Total Conversions"}, {"text": "1,870"}}},
						{"cells": []map[string]string{{"text": "Cost per Conversion"}, {"text": "$12.45"}}},
						{"cells": []map[string]string{{"text": "Total Spend"}, {"text": "$23,281.50"}}},
						{"cells": []map[string]string{{"text": "Return on Ad Spend"}, {"text": "4.2x"}}},
					},
				},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Report generated by Spark Digital Agency. For questions, contact reporting@sparkdigital.io"}}},
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
  "name": "Extract KPIs and generate client report in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract KPIs and Generate a Client Report\n\nMarketing and consulting agencies use this pipeline to automate monthly client reporting — extracting performance metrics from analytics exports or campaign summaries and generating branded PDF reports ready for delivery.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678901",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract KPIs\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678902",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Report PDF\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        725,
        100
      ],
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678903",
      "name": "Step 2 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678904",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"client_name\",\"type\":\"TEXT\",\"description\":\"Client or company name\"},{\"name\":\"report_period\",\"type\":\"TEXT\",\"description\":\"Reporting period\"},{\"name\":\"total_impressions\",\"type\":\"INTEGER\",\"description\":\"Total ad impressions\"},{\"name\":\"total_clicks\",\"type\":\"INTEGER\",\"description\":\"Total ad clicks\"},{\"name\":\"click_through_rate\",\"type\":\"DECIMAL\",\"description\":\"Click-through rate as a percentage\"},{\"name\":\"total_conversions\",\"type\":\"INTEGER\",\"description\":\"Total conversions\"},{\"name\":\"cost_per_conversion\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Average cost per conversion\"},{\"name\":\"total_spend\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Total advertising spend\"},{\"name\":\"return_on_ad_spend\",\"type\":\"DECIMAL\",\"description\":\"Return on ad spend ratio\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "campaign-summary-q1.pdf",
              "fileUrl": "https://example.com/reports/campaign-summary-q1-2026.pdf"
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
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678905",
      "name": "Extract KPIs",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"{{ $json.report_period }} Campaign Performance — {{ $json.client_name }}\",\n    \"author\": \"Spark Digital Agency\"\n  },\n  \"page\": {\n    \"size\": { \"preset\": \"A4\" },\n    \"margins\": { \"top_in_pt\": 54, \"right_in_pt\": 54, \"bottom_in_pt\": 54, \"left_in_pt\": 54 }\n  },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Spark Digital Agency\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"Campaign Performance Report — {{ $json.report_period }}\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Prepared for: \", \"font_weight\": \"bold\" }, { \"text\": \"{{ $json.client_name }}\" }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Key Performance Indicators\" },\n    { \"type\": \"table\", \"header\": { \"cells\": [{ \"text\": \"Metric\" }, { \"text\": \"Value\" }] }, \"rows\": [{ \"cells\": [{ \"text\": \"Total Impressions\" }, { \"text\": \"{{ $json.total_impressions }}\" }] }, { \"cells\": [{ \"text\": \"Total Clicks\" }, { \"text\": \"{{ $json.total_clicks }}\" }] }, { \"cells\": [{ \"text\": \"CTR\" }, { \"text\": \"{{ $json.click_through_rate }}%\" }] }, { \"cells\": [{ \"text\": \"Total Conversions\" }, { \"text\": \"{{ $json.total_conversions }}\" }] }, { \"cells\": [{ \"text\": \"Cost per Conversion\" }, { \"text\": \"{{ $json.cost_per_conversion }}\" }] }, { \"cells\": [{ \"text\": \"Total Spend\" }, { \"text\": \"{{ $json.total_spend }}\" }] }, { \"cells\": [{ \"text\": \"ROAS\" }, { \"text\": \"{{ $json.return_on_ad_spend }}x\" }] }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Report generated by Spark Digital Agency.\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "c3d4e5f6-a7b8-9012-cdef-012345678906",
      "name": "Generate Report PDF",
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
            "node": "Extract KPIs",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract KPIs": {
      "main": [
        [
          {
            "node": "Generate Report PDF",
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
Extract KPIs from the client document at [file URL], then generate a branded PDF report. Use the extract_document tool with these fields:

- client_name (TEXT): Client or company name
- report_period (TEXT): Reporting period, e.g. Q1 2026
- total_impressions (INTEGER): Total ad impressions
- total_clicks (INTEGER): Total ad clicks
- click_through_rate (DECIMAL): Click-through rate as a percentage
- total_conversions (INTEGER): Total conversions or leads generated
- cost_per_conversion (CURRENCY_AMOUNT): Average cost per conversion
- total_spend (CURRENCY_AMOUNT): Total advertising spend
- return_on_ad_spend (DECIMAL): Return on ad spend ratio

Then use the generate_document tool to create a branded A4 PDF report presenting these KPIs in a table with the agency and client name.
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
