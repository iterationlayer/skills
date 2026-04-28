---
name: extract-contracts-to-register
description: Extract key terms from multiple signed contracts and build a structured XLSX contract register tracking parties, dates, value, and renewal terms.
---

# Extract Contracts to a Register

Legal operations teams and in-house counsel use this pipeline to turn a batch of signed PDFs into a maintained contract register — ready for compliance reviews, renewals tracking, and audits — without manual data entry.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract key terms from all three contracts
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "msa-cloudpeak-solutions.pdf",
        "url": "https://example.com/contracts/msa-cloudpeak-solutions.pdf"
      },
      {
        "type": "url",
        "name": "nda-vertex-analytics.pdf",
        "url": "https://example.com/contracts/nda-vertex-analytics.pdf"
      },
      {
        "type": "url",
        "name": "sow-ironbridge-consulting.pdf",
        "url": "https://example.com/contracts/sow-ironbridge-consulting.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "contracts",
          "type": "ARRAY",
          "description": "One entry per contract file",
          "fields": [
            { "name": "contract_title", "type": "TEXT", "description": "Title or subject of the contract" },
            { "name": "counterparty", "type": "TEXT", "description": "Name of the other contracting party" },
            { "name": "effective_date", "type": "DATE", "description": "Date the contract becomes effective" },
            { "name": "expiry_date", "type": "DATE", "description": "Date the contract expires or ends" },
            { "name": "contract_value", "type": "CURRENCY_AMOUNT", "description": "Total or annual contract value if stated" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Currency of contract value" },
            { "name": "auto_renewal", "type": "BOOLEAN", "description": "Whether the contract auto-renews" },
            { "name": "governing_law", "type": "TEXT", "description": "Jurisdiction or governing law" }
          ]
        }
      ]
    }
  }')

# Step 2: Generate the contract register spreadsheet from extracted values
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
        "name": "Contract Register",
        "columns": [
          { "name": "Title", "width": 28 },
          { "name": "Counterparty", "width": 24 },
          { "name": "Effective Date", "width": 14 },
          { "name": "Expiry Date", "width": 14 },
          { "name": "Value", "width": 14 },
          { "name": "Currency", "width": 10 },
          { "name": "Auto-Renewal", "width": 14 },
          { "name": "Governing Law", "width": 18 }
        ],
        "rows": [
          [
            { "value": "Master Services Agreement" },
            { "value": "CloudPeak Solutions Inc." },
            { "value": "2026-01-01", "format": "date" },
            { "value": "2027-01-01", "format": "date" },
            { "value": 48000, "format": "currency" },
            { "value": "USD" },
            { "value": "Yes" },
            { "value": "Delaware" }
          ],
          [
            { "value": "Non-Disclosure Agreement" },
            { "value": "Vertex Analytics Ltd." },
            { "value": "2026-02-15", "format": "date" },
            { "value": "2028-02-15", "format": "date" },
            { "value": null },
            { "value": null },
            { "value": "No" },
            { "value": "California" }
          ],
          [
            { "value": "Statement of Work — Phase 1" },
            { "value": "Ironbridge Consulting LLC" },
            { "value": "2026-03-01", "format": "date" },
            { "value": "2026-09-30", "format": "date" },
            { "value": 22500, "format": "currency" },
            { "value": "USD" },
            { "value": "No" },
            { "value": "New York" }
          ]
        ]
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract key terms from all three contracts
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "msa-cloudpeak-solutions.pdf",
      url: "https://example.com/contracts/msa-cloudpeak-solutions.pdf",
    },
    {
      type: "url",
      name: "nda-vertex-analytics.pdf",
      url: "https://example.com/contracts/nda-vertex-analytics.pdf",
    },
    {
      type: "url",
      name: "sow-ironbridge-consulting.pdf",
      url: "https://example.com/contracts/sow-ironbridge-consulting.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "contracts",
        type: "ARRAY",
        description: "One entry per contract file",
        fields: [
          {
            name: "contract_title",
            type: "TEXT",
            description: "Title or subject of the contract",
          },
          {
            name: "counterparty",
            type: "TEXT",
            description: "Name of the other contracting party",
          },
          {
            name: "effective_date",
            type: "DATE",
            description: "Date the contract becomes effective",
          },
          {
            name: "expiry_date",
            type: "DATE",
            description: "Date the contract expires or ends",
          },
          {
            name: "contract_value",
            type: "CURRENCY_AMOUNT",
            description: "Total or annual contract value if stated",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Currency of contract value",
          },
          {
            name: "auto_renewal",
            type: "BOOLEAN",
            description: "Whether the contract auto-renews",
          },
          {
            name: "governing_law",
            type: "TEXT",
            description: "Jurisdiction or governing law",
          },
        ],
      },
    ],
  },
});

