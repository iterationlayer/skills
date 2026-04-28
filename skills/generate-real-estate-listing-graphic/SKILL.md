---
name: generate-real-estate-listing-graphic
description: Generate a branded property listing graphic with a property photo, status badge, price, address, and key stats.
---

# Generate Real Estate Listing Graphic

Real estate agencies and property platforms use this recipe to generate listing graphics for social media, email campaigns, and digital signage. Feed in property data and a photo URL to get a print-ready branded card — no design software, no manual exports.

## APIs Used

Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST \
  https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": { "width_in_px": 1080, "height_in_px": 1080 },
    "output_format": "jpeg",
    "layers": [
      {
        "index": 0,
        "type": "image",
        "file": {
          "type": "url",
          "name": "property.jpg",
          "url": "https://example.com/properties/412-ridgewood.jpg"
        },
        "dimensions": { "width_in_px": 1080, "height_in_px": 720 }
      },
      {
        "index": 1,
        "type": "solid-color",
        "hex_color": "#1A1A2E",
        "position": { "x_in_px": 0, "y_in_px": 720 },
        "dimensions": { "width_in_px": 1080, "height_in_px": 360 }
      },
      {
        "index": 2,
        "type": "solid-color",
        "hex_color": "#E63946",
        "position": { "x_in_px": 40, "y_in_px": 40 },
        "dimensions": { "width_in_px": 180, "height_in_px": 48 },
        "border_radius": 6
      },
      {
        "index": 3,
        "type": "text",
        "text": "JUST LISTED",
        "font_name": "Inter",
        "font_size_in_px": 18,
        "font_weight": "bold",
        "text_color": "#FFFFFF",
        "text_align": "center",
        "vertical_align": "center",
        "position": { "x_in_px": 40, "y_in_px": 40 },
        "dimensions": { "width_in_px": 180, "height_in_px": 48 }
      },
      {
        "index": 4,
        "type": "text",
        "text": "$685,000",
        "font_name": "Inter",
        "font_size_in_px": 52,
        "font_weight": "bold",
        "text_color": "#FFFFFF",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 40, "y_in_px": 740 },
        "dimensions": { "width_in_px": 700, "height_in_px": 72 }
      },
      {
        "index": 5,
        "type": "text",
        "text": "412 Ridgewood Lane, Denver, CO 80203",
        "font_name": "Inter",
        "font_size_in_px": 22,
        "font_weight": "regular",
        "text_color": "#CBD5E1",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 40, "y_in_px": 820 },
        "dimensions": { "width_in_px": 900, "height_in_px": 36 }
      },
      {
        "index": 6,
        "type": "text",
        "text": "4 bed  ·  2.5 bath  ·  2,140 sq ft",
        "font_name": "Inter",
        "font_size_in_px": 20,
        "font_weight": "regular",
        "text_color": "#94A3B8",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 40, "y_in_px": 870 },
        "dimensions": { "width_in_px": 900, "height_in_px": 36 }
      },
      {
        "index": 7,
        "type": "text",
        "text": "Acme Realty",
        "font_name": "Inter",
        "font_size_in_px": 18,
        "font_weight": "semibold",
        "text_color": "#64748B",
        "text_align": "right",
        "vertical_align": "center",
        "position": { "x_in_px": 40, "y_in_px": 1020 },
        "dimensions": { "width_in_px": 1000, "height_in_px": 36 }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateImage({
  dimensions: { width_in_px: 1080, height_in_px: 1080 },
  output_format: "jpeg",
  layers: [
    {
      index: 0,
      type: "image",
      file: {
        type: "url",
        name: "property.jpg",
        url: "https://example.com/properties/412-ridgewood.jpg",
      },
      dimensions: { width_in_px: 1080, height_in_px: 720 },
    },
    {
      index: 1,
      type: "solid-color",
      hex_color: "#1A1A2E",
      position: { x_in_px: 0, y_in_px: 720 },
      dimensions: { width_in_px: 1080, height_in_px: 360 },
    },
    {
      index: 2,
      type: "solid-color",
      hex_color: "#E63946",
      position: { x_in_px: 40, y_in_px: 40 },
      dimensions: { width_in_px: 180, height_in_px: 48 },
      border_radius: 6,
    },
    {
      index: 3,
      type: "text",
      text: "JUST LISTED",
      font_name: "Inter",
      font_size_in_px: 18,
      font_weight: "bold",
      text_color: "#FFFFFF",
      text_align: "center",
      vertical_align: "center",
      position: { x_in_px: 40, y_in_px: 40 },
      dimensions: { width_in_px: 180, height_in_px: 48 },
    },
    {
      index: 4,
      type: "text",
      text: "$685,000",
      font_name: "Inter",
      font_size_in_px: 52,
      font_weight: "bold",
      text_color: "#FFFFFF",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 40, y_in_px: 740 },
      dimensions: { width_in_px: 700, height_in_px: 72 },
    },
    {
      index: 5,
      type: "text",
      text: "412 Ridgewood Lane, Denver, CO 80203",
      font_name: "Inter",
      font_size_in_px: 22,
      font_weight: "regular",
      text_color: "#CBD5E1",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 40, y_in_px: 820 },
      dimensions: { width_in_px: 900, height_in_px: 36 },
    },
    {
      index: 6,
      type: "text",
      text: "4 bed  ·  2.5 bath  ·  2,140 sq ft",
      font_name: "Inter",
      font_size_in_px: 20,
      font_weight: "regular",
      text_color: "#94A3B8",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 40, y_in_px: 870 },
      dimensions: { width_in_px: 900, height_in_px: 36 },
    },
    {
      index: 7,
      type: "text",
      text: "Acme Realty",
      font_name: "Inter",
      font_size_in_px: 18,
      font_weight: "semibold",
      text_color: "#64748B",
      text_align: "right",
      vertical_align: "center",
      position: { x_in_px: 40, y_in_px: 1020 },
      dimensions: { width_in_px: 1000, height_in_px: 36 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_image(
    dimensions={"width_in_px": 1080, "height_in_px": 1080},
    output_format="jpeg",
    layers=[
        {
            "index": 0,
            "type": "image",
            "file": {
                "type": "url",
                "name": "property.jpg",
                "url": "https://example.com/properties/412-ridgewood.jpg",
            },
            "dimensions": {"width_in_px": 1080, "height_in_px": 720},
        },
        {
            "index": 1,
            "type": "solid-color",
            "hex_color": "#1A1A2E",
            "position": {"x_in_px": 0, "y_in_px": 720},
            "dimensions": {"width_in_px": 1080, "height_in_px": 360},
        },
        {
            "index": 2,
            "type": "solid-color",
            "hex_color": "#E63946",
            "position": {"x_in_px": 40, "y_in_px": 40},
            "dimensions": {"width_in_px": 180, "height_in_px": 48},
            "border_radius": 6,
        },
        {
            "index": 3,
            "type": "text",
            "text": "JUST LISTED",
            "font_name": "Inter",
            "font_size_in_px": 18,
            "font_weight": "bold",
            "text_color": "#FFFFFF",
            "text_align": "center",
            "vertical_align": "center",
            "position": {"x_in_px": 40, "y_in_px": 40},
            "dimensions": {"width_in_px": 180, "height_in_px": 48},
        },
        {
            "index": 4,
            "type": "text",
            "text": "$685,000",
            "font_name": "Inter",
            "font_size_in_px": 52,
            "font_weight": "bold",
            "text_color": "#FFFFFF",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 40, "y_in_px": 740},
            "dimensions": {"width_in_px": 700, "height_in_px": 72},
        },
        {
            "index": 5,
            "type": "text",
            "text": "412 Ridgewood Lane, Denver, CO 80203",
            "font_name": "Inter",
            "font_size_in_px": 22,
            "font_weight": "regular",
            "text_color": "#CBD5E1",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 40, "y_in_px": 820},
            "dimensions": {"width_in_px": 900, "height_in_px": 36},
        },
        {
            "index": 6,
            "type": "text",
            "text": "4 bed  ·  2.5 bath  ·  2,140 sq ft",
            "font_name": "Inter",
            "font_size_in_px": 20,
            "font_weight": "regular",
            "text_color": "#94A3B8",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 40, "y_in_px": 870},
            "dimensions": {"width_in_px": 900, "height_in_px": 36},
        },
        {
            "index": 7,
            "type": "text",
            "text": "Acme Realty",
            "font_name": "Inter",
            "font_size_in_px": 18,
            "font_weight": "semibold",
            "text_color": "#64748B",
            "text_align": "right",
            "vertical_align": "center",
            "position": {"x_in_px": 40, "y_in_px": 1020},
            "dimensions": {"width_in_px": 1000, "height_in_px": 36},
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
		Dimensions:   il.Dimensions{WidthInPx: 1080, HeightInPx: 1080},
		OutputFormat: "jpeg",
		Layers: []il.Layer{
			il.NewImageLayer(
				0,
				il.NewFileFromURL("property.jpg", "https://example.com/properties/412-ridgewood.jpg"),
				il.Position{XInPx: 0, YInPx: 0},
				il.Dimensions{WidthInPx: 1080, HeightInPx: 720},
			),
			il.NewSolidColorLayer(1, "#1A1A2E",
				il.Position{XInPx: 0, YInPx: 720},
				il.Dimensions{WidthInPx: 1080, HeightInPx: 360},
			),
			il.NewSolidColorLayer(2, "#E63946",
				il.Position{XInPx: 40, YInPx: 40},
				il.Dimensions{WidthInPx: 180, HeightInPx: 48},
			),
			il.NewTextLayer(3, "JUST LISTED", "Inter", 18, "#FFFFFF",
				il.Position{XInPx: 40, YInPx: 40},
				il.Dimensions{WidthInPx: 180, HeightInPx: 48},
			),
			il.NewTextLayer(4, "$685,000", "Inter", 52, "#FFFFFF",
				il.Position{XInPx: 40, YInPx: 740},
				il.Dimensions{WidthInPx: 700, HeightInPx: 72},
			),
			il.NewTextLayer(5, "412 Ridgewood Lane, Denver, CO 80203", "Inter", 22, "#CBD5E1",
				il.Position{XInPx: 40, YInPx: 820},
				il.Dimensions{WidthInPx: 900, HeightInPx: 36},
			),
			il.NewTextLayer(6, "4 bed  ·  2.5 bath  ·  2,140 sq ft", "Inter", 20, "#94A3B8",
				il.Position{XInPx: 40, YInPx: 870},
				il.Dimensions{WidthInPx: 900, HeightInPx: 36},
			),
			il.NewTextLayer(7, "Acme Realty", "Inter", 18, "#64748B",
				il.Position{XInPx: 40, YInPx: 1020},
				il.Dimensions{WidthInPx: 1000, HeightInPx: 36},
			),
		},
	})
	if err != nil {
		panic(err)
	}
	_ = result
}
```

```n8n
{
  "name": "Generate Real Estate Listing Graphic",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Real Estate Listing Graphic\n\nReal estate agencies and property platforms use this recipe to generate listing graphics for social media, email campaigns, and digital signage. Feed in property data and a photo URL to get a print-ready branded card \u2014 no design software, no manual exports.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "4c51f576-8de8-4023-b28e-4798e54af8d7",
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
      "id": "43d2927f-bae6-4ae8-aef7-177d92f1aac5",
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
      "id": "d0e1f2a3-aaaa-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 1080,
        "heightInPx": 1080,
        "outputFormat": "jpeg",
        "layersJson": "[{\"index\":0,\"type\":\"image\",\"file\":{\"type\":\"url\",\"name\":\"property.jpg\",\"url\":\"https://example.com/properties/412-ridgewood.jpg\"},\"dimensions\":{\"width\":1080,\"height\":720}},{\"index\":1,\"type\":\"solid-color\",\"hex_color\":\"#1A1A2E\",\"position\":{\"x\":0,\"y\":720},\"dimensions\":{\"width\":1080,\"height\":360}},{\"index\":2,\"type\":\"solid-color\",\"hex_color\":\"#E63946\",\"position\":{\"x\":40,\"y\":40},\"dimensions\":{\"width\":180,\"height\":48},\"border_radius\":6},{\"index\":3,\"type\":\"text\",\"text\":\"JUST LISTED\",\"font_name\":\"Inter\",\"font_size_in_px\":18,\"font_weight\":\"bold\",\"text_color\":\"#FFFFFF\",\"text_align\":\"center\",\"vertical_align\":\"center\",\"position\":{\"x\":40,\"y\":40},\"dimensions\":{\"width\":180,\"height\":48}},{\"index\":4,\"type\":\"text\",\"text\":\"$685,000\",\"font_name\":\"Inter\",\"font_size_in_px\":52,\"font_weight\":\"bold\",\"text_color\":\"#FFFFFF\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":40,\"y\":740},\"dimensions\":{\"width\":700,\"height\":72}},{\"index\":5,\"type\":\"text\",\"text\":\"412 Ridgewood Lane, Denver, CO 80203\",\"font_name\":\"Inter\",\"font_size_in_px\":22,\"font_weight\":\"regular\",\"text_color\":\"#CBD5E1\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":40,\"y\":820},\"dimensions\":{\"width\":900,\"height\":36}},{\"index\":6,\"type\":\"text\",\"text\":\"4 bed  \\u00b7  2.5 bath  \\u00b7  2,140 sq ft\",\"font_name\":\"Inter\",\"font_size_in_px\":20,\"font_weight\":\"regular\",\"text_color\":\"#94A3B8\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":40,\"y\":870},\"dimensions\":{\"width\":900,\"height\":36}},{\"index\":7,\"type\":\"text\",\"text\":\"Acme Realty\",\"font_name\":\"Inter\",\"font_size_in_px\":18,\"font_weight\":\"semibold\",\"text_color\":\"#64748B\",\"text_align\":\"right\",\"vertical_align\":\"center\",\"position\":{\"x\":40,\"y\":1020},\"dimensions\":{\"width\":1000,\"height\":36}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "d0e1f2a3-aaaa-4000-8000-000000000002",
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
Generate a real estate listing graphic at 1200x628 pixels. Use the generate_image tool with these layers:

1. Property photo: image layer with [property image URL] as full background
2. Gradient overlay: gradient layer for text readability at the bottom
3. Status badge: solid-color rectangle with [status] text (e.g., "For Sale", "Sold")
4. Price: text layer with [price] in large bold text
5. Address: text layer with [street address, city, state]
6. Stats: text layers for [bedrooms] bd, [bathrooms] ba, [square feet] sqft
7. Agency logo: image layer with [logo URL]
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "/9j/4AAQSkZJRgABAQ...",
    "mime_type": "image/jpeg"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
