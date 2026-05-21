---
name: generate-packing-slip-and-shipping-label
description: Generate a PDF packing slip and a PNG shipping label from the same order data in a single fulfillment workflow.
---

# Generate Packing Slip and Shipping Label

Fulfillment centers and e-commerce platforms generate a PDF packing slip and a PNG shipping label from the same order data in a single workflow — both documents ready for printing without entering data twice.

## APIs Used

Document Generation (2 credits/request), Image Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Generate PDF packing slip
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Packing Slip - Order #WH-29341",
        "author": "Summit Outdoor Gear"
      },
      "page": {
        "size": { "preset": "A4" },
        "margins": {
          "top_in_pt": 54,
          "right_in_pt": 54,
          "bottom_in_pt": 54,
          "left_in_pt": 54
        }
      },
      "styles": {
        "text": {
          "font_family": "Helvetica",
          "font_size_in_pt": 11.0,
          "line_height": 1.5,
          "color": "#333333"
        },
        "headline": {
          "font_family": "Helvetica",
          "font_size_in_pt": 24.0,
          "color": "#111111",
          "spacing_before_in_pt": 12.0,
          "spacing_after_in_pt": 6.0,
          "font_weight": "bold"
        },
        "link": {
          "color": "#0066CC",
          "is_underlined": true
        },
        "list": {
          "text_style": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11.0,
            "line_height": 1.5,
            "color": "#333333"
          },
          "marker_color": "#333333",
          "marker_gap_in_pt": 8.0
        },
        "table": {
          "header": {
            "background_color": "#1A1A2E",
            "text_color": "#FFFFFF",
            "font_size_in_pt": 11.0,
            "font_weight": "bold"
          },
          "body": {
            "background_color": "#FFFFFF",
            "text_color": "#333333",
            "font_size_in_pt": 11.0
          },
          "border": {
            "outer": {
              "top": { "color": "#CCCCCC", "width_in_pt": 1.0 },
              "right": { "color": "#CCCCCC", "width_in_pt": 1.0 },
              "bottom": { "color": "#CCCCCC", "width_in_pt": 1.0 },
              "left": { "color": "#CCCCCC", "width_in_pt": 1.0 }
            },
            "inner": {
              "horizontal": { "color": "#EEEEEE", "width_in_pt": 0.5 },
              "vertical": { "color": "#EEEEEE", "width_in_pt": 0.5 }
            }
          }
        },
        "grid": {
          "background_color": "#FFFFFF",
          "border_color": "#CCCCCC",
          "border_width_in_pt": 0.0,
          "gap_in_pt": 12.0
        },
        "separator": {
          "color": "#CCCCCC",
          "thickness_in_pt": 1.0,
          "spacing_before_in_pt": 12.0,
          "spacing_after_in_pt": 12.0
        },
        "image": {
          "border_color": "#000000",
          "border_width_in_pt": 0.0
        }
      },
      "content": [
        { "type": "headline", "level": "h1", "text": "Summit Outdoor Gear" },
        {
          "type": "paragraph",
          "runs": [{ "text": "1200 Warehouse Pkwy, Denver, CO 80216  ·  summitoutdoorgear.com" }]
        },
        { "type": "headline", "level": "h2", "text": "Packing Slip" },
        {
          "type": "paragraph",
          "runs": [
            { "text": "Order #: ", "font_weight": "bold" },
            { "text": "WH-29341\n" },
            { "text": "Date: ", "font_weight": "bold" },
            { "text": "March 29, 2026\n" },
            { "text": "Customer: ", "font_weight": "bold" },
            { "text": "Sarah Mitchell" }
          ]
        },
        { "type": "separator" },
        {
          "type": "table",
          "column_widths_in_percent": [70, 30],
          "header": {
            "cells": [
              { "text": "Item" },
              { "text": "Qty", "horizontal_alignment": "center" }
            ]
          },
          "rows": [
            {
              "cells": [
                { "text": "Merino Wool Beanie" },
                { "text": "2", "horizontal_alignment": "center" }
              ]
            },
            {
              "cells": [
                { "text": "Trail Running Socks" },
                { "text": "4", "horizontal_alignment": "center" }
              ]
            },
            {
              "cells": [
                { "text": "Insulated Water Bottle" },
                { "text": "1", "horizontal_alignment": "center" }
              ]
            }
          ]
        },
        { "type": "separator" },
        {
          "type": "paragraph",
          "runs": [
            { "text": "Ship To:\n", "font_weight": "bold" },
            { "text": "Sarah Mitchell\n84 Maple Street\nPortland, OR 97201" }
          ]
        },
        { "type": "separator" },
        {
          "type": "paragraph",
          "runs": [{ "text": "Thank you for your order!" }]
        }
      ]
    }
  }'

