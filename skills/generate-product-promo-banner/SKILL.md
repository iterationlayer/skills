---
name: generate-product-promo-banner
description: Optimize a product photo and compose it into a promotional banner with sale text and pricing.
---

# Generate Product Promo Banner

E-commerce teams use this pipeline to create promotional banners from raw product photos. Optimize the product image for web, then composite it onto a banner with sale messaging and pricing — ready for ads, email campaigns, or social media.

## APIs Used

Image Transformation (1 credits/request), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Optimize product photo
TRANSFORM_RESULT=$(curl -s -X POST \
  https://api.iterationlayer.com/image-transformation/v1/transform \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "product-photo.jpg",
      "url": "https://example.com/products/raw-photo.jpg"
    },
    "operations": [
      {
        "type": "resize",
        "width_in_px": 1000,
        "height_in_px": 1000,
        "fit": "contain"
      },
      { "type": "auto_contrast" },
      {
          "type": "sharpen",
          "sigma": 1.5,
      },
      {
        "type": "convert",
        "format": "webp",
        "quality": 90
      }
    ]
  }')

PRODUCT_IMAGE_BASE64=$(echo "$TRANSFORM_RESULT" | jq -r '.data.buffer')

# Step 2: Generate promotional banner with optimized product photo
curl -X POST \
  https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"dimensions\": {
      \"width\": 1200,
      \"height\": 628
    },
    \"output_format\": \"png\",
    \"layers\": [
      {
        \"index\": 0,
        \"type\": \"solid-color\",
        \"hex_color\": \"#1a1a2e\"
      },
      {
        \"index\": 1,
        \"type\": \"static-image\",
        \"buffer\": \"$PRODUCT_IMAGE_BASE64\",
        \"position\": {
          \"x\": 600.0,
          \"y\": 64.0
        },
        \"dimensions\": {
          \"width\": 500,
          \"height\": 500
        }
      },
      {
        \"index\": 2,
        \"type\": \"text\",
        \"text\": \"Flash Sale\",
        \"font_name\": \"Montserrat\",
        \"font_size_in_px\": 56,
        \"text_color\": \"#FFFFFF\",
        \"font_weight\": \"bold\",
        \"position\": {
          \"x\": 60.0,
          \"y\": 180.0
        },
        \"dimensions\": {
          \"width\": 500,
          \"height\": 80
        }
      },
      {
        \"index\": 3,
        \"type\": \"text\",
        \"text\": \"Save 25%\",
        \"font_name\": \"Montserrat\",
        \"font_size_in_px\": 36,
        \"text_color\": \"#E94560\",
        \"font_weight\": \"bold\",
        \"position\": {
          \"x\": 60.0,
          \"y\": 280.0
        },
        \"dimensions\": {
          \"width\": 500,
          \"height\": 60
        }
      }
    ]
  }"
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Optimize product photo
const transformResult = await client.transformImage({
  file: {
    type: "url",
    name: "product-photo.jpg",
    url: "https://example.com/products/raw-photo.jpg",
  },
  operations: [
    {
      type: "resize",
      width_in_px: 1000,
      height_in_px: 1000,
      fit: "contain",
    },
    { type: "auto_contrast" },
    {
      type: "sharpen",
      sigma: 1.5,
    },
    {
      type: "convert",
      format: "webp",
      quality: 90,
    },
  ],
});

const productImageBase64 = transformResult.data.buffer;

