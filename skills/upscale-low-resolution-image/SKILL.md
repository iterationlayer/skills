---
name: upscale-low-resolution-image
description: Upscale a low-resolution image using AI super-resolution for print or high-DPI display.
---

# Upscale Low-Resolution Image

Marketing teams and e-commerce sellers upscale low-res product images or legacy assets for high-DPI displays and print materials. Upload a low-resolution image, apply AI-powered super resolution at 4x scale, and convert to PNG — ready for retina screens, large-format prints, or marketing collateral.

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
      "name": "low-res-image.jpg",
      "url": "https://example.com/low-res-image.jpg"
    },
    "operations": [
      {
        "type": "upscale",
        "factor": 4
      },
      {
        "type": "convert",
        "format": "png"
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
    name: "low-res-image.jpg",
    url: "https://example.com/low-res-image.jpg",
  },
  operations: [
    {
      type: "upscale",
      factor: 4,
    },
    {
      type: "convert",
      format: "png",
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
        "name": "low-res-image.jpg",
        "url": "https://example.com/low-res-image.jpg",
    },
    operations=[
        {
            "type": "upscale",
            "factor": 4,
        },
        {
            "type": "convert",
            "format": "png",
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL("low-res-image.jpg", "https://example.com/low-res-image.jpg"),
		Operations: []il.TransformOperation{
			{
     Type: "upscale",
     Factor: 4,
   },
			il.NewConvertOperation("png"),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Upscale Low-Resolution Image",
  "nodes": [
    {
      "parameters": {
        "content": "## Upscale Low-Resolution Image\n\nMarketing teams and e-commerce sellers upscale low-res product images or legacy assets for high-DPI displays and print materials. Upload a low-resolution image, apply AI-powered super resolution at 4x scale, and convert to PNG \u2014 ready for retina screens, large-format prints, or marketing collateral.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "14efa0b9-60d8-4c0c-a490-7dd4c4a5cb2a",
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
      "id": "7a2fee46-6f24-4a47-b778-6f3599618a1a",
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
      "id": "b4c5d6e7-f8a9-4b0c-1d2e-4f5a6b7c8d9e",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "low-res-image.jpg",
        "fileUrl": "https://example.com/low-res-image.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "upscale",
              "upscaleFactor": 4
            },
            {
              "operationType": "convert",
              "convertFormat": "png"
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
      "id": "e7f8a9b0-c1d2-4e3f-4a5b-6c7d8e9f0a1b",
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
Upscale the low-resolution image at [file URL]. Use the transform_image tool with an upscale operation at [scale factor]x and a convert operation to PNG.
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
