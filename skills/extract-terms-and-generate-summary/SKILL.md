---
name: extract-terms-and-generate-summary
description: Extract key clauses from terms and conditions documents, then generate a plain-language PDF summary for client review.
---

# Extract Terms and Generate a Simplified Summary

Legal agencies and compliance teams use this pipeline to process vendor or partner terms — extracting key clauses like liability limits, termination conditions, and data handling provisions, then generating a plain-language summary for non-legal stakeholders.

## APIs Used

Document Extraction (1 credit per page), Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract key clauses from T&C document
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
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
  }')

# Step 2: Generate a plain-language summary PDF
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "T&C Summary — CloudPeak SaaS Agreement",
        "author": "Whitmore Legal Advisors"
      },
      "page": {
        "size": {
          "preset": "A4"
        },
        "margins": {
          "top_in_pt": 54,
          "right_in_pt": 54,
          "bottom_in_pt": 54,
          "left_in_pt": 54
        }
      },
      "content": [
        {
          "type": "headline",
          "level": "h1",
          "text": "Terms & Conditions Summary"
        },
        {
          "type": "headline",
          "level": "h2",
          "text": "CloudPeak SaaS Master Services Agreement"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Effective: ",
              "font_weight": "bold"
            },
            {
              "text": "April 1, 2026"
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Termination"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Either party may terminate with 90 days written notice. CloudPeak may terminate immediately for non-payment after a 30-day cure period."
            }
          ]
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Liability"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Total liability is capped at 12 months of fees paid. Neither party is liable for indirect, consequential, or punitive damages."
            }
          ]
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Data Handling"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "CloudPeak processes data as a data processor under GDPR. Data is stored in EU-based data centers. A Data Processing Agreement is incorporated by reference."
            }
          ]
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Governing Law"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Governed by the laws of the Netherlands. Disputes resolved in the courts of Amsterdam."
            }
          ]
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Payment & Renewal"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Net 30 payment terms. The agreement auto-renews annually unless either party provides 60 days notice before the renewal date."
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "This summary is for informational purposes only and does not constitute legal advice. Prepared by Whitmore Legal Advisors."
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

