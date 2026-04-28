---
name: extract-nda-and-generate-checklist
description: Extract key terms from a non-disclosure agreement, then generate a compliance checklist spreadsheet tracking obligations and deadlines.
---

# Extract NDA Terms and Generate a Compliance Checklist

Legal agencies managing NDAs across multiple clients use this pipeline to extract key confidentiality terms and generate standardized compliance checklists — tracking obligation scope, duration, permitted disclosures, and return-of-materials deadlines.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract NDA terms
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "nda-vanguard-tech.pdf",
        "url": "https://example.com/legal/nda-vanguard-tech-2026.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "disclosing_party",
          "type": "TEXT",
          "description": "Name of the disclosing party"
        },
        {
          "name": "receiving_party",
          "type": "TEXT",
          "description": "Name of the receiving party"
        },
        {
          "name": "effective_date",
          "type": "DATE",
          "description": "NDA effective date"
        },
        {
          "name": "confidentiality_duration",
          "type": "TEXT",
          "description": "Duration of confidentiality obligations"
        },
        {
          "name": "scope_of_confidential_info",
          "type": "TEXTAREA",
          "description": "Definition and scope of confidential information"
        },
        {
          "name": "permitted_disclosures",
          "type": "TEXTAREA",
          "description": "Exceptions or permitted disclosures"
        },
        {
          "name": "return_of_materials",
          "type": "TEXTAREA",
          "description": "Obligations to return or destroy materials"
        },
        {
          "name": "non_solicitation",
          "type": "BOOLEAN",
          "description": "Whether the NDA includes a non-solicitation clause"
        },
        {
          "name": "governing_law",
          "type": "TEXT",
          "description": "Governing law and jurisdiction"
        },
        {
          "name": "breach_remedies",
          "type": "TEXTAREA",
          "description": "Remedies available in case of breach"
        }
      ]
    }
  }')

