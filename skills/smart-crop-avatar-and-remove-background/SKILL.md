---
name: smart-crop-avatar-and-remove-background
description: Smart crop to face, remove the background, and convert to WebP for a clean user avatar.
---

# Smart Crop Avatar and Remove Background

Social platforms and SaaS apps use this recipe to process user-uploaded profile pictures on the fly. Smart crop centers on the face, background removal strips the messy kitchen or parking lot, and WebP conversion keeps file sizes small — ready for consistent, professional-looking avatars.

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
      "name": "upload.jpg",
      "url": "https://cdn.example.com/uploads/user-photo.jpg"
    },
    "operations": [
      {
        "type": "smart_crop",
        "width_in_px": 256,
        "height_in_px": 256
      },
      {
        "type": "remove_background"
      },
      {
        "type": "convert",
        "format": "webp",
        "quality": 85
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
    name: "upload.jpg",
    url: "https://cdn.example.com/uploads/user-photo.jpg",
  },
  operations: [
    {
      type: "smart_crop",
      width_in_px: 256,
      height_in_px: 256,
    },
    { type: "remove_background" },
    {
      type: "convert",
      format: "webp",
      quality: 85,
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
        "name": "upload.jpg",
        "url": "https://cdn.example.com/uploads/user-photo.jpg",
    },
    operations=[
        {
            "type": "smart_crop",
            "width_in_px": 256,
            "height_in_px": 256,
        },
        {"type": "remove_background"},
        {
            "type": "convert",
            "format": "webp",
            "quality": 85,
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	quality := 85
	result, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL(
			"upload.jpg",
			"https://cdn.example.com/uploads/user-photo.jpg",
		),
		Operations: []il.TransformOperation{
			il.NewSmartCropOperation(256, 256),
			il.NewRemoveBackgroundOperation(),
			il.ConvertOperation{Type: "convert", Format: "webp", Quality: &quality},
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Smart Crop Avatar and Remove Background",
  "nodes": [
    {
      "parameters": {
        "content": "## Smart Crop Avatar and Remove Background\n\nSocial platforms and SaaS apps use this recipe to process user-uploaded profile pictures on the fly. Smart crop centers on the face, background removal strips the messy kitchen or parking lot, and WebP conversion keeps file sizes small \u2014 ready for consistent, professional-looking avatars.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "3f93355d-ff33-4297-b5c7-e3763b1236f9",
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
      "id": "d16abd9f-1d86-4e42-8ae0-7afc18f093fe",
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
      "id": "c5d6e7f8-a9b0-4c1d-2e3f-5a6b7c8d9e0f",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "upload.jpg",
        "fileUrl": "https://cdn.example.com/uploads/user-photo.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "smart_crop",
              "widthInPx": 256,
              "heightInPx": 256
            },
            {
              "operationType": "remove_background"
            },
            {
              "operationType": "convert",
              "convertFormat": "webp",
              "quality": 85
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
      "id": "f8a9b0c1-d2e3-4f4a-5b6c-7d8e9f0a1b2c",
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
Create a clean avatar from the photo at [file URL]. Use the transform_image tool with a smart_crop operation to [width]x[height] pixels, a remove_background operation, and a convert operation to WebP at quality 85.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "iVBORw0KGgoAAAANSUhEUg...",
    "mime_type": "image/webp"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