const contracts = extraction["contracts"].value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate the contract register spreadsheet from extracted values
const register = await client.generateSheet({
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
      name: "Contract Register",
      columns: [
        { name: "Title", width: 28 },
        { name: "Counterparty", width: 24 },
        { name: "Effective Date", width: 14 },
        { name: "Expiry Date", width: 14 },
        { name: "Value", width: 14 },
        { name: "Currency", width: 10 },
        { name: "Auto-Renewal", width: 14 },
        { name: "Governing Law", width: 18 },
      ],
      rows: contracts.map((contract) => [
        { value: String(contract["contract_title"].value) },
        { value: String(contract["counterparty"].value) },
        { value: String(contract["effective_date"].value), format: "date" },
        { value: String(contract["expiry_date"].value), format: "date" },
        { value: contract["contract_value"].value ?? null },
        { value: contract["currency"].value ?? null },
        { value: contract["auto_renewal"].value ? "Yes" : "No" },
        { value: String(contract["governing_law"].value) },
      ]),
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract key terms from all three contracts
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "msa-cloudpeak-solutions.pdf",
            "url": "https://example.com/contracts/msa-cloudpeak-solutions.pdf",
        },
        {
            "type": "url",
            "name": "nda-vertex-analytics.pdf",
            "url": "https://example.com/contracts/nda-vertex-analytics.pdf",
        },
        {
            "type": "url",
            "name": "sow-ironbridge-consulting.pdf",
            "url": "https://example.com/contracts/sow-ironbridge-consulting.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "contracts",
                "type": "ARRAY",
                "description": "One entry per contract file",
                "fields": [
                    {"name": "contract_title", "type": "TEXT", "description": "Title or subject of the contract"},
                    {"name": "counterparty", "type": "TEXT", "description": "Name of the other contracting party"},
                    {"name": "effective_date", "type": "DATE", "description": "Date the contract becomes effective"},
                    {"name": "expiry_date", "type": "DATE", "description": "Date the contract expires or ends"},
                    {"name": "contract_value", "type": "CURRENCY_AMOUNT", "description": "Total or annual contract value if stated"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Currency of contract value"},
                    {"name": "auto_renewal", "type": "BOOLEAN", "description": "Whether the contract auto-renews"},
                    {"name": "governing_law", "type": "TEXT", "description": "Jurisdiction or governing law"},
                ],
            }
        ]
    },
)

contracts = extraction["contracts"]["value"]

