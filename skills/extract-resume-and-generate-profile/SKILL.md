---
name: extract-resume-and-generate-profile
description: Extract candidate information from a resume PDF, then generate a formatted employee profile document for HR onboarding.
---

# Extract Resume Data and Generate an Employee Profile

Staffing agencies and HR departments use this pipeline to accelerate onboarding — extracting key details from a candidate's resume and generating a standardized employee profile document ready for internal systems.

## APIs Used

Document Extraction (1 credit per page), Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract resume data
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "resume-elena-vasquez.pdf",
        "url": "https://example.com/resumes/elena-vasquez-2026.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "full_name",
          "type": "TEXT",
          "description": "Candidate full name"
        },
        {
          "name": "email",
          "type": "EMAIL",
          "description": "Email address"
        },
        {
          "name": "phone",
          "type": "TEXT",
          "description": "Phone number"
        },
        {
          "name": "address",
          "type": "ADDRESS",
          "description": "Home address"
        },
        {
          "name": "job_title",
          "type": "TEXT",
          "description": "Most recent job title"
        },
        {
          "name": "employer",
          "type": "TEXT",
          "description": "Most recent employer"
        },
        {
          "name": "years_of_experience",
          "type": "INTEGER",
          "description": "Total years of professional experience"
        },
        {
          "name": "education",
          "type": "TEXT",
          "description": "Highest degree and institution"
        },
        {
          "name": "skills",
          "type": "TEXTAREA",
          "description": "Key skills listed on the resume"
        }
      ]
    }
  }')

# Step 2: Generate an employee profile document
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Employee Profile — Elena Vasquez",
        "author": "Apex Staffing Solutions"
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
          "text": "Employee Profile"
        },
        {
          "type": "headline",
          "level": "h2",
          "text": "Elena Vasquez"
        },
        {
          "type": "separator"
        },
        {
          "type": "table",
          "header": {
            "cells": [
              {
                "text": "Field"
              },
              {
                "text": "Details"
              }
            ]
          },
          "rows": [
            {
              "cells": [
                {
                  "text": "Email"
                },
                {
                  "text": "elena.vasquez@email.com"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Phone"
                },
                {
                  "text": "+1 (512) 555-0198"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Address"
                },
                {
                  "text": "814 Congress Ave, Austin, TX 78701"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Current Title"
                },
                {
                  "text": "Senior Product Manager"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Current Employer"
                },
                {
                  "text": "NovaTech Solutions"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Experience"
                },
                {
                  "text": "8 years"
                }
              ]
            },
            {
              "cells": [
                {
                  "text": "Education"
                },
                {
                  "text": "MBA, University of Texas at Austin"
                }
              ]
            }
          ]
        },
        {
          "type": "separator"
        },
        {
          "type": "headline",
          "level": "h3",
          "text": "Key Skills"
        },
        {
          "type": "paragraph",
          "runs": [
            {
              "text": "Product roadmap planning, Agile/Scrum methodology, cross-functional team leadership, data-driven decision making, user research, A/B testing, SQL, stakeholder management"
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
              "text": "Prepared by Apex Staffing Solutions — Confidential"
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

// Step 1: Extract resume data
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "resume-elena-vasquez.pdf",
      url: "https://example.com/resumes/elena-vasquez-2026.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "full_name", type: "TEXT", description: "Candidate full name" },
      { name: "email", type: "EMAIL", description: "Email address" },
      { name: "phone", type: "TEXT", description: "Phone number" },
      { name: "address", type: "ADDRESS", description: "Home address" },
      { name: "job_title", type: "TEXT", description: "Most recent job title" },
      { name: "employer", type: "TEXT", description: "Most recent employer" },
      { name: "years_of_experience", type: "INTEGER", description: "Total years of professional experience" },
      { name: "education", type: "TEXT", description: "Highest degree and institution" },
      { name: "skills", type: "TEXTAREA", description: "Key skills listed on the resume" },
    ],
  },
});

const fullName = String(extraction["full_name"].value);
const email = String(extraction["email"].value);
const phone = String(extraction["phone"].value);
const address = String(extraction["address"].value);
const jobTitle = String(extraction["job_title"].value);
const employer = String(extraction["employer"].value);
const yearsOfExperience = String(extraction["years_of_experience"].value);
const education = String(extraction["education"].value);
const skills = String(extraction["skills"].value);

