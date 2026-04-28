---
name: extract-invoices-to-spreadsheet
description: Extract structured data from multiple invoice PDFs in one call, then pipe the results directly into an XLSX accounts payable tracker.
---

# Extract Invoices and Build Accounts Payable Spreadsheet

Accounts payable teams receive PDF invoices from multiple vendors and need to log them into a spreadsheet for the approval workflow. This recipe extracts structured data from all invoices in a single call, then feeds the results directly into an XLSX tracker with one row per invoice.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract structured data from all three invoices
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "invoice-acme.pdf",
        "url": "https://example.com/invoices/invoice-acme.pdf"
      },
      {
        "type": "url",
        "name": "invoice-brightway.pdf",
        "url": "https://example.com/invoices/invoice-brightway.pdf"
      },
      {
        "type": "url",
        "name": "invoice-delta-freight.pdf",
        "url": "https://example.com/invoices/invoice-delta-freight.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "invoices",
          "type": "ARRAY",
          "description": "One entry per invoice file",
          "fields": [
            { "name": "vendor_name", "type": "TEXT", "description": "Vendor or supplier name" },
            { "name": "invoice_number", "type": "TEXT", "description": "Invoice reference number" },
            { "name": "invoice_date", "type": "DATE", "description": "Invoice issue date" },
            { "name": "due_date", "type": "DATE", "description": "Payment due date" },
            { "name": "total_amount", "type": "CURRENCY_AMOUNT", "description": "Total invoice amount" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Invoice currency" }
          ]
        }
      ]
    }
  }')

# Step 2: Generate XLSX accounts payable tracker from the extracted values
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "styles": {
      "header": {
        "is_bold": true,
        "background_color": "#1E3A5F",
        "font_color": "#FFFFFF"
      },
      "body": {
        "font_color": "#333333"
      }
    },
    "sheets": [
      {
        "name": "Accounts Payable",
        "columns": [
          { "name": "Vendor", "width": 30 },
          { "name": "Invoice #", "width": 18 },
          { "name": "Invoice Date", "width": 14 },
          { "name": "Due Date", "width": 14 },
          { "name": "Amount", "width": 16 },
          { "name": "Currency", "width": 12 },
          { "name": "Status", "width": 14 }
        ],
        "rows": [
          [
            { "value": "Acme Office Supplies" },
            { "value": "INV-2026-0041" },
            { "value": "2026-03-01", "format": "date" },
            { "value": "2026-03-31", "format": "date" },
            { "value": 888.98, "format": "currency" },
            { "value": "USD" },
            { "value": "Pending" }
          ],
          [
            { "value": "Brightway Marketing" },
            { "value": "BWM-10094" },
            { "value": "2026-03-05", "format": "date" },
            { "value": "2026-04-04", "format": "date" },
            { "value": 3450.00, "format": "currency" },
            { "value": "USD" },
            { "value": "Pending" }
          ],
          [
            { "value": "Delta Freight Solutions" },
            { "value": "DFS-88321" },
            { "value": "2026-03-10", "format": "date" },
            { "value": "2026-04-09", "format": "date" },
            { "value": 554.85, "format": "currency" },
            { "value": "USD" },
            { "value": "Pending" }
          ]
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract structured data from all three invoices
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "invoice-acme.pdf",
      url: "https://example.com/invoices/invoice-acme.pdf",
    },
    {
      type: "url",
      name: "invoice-brightway.pdf",
      url: "https://example.com/invoices/invoice-brightway.pdf",
    },
    {
      type: "url",
      name: "invoice-delta-freight.pdf",
      url: "https://example.com/invoices/invoice-delta-freight.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "invoices",
        type: "ARRAY",
        description: "One entry per invoice file",
        fields: [
          {
            name: "vendor_name",
            type: "TEXT",
            description: "Vendor or supplier name",
          },
          {
            name: "invoice_number",
            type: "TEXT",
            description: "Invoice reference number",
          },
          {
            name: "invoice_date",
            type: "DATE",
            description: "Invoice issue date",
          },
          { name: "due_date", type: "DATE", description: "Payment due date" },
          {
            name: "total_amount",
            type: "CURRENCY_AMOUNT",
            description: "Total invoice amount",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Invoice currency",
          },
        ],
      },
    ],
  },
});

