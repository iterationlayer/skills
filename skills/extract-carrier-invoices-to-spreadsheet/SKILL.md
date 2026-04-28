---
name: extract-carrier-invoices-to-spreadsheet
description: Extract line-level charges from multiple carrier invoices, then generate an XLSX freight cost tracker in a single pipeline.
---

# Extract Carrier Invoices to Spreadsheet

Logistics and freight operations teams receive PDF invoices from multiple carriers each month and need to reconcile all charges against shipment budgets. This pipeline extracts per-shipment charges from carrier invoices and writes them directly into a formatted XLSX cost tracker ready for finance review.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract carrier invoice data from all three PDFs
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "ups-invoice-march.pdf",
        "url": "https://example.com/freight/ups-invoice-march.pdf"
      },
      {
        "type": "url",
        "name": "fedex-invoice-march.pdf",
        "url": "https://example.com/freight/fedex-invoice-march.pdf"
      },
      {
        "type": "url",
        "name": "dhl-invoice-march.pdf",
        "url": "https://example.com/freight/dhl-invoice-march.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "carrier_invoices",
          "type": "ARRAY",
          "description": "One entry per carrier invoice",
          "fields": [
            { "name": "carrier", "type": "TEXT", "description": "Carrier name" },
            { "name": "invoice_number", "type": "TEXT", "description": "Invoice reference number" },
            { "name": "invoice_date", "type": "DATE", "description": "Invoice date" },
            { "name": "shipment_reference", "type": "TEXT", "description": "Shipment or tracking reference number" },
            { "name": "service_type", "type": "TEXT", "description": "Service level, e.g. Express or Ground" },
            { "name": "charges", "type": "CURRENCY_AMOUNT", "description": "Total charges for this shipment" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Billing currency" }
          ]
        }
      ]
    }
  }')

# Step 2: Generate XLSX freight cost tracker from the extracted invoice data
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
        "background_color": "#B45309",
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
        "name": "Freight Cost Tracker — March 2026",
        "columns": [
          { "name": "Carrier", "width": 16 },
          { "name": "Invoice #", "width": 18 },
          { "name": "Date", "width": 12 },
          { "name": "Shipment Ref", "width": 20 },
          { "name": "Service", "width": 14 },
          { "name": "Charges", "width": 14 },
          { "name": "Currency", "width": 10 }
        ],
        "rows": [
          [
            { "value": "UPS" },
            { "value": "UPS-INV-20260318-4471" },
            { "value": "2026-03-18", "format": "date" },
            { "value": "1Z4E29W00391842610" },
            { "value": "Express" },
            { "value": 184.50, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "FedEx" },
            { "value": "FX-9842201-MAR26" },
            { "value": "2026-03-19", "format": "date" },
            { "value": "795089650512" },
            { "value": "Ground" },
            { "value": 67.20, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "DHL" },
            { "value": "DHL-EXP-2026-88341" },
            { "value": "2026-03-21", "format": "date" },
            { "value": "JD014600006261234560" },
            { "value": "Express" },
            { "value": 212.75, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "Total" },
            { "value": null },
            { "value": null },
            { "value": null },
            { "value": null },
            { "value": null },
            { "value": null }
          ]
        ],
        "formulas": [
          {
            "row": 4,
            "col": 6,
            "expression": "=SUM(F2:F4)"
          }
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract carrier invoice data from all three PDFs
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "ups-invoice-march.pdf",
      url: "https://example.com/freight/ups-invoice-march.pdf",
    },
    {
      type: "url",
      name: "fedex-invoice-march.pdf",
      url: "https://example.com/freight/fedex-invoice-march.pdf",
    },
    {
      type: "url",
      name: "dhl-invoice-march.pdf",
      url: "https://example.com/freight/dhl-invoice-march.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "carrier_invoices",
        type: "ARRAY",
        description: "One entry per carrier invoice",
        fields: [
          {
            name: "carrier",
            type: "TEXT",
            description: "Carrier name",
          },
          {
            name: "invoice_number",
            type: "TEXT",
            description: "Invoice reference number",
          },
          {
            name: "invoice_date",
            type: "DATE",
            description: "Invoice date",
          },
          {
            name: "shipment_reference",
            type: "TEXT",
            description: "Shipment or tracking reference number",
          },
          {
            name: "service_type",
            type: "TEXT",
            description: "Service level, e.g. Express or Ground",
          },
          {
            name: "charges",
            type: "CURRENCY_AMOUNT",
            description: "Total charges for this shipment",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Billing currency",
          },
        ],
      },
    ],
  },
});

