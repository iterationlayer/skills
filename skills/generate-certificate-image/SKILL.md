---
name: generate-certificate-image
description: Generate a professional certificate image with recipient name, course title, and completion date for digital sharing, social media, or email delivery.
---

# Generate Certificate Image

Online course platforms and training providers use this recipe to generate a completion certificate on the fly. Define a canvas with an elegant background, add a decorative border, and render the recipient's name, course title, and date — ready for digital sharing, social media, or email delivery.

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
      "width_in_px": 1200,
      "height_in_px": 800
    },
    "layers": [
      {
        "index": 0,
        "type": "solid-color",
        "hex_color": "#FFFDF5"
      },
      {
        "index": 1,
        "type": "solid-color",
        "hex_color": "#C9A84C",
        "position": {
          "x_in_px": 30.0,
          "y_in_px": 30.0
        },
        "dimensions": {
          "width_in_px": 1140,
          "height_in_px": 740
        }
      },
      {
        "index": 2,
        "type": "solid-color",
        "hex_color": "#FFFDF5",
        "position": {
          "x_in_px": 40.0,
          "y_in_px": 40.0
        },
        "dimensions": {
          "width_in_px": 1120,
          "height_in_px": 720
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "Certificate of Completion",
        "font_name": "Playfair Display",
        "font_size_in_px": 42,
        "text_color": "#333333",
        "text_align": "center",
        "position": {
          "x_in_px": 100.0,
          "y_in_px": 100.0
        },
        "dimensions": {
          "width_in_px": 1000,
          "height_in_px": 60
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "Jane Smith",
        "font_name": "Playfair Display",
        "font_size_in_px": 56,
        "text_color": "#1a1a1a",
        "text_align": "center",
        "font_weight": "bold",
        "position": {
          "x_in_px": 100.0,
          "y_in_px": 280.0
        },
        "dimensions": {
          "width_in_px": 1000,
          "height_in_px": 80
        }
      },
      {
        "index": 5,
        "type": "text",
        "text": "Advanced API Design",
        "font_name": "Inter",
        "font_size_in_px": 28,
        "text_color": "#555555",
        "text_align": "center",
        "position": {
          "x_in_px": 100.0,
          "y_in_px": 420.0
        },
        "dimensions": {
          "width_in_px": 1000,
          "height_in_px": 50
        }
      },
      {
        "index": 6,
        "type": "text",
        "text": "March 5, 2026",
        "font_name": "Inter",
        "font_size_in_px": 20,
        "text_color": "#888888",
        "text_align": "center",
        "position": {
          "x_in_px": 100.0,
          "y_in_px": 600.0
        },
        "dimensions": {
          "width_in_px": 1000,
          "height_in_px": 40
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
    width_in_px: 1200,
    height_in_px: 800,
  },
  layers: [
    {
      index: 0,
      type: "solid-color",
      hex_color: "#FFFDF5",
    },
    {
      index: 1,
      type: "solid-color",
      hex_color: "#C9A84C",
      position: {
        x_in_px: 30,
        y_in_px: 30,
      },
      dimensions: {
        width_in_px: 1140,
        height_in_px: 740,
      },
    },
    {
      index: 2,
      type: "solid-color",
      hex_color: "#FFFDF5",
      position: {
        x_in_px: 40,
        y_in_px: 40,
      },
      dimensions: {
        width_in_px: 1120,
        height_in_px: 720,
      },
    },
    {
      index: 3,
      type: "text",
      text: "Certificate of Completion",
      font_name: "Playfair Display",
      font_size_in_px: 42,
      text_color: "#333333",
      text_align: "center",
      position: {
        x_in_px: 100,
        y_in_px: 100,
      },
      dimensions: {
        width_in_px: 1000,
        height_in_px: 60,
      },
    },
    {
      index: 4,
      type: "text",
      text: "Jane Smith",
      font_name: "Playfair Display",
      font_size_in_px: 56,
      text_color: "#1a1a1a",
      text_align: "center",
      font_weight: "bold",
      position: {
        x_in_px: 100,
        y_in_px: 280,
      },
      dimensions: {
        width_in_px: 1000,
        height_in_px: 80,
      },
    },
    {
      index: 5,
      type: "text",
      text: "Advanced API Design",
      font_name: "Inter",
      font_size_in_px: 28,
      text_color: "#555555",
      text_align: "center",
      position: {
        x_in_px: 100,
        y_in_px: 420,
      },
      dimensions: {
        width_in_px: 1000,
        height_in_px: 50,
      },
    },
    {
      index: 6,
      type: "text",
      text: "March 5, 2026",
      font_name: "Inter",
      font_size_in_px: 20,
      text_color: "#888888",
      text_align: "center",
      position: {
        x_in_px: 100,
        y_in_px: 600,
      },
      dimensions: {
        width_in_px: 1000,
        height_in_px: 40,
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
        "width_in_px": 1200,
        "height_in_px": 800,
    },
    layers=[
        {
            "index": 0,
            "type": "solid-color",
            "hex_color": "#FFFDF5",
        },
        {
            "index": 1,
            "type": "solid-color",
            "hex_color": "#C9A84C",
            "position": {
                "x_in_px": 30,
                "y_in_px": 30,
            },
            "dimensions": {
                "width_in_px": 1140,
                "height_in_px": 740,
            },
        },
        {
            "index": 2,
            "type": "solid-color",
            "hex_color": "#FFFDF5",
            "position": {
                "x_in_px": 40,
                "y_in_px": 40,
            },
            "dimensions": {
                "width_in_px": 1120,
                "height_in_px": 720,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "Certificate of Completion",
            "font_name": "Playfair Display",
            "font_size_in_px": 42,
            "text_color": "#333333",
            "text_align": "center",
            "position": {
                "x_in_px": 100,
                "y_in_px": 100,
            },
            "dimensions": {
                "width_in_px": 1000,
                "height_in_px": 60,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "Jane Smith",
            "font_name": "Playfair Display",
            "font_size_in_px": 56,
            "text_color": "#1a1a1a",
            "text_align": "center",
            "font_weight": "bold",
            "position": {
                "x_in_px": 100,
                "y_in_px": 280,
            },
            "dimensions": {
                "width_in_px": 1000,
                "height_in_px": 80,
            },
        },
        {
            "index": 5,
            "type": "text",
            "text": "Advanced API Design",
            "font_name": "Inter",
            "font_size_in_px": 28,
            "text_color": "#555555",
            "text_align": "center",
            "position": {
                "x_in_px": 100,
                "y_in_px": 420,
            },
            "dimensions": {
                "width_in_px": 1000,
                "height_in_px": 50,
            },
        },
        {
            "index": 6,
            "type": "text",
            "text": "March 5, 2026",
            "font_name": "Inter",
            "font_size_in_px": 20,
            "text_color": "#888888",
            "text_align": "center",
            "position": {
                "x_in_px": 100,
                "y_in_px": 600,
            },
            "dimensions": {
                "width_in_px": 1000,
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
    WidthInPx: 1200,
    HeightInPx: 800,
  },
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#FFFDF5"),
			il.NewRectangleLayer(1, "#C9A84C",
				il.Position{
      XInPx: 30,
      YInPx: 30,
    },
				il.Dimensions{
      WidthInPx: 1140,
      HeightInPx: 740,
    }),
			il.NewRectangleLayer(2, "#FFFDF5",
				il.Position{
      XInPx: 40,
      YInPx: 40,
    },
				il.Dimensions{
      WidthInPx: 1120,
      HeightInPx: 720,
    }),
			il.NewTextLayer(3, "Certificate of Completion", "Playfair Display", 42, "#333333",
				il.Position{
      XInPx: 100,
      YInPx: 100,
    },
				il.Dimensions{
      WidthInPx: 1000,
      HeightInPx: 60,
    }),
			il.NewTextLayer(4, "Jane Smith", "Playfair Display", 56, "#1a1a1a",
				il.Position{
      XInPx: 100,
      YInPx: 280,
    },
				il.Dimensions{
      WidthInPx: 1000,
      HeightInPx: 80,
    }),
			il.NewTextLayer(5, "Advanced API Design", "Inter", 28, "#555555",
				il.Position{
      XInPx: 100,
      YInPx: 420,
    },
				il.Dimensions{
      WidthInPx: 1000,
      HeightInPx: 50,
    }),
			il.NewTextLayer(6, "March 5, 2026", "Inter", 20, "#888888",
				il.Position{
      XInPx: 100,
      YInPx: 600,
    },
				il.Dimensions{
      WidthInPx: 1000,
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
  "name": "Generate Certificate Image",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Certificate Image\n\nOnline course platforms and training providers use this recipe to generate a completion certificate on the fly. Define a canvas with an elegant background, add a decorative border, and render the recipient's name, course title, and date \u2014 ready for digital sharing, social media, or email delivery.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "1a01d85c-c071-4b66-85a6-72a142bc3d8f",
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
      "id": "fb427096-5eed-4db4-a00a-6a709c63e4fe",
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
      "id": "b2c3d4e5-2222-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 1200,
        "heightInPx": 800,
        "outputFormat": "png",
        "layersJson": "[{\"index\":0,\"type\":\"solid-color\",\"hex_color\":\"#FFFDF5\"},{\"index\":1,\"type\":\"solid-color\",\"hex_color\":\"#C9A84C\",\"position\":{\"x\":30.0,\"y\":30.0},\"dimensions\":{\"width\":1140,\"height\":740}},{\"index\":2,\"type\":\"solid-color\",\"hex_color\":\"#FFFDF5\",\"position\":{\"x\":40.0,\"y\":40.0},\"dimensions\":{\"width\":1120,\"height\":720}},{\"index\":3,\"type\":\"text\",\"text\":\"Certificate of Completion\",\"font_name\":\"Playfair Display\",\"font_size_in_px\":42,\"text_color\":\"#333333\",\"text_align\":\"center\",\"position\":{\"x\":100.0,\"y\":100.0},\"dimensions\":{\"width\":1000,\"height\":60}},{\"index\":4,\"type\":\"text\",\"text\":\"Jane Smith\",\"font_name\":\"Playfair Display\",\"font_size_in_px\":56,\"text_color\":\"#1a1a1a\",\"text_align\":\"center\",\"font_weight\":\"bold\",\"position\":{\"x\":100.0,\"y\":280.0},\"dimensions\":{\"width\":1000,\"height\":80}},{\"index\":5,\"type\":\"text\",\"text\":\"Advanced API Design\",\"font_name\":\"Inter\",\"font_size_in_px\":28,\"text_color\":\"#555555\",\"text_align\":\"center\",\"position\":{\"x\":100.0,\"y\":420.0},\"dimensions\":{\"width\":1000,\"height\":50}},{\"index\":6,\"type\":\"text\",\"text\":\"March 5, 2026\",\"font_name\":\"Inter\",\"font_size_in_px\":20,\"text_color\":\"#888888\",\"text_align\":\"center\",\"position\":{\"x\":100.0,\"y\":600.0},\"dimensions\":{\"width\":1000,\"height\":40}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "b2c3d4e5-2222-4000-8000-000000000002",
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
Generate a certificate of completion image at 1200x800 pixels. Use the generate_image tool with these layers:

1. Background: solid-color layer with cream (#FFFDF5)
2. Border: solid-color layer with gold (#C9A84C) inset 30px
3. Inner fill: solid-color layer with cream inset 40px
4. Title: text layer with "Certificate of Completion" centered
5. Recipient: text layer with [recipient name] in large bold serif font
6. Course: text layer with [course title] centered
7. Date: text layer with [completion date] centered
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