# Step 2: Generate PNG shipping label
curl -X POST https://api.iterationlayer.com/image-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "dimensions": { "width_in_px": 600, "height_in_px": 400 },
    "output_format": "png",
    "layers": [
      { "index": 0, "type": "solid-color", "hex_color": "#FFFFFF" },
      {
        "index": 1,
        "type": "solid-color",
        "hex_color": "#1A1A2E",
        "position": { "x_in_px": 0, "y_in_px": 0 },
        "dimensions": { "width_in_px": 600, "height_in_px": 60 }
      },
      {
        "index": 2,
        "type": "text",
        "text": "SUMMIT OUTDOOR GEAR",
        "font_name": "Inter",
        "font_size_in_px": 18,
        "font_weight": "bold",
        "text_color": "#FFFFFF",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 0 },
        "dimensions": { "width_in_px": 552, "height_in_px": 60 }
      },
      {
        "index": 3,
        "type": "solid-color",
        "hex_color": "#CCCCCC",
        "position": { "x_in_px": 0, "y_in_px": 60 },
        "dimensions": { "width_in_px": 600, "height_in_px": 1 }
      },
      {
        "index": 4,
        "type": "text",
        "text": "SHIP TO:",
        "font_name": "Inter",
        "font_size_in_px": 12,
        "font_weight": "regular",
        "text_color": "#6B7280",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 80 },
        "dimensions": { "width_in_px": 400, "height_in_px": 20 }
      },
      {
        "index": 5,
        "type": "text",
        "text": "Sarah Mitchell",
        "font_name": "Inter",
        "font_size_in_px": 28,
        "font_weight": "bold",
        "text_color": "#1A1A2E",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 108 },
        "dimensions": { "width_in_px": 552, "height_in_px": 44 }
      },
      {
        "index": 6,
        "type": "text",
        "text": "84 Maple Street",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#374151",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 160 },
        "dimensions": { "width_in_px": 400, "height_in_px": 28 }
      },
      {
        "index": 7,
        "type": "text",
        "text": "Portland, OR 97201",
        "font_name": "Inter",
        "font_size_in_px": 16,
        "font_weight": "regular",
        "text_color": "#374151",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 192 },
        "dimensions": { "width_in_px": 400, "height_in_px": 28 }
      },
      {
        "index": 8,
        "type": "solid-color",
        "hex_color": "#CCCCCC",
        "position": { "x_in_px": 0, "y_in_px": 350 },
        "dimensions": { "width_in_px": 600, "height_in_px": 1 }
      },
      {
        "index": 9,
        "type": "text",
        "text": "#WH-29341",
        "font_name": "Inter",
        "font_size_in_px": 12,
        "font_weight": "regular",
        "text_color": "#6B7280",
        "text_align": "left",
        "vertical_align": "center",
        "position": { "x_in_px": 24, "y_in_px": 358 },
        "dimensions": { "width_in_px": 400, "height_in_px": 28 }
      }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Generate PDF packing slip
