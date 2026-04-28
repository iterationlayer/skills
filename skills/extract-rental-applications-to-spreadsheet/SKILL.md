---
name: extract-rental-applications-to-spreadsheet
description: Extract applicant details from rental application PDFs and generate a side-by-side XLSX for comparing income, employment, and move-in dates.
---

# Extract Rental Applications to Spreadsheet

Property managers and landlords use this recipe to compare rental applicants side by side without manually reading each PDF. Pass multiple application files, extract the key fields, and get a clean XLSX ready for shortlisting before scheduling viewings.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract data from all rental applications
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "application-chen.pdf",
        "url": "https://example.com/applications/application-chen.pdf"
      },
      {
        "type": "url",
        "name": "application-okafor.pdf",
        "url": "https://example.com/applications/application-okafor.pdf"
      },
      {
        "type": "url",
        "name": "application-petrov.pdf",
        "url": "https://example.com/applications/application-petrov.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "applications",
          "type": "ARRAY",
          "description": "One entry per rental application",
          "fields": [
            { "name": "applicant_name", "type": "TEXT", "description": "Full name of applicant" },
            { "name": "email", "type": "EMAIL", "description": "Applicant email address" },
            { "name": "monthly_income", "type": "CURRENCY_AMOUNT", "description": "Stated monthly income" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Income currency" },
            { "name": "employer", "type": "TEXT", "description": "Current employer name" },
            {
              "name": "employment_status",
              "type": "ENUM",
              "description": "Employment status",
              "values": ["Full-time", "Part-time", "Self-employed", "Unemployed", "Retired"]
            },
            { "name": "desired_move_in_date", "type": "DATE", "description": "Requested move-in date" },
            { "name": "has_pets", "type": "BOOLEAN", "description": "Whether the applicant has pets" }
          ]
          }
        }
      ]
    }
  }')

# Step 2: Generate the applicant comparison spreadsheet
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
        "background_color": "#4A5568",
        "font_color": "#FFFFFF"
      },
      "body": {
        "font_family": "Helvetica",
        "font_size_in_pt": 11,
        "font_color": "#1A1A1A"
      }
    },
    "sheets": [
      {
      "name": "Rental Applications — 14 Birchwood Lane",
      "columns": [
        { "name": "Applicant", "width": 22 },
        { "name": "Email", "width": 28 },
        { "name": "Employment", "width": 16 },
        { "name": "Employer", "width": 22 },
        { "name": "Monthly Income", "width": 16 },
        { "name": "Currency", "width": 10 },
        { "name": "Move-in Date", "width": 14 },
        { "name": "Pets", "width": 8 }
      ],
      "rows": [
        [
          { "value": "Wei Chen" },
          { "value": "wei.chen@gmail.com" },
          { "value": "Full-time" },
          { "value": "Meridian Software Ltd." },
          { "value": 6800, "format": "currency" },
          { "value": "USD" },
          { "value": "2026-05-01", "format": "date" },
          { "value": "No" }
        ],
        [
          { "value": "Amara Okafor" },
          { "value": "amara.okafor@consultio.co" },
          { "value": "Self-employed" },
          { "value": "Okafor Consulting" },
          { "value": 9200, "format": "currency" },
          { "value": "USD" },
          { "value": "2026-04-15", "format": "date" },
          { "value": "Yes" }
        ],
        [
          { "value": "Dmitri Petrov" },
          { "value": "d.petrov@healthcarenet.org" },
          { "value": "Full-time" },
          { "value": "Riverside General Hospital" },
          { "value": 5400, "format": "currency" },
          { "value": "USD" },
          { "value": "2026-05-15", "format": "date" },
          { "value": "No" }
        ]
      ]
    }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract data from all rental applications
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "application-chen.pdf",
      url: "https://example.com/applications/application-chen.pdf",
    },
    {
      type: "url",
      name: "application-okafor.pdf",
      url: "https://example.com/applications/application-okafor.pdf",
    },
    {
      type: "url",
      name: "application-petrov.pdf",
      url: "https://example.com/applications/application-petrov.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "applications",
        type: "ARRAY",
        description: "One entry per rental application",
        fields: [
          {
            name: "applicant_name",
            type: "TEXT",
            description: "Full name of applicant",
          },
          {
            name: "email",
            type: "EMAIL",
            description: "Applicant email address",
          },
          {
            name: "monthly_income",
            type: "CURRENCY_AMOUNT",
            description: "Stated monthly income",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Income currency",
          },
          {
            name: "employer",
            type: "TEXT",
            description: "Current employer name",
          },
          {
            name: "employment_status",
            type: "ENUM",
            description: "Employment status",
            values: [
              "Full-time",
              "Part-time",
              "Self-employed",
              "Unemployed",
              "Retired",
            ],
          },
          {
            name: "desired_move_in_date",
            type: "DATE",
            description: "Requested move-in date",
          },
          {
            name: "has_pets",
            type: "BOOLEAN",
            description: "Whether the applicant has pets",
          },
        ],
      },
    ],
  },
});

