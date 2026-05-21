---
name: generate-multi-client-usage-report
description: Generate a multi-sheet XLSX report tracking API usage, credit consumption, and billing across multiple agency clients.
---

# Generate a Multi-Client Usage Report

Agencies reselling API services to multiple clients use this recipe to generate consolidated monthly usage reports — with one sheet per client showing credit consumption, request counts, and billing totals.

## APIs Used

Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "sheets": [
      {
        "name": "Summary",
        "columns": [
          {
            "name": "Client",
            "width": 25
          },
          {
            "name": "Plan",
            "width": 15
          },
          {
            "name": "Credits Used",
            "width": 15
          },
          {
            "name": "Credits Remaining",
            "width": 18
          },
          {
            "name": "Requests",
            "width": 12
          },
          {
            "name": "Amount Billed",
            "width": 16
          }
        ],
        "rows": [
          [
            "Greenfield Organics",
            "Professional",
            4200,
            800,
            312,
            {
              "value": 420.00,
              "format": "currency",
              "currency_code": "EUR"
            }
          ],
          [
            "Atlas Logistics",
            "Enterprise",
            12800,
            7200,
            891,
            {
              "value": 1280.00,
              "format": "currency",
              "currency_code": "EUR"
            }
          ],
          [
            "Bright Pixel Studios",
            "Starter",
            850,
            150,
            64,
            {
              "value": 85.00,
              "format": "currency",
              "currency_code": "EUR"
            }
          ]
        ]
      },
      {
        "name": "Greenfield Organics",
        "columns": [
          {
            "name": "Date",
            "width": 14
          },
          {
            "name": "API",
            "width": 22
          },
          {
            "name": "Requests",
            "width": 12
          },
          {
            "name": "Credits",
            "width": 12
          },
          {
            "name": "Status",
            "width": 12
          }
        ],
        "rows": [
          [
            {
              "value": "2026-03-01",
              "format": "date"
            },
            "Document Extraction",
            45,
            540,
            "Success"
          ],
          [
            {
              "value": "2026-03-08",
              "format": "date"
            },
            "Document Generation",
            28,
            336,
            "Success"
          ],
          [
            {
              "value": "2026-03-15",
              "format": "date"
            },
            "Image Generation",
            120,
            1440,
            "Success"
          ],
          [
            {
              "value": "2026-03-22",
              "format": "date"
            },
            "Document Extraction",
            62,
            744,
            "Success"
          ],
          [
            {
              "value": "2026-03-29",
              "format": "date"
            },
            "Image Transformation",
            57,
            1140,
            "Success"
          ]
        ]
      },
      {
        "name": "Atlas Logistics",
        "columns": [
          {
            "name": "Date",
            "width": 14
          },
          {
            "name": "API",
            "width": 22
          },
          {
            "name": "Requests",
            "width": 12
          },
          {
            "name": "Credits",
            "width": 12
          },
          {
            "name": "Status",
            "width": 12
          }
        ],
        "rows": [
          [
            {
              "value": "2026-03-03",
              "format": "date"
            },
            "Document Extraction",
            340,
            4080,
            "Success"
          ],
          [
            {
              "value": "2026-03-10",
              "format": "date"
            },
            "Sheet Generation",
            180,
            2160,
            "Success"
          ],
          [
            {
              "value": "2026-03-17",
              "format": "date"
            },
            "Document to Markdown",
            210,
            2520,
            "Success"
          ],
          [
            {
              "value": "2026-03-24",
              "format": "date"
            },
            "Document Extraction",
            161,
            1932,
            "Success"
          ],
          [
            {
              "value": "2026-03-31",
              "format": "date"
            },
            "Document Generation",
            0,
            0,
            "No requests"
          ]
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const report = await client.generateSheet({
  format: "xlsx",
  sheets: [
    {
      name: "Summary",
      columns: [
        { name: "Client", width: 25 }, { name: "Plan", width: 15 }, { name: "Credits Used", width: 15 },
        { name: "Credits Remaining", width: 18 }, { name: "Requests", width: 12 }, { name: "Amount Billed", width: 16 },
      ],
      rows: [
        ["Greenfield Organics", "Professional", 4200, 800, 312, { value: 420.00, format: "currency", currency_code: "EUR" }],
        ["Atlas Logistics", "Enterprise", 12800, 7200, 891, { value: 1280.00, format: "currency", currency_code: "EUR" }],
        ["Bright Pixel Studios", "Starter", 850, 150, 64, { value: 85.00, format: "currency", currency_code: "EUR" }],
      ],
    },
    {
      name: "Greenfield Organics",
      columns: [
        { name: "Date", width: 14 }, { name: "API", width: 22 }, { name: "Requests", width: 12 },
        { name: "Credits", width: 12 }, { name: "Status", width: 12 },
      ],
      rows: [
        [{ value: "2026-03-01", format: "date" }, "Document Extraction", 45, 540, "Success"],
        [{ value: "2026-03-08", format: "date" }, "Document Generation", 28, 336, "Success"],
        [{ value: "2026-03-15", format: "date" }, "Image Generation", 120, 1440, "Success"],
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

report = client.generate_sheet(
    format="xlsx",
    sheets=[
        {
            "name": "Summary",
            "columns": [
                {"name": "Client", "width": 25}, {"name": "Plan", "width": 15}, {"name": "Credits Used", "width": 15},
                {"name": "Credits Remaining", "width": 18}, {"name": "Requests", "width": 12}, {"name": "Amount Billed", "width": 16},
            ],
            "rows": [
                ["Greenfield Organics", "Professional", 4200, 800, 312, {"value": 420.00, "format": "currency", "currency_code": "EUR"}],
                ["Atlas Logistics", "Enterprise", 12800, 7200, 891, {"value": 1280.00, "format": "currency", "currency_code": "EUR"}],
                ["Bright Pixel Studios", "Starter", 850, 150, 64, {"value": 85.00, "format": "currency", "currency_code": "EUR"}],
            ],
        },
        {
            "name": "Greenfield Organics",
            "columns": [{"name": "Date", "width": 14}, {"name": "API", "width": 22}, {"name": "Requests", "width": 12}, {"name": "Credits", "width": 12}, {"name": "Status", "width": 12}],
            "rows": [
                [{"value": "2026-03-01", "format": "date"}, "Document Extraction", 45, 540, "Success"],
                [{"value": "2026-03-08", "format": "date"}, "Document Generation", 28, 336, "Success"],
            ],
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	_, err := client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Sheets: []il.Sheet{
			{
				Name: "Summary",
				Columns: []il.SheetColumn{
					{Name: "Client", Width: 25}, {Name: "Plan", Width: 15}, {Name: "Credits Used", Width: 15},
					{Name: "Credits Remaining", Width: 18}, {Name: "Requests", Width: 12}, {Name: "Amount Billed", Width: 16},
				},
				Rows: [][]il.SheetCell{
					{{Value: "Greenfield Organics"}, {Value: "Professional"}, {Value: 4200}, {Value: 800}, {Value: 312}, {Value: 420.00, Format: "currency", CurrencyCode: "EUR"}},
					{{Value: "Atlas Logistics"}, {Value: "Enterprise"}, {Value: 12800}, {Value: 7200}, {Value: 891}, {Value: 1280.00, Format: "currency", CurrencyCode: "EUR"}},
				},
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
  "name": "Generate multi-client usage report in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate a Multi-Client Usage Report\n\nAgencies reselling API services to multiple clients use this recipe to generate consolidated monthly usage reports — with one sheet per client.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "e7f8a9b0-c1d2-3456-efab-456789012301",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Report\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "e7f8a9b0-c1d2-3456-efab-456789012302",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "e7f8a9b0-c1d2-3456-efab-456789012303",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "format": "xlsx",
        "sheetsJson": "[{\"name\":\"Summary\",\"columns\":[{\"name\":\"Client\",\"width\":25},{\"name\":\"Plan\",\"width\":15},{\"name\":\"Credits Used\",\"width\":15},{\"name\":\"Credits Remaining\",\"width\":18},{\"name\":\"Requests\",\"width\":12},{\"name\":\"Amount Billed\",\"width\":16}],\"rows\":[[\"Greenfield Organics\",\"Professional\",4200,800,312,{\"value\":420.00,\"format\":\"currency\",\"currency_code\":\"EUR\"}],[\"Atlas Logistics\",\"Enterprise\",12800,7200,891,{\"value\":1280.00,\"format\":\"currency\",\"currency_code\":\"EUR\"}]]}]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "e7f8a9b0-c1d2-3456-efab-456789012304",
      "name": "Generate Usage Report",
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
            "node": "Generate Usage Report",
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
Generate a multi-client usage report for [month/year]. Use the generate_sheet tool to create an XLSX workbook with:

- A "Summary" sheet with columns: Client, Plan, Credits Used, Credits Remaining, Requests, Amount Billed — one row per client
- One detail sheet per client with columns: Date, API, Requests, Credits, Status — showing weekly usage breakdowns

Include realistic data for [number of clients] clients with varied plan tiers.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "UEsDBBQAAAA...",
    "mime_type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
