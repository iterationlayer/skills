---
name: generate-invoice-spreadsheet
description: Generate an XLSX invoice with company info, line items, subtotal/tax/total formulas, and currency formatting.
---

# Generate Invoice Spreadsheet

Finance teams and billing platforms use this recipe to generate branded invoice spreadsheets on demand. Define your company details, line items with quantities and rates, and let formulas compute subtotals, tax, and totals automatically.

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
        "background_color": "#2B579A",
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
      "name": "Invoice #2026-0187",
      "columns": [
        {
          "name": "Description",
          "width": 35,
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
            "value": "Acme Consulting LLC — Invoice #2026-0187",
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 14,
            },
            "from_col": 0,
            "to_col": 3,
          }
        ],
        [
          {
            "value": "Bill To: Northwind Industries, 1200 Industrial Blvd, Denver, CO 80202",
            "from_col": 0,
            "to_col": 3,
          }
        ],
        [
          {
            "value": "Date: March 21, 2026 | Due: April 20, 2026",
            "from_col": 0,
            "to_col": 3,
          }
        ],
        [
          {
            "value": "",
          }
        ],
        [
          {
            "value": "Brand Strategy Workshop (2 days)",
          },
          {
            "value": 1,
            "format": "number",
          },
          {
            "value": 8500,
            "format": "currency",
          },
          {
            "value": 8500,
            "format": "currency",
          }
        ],
        [
          {
            "value": "UI/UX Design Sprint (5 days)",
          },
          {
            "value": 1,
            "format": "number",
          },
          {
            "value": 15000,
            "format": "currency",
          },
          {
            "value": 15000,
            "format": "currency",
          }
        ],
        [
          {
            "value": "Frontend Development (120 hrs)",
          },
          {
            "value": 120,
            "format": "number",
          },
          {
            "value": 175,
            "format": "currency",
          },
          {
            "value": 21000,
            "format": "currency",
          }
        ],
        [
          {
            "value": "QA & User Acceptance Testing (40 hrs)",
          },
          {
            "value": 40,
            "format": "number",
          },
          {
            "value": 150,
            "format": "currency",
          },
          {
            "value": 6000,
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
            "value": "Subtotal",
            "styles": {
              "is_bold": true,
            },
          },
          {
            "value": "=SUM(D5:D8)",
            "format": "currency",
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
            "value": "Tax (8.25%)",
            "styles": {
              "is_bold": true,
            },
          },
          {
            "value": "=D10*0.0825",
            "format": "currency",
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
            "value": "Total Due",
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 13,
            },
          },
          {
            "value": "=D10+D11",
            "format": "currency",
            "styles": {
              "is_bold": true,
              "font_size_in_pt": 13,
            },
          }
        ]
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
      background_color: "#2B579A",
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
      name: "Invoice #2026-0187",
      columns: [
        {
          name: "Description",
          width: 35,
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
            value: "Acme Consulting LLC — Invoice #2026-0187",
            styles: {
              is_bold: true,
              font_size_in_pt: 14,
            },
            from_col: 0,
            to_col: 3,
          },
        ],
        [
          {
            value:
              "Bill To: Northwind Industries, 1200 Industrial Blvd, Denver, CO 80202",
            from_col: 0,
            to_col: 3,
          },
        ],
        [
          {
            value: "Date: March 21, 2026 | Due: April 20, 2026",
            from_col: 0,
            to_col: 3,
          },
        ],
        [
          {
            value: "",
          },
        ],
        [
          {
            value: "Brand Strategy Workshop (2 days)",
          },
          {
            value: 1,
            format: "number",
          },
          {
            value: 8500,
            format: "currency",
          },
          {
            value: 8500,
            format: "currency",
          },
        ],
        [
          {
            value: "UI/UX Design Sprint (5 days)",
          },
          {
            value: 1,
            format: "number",
          },
          {
            value: 15000,
            format: "currency",
          },
          {
            value: 15000,
            format: "currency",
          },
        ],
        [
          {
            value: "Frontend Development (120 hrs)",
          },
          {
            value: 120,
            format: "number",
          },
          {
            value: 175,
            format: "currency",
          },
          {
            value: 21000,
            format: "currency",
          },
        ],
        [
          {
            value: "QA & User Acceptance Testing (40 hrs)",
          },
          {
            value: 40,
            format: "number",
          },
          {
            value: 150,
            format: "currency",
          },
          {
            value: 6000,
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
            value: "Subtotal",
            styles: {
              is_bold: true,
            },
          },
          {
            value: "=SUM(D5:D8)",
            format: "currency",
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
            value: "Tax (8.25%)",
            styles: {
              is_bold: true,
            },
          },
          {
            value: "=D10*0.0825",
            format: "currency",
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
            value: "Total Due",
            styles: {
              is_bold: true,
              font_size_in_pt: 13,
            },
          },
          {
            value: "=D10+D11",
            format: "currency",
            styles: {
              is_bold: true,
              font_size_in_pt: 13,
            },
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
            "background_color": "#2B579A",
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
        "name": "Invoice #2026-0187",
        "columns": [
            {
                "name": "Description",
                "width": 35,
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
                    "value": "Acme Consulting LLC — Invoice #2026-0187",
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 14,
                    },
                    "from_col": 0,
                    "to_col": 3,
                },
            ],
            [
                {
                    "value": "Bill To: Northwind Industries, 1200 Industrial Blvd, Denver, CO 80202",
                    "from_col": 0,
                    "to_col": 3,
                },
            ],
            [
                {
                    "value": "Date: March 21, 2026 | Due: April 20, 2026",
                    "from_col": 0,
                    "to_col": 3,
                },
            ],
            [
                {
                    "value": "",
                },
            ],
            [
                {
                    "value": "Brand Strategy Workshop (2 days)",
                },
                {
                    "value": 1,
                    "format": "number",
                },
                {
                    "value": 8500,
                    "format": "currency",
                },
                {
                    "value": 8500,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "UI/UX Design Sprint (5 days)",
                },
                {
                    "value": 1,
                    "format": "number",
                },
                {
                    "value": 15000,
                    "format": "currency",
                },
                {
                    "value": 15000,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "Frontend Development (120 hrs)",
                },
                {
                    "value": 120,
                    "format": "number",
                },
                {
                    "value": 175,
                    "format": "currency",
                },
                {
                    "value": 21000,
                    "format": "currency",
                },
            ],
            [
                {
                    "value": "QA & User Acceptance Testing (40 hrs)",
                },
                {
                    "value": 40,
                    "format": "number",
                },
                {
                    "value": 150,
                    "format": "currency",
                },
                {
                    "value": 6000,
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
                    "value": "Subtotal",
                    "styles": {
                        "is_bold": True,
                    },
                },
                {
                    "value": "=SUM(D5:D8)",
                    "format": "currency",
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
                    "value": "Tax (8.25%)",
                    "styles": {
                        "is_bold": True,
                    },
                },
                {
                    "value": "=D10*0.0825",
                    "format": "currency",
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
                    "value": "Total Due",
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 13,
                    },
                },
                {
                    "value": "=D10+D11",
                    "format": "currency",
                    "styles": {
                        "is_bold": True,
                        "font_size_in_pt": 13,
                    },
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
				BackgroundColor: "#2B579A",
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
			Name: "Invoice #2026-0187",
			Columns: []il.SheetColumn{
				{
					Name: "Description",
					Width: 35,
				},
				{
					Name: "Quantity",
					Width: 12,
				},
				{
					Name: "Unit Price",
					Width: 15,
				},
				{
					Name: "Amount",
					Width: 15,
				},
			},
			Rows: []il.SheetRow{
				{
					{
						Value: "Acme Consulting LLC — Invoice #2026-0187",
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 14,
						},
						FromCol: intPtr(0),
						ToCol:   intPtr(3),
					},
				},
				{
					{
						Value:   "Bill To: Northwind Industries, 1200 Industrial Blvd, Denver, CO 80202",
						FromCol: intPtr(0),
						ToCol:   intPtr(3),
					},
				},
				{
					{
						Value:   "Date: March 21, 2026 | Due: April 20, 2026",
						FromCol: intPtr(0),
						ToCol:   intPtr(3),
					},
				},
				{
					{
						Value: "",
					},
				},
				{
					{
						Value: "Brand Strategy Workshop (2 days)",
					},
					{
						Value: 1,
						Format: "number",
					},
					{
						Value: 8500,
						Format: "currency",
					},
					{
						Value: 8500,
						Format: "currency",
					},
				},
				{
					{
						Value: "UI/UX Design Sprint (5 days)",
					},
					{
						Value: 1,
						Format: "number",
					},
					{
						Value: 15000,
						Format: "currency",
					},
					{
						Value: 15000,
						Format: "currency",
					},
				},
				{
					{
						Value: "Frontend Development (120 hrs)",
					},
					{
						Value: 120,
						Format: "number",
					},
					{
						Value: 175,
						Format: "currency",
					},
					{
						Value: 21000,
						Format: "currency",
					},
				},
				{
					{
						Value: "QA & User Acceptance Testing (40 hrs)",
					},
					{
						Value: 40,
						Format: "number",
					},
					{
						Value: 150,
						Format: "currency",
					},
					{
						Value: 6000,
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
						Value: "Subtotal",
						Styles: &il.CellStyle{
							IsBold: true,
						},
					},
					{
						Value:  "=SUM(D5:D8)",
						Format: "currency",
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
						Value: "Tax (8.25%)",
						Styles: &il.CellStyle{
							IsBold: true,
						},
					},
					{
						Value:  "=D10*0.0825",
						Format: "currency",
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
						Value: "Total Due",
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 13,
						},
					},
					{
						Value:  "=D10+D11",
						Format: "currency",
						Styles: &il.CellStyle{
							IsBold:       true,
							FontSizeInPt: 13,
						},
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
  "name": "Generate Invoice Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Invoice Spreadsheet

Finance teams and billing platforms use this recipe to generate branded invoice spreadsheets on demand. Define your company details, line items with quantities and rates, and let formulas compute subtotals, tax, and totals automatically.

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
      "id": "d0026990-ad95-4745-a481-2199d50c24cb",
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
      "id": "4afecc35-37f6-4e82-bb86-898c4907cd93",
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
      "id": "4bc113c8-8784-4a0e-975b-17afed077051",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "sheetFormat": "xlsx",
        "sheetsJson": "[
  {
    \"name\": \"Invoice #2026-0187\",
    \"columns\": [
      {
        \"name\": \"Description\",
        \"width\": 35
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
          \"value\": \"Acme Consulting LLC \u2014 Invoice #2026-0187\",
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 14
          },
          \"from_col\": 0,
          \"to_col\": 3
        }
      ],
      [
        {
          \"value\": \"Bill To: Northwind Industries, 1200 Industrial Blvd, Denver, CO 80202\",
          \"from_col\": 0,
          \"to_col\": 3
        }
      ],
      [
        {
          \"value\": \"Date: March 21, 2026 | Due: April 20, 2026\",
          \"from_col\": 0,
          \"to_col\": 3
        }
      ],
      [
        {
          \"value\": \"\"
        }
      ],
      [
        {
          \"value\": \"Brand Strategy Workshop (2 days)\"
        },
        {
          \"value\": 1,
          \"format\": \"number\"
        },
        {
          \"value\": 8500,
          \"format\": \"currency\"
        },
        {
          \"value\": 8500,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"UI/UX Design Sprint (5 days)\"
        },
        {
          \"value\": 1,
          \"format\": \"number\"
        },
        {
          \"value\": 15000,
          \"format\": \"currency\"
        },
        {
          \"value\": 15000,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"Frontend Development (120 hrs)\"
        },
        {
          \"value\": 120,
          \"format\": \"number\"
        },
        {
          \"value\": 175,
          \"format\": \"currency\"
        },
        {
          \"value\": 21000,
          \"format\": \"currency\"
        }
      ],
      [
        {
          \"value\": \"QA & User Acceptance Testing (40 hrs)\"
        },
        {
          \"value\": 40,
          \"format\": \"number\"
        },
        {
          \"value\": 150,
          \"format\": \"currency\"
        },
        {
          \"value\": 6000,
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
          \"value\": \"Subtotal\",
          \"styles\": {
            \"is_bold\": true
          }
        },
        {
          \"value\": \"=SUM(D5:D8)\",
          \"format\": \"currency\"
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
          \"value\": \"Tax (8.25%)\",
          \"styles\": {
            \"is_bold\": true
          }
        },
        {
          \"value\": \"=D10*0.0825\",
          \"format\": \"currency\"
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
          \"value\": \"Total Due\",
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 13
          }
        },
        {
          \"value\": \"=D10+D11\",
          \"format\": \"currency\",
          \"styles\": {
            \"is_bold\": true,
            \"font_size_in_pt\": 13
          }
        }
      ]
    ]
  }
]",
        "sheetStylesJson": "{
  \"header\": {
    \"font_family\": \"Helvetica\",
    \"font_size_in_pt\": 11,
    \"is_bold\": true,
    \"background_color\": \"#2B579A\",
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
      "id": "26dd7925-5d27-437c-86ea-066d7bf09f96",
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
Generate an XLSX invoice spreadsheet for [customer name]. Use the generate_sheet tool with format "xlsx" and these columns:

- Description (text, width 35)
- Quantity (number, width 12)
- Unit Price (currency, width 15)
- Amount (currency, width 15)

Include a merged header row with [company name] and invoice number, billing address, date/due date, line items, and subtotal/tax/total formulas.
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
