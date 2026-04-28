---
name: remove-background-and-generate-product-card
description: Remove the background from a raw product photo, then compose it into a branded listing card with the product name and price.
---

# Remove Background and Generate Product Card

E-commerce sellers and marketplace teams take a raw product photo, remove the background to get a clean cutout, then compose it into a branded listing card with the product name, price, and brand — ready for Amazon, Shopify, or social media.

## APIs Used

Image Transformation (1 credits/request), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Remove background and resize to a clean 600×600 PNG cutout
# The base64 buffer from this response is used as the image layer file in step 2
curl -X POST https://api.iterationlayer.com/image-transformation/v1/transform \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "wallet-raw.jpg",
      "url": "https://cdn.example.com/products/wallet-raw.jpg"
    },
    "operations": [
      { "type": "remove_background" },
      {
        "type": "resize",
        "width_in_px": 600,
        "height_in_px": 600,
        "fit": "contain"
      },
      { "type": "convert", "format": "png" }
    ]
  }'

# Step 2: Compose the cleaned product cutout into a branded listing card
# Replace the base64 value below with the buffer returned by step 1
curl -X POST https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": { "width_in_px": 900, "height_in_px": 900 },
    "output_format": "png",
    "layers": [
      { "index": 0, "type": "solid-color", "hex_color": "#F5F5F5" },
      {
        "index": 1,
        "type": "solid-color",
        "hex_color": "#FFFFFF",
        "position": { "x_in_px": 30, "y_in_px": 30 },
        "dimensions": { "width_in_px": 840, "height_in_px": 840 },
        "border_radius": 12
      },
      {
        "index": 2,
        "type": "image",
        "file": {
          "type": "base64",
          "name": "product-clean.png",
          "base64": "<buffer from step 1>"
        },
        "position": { "x_in_px": 150, "y_in_px": 80 },
        "dimensions": { "width_in_px": 600, "height_in_px": 600 }
      },
      {
        "index": 3,
        "type": "text",
        "text": "CRAFTWELL GOODS",
        "font_name": "Inter",
        "font_size_in_px": 12,
        "font_weight": "regular",
        "text_color": "#94A3B8",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 48, "y_in_px": 718 },
        "dimensions": { "width_in_px": 500, "height_in_px": 20 }
      },
      {
        "index": 4,
        "type": "text",
        "text": "Full-Grain Leather Bifold Wallet",
        "font_name": "Inter",
        "font_size_in_px": 22,
        "font_weight": "bold",
        "text_color": "#0F172A",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 48, "y_in_px": 746 },
        "dimensions": { "width_in_px": 600, "height_in_px": 36 }
      },
      {
        "index": 5,
        "type": "text",
        "text": "$89.00",
        "font_name": "Inter",
        "font_size_in_px": 28,
        "font_weight": "bold",
        "text_color": "#B45309",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 48, "y_in_px": 790 },
        "dimensions": { "width_in_px": 300, "height_in_px": 44 }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Remove background and resize to a clean 600×600 PNG cutout
const transformed = await client.transformImage({
  file: {
    type: "url",
    name: "wallet-raw.jpg",
    url: "https://cdn.example.com/products/wallet-raw.jpg",
  },
  operations: [
    { type: "remove_background" },
    {
      type: "resize",
      width_in_px: 600,
      height_in_px: 600,
      fit: "contain",
    },
    { type: "convert", format: "png" },
  ],
});