// Step 1: Extract key clauses from T&C document
const extraction = await client.extractDocument({
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

const documentTitle = String(extraction["document_title"].value);
const effectiveDate = String(extraction["effective_date"].value);
const terminationClause = String(extraction["termination_clause"].value);
const liabilityLimitation = String(extraction["liability_limitation"].value);
const dataHandling = String(extraction["data_handling"].value);
const governingLaw = String(extraction["governing_law"].value);
const paymentTerms = String(extraction["payment_terms"].value);
const autoRenewal = Boolean(extraction["auto_renewal"].value);

// Step 2: Generate a plain-language summary PDF
const summary = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: { title: `T&C Summary — ${documentTitle}`, author: "Whitmore Legal Advisors" },
    page: {
      size: { preset: "A4" },
      margins: { top_in_pt: 54, right_in_pt: 54, bottom_in_pt: 54, left_in_pt: 54 },
    },
    content: [
      { type: "headline", level: "h1", text: "Terms & Conditions Summary" },
      { type: "headline", level: "h2", text: documentTitle },
      { type: "paragraph", runs: [{ text: "Effective: ", font_weight: "bold" }, { text: effectiveDate }] },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Termination" },
      { type: "paragraph", runs: [{ text: terminationClause }] },
      { type: "headline", level: "h3", text: "Liability" },
      { type: "paragraph", runs: [{ text: liabilityLimitation }] },
      { type: "headline", level: "h3", text: "Data Handling" },
      { type: "paragraph", runs: [{ text: dataHandling }] },
      { type: "headline", level: "h3", text: "Governing Law" },
      { type: "paragraph", runs: [{ text: governingLaw }] },
      { type: "headline", level: "h3", text: "Payment & Renewal" },
      { type: "paragraph", runs: [{ text: `${paymentTerms}. Auto-renewal: ${autoRenewal ? "Yes" : "No"}.` }] },
      { type: "separator" },
      { type: "paragraph", runs: [{ text: "This summary is for informational purposes only. Prepared by Whitmore Legal Advisors." }] },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract key clauses from T&C document
extraction = client.extract_document(
    files=[{"type": "url", "name": "vendor-terms-cloudpeak.pdf", "url": "https://example.com/contracts/vendor-terms-cloudpeak-2026.pdf"}],
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

document_title = extraction["document_title"]["value"]
effective_date = extraction["effective_date"]["value"]
termination_clause = extraction["termination_clause"]["value"]
liability_limitation = extraction["liability_limitation"]["value"]
data_handling = extraction["data_handling"]["value"]
governing_law = extraction["governing_law"]["value"]
payment_terms = extraction["payment_terms"]["value"]
auto_renewal = extraction["auto_renewal"]["value"]

# Step 2: Generate a plain-language summary PDF
summary = client.generate_document(
    format="pdf",
    document={
        "metadata": {"title": f"T&C Summary — {document_title}", "author": "Whitmore Legal Advisors"},
        "page": {"size": {"preset": "A4"}, "margins": {"top_in_pt": 54, "right_in_pt": 54, "bottom_in_pt": 54, "left_in_pt": 54}},
        "content": [
            {"type": "headline", "level": "h1", "text": "Terms & Conditions Summary"},
            {"type": "headline", "level": "h2", "text": document_title},
            {"type": "paragraph", "runs": [{"text": "Effective: ", "font_weight": "bold"}, {"text": effective_date}]},
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Termination"},
            {"type": "paragraph", "runs": [{"text": termination_clause}]},
            {"type": "headline", "level": "h3", "text": "Liability"},
            {"type": "paragraph", "runs": [{"text": liability_limitation}]},
            {"type": "headline", "level": "h3", "text": "Data Handling"},
            {"type": "paragraph", "runs": [{"text": data_handling}]},
            {"type": "headline", "level": "h3", "text": "Governing Law"},
            {"type": "paragraph", "runs": [{"text": governing_law}]},
            {"type": "headline", "level": "h3", "text": "Payment & Renewal"},
            {"type": "paragraph", "runs": [{"text": f"{payment_terms}. Auto-renewal: {'Yes' if auto_renewal else 'No'}."}]},
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": "This summary is for informational purposes only. Prepared by Whitmore Legal Advisors."}]},
        ],
    },
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

	// Step 1: Extract key clauses
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
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

	documentTitle := fmt.Sprintf("%v", (*extraction)["document_title"].Value)

	// Step 2: Generate a plain-language summary PDF
	_, err = client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{Title: "T&C Summary — " + documentTitle, Author: "Whitmore Legal Advisors"},
			Page: &il.DocumentPage{
				Size:    il.DocPageSize{Preset: "A4"},
				Margins: il.DocMargins{TopInPt: 54, RightInPt: 54, BottomInPt: 54, LeftInPt: 54},
			},
			Content: []il.ContentBlock{
				il.ContentBlock{"type": "headline", "level": "h1", "text": "Terms & Conditions Summary"},
				il.ContentBlock{"type": "headline", "level": "h2", "text": documentTitle},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Termination"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Either party may terminate with 90 days written notice."}}},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Liability"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Capped at 12 months of fees paid."}}},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Data Handling"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "GDPR data processor. EU-based data centers."}}},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Governing Law"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Laws of the Netherlands. Courts of Amsterdam."}}},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Prepared by Whitmore Legal Advisors."}}},
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
  "name": "Extract terms and generate summary in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Terms and Generate a Simplified Summary\n\nLegal agencies and compliance teams use this pipeline to process vendor or partner terms — extracting key clauses and generating a plain-language summary for non-legal stakeholders.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678901",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Terms\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678902",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Summary PDF\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [725, 100],
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678903",
      "name": "Step 2 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678904",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"document_title\",\"type\":\"TEXT\",\"description\":\"Title of the terms document\"},{\"name\":\"effective_date\",\"type\":\"DATE\",\"description\":\"Effective date\"},{\"name\":\"termination_clause\",\"type\":\"TEXTAREA\",\"description\":\"Termination conditions\"},{\"name\":\"liability_limitation\",\"type\":\"TEXTAREA\",\"description\":\"Liability limitation\"},{\"name\":\"data_handling\",\"type\":\"TEXTAREA\",\"description\":\"Data processing provisions\"},{\"name\":\"governing_law\",\"type\":\"TEXT\",\"description\":\"Governing law\"},{\"name\":\"payment_terms\",\"type\":\"TEXT\",\"description\":\"Payment terms\"},{\"name\":\"auto_renewal\",\"type\":\"BOOLEAN\",\"description\":\"Auto-renewal\"}]}",
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
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678905",
      "name": "Extract Terms",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": { \"title\": \"T&C Summary — {{ $json.document_title }}\", \"author\": \"Whitmore Legal Advisors\" },\n  \"page\": { \"size\": { \"preset\": \"A4\" }, \"margins\": { \"top_in_pt\": 54, \"right_in_pt\": 54, \"bottom_in_pt\": 54, \"left_in_pt\": 54 } },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Terms & Conditions Summary\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"{{ $json.document_title }}\" },\n    { \"type\": \"separator\" },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Termination\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.termination_clause }}\" }] },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Liability\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.liability_limitation }}\" }] },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Data Handling\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.data_handling }}\" }] },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Governing Law\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.governing_law }}\" }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Prepared by Whitmore Legal Advisors.\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [750, 300],
      "id": "a3b4c5d6-e7f8-9012-abcd-012345678906",
      "name": "Generate Summary PDF",
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
            "node": "Extract Terms",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Terms": {
      "main": [
        [
          {
            "node": "Generate Summary PDF",
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
Extract key clauses from the terms document at [file URL], then generate a plain-language summary. Use the extract_document tool with these fields:

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

Then use the generate_document tool to create a PDF summary with each clause explained in plain language, organized under clear headings.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "JVBERi0xLjQ...",
    "mime_type": "application/pdf"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