const applications = extraction["applications"].value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate the applicant comparison spreadsheet
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#4A5568",
      font_color: "#FFFFFF",
    },
    body: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      font_color: "#1A1A1A",
    },
  },
  sheets: [
    {
      name: "Rental Applications — 14 Birchwood Lane",
      columns: [
        { name: "Applicant", width: 22 },
        { name: "Email", width: 28 },
        { name: "Employment", width: 16 },
        { name: "Employer", width: 22 },
        { name: "Monthly Income", width: 16 },
        { name: "Currency", width: 10 },
        { name: "Move-in Date", width: 14 },
        { name: "Pets", width: 8 },
      ],
      rows: applications.map((application) => [
        { value: application["applicant_name"].value },
        { value: application["email"].value },
        { value: application["employment_status"].value },
        { value: application["employer"].value },
        {
          value: (application["monthly_income"].value as { amount: string })
            .amount,
          format: "currency",
        },
        { value: application["currency"].value },
        { value: application["desired_move_in_date"].value, format: "date" },
        { value: application["has_pets"].value ? "Yes" : "No" },
      ]),
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract data from all rental applications
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "application-chen.pdf",
            "url": "https://example.com/applications/application-chen.pdf",
        },
        {
            "type": "url",
            "name": "application-okafor.pdf",
            "url": "https://example.com/applications/application-okafor.pdf",
        },
        {
            "type": "url",
            "name": "application-petrov.pdf",
            "url": "https://example.com/applications/application-petrov.pdf",
        },
    ],
    schema={
        "fields": [
            {
                "name": "applications",
                "type": "ARRAY",
                "description": "One entry per rental application",
                "fields": [
                    {"name": "applicant_name", "type": "TEXT", "description": "Full name of applicant"},
                    {"name": "email", "type": "EMAIL", "description": "Applicant email address"},
                    {"name": "monthly_income", "type": "CURRENCY_AMOUNT", "description": "Stated monthly income"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Income currency"},
                    {"name": "employer", "type": "TEXT", "description": "Current employer name"},
                    {
                        "name": "employment_status",
                        "type": "ENUM",
                        "description": "Employment status",
                        "values": ["Full-time", "Part-time", "Self-employed", "Unemployed", "Retired"],
                    },
                    {"name": "desired_move_in_date", "type": "DATE", "description": "Requested move-in date"},
                    {"name": "has_pets", "type": "BOOLEAN", "description": "Whether the applicant has pets"},
                ]
                },
            }
        ]
    },
)

applications = extraction["applications"]["value"]

