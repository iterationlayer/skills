---
name: extract-product-data-and-generate-listing-image
description: Extract product details from a supplier data sheet, then generate a branded e-commerce listing image with the product name, price, and key specs.
---

# Extract Product Data and Generate a Listing Image

E-commerce agencies managing multiple storefronts use this pipeline to turn supplier data sheets into ready-to-publish listing images — extracting product specs and generating branded visuals without manual design rounds.

## APIs Used

Document Extraction (1 credit per page), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract product data from supplier sheet
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "supplier-datasheet.pdf",
        "url": "https://example.com/suppliers/alpine-gear-datasheet.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "product_name",
          "type": "TEXT",
          "description": "Product name or title"
        },
        {
          "name": "sku",
          "type": "TEXT",
          "description": "Product SKU or article number"
        },
        {
          "name": "retail_price",
          "type": "CURRENCY_AMOUNT",
          "description": "Recommended retail price"
        },
        {
          "name": "short_description",
          "type": "TEXT",
          "description": "One-line product description"
        },
        {
          "name": "material",
          "type": "TEXT",
          "description": "Primary material or fabric"
        },
        {
          "name": "weight",
          "type": "TEXT",
          "description": "Product weight with unit"
        },
        {
          "name": "available_colors",
          "type": "TEXT",
          "description": "Available color options"
        }
      ]
    }
  }')

# Step 2: Generate a listing image
curl -X POST https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": {
      "width_in_px": 1200,
      "height_in_px": 1200
    },
    "layers": [
      {
        "type": "solid-color",
        "index": 0,
        "hex_color": "#FFFFFF"
      },
      {
        "type": "solid-color",
        "index": 1,
        "hex_color": "#0D1B2A",
        "position": {
          "x_in_px": 0,
          "y_in_px": 0
        },
        "dimensions": {
          "width_in_px": 1200,
          "height_in_px": 200
        }
      },
      {
        "type": "text",
        "index": 2,
        "text": "Alpine Trail Pro 45L Backpack",
        "font_name": "Arial",
        "font_size_in_px": 42,
        "text_color": "#FFFFFF",
        "position": {
          "x_in_px": 60,
          "y_in_px": 50
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 60
        },
        "font_weight": "bold"
      },
      {
        "type": "text",
        "index": 3,
        "text": "Lightweight ripstop nylon | 1.2 kg | Slate, Forest, Black",
        "font_name": "Arial",
        "font_size_in_px": 24,
        "text_color": "#B0BEC5",
        "position": {
          "x_in_px": 60,
          "y_in_px": 130
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 40
        }
      },
      {
        "type": "text",
        "index": 4,
        "text": "$189.00",
        "font_name": "Arial",
        "font_size_in_px": 64,
        "text_color": "#0D1B2A",
        "position": {
          "x_in_px": 60,
          "y_in_px": 900
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 90
        },
        "font_weight": "bold"
      },
      {
        "type": "text",
        "index": 5,
        "text": "SKU: APT-45L-SL",
        "font_name": "Arial",
        "font_size_in_px": 20,
        "text_color": "#78909C",
        "position": {
          "x_in_px": 60,
          "y_in_px": 1010
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 30
        }
      },
      {
        "type": "text",
        "index": 6,
        "text": "Durable, ultralight hiking pack with ventilated back panel",
        "font_name": "Arial",
        "font_size_in_px": 26,
        "text_color": "#37474F",
        "position": {
          "x_in_px": 60,
          "y_in_px": 1080
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 60
        }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract product data from supplier sheet
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "supplier-datasheet.pdf",
      url: "https://example.com/suppliers/alpine-gear-datasheet.pdf",
    },
  ],
  schema: {
    fields: [
      { name: "product_name", type: "TEXT", description: "Product name or title" },
      { name: "sku", type: "TEXT", description: "Product SKU or article number" },
      { name: "retail_price", type: "CURRENCY_AMOUNT", description: "Recommended retail price" },
      { name: "short_description", type: "TEXT", description: "One-line product description" },
      { name: "material", type: "TEXT", description: "Primary material or fabric" },
      { name: "weight", type: "TEXT", description: "Product weight with unit" },
      { name: "available_colors", type: "TEXT", description: "Available color options" },
    ],
  },
});

