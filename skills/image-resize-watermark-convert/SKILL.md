---
name: image-resize-watermark-convert
description: Resize an image, add a branded text watermark, and convert to WebP in a two-step pipeline.
---

# Resize, Watermark, and Convert Image

E-commerce teams and content platforms use this recipe to process product images for web. Resize to standard dimensions, add brand watermark, and convert to WebP — in two API calls with no image processing infrastructure.

## APIs Used

Image Transformation (1 credits/request), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Resize and sharpen the image
# The base64 buffer from this response is used as the image layer file in step 2
curl -X POST https://api.iterationlayer.com/image-transformation/v1/transform \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "product-hero.jpg",
      "url": "https://cdn.example.com/products/product-hero.jpg"
    },
    "operations": [
      {
        "type": "resize",
        "width_in_px": 1200,
        "height_in_px": 1200,
        "fit": "cover",
        "position": "smart_crop"
      },
      {
        "type": "sharpen",
        "sigma": 0.5
      },
      { "type": "convert", "format": "png" }
    ]
  }'

# Step 2: Compose the resized image with a watermark overlay and output as WebP
# Replace the base64 value below with the buffer returned by step 1
curl -X POST https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": {
      "width_in_px": 1200,
      "height_in_px": 1200
    },
    "output_format": "webp",
    "layers": [
      {
        "index": 0,
        "type": "solid-color",
        "hex_color": "#FFFFFF"
      },
      {
        "index": 1,
        "type": "image",
        "file": {
          "type": "base64",
          "name": "product-resized.png",
          "base64": "<buffer from step 1>"
        },
        "position": {
          "x_in_px": 0,
          "y_in_px": 0
        },
        "dimensions": {
          "width_in_px": 1200,
          "height_in_px": 1200
        }
      },
      {
        "index": 2,
        "type": "text",
        "text": "© Riviera Home Goods",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#FFFFFF",
        "text_align": "right",
        "vertical_align": "center",
        "position": {
          "x_in_px": 900,
          "y_in_px": 1160
        },
        "dimensions": {
          "width_in_px": 280,
          "height_in_px": 24
        },
        "opacity": 60
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Resize and sharpen the image
const transformed = await client.transformImage({
  file: {
    type: "url",
    name: "product-hero.jpg",
    url: "https://cdn.example.com/products/product-hero.jpg",
  },
  operations: [
    {
      type: "resize",
      width_in_px: 1200,
      height_in_px: 1200,
      fit: "cover",
      position: "smart_crop",
    },
    {
      type: "sharpen",
      sigma: 0.5,
    },
    { type: "convert", format: "png" },
  ],
});

// Step 2: Compose the resized image with a watermark overlay and output as WebP
const result = await client.generateImage({
  dimensions: { width_in_px: 1200, height_in_px: 1200 },
  output_format: "webp",
  layers: [
    { index: 0, type: "solid-color", hex_color: "#FFFFFF" },
    {
      index: 1,
      type: "image",
      file: {
        type: "base64",
        name: "product-resized.png",
        base64: transformed.buffer,
      },
      position: { x_in_px: 0, y_in_px: 0 },
      dimensions: { width_in_px: 1200, height_in_px: 1200 },
    },
    {
      index: 2,
      type: "text",
      text: "\u00a9 Riviera Home Goods",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#FFFFFF",
      text_align: "right",
      vertical_align: "center",
      position: { x_in_px: 900, y_in_px: 1160 },
      dimensions: { width_in_px: 280, height_in_px: 24 },
      opacity: 60,
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Resize and sharpen the image
transformed = client.transform_image(
    file={
        "type": "url",
        "name": "product-hero.jpg",
        "url": "https://cdn.example.com/products/product-hero.jpg",
    },
    operations=[
        {
            "type": "resize",
            "width_in_px": 1200,
            "height_in_px": 1200,
            "fit": "cover",
            "position": "smart_crop",
        },
        {
            "type": "sharpen",
            "sigma": 0.5,
        },
        {"type": "convert", "format": "png"},
    ],
)

# Step 2: Compose the resized image with a watermark overlay and output as WebP
result = client.generate_image(
    dimensions={"width_in_px": 1200, "height_in_px": 1200},
    output_format="webp",
    layers=[
        {"index": 0, "type": "solid-color", "hex_color": "#FFFFFF"},
        {
            "index": 1,
            "type": "image",
            "file": {"type": "base64", "name": "product-resized.png", "base64": transformed["buffer"]},
            "position": {"x_in_px": 0, "y_in_px": 0},
            "dimensions": {"width_in_px": 1200, "height_in_px": 1200},
        },
        {
            "index": 2,
            "type": "text",
            "text": "\u00a9 Riviera Home Goods",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#FFFFFF",
            "text_align": "right",
            "vertical_align": "center",
            "position": {"x_in_px": 900, "y_in_px": 1160},
            "dimensions": {"width_in_px": 280, "height_in_px": 24},
            "opacity": 60,
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Resize and sharpen the image
	transformed, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL("product-hero.jpg", "https://cdn.example.com/products/product-hero.jpg"),
		Operations: []il.TransformOperation{
			il.NewResizeOperation(1200, 1200, "cover"),
			il.NewSharpenOperation(0.5),
			il.NewConvertOperation("png"),
		},
	})
	if err != nil {
		panic(err)
	}

	// Step 2: Compose the resized image with a watermark overlay and output as WebP
	_, err = client.GenerateImage(il.GenerateImageRequest{
		Dimensions:   il.Dimensions{WidthInPx: 1200, HeightInPx: 1200},
		OutputFormat: "webp",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#FFFFFF"),
			il.NewImageLayer(1, il.NewFileFromBase64("product-resized.png", transformed.Buffer),
				il.Position{XInPx: 0, YInPx: 0},
				il.Dimensions{WidthInPx: 1200, HeightInPx: 1200},
			),
			il.NewTextLayer(2, "\u00a9 Riviera Home Goods", "Inter", 16, "#FFFFFF",
				il.Position{XInPx: 900, YInPx: 1160},
				il.Dimensions{WidthInPx: 280, HeightInPx: 24},
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
  "name": "Resize, Watermark, and Convert Image",
  "nodes": [
    {
      "parameters": {
        "content": "## Resize, Watermark, and Convert Image\n\nE-commerce teams and content platforms use this recipe to process product images for web. Resize to standard dimensions, add brand watermark, and convert to WebP \u2014 in two API calls with no image processing infrastructure.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "ff8fd426-80e9-4d97-b534-b997ec1f6eab",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Resize and Sharpen\nResource: **Image Transformation**\n\nConfigure the Image Transformation parameters below, then connect your credentials.",
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
      "id": "4a44d3f6-06b1-4418-ba71-6def1ca74e97",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Add Watermark and Convert\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
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
      "id": "03b33364-4198-42e9-ba9d-61f82588fcce",
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
      "id": "e6f7a8b9-c0d1-2345-6e90-234567890123",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "product-hero.jpg",
        "fileUrl": "https://cdn.example.com/products/product-hero.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "resize",
              "widthInPx": 1200,
              "heightInPx": 1200,
              "resizeMode": "cover"
            },
            {
              "operationType": "sharpen",
              "sigma": 0.5
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
      "id": "f7a8b9c0-d1e2-3456-7f01-345678901234",
      "name": "Resize and Sharpen",
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
        "widthInPx": 1200,
        "heightInPx": 1200,
        "outputFormat": "webp",
        "layersJson": "[\n  {\n    \"index\": 0,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#FFFFFF\"\n  },\n  {\n    \"index\": 1,\n    \"type\": \"image\",\n    \"file\": {\n      \"type\": \"base64\",\n      \"name\": \"product-resized.png\",\n      \"base64\": \"<buffer from step 1>\"\n    },\n    \"position\": { \"x\": 0, \"y\": 0 },\n    \"dimensions\": { \"width\": 1200, \"height\": 1200 }\n  },\n  {\n    \"index\": 2,\n    \"type\": \"text\",\n    \"text\": \"\\u00a9 Riviera Home Goods\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 16,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#FFFFFF\",\n    \"text_align\": \"right\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 900, \"y\": 1160 },\n    \"dimensions\": { \"width\": 280, \"height\": 24 },\n    \"opacity\": 60\n  }\n]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "a8b9c0d1-e2f3-4567-8a12-456789012345",
      "name": "Add Watermark and Convert",
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
            "node": "Resize and Sharpen",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Resize and Sharpen": {
      "main": [
        [
          {
            "node": "Add Watermark and Convert",
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
Resize, watermark, and convert the image at [file URL]. First, use the transform_image tool with a resize operation to [width]x[height] pixels (fit: cover, position: smart_crop), a sharpen operation, and a convert to PNG. Then use the generate_image tool to compose the resized image with a text watermark layer showing "[watermark text]" and output as WebP.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "UklGRlYAAABXRUJQVlA4IEoA...",
    "mime_type": "image/webp"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