const packingSlip = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: "Packing Slip - Order #WH-29341",
      author: "Summit Outdoor Gear",
    },
    page: {
      size: { preset: "A4" },
      margins: {
        top_in_pt: 54,
        right_in_pt: 54,
        bottom_in_pt: 54,
        left_in_pt: 54,
      },
    },
    styles: {
      text: {
        font_family: "Helvetica",
        font_size_in_pt: 11.0,
        line_height: 1.5,
        color: "#333333",
      },
      headline: {
        font_family: "Helvetica",
        font_size_in_pt: 24.0,
        color: "#111111",
        spacing_before_in_pt: 12.0,
        spacing_after_in_pt: 6.0,
        font_weight: "bold",
      },
      link: {
        color: "#0066CC",
        is_underlined: true,
      },
      list: {
        text_style: {
          font_family: "Helvetica",
          font_size_in_pt: 11.0,
          line_height: 1.5,
          color: "#333333",
        },
        marker_color: "#333333",
        marker_gap_in_pt: 8.0,
      },
      table: {
        header: {
          background_color: "#1A1A2E",
          text_color: "#FFFFFF",
          font_size_in_pt: 11.0,
          font_weight: "bold",
        },
        body: {
          background_color: "#FFFFFF",
          text_color: "#333333",
          font_size_in_pt: 11.0,
        },
        border: {
          outer: {
            top: { color: "#CCCCCC", width_in_pt: 1.0 },
            right: { color: "#CCCCCC", width_in_pt: 1.0 },
            bottom: { color: "#CCCCCC", width_in_pt: 1.0 },
            left: { color: "#CCCCCC", width_in_pt: 1.0 },
          },
          inner: {
            horizontal: { color: "#EEEEEE", width_in_pt: 0.5 },
            vertical: { color: "#EEEEEE", width_in_pt: 0.5 },
          },
        },
      },
      grid: {
        background_color: "#FFFFFF",
        border_color: "#CCCCCC",
        border_width_in_pt: 0.0,
        gap_in_pt: 12.0,
      },
      separator: {
        color: "#CCCCCC",
        thickness_in_pt: 1.0,
        spacing_before_in_pt: 12.0,
        spacing_after_in_pt: 12.0,
      },
      image: {
        border_color: "#000000",
        border_width_in_pt: 0.0,
      },
    },
    content: [
      { type: "headline", level: "h1", text: "Summit Outdoor Gear" },
      {
        type: "paragraph",
        runs: [
          {
            text: "1200 Warehouse Pkwy, Denver, CO 80216  ·  summitoutdoorgear.com",
          },
        ],
      },
      { type: "headline", level: "h2", text: "Packing Slip" },
      {
        type: "paragraph",
        runs: [
          { text: "Order #: ", font_weight: "bold" },
          { text: "WH-29341\n" },
          { text: "Date: ", font_weight: "bold" },
          { text: "March 29, 2026\n" },
          { text: "Customer: ", font_weight: "bold" },
          { text: "Sarah Mitchell" },
        ],
      },
      { type: "separator" },
      {
        type: "table",
        column_widths_in_percent: [70, 30],
        header: {
          cells: [
            { text: "Item" },
            { text: "Qty", horizontal_alignment: "center" },
          ],
        },
        rows: [
          {
            cells: [
              { text: "Merino Wool Beanie" },
              { text: "2", horizontal_alignment: "center" },
            ],
          },
          {
            cells: [
              { text: "Trail Running Socks" },
              { text: "4", horizontal_alignment: "center" },
            ],
          },
          {
            cells: [
              { text: "Insulated Water Bottle" },
              { text: "1", horizontal_alignment: "center" },
            ],
          },
        ],
      },
      { type: "separator" },
      {
        type: "paragraph",
        runs: [
          { text: "Ship To:\n", font_weight: "bold" },
          { text: "Sarah Mitchell\n84 Maple Street\nPortland, OR 97201" },
        ],
      },
      { type: "separator" },
      { type: "paragraph", runs: [{ text: "Thank you for your order!" }] },
    ],
  },
});

