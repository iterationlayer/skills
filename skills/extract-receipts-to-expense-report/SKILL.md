---
name: extract-receipts-to-expense-report
description: Extract merchant, date, amount, and category from receipt photos and PDFs in one call, then generate an XLSX expense report with a totals row.
---

# Extract Receipts and Generate Expense Report

Employees and finance teams submit photos or scans of receipts and need them consolidated into an expense report for reimbursement or bookkeeping. This recipe extracts structured data from all receipts in a single call, then generates a formatted XLSX report with a SUM total row ready for approval.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract structured data from all three receipts
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "receipt-dinner.jpg",
        "url": "https://cdn.example.com/receipts/dinner-jan14.jpg"
      },
      {
        "type": "url",
        "name": "receipt-taxi.jpg",
        "url": "https://cdn.example.com/receipts/taxi-jan15.jpg"
      },
      {
        "type": "url",
        "name": "receipt-hotel.pdf",
        "url": "https://cdn.example.com/receipts/hotel-jan15.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "receipts",
          "type": "ARRAY",
          "description": "One entry per receipt file",
          "fields": [
            { "name": "merchant", "type": "TEXT", "description": "Merchant or vendor name" },
            { "name": "date", "type": "DATE", "description": "Transaction date" },
            { "name": "amount", "type": "CURRENCY_AMOUNT", "description": "Total amount paid" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Transaction currency" },
            {
              "name": "category",
              "type": "ENUM",
              "description": "Expense category",
              "values": ["Meals", "Travel", "Accommodation", "Software", "Office Supplies", "Other"],
              "max_selected": 1
            }
          ]
        }
      ]
    }
  }')

# Step 2: Generate XLSX expense report from the extracted values
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "styles": {
      "header": {
        "is_bold": true,
        "background_color": "#2D6A4F",
        "font_color": "#FFFFFF"
      },
      "body": {
        "font_color": "#333333"
      }
    },
    "sheets": [
      {
        "name": "Expense Report — January 2026",
        "columns": [
          { "name": "Date", "width": 12 },
          { "name": "Merchant", "width": 28 },
          { "name": "Category", "width": 18 },
          { "name": "Amount", "width": 14 },
          { "name": "Currency", "width": 10 },
          { "name": "Notes", "width": 24 }
        ],
        "rows": [
          [
            { "value": "2026-01-14", "format": "date" },
            { "value": "Trattoria Roma" },
            { "value": "Meals" },
            { "value": 87.50, "format": "currency" },
            { "value": "USD" },
            { "value": "Team dinner with client" }
          ],
          [
            { "value": "2026-01-15", "format": "date" },
            { "value": "City Cab Co." },
            { "value": "Travel" },
            { "value": 34.20, "format": "currency" },
            { "value": "USD" },
            { "value": "Taxi to airport" }
          ],
          [
            { "value": "2026-01-15", "format": "date" },
            { "value": "Marriott Downtown" },
            { "value": "Accommodation" },
            { "value": 219.00, "format": "currency" },
            { "value": "USD" },
            { "value": "1 night — conference stay" }
          ],
          [
            { "value": "" }
          ],
          [
            { "value": "" },
            { "value": "" },
            {
              "value": "Total",
              "styles": { "is_bold": true, "font_size_in_pt": 12 }
            },
            {
              "value": null,
              "styles": { "is_bold": true, "font_size_in_pt": 12 }
            },
            { "value": "" },
            { "value": "" }
          ]
        ],
        "formulas": [
          { "row": 5, "col": 3, "expression": "=SUM(D2:D4)" }
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract structured data from all three receipts
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "receipt-dinner.jpg",
      url: "https://cdn.example.com/receipts/dinner-jan14.jpg",
    },
    {
      type: "url",
      name: "receipt-taxi.jpg",
      url: "https://cdn.example.com/receipts/taxi-jan15.jpg",
    },
    {
      type: "url",
      name: "receipt-hotel.pdf",
      url: "https://cdn.example.com/receipts/hotel-jan15.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "receipts",
        type: "ARRAY",
        description: "One entry per receipt file",
        fields: [
          {
            name: "merchant",
            type: "TEXT",
            description: "Merchant or vendor name",
          },
          { name: "date", type: "DATE", description: "Transaction date" },
          {
            name: "amount",
            type: "CURRENCY_AMOUNT",
            description: "Total amount paid",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Transaction currency",
          },
          {
            name: "category",
            type: "ENUM",
            description: "Expense category",
            values: [
              "Meals",
              "Travel",
              "Accommodation",
              "Software",
              "Office Supplies",
              "Other",
            ],
            max_selected: 1,
          },
        ],
      },
    ],
  },
});