// Step 2: Generate an employee profile document
const profile = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: `Employee Profile — ${fullName}`,
      author: "Apex Staffing Solutions",
    },
    page: {
      size: { preset: "A4" },
      margins: { top_in_pt: 54, right_in_pt: 54, bottom_in_pt: 54, left_in_pt: 54 },
    },
    content: [
      { type: "headline", level: "h1", text: "Employee Profile" },
      { type: "headline", level: "h2", text: fullName },
      { type: "separator" },
      {
        type: "table",
        header: { cells: [{ text: "Field" }, { text: "Details" }] },
        rows: [
          { cells: [{ text: "Email" }, { text: email }] },
          { cells: [{ text: "Phone" }, { text: phone }] },
          { cells: [{ text: "Address" }, { text: address }] },
          { cells: [{ text: "Current Title" }, { text: jobTitle }] },
          { cells: [{ text: "Current Employer" }, { text: employer }] },
          { cells: [{ text: "Experience" }, { text: `${yearsOfExperience} years` }] },
          { cells: [{ text: "Education" }, { text: education }] },
        ],
      },
      { type: "separator" },
      { type: "headline", level: "h3", text: "Key Skills" },
      { type: "paragraph", runs: [{ text: skills }] },
      { type: "separator" },
      { type: "paragraph", runs: [{ text: "Prepared by Apex Staffing Solutions — Confidential" }] },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract resume data
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "resume-elena-vasquez.pdf",
            "url": "https://example.com/resumes/elena-vasquez-2026.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "full_name", "type": "TEXT", "description": "Candidate full name"},
            {"name": "email", "type": "EMAIL", "description": "Email address"},
            {"name": "phone", "type": "TEXT", "description": "Phone number"},
            {"name": "address", "type": "ADDRESS", "description": "Home address"},
            {"name": "job_title", "type": "TEXT", "description": "Most recent job title"},
            {"name": "employer", "type": "TEXT", "description": "Most recent employer"},
            {"name": "years_of_experience", "type": "INTEGER", "description": "Total years of professional experience"},
            {"name": "education", "type": "TEXT", "description": "Highest degree and institution"},
            {"name": "skills", "type": "TEXTAREA", "description": "Key skills listed on the resume"},
        ]
    },
)

full_name = extraction["full_name"]["value"]
email = extraction["email"]["value"]
phone = extraction["phone"]["value"]
address = extraction["address"]["value"]
job_title = extraction["job_title"]["value"]
employer = extraction["employer"]["value"]
years_of_experience = extraction["years_of_experience"]["value"]
education = extraction["education"]["value"]
skills = extraction["skills"]["value"]