const invoices = extraction.carrier_invoices.value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate XLSX freight cost tracker from the extracted invoice data
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#B45309",
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
      name: "Freight Cost Tracker — March 2026",
      columns: [
        { name: "Carrier", width: 16 },
        { name: "Invoice #", width: 18 },
        { name: "Date", width: 12 },
        { name: "Shipment Ref", width: 20 },
        { name: "Service", width: 14 },
        { name: "Charges", width: 14 },
        { name: "Currency", width: 10 },
      ],
      rows: [
        ...invoices.map((invoice) => [
          {
            value: String(invoice.carrier.value),
          },
          {
            value: String(invoice.invoice_number.value),
          },
          {
            value: String(invoice.invoice_date.value),
            format: "date",
          },
          {
            value: String(invoice.shipment_reference.value),
          },
          {
            value: String(invoice.service_type.value),
          },
          {
            value: (invoice.charges.value as { amount: string }).amount,
            format: "currency",
          },
          {
            value: String(invoice.currency.value),
          },
        ]),
        [
          {
            value: "Total",
          },
          {
            value: null,
          },
          {
            value: null,
          },
          {
            value: null,
          },
          {
            value: null,
          },
          {
            value: null,
          },
          {
            value: null,
          },
        ],
      ],
      formulas: [
        {
          row: invoices.length + 1,
          col: 6,
          expression: `=SUM(F2:F${invoices.length + 1})`,
        },
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract carrier invoice data from all three PDFs
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "ups-invoice-march.pdf",
            "url": "https://example.com/freight/ups-invoice-march.pdf",
        },
        {
            "type": "url",
            "name": "fedex-invoice-march.pdf",
            "url": "https://example.com/freight/fedex-invoice-march.pdf",
        },
        {
            "type": "url",
            "name": "dhl-invoice-march.pdf",
            "url": "https://example.com/freight/dhl-invoice-march.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "carrier_invoices",
                "type": "ARRAY",
                "description": "One entry per carrier invoice",
                "fields": [
                    {"name": "carrier", "type": "TEXT", "description": "Carrier name"},
                    {"name": "invoice_number", "type": "TEXT", "description": "Invoice reference number"},
                    {"name": "invoice_date", "type": "DATE", "description": "Invoice date"},
                    {"name": "shipment_reference", "type": "TEXT", "description": "Shipment or tracking reference number"},
                    {"name": "service_type", "type": "TEXT", "description": "Service level, e.g. Express or Ground"},
                    {"name": "charges", "type": "CURRENCY_AMOUNT", "description": "Total charges for this shipment"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Billing currency"},
                ],
            }
        ]
    },
)

invoices = extraction["carrier_invoices"]["value"]

