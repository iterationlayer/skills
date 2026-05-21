---
name: extract-listing-and-generate-brochure
description: Extract property listing details from a real estate document, then generate a branded PDF brochure and a social media image for the listing.
---

# Extract Listing Data and Generate a Brochure

Real estate agencies use this pipeline to turn raw listing PDFs into polished marketing materials — a branded brochure PDF for print and a social media image for Instagram or Facebook — without manual design work.

## APIs Used

Document Extraction (1 credit per page), Document Generation (2 credits/request), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract listing data
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "listing-42-oakwood.pdf",
        "url": "https://example.com/listings/42-oakwood-drive.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "property_address",
          "type": "TEXT",
          "description": "Full street address of the property"
        },
        {
          "name": "city",
          "type": "TEXT",
          "description": "City or town"
        },
        {
          "name": "listing_price",
          "type": "CURRENCY_AMOUNT",
          "description": "Asking price"
        },
        {
          "name": "bedrooms",
          "type": "INTEGER",
          "description": "Number of bedrooms"
        },
        {
          "name": "bathrooms",
          "type": "DECIMAL",
          "description": "Number of bathrooms"
        },
        {
          "name": "square_footage",
          "type": "INTEGER",
          "description": "Living area in square feet"
        },
        {
          "name": "year_built",
          "type": "INTEGER",
          "description": "Year the property was built"
        },
        {
          "name": "description",
          "type": "TEXTAREA",
          "description": "Marketing description of the property"
        },
        {
          "name": "agent_name",
          "type": "TEXT",
          "description": "Listing agent name"
        },
        {
          "name": "agent_phone",
          "type": "TEXT",
          "description": "Listing agent phone number"
        }
      ]
    }
  }')

# Step 2: Generate a branded PDF brochure
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "42 Oakwood Drive — Property Brochure",
        "author": "Crestview Realty Group"
      },
      "page": {
        "size": {
          "preset": "A4"
        },
        "margins": {
          "top_in_pt": 48,
          "right_in_pt": 48,
          "bottom_in_pt": 48,
          "left_in_pt": 48
        }
      },
      "content": [
        {
          "type": "headline",
          "level": "h1",
          "text": "42 Oakwood Drive"
        },
        {
          "type": "headline",
          "level": "h2",
          "text": "Westfield, NJ — $875,000"
        },
        {
          "type": "separator"
        },
        {
          "type": "table",
          "header": {
            "cells": [
              {
                "text": "Bedrooms"
              },
              {
                "text": "Bathrooms"
              },
              {
                "text": "Sq Ft"
              },
              {
                "text": "Year Built"
              }
            ]
          },
          "rows": [
            {
              "cells": [
                {
                  "text": "4"
                },
                {
                  "text": "2.5"
                },
                {
                  "text": "2,850"
                },
                {
                  "text": "2003"
                }
              ]
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
              "text": "Nestled on a quiet tree-lined street, this impeccably maintained colonial features an open-concept living area, gourmet kitchen with granite countertops, and a sun-drenched family room opening to a landscaped backyard. The primary suite includes a walk-in closet and spa-inspired bath. Minutes from top-rated schools and downtown shops."
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
              "text": "Listed by: ",
              "font_weight": "bold"
            },
            {
              "text": "Sarah Mitchell — Crestview Realty Group\n"
            },
            {
              "text": "Phone: ",
              "font_weight": "bold"
            },
            {
              "text": "(908) 555-0142"
            }
          ]
        }
      ]
    }
  }'

