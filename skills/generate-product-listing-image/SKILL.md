---
name: generate-product-listing-image
description: Generate a product listing image with photo, price badge, and promotional text overlay.
---

# Generate Product Listing Image

E-commerce teams and marketplace sellers use this recipe to generate a listing image with consistent branding. Combine a product photo with a price badge and title text — ready for a marketplace listing or promotional campaign.

## APIs Used

Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/image-generation/v1/render \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": {
        "width_in_px": 800,
        "height_in_px": 800,
    },
    "layers": [
      {
        "index": 0,
        "type": "solid-color",
        "hex_color": "#F5F5F5"
      },
      {
        "index": 1,
        "type": "image",
        "file": {
          "type": "base64",
          "name": "product-photo.png",
          "base64": "<base64-encoded-product-photo>"
        },
        "position": {
            "x_in_px": 50.0,
            "y_in_px": 50.0,
        }
      },
      {
        "index": 2,
        "type": "solid-color",
        "hex_color": "#E53935",
        "position": {
            "x_in_px": 560.0,
            "y_in_px": 50.0,
        },
        "dimensions": {
            "width_in_px": 190,
            "height_in_px": 60,
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "$49.99",
        "font_name": "Roboto",
        "font_size_in_px": 32,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
            "x_in_px": 560.0,
            "y_in_px": 55.0,
        },
        "dimensions": {
            "width_in_px": 190,
            "height_in_px": 50,
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "Premium Wireless Headphones",
        "font_name": "Inter",
        "font_size_in_px": 28,
        "text_color": "#1a1a1a",
        "text_align": "center",
        "position": {
            "x_in_px": 50.0,
            "y_in_px": 700.0,
        },
        "dimensions": {
            "width_in_px": 700,
            "height_in_px": 50,
        }
      }
    ],
    "output_format": "png"
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateImage({
  dimensions: {
    width_in_px: 800,
    height_in_px: 800,
  },
  layers: [
    {
      index: 0,
      type: "solid-color",
      hex_color: "#F5F5F5",
    },
    {
      index: 1,
      type: "image",
      file: {
        type: "base64",
        name: "product-photo.png",
        base64: "<base64-encoded-product-photo>",
      },
      position: {
        x_in_px: 50,
        y_in_px: 50,
      },
    },
    {
      index: 2,
      type: "solid-color",
      hex_color: "#E53935",
      position: {
        x_in_px: 560,
        y_in_px: 50,
      },
      dimensions: {
        width_in_px: 190,
        height_in_px: 60,
      },
    },
    {
      index: 3,
      type: "text",
      text: "$49.99",
      font_name: "Roboto",
      font_size_in_px: 32,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 560,
        y_in_px: 55,
      },
      dimensions: {
        width_in_px: 190,
        height_in_px: 50,
      },
    },
    {
      index: 4,
      type: "text",
      text: "Premium Wireless Headphones",
      font_name: "Inter",
      font_size_in_px: 28,
      text_color: "#1a1a1a",
      text_align: "center",
      position: {
        x_in_px: 50,
        y_in_px: 700,
      },
      dimensions: {
        width_in_px: 700,
        height_in_px: 50,
      },
    },
  ],
  output_format: "png",
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_image(
    dimensions={
        "width_in_px": 800,
        "height_in_px": 800,
    },
    layers=[
        {
            "index": 0,
            "type": "solid-color",
            "hex_color": "#F5F5F5",
        },
        {
            "index": 1,
            "type": "image",
            "file": {
                "type": "base64",
                "name": "product-photo.png",
                "base64": "<base64-encoded-product-photo>",
            },
            "position": {
                "x_in_px": 50,
                "y_in_px": 50,
            },
        },
        {
            "index": 2,
            "type": "solid-color",
            "hex_color": "#E53935",
            "position": {
                "x_in_px": 560,
                "y_in_px": 50,
            },
            "dimensions": {
                "width_in_px": 190,
                "height_in_px": 60,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "$49.99",
            "font_name": "Roboto",
            "font_size_in_px": 32,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 560,
                "y_in_px": 55,
            },
            "dimensions": {
                "width_in_px": 190,
                "height_in_px": 50,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "Premium Wireless Headphones",
            "font_name": "Inter",
            "font_size_in_px": 28,
            "text_color": "#1a1a1a",
            "text_align": "center",
            "position": {
                "x_in_px": 50,
                "y_in_px": 700,
            },
            "dimensions": {
                "width_in_px": 700,
                "height_in_px": 50,
            },
        },
    ],
    output_format="png",
)

```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.GenerateImage(il.GenerateImageRequest{
	Dimensions:   il.Dimensions{
   WidthInPx: 800,
   HeightInPx: 800,
 },
	OutputFormat: "png",
	Layers: []il.Layer{
		il.NewSolidColorBackgroundLayer(0, "#F5F5F5"),
		il.NewStaticImageLayer(1, il.FileInput{Buffer: "<base64-encoded-product-photo>"},
			il.Position{
     XInPx: 50,
     YInPx: 50,
   },
			il.Dimensions{}),
		il.NewRectangleLayer(2, "#E53935",
			il.Position{
     XInPx: 560,
     YInPx: 50,
   },
			il.Dimensions{
     WidthInPx: 190,
     HeightInPx: 60,
   }),
		il.NewTextLayer(3, "$49.99", "Roboto", 32, "#FFFFFF",
			il.Position{
     XInPx: 560,
     YInPx: 55,
   },
			il.Dimensions{
     WidthInPx: 190,
     HeightInPx: 50,
   }),
		il.NewTextLayer(4, "Premium Wireless Headphones", "Inter", 28, "#1a1a1a",
			il.Position{
     XInPx: 50,
     YInPx: 700,
   },
			il.Dimensions{
     WidthInPx: 700,
     HeightInPx: 50,
   }),
	},
	})
	if err != nil {
		panic(err)
	}

}
```

```n8n
{
  "name": "Generate Product Listing Image",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Product Listing Image\n\nE-commerce teams and marketplace sellers use this recipe to generate a listing image with consistent branding. Combine a product photo with a price badge and title text \u2014 ready for a marketplace listing or promotional campaign.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "07e4902e-c9aa-4b0d-8a75-e2f0ec55b4fc",
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
      "id": "e66e21eb-44f3-43d2-b1a0-6a24fefc045e",
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
      "id": "a7b8c9d0-7777-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 800,
        "heightInPx": 800,
        "outputFormat": "png",
        "layersJson": "[{\"index\":0,\"type\":\"solid-color\",\"hex_color\":\"#F5F5F5\"},{\"index\":1,\"type\":\"image\",\"file\":{\"type\":\"base64\",\"name\":\"product-photo.png\",\"base64\":\"<base64-encoded-product-photo>\"},\"position\":{\"x\":50.0,\"y\":50.0}},{\"index\":2,\"type\":\"solid-color\",\"hex_color\":\"#E53935\",\"position\":{\"x\":560.0,\"y\":50.0},\"dimensions\":{\"width\":190,\"height\":60}},{\"index\":3,\"type\":\"text\",\"text\":\"$49.99\",\"font_name\":\"Roboto\",\"font_size_in_px\":32,\"text_color\":\"#FFFFFF\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"position\":{\"x\":560.0,\"y\":55.0},\"dimensions\":{\"width\":190,\"height\":50}},{\"index\":4,\"type\":\"text\",\"text\":\"Premium Wireless Headphones\",\"font_name\":\"Inter\",\"font_size_in_px\":28,\"text_color\":\"#1a1a1a\",\"text_align\":\"center\",\"position\":{\"x\":50.0,\"y\":700.0},\"dimensions\":{\"width\":700,\"height\":50}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "a7b8c9d0-7777-4000-8000-000000000002",
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
Generate a product listing image at 800x800 pixels. Use the generate_image tool with these layers:

1. Background: solid-color layer with [background color]
2. Product photo: image layer with [product image URL] centered
3. Price badge: solid-color rectangle with [price] text overlay in bold
4. Product name: text layer with [product name] positioned below the photo
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