# Step 2: Generate an employee profile document
profile = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": f"Employee Profile — {full_name}",
            "author": "Apex Staffing Solutions",
        },
        "page": {
            "size": {"preset": "A4"},
            "margins": {"top_in_pt": 54, "right_in_pt": 54, "bottom_in_pt": 54, "left_in_pt": 54},
        },
        "content": [
            {"type": "headline", "level": "h1", "text": "Employee Profile"},
            {"type": "headline", "level": "h2", "text": full_name},
            {"type": "separator"},
            {
                "type": "table",
                "header": {"cells": [{"text": "Field"}, {"text": "Details"}]},
                "rows": [
                    {"cells": [{"text": "Email"}, {"text": email}]},
                    {"cells": [{"text": "Phone"}, {"text": phone}]},
                    {"cells": [{"text": "Address"}, {"text": str(address)}]},
                    {"cells": [{"text": "Current Title"}, {"text": job_title}]},
                    {"cells": [{"text": "Current Employer"}, {"text": employer}]},
                    {"cells": [{"text": "Experience"}, {"text": f"{years_of_experience} years"}]},
                    {"cells": [{"text": "Education"}, {"text": education}]},
                ],
            },
            {"type": "separator"},
            {"type": "headline", "level": "h3", "text": "Key Skills"},
            {"type": "paragraph", "runs": [{"text": skills}]},
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": "Prepared by Apex Staffing Solutions — Confidential"}]},
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

	// Step 1: Extract resume data
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("resume-elena-vasquez.pdf", "https://example.com/resumes/elena-vasquez-2026.pdf"),
		},
		Schema: il.ExtractionSchema{
			"full_name":             il.NewTextFieldConfig("full_name", "Candidate full name"),
			"email":                 il.NewEmailFieldConfig("email", "Email address"),
			"phone":                 il.NewTextFieldConfig("phone", "Phone number"),
			"address":               il.NewAddressFieldConfig("address", "Home address"),
			"job_title":             il.NewTextFieldConfig("job_title", "Most recent job title"),
			"employer":              il.NewTextFieldConfig("employer", "Most recent employer"),
			"years_of_experience":   il.NewIntegerFieldConfig("years_of_experience", "Total years of professional experience"),
			"education":             il.NewTextFieldConfig("education", "Highest degree and institution"),
			"skills":                il.NewTextareaFieldConfig("skills", "Key skills listed on the resume"),
		},
	})
	if err != nil {
		panic(err)
	}

	fullName := fmt.Sprintf("%v", (*extraction)["full_name"].Value)

	// Step 2: Generate an employee profile document
	_, err = client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{
				Title:  "Employee Profile — " + fullName,
				Author: "Apex Staffing Solutions",
			},
			Page: &il.DocumentPage{
				Size:    il.DocPageSize{Preset: "A4"},
				Margins: il.DocMargins{TopInPt: 54, RightInPt: 54, BottomInPt: 54, LeftInPt: 54},
			},
			Content: []il.ContentBlock{
				il.ContentBlock{"type": "headline", "level": "h1", "text": "Employee Profile"},
				il.ContentBlock{"type": "headline", "level": "h2", "text": fullName},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{
					"type":   "table",
					"header": map[string]any{"cells": []map[string]string{{"text": "Field"}, {"text": "Details"}}},
					"rows": []map[string]any{
						{"cells": []map[string]string{{"text": "Email"}, {"text": "elena.vasquez@email.com"}}},
						{"cells": []map[string]string{{"text": "Phone"}, {"text": "+1 (512) 555-0198"}}},
						{"cells": []map[string]string{{"text": "Address"}, {"text": "814 Congress Ave, Austin, TX 78701"}}},
						{"cells": []map[string]string{{"text": "Current Title"}, {"text": "Senior Product Manager"}}},
						{"cells": []map[string]string{{"text": "Current Employer"}, {"text": "NovaTech Solutions"}}},
						{"cells": []map[string]string{{"text": "Experience"}, {"text": "8 years"}}},
						{"cells": []map[string]string{{"text": "Education"}, {"text": "MBA, University of Texas at Austin"}}},
					},
				},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "headline", "level": "h3", "text": "Key Skills"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Product roadmap planning, Agile/Scrum methodology, cross-functional team leadership, data-driven decision making, user research, A/B testing, SQL, stakeholder management"}}},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Prepared by Apex Staffing Solutions — Confidential"}}},
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
  "name": "Extract resume data and generate employee profile in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Resume Data and Generate an Employee Profile\n\nStaffing agencies and HR departments use this pipeline to accelerate onboarding — extracting key details from a candidate's resume and generating a standardized employee profile document ready for internal systems.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [200, 40],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890101",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Resume Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [475, 100],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890102",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Employee Profile\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [725, 100],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890103",
      "name": "Step 2 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [250, 300],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890104",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"full_name\",\"type\":\"TEXT\",\"description\":\"Candidate full name\"},{\"name\":\"email\",\"type\":\"EMAIL\",\"description\":\"Email address\"},{\"name\":\"phone\",\"type\":\"TEXT\",\"description\":\"Phone number\"},{\"name\":\"address\",\"type\":\"ADDRESS\",\"description\":\"Home address\"},{\"name\":\"job_title\",\"type\":\"TEXT\",\"description\":\"Most recent job title\"},{\"name\":\"employer\",\"type\":\"TEXT\",\"description\":\"Most recent employer\"},{\"name\":\"years_of_experience\",\"type\":\"INTEGER\",\"description\":\"Total years of professional experience\"},{\"name\":\"education\",\"type\":\"TEXT\",\"description\":\"Highest degree and institution\"},{\"name\":\"skills\",\"type\":\"TEXTAREA\",\"description\":\"Key skills listed on the resume\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "resume-elena-vasquez.pdf",
              "fileUrl": "https://example.com/resumes/elena-vasquez-2026.pdf"
            }
          ]
        }
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [500, 300],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890105",
      "name": "Extract Resume Data",
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
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"Employee Profile — {{ $json.full_name }}\",\n    \"author\": \"Apex Staffing Solutions\"\n  },\n  \"page\": {\n    \"size\": { \"preset\": \"A4\" },\n    \"margins\": { \"top_in_pt\": 54, \"right_in_pt\": 54, \"bottom_in_pt\": 54, \"left_in_pt\": 54 }\n  },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Employee Profile\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"{{ $json.full_name }}\" },\n    { \"type\": \"separator\" },\n    { \"type\": \"table\", \"header\": { \"cells\": [{ \"text\": \"Field\" }, { \"text\": \"Details\" }] }, \"rows\": [{ \"cells\": [{ \"text\": \"Email\" }, { \"text\": \"{{ $json.email }}\" }] }, { \"cells\": [{ \"text\": \"Phone\" }, { \"text\": \"{{ $json.phone }}\" }] }, { \"cells\": [{ \"text\": \"Current Title\" }, { \"text\": \"{{ $json.job_title }}\" }] }, { \"cells\": [{ \"text\": \"Current Employer\" }, { \"text\": \"{{ $json.employer }}\" }] }, { \"cells\": [{ \"text\": \"Experience\" }, { \"text\": \"{{ $json.years_of_experience }} years\" }] }, { \"cells\": [{ \"text\": \"Education\" }, { \"text\": \"{{ $json.education }}\" }] }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"headline\", \"level\": \"h3\", \"text\": \"Key Skills\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.skills }}\" }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Prepared by Apex Staffing Solutions — Confidential\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [750, 300],
      "id": "e5f6a7b8-c9d0-1234-efab-234567890106",
      "name": "Generate Employee Profile",
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
            "node": "Extract Resume Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Resume Data": {
      "main": [
        [
          {
            "node": "Generate Employee Profile",
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
Extract resume data from the file at [file URL], then generate an employee profile document. Use the extract_document tool with these fields:

- full_name (TEXT): Candidate full name
- email (EMAIL): Email address
- phone (TEXT): Phone number
- address (ADDRESS): Home address
- job_title (TEXT): Most recent job title
- employer (TEXT): Most recent employer
- years_of_experience (INTEGER): Total years of professional experience
- education (TEXT): Highest degree and institution
- skills (TEXTAREA): Key skills listed on the resume

Then use the generate_document tool to create a PDF employee profile with a summary table and skills section, branded for the staffing agency.
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
