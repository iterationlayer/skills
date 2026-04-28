---
name: generate-billing-statement
description: Generate an XLSX billing statement with a merged company header, subscription line items, overages, credits, and subtotal/tax/total formulas.
---

# Generate Billing Statement

Subscription SaaS platforms use this recipe to generate monthly billing statements as XLSX for enterprise customers who need them for internal accounting and procurement. Define your charges, overages, and credits, and let formulas handle subtotals, tax, and totals automatically.

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
        "background_color": "#4A148C",
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
      "name": "Statement — March 2026",
      "columns": [
        {
          "name": "Date",
          "width": 14,
        },
        {
          "name": "Description",
          "width": 40,
        },
        {
          "name": "Quantity",
          "width": 12,
        },
        {
          "name": "Unit Price",
          "width": 15,
        },
        {
          "name": "Amount",
          "width": 15,
        }
      ],
      "rows": [
        [
          {
            "value": "CloudMetrics Inc. — Billing Statement",
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 14,
            },
            "from_col": 0,
            "to_col": 4,
          }
        ],
        [
          {
            "value": "Customer: Pinnacle Retail Group | Account #ACT-78412",
            "from_col": 0,
            "to_col": 4,
          }
        ],
        [
          {
            "value": "Billing Period: March 1–31, 2026 | Due: April 15, 2026",
            "from_col": 0,
            "to_col": 4,
          }
        ],
        [
          {
            "value": "",
          }
        ],
        [
          {
            "value": "2026-03-01",
            "format": "date",
          },
          {
            "value": "Business Plan — Monthly Subscription",
          },
          {
            "value": 1,
            "format": "number",
          },
          {
            "value": 499,
            "format": "currency",
          },
          {
            "value": 499,
            "format": "currency",
          }
        ],
        [
          {
            "value": "2026-03-01",
            "format": "date",
          },
          {
            "value": "Additional Seats (15 seats)",
          },
          {
            "value": 15,
            "format": "number",
          },
          {
            "value": 12,
            "format": "currency",
          },
          {
            "value": 180,
            "format": "currency",
          }
        ],
        [
          {
            "value": "2026-03-15",
            "format": "date",
          },
          {
            "value": "API Overage — 52,000 extra requests",
          },
          {
            "value": 52,
            "format": "number",
          },
          {
            "value": 0.50,
            "format": "currency",
          },
          {
            "value": 26,
            "format": "currency",
          }
        ],
        [
          {
            "value": "2026-03-15",
            "format": "date",
          },
          {
            "value": "Storage Overage — 120 GB above plan",
          },
          {
            "value": 120,
            "format": "number",
          },
          {
            "value": 0.15,
            "format": "currency",
          },
          {
            "value": 18,
            "format": "currency",
          }
        ],
        [
          {
            "value": "2026-03-01",
            "format": "date",
          },
          {
            "value": "Annual Loyalty Credit",
          },
          {
            "value": 1,
            "format": "number",
          },
          {
            "value": -50,
            "format": "currency",
          },
          {
            "value": -50,
            "format": "currency",
          }
        ],
        [
          {
            "value": "",
          }
        ],
        [
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "Subtotal",
            "styles": {
              "is_bold": true,
            },
          },
          {
            "value": null,
          }
        ],
        [
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "Tax (9.5%)",
            "styles": {
              "is_bold": true,
            },
          },
          {
            "value": null,
          }
        ],
        [
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "",
          },
          {
            "value": "Total Due",
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 13,
            },
          },
          {
            "value": null,
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 13,
            },
          }
        ]
      ],
      "formulas": [
        {
          "row": 10,
          "col": 4,
          "expression": "=SUM(E5:E9)",
        },
        {
          "row": 11,
          "col": 4,
          "expression": "=E11*0.095",
        },
        {
          "row": 12,
          "col": 4,
          "expression": "=E11+E12",
        }
      ],
      "merged_cells": [
        {
          "start_row": 0,
          "start_col": 0,
          "end_row": 0,
          "end_col": 4,
        },
        {
          "start_row": 1,
          "start_col": 0,
          "end_row": 1,
          "end_col": 4,
        },
        {
          "start_row": 2,
          "start_col": 0,
          "end_row": 2,
          "end_col": 4,
        }
      ]
    }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({
  apiKey: "YOUR_API_KEY",
});