// Step 2: Generate promotional banner with optimized product photo
const bannerResult = await client.generateImage({
  dimensions: {
    width_in_px: 1200,
    height_in_px: 628,
  },
  output_format: "png",
  layers: [
    {
      index: 0,
      type: "solid-color",
      hex_color: "#1a1a2e",
    },
    {
      index: 1,
      type: "image",
      buffer: productImageBase64,
      position: {
        x_in_px: 600.0,
        y_in_px: 64.0,
      },
      dimensions: {
        width_in_px: 500,
        height_in_px: 500,
      },
    },
    {
      index: 2,
      type: "text",
      text: "Flash Sale",
      font_name: "Montserrat",
      font_size_in_px: 56,
      text_color: "#FFFFFF",
      font_weight: "bold",
      position: {
        x_in_px: 60.0,
        y_in_px: 180.0,
      },
      dimensions: {
        width_in_px: 500,
        height_in_px: 80,
      },
    },
    {
      index: 3,
      type: "text",
      text: "Save 25%",
      font_name: "Montserrat",
      font_size_in_px: 36,
      text_color: "#E94560",
      font_weight: "bold",
      position: {
        x_in_px: 60.0,
        y_in_px: 280.0,
      },
      dimensions: {
        width_in_px: 500,
        height_in_px: 60,
      },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Optimize product photo
transform_result = client.transform_image(
    file={
        "type": "url",
        "name": "product-photo.jpg",
        "url": "https://example.com/products/raw-photo.jpg",
    },
    operations=[
        {
            "type": "resize",
            "width_in_px": 1000,
            "height_in_px": 1000,
            "fit": "contain",
        },
        {"type": "auto_contrast"},
        {
            "type": "sharpen",
            "sigma": 1.5,
        },
        {
            "type": "convert",
            "format": "webp",
            "quality": 90,
        },
    ],
)

product_image_base64 = transform_result["data"]["buffer"]

# Step 2: Generate promotional banner with optimized product photo
banner_result = client.generate_image(
    dimensions={
        "width_in_px": 1200,
        "height_in_px": 628,
    },
    output_format="png",
    layers=[
        {
            "index": 0,
            "type": "solid-color",
            "hex_color": "#1a1a2e",
        },
        {
            "index": 1,
            "type": "image",
            "buffer": product_image_base64,
            "position": {
                "x_in_px": 600.0,
                "y_in_px": 64.0,
            },
            "dimensions": {
                "width_in_px": 500,
                "height_in_px": 500,
            },
        },
        {
            "index": 2,
            "type": "text",
            "text": "Flash Sale",
            "font_name": "Montserrat",
            "font_size_in_px": 56,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "position": {
                "x_in_px": 60.0,
                "y_in_px": 180.0,
            },
            "dimensions": {
                "width_in_px": 500,
                "height_in_px": 80,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "Save 25%",
            "font_name": "Montserrat",
            "font_size_in_px": 36,
            "text_color": "#E94560",
            "font_weight": "bold",
            "position": {
                "x_in_px": 60.0,
                "y_in_px": 280.0,
            },
            "dimensions": {
                "width_in_px": 500,
                "height_in_px": 60,
            },
        },
    ],
)

```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Optimize product photo
	transformResult, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL(
			"product-photo.jpg",
			"https://example.com/products/raw-photo.jpg",
		),
		Operations: []il.TransformOperation{
			il.NewResizeOperation(1000, 1000, "contain"),
			{Type: "auto_contrast"},
			il.NewSharpenOperation(1.5),
			il.NewConvertOperation("webp"),
		},
	})
	if err != nil {
		panic(err)
	}

	// Step 2: Generate promotional banner with optimized product photo
	bannerResult, err := client.GenerateImage(
		il.GenerateImageRequest{
			Dimensions: il.Dimensions{
				WidthInPx: 1200, HeightInPx: 628,
			},
			OutputFormat: "png",
			Layers: []il.Layer{
				il.NewSolidColorBackgroundLayer(0, "#1a1a2e"),
				il.NewStaticImageLayer(
					1, transformResult.Data.Buffer,
					il.Position{
       XInPx: 600.0,
       YInPx: 64.0,
     },
					il.Dimensions{
       WidthInPx: 500,
       HeightInPx: 500,
     },
				),
				il.NewTextLayer(
					2, "Flash Sale", "Montserrat", 56, "#FFFFFF",
					il.Position{
       XInPx: 60.0,
       YInPx: 180.0,
     },
					il.Dimensions{
       WidthInPx: 500,
       HeightInPx: 80,
     },
				),
				il.NewTextLayer(
					3, "Save 25%", "Montserrat", 36, "#E94560",
					il.Position{
       XInPx: 60.0,
       YInPx: 280.0,
     },
					il.Dimensions{
       WidthInPx: 500,
       HeightInPx: 60,
     },
				),
			},
		},
	)
	if err != nil {
		panic(err)
	}

}
```

```n8n
{
  "name": "Generate Product Promo Banner",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Product Promo Banner\n\nE-commerce teams use this pipeline to create promotional banners from raw product photos. Optimize the product image for web, then composite it onto a banner with sale messaging and pricing \u2014 ready for ads, email campaigns, or social media.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "165c6cae-c517-466c-b457-017d41ef255f",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Image\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
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
      "id": "d0e30702-3236-4d5b-b6db-58943e78c2e7",
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
      "id": "8c932661-770b-4c6d-b9af-e11756428109",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 1200,
        "heightInPx": 630,
        "outputFormat": "png",
        "layersJson": "[]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "c8fc1270-b021-4063-a9f2-c926a0b9e623",
      "name": "Generate Image",
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
            "node": "Generate Image",
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
Optimize a product photo and compose it into a promotional banner. First, use transform_image to resize and optimize [product image URL] for web. Then, use generate_image to create a banner with these layers:

1. Background: gradient layer with [brand colors]
2. Product photo: image layer with the optimized product image
3. Sale text: text layer with [sale message] in large bold text
4. Price: text layer with [price] and [original price] strikethrough
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
