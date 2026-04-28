---
name: resize-image-for-social-media
description: Resize and crop a single image into platform-specific dimensions for social media.
---

# Resize Image for Social Media

Social media managers and marketing teams use this recipe to create a correctly sized image for a specific platform. Upload one source image and resize it to platform-specific dimensions — ready for publishing on Instagram, Twitter, LinkedIn, and more.

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
      "name": "brand-image.jpg",
      "url": "https://example.com/brand-image.jpg"
    },
    "operations": [
      {
        "type": "resize",
        "width_in_px": 1080,
        "height_in_px": 1080,
        "fit": "cover"
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
    name: "brand-image.jpg",
    url: "https://example.com/brand-image.jpg",
  },
  operations: [
    {
      type: "resize",
      width_in_px: 1080,
      height_in_px: 1080,
      fit: "cover",
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
        "name": "brand-image.jpg",
        "url": "https://example.com/brand-image.jpg",
    },
    operations=[
        {
            "type": "resize",
            "width_in_px": 1080,
            "height_in_px": 1080,
            "fit": "cover",
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

	result, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL("brand-image.jpg", "https://example.com/brand-image.jpg"),
		Operations: []il.TransformOperation{
			il.NewResizeOperation(1080, 1080, "cover"),
			il.NewConvertOperation("jpeg"),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Resize Image for Social Media",
  "nodes": [
    {
      "parameters": {
        "content": "## Resize Image for Social Media\n\nSocial media managers and marketing teams use this recipe to create a correctly sized image for a specific platform. Upload one source image and resize it to platform-specific dimensions \u2014 ready for publishing on Instagram, Twitter, LinkedIn, and more.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "c63dfab3-9ffe-40dd-9b8a-23d58ec44364",
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
      "id": "d9e863c7-31d9-45db-a518-1b00d2a076f9",
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
      "id": "e1f2a3b4-c5d6-4e7f-8a9b-1c2d3e4f5a6b",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "brand-image.jpg",
        "fileUrl": "https://example.com/brand-image.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "resize",
              "widthInPx": 1080,
              "heightInPx": 1080,
              "fit": "cover"
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
      "id": "b4c5d6e7-f8a9-4b0c-1d2e-3f4a5b6c7d8e",
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
Resize the image at [file URL] for social media. Use the transform_image tool with a resize operation to [width]x[height] pixels (fit: cover) and a convert operation to JPEG at quality [quality].
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "iVBORw0KGgoAAAANSUhEUgAA...",
    "mime_type": "image/jpeg"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
