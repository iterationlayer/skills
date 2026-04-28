---
name: optimize-for-amazon
description: Prepare a product photo to meet Amazon's main image requirements: pure white background, square format, 2000×2000px, JPEG.
---

# Optimize Product Image for Amazon

E-commerce sellers and agencies use this recipe to prepare product photos that pass Amazon's main image requirements. One API call removes the background, fills it white, resizes to the required square format, and outputs a JPEG — no manual editing, no Photoshop.

## APIs Used

Image Transformation (1 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/image-transformation/v1/transform \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "product.jpg",
      "url": "https://example.com/products/raw/jacket-001.jpg"
    },
    "operations": [
      {
        "type": "remove_background",
        "background_hex_color": "#FFFFFF"
      },
      {
        "type": "resize",
        "width_in_px": 2000,
        "height_in_px": 2000,
        "fit": "contain"
      },
      {
        "type": "convert",
        "format": "jpeg",
        "quality": 90
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.transformImage({
  file: {
    type: "url",
    name: "product.jpg",
    url: "https://example.com/products/raw/jacket-001.jpg",
  },
  operations: [
    {
      type: "remove_background",
      background_hex_color: "#FFFFFF",
    },
    {
      type: "resize",
      width_in_px: 2000,
      height_in_px: 2000,
      fit: "contain",
    },
    {
      type: "convert",
      format: "jpeg",
      quality: 90,
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.transform_image(
    file={
        "type": "url",
        "name": "product.jpg",
        "url": "https://example.com/products/raw/jacket-001.jpg",
    },
    operations=[
        {
            "type": "remove_background",
            "background_hex_color": "#FFFFFF",
        },
        {
            "type": "resize",
            "width_in_px": 2000,
            "height_in_px": 2000,
            "fit": "contain",
        },
        {
            "type": "convert",
            "format": "jpeg",
            "quality": 90,
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	quality := 90
	result, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL("product.jpg", "https://example.com/products/raw/jacket-001.jpg"),
		Operations: []il.TransformOperation{
			il.RemoveBackgroundOperation{Type: "remove_background", BackgroundHexColor: "#FFFFFF"},
			il.NewResizeOperation(2000, 2000, "contain"),
			il.ConvertOperation{Type: "convert", Format: "jpeg", Quality: &quality},
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
  "name": "Optimize Product Image for Amazon",
  "nodes": [
    {
      "parameters": {
        "content": "## Optimize Product Image for Amazon\n\nE-commerce sellers and agencies use this recipe to prepare product photos that pass Amazon's main image requirements. One API call removes the background, fills it white, resizes to the required square format, and outputs a JPEG \u2014 no manual editing, no Photoshop.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
        "height": 280,
        "width": 500,
        "color": 2
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        200,
        40
      ],
      "id": "cc02028c-4051-4ba5-8c25-38fe96c4303c",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Transform Image\nResource: **Image Transformation**\n\nConfigure the Image Transformation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        475,
        100
      ],
      "id": "a3156e35-e86c-4589-bd75-791e9e13e956",
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
      "id": "d4e5f6a7-b8c9-4d0e-1f2a-4b5c6d7e8f9a",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "product.jpg",
        "fileUrl": "https://example.com/products/raw/jacket-001.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "remove_background",
              "hexColor": "#FFFFFF"
            },
            {
              "operationType": "resize",
              "widthInPx": 2000,
              "heightInPx": 2000,
              "fit": "contain"
            },
            {
              "operationType": "convert",
              "convertFormat": "jpeg",
              "quality": 90
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
      "id": "a7b8c9d0-e1f2-4a3b-4c5d-6e7f8a9b0c1d",
      "name": "Transform Image",
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
            "node": "Transform Image",
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
Optimize the product image at [file URL] for Amazon. Use the transform_image tool with a remove_background operation (background color #FFFFFF), a resize operation to 2000x2000 pixels (fit: contain), and a convert operation to JPEG at quality 90.
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