const invoices = extraction.invoices.value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate XLSX accounts payable tracker from the extracted values
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      is_bold: true,
      background_color: "#1E3A5F",
      font_color: "#FFFFFF",
    },
    body: {
      font_color: "#333333",
    },
  },
  sheets: [
    {
      name: "Accounts Payable",
      columns: [
        { name: "Vendor", width: 30 },
        { name: "Invoice #", width: 18 },
        { name: "Invoice Date", width: 14 },
        { name: "Due Date", width: 14 },
        { name: "Amount", width: 16 },
        { name: "Currency", width: 12 },
        { name: "Status", width: 14 },
      ],
      rows: invoices.map((invoice) => [
        { value: String(invoice.vendor_name.value) },
        { value: String(invoice.invoice_number.value) },
        { value: String(invoice.invoice_date.value), format: "date" },
        { value: String(invoice.due_date.value), format: "date" },
        {
          value: Number(
            (invoice.total_amount.value as { amount: string }).amount,
          ),
          format: "currency",
        },
        { value: String(invoice.currency.value) },
        { value: "Pending" },
      ]),
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract structured data from all three invoices
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "invoice-acme.pdf",
            "url": "https://example.com/invoices/invoice-acme.pdf",
        },
        {
            "type": "url",
            "name": "invoice-brightway.pdf",
            "url": "https://example.com/invoices/invoice-brightway.pdf",
        },
        {
            "type": "url",
            "name": "invoice-delta-freight.pdf",
            "url": "https://example.com/invoices/invoice-delta-freight.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "invoices",
                "type": "ARRAY",
                "description": "One entry per invoice file",
                "fields": [
                    {"name": "vendor_name", "type": "TEXT", "description": "Vendor or supplier name"},
                    {"name": "invoice_number", "type": "TEXT", "description": "Invoice reference number"},
                    {"name": "invoice_date", "type": "DATE", "description": "Invoice issue date"},
                    {"name": "due_date", "type": "DATE", "description": "Payment due date"},
                    {"name": "total_amount", "type": "CURRENCY_AMOUNT", "description": "Total invoice amount"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Invoice currency"},
                ],
            }
        ]
    },
)

invoices = extraction["invoices"]["value"]

