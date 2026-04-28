---
name: extract-fleet-vehicle-registration
description: Extract vehicle identification, owner details, registration dates, and technical specifications from vehicle registration documents.
---

# Extract Fleet Vehicle Registration Data

Fleet management agencies use this recipe to digitize vehicle registration documents — extracting VIN numbers, registration dates, insurance details, and technical specs for centralized fleet databases.

## APIs Used

Document Extraction (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "vehicle-registration-FL-4821.pdf",
        "url": "https://example.com/fleet/registration-FL-4821.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "license_plate",
          "type": "TEXT",
          "description": "Vehicle license plate number"
        },
        {
          "name": "vin",
          "type": "TEXT",
          "description": "Vehicle Identification Number (VIN)"
        },
        {
          "name": "make",
          "type": "TEXT",
          "description": "Vehicle manufacturer"
        },
        {
          "name": "model",
          "type": "TEXT",
          "description": "Vehicle model"
        },
        {
          "name": "year",
          "type": "INTEGER",
          "description": "Model year"
        },
        {
          "name": "body_type",
          "type": "TEXT",
          "description": "Body type, e.g. van, truck, sedan"
        },
        {
          "name": "fuel_type",
          "type": "TEXT",
          "description": "Fuel type, e.g. diesel, electric, petrol"
        },
        {
          "name": "registered_owner",
          "type": "TEXT",
          "description": "Registered owner name or company"
        },
        {
          "name": "registration_date",
          "type": "DATE",
          "description": "Date of first registration"
        },
        {
          "name": "registration_expiry",
          "type": "DATE",
          "description": "Registration expiry date"
        },
        {
          "name": "gross_vehicle_weight",
          "type": "TEXT",
          "description": "Maximum gross vehicle weight"
        },
        {
          "name": "emission_class",
          "type": "TEXT",
          "description": "Emission standard class"
        }
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "vehicle-registration-FL-4821.pdf",
      url: "https://example.com/fleet/registration-FL-4821.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "license_plate", type: "TEXT", description: "Vehicle license plate number" },
      { name: "vin", type: "TEXT", description: "Vehicle Identification Number (VIN)" },
      { name: "make", type: "TEXT", description: "Vehicle manufacturer" },
      { name: "model", type: "TEXT", description: "Vehicle model" },
      { name: "year", type: "INTEGER", description: "Model year" },
      { name: "body_type", type: "TEXT", description: "Body type, e.g. van, truck, sedan" },
      { name: "fuel_type", type: "TEXT", description: "Fuel type, e.g. diesel, electric, petrol" },
      { name: "registered_owner", type: "TEXT", description: "Registered owner name or company" },
      { name: "registration_date", type: "DATE", description: "Date of first registration" },
      { name: "registration_expiry", type: "DATE", description: "Registration expiry date" },
      { name: "gross_vehicle_weight", type: "TEXT", description: "Maximum gross vehicle weight" },
      { name: "emission_class", type: "TEXT", description: "Emission standard class" },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "vehicle-registration-FL-4821.pdf",
            "url": "https://example.com/fleet/registration-FL-4821.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "license_plate", "type": "TEXT", "description": "Vehicle license plate number"},
            {"name": "vin", "type": "TEXT", "description": "Vehicle Identification Number (VIN)"},
            {"name": "make", "type": "TEXT", "description": "Vehicle manufacturer"},
            {"name": "model", "type": "TEXT", "description": "Vehicle model"},
            {"name": "year", "type": "INTEGER", "description": "Model year"},
            {"name": "body_type", "type": "TEXT", "description": "Body type, e.g. van, truck, sedan"},
            {"name": "fuel_type", "type": "TEXT", "description": "Fuel type, e.g. diesel, electric, petrol"},
            {"name": "registered_owner", "type": "TEXT", "description": "Registered owner name or company"},
            {"name": "registration_date", "type": "DATE", "description": "Date of first registration"},
            {"name": "registration_expiry", "type": "DATE", "description": "Registration expiry date"},
            {"name": "gross_vehicle_weight", "type": "TEXT", "description": "Maximum gross vehicle weight"},
            {"name": "emission_class", "type": "TEXT", "description": "Emission standard class"},
        ]
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("vehicle-registration-FL-4821.pdf", "https://example.com/fleet/registration-FL-4821.pdf"),
		},
		Schema: il.ExtractionSchema{
			"license_plate":        il.NewTextFieldConfig("license_plate", "Vehicle license plate number"),
			"vin":                  il.NewTextFieldConfig("vin", "Vehicle Identification Number (VIN)"),
			"make":                 il.NewTextFieldConfig("make", "Vehicle manufacturer"),
			"model":                il.NewTextFieldConfig("model", "Vehicle model"),
			"year":                 il.NewIntegerFieldConfig("year", "Model year"),
			"body_type":            il.NewTextFieldConfig("body_type", "Body type, e.g. van, truck, sedan"),
			"fuel_type":            il.NewTextFieldConfig("fuel_type", "Fuel type, e.g. diesel, electric, petrol"),
			"registered_owner":     il.NewTextFieldConfig("registered_owner", "Registered owner name or company"),
			"registration_date":    il.NewDateFieldConfig("registration_date", "Date of first registration"),
			"registration_expiry":  il.NewDateFieldConfig("registration_expiry", "Registration expiry date"),
			"gross_vehicle_weight": il.NewTextFieldConfig("gross_vehicle_weight", "Maximum gross vehicle weight"),
			"emission_class":       il.NewTextFieldConfig("emission_class", "Emission standard class"),
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
  "name": "Extract fleet vehicle registration data in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Fleet Vehicle Registration Data\n\nFleet management agencies use this recipe to digitize vehicle registration documents — extracting VIN numbers, registration dates, insurance details, and technical specs for centralized fleet databases.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "b8c9d0e1-f2a3-4567-bcde-567890123401",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Registration Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "b8c9d0e1-f2a3-4567-bcde-567890123402",
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
      "id": "b8c9d0e1-f2a3-4567-bcde-567890123403",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"license_plate\",\"type\":\"TEXT\",\"description\":\"Vehicle license plate number\"},{\"name\":\"vin\",\"type\":\"TEXT\",\"description\":\"Vehicle Identification Number (VIN)\"},{\"name\":\"make\",\"type\":\"TEXT\",\"description\":\"Vehicle manufacturer\"},{\"name\":\"model\",\"type\":\"TEXT\",\"description\":\"Vehicle model\"},{\"name\":\"year\",\"type\":\"INTEGER\",\"description\":\"Model year\"},{\"name\":\"body_type\",\"type\":\"TEXT\",\"description\":\"Body type\"},{\"name\":\"fuel_type\",\"type\":\"TEXT\",\"description\":\"Fuel type\"},{\"name\":\"registered_owner\",\"type\":\"TEXT\",\"description\":\"Registered owner name or company\"},{\"name\":\"registration_date\",\"type\":\"DATE\",\"description\":\"Date of first registration\"},{\"name\":\"registration_expiry\",\"type\":\"DATE\",\"description\":\"Registration expiry date\"},{\"name\":\"gross_vehicle_weight\",\"type\":\"TEXT\",\"description\":\"Maximum gross vehicle weight\"},{\"name\":\"emission_class\",\"type\":\"TEXT\",\"description\":\"Emission standard class\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "vehicle-registration-FL-4821.pdf",
              "fileUrl": "https://example.com/fleet/registration-FL-4821.pdf"
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
      "id": "b8c9d0e1-f2a3-4567-bcde-567890123404",
      "name": "Extract Registration Data",
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
            "node": "Extract Registration Data",
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
Extract fleet vehicle registration data from the file at [file URL]. Use the extract_document tool with these fields:

- license_plate (TEXT): Vehicle license plate number
- vin (TEXT): Vehicle Identification Number (VIN)
- make (TEXT): Vehicle manufacturer
- model (TEXT): Vehicle model
- year (INTEGER): Model year
- body_type (TEXT): Body type, e.g. van, truck, sedan
- fuel_type (TEXT): Fuel type, e.g. diesel, electric, petrol
- registered_owner (TEXT): Registered owner name or company
- registration_date (DATE): Date of first registration
- registration_expiry (DATE): Registration expiry date
- gross_vehicle_weight (TEXT): Maximum gross vehicle weight
- emission_class (TEXT): Emission standard class
```

### Response


```json
{
  "success": true,
  "data": {
    "license_plate": {
      "value": "MI-4821-KZ",
      "confidence": 0.99,
      "citations": [
        "Targa: MI-4821-KZ"
      ]
    },
    "vin": {
      "value": "WVGZZZ5NZYW048291",
      "confidence": 0.98,
      "citations": [
        "Telaio: WVGZZZ5NZYW048291"
      ]
    },
    "make": {
      "value": "Iveco",
      "confidence": 0.99,
      "citations": [
        "Marca: Iveco"
      ]
    },
    "model": {
      "value": "Daily 35S14",
      "confidence": 0.97,
      "citations": [
        "Modello: Daily 35S14"
      ]
    },
    "year": {
      "value": 2024,
      "confidence": 0.98,
      "citations": [
        "Anno: 2024"
      ]
    },
    "body_type": {
      "value": "Van",
      "confidence": 0.96,
      "citations": [
        "Carrozzeria: Furgone"
      ]
    },
    "fuel_type": {
      "value": "Diesel",
      "confidence": 0.99,
      "citations": [
        "Alimentazione: Gasolio"
      ]
    },
    "registered_owner": {
      "value": "Veloce Logistics S.r.l.",
      "confidence": 0.98,
      "citations": [
        "Intestatario: Veloce Logistics S.r.l."
      ]
    },
    "registration_date": {
      "value": "2024-06-12",
      "confidence": 0.97,
      "citations": [
        "Data immatricolazione: 12/06/2024"
      ]
    },
    "registration_expiry": {
      "value": "2026-06-12",
      "confidence": 0.96,
      "citations": [
        "Scadenza revisione: 12/06/2026"
      ]
    },
    "gross_vehicle_weight": {
      "value": "3,500 kg",
      "confidence": 0.97,
      "citations": [
        "Massa complessiva: 3.500 kg"
      ]
    },
    "emission_class": {
      "value": "Euro 6d",
      "confidence": 0.95,
      "citations": [
        "Classe emissioni: Euro 6d"
      ]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