// Step 2: Generate PNG shipping label
const shippingLabel = await client.generateImage({
  dimensions: { width_in_px: 600, height_in_px: 400 },
  output_format: "png",
  layers: [
    { index: 0, type: "solid-color", hex_color: "#FFFFFF" },
    {
      index: 1,
      type: "solid-color",
      hex_color: "#1A1A2E",
      position: { x_in_px: 0, y_in_px: 0 },
      dimensions: { width_in_px: 600, height_in_px: 60 },
    },
    {
      index: 2,
      type: "text",
      text: "SUMMIT OUTDOOR GEAR",
      font_name: "Inter",
      font_size_in_px: 18,
      font_weight: "bold",
      text_color: "#FFFFFF",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 0 },
      dimensions: { width_in_px: 552, height_in_px: 60 },
    },
    {
      index: 3,
      type: "solid-color",
      hex_color: "#CCCCCC",
      position: { x_in_px: 0, y_in_px: 60 },
      dimensions: { width_in_px: 600, height_in_px: 1 },
    },
    {
      index: 4,
      type: "text",
      text: "SHIP TO:",
      font_name: "Inter",
      font_size_in_px: 12,
      font_weight: "regular",
      text_color: "#6B7280",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 80 },
      dimensions: { width_in_px: 400, height_in_px: 20 },
    },
    {
      index: 5,
      type: "text",
      text: "Sarah Mitchell",
      font_name: "Inter",
      font_size_in_px: 28,
      font_weight: "bold",
      text_color: "#1A1A2E",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 108 },
      dimensions: { width_in_px: 552, height_in_px: 44 },
    },
    {
      index: 6,
      type: "text",
      text: "84 Maple Street",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#374151",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 160 },
      dimensions: { width_in_px: 400, height_in_px: 28 },
    },
    {
      index: 7,
      type: "text",
      text: "Portland, OR 97201",
      font_name: "Inter",
      font_size_in_px: 16,
      font_weight: "regular",
      text_color: "#374151",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 192 },
      dimensions: { width_in_px: 400, height_in_px: 28 },
    },
    {
      index: 8,
      type: "solid-color",
      hex_color: "#CCCCCC",
      position: { x_in_px: 0, y_in_px: 350 },
      dimensions: { width_in_px: 600, height_in_px: 1 },
    },
    {
      index: 9,
      type: "text",
      text: "#WH-29341",
      font_name: "Inter",
      font_size_in_px: 12,
      font_weight: "regular",
      text_color: "#6B7280",
      text_align: "left",
      vertical_align: "center",
      position: { x_in_px: 24, y_in_px: 358 },
      dimensions: { width_in_px: 400, height_in_px: 28 },
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Generate PDF packing slip
packing_slip = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": "Packing Slip - Order #WH-29341",
            "author": "Summit Outdoor Gear",
        },
        "page": {
            "size": {"preset": "A4"},
            "margins": {
                "top_in_pt": 54,
                "right_in_pt": 54,
                "bottom_in_pt": 54,
                "left_in_pt": 54,
            },
        },
        "styles": {
            "text": {
                "font_family": "Helvetica",
                "font_size_in_pt": 11.0,
                "line_height": 1.5,
                "color": "#333333",
            },
            "headline": {
                "font_family": "Helvetica",
                "font_size_in_pt": 24.0,
                "color": "#111111",
                "spacing_before_in_pt": 12.0,
                "spacing_after_in_pt": 6.0,
                "font_weight": "bold",
            },
            "link": {
                "color": "#0066CC",
                "is_underlined": True,
            },
            "list": {
                "text_style": {
                    "font_family": "Helvetica",
                    "font_size_in_pt": 11.0,
                    "line_height": 1.5,
                    "color": "#333333",
                },
                "marker_color": "#333333",
                "marker_gap_in_pt": 8.0,
            },
            "table": {
                "header": {
                    "background_color": "#1A1A2E",
                    "text_color": "#FFFFFF",
                    "font_size_in_pt": 11.0,
                    "font_weight": "bold",
                },
                "body": {
                    "background_color": "#FFFFFF",
                    "text_color": "#333333",
                    "font_size_in_pt": 11.0,
                },
                "border": {
                    "outer": {
                        "top": {"color": "#CCCCCC", "width_in_pt": 1.0},
                        "right": {"color": "#CCCCCC", "width_in_pt": 1.0},
                        "bottom": {"color": "#CCCCCC", "width_in_pt": 1.0},
                        "left": {"color": "#CCCCCC", "width_in_pt": 1.0},
                    },
                    "inner": {
                        "horizontal": {"color": "#EEEEEE", "width_in_pt": 0.5},
                        "vertical": {"color": "#EEEEEE", "width_in_pt": 0.5},
                    },
                },
            },
            "grid": {
                "background_color": "#FFFFFF",
                "border_color": "#CCCCCC",
                "border_width_in_pt": 0.0,
                "gap_in_pt": 12.0,
            },
            "separator": {
                "color": "#CCCCCC",
                "thickness_in_pt": 1.0,
                "spacing_before_in_pt": 12.0,
                "spacing_after_in_pt": 12.0,
            },
            "image": {
                "border_color": "#000000",
                "border_width_in_pt": 0.0,
            },
        },
        "content": [
            {"type": "headline", "level": "h1", "text": "Summit Outdoor Gear"},
            {
                "type": "paragraph",
                "runs": [{"text": "1200 Warehouse Pkwy, Denver, CO 80216  ·  summitoutdoorgear.com"}],
            },
            {"type": "headline", "level": "h2", "text": "Packing Slip"},
            {
                "type": "paragraph",
                "runs": [
                    {"text": "Order #: ", "font_weight": "bold"},
                    {"text": "WH-29341\n"},
                    {"text": "Date: ", "font_weight": "bold"},
                    {"text": "March 29, 2026\n"},
                    {"text": "Customer: ", "font_weight": "bold"},
                    {"text": "Sarah Mitchell"},
                ],
            },
            {"type": "separator"},
            {
                "type": "table",
                "column_widths_in_percent": [70, 30],
                "header": {
                    "cells": [
                        {"text": "Item"},
                        {"text": "Qty", "horizontal_alignment": "center"},
                    ],
                },
                "rows": [
                    {"cells": [{"text": "Merino Wool Beanie"}, {"text": "2", "horizontal_alignment": "center"}]},
                    {"cells": [{"text": "Trail Running Socks"}, {"text": "4", "horizontal_alignment": "center"}]},
                    {"cells": [{"text": "Insulated Water Bottle"}, {"text": "1", "horizontal_alignment": "center"}]},
                ],
            },
            {"type": "separator"},
            {
                "type": "paragraph",
                "runs": [
                    {"text": "Ship To:\n", "font_weight": "bold"},
                    {"text": "Sarah Mitchell\n84 Maple Street\nPortland, OR 97201"},
                ],
            },
            {"type": "separator"},
            {"type": "paragraph", "runs": [{"text": "Thank you for your order!"}]},
        ],
    },
)