# Step 2: Generate the contract register spreadsheet from extracted values
register = client.generate_sheet(
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
            "name": "Contract Register",
            "columns": [
                {"name": "Title", "width": 28},
                {"name": "Counterparty", "width": 24},
                {"name": "Effective Date", "width": 14},
                {"name": "Expiry Date", "width": 14},
                {"name": "Value", "width": 14},
                {"name": "Currency", "width": 10},
                {"name": "Auto-Renewal", "width": 14},
                {"name": "Governing Law", "width": 18},
            ],
            "rows": [
                [
                    {"value": contract["contract_title"]["value"]},
                    {"value": contract["counterparty"]["value"]},
                    {"value": contract["effective_date"]["value"], "format": "date"},
                    {"value": contract["expiry_date"]["value"], "format": "date"},
                    {"value": contract["contract_value"]["value"]},
                    {"value": contract["currency"]["value"]},
                    {"value": "Yes" if contract["auto_renewal"]["value"] else "No"},
                    {"value": contract["governing_law"]["value"]},
                ]
                for contract in contracts
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

	// Step 1: Extract key terms from all three contracts
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("msa-cloudpeak-solutions.pdf", "https://example.com/contracts/msa-cloudpeak-solutions.pdf"),
			il.NewFileFromURL("nda-vertex-analytics.pdf", "https://example.com/contracts/nda-vertex-analytics.pdf"),
			il.NewFileFromURL("sow-ironbridge-consulting.pdf", "https://example.com/contracts/sow-ironbridge-consulting.pdf"),
		},
		Schema: il.ExtractionSchema{
			"contracts": il.NewArrayFieldConfig(
				"contracts",
				"One entry per contract file",
				[]il.FieldConfig{
					il.NewTextFieldConfig("contract_title", "Title or subject of the contract"),
					il.NewTextFieldConfig("counterparty", "Name of the other contracting party"),
					il.NewDateFieldConfig("effective_date", "Date the contract becomes effective"),
					il.NewDateFieldConfig("expiry_date", "Date the contract expires or ends"),
					il.NewCurrencyAmountFieldConfig("contract_value", "Total or annual contract value if stated"),
					il.NewCurrencyCodeFieldConfig("currency", "Currency of contract value"),
					il.NewBooleanFieldConfig("auto_renewal", "Whether the contract auto-renews"),
					il.NewTextFieldConfig("governing_law", "Jurisdiction or governing law"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	rawContracts, _ := (*extraction)["contracts"].Value.([]interface{})

	sheetRows := make([]il.SheetRow, 0, len(rawContracts))
	for _, rawContract := range rawContracts {
		contract, _ := rawContract.(map[string]interface{})
		getSV := func(key string) string {
			if field, ok := contract[key].(map[string]interface{}); ok {
				return fmt.Sprintf("%v", field["value"])
			}
			return ""
		}
		contractValue := ""
		if field, ok := contract["contract_value"].(map[string]interface{}); ok {
			if amountMap, ok := field["value"].(map[string]interface{}); ok {
				contractValue = fmt.Sprintf("%v", amountMap["amount"])
			}
		}
		autoRenewal := "No"
		if field, ok := contract["auto_renewal"].(map[string]interface{}); ok {
			if boolVal, ok := field["value"].(bool); ok && boolVal {
				autoRenewal = "Yes"
			}
		}
		sheetRows = append(sheetRows, il.SheetRow{
			{Value: getSV("contract_title")},
			{Value: getSV("counterparty")},
			{Value: getSV("effective_date"), Format: "date"},
			{Value: getSV("expiry_date"), Format: "date"},
			{Value: contractValue, Format: "currency"},
			{Value: getSV("currency")},
			{Value: autoRenewal},
			{Value: getSV("governing_law")},
		})
	}

	// Step 2: Generate the contract register spreadsheet from extracted values
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
				Name: "Contract Register",
				Columns: []il.SheetColumn{
					{Name: "Title", Width: 28},
					{Name: "Counterparty", Width: 24},
					{Name: "Effective Date", Width: 14},
					{Name: "Expiry Date", Width: 14},
					{Name: "Value", Width: 14},
					{Name: "Currency", Width: 10},
					{Name: "Auto-Renewal", Width: 14},
					{Name: "Governing Law", Width: 18},
				},
				Rows: sheetRows,
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
  "name": "Extract Contracts to a Register",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Contracts to a Register\n\nLegal operations teams and in-house counsel use this pipeline to turn a batch of signed PDFs into a maintained contract register \u2014 ready for compliance reviews, renewals tracking, and audits \u2014 without manual data entry.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "817d2c3b-1b09-4d93-993f-6b466fdd9b89",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Contract Terms\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "91efeebc-ac6d-459d-b38e-891c552b4cb4",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Contract Register\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "c0707b92-a66d-4779-9446-da458c28772f",
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
      "id": "1a2b3c4d-5e6f-7890-abcd-ef0123456789",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"contracts\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per contract file\",\n    \"children\": [\n      {\n        \"name\": \"contract_title\",\n        \"type\": \"TEXT\",\n        \"description\": \"Title or subject of the contract\"\n      },\n      {\n        \"name\": \"counterparty\",\n        \"type\": \"TEXT\",\n        \"description\": \"Name of the other contracting party\"\n      },\n      {\n        \"name\": \"effective_date\",\n        \"type\": \"DATE\",\n        \"description\": \"Date the contract becomes effective\"\n      },\n      {\n        \"name\": \"expiry_date\",\n        \"type\": \"DATE\",\n        \"description\": \"Date the contract expires or ends\"\n      },\n      {\n        \"name\": \"contract_value\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Total or annual contract value if stated\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Currency of contract value\"\n      },\n      {\n        \"name\": \"auto_renewal\",\n        \"type\": \"BOOLEAN\",\n        \"description\": \"Whether the contract auto-renews\"\n      },\n      {\n        \"name\": \"governing_law\",\n        \"type\": \"TEXT\",\n        \"description\": \"Jurisdiction or governing law\"\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/contracts/msa-cloudpeak-solutions.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/contracts/nda-vertex-analytics.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/contracts/sow-ironbridge-consulting.pdf"
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
      "id": "2b3c4d5e-6f7a-8901-bcde-f12345678901",
      "name": "Extract Contract Terms",
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
        "sheetsJson": "[\n  {\n    \"name\": \"Contract Register\",\n    \"columns\": [\n      { \"name\": \"Title\", \"width\": 28 },\n      { \"name\": \"Counterparty\", \"width\": 24 },\n      { \"name\": \"Effective Date\", \"width\": 14 },\n      { \"name\": \"Expiry Date\", \"width\": 14 },\n      { \"name\": \"Value\", \"width\": 14 },\n      { \"name\": \"Currency\", \"width\": 10 },\n      { \"name\": \"Auto-Renewal\", \"width\": 14 },\n      { \"name\": \"Governing Law\", \"width\": 18 }\n    ],\n    \"rows\": \"{{ $json.contracts }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"is_bold\": true,\n    \"background_color\": \"#1E3A5F\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_color\": \"#333333\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "3c4d5e6f-7a8b-9012-cdef-234567890123",
      "name": "Generate Contract Register",
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
            "node": "Extract Contract Terms",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Contract Terms": {
      "main": [
        [
          {
            "node": "Generate Contract Register",
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
Extract key terms from the contracts at [file URL 1], [file URL 2], and [file URL 3], then generate a contract register spreadsheet. Use the extract_document tool with these fields:

- contracts (ARRAY): One entry per contract file, each with:
  - contract_title (TEXT): Title or subject of the contract
  - counterparty (TEXT): Name of the other contracting party
  - effective_date (DATE): Date the contract becomes effective
  - expiry_date (DATE): Date the contract expires or ends
  - contract_value (CURRENCY_AMOUNT): Total or annual contract value if stated
  - currency (CURRENCY_CODE): Currency of contract value
  - auto_renewal (BOOLEAN): Whether the contract auto-renews
  - governing_law (TEXT): Jurisdiction or governing law

Then use the generate_sheet tool to create an XLSX contract register with the extracted rows.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "UEsDBBQAAAAI...",
    "mime_type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
