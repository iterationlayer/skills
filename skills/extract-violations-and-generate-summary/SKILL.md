---
name: extract-violations-and-generate-summary
description: Extract traffic violation data from fine notices, then generate a spreadsheet summarizing all violations for fleet management.
---

# Extract Violations and Generate a Fine Summary

Fleet management agencies use this pipeline to process stacks of traffic fine notices — extracting violation details and generating a consolidated spreadsheet for client review, accounting, and driver accountability.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract violation data from a fine notice
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "fine-notice-TRF-2026-4471.pdf",
        "url": "https://example.com/fines/fine-notice-TRF-2026-4471.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "violation_number",
          "type": "TEXT",
          "description": "Violation or ticket reference number"
        },
        {
          "name": "violation_date",
          "type": "DATE",
          "description": "Date the violation occurred"
        },
        {
          "name": "vehicle_plate",
          "type": "TEXT",
          "description": "License plate number"
        },
        {
          "name": "driver_name",
          "type": "TEXT",
          "description": "Name of the driver if listed"
        },
        {
          "name": "violation_type",
          "type": "TEXT",
          "description": "Type of violation, e.g. speeding, red light"
        },
        {
          "name": "location",
          "type": "TEXT",
          "description": "Location where the violation occurred"
        },
        {
          "name": "fine_amount",
          "type": "CURRENCY_AMOUNT",
          "description": "Fine amount"
        },
        {
          "name": "due_date",
          "type": "DATE",
          "description": "Payment due date"
        },
        {
          "name": "points",
          "type": "INTEGER",
          "description": "Penalty points assigned"
        }
      ]
    }
  }')

