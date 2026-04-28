---
name: generate-book-cover-spreads
description: Generate print-ready book cover spreads with back cover, spine, and front cover in a single image.
---

# Generate Book Cover Spreads

Self-publishing platforms and print-on-demand services use this recipe to generate complete book cover spreads for KDP, IngramSpark, or other print-on-demand platforms. Calculate spine width from page count, compose back cover text with an ISBN barcode, add a rotated spine title, and place the front cover image — all in one API call at 300 DPI. Output is RGB (PNG, JPEG, or WebP); CMYK conversion for print-ready files can be done as a separate post-processing step.

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
      "width_in_px": 3826,
      "height_in_px": 2775
    },
    "output_format": "png",
    "layers": [
      {
        "index": 0,
        "type": "solid-color",
        "hex_color": "#1B2838"
      },
      {
        "index": 1,
        "type": "image",
        "buffer": "<base64-encoded-front-cover>",
        "position": {
          "x_in_px": 1988.0,
          "y_in_px": 0.0
        },
        "dimensions": {
          "width_in_px": 1838,
          "height_in_px": 2775
        }
      },
      {
        "index": 2,
        "type": "text",
        "text": "A THRILLER NOVEL",
        "font_name": "Montserrat",
        "font_size_in_px": 30,
        "text_color": "#94A3B8",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
          "x_in_px": 150.0,
          "y_in_px": 225.0
        },
        "dimensions": {
          "width_in_px": 1575,
          "height_in_px": 50
        }
      },
      {
        "index": 3,
        "type": "text",
        "text": "The Last Algorithm",
        "font_name": "PlayfairDisplay",
        "font_size_in_px": 72,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "position": {
          "x_in_px": 150.0,
          "y_in_px": 325.0
        },
        "dimensions": {
          "width_in_px": 1575,
          "height_in_px": 150
        }
      },
      {
        "index": 4,
        "type": "text",
        "text": "A rogue AI researcher discovers that the optimization algorithm she created has already escaped the lab — and it is rewriting the rules of global finance. Now she has 72 hours to shut it down before it becomes impossible to stop.",
        "font_name": "Lora",
        "font_size_in_px": 28,
        "text_color": "#CBD5E1",
        "text_align": "center",
        "position": {
          "x_in_px": 200.0,
          "y_in_px": 600.0
        },
        "dimensions": {
          "width_in_px": 1475,
          "height_in_px": 500
        }
      },
      {
        "index": 5,
        "type": "text",
        "text": "ELENA VASQUEZ",
        "font_name": "Montserrat",
        "font_size_in_px": 32,
        "text_color": "#94A3B8",
        "text_align": "center",
        "position": {
          "x_in_px": 150.0,
          "y_in_px": 1200.0
        },
        "dimensions": {
          "width_in_px": 1575,
          "height_in_px": 50
        }
      },
      {
        "index": 6,
        "type": "solid-color",
        "hex_color": "#FFFFFF",
        "position": {
          "x_in_px": 225.0,
          "y_in_px": 2250.0
        },
        "dimensions": {
          "width_in_px": 450,
          "height_in_px": 350
        }
      },
      {
        "index": 7,
        "type": "barcode",
        "value": "9781234567890",
        "format": "ean13",
        "position": {
          "x_in_px": 250.0,
          "y_in_px": 2275.0
        },
        "dimensions": {
          "width_in_px": 400,
          "height_in_px": 250
        },
        "foreground_hex_color": "#000000",
        "background_hex_color": "#FFFFFF"
      },
      {
        "index": 8,
        "type": "text",
        "text": "The Last Algorithm",
        "font_name": "Montserrat",
        "font_size_in_px": 24,
        "text_color": "#FFFFFF",
        "font_weight": "bold",
        "text_align": "center",
        "vertical_align": "center",
        "rotation_in_degrees": -90,
        "position": {
          "x_in_px": 1838.0,
          "y_in_px": 375.0
        },
        "dimensions": {
          "width_in_px": 150,
          "height_in_px": 2025
        }
      },
      {
        "index": 9,
        "type": "text",
        "text": "ELENA VASQUEZ",
        "font_name": "Montserrat",
        "font_size_in_px": 18,
        "text_color": "#CBD5E1",
        "text_align": "center",
        "vertical_align": "center",
        "rotation_in_degrees": -90,
        "position": {
          "x_in_px": 1838.0,
          "y_in_px": 150.0
        },
        "dimensions": {
          "width_in_px": 150,
          "height_in_px": 225
        }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const BLEED_IN_PX = 38;
const TRIM_WIDTH_IN_PX = 1800;
const TRIM_HEIGHT_IN_PX = 2700;
const PAGE_COUNT = 200;
const SPINE_WIDTH_IN_PX = Math.round(PAGE_COUNT * 0.75);
const COVER_WIDTH_IN_PX = TRIM_WIDTH_IN_PX + BLEED_IN_PX;
const TOTAL_WIDTH_IN_PX = COVER_WIDTH_IN_PX * 2 + SPINE_WIDTH_IN_PX;
const TOTAL_HEIGHT_IN_PX = TRIM_HEIGHT_IN_PX + BLEED_IN_PX * 2;
const SPINE_X_IN_PX = COVER_WIDTH_IN_PX;
const FRONT_COVER_X_IN_PX = COVER_WIDTH_IN_PX + SPINE_WIDTH_IN_PX;

const result = await client.generateImage({
  dimensions: {
    width_in_px: TOTAL_WIDTH_IN_PX,
    height_in_px: TOTAL_HEIGHT_IN_PX,
  },
  output_format: "png",
  layers: [
    {
      index: 0,
      type: "solid-color",
      hex_color: "#1B2838",
    },
    {
      index: 1,
      type: "image",
      buffer: frontCoverBase64,
      position: {
        x_in_px: FRONT_COVER_X_IN_PX,
        y_in_px: 0,
      },
      dimensions: {
        width_in_px: COVER_WIDTH_IN_PX,
        height_in_px: TOTAL_HEIGHT_IN_PX,
      },
    },
    {
      index: 2,
      type: "text",
      text: "A THRILLER NOVEL",
      font_name: "Montserrat",
      font_size_in_px: 30,
      text_color: "#94A3B8",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 150,
        y_in_px: 225,
      },
      dimensions: {
        width_in_px: 1575,
        height_in_px: 50,
      },
    },
    {
      index: 3,
      type: "text",
      text: "The Last Algorithm",
      font_name: "PlayfairDisplay",
      font_size_in_px: 72,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      position: {
        x_in_px: 150,
        y_in_px: 325,
      },
      dimensions: {
        width_in_px: 1575,
        height_in_px: 150,
      },
    },
    {
      index: 4,
      type: "text",
      text: "A rogue AI researcher discovers that the optimization algorithm she created has already escaped the lab — and it is rewriting the rules of global finance. Now she has 72 hours to shut it down before it becomes impossible to stop.",
      font_name: "Lora",
      font_size_in_px: 28,
      text_color: "#CBD5E1",
      text_align: "center",
      position: {
        x_in_px: 200,
        y_in_px: 600,
      },
      dimensions: {
        width_in_px: 1475,
        height_in_px: 500,
      },
    },
    {
      index: 5,
      type: "text",
      text: "ELENA VASQUEZ",
      font_name: "Montserrat",
      font_size_in_px: 32,
      text_color: "#94A3B8",
      text_align: "center",
      position: {
        x_in_px: 150,
        y_in_px: 1200,
      },
      dimensions: {
        width_in_px: 1575,
        height_in_px: 50,
      },
    },
    {
      index: 6,
      type: "solid-color",
      hex_color: "#FFFFFF",
      position: {
        x_in_px: 225,
        y_in_px: 2250,
      },
      dimensions: {
        width_in_px: 450,
        height_in_px: 350,
      },
    },
    {
      index: 7,
      type: "barcode",
      value: "9781234567890",
      format: "ean13",
      position: {
        x_in_px: 250,
        y_in_px: 2275,
      },
      dimensions: {
        width_in_px: 400,
        height_in_px: 250,
      },
      foreground_hex_color: "#000000",
      background_hex_color: "#FFFFFF",
    },
    {
      index: 8,
      type: "text",
      text: "The Last Algorithm",
      font_name: "Montserrat",
      font_size_in_px: 24,
      text_color: "#FFFFFF",
      font_weight: "bold",
      text_align: "center",
      vertical_align: "center",
      rotation_in_degrees: -90,
      position: {
        x_in_px: SPINE_X_IN_PX,
        y_in_px: 375,
      },
      dimensions: {
        width_in_px: SPINE_WIDTH_IN_PX,
        height_in_px: 2025,
      },
    },
    {
      index: 9,
      type: "text",
      text: "ELENA VASQUEZ",
      font_name: "Montserrat",
      font_size_in_px: 18,
      text_color: "#CBD5E1",
      text_align: "center",
      vertical_align: "center",
      rotation_in_degrees: -90,
      position: {
        x_in_px: SPINE_X_IN_PX,
        y_in_px: 150,
      },
      dimensions: {
        width_in_px: SPINE_WIDTH_IN_PX,
        height_in_px: 225,
      },
    },
  ],
});
```

```python
import math
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