// Step 2: Compose the cleaned product cutout into a branded listing card
const card = await client.generateImage({
  dimensions: { width_in_px: 900, height_in_px: 900 },
  output_format: "png",
  layers: [
    { index: 0, type: "solid-color", hex_color: "#F5F5F5" },
    {
      index: 1,
      type: "solid-color",
      hex_color: "#FFFFFF",
      position: { x_in_px: 30, y_in_px: 30 },
      dimensions: { width_in_px: 840, height_in_px: 840 },
      border_radius: 12,
    },
    {
      index: 2,
      type: "image",
      file: {
        type: "base64",
        name: "product-clean.png",
        base64: transformed.buffer,
      },
      position: { x_in_px: 150, y_in_px: 80 },
      dimensions: { width_in_px: 600, height_in_px: 600 },
    },
    {
      index: 3,
      type: "text",
      text: "CRAFTWELL GOODS",
      font_name: "Inter",
      font_size_in_px: 12,
      font_weight: "regular",
      text_color: "#94A3B8",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 48, y_in_px: 718 },
      dimensions: { width_in_px: 500, height_in_px: 20 },
    },
    {
      index: 4,
      type: "text",
      text: "Full-Grain Leather Bifold Wallet",
      font_name: "Inter",
      font_size_in_px: 22,
      font_weight: "bold",
      text_color: "#0F172A",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 48, y_in_px: 746 },
      dimensions: { width_in_px: 600, height_in_px: 36 },
    },
    {
      index: 5,
      type: "text",
      text: "$89.00",
      font_name: "Inter",
      font_size_in_px: 28,
      font_weight: "bold",
      text_color: "#B45309",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 48, y_in_px: 790 },
      dimensions: { width_in_px: 300, height_in_px: 44 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Remove background and resize to a clean 600×600 PNG cutout
transformed = client.transform_image(
    file={
        "type": "url",
        "name": "wallet-raw.jpg",
        "url": "https://cdn.example.com/products/wallet-raw.jpg",
    },
    operations=[
        {"type": "remove_background"},
        {
            "type": "resize",
            "width_in_px": 600,
            "height_in_px": 600,
            "fit": "contain",
        },
        {"type": "convert", "format": "png"},
    ],
)

# Step 2: Compose the cleaned product cutout into a branded listing card
card = client.generate_image(
    dimensions={"width_in_px": 900, "height_in_px": 900},
    output_format="png",
    layers=[
        {"index": 0, "type": "solid-color", "hex_color": "#F5F5F5"},
        {
            "index": 1,
            "type": "solid-color",
            "hex_color": "#FFFFFF",
            "position": {"x_in_px": 30, "y_in_px": 30},
            "dimensions": {"width_in_px": 840, "height_in_px": 840},
            "border_radius": 12,
        },
        {
            "index": 2,
            "type": "image",
            "file": {"type": "base64", "name": "product-clean.png", "base64": transformed["buffer"]},
            "position": {"x_in_px": 150, "y_in_px": 80},
            "dimensions": {"width_in_px": 600, "height_in_px": 600},
        },
        {
            "index": 3,
            "type": "text",
            "text": "CRAFTWELL GOODS",
            "font_name": "Inter",
            "font_size_in_px": 12,
            "font_weight": "regular",
            "text_color": "#94A3B8",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 48, "y_in_px": 718},
            "dimensions": {"width_in_px": 500, "height_in_px": 20},
        },
        {
            "index": 4,
            "type": "text",
            "text": "Full-Grain Leather Bifold Wallet",
            "font_name": "Inter",
            "font_size_in_px": 22,
            "font_weight": "bold",
            "text_color": "#0F172A",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 48, "y_in_px": 746},
            "dimensions": {"width_in_px": 600, "height_in_px": 36},
        },
        {
            "index": 5,
            "type": "text",
            "text": "$89.00",
            "font_name": "Inter",
            "font_size_in_px": 28,
            "font_weight": "bold",
            "text_color": "#B45309",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 48, "y_in_px": 790},
            "dimensions": {"width_in_px": 300, "height_in_px": 44},
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Remove background and resize to a clean 600×600 PNG cutout
	transformed, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL("wallet-raw.jpg", "https://cdn.example.com/products/wallet-raw.jpg"),
		Operations: []il.TransformOperation{
			il.NewRemoveBackgroundOperation(),
			il.NewResizeOperation(600, 600, "contain"),
			il.NewConvertOperation("png"),
		},
	})
	if err != nil {
		panic(err)
	}

	// Step 2: Compose the cleaned product cutout into a branded listing card
	_, err = client.GenerateImage(il.GenerateImageRequest{
		Dimensions:   il.Dimensions{WidthInPx: 900, HeightInPx: 900},
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#F5F5F5"),
			il.NewSolidColorLayer(1, "#FFFFFF",
				il.Position{XInPx: 30, YInPx: 30},
				il.Dimensions{WidthInPx: 840, HeightInPx: 840},
			),
			il.NewImageLayer(2, il.NewFileFromBase64("product-clean.png", transformed.Buffer),
				il.Position{XInPx: 150, YInPx: 80},
				il.Dimensions{WidthInPx: 600, HeightInPx: 600},
			),
			il.NewTextLayer(3, "CRAFTWELL GOODS", "Inter", 12, "#94A3B8",
				il.Position{XInPx: 48, YInPx: 718},
				il.Dimensions{WidthInPx: 500, HeightInPx: 20},
			),
			il.NewTextLayer(4, "Full-Grain Leather Bifold Wallet", "Inter", 22, "#0F172A",
				il.Position{XInPx: 48, YInPx: 746},
				il.Dimensions{WidthInPx: 600, HeightInPx: 36},
			),
			il.NewTextLayer(5, "$89.00", "Inter", 28, "#B45309",
				il.Position{XInPx: 48, YInPx: 790},
				il.Dimensions{WidthInPx: 300, HeightInPx: 44},
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
  "name": "Remove Background and Generate Product Card",
  "nodes": [
    {
      "parameters": {
        "content": "## Remove Background and Generate Product Card\n\nE-commerce sellers and marketplace teams take a raw product photo, remove the background to get a clean cutout, then compose it into a branded listing card with the product name, price, and brand \u2014 ready for Amazon, Shopify, or social media.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "345823ee-3beb-412a-97be-00a5d219e9f5",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Remove Background\nResource: **Image Transformation**\n\nConfigure the Image Transformation parameters below, then connect your credentials.",
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
      "id": "f00f7d1c-d454-4ff3-86cd-6ee5cebbb132",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Product Card\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
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
      "id": "e69408b7-dc7e-4053-8ff2-735134e6c12a",
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
      "id": "b9c0d1e2-f3a4-5678-9b23-567890123456",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "wallet-raw.jpg",
        "fileUrl": "https://cdn.example.com/products/wallet-raw.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "removeBackground"
            },
            {
              "operationType": "resize",
              "widthInPx": 600,
              "heightInPx": 600,
              "resizeMode": "contain"
            },
            {
              "operationType": "convert",
              "convertFormat": "png"
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
      "id": "c0d1e2f3-a4b5-6789-0c34-678901234567",
      "name": "Remove Background",
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
        "widthInPx": 900,
        "heightInPx": 900,
        "outputFormat": "png",
        "layersJson": "[\n  {\n    \"index\": 0,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#F5F5F5\"\n  },\n  {\n    \"index\": 1,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#FFFFFF\",\n    \"position\": { \"x\": 30, \"y\": 30 },\n    \"dimensions\": { \"width\": 840, \"height\": 840 },\n    \"border_radius\": 12\n  },\n  {\n    \"index\": 2,\n    \"type\": \"image\",\n    \"file\": {\n      \"type\": \"base64\",\n      \"name\": \"product-clean.png\",\n      \"base64\": \"<buffer from step 1>\"\n    },\n    \"position\": { \"x\": 150, \"y\": 80 },\n    \"dimensions\": { \"width\": 600, \"height\": 600 }\n  },\n  {\n    \"index\": 3,\n    \"type\": \"text\",\n    \"text\": \"CRAFTWELL GOODS\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 12,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#94A3B8\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 48, \"y\": 718 },\n    \"dimensions\": { \"width\": 500, \"height\": 20 }\n  },\n  {\n    \"index\": 4,\n    \"type\": \"text\",\n    \"text\": \"Full-Grain Leather Bifold Wallet\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 22,\n    \"font_weight\": \"bold\",\n    \"text_color\": \"#0F172A\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 48, \"y\": 746 },\n    \"dimensions\": { \"width\": 600, \"height\": 36 }\n  },\n  {\n    \"index\": 5,\n    \"type\": \"text\",\n    \"text\": \"$89.00\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 28,\n    \"font_weight\": \"bold\",\n    \"text_color\": \"#B45309\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 48, \"y\": 790 },\n    \"dimensions\": { \"width\": 300, \"height\": 44 }\n  }\n]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "d1e2f3a4-b5c6-7890-1d45-789012345678",
      "name": "Generate Product Card",
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
            "node": "Remove Background",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Remove Background": {
      "main": [
        [
          {
            "node": "Generate Product Card",
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
Remove the background from the product image at [file URL] and generate a branded product card. First, use the transform_image tool with a remove_background operation, a resize operation to 600x600 pixels (fit: contain), and a convert to PNG. Then use the generate_image tool to compose the cutout into a 900x900 card with a background color, the product image, brand name "[brand name]", product name "[product name]", and price "[price]" as text layers, output as PNG.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "iVBORw0KGgoAAAANSUhEUgAA...",
    "mime_type": "image/png"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
