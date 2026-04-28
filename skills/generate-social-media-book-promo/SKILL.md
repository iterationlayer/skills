---
name: generate-social-media-book-promo
description: Generate a vertical story image for TikTok or Instagram book promotion with cover art, hook text, and author branding.
---

# Generate Social Media Book Promo

Authors and book marketing teams use this recipe to generate vertical story images for TikTok and Instagram. Compose a gradient background, book cover, hook text, and author branding at 1080x1920 — ready to post or schedule across social platforms.

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
      "width_in_px": 1080,
      "height_in_px": 1920
    },
    "output_format": "png",
    "layers": [
      {
        "index": 0,
        "type": "gradient",
        "gradient_type": "linear",
        "angle_in_degrees": 180.0,
        "colors": [
          {
            "hex_color": "#0F0A1A",
            "position": 0.0
          },
          {
            "hex_color": "#1B1464",
            "position": 50.0
          },
          {
            "hex_color": "#2D1B69",
            "position": 100.0
          }
        ],
        "position": {
          "x_in_px": 0.0,
          "y_in_px": 0.0
        },
        "dimensions": {
          "width_in_px": 1080,
          "height_in_px": 1920
        }
      },
      {
        "index": 1,
        "type": "text",
        "text": "What if the person you trusted most\nwas the one keeping the secret?",
        "font_name": "Lora",
        "font_size_in_px": 36,
        "text_color": "#E2E8F0",
        "font_weight": "regular",
        "font_style": "Italic",
        "text_align": "center",
        "position": {
          "x_in_px": 80.0,
          "y_in_px": 140.0
        },
        "dimensions": {
          "width_in_px": 920,
          "height_in_px": 200
        }
      },
      {
        "index": 2,
        "type": "image",
        "buffer": "<base64-encoded-book-cover>",
        "position": {
          "x_in_px": 240.0,
          "y_in_px": 420.0
        },
        "dimensions": {
          "width_in_px": 600,
          "height_in_px": 900
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "THE ASHWORTH CHRONICLES",
        "font_name": "Montserrat",
        "font_size_in_px": 16,
        "text_color": "#6C63FF",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
          "x_in_px": 80.0,
          "y_in_px": 1400.0
        },
        "dimensions": {
          "width_in_px": 920,
          "height_in_px": 28
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "The Long Silence",
        "font_name": "PlayfairDisplay",
        "font_size_in_px": 48,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
          "x_in_px": 80.0,
          "y_in_px": 1450.0
        },
        "dimensions": {
          "width_in_px": 920,
          "height_in_px": 70
        }
      },
      {
        "index": 5,
        "type": "text",
        "text": "by Elizabeth Ashworth",
        "font_name": "Lora",
        "font_size_in_px": 22,
        "text_color": "#94A3B8",
        "text_align": "center",
        "position": {
          "x_in_px": 80.0,
          "y_in_px": 1540.0
        },
        "dimensions": {
          "width_in_px": 920,
          "height_in_px": 36
        }
      },
      {
        "index": 6,
        "type": "solid-color",
        "hex_color": "#6C63FF",
        "position": {
          "x_in_px": 340.0,
          "y_in_px": 1660.0
        },
        "dimensions": {
          "width_in_px": 400,
          "height_in_px": 56
        }
      },
      {
        "index": 7,
        "type": "text",
        "text": "Read the first chapter free",
        "font_name": "Montserrat",
        "font_size_in_px": 18,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "vertical_align": "center",
        "position": {
          "x_in_px": 340.0,
          "y_in_px": 1660.0
        },
        "dimensions": {
          "width_in_px": 400,
          "height_in_px": 56
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
    width_in_px: 1080,
    height_in_px: 1920,
  },
  output_format: "png",
  layers: [
    {
      index: 0,
      type: "gradient",
      gradient_type: "linear",
      angle_in_degrees: 180.0,
      colors: [
        {
          hex_color: "#0F0A1A",
          position: 0.0,
        },
        {
          hex_color: "#1B1464",
          position: 50.0,
        },
        {
          hex_color: "#2D1B69",
          position: 100.0,
        },
      ],
      position: {
        x_in_px: 0.0,
        y_in_px: 0.0,
      },
      dimensions: {
        width_in_px: 1080,
        height_in_px: 1920,
      },
    },
    {
      index: 1,
      type: "text",
      text: "What if the person you trusted most\nwas the one keeping the secret?",
      font_name: "Lora",
      font_size_in_px: 36,
      text_color: "#E2E8F0",
      font_weight: "regular",
      font_style: "Italic",
      text_align: "center",
      position: {
        x_in_px: 80.0,
        y_in_px: 140.0,
      },
      dimensions: {
        width_in_px: 920,
        height_in_px: 200,
      },
    },
    {
      index: 2,
      type: "image",
      buffer: bookCoverBase64,
      position: {
        x_in_px: 240.0,
        y_in_px: 420.0,
      },
      dimensions: {
        width_in_px: 600,
        height_in_px: 900,
      },
    },
    {
      index: 3,
      type: "text",
      text: "THE ASHWORTH CHRONICLES",
      font_name: "Montserrat",
      font_size_in_px: 16,
      text_color: "#6C63FF",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 80.0,
        y_in_px: 1400.0,
      },
      dimensions: {
        width_in_px: 920,
        height_in_px: 28,
      },
    },
    {
      index: 4,
      type: "text",
      text: "The Long Silence",
      font_name: "PlayfairDisplay",
      font_size_in_px: 48,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 80.0,
        y_in_px: 1450.0,
      },
      dimensions: {
        width_in_px: 920,
        height_in_px: 70,
      },
    },
    {
      index: 5,
      type: "text",
      text: "by Elizabeth Ashworth",
      font_name: "Lora",
      font_size_in_px: 22,
      text_color: "#94A3B8",
      text_align: "center",
      position: {
        x_in_px: 80.0,
        y_in_px: 1540.0,
      },
      dimensions: {
        width_in_px: 920,
        height_in_px: 36,
      },
    },
    {
      index: 6,
      type: "solid-color",
      hex_color: "#6C63FF",
      position: {
        x_in_px: 340.0,
        y_in_px: 1660.0,
      },
      dimensions: {
        width_in_px: 400,
        height_in_px: 56,
      },
    },
    {
      index: 7,
      type: "text",
      text: "Read the first chapter free",
      font_name: "Montserrat",
      font_size_in_px: 18,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      vertical_align: "center",
      position: {
        x_in_px: 340.0,
        y_in_px: 1660.0,
      },
      dimensions: {
        width_in_px: 400,
        height_in_px: 56,
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
        "width_in_px": 1080,
        "height_in_px": 1920,
    },
    output_format="png",
    layers=[
        {
            "index": 0,
            "type": "gradient",
            "gradient_type": "linear",
            "angle_in_degrees": 180.0,
            "colors": [
                {
                    "hex_color": "#0F0A1A",
                    "position": 0.0,
                },
                {
                    "hex_color": "#1B1464",
                    "position": 50.0,
                },
                {
                    "hex_color": "#2D1B69",
                    "position": 100.0,
                },
            ],
            "position": {
                "x_in_px": 0.0,
                "y_in_px": 0.0,
            },
            "dimensions": {
                "width_in_px": 1080,
                "height_in_px": 1920,
            },
        },
        {
            "index": 1,
            "type": "text",
            "text": "What if the person you trusted most\nwas the one keeping the secret?",
            "font_name": "Lora",
            "font_size_in_px": 36,
            "text_color": "#E2E8F0",
            "font_weight": "regular",
            "font_style": "Italic",
            "text_align": "center",
            "position": {
                "x_in_px": 80.0,
                "y_in_px": 140.0,
            },
            "dimensions": {
                "width_in_px": 920,
                "height_in_px": 200,
            },
        },
        {
            "index": 2,
            "type": "image",
            "buffer": book_cover_base64,
            "position": {
                "x_in_px": 240.0,
                "y_in_px": 420.0,
            },
            "dimensions": {
                "width_in_px": 600,
                "height_in_px": 900,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "THE ASHWORTH CHRONICLES",
            "font_name": "Montserrat",
            "font_size_in_px": 16,
            "text_color": "#6C63FF",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 80.0,
                "y_in_px": 1400.0,
            },
            "dimensions": {
                "width_in_px": 920,
                "height_in_px": 28,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "The Long Silence",
            "font_name": "PlayfairDisplay",
            "font_size_in_px": 48,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 80.0,
                "y_in_px": 1450.0,
            },
            "dimensions": {
                "width_in_px": 920,
                "height_in_px": 70,
            },
        },
        {
            "index": 5,
            "type": "text",
            "text": "by Elizabeth Ashworth",
            "font_name": "Lora",
            "font_size_in_px": 22,
            "text_color": "#94A3B8",
            "text_align": "center",
            "position": {
                "x_in_px": 80.0,
                "y_in_px": 1540.0,
            },
            "dimensions": {
                "width_in_px": 920,
                "height_in_px": 36,
            },
        },
        {
            "index": 6,
            "type": "solid-color",
            "hex_color": "#6C63FF",
            "position": {
                "x_in_px": 340.0,
                "y_in_px": 1660.0,
            },
            "dimensions": {
                "width_in_px": 400,
                "height_in_px": 56,
            },
        },
        {
            "index": 7,
            "type": "text",
            "text": "Read the first chapter free",
            "font_name": "Montserrat",
            "font_size_in_px": 18,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "vertical_align": "center",
            "position": {
                "x_in_px": 340.0,
                "y_in_px": 1660.0,
            },
            "dimensions": {
                "width_in_px": 400,
                "height_in_px": 56,
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
		Dimensions: il.Dimensions{
			WidthInPx:  1080,
			HeightInPx: 1920,
		},
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewGradientLayer(0, "linear", []il.GradientColor{
				{
      HexColor: "#0F0A1A",
      Position: 0.0,
    },
				{
      HexColor: "#1B1464",
      Position: 50.0,
    },
				{
      HexColor: "#2D1B69",
      Position: 100.0,
    },
			}, il.Position{
     XInPx: 0.0,
     YInPx: 0.0,
   }, il.Dimensions{
     WidthInPx: 1080,
     HeightInPx: 1920,
   }),
			il.NewTextLayer(1,
				"What if the person you trusted most\nwas the one keeping the secret?",
				"Lora", 36, "#E2E8F0",
				il.Position{
      XInPx: 80.0,
      YInPx: 140.0,
    },
				il.Dimensions{
      WidthInPx: 920,
      HeightInPx: 200,
    }),
			il.NewStaticImageLayer(2, bookCoverBase64,
				il.Position{
      XInPx: 240.0,
      YInPx: 420.0,
    },
				il.Dimensions{
      WidthInPx: 600,
      HeightInPx: 900,
    }),
			il.NewTextLayer(3, "THE ASHWORTH CHRONICLES", "Montserrat", 16, "#6C63FF",
				il.Position{
      XInPx: 80.0,
      YInPx: 1400.0,
    },
				il.Dimensions{
      WidthInPx: 920,
      HeightInPx: 28,
    }),
			il.NewTextLayer(4, "The Long Silence", "PlayfairDisplay", 48, "#FFFFFF",
				il.Position{
      XInPx: 80.0,
      YInPx: 1450.0,
    },
				il.Dimensions{
      WidthInPx: 920,
      HeightInPx: 70,
    }),
			il.NewTextLayer(5, "by Elizabeth Ashworth", "Lora", 22, "#94A3B8",
				il.Position{
      XInPx: 80.0,
      YInPx: 1540.0,
    },
				il.Dimensions{
      WidthInPx: 920,
      HeightInPx: 36,
    }),
			il.NewRectangleLayer(6, "#6C63FF",
				il.Position{
      XInPx: 340.0,
      YInPx: 1660.0,
    },
				il.Dimensions{
      WidthInPx: 400,
      HeightInPx: 56,
    }),
			il.NewTextLayer(7, "Read the first chapter free", "Montserrat", 18, "#FFFFFF",
				il.Position{
      XInPx: 340.0,
      YInPx: 1660.0,
    },
				il.Dimensions{
      WidthInPx: 400,
      HeightInPx: 56,
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
  "name": "Generate Social Media Book Promo",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Social Media Book Promo\n\nAuthors and book marketing teams use this recipe to generate vertical story images for TikTok and Instagram. Compose a gradient background, book cover, hook text, and author branding at 1080x1920 \u2014 ready to post or schedule across social platforms.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "4c65718a-1c0d-40b4-af41-25d064886f9b",
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
      "id": "2e1e04e2-34b9-4031-bdc2-6c4f2fd543dd",
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
      "id": "e7ee734d-fb00-482d-9e28-882a8ed6519a",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 1080,
        "heightInPx": 1920,
        "outputFormat": "png",
        "layersJson": "[\n  {\n    \"index\": 0,\n    \"type\": \"gradient\",\n    \"gradient_type\": \"linear\",\n    \"angle_in_degrees\": 180.0,\n    \"colors\": [\n      {\n        \"hex_color\": \"#0F0A1A\",\n        \"position\": 0.0\n      },\n      {\n        \"hex_color\": \"#1B1464\",\n        \"position\": 50.0\n      },\n      {\n        \"hex_color\": \"#2D1B69\",\n        \"position\": 100.0\n      }\n    ],\n    \"position\": {\n      \"x\": 0.0,\n      \"y\": 0.0\n    },\n    \"dimensions\": {\n      \"width\": 1080,\n      \"height\": 1920\n    }\n  },\n  {\n    \"index\": 1,\n    \"type\": \"text\",\n    \"text\": \"What if the person you trusted most\\nwas the one keeping the secret?\",\n    \"font_name\": \"Lora\",\n    \"font_size_in_px\": 36,\n    \"text_color\": \"#E2E8F0\",\n    \"font_weight\": \"regular\",\n    \"font_style\": \"Italic\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 80.0,\n      \"y\": 140.0\n    },\n    \"dimensions\": {\n      \"width\": 920,\n      \"height\": 200\n    }\n  },\n  {\n    \"index\": 2,\n    \"type\": \"image\",\n    \"buffer\": \"<base64-encoded-book-cover>\",\n    \"position\": {\n      \"x\": 240.0,\n      \"y\": 420.0\n    },\n    \"dimensions\": {\n      \"width\": 600,\n      \"height\": 900\n    }\n  },\n  {\n    \"index\": 3,\n    \"type\": \"text\",\n    \"text\": \"THE ASHWORTH CHRONICLES\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 16,\n    \"text_color\": \"#6C63FF\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 80.0,\n      \"y\": 1400.0\n    },\n    \"dimensions\": {\n      \"width\": 920,\n      \"height\": 28\n    }\n  },\n  {\n    \"index\": 4,\n    \"type\": \"text\",\n    \"text\": \"The Long Silence\",\n    \"font_name\": \"PlayfairDisplay\",\n    \"font_size_in_px\": 48,\n    \"text_color\": \"#FFFFFF\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 80.0,\n      \"y\": 1450.0\n    },\n    \"dimensions\": {\n      \"width\": 920,\n      \"height\": 70\n    }\n  },\n  {\n    \"index\": 5,\n    \"type\": \"text\",\n    \"text\": \"by Elizabeth Ashworth\",\n    \"font_name\": \"Lora\",\n    \"font_size_in_px\": 22,\n    \"text_color\": \"#94A3B8\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 80.0,\n      \"y\": 1540.0\n    },\n    \"dimensions\": {\n      \"width\": 920,\n      \"height\": 36\n    }\n  },\n  {\n    \"index\": 6,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#6C63FF\",\n    \"position\": {\n      \"x\": 340.0,\n      \"y\": 1660.0\n    },\n    \"dimensions\": {\n      \"width\": 400,\n      \"height\": 56\n    }\n  },\n  {\n    \"index\": 7,\n    \"type\": \"text\",\n    \"text\": \"Read the first chapter free\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 18,\n    \"text_color\": \"#FFFFFF\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"vertical_align\": \"center\",\n    \"position\": {\n      \"x\": 340.0,\n      \"y\": 1660.0\n    },\n    \"dimensions\": {\n      \"width\": 400,\n      \"height\": 56\n    }\n  }\n]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "e6420fa2-9ac1-4cfd-ab76-10f904748538",
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
Generate a vertical social media book promo image at 1080x1920 pixels. Use the generate_image tool with these layers:

1. Background: gradient layer with [color scheme]
2. Book cover: image layer with [cover image] centered
3. Hook text: text layer with [hook/tagline] in large bold text above the cover
4. Author name: text layer with [author name] below the cover
5. CTA: text layer with [call-to-action] (e.g., "Available Now") at the bottom
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