const receipts = extraction.receipts.value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate XLSX expense report from the extracted values
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      is_bold: true,
      background_color: "#2D6A4F",
      font_color: "#FFFFFF",
    },
    body: {
      font_color: "#333333",
    },
  },
  sheets: [
    {
      name: "Expense Report — January 2026",
      columns: [
        { name: "Date", width: 12 },
        { name: "Merchant", width: 28 },
        { name: "Category", width: 18 },
        { name: "Amount", width: 14 },
        { name: "Currency", width: 10 },
        { name: "Notes", width: 24 },
      ],
      rows: [
        ...receipts.map((receipt) => [
          { value: String(receipt.date.value), format: "date" },
          { value: String(receipt.merchant.value) },
          {
            value: String(
              (receipt.category.value as string[])[0] ?? receipt.category.value,
            ),
          },
          {
            value: Number((receipt.amount.value as { amount: string }).amount),
            format: "currency",
          },
          { value: String(receipt.currency.value) },
          { value: "" },
        ]),
        [{ value: "" }],
        [
          { value: "" },
          { value: "" },
          { value: "Total", styles: { is_bold: true, font_size_in_pt: 12 } },
          { value: null, styles: { is_bold: true, font_size_in_pt: 12 } },
          { value: "" },
          { value: "" },
        ],
      ],
      formulas: [
        {
          row: receipts.length + 2,
          col: 3,
          expression: `=SUM(D2:D${receipts.length + 1})`,
        },
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract structured data from all three receipts
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "receipt-dinner.jpg",
            "url": "https://cdn.example.com/receipts/dinner-jan14.jpg",
        },
        {
            "type": "url",
            "name": "receipt-taxi.jpg",
            "url": "https://cdn.example.com/receipts/taxi-jan15.jpg",
        },
        {
            "type": "url",
            "name": "receipt-hotel.pdf",
            "url": "https://cdn.example.com/receipts/hotel-jan15.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "receipts",
                "type": "ARRAY",
                "description": "One entry per receipt file",
                "fields": [
                    {"name": "merchant", "type": "TEXT", "description": "Merchant or vendor name"},
                    {"name": "date", "type": "DATE", "description": "Transaction date"},
                    {"name": "amount", "type": "CURRENCY_AMOUNT", "description": "Total amount paid"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Transaction currency"},
                    {
                        "name": "category",
                        "type": "ENUM",
                        "description": "Expense category",
                        "values": ["Meals", "Travel", "Accommodation", "Software", "Office Supplies", "Other"],
                        "max_selected": 1,
                    },
                ],
            }
        ]
    },
)

receipts = extraction["receipts"]["value"]
last_data_row = len(receipts) + 1