# Step 3: Generate a social media image
curl -X POST https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": {
      "width_in_px": 1080,
      "height_in_px": 1080
    },
    "layers": [
      {
        "type": "solid-color",
        "index": 0,
        "hex_color": "#1B3A4B"
      },
      {
        "type": "text",
        "index": 1,
        "text": "JUST LISTED",
        "font_name": "Arial",
        "font_size_in_px": 32,
        "text_color": "#F0C040",
        "position": {
          "x_in_px": 60,
          "y_in_px": 60
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 50
        },
        "font_weight": "bold"
      },
      {
        "type": "text",
        "index": 2,
        "text": "42 Oakwood Drive",
        "font_name": "Arial",
        "font_size_in_px": 56,
        "text_color": "#FFFFFF",
        "position": {
          "x_in_px": 60,
          "y_in_px": 140
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 80
        },
        "font_weight": "bold"
      },
      {
        "type": "text",
        "index": 3,
        "text": "Westfield, NJ",
        "font_name": "Arial",
        "font_size_in_px": 28,
        "text_color": "#B0C4DE",
        "position": {
          "x_in_px": 60,
          "y_in_px": 240
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 40
        }
      },
      {
        "type": "text",
        "index": 4,
        "text": "$875,000",
        "font_name": "Arial",
        "font_size_in_px": 64,
        "text_color": "#FFFFFF",
        "position": {
          "x_in_px": 60,
          "y_in_px": 400
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 90
        },
        "font_weight": "bold"
      },
      {
        "type": "text",
        "index": 5,
        "text": "4 BD  |  2.5 BA  |  2,850 SQ FT",
        "font_name": "Arial",
        "font_size_in_px": 28,
        "text_color": "#FFFFFF",
        "position": {
          "x_in_px": 60,
          "y_in_px": 520
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 40
        }
      },
      {
        "type": "text",
        "index": 6,
        "text": "Sarah Mitchell — Crestview Realty Group\n(908) 555-0142",
        "font_name": "Arial",
        "font_size_in_px": 22,
        "text_color": "#B0C4DE",
        "position": {
          "x_in_px": 60,
          "y_in_px": 920
        },
        "dimensions": {
          "width_in_px": 960,
          "height_in_px": 100
        }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract listing data
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "listing-42-oakwood.pdf",
      url: "https://example.com/listings/42-oakwood-drive.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "property_address", type: "TEXT", description: "Full street address of the property" },
      { name: "city", type: "TEXT", description: "City or town" },
      { name: "listing_price", type: "CURRENCY_AMOUNT", description: "Asking price" },
      { name: "bedrooms", type: "INTEGER", description: "Number of bedrooms" },
      { name: "bathrooms", type: "DECIMAL", description: "Number of bathrooms" },
      { name: "square_footage", type: "INTEGER", description: "Living area in square feet" },
      { name: "year_built", type: "INTEGER", description: "Year the property was built" },
      { name: "description", type: "TEXTAREA", description: "Marketing description of the property" },
      { name: "agent_name", type: "TEXT", description: "Listing agent name" },
      { name: "agent_phone", type: "TEXT", description: "Listing agent phone number" },
    ],
  },
});

const propertyAddress = String(extraction["property_address"].value);
const city = String(extraction["city"].value);
const listingPrice = String(extraction["listing_price"].value);
const bedrooms = String(extraction["bedrooms"].value);
const bathrooms = String(extraction["bathrooms"].value);
const squareFootage = String(extraction["square_footage"].value);
const yearBuilt = String(extraction["year_built"].value);
const propertyDescription = String(extraction["description"].value);
const agentName = String(extraction["agent_name"].value);
const agentPhone = String(extraction["agent_phone"].value);

// Step 2: Generate a branded PDF brochure
const brochure = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: `${propertyAddress} — Property Brochure`,
      author: "Crestview Realty Group",
    },
    page: {
      size: { preset: "A4" },
      margins: { top_in_pt: 48, right_in_pt: 48, bottom_in_pt: 48, left_in_pt: 48 },
    },
    content: [
      { type: "headline", level: "h1", text: propertyAddress },
      { type: "headline", level: "h2", text: `${city} — ${listingPrice}` },
      { type: "separator" },
      {
        type: "table",
        header: {
          cells: [
            { text: "Bedrooms" },
            { text: "Bathrooms" },
            { text: "Sq Ft" },
            { text: "Year Built" },
          ],
        },
        rows: [
          {
            cells: [
              { text: bedrooms },
              { text: bathrooms },
              { text: squareFootage },
              { text: yearBuilt },
            ],
          },
        ],
      },
      { type: "separator" },
      { type: "paragraph", runs: [{ text: propertyDescription }] },
      { type: "separator" },
      {
        type: "paragraph",
        runs: [
          { text: "Listed by: ", font_weight: "bold" },
          { text: `${agentName} — Crestview Realty Group\n` },
          { text: "Phone: ", font_weight: "bold" },
          { text: agentPhone },
        ],
      },
    ],
  },
});

