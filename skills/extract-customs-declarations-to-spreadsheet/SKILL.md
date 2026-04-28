---
name: extract-customs-declarations-to-spreadsheet
description: Extract HS codes, declared values, and duty amounts from customs declaration documents, then generate an XLSX import duty log.
---

# Extract Customs Declarations to Spreadsheet

Import operations and trade compliance teams receive customs declaration documents for every inbound shipment and need to log HS codes, declared values, and duty amounts for compliance reporting and cost reconciliation. This pipeline extracts the key fields from each declaration and writes them into a formatted XLSX duty tracker.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract customs declaration data from all three documents
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "declaration-cn-electronics.pdf",
        "url": "https://example.com/customs/declaration-cn-electronics.pdf"
      },
      {
        "type": "url",
        "name": "declaration-de-machinery.pdf",
        "url": "https://example.com/customs/declaration-de-machinery.pdf"
      },
      {
        "type": "url",
        "name": "declaration-vn-textiles.pdf",
        "url": "https://example.com/customs/declaration-vn-textiles.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "declarations",
          "type": "ARRAY",
          "description": "One entry per customs declaration document",
          "fields": [
            { "name": "declaration_number", "type": "TEXT", "description": "Customs declaration reference number" },
            { "name": "declaration_date", "type": "DATE", "description": "Date of customs declaration" },
            { "name": "country_of_origin", "type": "TEXT", "description": "Country where goods were manufactured" },
            { "name": "hs_code", "type": "TEXT", "description": "Harmonized System tariff code" },
            { "name": "goods_description", "type": "TEXT", "description": "Description of imported goods" },
            { "name": "declared_value", "type": "CURRENCY_AMOUNT", "description": "Customs declared value of goods" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Currency of declared value" },
            { "name": "duty_amount", "type": "CURRENCY_AMOUNT", "description": "Duty assessed on this declaration" }
          ]
        }
      ]
    }
  }')

# Step 2: Generate XLSX import duty log from the extracted declaration data
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
        "background_color": "#374151",
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
        "name": "Import Duty Log — Q1 2026",
        "columns": [
          { "name": "Declaration #", "width": 18 },
          { "name": "Date", "width": 12 },
          { "name": "Origin", "width": 14 },
          { "name": "HS Code", "width": 14 },
          { "name": "Description", "width": 30 },
          { "name": "Declared Value", "width": 16 },
          { "name": "Duty", "width": 14 },
          { "name": "Currency", "width": 10 }
        ],
        "rows": [
          [
            { "value": "IMP-2026-CN-00841" },
            { "value": "2026-01-14", "format": "date" },
            { "value": "China" },
            { "value": "8471.30" },
            { "value": "Laptop computers and portable data processing machines" },
            { "value": 48750.00, "format": "currency" },
            { "value": 2437.50, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "IMP-2026-DE-00392" },
            { "value": "2026-02-03", "format": "date" },
            { "value": "Germany" },
            { "value": "8457.10" },
            { "value": "Machining centres for working metal" },
            { "value": 124300.00, "format": "currency" },
            { "value": 0.00, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "IMP-2026-VN-01175" },
            { "value": "2026-03-07", "format": "date" },
            { "value": "Vietnam" },
            { "value": "6204.62" },
            { "value": "Women's trousers and shorts of cotton" },
            { "value": 18420.00, "format": "currency" },
            { "value": 2947.20, "format": "currency" },
            { "value": "USD" }
          ],
          [
            { "value": "Total" },
            { "value": null },
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
          },
          {
            "row": 4,
            "col": 7,
            "expression": "=SUM(G2:G4)"
          }
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract customs declaration data from all three documents
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "declaration-cn-electronics.pdf",
      url: "https://example.com/customs/declaration-cn-electronics.pdf",
    },
    {
      type: "url",
      name: "declaration-de-machinery.pdf",
      url: "https://example.com/customs/declaration-de-machinery.pdf",
    },
    {
      type: "url",
      name: "declaration-vn-textiles.pdf",
      url: "https://example.com/customs/declaration-vn-textiles.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "declarations",
        type: "ARRAY",
        description: "One entry per customs declaration document",
        fields: [
          {
            name: "declaration_number",
            type: "TEXT",
            description: "Customs declaration reference number",
          },
          {
            name: "declaration_date",
            type: "DATE",
            description: "Date of customs declaration",
          },
          {
            name: "country_of_origin",
            type: "TEXT",
            description: "Country where goods were manufactured",
          },
          {
            name: "hs_code",
            type: "TEXT",
            description: "Harmonized System tariff code",
          },
          {
            name: "goods_description",
            type: "TEXT",
            description: "Description of imported goods",
          },
          {
            name: "declared_value",
            type: "CURRENCY_AMOUNT",
            description: "Customs declared value of goods",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Currency of declared value",
          },
          {
            name: "duty_amount",
            type: "CURRENCY_AMOUNT",
            description: "Duty assessed on this declaration",
          },
        ],
      },
    ],
  },
});