const result = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#4A148C",
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
      name: "Statement — March 2026",
      columns: [
        {
          name: "Date",
          width: 14,
        },
        {
          name: "Description",
          width: 40,
        },
        {
          name: "Quantity",
          width: 12,
        },
        {
          name: "Unit Price",
          width: 15,
        },
        {
          name: "Amount",
          width: 15,
        },
      ],
      rows: [
        [
          {
            value: "CloudMetrics Inc. — Billing Statement",
            styles: {
              is_bold: true,
              font_size_in_pt: 14,
            },
            from_col: 0,
            to_col: 4,
          },
        ],
        [
          {
            value: "Customer: Pinnacle Retail Group | Account #ACT-78412",
            from_col: 0,
            to_col: 4,
          },
        ],
        [
          {
            value: "Billing Period: March 1–31, 2026 | Due: April 15, 2026",
            from_col: 0,
            to_col: 4,
          },
        ],
        [
          {
            value: "",
          },
        ],
        [
          {
            value: "2026-03-01",
            format: "date",
          },
          {
            value: "Business Plan — Monthly Subscription",
          },
          {
            value: 1,
            format: "number",
          },
          {
            value: 499,
            format: "currency",
          },
          {
            value: 499,
            format: "currency",
          },
        ],
        [
          {
            value: "2026-03-01",
            format: "date",
          },
          {
            value: "Additional Seats (15 seats)",
          },
          {
            value: 15,
            format: "number",
          },
          {
            value: 12,
            format: "currency",
          },
          {
            value: 180,
            format: "currency",
          },
        ],
        [
          {
            value: "2026-03-15",
            format: "date",
          },
          {
            value: "API Overage — 52,000 extra requests",
          },
          {
            value: 52,
            format: "number",
          },
          {
            value: 0.5,
            format: "currency",
          },
          {
            value: 26,
            format: "currency",
          },
        ],
        [
          {
            value: "2026-03-15",
            format: "date",
          },
          {
            value: "Storage Overage — 120 GB above plan",
          },
          {
            value: 120,
            format: "number",
          },
          {
            value: 0.15,
            format: "currency",
          },
          {
            value: 18,
            format: "currency",
          },
        ],
        [
          {
            value: "2026-03-01",
            format: "date",
          },
          {
            value: "Annual Loyalty Credit",
          },
          {
            value: 1,
            format: "number",
          },
          {
            value: -50,
            format: "currency",
          },
          {
            value: -50,
            format: "currency",
          },
        ],
        [
          {
            value: "",
          },
        ],
        [
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "Subtotal",
            styles: {
              is_bold: true,
            },
          },
          {
            value: null,
          },
        ],
        [
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "Tax (9.5%)",
            styles: {
              is_bold: true,
            },
          },
          {
            value: null,
          },
        ],
        [
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "",
          },
          {
            value: "Total Due",
            styles: {
              is_bold: true,
              font_size_in_pt: 13,
            },
          },
          {
            value: null,
            styles: {
              is_bold: true,
              font_size_in_pt: 13,
            },
          },
        ],
      ],
      formulas: [
        {
          row: 10,
          col: 4,
          expression: "=SUM(E5:E9)",
        },
        {
          row: 11,
          col: 4,
          expression: "=E11*0.095",
        },
        {
          row: 12,
          col: 4,
          expression: "=E11+E12",
        },
      ],
      merged_cells: [
        {
          start_row: 0,
          start_col: 0,
          end_row: 0,
          end_col: 4,
        },
        {
          start_row: 1,
          start_col: 0,
          end_row: 1,
          end_col: 4,
        },
        {
          start_row: 2,
          start_col: 0,
          end_row: 2,
          end_col: 4,
        },
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
            "background_color": "#4A148C",
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
        "name": "Statement — March 2026",
        "columns": [
            {
                "name": "Date",
                "width": 14,
            },
            {
                "name": "Description",
                "width": 40,
            },
            {
                "name": "Quantity",
                "width": 12,
            },
            {
                "name": "Unit Price",
                "width": 15,
            },
            {
                "name": "Amount",
                "width": 15,
            },
        ],
        "rows": [
            [
                {
                    "value": "CloudMetrics Inc. — Billing Statement",
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 14,
                    },
                    "from_col": 0,
                    "to_col": 4,
                },
            ],
            [
                {
                    "value": "Customer: Pinnacle Retail Group | Account #ACT-78412",
                    "from_col": 0,
                    "to_col": 4,
                },
            ],
            [
                {
                    "value": "Billing Period: March 1–31, 2026 | Due: April 15, 2026",
                    "from_col": 0,
                    "to_col": 4,
                },
            ],
            [
                {
                    "value": "",
                },
            ],
            [
                {
                    "value": "2026-03-01",
                    "format": "date",
                },
                {
                    "value": "Business Plan — Monthly Subscription",
                },
                {
                    "value": 1,
                    "format": "number",
                },
                {
                    "value": 499,
                    "format": "currency",
                },
                {
                    "value": 499,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "2026-03-01",
                    "format": "date",
                },
                {
                    "value": "Additional Seats (15 seats)",
                },
                {
                    "value": 15,
                    "format": "number",
                },
                {
                    "value": 12,
                    "format": "currency",
                },
                {
                    "value": 180,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "2026-03-15",
                    "format": "date",
                },
                {
                    "value": "API Overage — 52,000 extra requests",
                },
                {
                    "value": 52,
                    "format": "number",
                },
                {
                    "value": 0.50,
                    "format": "currency",
                },
                {
                    "value": 26,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "2026-03-15",
                    "format": "date",
                },
                {
                    "value": "Storage Overage — 120 GB above plan",
                },
                {
                    "value": 120,
                    "format": "number",
                },
                {
                    "value": 0.15,
                    "format": "currency",
                },
                {
                    "value": 18,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "2026-03-01",
                    "format": "date",
                },
                {
                    "value": "Annual Loyalty Credit",
                },
                {
                    "value": 1,
                    "format": "number",
                },
                {
                    "value": -50,
                    "format": "currency",
                },
                {
                    "value": -50,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "",
                },
            ],
            [
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "Subtotal",
                    "styles": {
                        "is_bold": True,
                    },
                },
                {
                    "value": None,
                },
            ],
            [
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "Tax (9.5%)",
                    "styles": {
                        "is_bold": True,
                    },
                },
                {
                    "value": None,
                },
            ],
            [
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "",
                },
                {
                    "value": "Total Due",
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 13,
                    },
                },
                {
                    "value": None,
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 13,
                    },
                },
            ],
        ],
        "formulas": [
            {
                "row": 10,
                "col": 4,
                "expression": "=SUM(E5:E9)",
            },
            {
                "row": 11,
                "col": 4,
                "expression": "=E11*0.095",
            },
            {
                "row": 12,
                "col": 4,
                "expression": "=E11+E12",
            },
        ],
        "merged_cells": [
            {
                "start_row": 0,
                "start_col": 0,
                "end_row": 0,
                "end_col": 4,
            },
            {
                "start_row": 1,
                "start_col": 0,
                "end_row": 1,
                "end_col": 4,
            },
            {
                "start_row": 2,
                "start_col": 0,
                "end_row": 2,
                "end_col": 4,
            },
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
				BackgroundColor: "#4A148C",
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
			Name: "Statement — March 2026",
			Columns: []il.SheetColumn{
				{
					Name:  "Date",
					Width: 14,
				},
				{
					Name:  "Description",
					Width: 40,
				},
				{
					Name:  "Quantity",
					Width: 12,
				},
				{
					Name:  "Unit Price",
					Width: 15,
				},
				{
					Name:  "Amount",
					Width: 15,
				},
			},
			Rows: []il.SheetRow{
				{
					{
						Value: "CloudMetrics Inc. — Billing Statement",
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 14,
						},
						FromCol: intPtr(0),
						ToCol:   intPtr(4),
					},
				},
				{
					{
						Value:   "Customer: Pinnacle Retail Group | Account #ACT-78412",
						FromCol: intPtr(0),
						ToCol:   intPtr(4),
					},
				},
				{
					{
						Value:   "Billing Period: March 1–31, 2026 | Due: April 15, 2026",
						FromCol: intPtr(0),
						ToCol:   intPtr(4),
					},
				},
				{
					{
						Value: "",
					},
				},
				{
					{
						Value:  "2026-03-01",
						Format: "date",
					},
					{
						Value: "Business Plan — Monthly Subscription",
					},
					{
						Value:  1,
						Format: "number",
					},
					{
						Value:  499,
						Format: "currency",
					},
					{
						Value:  499,
						Format: "currency",
					},
				},
				{
					{
						Value:  "2026-03-01",
						Format: "date",
					},
					{
						Value: "Additional Seats (15 seats)",
					},
					{
						Value:  15,
						Format: "number",
					},
					{
						Value:  12,
						Format: "currency",
					},
					{
						Value:  180,
						Format: "currency",
					},
				},
				{
					{
						Value:  "2026-03-15",
						Format: "date",
					},
					{
						Value: "API Overage — 52,000 extra requests",
					},
					{
						Value:  52,
						Format: "number",
					},
					{
						Value:  0.50,
						Format: "currency",
					},
					{
						Value:  26,
						Format: "currency",
					},
				},
				{
					{
						Value:  "2026-03-15",
						Format: "date",
					},
					{
						Value: "Storage Overage — 120 GB above plan",
					},
					{
						Value:  120,
						Format: "number",
					},
					{
						Value:  0.15,
						Format: "currency",
					},
					{
						Value:  18,
						Format: "currency",
					},
				},
				{
					{
						Value:  "2026-03-01",
						Format: "date",
					},
					{
						Value: "Annual Loyalty Credit",
					},
					{
						Value:  1,
						Format: "number",
					},
					{
						Value:  -50,
						Format: "currency",
					},
					{
						Value:  -50,
						Format: "currency",
					},
				},
				{
					{
						Value: "",
					},
				},
				{
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "Subtotal",
						Styles: &il.CellStyle{
							IsBold: true,
						},
					},
					{
						Value: nil,
					},
				},
				{
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "Tax (9.5%)",
						Styles: &il.CellStyle{
							IsBold: true,
						},
					},
					{
						Value: nil,
					},
				},
				{
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "",
					},
					{
						Value: "Total Due",
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 13,
						},
					},
					{
						Value: nil,
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 13,
						},
					},
				},
			},
			Formulas: []il.Formula{
				{
					Row:        10,
					Col:        4,
					Expression: "=SUM(E5:E9)",
				},
				{
					Row:        11,
					Col:        4,
					Expression: "=E11*0.095",
				},
				{
					Row:        12,
					Col:        4,
					Expression: "=E11+E12",
				},
			},
			MergedCells: []il.MergedCell{
				{
					StartRow: 0,
					StartCol: 0,
					EndRow:   0,
					EndCol:   4,
				},
				{
					StartRow: 1,
					StartCol: 0,
					EndRow:   1,
					EndCol:   4,
				},
				{
					StartRow: 2,
					StartCol: 0,
					EndRow:   2,
					EndCol:   4,
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
  "name": "Generate Billing Statement",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Billing Statement

Subscription SaaS platforms use this recipe to generate monthly billing statements as XLSX for enterprise customers who need them for internal accounting and procurement. Define your charges, overages, and credits, and let formulas handle subtotals, tax, and totals automatically.

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
      "id": "be6af81d-13b3-4e80-baae-ad9f3664031e",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Spreadsheet
Resource: **Sheet Generation**

Configure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "7cc399fe-4ffe-41fd-b760-9ba76612417c",
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
      "id": "0b75aa47-44e7-4fb4-8467-a19847a5e03b",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "sheetFormat": "xlsx",
        "sheetsJson": "[
  {
    \"name\": \"Statement \u2014 March 2026\",
    \"columns\": [
      {
        \"name\": \"Date\",
        \"width\": 14
      },
      {
        \"name\": \"Description\",
        \"width\": 40
      },
      {
        \"name\": \"Quantity\",
        \"width\": 12
      },
      {
        \"name\": \"Unit Price\",
        \"width\": 15
      },
      {
        \"name\": \"Amount\",
        \"width\": 15
      }
    ],
    \"rows\": [
      [
        {
          \"value\": \"CloudMetrics Inc. \u2014 Billing Statement\",
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 14
          },
          \"from_col\": 0,
          \"to_col\": 4
        }
      ],
      [
        {
          \"value\": \"Customer: Pinnacle Retail Group | Account #ACT-78412\",
          \"from_col\": 0,
          \"to_col\": 4
        }
      ],
      [
        {
          \"value\": \"Billing Period: March 1\u201331, 2026 | Due: April 15, 2026\",
          \"from_col\": 0,
          \"to_col\": 4
        }
      ],
      [
        {
          \"value\": \"\"
        }
      ],
      [
        {
          \"value\": \"2026-03-01\",
          \"format\": \"date\"
        },
        {
          \"value\": \"Business Plan \u2014 Monthly Subscription\"
        },
        {
          \"value\": 1,
          \"format\": \"number\"
        },
        {
          \"value\": 499,
          \"format\": \"currency\"
        },
        {
          \"value\": 499,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"2026-03-01\",
          \"format\": \"date\"
        },
        {
          \"value\": \"Additional Seats (15 seats)\"
        },
        {
          \"value\": 15,
          \"format\": \"number\"
        },
        {
          \"value\": 12,
          \"format\": \"currency\"
        },
        {
          \"value\": 180,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"2026-03-15\",
          \"format\": \"date\"
        },
        {
          \"value\": \"API Overage \u2014 52,000 extra requests\"
        },
        {
          \"value\": 52,
          \"format\": \"number\"
        },
        {
          \"value\": 0.5,
          \"format\": \"currency\"
        },
        {
          \"value\": 26,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"2026-03-15\",
          \"format\": \"date\"
        },
        {
          \"value\": \"Storage Overage \u2014 120 GB above plan\"
        },
        {
          \"value\": 120,
          \"format\": \"number\"
        },
        {
          \"value\": 0.15,
          \"format\": \"currency\"
        },
        {
          \"value\": 18,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"2026-03-01\",
          \"format\": \"date\"
        },
        {
          \"value\": \"Annual Loyalty Credit\"
        },
        {
          \"value\": 1,
          \"format\": \"number\"
        },
        {
          \"value\": -50,
          \"format\": \"currency\"
        },
        {
          \"value\": -50,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"\"
        }
      ],
      [
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"Subtotal\",
          \"styles\": {
            \"is_bold\": true
          }
        },
        {
          \"value\": null
        }
      ],
      [
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"Tax (9.5%)\",
          \"styles\": {
            \"is_bold\": true
          }
        },
        {
          \"value\": null
        }
      ],
      [
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"\"
        },
        {
          \"value\": \"Total Due\",
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 13
          }
        },
        {
          \"value\": null,
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 13
          }
        }
      ]
    ],
    \"formulas\": [
      {
        \"row\": 10,
        \"col\": 4,
        \"expression\": \"=SUM(E5:E9)\"
      },
      {
        \"row\": 11,
        \"col\": 4,
        \"expression\": \"=E11*0.095\"
      },
      {
        \"row\": 12,
        \"col\": 4,
        \"expression\": \"=E11+E12\"
      }
    ],
    \"merged_cells\": [
      {
        \"start_row\": 0,
        \"start_col\": 0,
        \"end_row\": 0,
        \"end_col\": 4
      },
      {
        \"start_row\": 1,
        \"start_col\": 0,
        \"end_row\": 1,
        \"end_col\": 4
      },
      {
        \"start_row\": 2,
        \"start_col\": 0,
        \"end_row\": 2,
        \"end_col\": 4
      }
    ]
  }
]",
        "sheetStylesJson": "{
  \"header\": {
    \"font_family\": \"Helvetica\",
    \"font_size_in_pt\": 11,
    \"is_bold\": true,
    \"background_color\": \"#4A148C\",
    \"font_color\": \"#FFFFFF\"
  },
  \"body\": {
    \"font_family\": \"Helvetica\",
    \"font_size_in_pt\": 11,
    \"font_color\": \"#333333\"
  }
}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "59d8c1e0-1c23-4ef6-8ac9-601d193c7516",
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
Generate an XLSX billing statement for [customer name]. Use the generate_sheet tool with format "xlsx" and these columns:

- Date (date, width 14)
- Description (text, width 40)
- Quantity (number, width 12)
- Unit Price (currency, width 15)
- Amount (currency, width 15)

Include a merged header with [company name], customer and account info, billing period, subscription charges, overage line items, credits, and subtotal/tax/total formulas.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "UEsDBBQAAAAIAA...",
    "mime_type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
