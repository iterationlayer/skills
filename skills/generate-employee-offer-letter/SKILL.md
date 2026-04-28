---
name: generate-employee-offer-letter
description: Generate a professional offer letter PDF with role, compensation, start date, and company details.
---

# Generate Employee Offer Letter

HR departments generate personalized offer letters with role title, compensation details, start date, and company information — ready for candidate delivery.

## APIs Used

Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Offer Letter - Senior Software Engineer",
        "author": "Cascade Systems Inc."
      },
      "page": {
        "size": { "preset": "A4" },
        "margins": {
          "top_in_pt": 72,
          "right_in_pt": 72,
          "bottom_in_pt": 72,
          "left_in_pt": 72
        }
      },
      "styles": {
        "text": {
          "font_family": "Helvetica",
          "font_size_in_pt": 11.0,
          "line_height": 1.5,
          "color": "#333333"
        },
        "headline": {
          "font_family": "Helvetica",
          "font_size_in_pt": 24.0,
          "color": "#111111",
          "spacing_before_in_pt": 12.0,
          "spacing_after_in_pt": 6.0,
          "font_weight": "bold"
        },
        "link": {
          "color": "#0066CC",
          "is_underlined": true
        },
        "list": {
          "text_style": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11.0,
            "line_height": 1.5,
            "color": "#333333"
          },
          "marker_color": "#333333",
          "marker_gap_in_pt": 8.0
        },
        "table": {
          "header": {
            "background_color": "#333333",
            "text_color": "#FFFFFF",
            "font_size_in_pt": 11.0,
            "font_weight": "bold"
          },
          "body": {
            "background_color": "#FFFFFF",
            "text_color": "#333333",
            "font_size_in_pt": 11.0
          },
          "border": {
            "outer": {
              "top": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "right": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "bottom": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "left": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              }
            },
            "inner": {
              "horizontal": {
                "color": "#EEEEEE",
                "width_in_pt": 0.5
              },
              "vertical": {
                "color": "#EEEEEE",
                "width_in_pt": 0.5
              }
            }
          }
        },
        "grid": {
          "background_color": "#FFFFFF",
          "border_color": "#CCCCCC",
          "border_width_in_pt": 0.0,
          "gap_in_pt": 12.0
        },
        "separator": {
          "color": "#CCCCCC",
          "thickness_in_pt": 1.0,
          "spacing_before_in_pt": 12.0,
          "spacing_after_in_pt": 12.0
        },
        "image": {
          "border_color": "#000000",
          "border_width_in_pt": 0.0
        }
      },
      "footer": [
        {
          "type": "paragraph",
          "runs": [{
            "text": "Cascade Systems Inc. · 350 Spear Street, Suite 900, San Francisco, CA 94105 · cascadesystems.com"
          }]
        }
      ],
      "content": [
        {
            "type": "headline",
            "level": "h1",
            "text": "Cascade Systems Inc.",
        },
        { "type": "paragraph", "runs": [
          {
              "text": "March 14,
              2026",
          }
        ] },
        { "type": "paragraph", "runs": [
          {
              "text": "Dear Alex Morgan,
              ",
          }
        ] },
        { "type": "paragraph", "runs": [
          { "text": "We are pleased to offer you the position of " },
          {
              "text": "Senior Software Engineer",
              "font_weight": "bold",
          },
          { "text": " at Cascade Systems Inc. We were impressed by your experience and believe you will be a valuable addition to our engineering team. Below are the details of your offer." }
        ] },
        { "type": "paragraph", "runs": [
          {
              "text": "Role: ",
              "font_weight": "bold",
          },
          { "text": "Senior Software Engineer\n" },
          {
              "text": "Department: ",
              "font_weight": "bold",
          },
          { "text": "Platform Engineering\n" },
          {
              "text": "Annual Salary: ",
              "font_weight": "bold",
          },
          {
              "text": "$185,
              000.00\n",
          },
          {
              "text": "Equity Grant: ",
              "font_weight": "bold",
          },
          {
              "text": "15,
              000 stock options (4-year vesting, 1-year cliff)\n",
          },
          {
              "text": "Start Date: ",
              "font_weight": "bold",
          },
          {
              "text": "April 14,
              2026\n",
          },
          {
              "text": "Reporting To: ",
              "font_weight": "bold",
          },
          {
              "text": "Elena Vasquez,
              VP of Engineering",
          }
        ] },
        { "type": "separator" },
        {
            "type": "headline",
            "level": "h2",
            "text": "Benefits",
        },
        {
          "type": "list",
          "variant": "unordered",
          "items": [
            {
                "text": "Comprehensive health,
                dental,
                and vision insurance (100% employee, 75% dependents)",
            },
            { "text": "401(k) retirement plan with 4% company match" },
            { "text": "Unlimited paid time off with a 3-week minimum encouraged" },
            {
                "text": "$5,
                000 annual learning and development budget",
            },
            { "text": "Flexible remote work policy with quarterly team on-sites" },
            {
                "text": "$2,
                500 home office setup stipend",
            }
          ]
        },
        { "type": "separator" },
        { "type": "paragraph", "runs": [
          { "text": "This offer is contingent upon successful completion of a background check and is valid for 7 business days from the date of this letter. This letter is not a contract of employment and does not guarantee employment for any specific duration." }
        ] },
        { "type": "separator" },
        {
          "type": "grid",
          "columns": [
            {
              "column_span": 6,
              "blocks": [
                { "type": "paragraph", "runs": [
                  {
                      "text": "For Cascade Systems Inc.\n\n",
                      "font_weight": "bold",
                  },
                  { "text": "Signature: ________________________\n\n" },
                  {
                      "text": "Name: ",
                      "font_weight": "bold",
                  },
                  { "text": "Elena Vasquez\n" },
                  {
                      "text": "Title: ",
                      "font_weight": "bold",
                  },
                  { "text": "VP of Engineering" }
                ] }
              ]
            },
            {
              "column_span": 6,
              "blocks": [
                { "type": "paragraph", "runs": [
                  {
                      "text": "Accepted by Candidate\n\n",
                      "font_weight": "bold",
                  },
                  { "text": "Signature: ________________________\n\n" },
                  {
                      "text": "Name: ",
                      "font_weight": "bold",
                  },
                  { "text": "____________________\n" },
                  {
                      "text": "Date: ",
                      "font_weight": "bold",
                  },
                  { "text": "____________________" }
                ] }
              ]
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

const result = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: "Offer Letter - Senior Software Engineer",
      author: "Cascade Systems Inc.",
    },
    page: {
      size: { preset: "A4" },
      margins: {
        top_in_pt: 72,
        right_in_pt: 72,
        bottom_in_pt: 72,
        left_in_pt: 72,
      },
    },
    styles: {
      text: {
        font_family: "Helvetica",
        font_size_in_pt: 11.0,
        line_height: 1.5,
        color: "#333333",
      },
      headline: {
        font_family: "Helvetica",
        font_size_in_pt: 24.0,
        color: "#111111",
        spacing_before_in_pt: 12.0,
        spacing_after_in_pt: 6.0,
        font_weight: "bold",
      },
      link: {
        color: "#0066CC",
        is_underlined: true,
      },
      list: {
        text_style: {
          font_family: "Helvetica",
          font_size_in_pt: 11.0,
          line_height: 1.5,
          color: "#333333",
        },
        marker_color: "#333333",
        marker_gap_in_pt: 8.0,
      },
      table: {
        header: {
          background_color: "#333333",
          text_color: "#FFFFFF",
          font_size_in_pt: 11.0,
          font_weight: "bold",
        },
        body: {
          background_color: "#FFFFFF",
          text_color: "#333333",
          font_size_in_pt: 11.0,
        },
        border: {
          outer: {
            top: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            right: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            bottom: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            left: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
          },
          inner: {
            horizontal: {
              color: "#EEEEEE",
              width_in_pt: 0.5,
            },
            vertical: {
              color: "#EEEEEE",
              width_in_pt: 0.5,
            },
          },
        },
      },
      grid: {
        background_color: "#FFFFFF",
        border_color: "#CCCCCC",
        border_width_in_pt: 0.0,
        gap_in_pt: 12.0,
      },
      separator: {
        color: "#CCCCCC",
        thickness_in_pt: 1.0,
        spacing_before_in_pt: 12.0,
        spacing_after_in_pt: 12.0,
      },
      image: {
        border_color: "#000000",
        border_width_in_pt: 0.0,
      },
    },
    footer: [
      {
        type: "paragraph",
        runs: [{
          text: "Cascade Systems Inc. · 350 Spear Street, Suite 900, San Francisco, CA 94105 · cascadesystems.com",
        }],
      },
    ],
    content: [
      {
          type: "headline",
          level: "h1",
          text: "Cascade Systems Inc.",
      },
      { type: "paragraph", runs: [
        {
            text: "March 14,
            2026",
        },
      ] },
      { type: "paragraph", runs: [
        {
            text: "Dear Alex Morgan,
            ",
        },
      ] },
      { type: "paragraph", runs: [
        { text: "We are pleased to offer you the position of " },
        {
            text: "Senior Software Engineer",
            font_weight: "bold",
        },
        { text: " at Cascade Systems Inc. We were impressed by your experience and believe you will be a valuable addition to our engineering team. Below are the details of your offer." },
      ] },
      { type: "paragraph", runs: [
        {
            text: "Role: ",
            font_weight: "bold",
        },
        { text: "Senior Software Engineer\n" },
        {
            text: "Department: ",
            font_weight: "bold",
        },
        { text: "Platform Engineering\n" },
        {
            text: "Annual Salary: ",
            font_weight: "bold",
        },
        {
            text: "$185,
            000.00\n",
        },
        {
            text: "Equity Grant: ",
            font_weight: "bold",
        },
        {
            text: "15,
            000 stock options (4-year vesting, 1-year cliff)\n",
        },
        {
            text: "Start Date: ",
            font_weight: "bold",
        },
        {
            text: "April 14,
            2026\n",
        },
        {
            text: "Reporting To: ",
            font_weight: "bold",
        },
        {
            text: "Elena Vasquez,
            VP of Engineering",
        },
      ] },
      { type: "separator" },
      {
          type: "headline",
          level: "h2",
          text: "Benefits",
      },
      {
        type: "list",
        variant: "unordered",
        items: [
          {
              text: "Comprehensive health,
              dental,
              and vision insurance (100% employee, 75% dependents)",
          },
          { text: "401(k) retirement plan with 4% company match" },
          { text: "Unlimited paid time off with a 3-week minimum encouraged" },
          {
              text: "$5,
              000 annual learning and development budget",
          },
          { text: "Flexible remote work policy with quarterly team on-sites" },
          {
              text: "$2,
              500 home office setup stipend",
          },
        ],
      },
      { type: "separator" },
      { type: "paragraph", runs: [
        { text: "This offer is contingent upon successful completion of a background check and is valid for 7 business days from the date of this letter. This letter is not a contract of employment and does not guarantee employment for any specific duration." },
      ] },
      { type: "separator" },
      {
        type: "grid",
        columns: [
          {
            column_span: 6,
            blocks: [
              { type: "paragraph", runs: [
                {
                    text: "For Cascade Systems Inc.\n\n",
                    font_weight: "bold",
                },
                { text: "Signature: ________________________\n\n" },
                {
                    text: "Name: ",
                    font_weight: "bold",
                },
                { text: "Elena Vasquez\n" },
                {
                    text: "Title: ",
                    font_weight: "bold",
                },
                { text: "VP of Engineering" },
              ] },
            ],
          },
          {
            column_span: 6,
            blocks: [
              { type: "paragraph", runs: [
                {
                    text: "Accepted by Candidate\n\n",
                    font_weight: "bold",
                },
                { text: "Signature: ________________________\n\n" },
                {
                    text: "Name: ",
                    font_weight: "bold",
                },
                { text: "____________________\n" },
                {
                    text: "Date: ",
                    font_weight: "bold",
                },
                { text: "____________________" },
              ] },
            ],
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

result = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": "Offer Letter - Senior Software Engineer",
            "author": "Cascade Systems Inc.",
        },
        "page": {
            "size": {"preset": "A4"},
            "margins": {
                "top_in_pt": 72,
                "right_in_pt": 72,
                "bottom_in_pt": 72,
                "left_in_pt": 72,
            },
        },
        "styles": {
            "text": {
                "font_family": "Helvetica",
                "font_size_in_pt": 11.0,
                "line_height": 1.5,
                "color": "#333333",
            },
            "headline": {
                "font_family": "Helvetica",
                "font_size_in_pt": 24.0,
                "color": "#111111",
                "spacing_before_in_pt": 12.0,
                "spacing_after_in_pt": 6.0,
                "font_weight": "bold",
            },
            "link": {
                "color": "#0066CC",
                "is_underlined": True,
            },
            "list": {
                "text_style": {
                    "font_family": "Helvetica",
                    "font_size_in_pt": 11.0,
                    "line_height": 1.5,
                    "color": "#333333",
                },
                "marker_color": "#333333",
                "marker_gap_in_pt": 8.0,
            },
            "table": {
                "header": {
                    "background_color": "#333333",
                    "text_color": "#FFFFFF",
                    "font_size_in_pt": 11.0,
                    "font_weight": "bold",
                },
                "body": {
                    "background_color": "#FFFFFF",
                    "text_color": "#333333",
                    "font_size_in_pt": 11.0,
                },
                "border": {
                    "outer": {
                        "top": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "right": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "bottom": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "left": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                    },
                    "inner": {
                        "horizontal": {
                            "color": "#EEEEEE",
                            "width_in_pt": 0.5,
                        },
                        "vertical": {
                            "color": "#EEEEEE",
                            "width_in_pt": 0.5,
                        },
                    },
                },
            },
            "grid": {
                "background_color": "#FFFFFF",
                "border_color": "#CCCCCC",
                "border_width_in_pt": 0.0,
                "gap_in_pt": 12.0,
            },
            "separator": {
                "color": "#CCCCCC",
                "thickness_in_pt": 1.0,
                "spacing_before_in_pt": 12.0,
                "spacing_after_in_pt": 12.0,
            },
            "image": {
                "border_color": "#000000",
                "border_width_in_pt": 0.0,
            },
        },
        "footer": [
            {
                "type": "paragraph",
                "runs": [{
                    "text": "Cascade Systems Inc. · 350 Spear Street, Suite 900, San Francisco, CA 94105 · cascadesystems.com",
                }],
            },
        ],
        "content": [
            {
                "type": "headline",
                "level": "h1",
                "text": "Cascade Systems Inc.",
            },
            {"type": "paragraph", "runs": [
                {
                    "text": "March 14,
                    2026",
                },
            ]},
            {"type": "paragraph", "runs": [
                {
                    "text": "Dear Alex Morgan,
                    ",
                },
            ]},
            {"type": "paragraph", "runs": [
                {"text": "We are pleased to offer you the position of "},
                {
                    "text": "Senior Software Engineer",
                    "font_weight": "bold",
                },
                {"text": " at Cascade Systems Inc. We were impressed by your experience and believe you will be a valuable addition to our engineering team. Below are the details of your offer."},
            ]},
            {"type": "paragraph", "runs": [
                {
                    "text": "Role: ",
                    "font_weight": "bold",
                },
                {"text": "Senior Software Engineer\n"},
                {
                    "text": "Department: ",
                    "font_weight": "bold",
                },
                {"text": "Platform Engineering\n"},
                {
                    "text": "Annual Salary: ",
                    "font_weight": "bold",
                },
                {
                    "text": "$185,
                    000.00\n",
                },
                {
                    "text": "Equity Grant: ",
                    "font_weight": "bold",
                },
                {
                    "text": "15,
                    000 stock options (4-year vesting, 1-year cliff)\n",
                },
                {
                    "text": "Start Date: ",
                    "font_weight": "bold",
                },
                {
                    "text": "April 14,
                    2026\n",
                },
                {
                    "text": "Reporting To: ",
                    "font_weight": "bold",
                },
                {
                    "text": "Elena Vasquez,
                    VP of Engineering",
                },
            ]},
            {"type": "separator"},
            {
                "type": "headline",
                "level": "h2",
                "text": "Benefits",
            },
            {
                "type": "list",
                "variant": "unordered",
                "items": [
                    {
                        "text": "Comprehensive health,
                        dental,
                        and vision insurance (100% employee, 75% dependents)",
                    },
                    {"text": "401(k) retirement plan with 4% company match"},
                    {"text": "Unlimited paid time off with a 3-week minimum encouraged"},
                    {
                        "text": "$5,
                        000 annual learning and development budget",
                    },
                    {"text": "Flexible remote work policy with quarterly team on-sites"},
                    {
                        "text": "$2,
                        500 home office setup stipend",
                    },
                ],
            },
            {"type": "separator"},
            {"type": "paragraph", "runs": [
                {"text": "This offer is contingent upon successful completion of a background check and is valid for 7 business days from the date of this letter. This letter is not a contract of employment and does not guarantee employment for any specific duration."},
            ]},
            {"type": "separator"},
            {
                "type": "grid",
                "columns": [
                    {
                        "column_span": 6,
                        "blocks": [
                            {"type": "paragraph", "runs": [
                                {
                                    "text": "For Cascade Systems Inc.\n\n",
                                    "font_weight": "bold",
                                },
                                {"text": "Signature: ________________________\n\n"},
                                {
                                    "text": "Name: ",
                                    "font_weight": "bold",
                                },
                                {"text": "Elena Vasquez\n"},
                                {
                                    "text": "Title: ",
                                    "font_weight": "bold",
                                },
                                {"text": "VP of Engineering"},
                            ]},
                        ],
                    },
                    {
                        "column_span": 6,
                        "blocks": [
                            {"type": "paragraph", "runs": [
                                {
                                    "text": "Accepted by Candidate\n\n",
                                    "font_weight": "bold",
                                },
                                {"text": "Signature: ________________________\n\n"},
                                {
                                    "text": "Name: ",
                                    "font_weight": "bold",
                                },
                                {"text": "____________________\n"},
                                {
                                    "text": "Date: ",
                                    "font_weight": "bold",
                                },
                                {"text": "____________________"},
                            ]},
                        ],
                    },
                ],
            },
        ],
    },
)

```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{
				Title:  "Offer Letter - Senior Software Engineer",
				Author: "Cascade Systems Inc.",
			},
			Page: il.PageConfig{
				Size:    il.PageSize{Preset: "A4"},
				Margins: il.Margins{
      TopInPt: 72,
      RightInPt: 72,
      BottomInPt: 72,
      LeftInPt: 72,
    },
			},
			Content: []il.ContentBlock{
				il.NewHeadlineBlock("h1", "Cascade Systems Inc."),
				il.ParagraphBlock{
      Type: "paragraph",
      Markdown: "March 14,
      2026",
    },
				il.ParagraphBlock{
      Type: "paragraph",
      Markdown: "Dear Alex Morgan,
      ",
    },
				il.ParagraphBlock{
      Type: "paragraph",
      Markdown: "We are pleased to offer you the position of **Senior Software Engineer** at Cascade Systems Inc.",
    },
				il.ParagraphBlock{Type: "paragraph", Runs: []il.TextRun{
					{
       Text: "Role: ",
       IsBold: true,
     },
					{Text: "Senior Software Engineer\n"},
					{
       Text: "Annual Salary: ",
       IsBold: true,
     },
					{
       Text: "$185,
       000.00\n",
     },
					{
       Text: "Start Date: ",
       IsBold: true,
     },
					{
       Text: "April 14,
       2026",
     },
				}},
				il.NewSeparatorBlock(),
				il.NewHeadlineBlock("h2", "Benefits"),
				il.ListBlock{Type: "list", Variant: "unordered", Items: []il.ListItem{
					{
       Text: "Comprehensive health,
       dental,
       and vision insurance",
     },
					{Text: "401(k) retirement plan with 4% company match"},
					{Text: "Unlimited paid time off"},
					{
       Text: "$5,
       000 annual learning and development budget",
     },
					{Text: "Flexible remote work policy"},
					{
       Text: "$2,
       500 home office setup stipend",
     },
				}},
				il.NewSeparatorBlock(),
				il.ParagraphBlock{
      Type: "paragraph",
      Markdown: "This offer is valid for 7 business days from the date of this letter.",
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
  "name": "Generate Employee Offer Letter",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Employee Offer Letter\n\nHR departments generate personalized offer letters with role title, compensation details, start date, and company information \u2014 ready for candidate delivery.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "34de3c71-52a0-42aa-bbe7-f14c3f5534bf",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Document\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
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
      "id": "2c91c097-3e73-44fb-b7d9-9d3c6fdf4577",
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
      "id": "f9bc6cbd-6641-4d75-b061-463ea9b9d9a3",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"Offer Letter - Senior Software Engineer\",\n    \"author\": \"Cascade Systems Inc.\"\n  },\n  \"page\": {\n    \"size\": {\n      \"preset\": \"A4\"\n    },\n    \"margins\": {\n      \"top_in_pt\": 72,\n      \"right_in_pt\": 72,\n      \"bottom_in_pt\": 72,\n      \"left_in_pt\": 72\n    }\n  },\n  \"styles\": {\n    \"text\": {\n      \"font_family\": \"Helvetica\",\n      \"font_size_in_pt\": 11.0,\n      \"line_height\": 1.5,\n      \"color\": \"#333333\"\n    },\n    \"headline\": {\n      \"font_family\": \"Helvetica\",\n      \"font_size_in_pt\": 24.0,\n      \"color\": \"#111111\",\n      \"spacing_before_in_pt\": 12.0,\n      \"spacing_after_in_pt\": 6.0,\n      \"font_weight\": \"bold\"\n    },\n    \"link\": {\n      \"color\": \"#0066CC\",\n      \"is_underlined\": true\n    },\n    \"list\": {\n      \"text_style\": {\n        \"font_family\": \"Helvetica\",\n        \"font_size_in_pt\": 11.0,\n        \"line_height\": 1.5,\n        \"color\": \"#333333\"\n      },\n      \"marker_color\": \"#333333\",\n      \"marker_gap_in_pt\": 8.0\n    },\n    \"table\": {\n      \"header\": {\n        \"background_color\": \"#333333\",\n        \"text_color\": \"#FFFFFF\",\n        \"font_size_in_pt\": 11.0,\n        \"font_weight\": \"bold\"\n      },\n      \"body\": {\n        \"background_color\": \"#FFFFFF\",\n        \"text_color\": \"#333333\",\n        \"font_size_in_pt\": 11.0\n      },\n      \"border\": {\n        \"outer\": {\n          \"top\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"right\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"bottom\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"left\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          }\n        },\n        \"inner\": {\n          \"horizontal\": {\n            \"color\": \"#EEEEEE\",\n            \"width_in_pt\": 0.5\n          },\n          \"vertical\": {\n            \"color\": \"#EEEEEE\",\n            \"width_in_pt\": 0.5\n          }\n        }\n      }\n    },\n    \"grid\": {\n      \"background_color\": \"#FFFFFF\",\n      \"border_color\": \"#CCCCCC\",\n      \"border_width_in_pt\": 0.0,\n      \"gap_in_pt\": 12.0\n    },\n    \"separator\": {\n      \"color\": \"#CCCCCC\",\n      \"thickness_in_pt\": 1.0,\n      \"spacing_before_in_pt\": 12.0,\n      \"spacing_after_in_pt\": 12.0\n    },\n    \"image\": {\n      \"border_color\": \"#000000\",\n      \"border_width_in_pt\": 0.0\n    }\n  },\n  \"footer\": [\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"Cascade Systems Inc. \\u00b7 350 Spear Street, Suite 900, San Francisco, CA 94105 \\u00b7 cascadesystems.com\"\n        }\n      ]\n    }\n  ],\n  \"content\": [\n    {\n      \"type\": \"headline\",\n      \"level\": \"h1\",\n      \"text\": \"Cascade Systems Inc.\"\n    },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"March 14,\\n              2026\"\n        }\n      ]\n    },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"Dear Alex Morgan,\\n              \"\n        }\n      ]\n    },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"We are pleased to offer you the position of \"\n        },\n        {\n          \"text\": \"Senior Software Engineer\",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \" at Cascade Systems Inc. We were impressed by your experience and believe you will be a valuable addition to our engineering team. Below are the details of your offer.\"\n        }\n      ]\n    },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"Role: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"Senior Software Engineer\\n\"\n        },\n        {\n          \"text\": \"Department: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"Platform Engineering\\n\"\n        },\n        {\n          \"text\": \"Annual Salary: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"$185,\\n              000.00\\n\"\n        },\n        {\n          \"text\": \"Equity Grant: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"15,\\n              000 stock options (4-year vesting, 1-year cliff)\\n\"\n        },\n        {\n          \"text\": \"Start Date: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"April 14,\\n              2026\\n\"\n        },\n        {\n          \"text\": \"Reporting To: \",\n          \"font_weight\": \"bold\"\n        },\n        {\n          \"text\": \"Elena Vasquez,\\n              VP of Engineering\"\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"headline\",\n      \"level\": \"h2\",\n      \"text\": \"Benefits\"\n    },\n    {\n      \"type\": \"list\",\n      \"variant\": \"unordered\",\n      \"items\": [\n        {\n          \"text\": \"Comprehensive health,\\n                dental,\\n                and vision insurance (100% employee, 75% dependents)\"\n        },\n        {\n          \"text\": \"401(k) retirement plan with 4% company match\"\n        },\n        {\n          \"text\": \"Unlimited paid time off with a 3-week minimum encouraged\"\n        },\n        {\n          \"text\": \"$5,\\n                000 annual learning and development budget\"\n        },\n        {\n          \"text\": \"Flexible remote work policy with quarterly team on-sites\"\n        },\n        {\n          \"text\": \"$2,\\n                500 home office setup stipend\"\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"This offer is contingent upon successful completion of a background check and is valid for 7 business days from the date of this letter. This letter is not a contract of employment and does not guarantee employment for any specific duration.\"\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"grid\",\n      \"columns\": [\n        {\n          \"column_span\": 6,\n          \"blocks\": [\n            {\n              \"type\": \"paragraph\",\n              \"runs\": [\n                {\n                  \"text\": \"For Cascade Systems Inc.\\n\\n\",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"Signature: ________________________\\n\\n\"\n                },\n                {\n                  \"text\": \"Name: \",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"Elena Vasquez\\n\"\n                },\n                {\n                  \"text\": \"Title: \",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"VP of Engineering\"\n                }\n              ]\n            }\n          ]\n        },\n        {\n          \"column_span\": 6,\n          \"blocks\": [\n            {\n              \"type\": \"paragraph\",\n              \"runs\": [\n                {\n                  \"text\": \"Accepted by Candidate\\n\\n\",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"Signature: ________________________\\n\\n\"\n                },\n                {\n                  \"text\": \"Name: \",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"____________________\\n\"\n                },\n                {\n                  \"text\": \"Date: \",\n                  \"font_weight\": \"bold\"\n                },\n                {\n                  \"text\": \"____________________\"\n                }\n              ]\n            }\n          ]\n        }\n      ]\n    }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "0649ee20-56d7-4bd4-86b5-2880505569b6",
      "name": "Generate Document",
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
            "node": "Generate Document",
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
Generate an offer letter PDF for [candidate name]. Use the generate_document tool with format "pdf" and these content blocks:

- Headline: [company name]
- Paragraph: company address and date
- Headline: "Offer of Employment"
- Paragraph: role title, department, reporting manager, and start date
- Headline: "Compensation" with salary, equity, and benefits details
- Paragraph: acceptance instructions and deadline
- Paragraph: signature lines for hiring manager and candidate
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "JVBERi0xLjcKMSAwIG9iago8...",
    "mime_type": "application/pdf"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