const declarations = extraction.declarations.value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate XLSX import duty log from the extracted declaration data
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#374151",
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
      name: "Import Duty Log — Q1 2026",
      columns: [
        { name: "Declaration #", width: 18 },
        { name: "Date", width: 12 },
        { name: "Origin", width: 14 },
        { name: "HS Code", width: 14 },
        { name: "Description", width: 30 },
        { name: "Declared Value", width: 16 },
        { name: "Duty", width: 14 },
        { name: "Currency", width: 10 },
      ],
      rows: [
        ...declarations.map((declaration) => [
          { value: String(declaration.declaration_number.value) },
          { value: String(declaration.declaration_date.value), format: "date" },
          { value: String(declaration.country_of_origin.value) },
          { value: String(declaration.hs_code.value) },
          { value: String(declaration.goods_description.value) },
          {
            value: (declaration.declared_value.value as { amount: string })
              .amount,
            format: "currency",
          },
          {
            value: (declaration.duty_amount.value as { amount: string }).amount,
            format: "currency",
          },
          { value: String(declaration.currency.value) },
        ]),
        [
          { value: "Total" },
          { value: null },
          { value: null },
          { value: null },
          { value: null },
          { value: null },
          { value: null },
          { value: null },
        ],
      ],
      formulas: [
        {
          row: declarations.length + 1,
          col: 6,
          expression: `=SUM(F2:F${declarations.length + 1})`,
        },
        {
          row: declarations.length + 1,
          col: 7,
          expression: `=SUM(G2:G${declarations.length + 1})`,
        },
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract customs declaration data from all three documents
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "declaration-cn-electronics.pdf",
            "url": "https://example.com/customs/declaration-cn-electronics.pdf",
        },
        {
            "type": "url",
            "name": "declaration-de-machinery.pdf",
            "url": "https://example.com/customs/declaration-de-machinery.pdf",
        },
        {
            "type": "url",
            "name": "declaration-vn-textiles.pdf",
            "url": "https://example.com/customs/declaration-vn-textiles.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "declarations",
                "type": "ARRAY",
                "description": "One entry per customs declaration document",
                "fields": [
                    {"name": "declaration_number", "type": "TEXT", "description": "Customs declaration reference number"},
                    {"name": "declaration_date", "type": "DATE", "description": "Date of customs declaration"},
                    {"name": "country_of_origin", "type": "TEXT", "description": "Country where goods were manufactured"},
                    {"name": "hs_code", "type": "TEXT", "description": "Harmonized System tariff code"},
                    {"name": "goods_description", "type": "TEXT", "description": "Description of imported goods"},
                    {"name": "declared_value", "type": "CURRENCY_AMOUNT", "description": "Customs declared value of goods"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Currency of declared value"},
                    {"name": "duty_amount", "type": "CURRENCY_AMOUNT", "description": "Duty assessed on this declaration"},
                ],
            }
        ]
    },
)

declarations = extraction["declarations"]["value"]

