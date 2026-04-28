---
name: generate-aplus-content-banner
description: Generate an Amazon A+ Content banner image for book marketing with cover art, title, and branding.
---

# Generate A+ Content Banner

Self-publishing authors and book marketing teams use this recipe to generate Amazon A+ Content banners at the standard 970x600 module size. Compose a book cover image, title text, series name, genre label, and availability badge — ready to upload to Amazon Author Central.

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
      "width_in_px": 970,
      "height_in_px": 600
    },
    "output_format": "png",
    "layers": [
      {
        "index": 0,
        "type": "gradient",
        "gradient_type": "linear",
        "angle_in_degrees": 135.0,
        "colors": [
          {
            "hex_color": "#1B2838",
            "position": 0.0
          },
          {
            "hex_color": "#2D3A4A",
            "position": 100.0
          }
        ],
        "position": {
          "x_in_px": 0.0,
          "y_in_px": 0.0
        },
        "dimensions": {
          "width_in_px": 970,
          "height_in_px": 600
        }
      },
      {
        "index": 1,
        "type": "image",
        "buffer": "<base64-encoded-book-cover>",
        "position": {
          "x_in_px": 60.0,
          "y_in_px": 60.0
        },
        "dimensions": {
          "width_in_px": 320,
          "height_in_px": 480
        }
      },
      {
        "index": 2,
        "type": "text",
        "text": "THE ASHWORTH CHRONICLES",
        "font_name": "Montserrat",
        "font_size_in_px": 14,
        "text_color": "#94A3B8",
        "font_weight": "bold",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 80.0
        },
        "dimensions": {
          "width_in_px": 480,
          "height_in_px": 24
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "The Long Silence",
        "font_name": "PlayfairDisplay",
        "font_size_in_px": 44,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 120.0
        },
        "dimensions": {
          "width_in_px": 480,
          "height_in_px": 110
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "Elizabeth Ashworth",
        "font_name": "Lora",
        "font_size_in_px": 20,
        "text_color": "#CBD5E1",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 250.0
        },
        "dimensions": {
          "width_in_px": 480,
          "height_in_px": 32
        }
      },
      {
        "index": 5,
        "type": "solid-color",
        "hex_color": "#6C63FF",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 320.0
        },
        "dimensions": {
          "width_in_px": 120,
          "height_in_px": 32
        }
      },
      {
        "index": 6,
        "type": "text",
        "text": "THRILLER",
        "font_name": "Montserrat",
        "font_size_in_px": 12,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "vertical_align": "center",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 320.0
        },
        "dimensions": {
          "width_in_px": 120,
          "height_in_px": 32
        }
      },
      {
        "index": 7,
        "type": "solid-color",
        "hex_color": "#22C55E",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 480.0
        },
        "dimensions": {
          "width_in_px": 180,
          "height_in_px": 40
        }
      },
      {
        "index": 8,
        "type": "text",
        "text": "Available Now",
        "font_name": "Montserrat",
        "font_size_in_px": 14,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "vertical_align": "center",
        "position": {
          "x_in_px": 440.0,
          "y_in_px": 480.0
        },
        "dimensions": {
          "width_in_px": 180,
          "height_in_px": 40
        }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateImage({
  dimensions: {
    width_in_px: 970,
    height_in_px: 600,
  },
  output_format: "png",
  layers: [
    {
      index: 0,
      type: "gradient",
      gradient_type: "linear",
      angle_in_degrees: 135.0,
      colors: [
        {
          hex_color: "#1B2838",
          position: 0.0,
        },
        {
          hex_color: "#2D3A4A",
          position: 100.0,
        },
      ],
      position: {
        x_in_px: 0.0,
        y_in_px: 0.0,
      },
      dimensions: {
        width_in_px: 970,
        height_in_px: 600,
      },
    },
    {
      index: 1,
      type: "image",
      buffer: bookCoverBase64,
      position: {
        x_in_px: 60.0,
        y_in_px: 60.0,
      },
      dimensions: {
        width_in_px: 320,
        height_in_px: 480,
      },
    },
    {
      index: 2,
      type: "text",
      text: "THE ASHWORTH CHRONICLES",
      font_name: "Montserrat",
      font_size_in_px: 14,
      text_color: "#94A3B8",
      font_weight: "bold",
      position: {
        x_in_px: 440.0,
        y_in_px: 80.0,
      },
      dimensions: {
        width_in_px: 480,
        height_in_px: 24,
      },
    },
    {
      index: 3,
      type: "text",
      text: "The Long Silence",
      font_name: "PlayfairDisplay",
      font_size_in_px: 44,
      text_color: "#FFFFFF",
      font_weight: "bold",
      position: {
        x_in_px: 440.0,
        y_in_px: 120.0,
      },
      dimensions: {
        width_in_px: 480,
        height_in_px: 110,
      },
    },
    {
      index: 4,
      type: "text",
      text: "Elizabeth Ashworth",
      font_name: "Lora",
      font_size_in_px: 20,
      text_color: "#CBD5E1",
      position: {
        x_in_px: 440.0,
        y_in_px: 250.0,
      },
      dimensions: {
        width_in_px: 480,
        height_in_px: 32,
      },
    },
    {
      index: 5,
      type: "solid-color",
      hex_color: "#6C63FF",
      position: {
        x_in_px: 440.0,
        y_in_px: 320.0,
      },
      dimensions: {
        width_in_px: 120,
        height_in_px: 32,
      },
    },
    {
      index: 6,
      type: "text",
      text: "THRILLER",
      font_name: "Montserrat",
      font_size_in_px: 12,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      vertical_align: "center",
      position: {
        x_in_px: 440.0,
        y_in_px: 320.0,
      },
      dimensions: {
        width_in_px: 120,
        height_in_px: 32,
      },
    },
    {
      index: 7,
      type: "solid-color",
      hex_color: "#22C55E",
      position: {
        x_in_px: 440.0,
        y_in_px: 480.0,
      },
      dimensions: {
        width_in_px: 180,
        height_in_px: 40,
      },
    },
    {
      index: 8,
      type: "text",
      text: "Available Now",
      font_name: "Montserrat",
      font_size_in_px: 14,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      vertical_align: "center",
      position: {
        x_in_px: 440.0,
        y_in_px: 480.0,
      },
      dimensions: {
        width_in_px: 180,
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
        "width_in_px": 970,
        "height_in_px": 600,
    },
    output_format="png",
    layers=[
        {
            "index": 0,
            "type": "gradient",
            "gradient_type": "linear",
            "angle_in_degrees": 135.0,
            "colors": [
                {
                    "hex_color": "#1B2838",
                    "position": 0.0,
                },
                {
                    "hex_color": "#2D3A4A",
                    "position": 100.0,
                },
            ],
            "position": {
                "x_in_px": 0.0,
                "y_in_px": 0.0,
            },
            "dimensions": {
                "width_in_px": 970,
                "height_in_px": 600,
            },
        },
        {
            "index": 1,
            "type": "image",
            "buffer": book_cover_base64,
            "position": {
                "x_in_px": 60.0,
                "y_in_px": 60.0,
            },
            "dimensions": {
                "width_in_px": 320,
                "height_in_px": 480,
            },
        },
        {
            "index": 2,
            "type": "text",
            "text": "THE ASHWORTH CHRONICLES",
            "font_name": "Montserrat",
            "font_size_in_px": 14,
            "text_color": "#94A3B8",
            "font_weight": "bold",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 80.0,
            },
            "dimensions": {
                "width_in_px": 480,
                "height_in_px": 24,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "The Long Silence",
            "font_name": "PlayfairDisplay",
            "font_size_in_px": 44,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 120.0,
            },
            "dimensions": {
                "width_in_px": 480,
                "height_in_px": 110,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "Elizabeth Ashworth",
            "font_name": "Lora",
            "font_size_in_px": 20,
            "text_color": "#CBD5E1",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 250.0,
            },
            "dimensions": {
                "width_in_px": 480,
                "height_in_px": 32,
            },
        },
        {
            "index": 5,
            "type": "solid-color",
            "hex_color": "#6C63FF",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 320.0,
            },
            "dimensions": {
                "width_in_px": 120,
                "height_in_px": 32,
            },
        },
        {
            "index": 6,
            "type": "text",
            "text": "THRILLER",
            "font_name": "Montserrat",
            "font_size_in_px": 12,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "vertical_align": "center",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 320.0,
            },
            "dimensions": {
                "width_in_px": 120,
                "height_in_px": 32,
            },
        },
        {
            "index": 7,
            "type": "solid-color",
            "hex_color": "#22C55E",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 480.0,
            },
            "dimensions": {
                "width_in_px": 180,
                "height_in_px": 40,
            },
        },
        {
            "index": 8,
            "type": "text",
            "text": "Available Now",
            "font_name": "Montserrat",
            "font_size_in_px": 14,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "vertical_align": "center",
            "position": {
                "x_in_px": 440.0,
                "y_in_px": 480.0,
            },
            "dimensions": {
                "width_in_px": 180,
                "height_in_px": 40,
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

	result, err := client.GenerateImage(il.GenerateImageRequest{
		Dimensions:   il.Dimensions{
    WidthInPx: 970,
    HeightInPx: 600,
  },
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewGradientLayer(0, "linear", []il.GradientColor{
				{
      HexColor: "#1B2838",
      Position: 0.0,
    },
				{
      HexColor: "#2D3A4A",
      Position: 100.0,
    },
			}, il.Position{
     XInPx: 0.0,
     YInPx: 0.0,
   }, il.Dimensions{
     WidthInPx: 970,
     HeightInPx: 600,
   }),
			il.NewStaticImageLayer(1, bookCoverBase64,
				il.Position{
      XInPx: 60.0,
      YInPx: 60.0,
    },
				il.Dimensions{
      WidthInPx: 320,
      HeightInPx: 480,
    }),
			il.NewTextLayer(2, "THE ASHWORTH CHRONICLES", "Montserrat", 14, "#94A3B8",
				il.Position{
      XInPx: 440.0,
      YInPx: 80.0,
    },
				il.Dimensions{
      WidthInPx: 480,
      HeightInPx: 24,
    }),
			il.NewTextLayer(3, "The Long Silence", "PlayfairDisplay", 44, "#FFFFFF",
				il.Position{
      XInPx: 440.0,
      YInPx: 120.0,
    },
				il.Dimensions{
      WidthInPx: 480,
      HeightInPx: 110,
    }),
			il.NewTextLayer(4, "Elizabeth Ashworth", "Lora", 20, "#CBD5E1",
				il.Position{
      XInPx: 440.0,
      YInPx: 250.0,
    },
				il.Dimensions{
      WidthInPx: 480,
      HeightInPx: 32,
    }),
			il.NewRectangleLayer(5, "#6C63FF",
				il.Position{
      XInPx: 440.0,
      YInPx: 320.0,
    },
				il.Dimensions{
      WidthInPx: 120,
      HeightInPx: 32,
    }),
			il.NewTextLayer(6, "THRILLER", "Montserrat", 12, "#FFFFFF",
				il.Position{
      XInPx: 440.0,
      YInPx: 320.0,
    },
				il.Dimensions{
      WidthInPx: 120,
      HeightInPx: 32,
    }),
			il.NewRectangleLayer(7, "#22C55E",
				il.Position{
      XInPx: 440.0,
      YInPx: 480.0,
    },
				il.Dimensions{
      WidthInPx: 180,
      HeightInPx: 40,
    }),
			il.NewTextLayer(8, "Available Now", "Montserrat", 14, "#FFFFFF",
				il.Position{
      XInPx: 440.0,
      YInPx: 480.0,
    },
				il.Dimensions{
      WidthInPx: 180,
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
  "name": "Generate A+ Content Banner",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate A+ Content Banner\n\nSelf-publishing authors and book marketing teams use this recipe to generate Amazon A+ Content banners at the standard 970x600 module size. Compose a book cover image, title text, series name, genre label, and availability badge \u2014 ready to upload to Amazon Author Central.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "559fde02-4e03-4197-a2af-a8ed47636035",
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
      "id": "18e11dfd-dd4f-400c-983e-df6bf61e33c9",
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
      "id": "a1b2c3d4-1111-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 970,
        "heightInPx": 600,
        "outputFormat": "png",
        "layersJson": "[{\"index\":0,\"type\":\"gradient\",\"gradient_type\":\"linear\",\"angle_in_degrees\":135.0,\"colors\":[{\"hex_color\":\"#1B2838\",\"position\":0.0},{\"hex_color\":\"#2D3A4A\",\"position\":100.0}],\"position\":{\"x\":0.0,\"y\":0.0},\"dimensions\":{\"width\":970,\"height\":600}},{\"index\":1,\"type\":\"image\",\"buffer\":\"<base64-encoded-book-cover>\",\"position\":{\"x\":60.0,\"y\":60.0},\"dimensions\":{\"width\":320,\"height\":480}},{\"index\":2,\"type\":\"text\",\"text\":\"THE ASHWORTH CHRONICLES\",\"font_name\":\"Montserrat\",\"font_size_in_px\":14,\"text_color\":\"#94A3B8\",\"font_weight\":\"bold\",\"position\":{\"x\":440.0,\"y\":80.0},\"dimensions\":{\"width\":480,\"height\":24}},{\"index\":3,\"type\":\"text\",\"text\":\"The Long Silence\",\"font_name\":\"PlayfairDisplay\",\"font_size_in_px\":44,\"text_color\":\"#FFFFFF\",\"font_weight\":\"bold\",\"position\":{\"x\":440.0,\"y\":120.0},\"dimensions\":{\"width\":480,\"height\":110}},{\"index\":4,\"type\":\"text\",\"text\":\"Elizabeth Ashworth\",\"font_name\":\"Lora\",\"font_size_in_px\":20,\"text_color\":\"#CBD5E1\",\"position\":{\"x\":440.0,\"y\":250.0},\"dimensions\":{\"width\":480,\"height\":32}},{\"index\":5,\"type\":\"solid-color\",\"hex_color\":\"#6C63FF\",\"position\":{\"x\":440.0,\"y\":320.0},\"dimensions\":{\"width\":120,\"height\":32}},{\"index\":6,\"type\":\"text\",\"text\":\"THRILLER\",\"font_name\":\"Montserrat\",\"font_size_in_px\":12,\"text_color\":\"#FFFFFF\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"vertical_align\":\"center\",\"position\":{\"x\":440.0,\"y\":320.0},\"dimensions\":{\"width\":120,\"height\":32}},{\"index\":7,\"type\":\"solid-color\",\"hex_color\":\"#22C55E\",\"position\":{\"x\":440.0,\"y\":480.0},\"dimensions\":{\"width\":180,\"height\":40}},{\"index\":8,\"type\":\"text\",\"text\":\"Available Now\",\"font_name\":\"Montserrat\",\"font_size_in_px\":14,\"text_color\":\"#FFFFFF\",\"font_weight\":\"bold\",\"text_align\":\"center\",\"vertical_align\":\"center\",\"position\":{\"x\":440.0,\"y\":480.0},\"dimensions\":{\"width\":180,\"height\":40}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "a1b2c3d4-1111-4000-8000-000000000002",
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
Generate an Amazon A+ Content banner at 970x600 pixels. Use the generate_image tool with these layers:

1. Background: gradient layer from [dark color] to [lighter color] at 135 degrees
2. Book cover: image layer with [cover image] positioned left
3. Series name: text layer with [series name] in small caps
4. Title: text layer with [book title] in large bold serif font
5. Author: text layer with [author name]
6. Genre badge: solid-color rectangle with [genre] text overlay
7. CTA button: solid-color rectangle with "Available Now" text overlay
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
