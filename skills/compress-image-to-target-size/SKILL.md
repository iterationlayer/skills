---
name: compress-image-to-target-size
description: Compress an image to fit within a specific file size in bytes using quality-first compression.
---

# Compress Image to Target File Size

Marketing teams and content platforms use this recipe to compress images for email attachments, CMS upload limits, or mobile app asset budgets. Specify a target file size in bytes and the API handles the quality-dimension tradeoff automatically.

## APIs Used

Image Transformation (1 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST \
  https://api.iterationlayer.com/image-transformation/v1/transform \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "file": {
      "type": "url",
      "name": "photo.jpg",
      "url": "https://cdn.example.com/uploads/photo.jpg"
    },
    "operations": [
      {
        "type": "compress_to_size",
        "max_file_size_in_bytes": 500000
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
    name: "photo.jpg",
    url: "https://cdn.example.com/uploads/photo.jpg",
  },
  operations: [
    {
      type: "compress_to_size",
      max_file_size_in_bytes: 500_000,
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
        "name": "photo.jpg",
        "url": "https://cdn.example.com/uploads/photo.jpg",
    },
    operations=[
        {
            "type": "compress_to_size",
            "max_file_size_in_bytes": 500_000,
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
		File: il.NewFileFromURL(
			"photo.jpg",
			"https://cdn.example.com/uploads/photo.jpg",
		),
		Operations: []il.TransformOperation{
			il.NewCompressToSizeOperation(500_000),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Compress Image to Target File Size",
  "nodes": [
    {
      "parameters": {
        "content": "## Compress Image to Target File Size

Marketing teams and content platforms use this recipe to compress images for email attachments, CMS upload limits, or mobile app asset budgets. Specify a target file size in bytes and the API handles the quality-dimension tradeoff automatically.

**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "620f3f66-b3fe-48b4-a3a5-130a7ed310b4",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Transform Image
Resource: **Image Transformation**

Configure the Image Transformation parameters below, then connect your credentials.",
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
      "id": "a87d9afb-d1c2-4e2b-9fac-05039ee12b2a",
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
      "id": "b2c3d4e5-f6a7-4b8c-9d0e-1f2a3b4c5d6e",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "photo.jpg",
        "fileUrl": "https://cdn.example.com/uploads/photo.jpg",
        "operations": {
          "operationValues": [
            {
              "operationType": "compress_to_size",
              "maxFileSizeInBytes": 500000
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
      "id": "e5f6a7b8-c9d0-4e1f-2a3b-4c5d6e7f8a9b",
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
Compress the image at [file URL] to fit within [max file size in bytes] bytes. Use the transform_image tool with a compress_to_size operation.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "/9j/4AAQSkZJRgABAQAAAQ...",
    "mime_type": "image/jpeg"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