# Step 2: Generate XLSX import duty log from the extracted declaration data
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "is_bold": True,
            "background_color": "#374151",
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
            "name": "Import Duty Log — Q1 2026",
            "columns": [
                {"name": "Declaration #", "width": 18},
                {"name": "Date", "width": 12},
                {"name": "Origin", "width": 14},
                {"name": "HS Code", "width": 14},
                {"name": "Description", "width": 30},
                {"name": "Declared Value", "width": 16},
                {"name": "Duty", "width": 14},
                {"name": "Currency", "width": 10},
            ],
            "rows": [
                *[
                    [
                        {"value": declaration["declaration_number"]["value"]},
                        {"value": declaration["declaration_date"]["value"], "format": "date"},
                        {"value": declaration["country_of_origin"]["value"]},
                        {"value": declaration["hs_code"]["value"]},
                        {"value": declaration["goods_description"]["value"]},
                        {"value": declaration["declared_value"]["value"]["amount"], "format": "currency"},
                        {"value": declaration["duty_amount"]["value"]["amount"], "format": "currency"},
                        {"value": declaration["currency"]["value"]},
                    ]
                    for declaration in declarations
                ],
                [
                    {"value": "Total"},
                    {"value": None},
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
                    "row": len(declarations) + 1,
                    "col": 6,
                    "expression": f"=SUM(F2:F{len(declarations) + 1})",
                },
                {
                    "row": len(declarations) + 1,
                    "col": 7,
                    "expression": f"=SUM(G2:G{len(declarations) + 1})",
                },
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

	// Step 1: Extract customs declaration data from all three documents
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("declaration-cn-electronics.pdf", "https://example.com/customs/declaration-cn-electronics.pdf"),
			il.NewFileFromURL("declaration-de-machinery.pdf", "https://example.com/customs/declaration-de-machinery.pdf"),
			il.NewFileFromURL("declaration-vn-textiles.pdf", "https://example.com/customs/declaration-vn-textiles.pdf"),
		},
		Schema: il.ExtractionSchema{
			"declarations": il.NewArrayFieldConfig(
				"declarations",
				"One entry per customs declaration document",
				[]il.FieldConfig{
					il.NewTextFieldConfig("declaration_number", "Customs declaration reference number"),
					il.NewDateFieldConfig("declaration_date", "Date of customs declaration"),
					il.NewTextFieldConfig("country_of_origin", "Country where goods were manufactured"),
					il.NewTextFieldConfig("hs_code", "Harmonized System tariff code"),
					il.NewTextFieldConfig("goods_description", "Description of imported goods"),
					il.NewCurrencyAmountFieldConfig("declared_value", "Customs declared value of goods"),
					il.NewCurrencyCodeFieldConfig("currency", "Currency of declared value"),
					il.NewCurrencyAmountFieldConfig("duty_amount", "Duty assessed on this declaration"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	declarationItems, _ := (*extraction)["declarations"].Value.([]interface{})

	rows := make([]il.SheetRow, 0, len(declarationItems)+1)
	for _, item := range declarationItems {
		declaration, _ := item.(map[string]interface{})
		getVal := func(key string) string {
			if field, ok := declaration[key].(map[string]interface{}); ok {
				return fmt.Sprintf("%v", field["value"])
			}
			return ""
		}
		getCurrencyAmount := func(key string) string {
			if field, ok := declaration[key].(map[string]interface{}); ok {
				if val, ok := field["value"].(map[string]interface{}); ok {
					return fmt.Sprintf("%v", val["amount"])
				}
			}
			return ""
		}
		rows = append(rows, il.SheetRow{
			{Value: getVal("declaration_number")},
			{Value: getVal("declaration_date"), Format: "date"},
			{Value: getVal("country_of_origin")},
			{Value: getVal("hs_code")},
			{Value: getVal("goods_description")},
			{Value: getCurrencyAmount("declared_value"), Format: "currency"},
			{Value: getCurrencyAmount("duty_amount"), Format: "currency"},
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
		{Value: nil},
	})

	// Step 2: Generate XLSX import duty log from the extracted declaration data
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				FontFamily:      "Helvetica",
				FontSizeInPt:    11,
				IsBold:          true,
				BackgroundColor: "#374151",
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
				Name: "Import Duty Log — Q1 2026",
				Columns: []il.SheetColumn{
					{Name: "Declaration #", Width: 18},
					{Name: "Date", Width: 12},
					{Name: "Origin", Width: 14},
					{Name: "HS Code", Width: 14},
					{Name: "Description", Width: 30},
					{Name: "Declared Value", Width: 16},
					{Name: "Duty", Width: 14},
					{Name: "Currency", Width: 10},
				},
				Rows: rows,
				Formulas: []il.Formula{
					{
						Row:        len(declarationItems) + 1,
						Col:        6,
						Expression: fmt.Sprintf("=SUM(F2:F%d)", len(declarationItems)+1),
					},
					{
						Row:        len(declarationItems) + 1,
						Col:        7,
						Expression: fmt.Sprintf("=SUM(G2:G%d)", len(declarationItems)+1),
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
  "name": "Extract Customs Declarations to Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Customs Declarations to Spreadsheet\n\nImport operations and trade compliance teams receive customs declaration documents for every inbound shipment and need to log HS codes, declared values, and duty amounts for compliance reporting and cost reconciliation. This pipeline extracts the key fields from each declaration and writes them into a formatted XLSX duty tracker.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "8719816b-8100-441b-ad4e-4171ae3b585d",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Declaration Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "883c0bca-0a24-4d06-ba03-8ee134c8a714",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Import Duty Log\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "11b08cf6-37c0-414c-b1db-9beed2c32d41",
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
      "id": "4d5e6f7a-8b9c-0123-def0-345678901234",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"declarations\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per customs declaration document\",\n    \"children\": [\n      {\n        \"name\": \"declaration_number\",\n        \"type\": \"TEXT\",\n        \"description\": \"Customs declaration reference number\"\n      },\n      {\n        \"name\": \"declaration_date\",\n        \"type\": \"DATE\",\n        \"description\": \"Date of customs declaration\"\n      },\n      {\n        \"name\": \"country_of_origin\",\n        \"type\": \"TEXT\",\n        \"description\": \"Country where goods were manufactured\"\n      },\n      {\n        \"name\": \"hs_code\",\n        \"type\": \"TEXT\",\n        \"description\": \"Harmonized System tariff code\"\n      },\n      {\n        \"name\": \"goods_description\",\n        \"type\": \"TEXT\",\n        \"description\": \"Description of imported goods\"\n      },\n      {\n        \"name\": \"declared_value\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Customs declared value of goods\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Currency of declared value\"\n      },\n      {\n        \"name\": \"duty_amount\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Duty assessed on this declaration\"\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/customs/declaration-cn-electronics.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/customs/declaration-de-machinery.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/customs/declaration-vn-textiles.pdf"
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
      "id": "5e6f7a8b-9c0d-1234-ef01-456789012345",
      "name": "Extract Declaration Data",
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
        "sheetsJson": "[\n  {\n    \"name\": \"Import Duty Log \u2014 Q1 2026\",\n    \"columns\": [\n      { \"name\": \"Declaration #\", \"width\": 18 },\n      { \"name\": \"Date\", \"width\": 12 },\n      { \"name\": \"Origin\", \"width\": 14 },\n      { \"name\": \"HS Code\", \"width\": 14 },\n      { \"name\": \"Description\", \"width\": 30 },\n      { \"name\": \"Declared Value\", \"width\": 16 },\n      { \"name\": \"Duty\", \"width\": 14 },\n      { \"name\": \"Currency\", \"width\": 10 }\n    ],\n    \"rows\": \"{{ $json.declarations }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"is_bold\": true,\n    \"background_color\": \"#374151\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"font_color\": \"#333333\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "6f7a8b9c-0d1e-2345-f012-567890123456",
      "name": "Generate Import Duty Log",
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
            "node": "Extract Declaration Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Declaration Data": {
      "main": [
        [
          {
            "node": "Generate Import Duty Log",
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
Extract customs declaration data from the files at [file URL 1], [file URL 2], and [file URL 3], then generate an import duty spreadsheet. Use the extract_document tool with these fields:

- declarations (ARRAY): One entry per customs declaration, each with:
  - declaration_number (TEXT): Customs declaration reference number
  - declaration_date (DATE): Date of customs declaration
  - country_of_origin (TEXT): Country where goods were manufactured
  - hs_code (TEXT): Harmonized System tariff code
  - goods_description (TEXT): Description of imported goods
  - declared_value (CURRENCY_AMOUNT): Customs declared value of goods
  - currency (CURRENCY_CODE): Currency of declared value
  - duty_amount (CURRENCY_AMOUNT): Duty assessed on this declaration

Then use the generate_sheet tool to create an XLSX import duty log with the extracted rows.
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
