---
name: generate-report-card-image
description: Generate a visual KPI report card with a headline metric, secondary stats, and branding — shareable as an image.
---

# Generate Report Card Image

Analytics platforms, SaaS dashboards, and agencies use this recipe to generate shareable report card images from live data. Feed in numbers and labels to get a clean branded card ready for Slack, email, or embedded in a PDF — no charting libraries, no screenshots.

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
    "dimensions": { "width_in_px": 800, "height_in_px": 480 },
    "output_format": "png",
    "layers": [
      {
        "index": 0,
        "type": "solid-color",
        "hex_color": "#0F172A"
      },
      {
        "index": 1,
        "type": "solid-color",
        "hex_color": "#1E293B",
        "position": { "x_in_px": 32, "y_in_px": 32 },
        "dimensions": { "width_in_px": 736, "height_in_px": 416 },
        "border_radius": 12
      },
      {
        "index": 2,
        "type": "text",
        "text": "Monthly Revenue",
        "font_name": "Inter",
        "font_size_in_px": 18,
        "font_weight": "regular",
        "text_color": "#94A3B8",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 64 },
        "dimensions": { "width_in_px": 600, "height_in_px": 28 }
      },
      {
        "index": 3,
        "type": "text",
        "text": "$124,800",
        "font_name": "Inter",
        "font_size_in_px": 72,
        "font_weight": "bold",
        "text_color": "#F8FAFC",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 108 },
        "dimensions": { "width_in_px": 600, "height_in_px": 96 }
      },
      {
        "index": 4,
        "type": "text",
        "text": "+12.4% vs last month",
        "font_name": "Inter",
        "font_size_in_px": 20,
        "font_weight": "regular",
        "text_color": "#4ADE80",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 216 },
        "dimensions": { "width_in_px": 400, "height_in_px": 32 }
      },
      {
        "index": 5,
        "type": "solid-color",
        "hex_color": "#334155",
        "position": { "x_in_px": 64, "y_in_px": 280 },
        "dimensions": { "width_in_px": 672, "height_in_px": 1 }
      },
      {
        "index": 6,
        "type": "text",
        "text": "New customers: 342",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#CBD5E1",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 304 },
        "dimensions": { "width_in_px": 300, "height_in_px": 28 }
      },
      {
        "index": 7,
        "type": "text",
        "text": "Churn rate: 1.8%",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#CBD5E1",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 340 },
        "dimensions": { "width_in_px": 300, "height_in_px": 28 }
      },
      {
        "index": 8,
        "type": "text",
        "text": "Avg deal size: $365",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#CBD5E1",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 420, "y_in_px": 304 },
        "dimensions": { "width_in_px": 300, "height_in_px": 28 }
      },
      {
        "index": 9,
        "type": "text",
        "text": "MRR: $41,600",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#CBD5E1",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 420, "y_in_px": 340 },
        "dimensions": { "width_in_px": 300, "height_in_px": 28 }
      },
      {
        "index": 10,
        "type": "text",
        "text": "March 2026",
        "font_name": "Inter",
        "font_size_in_px": 14,
        "font_weight": "regular",
        "text_color": "#475569",
        "text_align": "right",
        "vertical_align": "center",
        "position": { "x_in_px": 64, "y_in_px": 408 },
        "dimensions": { "width_in_px": 672, "height_in_px": 24 }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateImage({
  dimensions: { width_in_px: 800, height_in_px: 480 },
  output_format: "png",
  layers: [
    { index: 0, type: "solid-color", hex_color: "#0F172A" },
    {
      index: 1,
      type: "solid-color",
      hex_color: "#1E293B",
      position: { x_in_px: 32, y_in_px: 32 },
      dimensions: { width_in_px: 736, height_in_px: 416 },
      border_radius: 12,
    },
    {
      index: 2,
      type: "text",
      text: "Monthly Revenue",
      font_name: "Inter",
      font_size_in_px: 18,
      font_weight: "regular",
      text_color: "#94A3B8",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 64 },
      dimensions: { width_in_px: 600, height_in_px: 28 },
    },
    {
      index: 3,
      type: "text",
      text: "$124,800",
      font_name: "Inter",
      font_size_in_px: 72,
      font_weight: "bold",
      text_color: "#F8FAFC",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 108 },
      dimensions: { width_in_px: 600, height_in_px: 96 },
    },
    {
      index: 4,
      type: "text",
      text: "+12.4% vs last month",
      font_name: "Inter",
      font_size_in_px: 20,
      font_weight: "regular",
      text_color: "#4ADE80",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 216 },
      dimensions: { width_in_px: 400, height_in_px: 32 },
    },
    {
      index: 5,
      type: "solid-color",
      hex_color: "#334155",
      position: { x_in_px: 64, y_in_px: 280 },
      dimensions: { width_in_px: 672, height_in_px: 1 },
    },
    {
      index: 6,
      type: "text",
      text: "New customers: 342",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#CBD5E1",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 304 },
      dimensions: { width_in_px: 300, height_in_px: 28 },
    },
    {
      index: 7,
      type: "text",
      text: "Churn rate: 1.8%",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#CBD5E1",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 340 },
      dimensions: { width_in_px: 300, height_in_px: 28 },
    },
    {
      index: 8,
      type: "text",
      text: "Avg deal size: $365",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#CBD5E1",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 420, y_in_px: 304 },
      dimensions: { width_in_px: 300, height_in_px: 28 },
    },
    {
      index: 9,
      type: "text",
      text: "MRR: $41,600",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#CBD5E1",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 420, y_in_px: 340 },
      dimensions: { width_in_px: 300, height_in_px: 28 },
    },
    {
      index: 10,
      type: "text",
      text: "March 2026",
      font_name: "Inter",
      font_size_in_px: 14,
      font_weight: "regular",
      text_color: "#475569",
      text_align: "right",
      vertical_align: "center",
      position: { x_in_px: 64, y_in_px: 408 },
      dimensions: { width_in_px: 672, height_in_px: 24 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_image(
    dimensions={"width_in_px": 800, "height_in_px": 480},
    output_format="png",
    layers=[
        {"index": 0, "type": "solid-color", "hex_color": "#0F172A"},
        {
            "index": 1,
            "type": "solid-color",
            "hex_color": "#1E293B",
            "position": {"x_in_px": 32, "y_in_px": 32},
            "dimensions": {"width_in_px": 736, "height_in_px": 416},
            "border_radius": 12,
        },
        {
            "index": 2,
            "type": "text",
            "text": "Monthly Revenue",
            "font_name": "Inter",
            "font_size_in_px": 18,
            "font_weight": "regular",
            "text_color": "#94A3B8",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 64},
            "dimensions": {"width_in_px": 600, "height_in_px": 28},
        },
        {
            "index": 3,
            "type": "text",
            "text": "$124,800",
            "font_name": "Inter",
            "font_size_in_px": 72,
            "font_weight": "bold",
            "text_color": "#F8FAFC",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 108},
            "dimensions": {"width_in_px": 600, "height_in_px": 96},
        },
        {
            "index": 4,
            "type": "text",
            "text": "+12.4% vs last month",
            "font_name": "Inter",
            "font_size_in_px": 20,
            "font_weight": "regular",
            "text_color": "#4ADE80",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 216},
            "dimensions": {"width_in_px": 400, "height_in_px": 32},
        },
        {
            "index": 5,
            "type": "solid-color",
            "hex_color": "#334155",
            "position": {"x_in_px": 64, "y_in_px": 280},
            "dimensions": {"width_in_px": 672, "height_in_px": 1},
        },
        {
            "index": 6,
            "type": "text",
            "text": "New customers: 342",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#CBD5E1",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 304},
            "dimensions": {"width_in_px": 300, "height_in_px": 28},
        },
        {
            "index": 7,
            "type": "text",
            "text": "Churn rate: 1.8%",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#CBD5E1",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 340},
            "dimensions": {"width_in_px": 300, "height_in_px": 28},
        },
        {
            "index": 8,
            "type": "text",
            "text": "Avg deal size: $365",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#CBD5E1",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 420, "y_in_px": 304},
            "dimensions": {"width_in_px": 300, "height_in_px": 28},
        },
        {
            "index": 9,
            "type": "text",
            "text": "MRR: $41,600",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#CBD5E1",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 420, "y_in_px": 340},
            "dimensions": {"width_in_px": 300, "height_in_px": 28},
        },
        {
            "index": 10,
            "type": "text",
            "text": "March 2026",
            "font_name": "Inter",
            "font_size_in_px": 14,
            "font_weight": "regular",
            "text_color": "#475569",
            "text_align": "right",
            "vertical_align": "center",
            "position": {"x_in_px": 64, "y_in_px": 408},
            "dimensions": {"width_in_px": 672, "height_in_px": 24},
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
		Dimensions:   il.Dimensions{WidthInPx: 800, HeightInPx: 480},
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#0F172A"),
			il.NewSolidColorLayer(1, "#1E293B",
				il.Position{XInPx: 32, YInPx: 32},
				il.Dimensions{WidthInPx: 736, HeightInPx: 416},
			),
			il.NewTextLayer(2, "Monthly Revenue", "Inter", 18, "#94A3B8",
				il.Position{XInPx: 64, YInPx: 64},
				il.Dimensions{WidthInPx: 600, HeightInPx: 28},
			),
			il.NewTextLayer(3, "$124,800", "Inter", 72, "#F8FAFC",
				il.Position{XInPx: 64, YInPx: 108},
				il.Dimensions{WidthInPx: 600, HeightInPx: 96},
			),
			il.NewTextLayer(4, "+12.4% vs last month", "Inter", 20, "#4ADE80",
				il.Position{XInPx: 64, YInPx: 216},
				il.Dimensions{WidthInPx: 400, HeightInPx: 32},
			),
			il.NewSolidColorLayer(5, "#334155",
				il.Position{XInPx: 64, YInPx: 280},
				il.Dimensions{WidthInPx: 672, HeightInPx: 1},
			),
			il.NewTextLayer(6, "New customers: 342", "Inter", 16, "#CBD5E1",
				il.Position{XInPx: 64, YInPx: 304},
				il.Dimensions{WidthInPx: 300, HeightInPx: 28},
			),
			il.NewTextLayer(7, "Churn rate: 1.8%", "Inter", 16, "#CBD5E1",
				il.Position{XInPx: 64, YInPx: 340},
				il.Dimensions{WidthInPx: 300, HeightInPx: 28},
			),
			il.NewTextLayer(8, "Avg deal size: $365", "Inter", 16, "#CBD5E1",
				il.Position{XInPx: 420, YInPx: 304},
				il.Dimensions{WidthInPx: 300, HeightInPx: 28},
			),
			il.NewTextLayer(9, "MRR: $41,600", "Inter", 16, "#CBD5E1",
				il.Position{XInPx: 420, YInPx: 340},
				il.Dimensions{WidthInPx: 300, HeightInPx: 28},
			),
			il.NewTextLayer(10, "March 2026", "Inter", 14, "#475569",
				il.Position{XInPx: 64, YInPx: 408},
				il.Dimensions{WidthInPx: 672, HeightInPx: 24},
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
  "name": "Generate Report Card Image",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Report Card Image\n\nAnalytics platforms, SaaS dashboards, and agencies use this recipe to generate shareable report card images from live data. Feed in numbers and labels to get a clean branded card ready for Slack, email, or embedded in a PDF \u2014 no charting libraries, no screenshots.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "d35d93a7-c164-43a5-96ff-7b2d3bc07d9e",
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
      "id": "258ff3a6-dd60-4125-8d7d-a32cad4f15eb",
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
      "id": "e1f2a3b4-bbbb-4000-8000-000000000001",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 800,
        "heightInPx": 480,
        "outputFormat": "png",
        "layersJson": "[{\"index\":0,\"type\":\"solid-color\",\"hex_color\":\"#0F172A\"},{\"index\":1,\"type\":\"solid-color\",\"hex_color\":\"#1E293B\",\"position\":{\"x\":32,\"y\":32},\"dimensions\":{\"width\":736,\"height\":416},\"border_radius\":12},{\"index\":2,\"type\":\"text\",\"text\":\"Monthly Revenue\",\"font_name\":\"Inter\",\"font_size_in_px\":18,\"font_weight\":\"regular\",\"text_color\":\"#94A3B8\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":64},\"dimensions\":{\"width\":600,\"height\":28}},{\"index\":3,\"type\":\"text\",\"text\":\"$124,800\",\"font_name\":\"Inter\",\"font_size_in_px\":72,\"font_weight\":\"bold\",\"text_color\":\"#F8FAFC\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":108},\"dimensions\":{\"width\":600,\"height\":96}},{\"index\":4,\"type\":\"text\",\"text\":\"+12.4% vs last month\",\"font_name\":\"Inter\",\"font_size_in_px\":20,\"font_weight\":\"regular\",\"text_color\":\"#4ADE80\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":216},\"dimensions\":{\"width\":400,\"height\":32}},{\"index\":5,\"type\":\"solid-color\",\"hex_color\":\"#334155\",\"position\":{\"x\":64,\"y\":280},\"dimensions\":{\"width\":672,\"height\":1}},{\"index\":6,\"type\":\"text\",\"text\":\"New customers: 342\",\"font_name\":\"Inter\",\"font_size_in_px\":16,\"font_weight\":\"regular\",\"text_color\":\"#CBD5E1\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":304},\"dimensions\":{\"width\":300,\"height\":28}},{\"index\":7,\"type\":\"text\",\"text\":\"Churn rate: 1.8%\",\"font_name\":\"Inter\",\"font_size_in_px\":16,\"font_weight\":\"regular\",\"text_color\":\"#CBD5E1\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":340},\"dimensions\":{\"width\":300,\"height\":28}},{\"index\":8,\"type\":\"text\",\"text\":\"Avg deal size: $365\",\"font_name\":\"Inter\",\"font_size_in_px\":16,\"font_weight\":\"regular\",\"text_color\":\"#CBD5E1\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":420,\"y\":304},\"dimensions\":{\"width\":300,\"height\":28}},{\"index\":9,\"type\":\"text\",\"text\":\"MRR: $41,600\",\"font_name\":\"Inter\",\"font_size_in_px\":16,\"font_weight\":\"regular\",\"text_color\":\"#CBD5E1\",\"text_align\":\"left\",\"vertical_align\":\"center\",\"position\":{\"x\":420,\"y\":340},\"dimensions\":{\"width\":300,\"height\":28}},{\"index\":10,\"type\":\"text\",\"text\":\"March 2026\",\"font_name\":\"Inter\",\"font_size_in_px\":14,\"font_weight\":\"regular\",\"text_color\":\"#475569\",\"text_align\":\"right\",\"vertical_align\":\"center\",\"position\":{\"x\":64,\"y\":408},\"dimensions\":{\"width\":672,\"height\":24}}]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "e1f2a3b4-bbbb-4000-8000-000000000002",
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
Generate a visual KPI report card image at 1200x628 pixels. Use the generate_image tool with these layers:

1. Background: solid-color layer with [background color]
2. Headline metric: text layer with [primary metric value] in large bold text
3. Metric label: text layer with [metric name] below the headline
4. Secondary stats: text layers for [stat 1 label]: [stat 1 value], [stat 2 label]: [stat 2 value], [stat 3 label]: [stat 3 value]
5. Branding: text layer with [company/report name] and date
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