# Step 2: Generate a compliance checklist spreadsheet
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "sheets": [
      {
        "name": "NDA Compliance Checklist",
        "columns": [
          {
            "name": "Obligation",
            "width": 35
          },
          {
            "name": "Details",
            "width": 50
          },
          {
            "name": "Status",
            "width": 15
          },
          {
            "name": "Due Date",
            "width": 15
          },
          {
            "name": "Notes",
            "width": 30
          }
        ],
        "rows": [
          [
            {
              "value": "Confidentiality scope acknowledged"
            },
            {
              "value": "All technical specifications, business plans, and customer data shared by Vanguard Technologies are covered."
            },
            {
              "value": "Pending"
            },
            {
              "value": "2026-04-15",
              "format": "date"
            },
            {
              "value": "Brief internal team on scope"
            }
          ],
          [
            {
              "value": "Permitted disclosure carve-outs documented"
            },
            {
              "value": "Court orders, regulatory requirements, and pre-existing public knowledge are excluded."
            },
            {
              "value": "Pending"
            },
            {
              "value": "2026-04-15",
              "format": "date"
            },
            {
              "value": ""
            }
          ],
          [
            {
              "value": "Confidentiality end date tracked"
            },
            {
              "value": "Obligations remain in effect for 3 years from the effective date."
            },
            {
              "value": "Pending"
            },
            {
              "value": "2029-04-01",
              "format": "date"
            },
            {
              "value": "Calendar reminder set"
            }
          ],
          [
            {
              "value": "Return of materials process defined"
            },
            {
              "value": "All materials must be returned or certified destroyed within 30 days of termination."
            },
            {
              "value": "Pending"
            },
            {
              "value": ""
            },
            {
              "value": "Applies at termination"
            }
          ],
          [
            {
              "value": "Non-solicitation compliance"
            },
            {
              "value": "No solicitation of disclosing party employees for 12 months."
            },
            {
              "value": "Active"
            },
            {
              "value": "2027-04-01",
              "format": "date"
            },
            {
              "value": ""
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

// Step 1: Extract NDA terms
const extraction = await client.extractDocument({
  files: [{ type: "url", name: "nda-vanguard-tech.pdf", url: "https://example.com/legal/nda-vanguard-tech-2026.pdf" }],
  schema: {
    fields: [
      { name: "disclosing_party", type: "TEXT", description: "Name of the disclosing party" },
      { name: "receiving_party", type: "TEXT", description: "Name of the receiving party" },
      { name: "effective_date", type: "DATE", description: "NDA effective date" },
      { name: "confidentiality_duration", type: "TEXT", description: "Duration of confidentiality obligations" },
      { name: "scope_of_confidential_info", type: "TEXTAREA", description: "Definition and scope of confidential information" },
      { name: "permitted_disclosures", type: "TEXTAREA", description: "Exceptions or permitted disclosures" },
      { name: "return_of_materials", type: "TEXTAREA", description: "Obligations to return or destroy materials" },
      { name: "non_solicitation", type: "BOOLEAN", description: "Whether the NDA includes a non-solicitation clause" },
      { name: "governing_law", type: "TEXT", description: "Governing law and jurisdiction" },
      { name: "breach_remedies", type: "TEXTAREA", description: "Remedies available in case of breach" },
    ],
  },
});

const scopeOfConfidentialInfo = String(extraction["scope_of_confidential_info"].value);
const permittedDisclosures = String(extraction["permitted_disclosures"].value);
const confidentialityDuration = String(extraction["confidentiality_duration"].value);
const returnOfMaterials = String(extraction["return_of_materials"].value);

// Step 2: Generate a compliance checklist spreadsheet
const checklist = await client.generateSheet({
  format: "xlsx",
  sheets: [
    {
      name: "NDA Compliance Checklist",
      columns: [
        { name: "Obligation", width: 35 },
        { name: "Details", width: 50 },
        { name: "Status", width: 15 },
        { name: "Due Date", width: 15 },
        { name: "Notes", width: 30 },
      ],
      rows: [
        [{ value: "Confidentiality scope acknowledged" }, { value: scopeOfConfidentialInfo }, { value: "Pending" }, { value: "" }, { value: "Brief internal team on scope" }],
        [{ value: "Permitted disclosure carve-outs documented" }, { value: permittedDisclosures }, { value: "Pending" }, { value: "" }, { value: "" }],
        [{ value: "Confidentiality end date tracked" }, { value: confidentialityDuration }, { value: "Pending" }, { value: "" }, { value: "Calendar reminder set" }],
        [{ value: "Return of materials process defined" }, { value: returnOfMaterials }, { value: "Pending" }, { value: "" }, { value: "Applies at termination" }],
      ],
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract NDA terms
extraction = client.extract_document(
    files=[{"type": "url", "name": "nda-vanguard-tech.pdf", "url": "https://example.com/legal/nda-vanguard-tech-2026.pdf"}],
    schema={
        "fields": [
            {"name": "disclosing_party", "type": "TEXT", "description": "Name of the disclosing party"},
            {"name": "receiving_party", "type": "TEXT", "description": "Name of the receiving party"},
            {"name": "effective_date", "type": "DATE", "description": "NDA effective date"},
            {"name": "confidentiality_duration", "type": "TEXT", "description": "Duration of confidentiality obligations"},
            {"name": "scope_of_confidential_info", "type": "TEXTAREA", "description": "Definition and scope of confidential information"},
            {"name": "permitted_disclosures", "type": "TEXTAREA", "description": "Exceptions or permitted disclosures"},
            {"name": "return_of_materials", "type": "TEXTAREA", "description": "Obligations to return or destroy materials"},
            {"name": "non_solicitation", "type": "BOOLEAN", "description": "Whether the NDA includes a non-solicitation clause"},
            {"name": "governing_law", "type": "TEXT", "description": "Governing law and jurisdiction"},
            {"name": "breach_remedies", "type": "TEXTAREA", "description": "Remedies available in case of breach"},
        ]
    },
)

scope_of_confidential_info = extraction["scope_of_confidential_info"]["value"]
permitted_disclosures = extraction["permitted_disclosures"]["value"]
confidentiality_duration = extraction["confidentiality_duration"]["value"]
return_of_materials = extraction["return_of_materials"]["value"]

# Step 2: Generate a compliance checklist spreadsheet
checklist = client.generate_sheet(
    format="xlsx",
    sheets=[
        {
            "name": "NDA Compliance Checklist",
            "columns": [
                {"name": "Obligation", "width": 35},
                {"name": "Details", "width": 50},
                {"name": "Status", "width": 15},
                {"name": "Due Date", "width": 15},
                {"name": "Notes", "width": 30},
            ],
            "rows": [
                [{"value": "Confidentiality scope acknowledged"}, {"value": scope_of_confidential_info}, {"value": "Pending"}, {"value": ""}, {"value": "Brief internal team on scope"}],
                [{"value": "Permitted disclosure carve-outs documented"}, {"value": permitted_disclosures}, {"value": "Pending"}, {"value": ""}, {"value": ""}],
                [{"value": "Confidentiality end date tracked"}, {"value": confidentiality_duration}, {"value": "Pending"}, {"value": ""}, {"value": "Calendar reminder set"}],
                [{"value": "Return of materials process defined"}, {"value": return_of_materials}, {"value": "Pending"}, {"value": ""}, {"value": "Applies at termination"}],
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

	// Step 1: Extract NDA terms
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("nda-vanguard-tech.pdf", "https://example.com/legal/nda-vanguard-tech-2026.pdf"),
		},
		Schema: il.ExtractionSchema{
			"disclosing_party":          il.NewTextFieldConfig("disclosing_party", "Name of the disclosing party"),
			"receiving_party":           il.NewTextFieldConfig("receiving_party", "Name of the receiving party"),
			"effective_date":            il.NewDateFieldConfig("effective_date", "NDA effective date"),
			"confidentiality_duration":  il.NewTextFieldConfig("confidentiality_duration", "Duration of confidentiality obligations"),
			"scope_of_confidential_info": il.NewTextareaFieldConfig("scope_of_confidential_info", "Definition and scope of confidential information"),
			"permitted_disclosures":     il.NewTextareaFieldConfig("permitted_disclosures", "Exceptions or permitted disclosures"),
			"return_of_materials":       il.NewTextareaFieldConfig("return_of_materials", "Obligations to return or destroy materials"),
			"non_solicitation":          il.NewBooleanFieldConfig("non_solicitation", "Whether the NDA includes a non-solicitation clause"),
			"governing_law":             il.NewTextFieldConfig("governing_law", "Governing law and jurisdiction"),
			"breach_remedies":           il.NewTextareaFieldConfig("breach_remedies", "Remedies available in case of breach"),
		},
	})
	if err != nil {
		panic(err)
	}

	scopeOfConfidentialInfo := fmt.Sprintf("%v", (*extraction)["scope_of_confidential_info"].Value)
	permittedDisclosures := fmt.Sprintf("%v", (*extraction)["permitted_disclosures"].Value)
	confidentialityDuration := fmt.Sprintf("%v", (*extraction)["confidentiality_duration"].Value)
	returnOfMaterials := fmt.Sprintf("%v", (*extraction)["return_of_materials"].Value)

	// Step 2: Generate a compliance checklist spreadsheet
	_, err = client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Sheets: []il.Sheet{
			{
				Name: "NDA Compliance Checklist",
				Columns: []il.SheetColumn{
					{Name: "Obligation", Width: 35}, {Name: "Details", Width: 50}, {Name: "Status", Width: 15}, {Name: "Due Date", Width: 15}, {Name: "Notes", Width: 30},
				},
				Rows: [][]il.SheetCell{
					{{Value: "Confidentiality scope acknowledged"}, {Value: scopeOfConfidentialInfo}, {Value: "Pending"}, {Value: ""}, {Value: "Brief internal team"}},
					{{Value: "Permitted disclosures documented"}, {Value: permittedDisclosures}, {Value: "Pending"}, {Value: ""}, {Value: ""}},
					{{Value: "Confidentiality end date tracked"}, {Value: confidentialityDuration}, {Value: "Pending"}, {Value: ""}, {Value: "Calendar reminder set"}},
					{{Value: "Return of materials process"}, {Value: returnOfMaterials}, {Value: "Pending"}, {Value: ""}, {Value: "Applies at termination"}},
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
  "name": "Extract NDA terms and generate compliance checklist in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract NDA Terms and Generate a Compliance Checklist\n\nLegal agencies managing NDAs across multiple clients use this pipeline to extract key confidentiality terms and generate standardized compliance checklists.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789001",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract NDA Terms\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789002",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Checklist\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [725, 100],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789003",
      "name": "Step 2 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789004",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"disclosing_party\",\"type\":\"TEXT\",\"description\":\"Disclosing party\"},{\"name\":\"receiving_party\",\"type\":\"TEXT\",\"description\":\"Receiving party\"},{\"name\":\"effective_date\",\"type\":\"DATE\",\"description\":\"NDA effective date\"},{\"name\":\"confidentiality_duration\",\"type\":\"TEXT\",\"description\":\"Duration of obligations\"},{\"name\":\"scope_of_confidential_info\",\"type\":\"TEXTAREA\",\"description\":\"Scope of confidential information\"},{\"name\":\"permitted_disclosures\",\"type\":\"TEXTAREA\",\"description\":\"Permitted disclosures\"},{\"name\":\"return_of_materials\",\"type\":\"TEXTAREA\",\"description\":\"Return of materials\"},{\"name\":\"non_solicitation\",\"type\":\"BOOLEAN\",\"description\":\"Non-solicitation clause\"},{\"name\":\"governing_law\",\"type\":\"TEXT\",\"description\":\"Governing law\"},{\"name\":\"breach_remedies\",\"type\":\"TEXTAREA\",\"description\":\"Breach remedies\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "nda-vanguard-tech.pdf",
              "fileUrl": "https://example.com/legal/nda-vanguard-tech-2026.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789005",
      "name": "Extract NDA Terms",
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
        "sheetsJson": "[{\"name\":\"NDA Compliance Checklist\",\"columns\":[{\"name\":\"Obligation\",\"width\":35},{\"name\":\"Details\",\"width\":50},{\"name\":\"Status\",\"width\":15},{\"name\":\"Due Date\",\"width\":15},{\"name\":\"Notes\",\"width\":30}],\"rows\":[[{\"value\":\"Confidentiality scope acknowledged\"},{\"value\":\"{{ $json.scope_of_confidential_info }}\"},{\"value\":\"Pending\"},{\"value\":\"\"},{\"value\":\"Brief team\"}],[{\"value\":\"Permitted disclosures documented\"},{\"value\":\"{{ $json.permitted_disclosures }}\"},{\"value\":\"Pending\"},{\"value\":\"\"},{\"value\":\"\"}],[{\"value\":\"Confidentiality end date tracked\"},{\"value\":\"{{ $json.confidentiality_duration }}\"},{\"value\":\"Pending\"},{\"value\":\"\"},{\"value\":\"Set calendar reminder\"}],[{\"value\":\"Return of materials process\"},{\"value\":\"{{ $json.return_of_materials }}\"},{\"value\":\"Pending\"},{\"value\":\"\"},{\"value\":\"At termination\"}]]}]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [750, 300],
      "id": "b4c5d6e7-f8a9-0123-bcde-123456789006",
      "name": "Generate Checklist",
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
            "node": "Extract NDA Terms",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract NDA Terms": {
      "main": [
        [
          {
            "node": "Generate Checklist",
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
Extract NDA terms from the document at [file URL], then generate a compliance checklist. Use the extract_document tool with these fields:

- disclosing_party (TEXT): Name of the disclosing party
- receiving_party (TEXT): Name of the receiving party
- effective_date (DATE): NDA effective date
- confidentiality_duration (TEXT): Duration of confidentiality obligations
- scope_of_confidential_info (TEXTAREA): Definition and scope of confidential information
- permitted_disclosures (TEXTAREA): Exceptions or permitted disclosures
- return_of_materials (TEXTAREA): Obligations to return or destroy materials
- non_solicitation (BOOLEAN): Whether the NDA includes a non-solicitation clause
- governing_law (TEXT): Governing law and jurisdiction
- breach_remedies (TEXTAREA): Remedies available in case of breach

Then use the generate_sheet tool to create an XLSX checklist with columns for Obligation, Details, Status, Due Date, and Notes, with one row per key obligation extracted from the NDA.
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
