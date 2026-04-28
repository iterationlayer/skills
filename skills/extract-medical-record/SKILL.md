---
name: extract-medical-record
description: Extract patient details, diagnoses, and medications from a medical record into structured JSON for healthcare workflows.
---

# Extract Medical Record

Healthcare providers and clinical software platforms use this recipe to digitize a patient record. Upload a medical document and receive structured JSON with patient details, diagnosis codes, and medication list — ready for integration with your EHR or clinical database.

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
        "name": "medical-record.pdf",
        "url": "https://example.com/records/medical-record.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "patient_name",
          "type": "TEXT",
          "description": "Full name of the patient"
        },
        {
          "name": "date_of_birth",
          "type": "DATE",
          "description": "Patient date of birth"
        },
        {
          "name": "diagnoses",
          "type": "ARRAY",
          "description": "List of diagnoses",
          "fields": [
            {
              "name": "code",
              "type": "TEXT",
              "description": "Diagnosis or ICD code"
            },
            {
              "name": "description",
              "type": "TEXT",
              "description": "Diagnosis description"
            }
          ]
        },
        {
          "name": "medications",
          "type": "ARRAY",
          "description": "List of prescribed medications",
          "fields": [
            {
              "name": "name",
              "type": "TEXT",
              "description": "Medication name"
            },
            {
              "name": "dosage",
              "type": "TEXT",
              "description": "Prescribed dosage"
            }
          ]
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
      name: "medical-record.pdf",
      url: "https://example.com/records/medical-record.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "patient_name",
        type: "TEXT",
        description: "Full name of the patient",
      },
      {
        name: "date_of_birth",
        type: "DATE",
        description: "Patient date of birth",
      },
      {
        name: "diagnoses",
        type: "ARRAY",
        description: "List of diagnoses",
        fields: [
          {
            name: "code",
            type: "TEXT",
            description: "Diagnosis or ICD code",
          },
          {
            name: "description",
            type: "TEXT",
            description: "Diagnosis description",
          },
        ],
      },
      {
        name: "medications",
        type: "ARRAY",
        description: "List of prescribed medications",
        fields: [
          {
            name: "name",
            type: "TEXT",
            description: "Medication name",
          },
          {
            name: "dosage",
            type: "TEXT",
            description: "Prescribed dosage",
          },
        ],
      },
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
            "name": "medical-record.pdf",
            "url": "https://example.com/records/medical-record.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "patient_name",
                "type": "TEXT",
                "description": "Full name of the patient",
            },
            {
                "name": "date_of_birth",
                "type": "DATE",
                "description": "Patient date of birth",
            },
            {
                "name": "diagnoses",
                "type": "ARRAY",
                "description": "List of diagnoses",
                "fields": [
                    {
                        "name": "code",
                        "type": "TEXT",
                        "description": "Diagnosis or ICD code",
                    },
                    {
                        "name": "description",
                        "type": "TEXT",
                        "description": "Diagnosis description",
                    },
                ],
            },
            {
                "name": "medications",
                "type": "ARRAY",
                "description": "List of prescribed medications",
                "fields": [
                    {
                        "name": "name",
                        "type": "TEXT",
                        "description": "Medication name",
                    },
                    {
                        "name": "dosage",
                        "type": "TEXT",
                        "description": "Prescribed dosage",
                    },
                ],
            },
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
			il.NewFileFromURL(
				"medical-record.pdf",
				"https://example.com/records/medical-record.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"patient_name": il.NewTextFieldConfig(
				"patient_name",
				"Full name of the patient",
			),
			"date_of_birth": il.NewDateFieldConfig(
				"date_of_birth",
				"Patient date of birth",
			),
			"diagnoses": il.NewArrayFieldConfig(
				"diagnoses",
				"List of diagnoses",
				[]il.FieldConfig{
					il.NewTextFieldConfig(
						"code",
						"Diagnosis or ICD code",
					),
					il.NewTextFieldConfig(
						"description",
						"Diagnosis description",
					),
				},
			),
			"medications": il.NewArrayFieldConfig(
				"medications",
				"List of prescribed medications",
				[]il.FieldConfig{
					il.NewTextFieldConfig(
						"name",
						"Medication name",
					),
					il.NewTextFieldConfig(
						"dosage",
						"Prescribed dosage",
					),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Extract Medical Record",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Medical Record\n\nHealthcare providers and clinical software platforms use this recipe to digitize a patient record. Upload a medical document and receive structured JSON with patient details, diagnosis codes, and medication list \u2014 ready for integration with your EHR or clinical database.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "acff055e-c5f3-432a-ad13-7ba4b47fb3a6",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "3ec7e1a2-d227-42d7-93a5-7711a7a353b7",
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
      "id": "e1f2a3b4-c5d6-7890-efab-901234567890",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"patient_name\",\"type\":\"TEXT\",\"description\":\"Full name of the patient\"},{\"name\":\"date_of_birth\",\"type\":\"DATE\",\"description\":\"Patient date of birth\"},{\"name\":\"diagnoses\",\"type\":\"ARRAY\",\"description\":\"List of diagnoses\",\"fields\":[{\"name\":\"code\",\"type\":\"TEXT\",\"description\":\"Diagnosis or ICD code\"},{\"name\":\"description\",\"type\":\"TEXT\",\"description\":\"Diagnosis description\"}]},{\"name\":\"medications\",\"type\":\"ARRAY\",\"description\":\"List of prescribed medications\",\"fields\":[{\"name\":\"name\",\"type\":\"TEXT\",\"description\":\"Medication name\"},{\"name\":\"dosage\",\"type\":\"TEXT\",\"description\":\"Prescribed dosage\"}]}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "medical-record.pdf",
              "fileUrl": "https://example.com/records/medical-record.pdf"
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
      "id": "f2a3b4c5-d6e7-8901-fabc-012345678901",
      "name": "Extract Data",
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
            "node": "Extract Data",
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
Extract medical record data from the file at [file URL]. Use the extract_document tool with these fields:

- patient_name (TEXT): Full name of the patient
- date_of_birth (DATE): Patient date of birth
- diagnoses (ARRAY): Each with code (TEXT) and description (TEXT)
- medications (ARRAY): Each with name (TEXT) and dosage (TEXT)
```

### Response


```json
{
  "success": true,
  "data": {
    "patient_name": {
      "value": "James R. Patterson",
      "confidence": 0.99,
      "citations": ["Patient: James R. Patterson"]
    },
    "date_of_birth": {
      "value": "1978-03-22",
      "confidence": 0.98,
      "citations": ["DOB: 03/22/1978"]
    },
    "diagnoses": {
      "value": [
        {
          "code": {
            "value": "E11.9",
            "confidence": 0.97,
            "citations": ["E11.9"]
          },
          "description": {
            "value": "Type 2 diabetes mellitus without complications",
            "confidence": 0.96,
            "citations": ["Type 2 diabetes mellitus without complications"]
          }
        },
        {
          "code": {
            "value": "I10",
            "confidence": 0.98,
            "citations": ["I10"]
          },
          "description": {
            "value": "Essential hypertension",
            "confidence": 0.97,
            "citations": ["Essential (primary) hypertension"]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    },
    "medications": {
      "value": [
        {
          "name": {
            "value": "Metformin",
            "confidence": 0.98,
            "citations": ["Metformin HCl"]
          },
          "dosage": {
            "value": "500mg twice daily",
            "confidence": 0.95,
            "citations": ["500mg BID"]
          }
        },
        {
          "name": {
            "value": "Lisinopril",
            "confidence": 0.97,
            "citations": ["Lisinopril"]
          },
          "dosage": {
            "value": "10mg once daily",
            "confidence": 0.96,
            "citations": ["10mg QD"]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