# Step 2: Generate XLSX expense report from the extracted values
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "is_bold": True,
            "background_color": "#2D6A4F",
            "font_color": "#FFFFFF",
        },
        "body": {
            "font_color": "#333333",
        },
    },
    sheets=[
        {
            "name": "Expense Report — January 2026",
            "columns": [
                {"name": "Date", "width": 12},
                {"name": "Merchant", "width": 28},
                {"name": "Category", "width": 18},
                {"name": "Amount", "width": 14},
                {"name": "Currency", "width": 10},
                {"name": "Notes", "width": 24},
            ],
            "rows": [
                *[
                    [
                        {"value": receipt["date"]["value"], "format": "date"},
                        {"value": receipt["merchant"]["value"]},
                        {"value": receipt["category"]["value"][0] if isinstance(receipt["category"]["value"], list) else receipt["category"]["value"]},
                        {"value": float(receipt["amount"]["value"]["amount"]), "format": "currency"},
                        {"value": receipt["currency"]["value"]},
                        {"value": ""},
                    ]
                    for receipt in receipts
                ],
                [{"value": ""}],
                [
                    {"value": ""},
                    {"value": ""},
                    {"value": "Total", "styles": {"is_bold": True, "font_size_in_pt": 12}},
                    {"value": None, "styles": {"is_bold": True, "font_size_in_pt": 12}},
                    {"value": ""},
                    {"value": ""},
                ],
            ],
            "formulas": [
                {"row": len(receipts) + 2, "col": 3, "expression": f"=SUM(D2:D{last_data_row})"},
            ],
        }
    ],
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

	// Step 1: Extract structured data from all three receipts
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("receipt-dinner.jpg", "https://cdn.example.com/receipts/dinner-jan14.jpg"),
			il.NewFileFromURL("receipt-taxi.jpg", "https://cdn.example.com/receipts/taxi-jan15.jpg"),
			il.NewFileFromURL("receipt-hotel.pdf", "https://cdn.example.com/receipts/hotel-jan15.pdf"),
		},
		Schema: il.ExtractionSchema{
			"receipts": il.NewArrayFieldConfig(
				"receipts",
				"One entry per receipt file",
				[]il.FieldConfig{
					il.NewTextFieldConfig("merchant", "Merchant or vendor name"),
					il.NewDateFieldConfig("date", "Transaction date"),
					il.NewCurrencyAmountFieldConfig("amount", "Total amount paid"),
					il.NewCurrencyCodeFieldConfig("currency", "Transaction currency"),
					il.NewEnumFieldConfig(
						"category",
						"Expense category",
						[]string{"Meals", "Travel", "Accommodation", "Software", "Office Supplies", "Other"},
					),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	receiptsRaw, _ := (*extraction)["receipts"].Value.([]interface{})

	rows := make([]il.SheetRow, 0, len(receiptsRaw)+2)
	for _, item := range receiptsRaw {
		receipt, _ := item.(map[string]interface{})
		getStr := func(field string) string {
			f, _ := receipt[field].(map[string]interface{})
			return fmt.Sprintf("%v", f["value"])
		}
		amountField, _ := receipt["amount"].(map[string]interface{})
		amountValue, _ := amountField["value"].(map[string]interface{})
		amountStr := fmt.Sprintf("%v", amountValue["amount"])

		categoryField, _ := receipt["category"].(map[string]interface{})
		categorySlice, _ := categoryField["value"].([]interface{})
		categoryStr := ""
		if len(categorySlice) > 0 {
			categoryStr = fmt.Sprintf("%v", categorySlice[0])
		}

		rows = append(rows, il.SheetRow{
			{Value: getStr("date"), Format: "date"},
			{Value: getStr("merchant")},
			{Value: categoryStr},
			{Value: amountStr, Format: "currency"},
			{Value: getStr("currency")},
			{Value: ""},
		})
	}

	boldStyle := &il.CellStyle{IsBold: true, FontSizeInPt: 12}
	rows = append(rows,
		il.SheetRow{{Value: ""}},
		il.SheetRow{
			{Value: ""},
			{Value: ""},
			{Value: "Total", Styles: boldStyle},
			{Value: nil, Styles: boldStyle},
			{Value: ""},
			{Value: ""},
		},
	)

	lastDataRow := len(receiptsRaw) + 1
	totalRow := len(receiptsRaw) + 2

	// Step 2: Generate XLSX expense report from the extracted values
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				IsBold:          true,
				BackgroundColor: "#2D6A4F",
				FontColor:       "#FFFFFF",
			},
			Body: &il.CellStyle{
				FontColor: "#333333",
			},
		},
		Sheets: []il.Sheet{
			{
				Name: "Expense Report — January 2026",
				Columns: []il.SheetColumn{
					{Name: "Date", Width: 12},
					{Name: "Merchant", Width: 28},
					{Name: "Category", Width: 18},
					{Name: "Amount", Width: 14},
					{Name: "Currency", Width: 10},
					{Name: "Notes", Width: 24},
				},
				Rows: rows,
				Formulas: []il.Formula{
					{
						Row:        totalRow,
						Col:        3,
						Expression: fmt.Sprintf("=SUM(D2:D%d)", lastDataRow),
					},
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
  "name": "Extract Receipts and Generate Expense Report",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Receipts and Generate Expense Report\n\nEmployees and finance teams submit photos or scans of receipts and need them consolidated into an expense report for reimbursement or bookkeeping. This recipe extracts structured data from all receipts in a single call, then generates a formatted XLSX report with a SUM total row ready for approval.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "9cc45c48-7954-4c7b-9881-fda7062440df",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Receipt Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "0a70efa1-bf36-4984-bb29-d4a877ba5c27",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Expense Report\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "0653a2cc-d045-4e05-ad49-f85b05f5399e",
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
      "id": "d3e4f5a6-b7c8-9012-3def-012345678901",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"receipts\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per receipt file\",\n    \"children\": [\n      {\n        \"name\": \"merchant\",\n        \"type\": \"TEXT\",\n        \"description\": \"Merchant or vendor name\"\n      },\n      {\n        \"name\": \"date\",\n        \"type\": \"DATE\",\n        \"description\": \"Transaction date\"\n      },\n      {\n        \"name\": \"amount\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Total amount paid\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Transaction currency\"\n      },\n      {\n        \"name\": \"category\",\n        \"type\": \"ENUM\",\n        \"description\": \"Expense category\",\n        \"values\": [\"Meals\", \"Travel\", \"Accommodation\", \"Software\", \"Office Supplies\", \"Other\"]\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://cdn.example.com/receipts/dinner-jan14.jpg"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://cdn.example.com/receipts/taxi-jan15.jpg"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://cdn.example.com/receipts/hotel-jan15.pdf"
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
      "id": "e4f5a6b7-c8d9-0123-4ef0-123456789012",
      "name": "Extract Receipt Data",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "sheetFormat": "xlsx",
        "sheetsJson": "[\n  {\n    \"name\": \"Expense Report \u2014 January 2026\",\n    \"columns\": [\n      { \"name\": \"Date\", \"width\": 12 },\n      { \"name\": \"Merchant\", \"width\": 28 },\n      { \"name\": \"Category\", \"width\": 18 },\n      { \"name\": \"Amount\", \"width\": 14 },\n      { \"name\": \"Currency\", \"width\": 10 },\n      { \"name\": \"Notes\", \"width\": 24 }\n    ],\n    \"rows\": \"{{ $json.receipts }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"is_bold\": true,\n    \"background_color\": \"#2D6A4F\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_color\": \"#333333\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "f5a6b7c8-d9e0-1234-5f01-234567890123",
      "name": "Generate Expense Report",
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
            "node": "Extract Receipt Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Receipt Data": {
      "main": [
        [
          {
            "node": "Generate Expense Report",
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
Extract receipt data from the files at [file URL 1], [file URL 2], and [file URL 3], then generate an expense report spreadsheet. Use the extract_document tool with these fields:

- receipts (ARRAY): One entry per receipt file, each with:
  - merchant (TEXT): Merchant or vendor name
  - date (DATE): Transaction date
  - amount (CURRENCY_AMOUNT): Total amount paid
  - currency (CURRENCY_CODE): Transaction currency
  - category (ENUM): Expense category (Meals, Travel, Accommodation, Software, Office Supplies, Other)

Then use the generate_sheet tool to create an XLSX expense report with the extracted rows and a totals row.
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