# Step 2: Generate XLSX freight cost tracker from the extracted invoice data
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "is_bold": True,
            "background_color": "#B45309",
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
            "name": "Freight Cost Tracker — March 2026",
            "columns": [
                {"name": "Carrier", "width": 16},
                {"name": "Invoice #", "width": 18},
                {"name": "Date", "width": 12},
                {"name": "Shipment Ref", "width": 20},
                {"name": "Service", "width": 14},
                {"name": "Charges", "width": 14},
                {"name": "Currency", "width": 10},
            ],
            "rows": [
                *[
                    [
                        {"value": invoice["carrier"]["value"]},
                        {"value": invoice["invoice_number"]["value"]},
                        {"value": invoice["invoice_date"]["value"], "format": "date"},
                        {"value": invoice["shipment_reference"]["value"]},
                        {"value": invoice["service_type"]["value"]},
                        {"value": invoice["charges"]["value"]["amount"], "format": "currency"},
                        {"value": invoice["currency"]["value"]},
                    ]
                    for invoice in invoices
                ],
                [
                    {"value": "Total"},
                    {"value": None},
                    {"value": None},
                    {"value": None},
                    {"value": None},
                    {"value": None},
                    {"value": None},
                ],
            ],
            "formulas": [
                {
                    "row": len(invoices) + 1,
                    "col": 6,
                    "expression": f"=SUM(F2:F{len(invoices) + 1})",
                }
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

	// Step 1: Extract carrier invoice data from all three PDFs
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("ups-invoice-march.pdf", "https://example.com/freight/ups-invoice-march.pdf"),
			il.NewFileFromURL("fedex-invoice-march.pdf", "https://example.com/freight/fedex-invoice-march.pdf"),
			il.NewFileFromURL("dhl-invoice-march.pdf", "https://example.com/freight/dhl-invoice-march.pdf"),
		},
		Schema: il.ExtractionSchema{
			"carrier_invoices": il.NewArrayFieldConfig(
				"carrier_invoices",
				"One entry per carrier invoice",
				[]il.FieldConfig{
					il.NewTextFieldConfig("carrier", "Carrier name"),
					il.NewTextFieldConfig("invoice_number", "Invoice reference number"),
					il.NewDateFieldConfig("invoice_date", "Invoice date"),
					il.NewTextFieldConfig("shipment_reference", "Shipment or tracking reference number"),
					il.NewTextFieldConfig("service_type", "Service level, e.g. Express or Ground"),
					il.NewCurrencyAmountFieldConfig("charges", "Total charges for this shipment"),
					il.NewCurrencyCodeFieldConfig("currency", "Billing currency"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	invoiceItems, _ := (*extraction)["carrier_invoices"].Value.([]interface{})

	rows := make([]il.SheetRow, 0, len(invoiceItems)+1)
	for _, item := range invoiceItems {
		invoice, _ := item.(map[string]interface{})
		getVal := func(key string) string {
			if field, ok := invoice[key].(map[string]interface{}); ok {
				return fmt.Sprintf("%v", field["value"])
			}
			return ""
		}
		chargesAmount := ""
		if field, ok := invoice["charges"].(map[string]interface{}); ok {
			if val, ok := field["value"].(map[string]interface{}); ok {
				chargesAmount = fmt.Sprintf("%v", val["amount"])
			}
		}
		rows = append(rows, il.SheetRow{
			{Value: getVal("carrier")},
			{Value: getVal("invoice_number")},
			{Value: getVal("invoice_date"), Format: "date"},
			{Value: getVal("shipment_reference")},
			{Value: getVal("service_type")},
			{Value: chargesAmount, Format: "currency"},
			{Value: getVal("currency")},
		})
	}
	rows = append(rows, il.SheetRow{
		{Value: "Total"},
		{Value: nil},
		{Value: nil},
		{Value: nil},
		{Value: nil},
		{Value: nil},
		{Value: nil},
	})

	// Step 2: Generate XLSX freight cost tracker from the extracted invoice data
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				FontFamily:      "Helvetica",
				FontSizeInPt:    11,
				IsBold:          true,
				BackgroundColor: "#B45309",
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
				Name: "Freight Cost Tracker — March 2026",
				Columns: []il.SheetColumn{
					{Name: "Carrier", Width: 16},
					{Name: "Invoice #", Width: 18},
					{Name: "Date", Width: 12},
					{Name: "Shipment Ref", Width: 20},
					{Name: "Service", Width: 14},
					{Name: "Charges", Width: 14},
					{Name: "Currency", Width: 10},
				},
				Rows: rows,
				Formulas: []il.Formula{
					{
						Row:        len(invoiceItems) + 1,
						Col:        6,
						Expression: fmt.Sprintf("=SUM(F2:F%d)", len(invoiceItems)+1),
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
  "name": "Extract Carrier Invoices to Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Carrier Invoices to Spreadsheet\n\nLogistics and freight operations teams receive PDF invoices from multiple carriers each month and need to reconcile all charges against shipment budgets. This pipeline extracts per-shipment charges from carrier invoices and writes them directly into a formatted XLSX cost tracker ready for finance review.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "97ca7a4d-891b-4ae3-b848-5baf12b8d4e2",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Carrier Invoice Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "69351e65-6d91-48e5-955f-6e217523f729",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Freight Cost Spreadsheet\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "8d3e90eb-4808-4637-8c59-6dbbf5597fe0",
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
      "id": "d4e5f6a7-b8c9-0123-4567-890abcdef012",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"carrier_invoices\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per carrier invoice\",\n    \"children\": [\n      {\n        \"name\": \"carrier\",\n        \"type\": \"TEXT\",\n        \"description\": \"Carrier name\"\n      },\n      {\n        \"name\": \"invoice_number\",\n        \"type\": \"TEXT\",\n        \"description\": \"Invoice reference number\"\n      },\n      {\n        \"name\": \"invoice_date\",\n        \"type\": \"DATE\",\n        \"description\": \"Invoice date\"\n      },\n      {\n        \"name\": \"shipment_reference\",\n        \"type\": \"TEXT\",\n        \"description\": \"Shipment or tracking reference number\"\n      },\n      {\n        \"name\": \"service_type\",\n        \"type\": \"TEXT\",\n        \"description\": \"Service level, e.g. Express or Ground\"\n      },\n      {\n        \"name\": \"charges\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Total charges for this shipment\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Billing currency\"\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/freight/ups-invoice-march.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/freight/fedex-invoice-march.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/freight/dhl-invoice-march.pdf"
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
      "id": "e5f6a7b8-c9d0-1234-5678-90abcdef0123",
      "name": "Extract Carrier Invoice Data",
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
        "sheetsJson": "[\n  {\n    \"name\": \"Freight Cost Tracker \u2014 March 2026\",\n    \"columns\": [\n      { \"name\": \"Carrier\", \"width\": 16 },\n      { \"name\": \"Invoice #\", \"width\": 18 },\n      { \"name\": \"Date\", \"width\": 12 },\n      { \"name\": \"Shipment Ref\", \"width\": 20 },\n      { \"name\": \"Service\", \"width\": 14 },\n      { \"name\": \"Charges\", \"width\": 14 },\n      { \"name\": \"Currency\", \"width\": 10 }\n    ],\n    \"rows\": \"{{ $json.carrier_invoices }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"is_bold\": true,\n    \"background_color\": \"#B45309\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"font_color\": \"#333333\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "f6a7b8c9-d0e1-2345-6789-0abcdef01234",
      "name": "Generate Freight Cost Spreadsheet",
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
            "node": "Extract Carrier Invoice Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Carrier Invoice Data": {
      "main": [
        [
          {
            "node": "Generate Freight Cost Spreadsheet",
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
Extract carrier invoice data from the files at [file URL 1], [file URL 2], and [file URL 3], then generate a freight cost spreadsheet. Use the extract_document tool with these fields:

- carrier_invoices (ARRAY): One entry per carrier invoice, each with:
  - carrier (TEXT): Carrier name
  - invoice_number (TEXT): Invoice reference number
  - invoice_date (DATE): Invoice date
  - shipment_reference (TEXT): Shipment or tracking reference number
  - service_type (TEXT): Service level, e.g. Express or Ground
  - charges (CURRENCY_AMOUNT): Total charges for this shipment
  - currency (CURRENCY_CODE): Billing currency

Then use the generate_sheet tool to create an XLSX freight cost tracker with the extracted rows.
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
