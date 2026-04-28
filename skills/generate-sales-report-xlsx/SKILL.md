---
name: generate-sales-report-xlsx
description: Generate a formatted XLSX spreadsheet with sales data, currency formatting, and bold headers.
---

# Generate Sales Report Spreadsheet

Business intelligence teams and reporting platforms use this recipe to generate Excel reports from structured data. Define columns, rows, and cell formats — currency, percentage, bold headers — and receive a ready-to-distribute XLSX file.

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
    "styles": {
      "header": {
        "font_family": "Helvetica",
        "font_size_in_pt": 11,
        "is_bold": true,
        "background_color": "#1E3A5F",
        "font_color": "#FFFFFF"
      },
      "body": {
        "font_family": "Helvetica",
        "font_size_in_pt": 11,
        "font_color": "#333333"
      }
    },
    "sheets": [
      {
        "name": "Q1 Sales",
        "columns": [
          {
            "name": "Region",
            "width": 22
          },
          {
            "name": "Revenue",
            "width": 18
          },
          {
            "name": "Growth",
            "width": 14
          }
        ],
        "rows": [
          [
            {
              "value": "North America"
            },
            {
              "value": 4520000,
              "format": "currency"
            },
            {
              "value": 0.071,
              "format": "percentage"
            }
          ],
          [
            {
              "value": "EMEA"
            },
            {
              "value": 3310000,
              "format": "currency"
            },
            {
              "value": 0.178,
              "format": "percentage"
            }
          ],
          [
            {
              "value": "APAC"
            },
            {
              "value": 1400000,
              "format": "currency"
            },
            {
              "value": 0.273,
              "format": "percentage"
            }
          ]
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#1E3A5F",
      font_color: "#FFFFFF",
    },
    body: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      font_color: "#333333",
    },
  },
  sheets: [
    {
      name: "Q1 Sales",
      columns: [
        {
          name: "Region",
          width: 22,
        },
        {
          name: "Revenue",
          width: 18,
        },
        {
          name: "Growth",
          width: 14,
        },
      ],
      rows: [
        [
          {
            value: "North America",
          },
          {
            value: 4520000,
            format: "currency",
          },
          {
            value: 0.071,
            format: "percentage",
          },
        ],
        [
          {
            value: "EMEA",
          },
          {
            value: 3310000,
            format: "currency",
          },
          {
            value: 0.178,
            format: "percentage",
          },
        ],
        [
          {
            value: "APAC",
          },
          {
            value: 1400000,
            format: "currency",
          },
          {
            value: 0.273,
            format: "percentage",
          },
        ],
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "is_bold": True,
            "background_color": "#1E3A5F",
            "font_color": "#FFFFFF",
        },
        "body": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "font_color": "#333333",
        },
    },
    sheets=[
        {
            "name": "Q1 Sales",
            "columns": [
                {
                    "name": "Region",
                    "width": 22,
                },
                {
                    "name": "Revenue",
                    "width": 18,
                },
                {
                    "name": "Growth",
                    "width": 14,
                },
            ],
            "rows": [
                [
                    {
                        "value": "North America",
                    },
                    {
                        "value": 4520000,
                        "format": "currency",
                    },
                    {
                        "value": 0.071,
                        "format": "percentage",
                    },
                ],
                [
                    {
                        "value": "EMEA",
                    },
                    {
                        "value": 3310000,
                        "format": "currency",
                    },
                    {
                        "value": 0.178,
                        "format": "percentage",
                    },
                ],
                [
                    {
                        "value": "APAC",
                    },
                    {
                        "value": 1400000,
                        "format": "currency",
                    },
                    {
                        "value": 0.273,
                        "format": "percentage",
                    },
                ],
            ],
        }
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				FontFamily:      "Helvetica",
				FontSizeInPt:    11,
				IsBold:          true,
				BackgroundColor: "#1E3A5F",
				FontColor:       "#FFFFFF",
			},
			Body: &il.CellStyle{
				FontFamily:   "Helvetica",
				FontSizeInPt: 11,
				FontColor:    "#333333",
			},
		},
		Sheets: []il.Sheet{
			{
				Name: "Q1 Sales",
				Columns: []il.SheetColumn{
					{
						Name:  "Region",
						Width: 22,
					},
					{
						Name:  "Revenue",
						Width: 18,
					},
					{
						Name:  "Growth",
						Width: 14,
					},
				},
				Rows: []il.SheetRow{
					{
						{
							Value:  "North America",
						},
						{
							Value:  4520000,
							Format: "currency",
						},
						{
							Value:  0.071,
							Format: "percentage",
						},
					},
					{
						{
							Value:  "EMEA",
						},
						{
							Value:  3310000,
							Format: "currency",
						},
						{
							Value:  0.178,
							Format: "percentage",
						},
					},
					{
						{
							Value:  "APAC",
						},
						{
							Value:  1400000,
							Format: "currency",
						},
						{
							Value:  0.273,
							Format: "percentage",
						},
					},
				},
			},
		},
	})
	if err != nil {
		panic(err)
	}
	_ = result
}
```

```n8n
{
  "name": "Generate Sales Report Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Sales Report Spreadsheet\n\nBusiness intelligence teams and reporting platforms use this recipe to generate Excel reports from structured data. Define columns, rows, and cell formats \u2014 currency, percentage, bold headers \u2014 and receive a ready-to-distribute XLSX file.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "ad633e8a-69f0-4690-9d6e-bfdda52d6f75",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Spreadsheet\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "6a962e0c-c0e8-4754-9043-b3dd95d2b285",
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
      "id": "d4bf0905-26f1-400c-a3f5-2915bf0e0953",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "sheetFormat": "xlsx",
        "sheetsJson": "[\n  {\n    \"name\": \"Q1 Sales\",\n    \"columns\": [\n      {\n        \"name\": \"Region\",\n        \"width\": 22\n      },\n      {\n        \"name\": \"Revenue\",\n        \"width\": 18\n      },\n      {\n        \"name\": \"Growth\",\n        \"width\": 14\n      }\n    ],\n    \"rows\": [\n      [\n        {\n          \"value\": \"North America\"\n        },\n        {\n          \"value\": 4520000,\n          \"format\": \"currency\"\n        },\n        {\n          \"value\": 0.071,\n          \"format\": \"percentage\"\n        }\n      ],\n      [\n        {\n          \"value\": \"EMEA\"\n        },\n        {\n          \"value\": 3310000,\n          \"format\": \"currency\"\n        },\n        {\n          \"value\": 0.178,\n          \"format\": \"percentage\"\n        }\n      ],\n      [\n        {\n          \"value\": \"APAC\"\n        },\n        {\n          \"value\": 1400000,\n          \"format\": \"currency\"\n        },\n        {\n          \"value\": 0.273,\n          \"format\": \"percentage\"\n        }\n      ]\n    ]\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"is_bold\": true,\n    \"background_color\": \"#1E3A5F\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"font_color\": \"#333333\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "bf6516ea-c9fc-4669-b932-41725d97f4fa",
      "name": "Generate Spreadsheet",
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
            "node": "Generate Spreadsheet",
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
Generate a formatted XLSX sales report. Use the generate_sheet tool with format "xlsx" and these columns:

- Date (date)
- Product (text)
- Region (text)
- Units Sold (number)
- Revenue (currency)
- Margin % (percentage)

Include styled bold headers, one row per sale, and currency/percentage formatting on the appropriate columns.
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