const productName = String(extraction["product_name"].value);
const sku = String(extraction["sku"].value);
const retailPrice = String(extraction["retail_price"].value);
const shortDescription = String(extraction["short_description"].value);
const material = String(extraction["material"].value);
const weight = String(extraction["weight"].value);
const availableColors = String(extraction["available_colors"].value);

// Step 2: Generate a listing image
const listingImage = await client.generateImage({
  dimensions: { width_in_px: 1200, height_in_px: 1200 },
  layers: [
    { type: "solid-color", index: 0, hex_color: "#FFFFFF" },
    {
      type: "solid-color",
      index: 1,
      hex_color: "#0D1B2A",
      position: { x_in_px: 0, y_in_px: 0 },
      dimensions: { width_in_px: 1200, height_in_px: 200 },
    },
    {
      type: "text",
      index: 2,
      text: productName,
      font_name: "Arial",
      font_size_in_px: 42,
      text_color: "#FFFFFF",
      position: { x_in_px: 60, y_in_px: 50 },
      dimensions: { width_in_px: 1080, height_in_px: 60 },
      font_weight: "bold",
    },
    {
      type: "text",
      index: 3,
      text: `${material} | ${weight} | ${availableColors}`,
      font_name: "Arial",
      font_size_in_px: 24,
      text_color: "#B0BEC5",
      position: { x_in_px: 60, y_in_px: 130 },
      dimensions: { width_in_px: 1080, height_in_px: 40 },
    },
    {
      type: "text",
      index: 4,
      text: retailPrice,
      font_name: "Arial",
      font_size_in_px: 64,
      text_color: "#0D1B2A",
      position: { x_in_px: 60, y_in_px: 900 },
      dimensions: { width_in_px: 1080, height_in_px: 90 },
      font_weight: "bold",
    },
    {
      type: "text",
      index: 5,
      text: `SKU: ${sku}`,
      font_name: "Arial",
      font_size_in_px: 20,
      text_color: "#78909C",
      position: { x_in_px: 60, y_in_px: 1010 },
      dimensions: { width_in_px: 1080, height_in_px: 30 },
    },
    {
      type: "text",
      index: 6,
      text: shortDescription,
      font_name: "Arial",
      font_size_in_px: 26,
      text_color: "#37474F",
      position: { x_in_px: 60, y_in_px: 1080 },
      dimensions: { width_in_px: 1080, height_in_px: 60 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract product data from supplier sheet
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "supplier-datasheet.pdf",
            "url": "https://example.com/suppliers/alpine-gear-datasheet.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "product_name", "type": "TEXT", "description": "Product name or title"},
            {"name": "sku", "type": "TEXT", "description": "Product SKU or article number"},
            {"name": "retail_price", "type": "CURRENCY_AMOUNT", "description": "Recommended retail price"},
            {"name": "short_description", "type": "TEXT", "description": "One-line product description"},
            {"name": "material", "type": "TEXT", "description": "Primary material or fabric"},
            {"name": "weight", "type": "TEXT", "description": "Product weight with unit"},
            {"name": "available_colors", "type": "TEXT", "description": "Available color options"},
        ]
    },
)

product_name = extraction["product_name"]["value"]
sku = extraction["sku"]["value"]
retail_price = extraction["retail_price"]["value"]
short_description = extraction["short_description"]["value"]
material = extraction["material"]["value"]
weight = extraction["weight"]["value"]
available_colors = extraction["available_colors"]["value"]

# Step 2: Generate a listing image
listing_image = client.generate_image(
    dimensions={"width_in_px": 1200, "height_in_px": 1200},
    layers=[
        {"type": "solid-color", "index": 0, "hex_color": "#FFFFFF"},
        {"type": "solid-color", "index": 1, "hex_color": "#0D1B2A", "position": {"x_in_px": 0, "y_in_px": 0}, "dimensions": {"width_in_px": 1200, "height_in_px": 200}},
        {
            "type": "text",
            "index": 2,
            "text": product_name,
            "font_name": "Arial",
            "font_size_in_px": 42,
            "text_color": "#FFFFFF",
            "position": {"x_in_px": 60, "y_in_px": 50},
            "dimensions": {"width_in_px": 1080, "height_in_px": 60},
            "font_weight": "bold",
        },
        {
            "type": "text",
            "index": 3,
            "text": f"{material} | {weight} | {available_colors}",
            "font_name": "Arial",
            "font_size_in_px": 24,
            "text_color": "#B0BEC5",
            "position": {"x_in_px": 60, "y_in_px": 130},
            "dimensions": {"width_in_px": 1080, "height_in_px": 40},
        },
        {
            "type": "text",
            "index": 4,
            "text": str(retail_price),
            "font_name": "Arial",
            "font_size_in_px": 64,
            "text_color": "#0D1B2A",
            "position": {"x_in_px": 60, "y_in_px": 900},
            "dimensions": {"width_in_px": 1080, "height_in_px": 90},
            "font_weight": "bold",
        },
        {
            "type": "text",
            "index": 5,
            "text": f"SKU: {sku}",
            "font_name": "Arial",
            "font_size_in_px": 20,
            "text_color": "#78909C",
            "position": {"x_in_px": 60, "y_in_px": 1010},
            "dimensions": {"width_in_px": 1080, "height_in_px": 30},
        },
        {
            "type": "text",
            "index": 6,
            "text": short_description,
            "font_name": "Arial",
            "font_size_in_px": 26,
            "text_color": "#37474F",
            "position": {"x_in_px": 60, "y_in_px": 1080},
            "dimensions": {"width_in_px": 1080, "height_in_px": 60},
        },
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

	// Step 1: Extract product data from supplier sheet
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("supplier-datasheet.pdf", "https://example.com/suppliers/alpine-gear-datasheet.pdf"),
		},
		Schema: il.ExtractionSchema{
			"product_name":      il.NewTextFieldConfig("product_name", "Product name or title"),
			"sku":               il.NewTextFieldConfig("sku", "Product SKU or article number"),
			"retail_price":      il.NewCurrencyAmountFieldConfig("retail_price", "Recommended retail price"),
			"short_description": il.NewTextFieldConfig("short_description", "One-line product description"),
			"material":          il.NewTextFieldConfig("material", "Primary material or fabric"),
			"weight":            il.NewTextFieldConfig("weight", "Product weight with unit"),
			"available_colors":  il.NewTextFieldConfig("available_colors", "Available color options"),
		},
	})
	if err != nil {
		panic(err)
	}

	productName := fmt.Sprintf("%v", (*extraction)["product_name"].Value)
	sku := fmt.Sprintf("%v", (*extraction)["sku"].Value)
	retailPrice := fmt.Sprintf("%v", (*extraction)["retail_price"].Value)
	shortDescription := fmt.Sprintf("%v", (*extraction)["short_description"].Value)
	material := fmt.Sprintf("%v", (*extraction)["material"].Value)
	weight := fmt.Sprintf("%v", (*extraction)["weight"].Value)
	availableColors := fmt.Sprintf("%v", (*extraction)["available_colors"].Value)

	// Step 2: Generate a listing image
	_, err = client.GenerateImage(il.GenerateImageRequest{
		Dimensions: il.Dimensions{WidthInPx: 1200, HeightInPx: 1200},
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#FFFFFF"),
			il.NewSolidColorLayer(1, "#0D1B2A", il.Position{XInPx: 0, YInPx: 0}, il.Dimensions{WidthInPx: 1200, HeightInPx: 200}),
			il.NewTextLayer(2, productName, "Arial", 42, "#FFFFFF", il.Position{XInPx: 60, YInPx: 50}, il.Dimensions{WidthInPx: 1080, HeightInPx: 60}),
			il.NewTextLayer(3, material+" | "+weight+" | "+availableColors, "Arial", 24, "#B0BEC5", il.Position{XInPx: 60, YInPx: 130}, il.Dimensions{WidthInPx: 1080, HeightInPx: 40}),
			il.NewTextLayer(4, retailPrice, "Arial", 64, "#0D1B2A", il.Position{XInPx: 60, YInPx: 900}, il.Dimensions{WidthInPx: 1080, HeightInPx: 90}),
			il.NewTextLayer(5, "SKU: "+sku, "Arial", 20, "#78909C", il.Position{XInPx: 60, YInPx: 1010}, il.Dimensions{WidthInPx: 1080, HeightInPx: 30}),
			il.NewTextLayer(6, shortDescription, "Arial", 26, "#37474F", il.Position{XInPx: 60, YInPx: 1080}, il.Dimensions{WidthInPx: 1080, HeightInPx: 60}),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Extract product data and generate listing image in Iteration Layer",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Product Data and Generate a Listing Image\n\nE-commerce agencies managing multiple storefronts use this pipeline to turn supplier data sheets into ready-to-publish listing images — extracting product specs and generating branded visuals without manual design rounds.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567801",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Product Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567802",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Listing Image\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567803",
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567804",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"product_name\",\"type\":\"TEXT\",\"description\":\"Product name or title\"},{\"name\":\"sku\",\"type\":\"TEXT\",\"description\":\"Product SKU or article number\"},{\"name\":\"retail_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Recommended retail price\"},{\"name\":\"short_description\",\"type\":\"TEXT\",\"description\":\"One-line product description\"},{\"name\":\"material\",\"type\":\"TEXT\",\"description\":\"Primary material or fabric\"},{\"name\":\"weight\",\"type\":\"TEXT\",\"description\":\"Product weight with unit\"},{\"name\":\"available_colors\",\"type\":\"TEXT\",\"description\":\"Available color options\"}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "supplier-datasheet.pdf",
              "fileUrl": "https://example.com/suppliers/alpine-gear-datasheet.pdf"
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
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567805",
      "name": "Extract Product Data",
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
        "width_in_px": 1200,
        "height_in_px": 1200,
        "layersJson": "[\n  { \"type\": \"solid-color\", \"index\": 0, \"hex_color\": \"#FFFFFF\" },\n  { \"type\": \"solid-color\", \"index\": 1, \"hex_color\": \"#0D1B2A\", \"position\": { \"x\": 0, \"y\": 0 }, \"dimensions\": { \"width\": 1200, \"height\": 200 } },\n  { \"type\": \"text\", \"index\": 2, \"text\": \"{{ $json.product_name }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 42, \"text_color\": \"#FFFFFF\", \"position\": { \"x\": 60, \"y\": 50 }, \"dimensions\": { \"width\": 1080, \"height\": 60 }, \"font_weight\": \"bold\" },\n  { \"type\": \"text\", \"index\": 3, \"text\": \"{{ $json.material }} | {{ $json.weight }} | {{ $json.available_colors }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 24, \"text_color\": \"#B0BEC5\", \"position\": { \"x\": 60, \"y\": 130 }, \"dimensions\": { \"width\": 1080, \"height\": 40 } },\n  { \"type\": \"text\", \"index\": 4, \"text\": \"{{ $json.retail_price }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 64, \"text_color\": \"#0D1B2A\", \"position\": { \"x\": 60, \"y\": 900 }, \"dimensions\": { \"width\": 1080, \"height\": 90 }, \"font_weight\": \"bold\" },\n  { \"type\": \"text\", \"index\": 5, \"text\": \"SKU: {{ $json.sku }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 20, \"text_color\": \"#78909C\", \"position\": { \"x\": 60, \"y\": 1010 }, \"dimensions\": { \"width\": 1080, \"height\": 30 } },\n  { \"type\": \"text\", \"index\": 6, \"text\": \"{{ $json.short_description }}\", \"font_name\": \"Arial\", \"font_size_in_px\": 26, \"text_color\": \"#37474F\", \"position\": { \"x\": 60, \"y\": 1080 }, \"dimensions\": { \"width\": 1080, \"height\": 60 } }\n]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "b2c3d4e5-f6a7-8901-bcde-f01234567806",
      "name": "Generate Listing Image",
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
            "node": "Extract Product Data",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Product Data": {
      "main": [
        [
          {
            "node": "Generate Listing Image",
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
Extract product data from the supplier sheet at [file URL], then generate a listing image. Use the extract_document tool with these fields:

- product_name (TEXT): Product name or title
- sku (TEXT): Product SKU or article number
- retail_price (CURRENCY_AMOUNT): Recommended retail price
- short_description (TEXT): One-line product description
- material (TEXT): Primary material or fabric
- weight (TEXT): Product weight with unit
- available_colors (TEXT): Available color options

Then use the generate_image tool to create a 1200x1200 listing image with the product name, specs, price, and SKU on a branded background.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "iVBORw0KGgo...",
    "mime_type": "image/png"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
