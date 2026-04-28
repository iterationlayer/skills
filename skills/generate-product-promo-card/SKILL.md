---
name: generate-product-promo-card
description: Generate a product promotional card with a product photo, sale badge, and pricing text.
---

# Generate Product Promo Card

E-commerce teams use this recipe to generate promotional cards for ads, social media, or email campaigns. Define a clean canvas with a product photo, overlay a bold sale badge with pricing, and add product name and call-to-action text — ready for Instagram ads, Facebook campaigns, or newsletter banners.

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
        "height_in_px": 1000,
    },
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
          "name": "product-photo.png",
          "base64": "<base64-encoded-product-photo>"
        },
        "position": {
            "x_in_px": 100.0,
            "y_in_px": 40.0,
        }
      },
      {
        "index": 2,
        "type": "solid-color",
        "hex_color": "#DC2626",
        "position": {
            "x_in_px": 560.0,
            "y_in_px": 40.0,
        },
        "dimensions": {
            "width_in_px": 200,
            "height_in_px": 70,
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "$29.99",
        "font_name": "Montserrat",
        "font_size_in_px": 36,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
            "x_in_px": 560.0,
            "y_in_px": 50.0,
        },
        "dimensions": {
            "width_in_px": 200,
            "height_in_px": 50,
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "Ultra-Light Running Shoes",
        "font_name": "Inter",
        "font_size_in_px": 32,
        "text_color": "#1a1a1a",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
            "x_in_px": 50.0,
            "y_in_px": 780.0,
        },
        "dimensions": {
            "width_in_px": 700,
            "height_in_px": 50,
        }
      },
      {
        "index": 5,
        "type": "text",
        "text": "Shop Now →",
        "font_name": "Inter",
        "font_size_in_px": 22,
        "text_color": "#DC2626",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
            "x_in_px": 250.0,
            "y_in_px": 880.0,
        },
        "dimensions": {
            "width_in_px": 300,
            "height_in_px": 40,
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
    height_in_px: 1000,
  },
  output_format: "png",
  layers: [
    {
      index: 0,
      type: "solid-color",
      hex_color: "#FFFFFF",
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
        x_in_px: 100,
        y_in_px: 40,
      },
    },
    {
      index: 2,
      type: "solid-color",
      hex_color: "#DC2626",
      position: {
        x_in_px: 560,
        y_in_px: 40,
      },
      dimensions: {
        width_in_px: 200,
        height_in_px: 70,
      },
    },
    {
      index: 3,
      type: "text",
      text: "$29.99",
      font_name: "Montserrat",
      font_size_in_px: 36,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 560,
        y_in_px: 50,
      },
      dimensions: {
        width_in_px: 200,
        height_in_px: 50,
      },
    },
    {
      index: 4,
      type: "text",
      text: "Ultra-Light Running Shoes",
      font_name: "Inter",
      font_size_in_px: 32,
      text_color: "#1a1a1a",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 50,
        y_in_px: 780,
      },
      dimensions: {
        width_in_px: 700,
        height_in_px: 50,
      },
    },
    {
      index: 5,
      type: "text",
      text: "Shop Now →",
      font_name: "Inter",
      font_size_in_px: 22,
      text_color: "#DC2626",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 250,
        y_in_px: 880,
      },
      dimensions: {
        width_in_px: 300,
        height_in_px: 40,
      },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_image(
    dimensions={
        "width_in_px": 800,
        "height_in_px": 1000,
    },
    output_format="png",
    layers=[
        {
            "index": 0,
            "type": "solid-color",
            "hex_color": "#FFFFFF",
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
                "x_in_px": 100,
                "y_in_px": 40,
            },
        },
        {
            "index": 2,
            "type": "solid-color",
            "hex_color": "#DC2626",
            "position": {
                "x_in_px": 560,
                "y_in_px": 40,
            },
            "dimensions": {
                "width_in_px": 200,
                "height_in_px": 70,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "$29.99",
            "font_name": "Montserrat",
            "font_size_in_px": 36,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 560,
                "y_in_px": 50,
            },
            "dimensions": {
                "width_in_px": 200,
                "height_in_px": 50,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "Ultra-Light Running Shoes",
            "font_name": "Inter",
            "font_size_in_px": 32,
            "text_color": "#1a1a1a",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 50,
                "y_in_px": 780,
            },
            "dimensions": {
                "width_in_px": 700,
                "height_in_px": 50,
            },
        },
        {
            "index": 5,
            "type": "text",
            "text": "Shop Now →",
            "font_name": "Inter",
            "font_size_in_px": 22,
            "text_color": "#DC2626",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 250,
                "y_in_px": 880,
            },
            "dimensions": {
                "width_in_px": 300,
                "height_in_px": 40,
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
    HeightInPx: 1000,
  },
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#FFFFFF"),
			il.NewStaticImageLayer(1, il.FileInput{Buffer: "<base64-encoded-product-photo>"},
				il.Position{
      XInPx: 100,
      YInPx: 40,
    },
				il.Dimensions{}),
			il.NewRectangleLayer(2, "#DC2626",
				il.Position{
      XInPx: 560,
      YInPx: 40,
    },
				il.Dimensions{
      WidthInPx: 200,
      HeightInPx: 70,
    }),
			il.NewTextLayer(3, "$29.99", "Montserrat", 36, "#FFFFFF",
				il.Position{
      XInPx: 560,
      YInPx: 50,
    },
				il.Dimensions{
      WidthInPx: 200,
      HeightInPx: 50,
    }),
			il.NewTextLayer(4, "Ultra-Light Running Shoes", "Inter", 32, "#1a1a1a",
				il.Position{
      XInPx: 50,
      YInPx: 780,
    },
				il.Dimensions{
      WidthInPx: 700,
      HeightInPx: 50,
    }),
			il.NewTextLayer(5, "Shop Now →", "Inter", 22, "#DC2626",
				il.Position{
      XInPx: 250,
      YInPx: 880,
    },
				il.Dimensions{
      WidthInPx: 300,
      HeightInPx: 40,
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
  "name": "Generate Product Promo Card",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Product Promo Card\n\nE-commerce teams use this recipe to generate promotional cards for ads, social media, or email campaigns. Define a clean canvas with a product photo, overlay a bold sale badge with pricing, and add product name and call-to-action text \u2014 ready for Instagram ads, Facebook campaigns, or newsletter banners.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "8d3de227-5fa0-4761-bcf7-fd45b1dd5f00",
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
      "id": "10cef9e2-3bb9-4700-a457-622dc7c1c829",
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
      "id": "b8c9d0e1-8888-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 800,
        "heightInPx": 1000,
        "outputFormat": "png",
        "layersJson": "[{\"index\":0,\"type\":\"solid-color\",\"hex_color\":\"#FFFFFF\"},{\"index\":1,\"type\":\"image\",\"file\":{\"type\":\"base64\",\"name\":\"product-photo.png\",\"base64\":\"<base64-encoded-product-photo>\"},\"position\":{\"x\":100.0,\"y\":40.0}},{\"index\":2,\"type\":\"solid-color\",\"hex_color\":\"#DC2626\",\"position\":{\"x\":560.0,\"y\":40.0},\"dimensions\":{\"width\":200,\"height\":70}},{\"index\":3,\"type\":\"text\",\"text\":\"$29.99\",\"font_name\":\"Montserrat\",\"font_size_in_px\":36,\"text_color\":\"#FFFFFF\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"position\":{\"x\":560.0,\"y\":50.0},\"dimensions\":{\"width\":200,\"height\":50}},{\"index\":4,\"type\":\"text\",\"text\":\"Ultra-Light Running Shoes\",\"font_name\":\"Inter\",\"font_size_in_px\":32,\"text_color\":\"#1a1a1a\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"position\":{\"x\":50.0,\"y\":780.0},\"dimensions\":{\"width\":700,\"height\":50}},{\"index\":5,\"type\":\"text\",\"text\":\"Shop Now \\u2192\",\"font_name\":\"Inter\",\"font_size_in_px\":22,\"text_color\":\"#DC2626\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"position\":{\"x\":250.0,\"y\":880.0},\"dimensions\":{\"width\":300,\"height\":40}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "b8c9d0e1-8888-4000-8000-000000000002",
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
Generate a product promotional card image at 1080x1080 pixels. Use the generate_image tool with these layers:

1. Background: solid-color layer with [background color]
2. Product photo: image layer with [product image URL] centered
3. Sale badge: solid-color rectangle with [discount %] text overlay in bold
4. Product name: text layer with [product name]
5. Price: text layer with [sale price] and [original price]
6. CTA: text layer with [call-to-action text]
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