// Step 3: Generate a social media image
const socialImage = await client.generateImage({
  dimensions: { width_in_px: 1080, height_in_px: 1080 },
  layers: [
    { type: "solid-color", index: 0, hex_color: "#1B3A4B" },
    {
      type: "text",
      index: 1,
      text: "JUST LISTED",
      font_name: "Arial",
      font_size_in_px: 32,
      text_color: "#F0C040",
      position: { x_in_px: 60, y_in_px: 60 },
      dimensions: { width_in_px: 960, height_in_px: 50 },
      font_weight: "bold",
    },
    {
      type: "text",
      index: 2,
      text: propertyAddress,
      font_name: "Arial",
      font_size_in_px: 56,
      text_color: "#FFFFFF",
      position: { x_in_px: 60, y_in_px: 140 },
      dimensions: { width_in_px: 960, height_in_px: 80 },
      font_weight: "bold",
    },
    {
      type: "text",
      index: 3,
      text: city,
      font_name: "Arial",
      font_size_in_px: 28,
      text_color: "#B0C4DE",
      position: { x_in_px: 60, y_in_px: 240 },
      dimensions: { width_in_px: 960, height_in_px: 40 },
    },
    {
      type: "text",
      index: 4,
      text: listingPrice,
      font_name: "Arial",
      font_size_in_px: 64,
      text_color: "#FFFFFF",
      position: { x_in_px: 60, y_in_px: 400 },
      dimensions: { width_in_px: 960, height_in_px: 90 },
      font_weight: "bold",
    },
    {
      type: "text",
      index: 5,
      text: `${bedrooms} BD  |  ${bathrooms} BA  |  ${squareFootage} SQ FT`,
      font_name: "Arial",
      font_size_in_px: 28,
      text_color: "#FFFFFF",
      position: { x_in_px: 60, y_in_px: 520 },
      dimensions: { width_in_px: 960, height_in_px: 40 },
    },
    {
      type: "text",
      index: 6,
      text: `${agentName} — Crestview Realty Group\n${agentPhone}`,
      font_name: "Arial",
      font_size_in_px: 22,
      text_color: "#B0C4DE",
      position: { x_in_px: 60, y_in_px: 920 },
      dimensions: { width_in_px: 960, height_in_px: 100 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract listing data
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "listing-42-oakwood.pdf",
            "url": "https://example.com/listings/42-oakwood-drive.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "property_address", "type": "TEXT", "description": "Full street address of the property"},
            {"name": "city", "type": "TEXT", "description": "City or town"},
            {"name": "listing_price", "type": "CURRENCY_AMOUNT", "description": "Asking price"},
            {"name": "bedrooms", "type": "INTEGER", "description": "Number of bedrooms"},
            {"name": "bathrooms", "type": "DECIMAL", "description": "Number of bathrooms"},
            {"name": "square_footage", "type": "INTEGER", "description": "Living area in square feet"},
            {"name": "year_built", "type": "INTEGER", "description": "Year the property was built"},
            {"name": "description", "type": "TEXTAREA", "description": "Marketing description of the property"},
            {"name": "agent_name", "type": "TEXT", "description": "Listing agent name"},
            {"name": "agent_phone", "type": "TEXT", "description": "Listing agent phone number"},
        ]
    },
)

property_address = extraction["property_address"]["value"]
city = extraction["city"]["value"]
listing_price = extraction["listing_price"]["value"]
bedrooms = extraction["bedrooms"]["value"]
bathrooms = extraction["bathrooms"]["value"]
square_footage = extraction["square_footage"]["value"]
year_built = extraction["year_built"]["value"]
property_description = extraction["description"]["value"]
agent_name = extraction["agent_name"]["value"]
agent_phone = extraction["agent_phone"]["value"]

# Step 2: Generate a branded PDF brochure
brochure = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": f"{property_address} — Property Brochure",
            "author": "Crestview Realty Group",
        },
        "page": {
            "size": {"preset": "A4"},
            "margins": {"top_in_pt": 48, "right_in_pt": 48, "bottom_in_pt": 48, "left_in_pt": 48},
        },
        "content": [
            {"type": "headline", "level": "h1", "text": property_address},
            {"type": "headline", "level": "h2", "text": f"{city} — {listing_price}"},
            {"type": "separator"},
            {
                "type": "table",
                "header": {
                    "cells": [
                        {"text": "Bedrooms"},
                        {"text": "Bathrooms"},
                        {"text": "Sq Ft"},
                        {"text": "Year Built"},
                    ]
                },
                "rows": [
                    {
                        "cells": [
                            {"text": str(bedrooms)},
                            {"text": str(bathrooms)},
                            {"text": str(square_footage)},
                            {"text": str(year_built)},
                        ]
                    }
                ],
            },
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": property_description}]},
            {"type": "separator"},
            {
                "type": "paragraph",
                "runs": [
                    {"text": "Listed by: ", "font_weight": "bold"},
                    {"text": f"{agent_name} — Crestview Realty Group\n"},
                    {"text": "Phone: ", "font_weight": "bold"},
                    {"text": agent_phone},
                ],
            },
        ],
    },
)