# Step 2: Generate XLSX accounts payable tracker from the extracted values
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "is_bold": True,
            "background_color": "#1E3A5F",
            "font_color": "#FFFFFF",
        },
        "body": {
            "font_color": "#333333",
        },
    },
    sheets=[
        {
            "name": "Accounts Payable",
            "columns": [
                {"name": "Vendor", "width": 30},
                {"name": "Invoice #", "width": 18},
                {"name": "Invoice Date", "width": 14},
                {"name": "Due Date", "width": 14},
                {"name": "Amount", "width": 16},
                {"name": "Currency", "width": 12},
                {"name": "Status", "width": 14},
            ],
            "rows": [
                [
                    {"value": invoice["vendor_name"]["value"]},
                    {"value": invoice["invoice_number"]["value"]},
                    {"value": invoice["invoice_date"]["value"], "format": "date"},
                    {"value": invoice["due_date"]["value"], "format": "date"},
                    {"value": float(invoice["total_amount"]["value"]["amount"]), "format": "currency"},
                    {"value": invoice["currency"]["value"]},
                    {"value": "Pending"},
                ]
                for invoice in invoices
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

	// Step 1: Extract structured data from all three invoices
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("invoice-acme.pdf", "https://example.com/invoices/invoice-acme.pdf"),
			il.NewFileFromURL("invoice-brightway.pdf", "https://example.com/invoices/invoice-brightway.pdf"),
			il.NewFileFromURL("invoice-delta-freight.pdf", "https://example.com/invoices/invoice-delta-freight.pdf"),
		},
		Schema: il.ExtractionSchema{
			"invoices": il.NewArrayFieldConfig(
				"invoices",
				"One entry per invoice file",
				[]il.FieldConfig{
					il.NewTextFieldConfig("vendor_name", "Vendor or supplier name"),
					il.NewTextFieldConfig("invoice_number", "Invoice reference number"),
					il.NewDateFieldConfig("invoice_date", "Invoice issue date"),
					il.NewDateFieldConfig("due_date", "Payment due date"),
					il.NewCurrencyAmountFieldConfig("total_amount", "Total invoice amount"),
					il.NewCurrencyCodeFieldConfig("currency", "Invoice currency"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	invoicesRaw, _ := (*extraction)["invoices"].Value.([]interface{})

	rows := make([]il.SheetRow, 0, len(invoicesRaw))
	for _, item := range invoicesRaw {
		invoice, _ := item.(map[string]interface{})
		getStr := func(field string) string {
			f, _ := invoice[field].(map[string]interface{})
			return fmt.Sprintf("%v", f["value"])
		}
		totalAmountField, _ := invoice["total_amount"].(map[string]interface{})
		totalAmountValue, _ := totalAmountField["value"].(map[string]interface{})
		amountStr := fmt.Sprintf("%v", totalAmountValue["amount"])

		rows = append(rows, il.SheetRow{
			{Value: getStr("vendor_name")},
			{Value: getStr("invoice_number")},
			{Value: getStr("invoice_date"), Format: "date"},
			{Value: getStr("due_date"), Format: "date"},
			{Value: amountStr, Format: "currency"},
			{Value: getStr("currency")},
			{Value: "Pending"},
		})
	}

	// Step 2: Generate XLSX accounts payable tracker from the extracted values
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				IsBold:          true,
				BackgroundColor: "#1E3A5F",
				FontColor:       "#FFFFFF",
			},
			Body: &il.CellStyle{
				FontColor: "#333333",
			},
		},
		Sheets: []il.Sheet{
			{
				Name: "Accounts Payable",
				Columns: []il.SheetColumn{
					{Name: "Vendor", Width: 30},
					{Name: "Invoice #", Width: 18},
					{Name: "Invoice Date", Width: 14},
					{Name: "Due Date", Width: 14},
					{Name: "Amount", Width: 16},
					{Name: "Currency", Width: 12},
					{Name: "Status", Width: 14},
				},
				Rows: rows,
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
  "name": "Extract Invoices and Build Accounts Payable Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Invoices and Build Accounts Payable Spreadsheet

Accounts payable teams receive PDF invoices from multiple vendors and need to log them into a spreadsheet for the approval workflow. This recipe extracts structured data from all invoices in a single call, then feeds the results directly into an XLSX tracker with one row per invoice.

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
      "id": "b74d5200-3556-44d1-9654-3cf1b1cb17a8",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Invoice Data
Resource: **Document Extraction**

Configure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "d2e01ad3-378d-4e57-b265-a818cc18ae11",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate AP Spreadsheet
Resource: **Sheet Generation**

Configure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "cb8ecc73-6801-47f2-b17a-948e94a8dd19",
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
      "id": "7a8b9c0d-1e2f-3456-0123-678901234567",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[
  {
    \"name\": \"invoices\",
    \"type\": \"ARRAY\",
    \"description\": \"One entry per invoice file\",
    \"children\": [
      {
        \"name\": \"vendor_name\",
        \"type\": \"TEXT\",
        \"description\": \"Vendor or supplier name\"
      },
      {
        \"name\": \"invoice_number\",
        \"type\": \"TEXT\",
        \"description\": \"Invoice reference number\"
      },
      {
        \"name\": \"invoice_date\",
        \"type\": \"DATE\",
        \"description\": \"Invoice issue date\"
      },
      {
        \"name\": \"due_date\",
        \"type\": \"DATE\",
        \"description\": \"Payment due date\"
      },
      {
        \"name\": \"total_amount\",
        \"type\": \"CURRENCY_AMOUNT\",
        \"description\": \"Total invoice amount\"
      },
      {
        \"name\": \"currency\",
        \"type\": \"CURRENCY_CODE\",
        \"description\": \"Invoice currency\"
      }
    ]
  }
]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/invoices/invoice-acme.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/invoices/invoice-brightway.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/invoices/invoice-delta-freight.pdf"
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
      "id": "8b9c0d1e-2f3a-4567-1234-789012345678",
      "name": "Extract Invoice Data",
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
        "sheetsJson": "[
  {
    \"name\": \"Accounts Payable\",
    \"columns\": [
      { \"name\": \"Vendor\", \"width\": 30 },
      { \"name\": \"Invoice #\", \"width\": 18 },
      { \"name\": \"Invoice Date\", \"width\": 14 },
      { \"name\": \"Due Date\", \"width\": 14 },
      { \"name\": \"Amount\", \"width\": 16 },
      { \"name\": \"Currency\", \"width\": 12 },
      { \"name\": \"Status\", \"width\": 14 }
    ],
    \"rows\": \"{{ $json.invoices }}\"
  }
]",
        "sheetStylesJson": "{
  \"header\": {
    \"is_bold\": true,
    \"background_color\": \"#1E3A5F\",
    \"font_color\": \"#FFFFFF\"
  },
  \"body\": {
    \"font_color\": \"#333333\"
  }
}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "9c0d1e2f-3a4b-5678-2345-890123456789",
      "name": "Generate AP Spreadsheet",
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
            "node": "Extract Invoice Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Invoice Data": {
      "main": [
        [
          {
            "node": "Generate AP Spreadsheet",
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
Extract invoice data from the files at [file URL 1], [file URL 2], and [file URL 3], then generate an accounts payable spreadsheet. Use the extract_document tool with these fields:

- invoices (ARRAY): One entry per invoice file, each with:
  - vendor_name (TEXT): Vendor or supplier name
  - invoice_number (TEXT): Invoice reference number
  - invoice_date (DATE): Invoice issue date
  - due_date (DATE): Payment due date
  - total_amount (CURRENCY_AMOUNT): Total invoice amount
  - currency (CURRENCY_CODE): Invoice currency

Then use the generate_sheet tool to create an XLSX accounts payable tracker with the extracted rows.
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