bleed_in_px = 38
trim_width = 1800
trim_height = 2700
page_count = 200
spine_width = round(page_count * 0.75)
cover_width = trim_width + bleed_in_px
total_width = cover_width * 2 + spine_width
total_height = trim_height + bleed_in_px * 2
spine_x = cover_width
front_cover_x = cover_width + spine_width

result = client.generate_image(
    dimensions={
        "width_in_px": total_width,
        "height_in_px": total_height,
    },
    output_format="png",
    layers=[
        {
            "index": 0,
            "type": "solid-color",
            "hex_color": "#1B2838",
        },
        {
            "index": 1,
            "type": "image",
            "buffer": front_cover_base64,
            "position": {
                "x_in_px": front_cover_x,
                "y_in_px": 0,
            },
            "dimensions": {
                "width_in_px": cover_width,
                "height_in_px": total_height,
            },
        },
        {
            "index": 2,
            "type": "text",
            "text": "A THRILLER NOVEL",
            "font_name": "Montserrat",
            "font_size_in_px": 30,
            "text_color": "#94A3B8",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 150,
                "y_in_px": 225,
            },
            "dimensions": {
                "width_in_px": 1575,
                "height_in_px": 50,
            },
        },
        {
            "index": 3,
            "type": "text",
            "text": "The Last Algorithm",
            "font_name": "PlayfairDisplay",
            "font_size_in_px": 72,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "position": {
                "x_in_px": 150,
                "y_in_px": 325,
            },
            "dimensions": {
                "width_in_px": 1575,
                "height_in_px": 150,
            },
        },
        {
            "index": 4,
            "type": "text",
            "text": "A rogue AI researcher discovers that the optimization algorithm she created has already escaped the lab — and it is rewriting the rules of global finance. Now she has 72 hours to shut it down before it becomes impossible to stop.",
            "font_name": "Lora",
            "font_size_in_px": 28,
            "text_color": "#CBD5E1",
            "text_align": "center",
            "position": {
                "x_in_px": 200,
                "y_in_px": 600,
            },
            "dimensions": {
                "width_in_px": 1475,
                "height_in_px": 500,
            },
        },
        {
            "index": 5,
            "type": "text",
            "text": "ELENA VASQUEZ",
            "font_name": "Montserrat",
            "font_size_in_px": 32,
            "text_color": "#94A3B8",
            "text_align": "center",
            "position": {
                "x_in_px": 150,
                "y_in_px": 1200,
            },
            "dimensions": {
                "width_in_px": 1575,
                "height_in_px": 50,
            },
        },
        {
            "index": 6,
            "type": "solid-color",
            "hex_color": "#FFFFFF",
            "position": {
                "x_in_px": 225,
                "y_in_px": 2250,
            },
            "dimensions": {
                "width_in_px": 450,
                "height_in_px": 350,
            },
        },
        {
            "index": 7,
            "type": "barcode",
            "value": "9781234567890",
            "format": "ean13",
            "position": {
                "x_in_px": 250,
                "y_in_px": 2275,
            },
            "dimensions": {
                "width_in_px": 400,
                "height_in_px": 250,
            },
            "foreground_hex_color": "#000000",
            "background_hex_color": "#FFFFFF",
        },
        {
            "index": 8,
            "type": "text",
            "text": "The Last Algorithm",
            "font_name": "Montserrat",
            "font_size_in_px": 24,
            "text_color": "#FFFFFF",
            "font_weight": "bold",
            "text_align": "center",
            "vertical_align": "center",
            "rotation_in_degrees": -90,
            "position": {
                "x_in_px": spine_x,
                "y_in_px": 375,
            },
            "dimensions": {
                "width_in_px": spine_width,
                "height_in_px": 2025,
            },
        },
        {
            "index": 9,
            "type": "text",
            "text": "ELENA VASQUEZ",
            "font_name": "Montserrat",
            "font_size_in_px": 18,
            "text_color": "#CBD5E1",
            "text_align": "center",
            "vertical_align": "center",
            "rotation_in_degrees": -90,
            "position": {
                "x_in_px": spine_x,
                "y_in_px": 150,
            },
            "dimensions": {
                "width_in_px": spine_width,
                "height_in_px": 225,
            },
        },
    ],
)
```

```go
package main