# Step 3: Generate a social media image
social_image = client.generate_image(
    dimensions={"width_in_px": 1080, "height_in_px": 1080},
    layers=[
        {"type": "solid-color", "index": 0, "hex_color": "#1B3A4B"},
        {
            "type": "text",
            "index": 1,
            "text": "JUST LISTED",
            "font_name": "Arial",
            "font_size_in_px": 32,
            "text_color": "#F0C040",
            "position": {"x_in_px": 60, "y_in_px": 60},
            "dimensions": {"width_in_px": 960, "height_in_px": 50},
            "font_weight": "bold",
        },
        {
            "type": "text",
            "index": 2,
            "text": property_address,
            "font_name": "Arial",
            "font_size_in_px": 56,
            "text_color": "#FFFFFF",
            "position": {"x_in_px": 60, "y_in_px": 140},
            "dimensions": {"width_in_px": 960, "height_in_px": 80},
            "font_weight": "bold",
        },
        {
            "type": "text",
            "index": 3,
            "text": city,
            "font_name": "Arial",
            "font_size_in_px": 28,
            "text_color": "#B0C4DE",
            "position": {"x_in_px": 60, "y_in_px": 240},
            "dimensions": {"width_in_px": 960, "height_in_px": 40},
        },
        {
            "type": "text",
            "index": 4,
            "text": str(listing_price),
            "font_name": "Arial",
            "font_size_in_px": 64,
            "text_color": "#FFFFFF",
            "position": {"x_in_px": 60, "y_in_px": 400},
            "dimensions": {"width_in_px": 960, "height_in_px": 90},
            "font_weight": "bold",
        },
        {
            "type": "text",
            "index": 5,
            "text": f"{bedrooms} BD  |  {bathrooms} BA  |  {square_footage} SQ FT",
            "font_name": "Arial",
            "font_size_in_px": 28,
            "text_color": "#FFFFFF",
            "position": {"x_in_px": 60, "y_in_px": 520},
            "dimensions": {"width_in_px": 960, "height_in_px": 40},
        },
        {
            "type": "text",
            "index": 6,
            "text": f"{agent_name} — Crestview Realty Group\n{agent_phone}",
            "font_name": "Arial",
            "font_size_in_px": 22,
            "text_color": "#B0C4DE",
            "position": {"x_in_px": 60, "y_in_px": 920},
            "dimensions": {"width_in_px": 960, "height_in_px": 100},
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.FileInput{
				Type: "url",
				Name: "listing-42-oakwood.pdf",
				Url: "https://example.com/listings/42-oakwood-drive.pdf",
			},
		},
		Schema: il.ExtractionSchema{
			Fields: []any{
				il.TextFieldConfig{
					Name: "property_address",
					Type: "TEXT",
					Description: "Full street address of the property",
				},
				il.TextFieldConfig{
					Name: "city",
					Type: "TEXT",
					Description: "City or town",
				},
				il.CurrencyAmountFieldConfig{
					Name: "listing_price",
					Type: "CURRENCY_AMOUNT",
					Description: "Asking price",
				},
				il.IntegerFieldConfig{
					Name: "bedrooms",
					Type: "INTEGER",
					Description: "Number of bedrooms",
				},
				il.DecimalFieldConfig{
					Name: "bathrooms",
					Type: "DECIMAL",
					Description: "Number of bathrooms",
				},
				il.IntegerFieldConfig{
					Name: "square_footage",
					Type: "INTEGER",
					Description: "Living area in square feet",
				},
				il.IntegerFieldConfig{
					Name: "year_built",
					Type: "INTEGER",
					Description: "Year the property was built",
				},
				il.TextareaFieldConfig{
					Name: "description",
					Type: "TEXTAREA",
					Description: "Marketing description of the property",
				},
				il.TextFieldConfig{
					Name: "agent_name",
					Type: "TEXT",
					Description: "Listing agent name",
				},
				il.TextFieldConfig{
					Name: "agent_phone",
					Type: "TEXT",
					Description: "Listing agent phone number",
				},
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
  "name": "Extract Listing Data and Generate a Brochure",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Listing Data and Generate a Brochure\n\nReal estate agencies use this pipeline to turn raw listing PDFs into polished marketing materials — a branded brochure PDF for print and a social media image for Instagram or Facebook — without manual design work.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height_in_px": 280,
        "width_in_px": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        200,
        40
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456701",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Listing Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
        "height_in_px": 160,
        "width_in_px": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456702",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Brochure PDF\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height_in_px": 160,
        "width_in_px": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        725,
        100
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456703",
      "name": "Step 2 Note"
    },
    {
      "parameters": {
        "content": "### Step 3: Generate Social Image\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
        "height_in_px": 160,
        "width_in_px": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        975,
        100
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456704",
      "name": "Step 3 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456705",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"property_address\",\"type\":\"TEXT\",\"description\":\"Full street address of the property\"},{\"name\":\"city\",\"type\":\"TEXT\",\"description\":\"City or town\"},{\"name\":\"listing_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Asking price\"},{\"name\":\"bedrooms\",\"type\":\"INTEGER\",\"description\":\"Number of bedrooms\"},{\"name\":\"bathrooms\",\"type\":\"DECIMAL\",\"description\":\"Number of bathrooms\"},{\"name\":\"square_footage\",\"type\":\"INTEGER\",\"description\":\"Living area in square feet\"},{\"name\":\"year_built\",\"type\":\"INTEGER\",\"description\":\"Year the property was built\"},{\"name\":\"description\",\"type\":\"TEXTAREA\",\"description\":\"Marketing description of the property\"},{\"name\":\"agent_name\",\"type\":\"TEXT\",\"description\":\"Listing agent name\"},{\"name\":\"agent_phone\",\"type\":\"TEXT\",\"description\":\"Listing agent phone number\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "listing-42-oakwood.pdf",
              "fileUrl": "https://example.com/listings/42-oakwood-drive.pdf"
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
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456706",
      "name": "Extract Listing Data",
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
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"{{ $json.property_address }} — Property Brochure\",\n    \"author\": \"Crestview Realty Group\"\n  },\n  \"page\": {\n    \"size\": { \"preset\": \"A4\" },\n    \"margins\": { \"top_in_pt\": 48, \"right_in_pt\": 48, \"bottom_in_pt\": 48, \"left_in_pt\": 48 }\n  },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"{{ $json.property_address }}\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"{{ $json.city }} — {{ $json.listing_price }}\" },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"{{ $json.description }}\" }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Listed by: \", \"font_weight\": \"bold\" }, { \"text\": \"{{ $json.agent_name }} — Crestview Realty Group\\n\" }, { \"text\": \"Phone: \", \"font_weight\": \"bold\" }, { \"text\": \"{{ $json.agent_phone }}\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456707",
      "name": "Generate Brochure PDF",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "width_in_px": 1080,
        "height_in_px": 1080,
        "layersJson": "[\n  { \"type\": \"solid-color\", \"index\": 0, \"hex_color\": \"#1B3A4B\" },\n  { \"type\": \"text\", \"index\": 1, \"text\": \"JUST LISTED\", \"font_name\": \"Arial\", \"font_size_in_px\": 32, \"text_color\": \"#F0C040\", \"position\": { \"x\": 60, \"y\": 60 }, \"dimensions\": { \"width\": 960, \"height\": 50 }, \"font_weight\": \"bold\" },\n  { \"type\": \"text\", \"index\": 2, \"text\": \"{{ $json.property_address }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 56, \"text_color\": \"#FFFFFF\", \"position\": { \"x\": 60, \"y\": 140 }, \"dimensions\": { \"width\": 960, \"height\": 80 }, \"font_weight\": \"bold\" },\n  { \"type\": \"text\", \"index\": 3, \"text\": \"{{ $json.city }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 28, \"text_color\": \"#B0C4DE\", \"position\": { \"x\": 60, \"y\": 240 }, \"dimensions\": { \"width\": 960, \"height\": 40 } },\n  { \"type\": \"text\", \"index\": 4, \"text\": \"{{ $json.listing_price }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 64, \"text_color\": \"#FFFFFF\", \"position\": { \"x\": 60, \"y\": 400 }, \"dimensions\": { \"width\": 960, \"height\": 90 }, \"font_weight\": \"bold\" },\n  { \"type\": \"text\", \"index\": 5, \"text\": \"{{ $json.bedrooms }} BD  |  {{ $json.bathrooms }} BA  |  {{ $json.square_footage }} SQ FT\", \"font_name\": \"Arial\", \"font_size_in_px\": 28, \"text_color\": \"#FFFFFF\", \"position\": { \"x\": 60, \"y\": 520 }, \"dimensions\": { \"width\": 960, \"height\": 40 } },\n  { \"type\": \"text\", \"index\": 6, \"text\": \"{{ $json.agent_name }} — Crestview Realty Group\\n{{ $json.agent_phone }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 22, \"text_color\": \"#B0C4DE\", \"position\": { \"x\": 60, \"y\": 920 }, \"dimensions\": { \"width\": 960, \"height\": 100 } }\n]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        1000,
        300
      ],
      "id": "a1b2c3d4-e5f6-7890-abcd-ef0123456708",
      "name": "Generate Social Image",
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
            "node": "Extract Listing Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Listing Data": {
      "main": [
        [
          {
            "node": "Generate Brochure PDF",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate Brochure PDF": {
      "main": [
        [
          {
            "node": "Generate Social Image",
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
Extract real estate listing data from the file at [file URL], then generate a brochure and social media image. Use the extract_document tool with these fields:

- property_address (TEXT): Full street address of the property
- city (TEXT): City or town
- listing_price (CURRENCY_AMOUNT): Asking price
- bedrooms (INTEGER): Number of bedrooms
- bathrooms (DECIMAL): Number of bathrooms
- square_footage (INTEGER): Living area in square feet
- year_built (INTEGER): Year the property was built
- description (TEXTAREA): Marketing description of the property
- agent_name (TEXT): Listing agent name
- agent_phone (TEXT): Listing agent phone number

Then use the generate_document tool to create a branded A4 PDF brochure with the extracted property details, and the generate_image tool to create a 1080x1080 social media card with property highlights and agent contact info.
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
