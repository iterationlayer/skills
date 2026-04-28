---
name: resize-image-for-print-publishing
description: Resize and convert manuscript images to KDP-compliant dimensions at 300 DPI for print-on-demand.
---

# Resize Image for Print Publishing

Self-publishers and book production teams use this recipe to prepare manuscript images for KDP or IngramSpark. Constrain images to the printable area (1500x2400 pixels for a 6x9 inch page at 300 DPI), sharpen after downscale, and convert to JPEG at high quality — ready for interior pages.

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
      "name": "diagram-03.png",
      "url": "https://storage.example.com/book/diagram-03.png"
    },
    "operations": [
      {
        "type": "resize",
        "width_in_px": 1500,
        "height_in_px": 2400,
        "fit": "inside"
      },
      {
        "type": "sharpen",
        "sigma": 0.5
      },
      {
        "type": "convert",
        "format": "jpeg",
        "quality": 95
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
    name: "diagram-03.png",
    url: "https://storage.example.com/book/diagram-03.png",
  },
  operations: [
    {
      type: "resize",
      width_in_px: 1500,
      height_in_px: 2400,
      fit: "inside",
    },
    {
      type: "sharpen",
      sigma: 0.5,
    },
    {
      type: "convert",
      format: "jpeg",
      quality: 95,
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
        "name": "diagram-03.png",
        "url": "https://storage.example.com/book/diagram-03.png",
    },
    operations=[
        {
            "type": "resize",
            "width_in_px": 1500,
            "height_in_px": 2400,
            "fit": "inside",
        },
        {
            "type": "sharpen",
            "sigma": 0.5,
        },
        {
            "type": "convert",
            "format": "jpeg",
            "quality": 95,
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	quality := 95
	result, err := client.TransformImage(il.TransformImageRequest{
		File: il.NewFileFromURL(
			"diagram-03.png",
			"https://storage.example.com/book/diagram-03.png",
		),
		Operations: []il.TransformOperation{
			il.NewResizeOperation(1500, 2400, "inside"),
			il.NewSharpenOperation(0.5),
			il.ConvertOperation{Type: "convert", Format: "jpeg", Quality: &quality},
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Resize Image for Print Publishing",
  "nodes": [
    {
      "parameters": {
        "content": "## Resize Image for Print Publishing\n\nSelf-publishers and book production teams use this recipe to prepare manuscript images for KDP or IngramSpark. Constrain images to the printable area (1500x2400 pixels for a 6x9 inch page at 300 DPI), sharpen after downscale, and convert to JPEG at high quality \u2014 ready for interior pages.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "474c9048-ca01-43e7-b05f-2692707102ea",
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
      "id": "0ef17e6f-9466-40c3-9cdb-acadbf57e915",
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
      "id": "d0e1f2a3-b4c5-4d6e-7f8a-0b1c2d3e4f5a",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "imageTransformation",
        "fileInputMode": "url",
        "fileName": "diagram-03.png",
        "fileUrl": "https://storage.example.com/book/diagram-03.png",
        "operations": {
          "operationValues": [
            {
              "operationType": "resize",
              "widthInPx": 1500,
              "heightInPx": 2400,
              "fit": "inside"
            },
            {
              "operationType": "sharpen",
              "sigma": 0.5
            },
            {
              "operationType": "convert",
              "convertFormat": "jpeg",
              "quality": 95
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
      "id": "a3b4c5d6-e7f8-4a9b-0c1d-2e3f4a5b6c7d",
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
Resize the image at [file URL] for print publishing. Use the transform_image tool with a resize operation to [width]x[height] pixels (fit: inside), a sharpen operation, and a convert operation to JPEG at quality 95.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "iVBORw0KGgoAAAANSUhEUg...",
    "mime_type": "image/jpeg"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