import (
	"math"

	il "github.com/iterationlayer/sdk-go"
)

func main() {
	client := il.NewClient("YOUR_API_KEY")

	bleedInPx := 38
	trimWidthInPx := 1800
	trimHeightInPx := 2700
	pageCount := 200
	spineWidthInPx := int(math.Round(float64(pageCount) * 0.75))
	coverWidthInPx := trimWidthInPx + bleedInPx
	totalWidthInPx := coverWidthInPx*2 + spineWidthInPx
	totalHeightInPx := trimHeightInPx + bleedInPx*2
	spineXInPx := coverWidthInPx
	frontCoverXInPx := coverWidthInPx + spineWidthInPx

	result, err := client.GenerateImage(il.GenerateImageRequest{
		Dimensions:   il.Dimensions{
    WidthInPx: totalWidthInPx,
    HeightInPx: totalHeightInPx,
  },
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#1B2838"),
			il.NewStaticImageLayer(1, frontCoverBase64,
				il.Position{
      XInPx: float64(frontCoverXInPx),
      YInPx: 0,
    },
				il.Dimensions{
      WidthInPx: coverWidthInPx,
      HeightInPx: totalHeightInPx,
    }),
			il.NewTextLayer(2, "A THRILLER NOVEL", "Montserrat", 30, "#94A3B8",
				il.Position{
      XInPx: 150,
      YInPx: 225,
    },
				il.Dimensions{
      WidthInPx: 1575,
      HeightInPx: 50,
    }),
			il.NewTextLayer(3, "The Last Algorithm", "PlayfairDisplay", 72, "#FFFFFF",
				il.Position{
      XInPx: 150,
      YInPx: 325,
    },
				il.Dimensions{
      WidthInPx: 1575,
      HeightInPx: 150,
    }),
			il.NewTextLayer(4,
				"A rogue AI researcher discovers that the optimization algorithm she created has already escaped the lab — and it is rewriting the rules of global finance. Now she has 72 hours to shut it down before it becomes impossible to stop.",
				"Lora", 28, "#CBD5E1",
				il.Position{
      XInPx: 200,
      YInPx: 600,
    },
				il.Dimensions{
      WidthInPx: 1475,
      HeightInPx: 500,
    }),
			il.NewTextLayer(5, "ELENA VASQUEZ", "Montserrat", 32, "#94A3B8",
				il.Position{
      XInPx: 150,
      YInPx: 1200,
    },
				il.Dimensions{
      WidthInPx: 1575,
      HeightInPx: 50,
    }),
			il.NewRectangleLayer(6, "#FFFFFF",
				il.Position{
      XInPx: 225,
      YInPx: 2250,
    },
				il.Dimensions{
      WidthInPx: 450,
      HeightInPx: 350,
    }),
			il.NewBarcodeLayer(7, "9781234567890", "ean13",
				il.Position{
      XInPx: 250,
      YInPx: 2275,
    },
				il.Dimensions{
      WidthInPx: 400,
      HeightInPx: 250,
    },
				"#000000", "#FFFFFF"),
			il.NewTextLayerWithRotation(8, "The Last Algorithm", "Montserrat", 24, "#FFFFFF",
				il.Position{
      XInPx: float64(spineXInPx),
      YInPx: 375,
    },
				il.Dimensions{
      WidthInPx: spineWidthInPx,
      HeightInPx: 2025,
    }, -90),
			il.NewTextLayerWithRotation(9, "ELENA VASQUEZ", "Montserrat", 18, "#CBD5E1",
				il.Position{
      XInPx: float64(spineXInPx),
      YInPx: 150,
    },
				il.Dimensions{
      WidthInPx: spineWidthInPx,
      HeightInPx: 225,
    }, -90),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Generate Book Cover Spreads",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Book Cover Spreads\n\nSelf-publishing platforms and print-on-demand services use this recipe to generate complete book cover spreads for KDP, IngramSpark, or other print-on-demand platforms. Calculate spine width from page count, compose back cover text with an ISBN barcode, add a rotated spine title, and place the front cover image \u2014 all in one API call at 300 DPI. Output is RGB (PNG, JPEG, or WebP); CMYK conversion for print-ready files can be done as a separate post-processing step.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "fd237e28-1b9f-4710-8eb0-3ba45f5d22da",
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
      "id": "425da277-5d57-4eea-ac6e-6ba2438911cc",
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
      "id": "6e005a8f-c567-47db-9ece-698d2410926d",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 3826,
        "heightInPx": 2775,
        "outputFormat": "png",
        "layersJson": "[\n  {\n    \"index\": 0,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#1B2838\"\n  },\n  {\n    \"index\": 1,\n    \"type\": \"image\",\n    \"buffer\": \"<base64-encoded-front-cover>\",\n    \"position\": {\n      \"x\": 1988.0,\n      \"y\": 0.0\n    },\n    \"dimensions\": {\n      \"width\": 1838,\n      \"height\": 2775\n    }\n  },\n  {\n    \"index\": 2,\n    \"type\": \"text\",\n    \"text\": \"A THRILLER NOVEL\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 30,\n    \"text_color\": \"#94A3B8\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 150.0,\n      \"y\": 225.0\n    },\n    \"dimensions\": {\n      \"width\": 1575,\n      \"height\": 50\n    }\n  },\n  {\n    \"index\": 3,\n    \"type\": \"text\",\n    \"text\": \"The Last Algorithm\",\n    \"font_name\": \"PlayfairDisplay\",\n    \"font_size_in_px\": 72,\n    \"text_color\": \"#FFFFFF\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 150.0,\n      \"y\": 325.0\n    },\n    \"dimensions\": {\n      \"width\": 1575,\n      \"height\": 150\n    }\n  },\n  {\n    \"index\": 4,\n    \"type\": \"text\",\n    \"text\": \"A rogue AI researcher discovers that the optimization algorithm she created has already escaped the lab \\u2014 and it is rewriting the rules of global finance. Now she has 72 hours to shut it down before it becomes impossible to stop.\",\n    \"font_name\": \"Lora\",\n    \"font_size_in_px\": 28,\n    \"text_color\": \"#CBD5E1\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 200.0,\n      \"y\": 600.0\n    },\n    \"dimensions\": {\n      \"width\": 1475,\n      \"height\": 500\n    }\n  },\n  {\n    \"index\": 5,\n    \"type\": \"text\",\n    \"text\": \"ELENA VASQUEZ\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 32,\n    \"text_color\": \"#94A3B8\",\n    \"text_align\": \"center\",\n    \"position\": {\n      \"x\": 150.0,\n      \"y\": 1200.0\n    },\n    \"dimensions\": {\n      \"width\": 1575,\n      \"height\": 50\n    }\n  },\n  {\n    \"index\": 6,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#FFFFFF\",\n    \"position\": {\n      \"x\": 225.0,\n      \"y\": 2250.0\n    },\n    \"dimensions\": {\n      \"width\": 450,\n      \"height\": 350\n    }\n  },\n  {\n    \"index\": 7,\n    \"type\": \"barcode\",\n    \"value\": \"9781234567890\",\n    \"format\": \"ean13\",\n    \"position\": {\n      \"x\": 250.0,\n      \"y\": 2275.0\n    },\n    \"dimensions\": {\n      \"width\": 400,\n      \"height\": 250\n    },\n    \"foreground_hex_color\": \"#000000\",\n    \"background_hex_color\": \"#FFFFFF\"\n  },\n  {\n    \"index\": 8,\n    \"type\": \"text\",\n    \"text\": \"The Last Algorithm\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 24,\n    \"text_color\": \"#FFFFFF\",\n    \"font_weight\": \"bold\",\n    \"text_align\": \"center\",\n    \"vertical_align\": \"center\",\n    \"rotation_in_degrees\": -90,\n    \"position\": {\n      \"x\": 1838.0,\n      \"y\": 375.0\n    },\n    \"dimensions\": {\n      \"width\": 150,\n      \"height\": 2025\n    }\n  },\n  {\n    \"index\": 9,\n    \"type\": \"text\",\n    \"text\": \"ELENA VASQUEZ\",\n    \"font_name\": \"Montserrat\",\n    \"font_size_in_px\": 18,\n    \"text_color\": \"#CBD5E1\",\n    \"text_align\": \"center\",\n    \"vertical_align\": \"center\",\n    \"rotation_in_degrees\": -90,\n    \"position\": {\n      \"x\": 1838.0,\n      \"y\": 150.0\n    },\n    \"dimensions\": {\n      \"width\": 150,\n      \"height\": 225\n    }\n  }\n]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "f7745cd4-8d8f-41d0-a30a-7e98ed00c78b",
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
Generate a print-ready book cover spread image. Use the generate_image tool with dimensions calculated from [page count] (spine width = pages x 0.75px) and these layers:

1. Background: solid-color layer for back cover area
2. Front cover: image layer with [cover image] positioned on the right half
3. Back cover text: genre label, title, blurb, and author name as text layers
4. ISBN barcode: barcode layer with [ISBN] in EAN-13 format on the back cover
5. Spine: rotated text layers with title and author name
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