# Step 2: Generate a summary spreadsheet
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "sheets": [
      {
        "name": "Violation Summary",
        "columns": [
          {
            "name": "Violation #",
            "width": 20
          },
          {
            "name": "Date",
            "width": 14
          },
          {
            "name": "Plate",
            "width": 14
          },
          {
            "name": "Driver",
            "width": 22
          },
          {
            "name": "Type",
            "width": 18
          },
          {
            "name": "Location",
            "width": 30
          },
          {
            "name": "Fine",
            "width": 14
          },
          {
            "name": "Due Date",
            "width": 14
          },
          {
            "name": "Points",
            "width": 10
          }
        ],
        "rows": [
          [
            {
              "value": "TRF-2026-4471"
            },
            {
              "value": "2026-03-12",
              "format": "date"
            },
            {
              "value": "FL-8823-KM"
            },
            {
              "value": "Marco Vitali"
            },
            {
              "value": "Speeding (22 km/h over)"
            },
            {
              "value": "A1 Motorway, km 142.3, Lazio"
            },
            {
              "value": 185.00,
              "format": "currency",
              "currency_code": "EUR"
            },
            {
              "value": "2026-04-11",
              "format": "date"
            },
            {
              "value": 3,
              "format": "number"
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

// Step 1: Extract violation data from a fine notice
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "fine-notice-TRF-2026-4471.pdf",
      url: "https://example.com/fines/fine-notice-TRF-2026-4471.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "violation_number", type: "TEXT", description: "Violation or ticket reference number" },
      { name: "violation_date", type: "DATE", description: "Date the violation occurred" },
      { name: "vehicle_plate", type: "TEXT", description: "License plate number" },
      { name: "driver_name", type: "TEXT", description: "Name of the driver if listed" },
      { name: "violation_type", type: "TEXT", description: "Type of violation, e.g. speeding, red light" },
      { name: "location", type: "TEXT", description: "Location where the violation occurred" },
      { name: "fine_amount", type: "CURRENCY_AMOUNT", description: "Fine amount" },
      { name: "due_date", type: "DATE", description: "Payment due date" },
      { name: "points", type: "INTEGER", description: "Penalty points assigned" },
    ],
  },
});

const violationNumber = String(extraction["violation_number"].value);
const violationDate = String(extraction["violation_date"].value);
const vehiclePlate = String(extraction["vehicle_plate"].value);
const driverName = String(extraction["driver_name"].value);
const violationType = String(extraction["violation_type"].value);
const location = String(extraction["location"].value);
const fineAmount = extraction["fine_amount"].value as { amount: string; currency: string };
const dueDate = String(extraction["due_date"].value);
const points = Number(extraction["points"].value);

// Step 2: Generate a summary spreadsheet
const sheet = await client.generateSheet({
  format: "xlsx",
  sheets: [
    {
      name: "Violation Summary",
      columns: [
        { name: "Violation #", width: 20 },
        { name: "Date", width: 14 },
        { name: "Plate", width: 14 },
        { name: "Driver", width: 22 },
        { name: "Type", width: 18 },
        { name: "Location", width: 30 },
        { name: "Fine", width: 14 },
        { name: "Due Date", width: 14 },
        { name: "Points", width: 10 },
      ],
      rows: [
        [
          { value: violationNumber },
          { value: violationDate, format: "date" },
          { value: vehiclePlate },
          { value: driverName },
          { value: violationType },
          { value: location },
          { value: parseFloat(fineAmount.amount), format: "currency", currency_code: fineAmount.currency },
          { value: dueDate, format: "date" },
          { value: points, format: "number" },
        ],
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract violation data from a fine notice
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "fine-notice-TRF-2026-4471.pdf",
            "url": "https://example.com/fines/fine-notice-TRF-2026-4471.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "violation_number", "type": "TEXT", "description": "Violation or ticket reference number"},
            {"name": "violation_date", "type": "DATE", "description": "Date the violation occurred"},
            {"name": "vehicle_plate", "type": "TEXT", "description": "License plate number"},
            {"name": "driver_name", "type": "TEXT", "description": "Name of the driver if listed"},
            {"name": "violation_type", "type": "TEXT", "description": "Type of violation, e.g. speeding, red light"},
            {"name": "location", "type": "TEXT", "description": "Location where the violation occurred"},
            {"name": "fine_amount", "type": "CURRENCY_AMOUNT", "description": "Fine amount"},
            {"name": "due_date", "type": "DATE", "description": "Payment due date"},
            {"name": "points", "type": "INTEGER", "description": "Penalty points assigned"},
        ]
    },
)

violation_number = extraction["violation_number"]["value"]
violation_date = extraction["violation_date"]["value"]
vehicle_plate = extraction["vehicle_plate"]["value"]
driver_name = extraction["driver_name"]["value"]
violation_type = extraction["violation_type"]["value"]
location = extraction["location"]["value"]
fine_amount = extraction["fine_amount"]["value"]
due_date = extraction["due_date"]["value"]
points = extraction["points"]["value"]

# Step 2: Generate a summary spreadsheet
sheet = client.generate_sheet(
    format="xlsx",
    sheets=[
        {
            "name": "Violation Summary",
            "columns": [
                {"name": "Violation #", "width": 20},
                {"name": "Date", "width": 14},
                {"name": "Plate", "width": 14},
                {"name": "Driver", "width": 22},
                {"name": "Type", "width": 18},
                {"name": "Location", "width": 30},
                {"name": "Fine", "width": 14},
                {"name": "Due Date", "width": 14},
                {"name": "Points", "width": 10},
            ],
            "rows": [
                [
                    {"value": violation_number},
                    {"value": violation_date, "format": "date"},
                    {"value": vehicle_plate},
                    {"value": driver_name},
                    {"value": violation_type},
                    {"value": location},
                    {"value": fine_amount["amount"], "format": "currency", "currency_code": fine_amount["currency"]},
                    {"value": due_date, "format": "date"},
                    {"value": points, "format": "number"},
                ]
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

	// Step 1: Extract violation data from a fine notice
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("fine-notice-TRF-2026-4471.pdf", "https://example.com/fines/fine-notice-TRF-2026-4471.pdf"),
		},
		Schema: il.ExtractionSchema{
			"violation_number": il.NewTextFieldConfig("violation_number", "Violation or ticket reference number"),
			"violation_date":   il.NewDateFieldConfig("violation_date", "Date the violation occurred"),
			"vehicle_plate":    il.NewTextFieldConfig("vehicle_plate", "License plate number"),
			"driver_name":      il.NewTextFieldConfig("driver_name", "Name of the driver if listed"),
			"violation_type":   il.NewTextFieldConfig("violation_type", "Type of violation, e.g. speeding, red light"),
			"location":         il.NewTextFieldConfig("location", "Location where the violation occurred"),
			"fine_amount":      il.NewCurrencyAmountFieldConfig("fine_amount", "Fine amount"),
			"due_date":         il.NewDateFieldConfig("due_date", "Payment due date"),
			"points":           il.NewIntegerFieldConfig("points", "Penalty points assigned"),
		},
	})
	if err != nil {
		panic(err)
	}

	violationNumber := fmt.Sprintf("%v", (*extraction)["violation_number"].Value)
	violationDate := fmt.Sprintf("%v", (*extraction)["violation_date"].Value)
	vehiclePlate := fmt.Sprintf("%v", (*extraction)["vehicle_plate"].Value)
	driverName := fmt.Sprintf("%v", (*extraction)["driver_name"].Value)
	violationType := fmt.Sprintf("%v", (*extraction)["violation_type"].Value)
	location := fmt.Sprintf("%v", (*extraction)["location"].Value)
	dueDate := fmt.Sprintf("%v", (*extraction)["due_date"].Value)

	// Step 2: Generate a summary spreadsheet
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Sheets: []il.Sheet{
			{
				Name: "Violation Summary",
				Columns: []il.SheetColumn{
					{Name: "Violation #", Width: 20},
					{Name: "Date", Width: 14},
					{Name: "Plate", Width: 14},
					{Name: "Driver", Width: 22},
					{Name: "Type", Width: 18},
					{Name: "Location", Width: 30},
					{Name: "Fine", Width: 14},
					{Name: "Due Date", Width: 14},
					{Name: "Points", Width: 10},
				},
				Rows: [][]il.SheetCell{
					{
						{Value: violationNumber},
						{Value: violationDate, Format: "date"},
						{Value: vehiclePlate},
						{Value: driverName},
						{Value: violationType},
						{Value: location},
						{Value: 185.00, Format: "currency", CurrencyCode: "EUR"},
						{Value: dueDate, Format: "date"},
						{Value: 3, Format: "number"},
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
  "name": "Extract violations and generate fine summary in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Violations and Generate a Fine Summary\n\nFleet management agencies use this pipeline to process stacks of traffic fine notices — extracting violation details and generating a consolidated spreadsheet for client review, accounting, and driver accountability.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "d4e5f6a7-b8c9-0123-defa-123456789001",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Violation Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "d4e5f6a7-b8c9-0123-defa-123456789002",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Summary Spreadsheet\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "d4e5f6a7-b8c9-0123-defa-123456789003",
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
      "id": "d4e5f6a7-b8c9-0123-defa-123456789004",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"violation_number\",\"type\":\"TEXT\",\"description\":\"Violation or ticket reference number\"},{\"name\":\"violation_date\",\"type\":\"DATE\",\"description\":\"Date the violation occurred\"},{\"name\":\"vehicle_plate\",\"type\":\"TEXT\",\"description\":\"License plate number\"},{\"name\":\"driver_name\",\"type\":\"TEXT\",\"description\":\"Name of the driver if listed\"},{\"name\":\"violation_type\",\"type\":\"TEXT\",\"description\":\"Type of violation\"},{\"name\":\"location\",\"type\":\"TEXT\",\"description\":\"Location where the violation occurred\"},{\"name\":\"fine_amount\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Fine amount\"},{\"name\":\"due_date\",\"type\":\"DATE\",\"description\":\"Payment due date\"},{\"name\":\"points\",\"type\":\"INTEGER\",\"description\":\"Penalty points assigned\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "fine-notice-TRF-2026-4471.pdf",
              "fileUrl": "https://example.com/fines/fine-notice-TRF-2026-4471.pdf"
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
      "id": "d4e5f6a7-b8c9-0123-defa-123456789005",
      "name": "Extract Violation Data",
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
        "format": "xlsx",
        "sheetsJson": "[{\"name\":\"Violation Summary\",\"columns\":[{\"name\":\"Violation #\",\"width\":20},{\"name\":\"Date\",\"width\":14},{\"name\":\"Plate\",\"width\":14},{\"name\":\"Driver\",\"width\":22},{\"name\":\"Type\",\"width\":18},{\"name\":\"Location\",\"width\":30},{\"name\":\"Fine\",\"width\":14},{\"name\":\"Due Date\",\"width\":14},{\"name\":\"Points\",\"width\":10}],\"rows\":[[{\"value\":\"{{ $json.violation_number }}\"},{\"value\":\"{{ $json.violation_date }}\",\"format\":\"date\"},{\"value\":\"{{ $json.vehicle_plate }}\"},{\"value\":\"{{ $json.driver_name }}\"},{\"value\":\"{{ $json.violation_type }}\"},{\"value\":\"{{ $json.location }}\"},{\"value\":\"{{ $json.fine_amount }}\",\"format\":\"currency\",\"currency_code\":\"EUR\"},{\"value\":\"{{ $json.due_date }}\",\"format\":\"date\"},{\"value\":\"{{ $json.points }}\",\"format\":\"number\"}]]}]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "d4e5f6a7-b8c9-0123-defa-123456789006",
      "name": "Generate Summary Spreadsheet",
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
            "node": "Extract Violation Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Violation Data": {
      "main": [
        [
          {
            "node": "Generate Summary Spreadsheet",
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
Extract traffic violation data from the fine notice at [file URL], then generate a summary spreadsheet. Use the extract_document tool with these fields:

- violation_number (TEXT): Violation or ticket reference number
- violation_date (DATE): Date the violation occurred
- vehicle_plate (TEXT): License plate number
- driver_name (TEXT): Name of the driver if listed
- violation_type (TEXT): Type of violation, e.g. speeding, red light
- location (TEXT): Location where the violation occurred
- fine_amount (CURRENCY_AMOUNT): Fine amount
- due_date (DATE): Payment due date
- points (INTEGER): Penalty points assigned

Then use the generate_sheet tool to create an XLSX spreadsheet with columns for each field, containing the extracted violation data.
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