# Step 2: Generate PNG shipping label
shipping_label = client.generate_image(
    dimensions={"width_in_px": 600, "height_in_px": 400},
    output_format="png",
    layers=[
        {"index": 0, "type": "solid-color", "hex_color": "#FFFFFF"},
        {
            "index": 1,
            "type": "solid-color",
            "hex_color": "#1A1A2E",
            "position": {"x_in_px": 0, "y_in_px": 0},
            "dimensions": {"width_in_px": 600, "height_in_px": 60},
        },
        {
            "index": 2,
            "type": "text",
            "text": "SUMMIT OUTDOOR GEAR",
            "font_name": "Inter",
            "font_size_in_px": 18,
            "font_weight": "bold",
            "text_color": "#FFFFFF",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 0},
            "dimensions": {"width_in_px": 552, "height_in_px": 60},
        },
        {
            "index": 3,
            "type": "solid-color",
            "hex_color": "#CCCCCC",
            "position": {"x_in_px": 0, "y_in_px": 60},
            "dimensions": {"width_in_px": 600, "height_in_px": 1},
        },
        {
            "index": 4,
            "type": "text",
            "text": "SHIP TO:",
            "font_name": "Inter",
            "font_size_in_px": 12,
            "font_weight": "regular",
            "text_color": "#6B7280",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 80},
            "dimensions": {"width_in_px": 400, "height_in_px": 20},
        },
        {
            "index": 5,
            "type": "text",
            "text": "Sarah Mitchell",
            "font_name": "Inter",
            "font_size_in_px": 28,
            "font_weight": "bold",
            "text_color": "#1A1A2E",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 108},
            "dimensions": {"width_in_px": 552, "height_in_px": 44},
        },
        {
            "index": 6,
            "type": "text",
            "text": "84 Maple Street",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#374151",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 160},
            "dimensions": {"width_in_px": 400, "height_in_px": 28},
        },
        {
            "index": 7,
            "type": "text",
            "text": "Portland, OR 97201",
            "font_name": "Inter",
            "font_size_in_px": 16,
            "font_weight": "regular",
            "text_color": "#374151",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 192},
            "dimensions": {"width_in_px": 400, "height_in_px": 28},
        },
        {
            "index": 8,
            "type": "solid-color",
            "hex_color": "#CCCCCC",
            "position": {"x_in_px": 0, "y_in_px": 350},
            "dimensions": {"width_in_px": 600, "height_in_px": 1},
        },
        {
            "index": 9,
            "type": "text",
            "text": "#WH-29341",
            "font_name": "Inter",
            "font_size_in_px": 12,
            "font_weight": "regular",
            "text_color": "#6B7280",
            "text_align": "left",
            "vertical_align": "center",
            "position": {"x_in_px": 24, "y_in_px": 358},
            "dimensions": {"width_in_px": 400, "height_in_px": 28},
        },
    ],
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Generate PDF packing slip
	_, err := client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{
				Title:  "Packing Slip - Order #WH-29341",
				Author: "Summit Outdoor Gear",
			},
			Page: il.DocumentPage{
				Size: il.DocPageSize{Preset: "A4"},
				Margins: il.DocMargins{
					TopInPt:    54,
					RightInPt:  54,
					BottomInPt: 54,
					LeftInPt:   54,
				},
			},
			Content: []il.ContentBlock{
				il.NewHeadlineBlock("h1", "Summit Outdoor Gear"),
				il.ParagraphBlock{Type: "paragraph", Runs: []il.TextRun{
					{Text: "1200 Warehouse Pkwy, Denver, CO 80216  ·  summitoutdoorgear.com"},
				}},
				il.NewHeadlineBlock("h2", "Packing Slip"),
				il.ParagraphBlock{Type: "paragraph", Runs: []il.TextRun{
					{Text: "Order #: ", IsBold: true},
					{Text: "WH-29341\n"},
					{Text: "Date: ", IsBold: true},
					{Text: "March 29, 2026\n"},
					{Text: "Customer: ", IsBold: true},
					{Text: "Sarah Mitchell"},
				}},
				il.NewSeparatorBlock(),
				il.NewTableBlock([]il.TableRow{
					{Cells: []il.TableCell{{Text: "Merino Wool Beanie"}, {Text: "2"}}},
					{Cells: []il.TableCell{{Text: "Trail Running Socks"}, {Text: "4"}}},
					{Cells: []il.TableCell{{Text: "Insulated Water Bottle"}, {Text: "1"}}},
				}),
				il.NewSeparatorBlock(),
				il.ParagraphBlock{Type: "paragraph", Runs: []il.TextRun{
					{Text: "Ship To:\n", IsBold: true},
					{Text: "Sarah Mitchell\n84 Maple Street\nPortland, OR 97201"},
				}},
				il.NewSeparatorBlock(),
				il.ParagraphBlock{Type: "paragraph", Runs: []il.TextRun{
					{Text: "Thank you for your order!"},
				}},
			},
		},
	})
	if err != nil {
		panic(err)
	}

	// Step 2: Generate PNG shipping label
	_, err = client.GenerateImage(il.GenerateImageRequest{
		Dimensions:   il.Dimensions{WidthInPx: 600, HeightInPx: 400},
		OutputFormat: "png",
		Layers: []il.Layer{
			il.NewSolidColorBackgroundLayer(0, "#FFFFFF"),
			il.NewSolidColorLayer(1, "#1A1A2E",
				il.Position{XInPx: 0, YInPx: 0},
				il.Dimensions{WidthInPx: 600, HeightInPx: 60},
			),
			il.NewTextLayer(2, "SUMMIT OUTDOOR GEAR", "Inter", 18, "#FFFFFF",
				il.Position{XInPx: 24, YInPx: 0},
				il.Dimensions{WidthInPx: 552, HeightInPx: 60},
			),
			il.NewSolidColorLayer(3, "#CCCCCC",
				il.Position{XInPx: 0, YInPx: 60},
				il.Dimensions{WidthInPx: 600, HeightInPx: 1},
			),
			il.NewTextLayer(4, "SHIP TO:", "Inter", 12, "#6B7280",
				il.Position{XInPx: 24, YInPx: 80},
				il.Dimensions{WidthInPx: 400, HeightInPx: 20},
			),
			il.NewTextLayer(5, "Sarah Mitchell", "Inter", 28, "#1A1A2E",
				il.Position{XInPx: 24, YInPx: 108},
				il.Dimensions{WidthInPx: 552, HeightInPx: 44},
			),
			il.NewTextLayer(6, "84 Maple Street", "Inter", 16, "#374151",
				il.Position{XInPx: 24, YInPx: 160},
				il.Dimensions{WidthInPx: 400, HeightInPx: 28},
			),
			il.NewTextLayer(7, "Portland, OR 97201", "Inter", 16, "#374151",
				il.Position{XInPx: 24, YInPx: 192},
				il.Dimensions{WidthInPx: 400, HeightInPx: 28},
			),
			il.NewSolidColorLayer(8, "#CCCCCC",
				il.Position{XInPx: 0, YInPx: 350},
				il.Dimensions{WidthInPx: 600, HeightInPx: 1},
			),
			il.NewTextLayer(9, "#WH-29341", "Inter", 12, "#6B7280",
				il.Position{XInPx: 24, YInPx: 358},
				il.Dimensions{WidthInPx: 400, HeightInPx: 28},
			),
		},
	})
	if err != nil {
		panic(err)
	}
}
```

```n8n
{
  "name": "Generate Packing Slip and Shipping Label",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Packing Slip and Shipping Label\n\nFulfillment centers and e-commerce platforms generate a PDF packing slip and a PNG shipping label from the same order data in a single workflow \u2014 both documents ready for printing without entering data twice.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "a2c898a4-cf40-443e-b728-84c11b9dc008",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Packing Slip\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
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
      "id": "75a1545f-c7c4-4a23-992d-65ba960e3a2c",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Shipping Label\nResource: **Image Generation**\n\nConfigure the Image Generation parameters below, then connect your credentials.",
        "height_in_px": 160,
        "width_in_px": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        725,
        100
      ],
      "id": "941d9dbe-ca7c-4496-b9c7-71aef85d25f3",
      "name": "Step 2 Note"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        250,
        300
      ],
      "id": "a2b3c4d5-e6f7-8901-2a67-901234567890",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"Packing Slip - Order #WH-29341\",\n    \"author\": \"Summit Outdoor Gear\"\n  },\n  \"page\": {\n    \"size\": { \"preset\": \"A4\" },\n    \"margins\": {\n      \"top_in_pt\": 54,\n      \"right_in_pt\": 54,\n      \"bottom_in_pt\": 54,\n      \"left_in_pt\": 54\n    }\n  },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Summit Outdoor Gear\" },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [{ \"text\": \"1200 Warehouse Pkwy, Denver, CO 80216  \u00b7  summitoutdoorgear.com\" }]\n    },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"Packing Slip\" },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        { \"text\": \"Order #: \", \"font_weight\": \"bold\" },\n        { \"text\": \"WH-29341\\n\" },\n        { \"text\": \"Date: \", \"font_weight\": \"bold\" },\n        { \"text\": \"March 29, 2026\\n\" },\n        { \"text\": \"Customer: \", \"font_weight\": \"bold\" },\n        { \"text\": \"Sarah Mitchell\" }\n      ]\n    },\n    { \"type\": \"separator\" },\n    {\n      \"type\": \"table\",\n      \"column_widths_in_percent\": [70, 30],\n      \"header\": {\n        \"cells\": [\n          { \"text\": \"Item\" },\n          { \"text\": \"Qty\", \"horizontal_alignment\": \"center\" }\n        ]\n      },\n      \"rows\": [\n        { \"cells\": [{ \"text\": \"Merino Wool Beanie\" }, { \"text\": \"2\", \"horizontal_alignment\": \"center\" }] },\n        { \"cells\": [{ \"text\": \"Trail Running Socks\" }, { \"text\": \"4\", \"horizontal_alignment\": \"center\" }] },\n        { \"cells\": [{ \"text\": \"Insulated Water Bottle\" }, { \"text\": \"1\", \"horizontal_alignment\": \"center\" }] }\n      ]\n    },\n    { \"type\": \"separator\" },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        { \"text\": \"Ship To:\\n\", \"font_weight\": \"bold\" },\n        { \"text\": \"Sarah Mitchell\\n84 Maple Street\\nPortland, OR 97201\" }\n      ]\n    },\n    { \"type\": \"separator\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Thank you for your order!\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "b3c4d5e6-f7a8-9012-3b78-012345678901",
      "name": "Generate Packing Slip",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "imageGeneration",
        "widthInPx": 600,
        "heightInPx": 400,
        "outputFormat": "png",
        "layersJson": "[\n  {\n    \"index\": 0,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#FFFFFF\"\n  },\n  {\n    \"index\": 1,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#1A1A2E\",\n    \"position\": { \"x\": 0, \"y\": 0 },\n    \"dimensions\": { \"width\": 600, \"height\": 60 }\n  },\n  {\n    \"index\": 2,\n    \"type\": \"text\",\n    \"text\": \"SUMMIT OUTDOOR GEAR\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 18,\n    \"font_weight\": \"bold\",\n    \"text_color\": \"#FFFFFF\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 0 },\n    \"dimensions\": { \"width\": 552, \"height\": 60 }\n  },\n  {\n    \"index\": 3,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#CCCCCC\",\n    \"position\": { \"x\": 0, \"y\": 60 },\n    \"dimensions\": { \"width\": 600, \"height\": 1 }\n  },\n  {\n    \"index\": 4,\n    \"type\": \"text\",\n    \"text\": \"SHIP TO:\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 12,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#6B7280\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 80 },\n    \"dimensions\": { \"width\": 400, \"height\": 20 }\n  },\n  {\n    \"index\": 5,\n    \"type\": \"text\",\n    \"text\": \"Sarah Mitchell\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 28,\n    \"font_weight\": \"bold\",\n    \"text_color\": \"#1A1A2E\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 108 },\n    \"dimensions\": { \"width\": 552, \"height\": 44 }\n  },\n  {\n    \"index\": 6,\n    \"type\": \"text\",\n    \"text\": \"84 Maple Street\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 16,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#374151\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 160 },\n    \"dimensions\": { \"width\": 400, \"height\": 28 }\n  },\n  {\n    \"index\": 7,\n    \"type\": \"text\",\n    \"text\": \"Portland, OR 97201\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 16,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#374151\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 192 },\n    \"dimensions\": { \"width\": 400, \"height\": 28 }\n  },\n  {\n    \"index\": 8,\n    \"type\": \"solid-color\",\n    \"hex_color\": \"#CCCCCC\",\n    \"position\": { \"x\": 0, \"y\": 350 },\n    \"dimensions\": { \"width\": 600, \"height\": 1 }\n  },\n  {\n    \"index\": 9,\n    \"type\": \"text\",\n    \"text\": \"#WH-29341\",\n    \"font_name\": \"Inter\",\n    \"font_size_in_px\": 12,\n    \"font_weight\": \"regular\",\n    \"text_color\": \"#6B7280\",\n    \"text_align\": \"left\",\n    \"vertical_align\": \"center\",\n    \"position\": { \"x\": 24, \"y\": 358 },\n    \"dimensions\": { \"width\": 400, \"height\": 28 }\n  }\n]",
        "fontsJson": "[]"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "c4d5e6f7-a8b9-0123-4c89-123456789012",
      "name": "Generate Shipping Label",
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
            "node": "Generate Packing Slip",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate Packing Slip": {
      "main": [
        [
          {
            "node": "Generate Shipping Label",
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
Generate a PDF packing slip and a PNG shipping label from the same order data. First, use generate_document with format "pdf" to create a packing slip with order number, shipping address, and itemized contents. Then, use generate_image to create a 4x6 inch shipping label with sender address, recipient address, barcode with [tracking number], and shipping method.
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
