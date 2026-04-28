---
name: extract-terms-and-conditions
description: Extract clause types, obligations, limitations, and governing law from terms and conditions documents.
---

# Extract Terms and Conditions

Legal agencies, compliance teams, and e-commerce platforms use this recipe to extract key clauses from vendor or partner terms — pulling out termination conditions, liability caps, data handling provisions, and governing law for contract review and risk assessment.

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
        "name": "vendor-terms-cloudpeak.pdf",
        "url": "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "document_title",
          "type": "TEXT",
          "description": "Title of the terms document"
        },
        {
          "name": "effective_date",
          "type": "DATE",
          "description": "Effective date of the terms"
        },
        {
          "name": "parties",
          "type": "TEXT",
          "description": "Parties to the agreement"
        },
        {
          "name": "termination_clause",
          "type": "TEXTAREA",
          "description": "Termination conditions and notice period"
        },
        {
          "name": "liability_limitation",
          "type": "TEXTAREA",
          "description": "Liability limitation or cap"
        },
        {
          "name": "indemnification",
          "type": "TEXTAREA",
          "description": "Indemnification obligations"
        },
        {
          "name": "data_handling",
          "type": "TEXTAREA",
          "description": "Data processing and privacy provisions"
        },
        {
          "name": "governing_law",
          "type": "TEXT",
          "description": "Governing law and jurisdiction"
        },
        {
          "name": "payment_terms",
          "type": "TEXT",
          "description": "Payment terms and conditions"
        },
        {
          "name": "auto_renewal",
          "type": "BOOLEAN",
          "description": "Whether the agreement auto-renews"
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
      name: "vendor-terms-cloudpeak.pdf",
      url: "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "document_title", type: "TEXT", description: "Title of the terms document" },
      { name: "effective_date", type: "DATE", description: "Effective date of the terms" },
      { name: "parties", type: "TEXT", description: "Parties to the agreement" },
      { name: "termination_clause", type: "TEXTAREA", description: "Termination conditions and notice period" },
      { name: "liability_limitation", type: "TEXTAREA", description: "Liability limitation or cap" },
      { name: "indemnification", type: "TEXTAREA", description: "Indemnification obligations" },
      { name: "data_handling", type: "TEXTAREA", description: "Data processing and privacy provisions" },
      { name: "governing_law", type: "TEXT", description: "Governing law and jurisdiction" },
      { name: "payment_terms", type: "TEXT", description: "Payment terms and conditions" },
      { name: "auto_renewal", type: "BOOLEAN", description: "Whether the agreement auto-renews" },
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
            "name": "vendor-terms-cloudpeak.pdf",
            "url": "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "document_title", "type": "TEXT", "description": "Title of the terms document"},
            {"name": "effective_date", "type": "DATE", "description": "Effective date of the terms"},
            {"name": "parties", "type": "TEXT", "description": "Parties to the agreement"},
            {"name": "termination_clause", "type": "TEXTAREA", "description": "Termination conditions and notice period"},
            {"name": "liability_limitation", "type": "TEXTAREA", "description": "Liability limitation or cap"},
            {"name": "indemnification", "type": "TEXTAREA", "description": "Indemnification obligations"},
            {"name": "data_handling", "type": "TEXTAREA", "description": "Data processing and privacy provisions"},
            {"name": "governing_law", "type": "TEXT", "description": "Governing law and jurisdiction"},
            {"name": "payment_terms", "type": "TEXT", "description": "Payment terms and conditions"},
            {"name": "auto_renewal", "type": "BOOLEAN", "description": "Whether the agreement auto-renews"},
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
			il.NewFileFromURL("vendor-terms-cloudpeak.pdf", "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf"),
		},
		Schema: il.ExtractionSchema{
			"document_title":      il.NewTextFieldConfig("document_title", "Title of the terms document"),
			"effective_date":      il.NewDateFieldConfig("effective_date", "Effective date of the terms"),
			"parties":             il.NewTextFieldConfig("parties", "Parties to the agreement"),
			"termination_clause":  il.NewTextareaFieldConfig("termination_clause", "Termination conditions and notice period"),
			"liability_limitation": il.NewTextareaFieldConfig("liability_limitation", "Liability limitation or cap"),
			"indemnification":     il.NewTextareaFieldConfig("indemnification", "Indemnification obligations"),
			"data_handling":       il.NewTextareaFieldConfig("data_handling", "Data processing and privacy provisions"),
			"governing_law":       il.NewTextFieldConfig("governing_law", "Governing law and jurisdiction"),
			"payment_terms":       il.NewTextFieldConfig("payment_terms", "Payment terms and conditions"),
			"auto_renewal":        il.NewBooleanFieldConfig("auto_renewal", "Whether the agreement auto-renews"),
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
  "name": "Extract terms and conditions in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Terms and Conditions\n\nLegal agencies, compliance teams, and e-commerce platforms use this recipe to extract key clauses from vendor or partner terms — pulling out termination conditions, liability caps, data handling provisions, and governing law for contract review and risk assessment.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "a4b5c6d7-e8f9-0123-abcd-890123456701",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Terms and Conditions\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "a4b5c6d7-e8f9-0123-abcd-890123456702",
      "name": "Step 1 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "a4b5c6d7-e8f9-0123-abcd-890123456703",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"document_title\",\"type\":\"TEXT\",\"description\":\"Title of the terms document\"},{\"name\":\"effective_date\",\"type\":\"DATE\",\"description\":\"Effective date of the terms\"},{\"name\":\"parties\",\"type\":\"TEXT\",\"description\":\"Parties to the agreement\"},{\"name\":\"termination_clause\",\"type\":\"TEXTAREA\",\"description\":\"Termination conditions and notice period\"},{\"name\":\"liability_limitation\",\"type\":\"TEXTAREA\",\"description\":\"Liability limitation or cap\"},{\"name\":\"indemnification\",\"type\":\"TEXTAREA\",\"description\":\"Indemnification obligations\"},{\"name\":\"data_handling\",\"type\":\"TEXTAREA\",\"description\":\"Data processing and privacy provisions\"},{\"name\":\"governing_law\",\"type\":\"TEXT\",\"description\":\"Governing law and jurisdiction\"},{\"name\":\"payment_terms\",\"type\":\"TEXT\",\"description\":\"Payment terms and conditions\"},{\"name\":\"auto_renewal\",\"type\":\"BOOLEAN\",\"description\":\"Whether the agreement auto-renews\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "vendor-terms-cloudpeak.pdf",
              "fileUrl": "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "a4b5c6d7-e8f9-0123-abcd-890123456704",
      "name": "Extract Terms and Conditions",
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
            "node": "Extract Terms and Conditions",
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
Extract terms and conditions from the file at [file URL]. Use the extract_document tool with these fields:

- document_title (TEXT): Title of the terms document
- effective_date (DATE): Effective date of the terms
- parties (TEXT): Parties to the agreement
- termination_clause (TEXTAREA): Termination conditions and notice period
- liability_limitation (TEXTAREA): Liability limitation or cap
- indemnification (TEXTAREA): Indemnification obligations
- data_handling (TEXTAREA): Data processing and privacy provisions
- governing_law (TEXT): Governing law and jurisdiction
- payment_terms (TEXT): Payment terms and conditions
- auto_renewal (BOOLEAN): Whether the agreement auto-renews
```

### Response


```json
{
  "success": true,
  "data": {
    "document_title": {
      "value": "CloudPeak SaaS Master Services Agreement",
      "confidence": 0.99,
      "citations": ["CloudPeak SaaS Master Services Agreement"]
    },
    "effective_date": {
      "value": "2026-04-01",
      "confidence": 0.98,
      "citations": ["Effective Date: April 1, 2026"]
    },
    "parties": {
      "value": "CloudPeak Solutions B.V. and Whitmore Legal Advisors LLP",
      "confidence": 0.98,
      "citations": ["between CloudPeak Solutions B.V. (\"Provider\") and Whitmore Legal Advisors LLP (\"Customer\")"]
    },
    "termination_clause": {
      "value": "Either party may terminate with 90 days written notice. CloudPeak may terminate immediately for non-payment after a 30-day cure period. Upon termination, Customer data is exported within 30 days and deleted within 90 days.",
      "confidence": 0.96,
      "citations": ["Either party may terminate this Agreement upon ninety (90) days prior written notice", "Provider may terminate immediately if Customer fails to cure non-payment within thirty (30) days"]
    },
    "liability_limitation": {
      "value": "Total aggregate liability is capped at the fees paid during the 12 months preceding the claim. Neither party is liable for indirect, consequential, incidental, or punitive damages.",
      "confidence": 0.95,
      "citations": ["aggregate liability shall not exceed the total fees paid during the twelve (12) months preceding the event", "neither party shall be liable for any indirect, consequential, incidental, or punitive damages"]
    },
    "indemnification": {
      "value": "CloudPeak indemnifies Customer against third-party IP infringement claims arising from use of the service. Customer indemnifies CloudPeak against claims arising from Customer's data or misuse of the platform.",
      "confidence": 0.94,
      "citations": ["Provider shall indemnify Customer against third-party claims alleging intellectual property infringement", "Customer shall indemnify Provider against claims arising from Customer Data"]
    },
    "data_handling": {
      "value": "CloudPeak processes data as a data processor under GDPR. All data is stored in EU-based data centers (Frankfurt, Amsterdam). A Data Processing Agreement is incorporated by reference. Sub-processors require prior written consent.",
      "confidence": 0.96,
      "citations": ["Provider acts as a data processor under GDPR", "Data stored in EU-based data centers", "Data Processing Agreement incorporated by reference"]
    },
    "governing_law": {
      "value": "Laws of the Netherlands. Disputes resolved in the courts of Amsterdam.",
      "confidence": 0.98,
      "citations": ["governed by the laws of the Netherlands", "courts of Amsterdam shall have exclusive jurisdiction"]
    },
    "payment_terms": {
      "value": "Net 30 from invoice date. Invoices issued quarterly in advance. Late payments accrue interest at 1.5% per month.",
      "confidence": 0.97,
      "citations": ["Payment due within thirty (30) days of invoice", "invoiced quarterly in advance", "late payments shall accrue interest at 1.5% per month"]
    },
    "auto_renewal": {
      "value": true,
      "confidence": 0.97,
      "citations": ["Agreement automatically renews for successive one-year terms unless either party provides sixty (60) days written notice"]
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