# Step 2: Generate the applicant comparison spreadsheet
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "is_bold": True,
            "background_color": "#4A5568",
            "font_color": "#FFFFFF",
        },
        "body": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "font_color": "#1A1A1A",
        },
    },
    sheets=[
        {
            "name": "Rental Applications — 14 Birchwood Lane",
            "columns": [
                {"name": "Applicant", "width": 22},
                {"name": "Email", "width": 28},
                {"name": "Employment", "width": 16},
                {"name": "Employer", "width": 22},
                {"name": "Monthly Income", "width": 16},
                {"name": "Currency", "width": 10},
                {"name": "Move-in Date", "width": 14},
                {"name": "Pets", "width": 8},
            ],
            "rows": [
                [
                    {"value": app["applicant_name"]["value"]},
                    {"value": app["email"]["value"]},
                    {"value": app["employment_status"]["value"]},
                    {"value": app["employer"]["value"]},
                    {"value": app["monthly_income"]["value"]["amount"], "format": "currency"},
                    {"value": app["currency"]["value"]},
                    {"value": app["desired_move_in_date"]["value"], "format": "date"},
                    {"value": "Yes" if app["has_pets"]["value"] else "No"},
                ]
                for app in applications
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

	// Step 1: Extract data from all rental applications
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL(
				"application-chen.pdf",
				"https://example.com/applications/application-chen.pdf",
			),
			il.NewFileFromURL(
				"application-okafor.pdf",
				"https://example.com/applications/application-okafor.pdf",
			),
			il.NewFileFromURL(
				"application-petrov.pdf",
				"https://example.com/applications/application-petrov.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"applications": il.NewArrayFieldConfig(
				"applications",
				"One entry per rental application",
				[]il.FieldConfig{
					il.NewTextFieldConfig("applicant_name", "Full name of applicant"),
					il.NewEmailFieldConfig("email", "Applicant email address"),
					il.NewCurrencyAmountFieldConfig("monthly_income", "Stated monthly income"),
					il.NewCurrencyCodeFieldConfig("currency", "Income currency"),
					il.NewTextFieldConfig("employer", "Current employer name"),
					il.NewEnumFieldConfig("employment_status", "Employment status", []string{
						"Full-time", "Part-time", "Self-employed", "Unemployed", "Retired",
					}),
					il.NewDateFieldConfig("desired_move_in_date", "Requested move-in date"),
					il.NewBooleanFieldConfig("has_pets", "Whether the applicant has pets"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	rawApplications, _ := (*extraction)["applications"].Value.([]interface{})

	sheetRows := make([]il.SheetRow, 0, len(rawApplications))
	for _, rawApplication := range rawApplications {
		application, _ := rawApplication.(map[string]interface{})
		getSV := func(key string) string {
			if field, ok := application[key].(map[string]interface{}); ok {
				return fmt.Sprintf("%v", field["value"])
			}
			return ""
		}
		monthlyIncome := ""
		if field, ok := application["monthly_income"].(map[string]interface{}); ok {
			if amountMap, ok := field["value"].(map[string]interface{}); ok {
				monthlyIncome = fmt.Sprintf("%v", amountMap["amount"])
			}
		}
		hasPets := "No"
		if field, ok := application["has_pets"].(map[string]interface{}); ok {
			if field["value"] == true {
				hasPets = "Yes"
			}
		}
		sheetRows = append(sheetRows, il.SheetRow{
			{Value: getSV("applicant_name")},
			{Value: getSV("email")},
			{Value: getSV("employment_status")},
			{Value: getSV("employer")},
			{Value: monthlyIncome, Format: "currency"},
			{Value: getSV("currency")},
			{Value: getSV("desired_move_in_date"), Format: "date"},
			{Value: hasPets},
		})
	}

	// Step 2: Generate the applicant comparison spreadsheet
	result, err := client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				FontFamily:      "Helvetica",
				FontSizeInPt:    11,
				IsBold:          true,
				BackgroundColor: "#4A5568",
				FontColor:       "#FFFFFF",
			},
			Body: &il.CellStyle{
				FontFamily:   "Helvetica",
				FontSizeInPt: 11,
				FontColor:    "#1A1A1A",
			},
		},
		Sheets: []il.Sheet{
			{
				Name: "Rental Applications — 14 Birchwood Lane",
				Columns: []il.SheetColumn{
					{Name: "Applicant", Width: 22},
					{Name: "Email", Width: 28},
					{Name: "Employment", Width: 16},
					{Name: "Employer", Width: 22},
					{Name: "Monthly Income", Width: 16},
					{Name: "Currency", Width: 10},
					{Name: "Move-in Date", Width: 14},
					{Name: "Pets", Width: 8},
				},
				Rows: sheetRows,
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
  "name": "Extract Rental Applications to Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Rental Applications to Spreadsheet\n\nProperty managers and landlords use this recipe to compare rental applicants side by side without manually reading each PDF. Pass multiple application files, extract the key fields, and get a clean XLSX ready for shortlisting before scheduling viewings.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "684d20cc-481f-4249-bd45-c3736d02ef0a",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Application Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "986f423e-e74a-4c0f-8567-3342364af44d",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Comparison Spreadsheet\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "3e6217dd-3fa2-402b-9dc2-c47970732ce7",
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
      "id": "a6b7c8d9-e0f1-2345-6a01-345678901234",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"applications\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per rental application\",\n    \"children\": [\n      {\n        \"name\": \"applicant_name\",\n        \"type\": \"TEXT\",\n        \"description\": \"Full name of applicant\"\n      },\n      {\n        \"name\": \"email\",\n        \"type\": \"EMAIL\",\n        \"description\": \"Applicant email address\"\n      },\n      {\n        \"name\": \"monthly_income\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Stated monthly income\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Income currency\"\n      },\n      {\n        \"name\": \"employer\",\n        \"type\": \"TEXT\",\n        \"description\": \"Current employer name\"\n      },\n      {\n        \"name\": \"employment_status\",\n        \"type\": \"ENUM\",\n        \"description\": \"Employment status\",\n        \"values\": [\"Full-time\", \"Part-time\", \"Self-employed\", \"Unemployed\", \"Retired\"]\n      },\n      {\n        \"name\": \"desired_move_in_date\",\n        \"type\": \"DATE\",\n        \"description\": \"Requested move-in date\"\n      },\n      {\n        \"name\": \"has_pets\",\n        \"type\": \"BOOLEAN\",\n        \"description\": \"Whether the applicant has pets\"\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/applications/application-chen.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/applications/application-okafor.pdf"
            },
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/applications/application-petrov.pdf"
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
      "id": "b7c8d9e0-f1a2-3456-7b12-456789012345",
      "name": "Extract Application Data",
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
        "sheetsJson": "[\n  {\n    \"name\": \"Rental Applications \u2014 14 Birchwood Lane\",\n    \"columns\": [\n      { \"name\": \"Applicant\", \"width\": 22 },\n      { \"name\": \"Email\", \"width\": 28 },\n      { \"name\": \"Employment\", \"width\": 16 },\n      { \"name\": \"Employer\", \"width\": 22 },\n      { \"name\": \"Monthly Income\", \"width\": 16 },\n      { \"name\": \"Currency\", \"width\": 10 },\n      { \"name\": \"Move-in Date\", \"width\": 14 },\n      { \"name\": \"Pets\", \"width\": 8 }\n    ],\n    \"rows\": \"{{ $json.applications }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"is_bold\": true,\n    \"background_color\": \"#4A5568\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"font_color\": \"#1A1A1A\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "c8d9e0f1-a2b3-4567-8c23-567890123456",
      "name": "Generate Comparison Spreadsheet",
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
            "node": "Extract Application Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Application Data": {
      "main": [
        [
          {
            "node": "Generate Comparison Spreadsheet",
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
Extract rental application data from the files at [file URL 1], [file URL 2], and [file URL 3], then generate an applicant comparison spreadsheet. Use the extract_document tool with these fields:

- applications (ARRAY): One entry per rental application, each with:
  - applicant_name (TEXT): Full name of applicant
  - email (EMAIL): Applicant email address
  - monthly_income (CURRENCY_AMOUNT): Stated monthly income
  - currency (CURRENCY_CODE): Income currency
  - employer (TEXT): Current employer name
  - employment_status (ENUM): Employment status (Full-time, Part-time, Self-employed, Unemployed, Retired)
  - desired_move_in_date (DATE): Requested move-in date
  - has_pets (BOOLEAN): Whether the applicant has pets

Then use the generate_sheet tool to create an XLSX applicant comparison spreadsheet with the extracted rows.
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
