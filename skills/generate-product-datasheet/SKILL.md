---
name: generate-product-datasheet
description: Generate a professional product specification sheet with images, feature tables, technical specs, and contact information.
---

# Generate Product Datasheet

Hardware manufacturers and product marketing teams use this recipe to generate a consistent datasheet for a product. Define product images, feature highlights, technical specifications, and contact details — producing a polished PDF ready for sales teams, distributors, and trade shows.

## APIs Used

Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Product Datasheet - Apex SoundStage 360",
        "author": "Apex Audio Inc."
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
            "background_color": "#333333",
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
              "top": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "right": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "bottom": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              },
              "left": {
                "color": "#CCCCCC",
                "width_in_pt": 1.0
              }
            },
            "inner": {
              "horizontal": {
                "color": "#EEEEEE",
                "width_in_pt": 0.5
              },
              "vertical": {
                "color": "#EEEEEE",
                "width_in_pt": 0.5
              }
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
      "header": [
        {
          "type": "paragraph",
          "runs": [{
            "text": "Apex Audio Inc. — Product Datasheet",
            "font_weight": "bold"
          }]
        }
      ],
      "footer": [
        {
            "type": "page-number",
            "text_alignment": "center",
        }
      ],
      "content": [
        {
            "type": "headline",
            "level": "h1",
            "text": "Apex SoundStage 360",
        },
        {
            "type": "paragraph",
            "markdown": "The **Apex SoundStage 360** is a premium wireless smart speaker delivering immersive 360-degree audio with built-in voice assistant support,
            multi-room streaming,
            and audiophile-grade sound processing.",
        },
        { "type": "grid", "columns": [
          { "column_span": 5, "blocks": [
            {
              "type": "image",
              "buffer": "<base64-encoded-image-data>",
              "width_in_pt": 200,
              "height_in_pt": 200
            }
          ]},
          { "column_span": 7, "blocks": [
            {
                "type": "headline",
                "level": "h2",
                "text": "Key Features",
            },
            { "type": "list", "variant": "unordered", "items": [
              { "text": "360-degree omnidirectional sound projection" },
              { "text": "Dual 40mm full-range drivers + passive bass radiator" },
              { "text": "Wi-Fi 6E and Bluetooth 5.3 connectivity" },
              {
                  "text": "AirPlay 2,
                  Chromecast,
                  and Spotify Connect built in",
              },
              { "text": "IPX4 splash resistance for indoor/outdoor use" },
              { "text": "18-hour battery life with USB-C fast charging" }
            ]}
          ]}
        ]},
        { "type": "separator" },
        {
            "type": "headline",
            "level": "h2",
            "text": "Technical Specifications",
        },
        {
          "type": "table",
          "column_widths_in_percent": [35, 65],
          "header": {
            "cells": [
              { "text": "Specification" },
              { "text": "Details" }
            ]
          },
          "rows": [
            { "cells": [{ "text": "Drivers" }, { "text": "2 x 40mm full-range neodymium + 1 x 65mm passive bass radiator" }] },
            { "cells": [{ "text": "Frequency Response" }, { "text": "55 Hz – 20 kHz (±3 dB)" }] },
            { "cells": [{ "text": "Peak Output" }, { "text": "96 dB SPL @ 1m" }] },
            { "cells": [{ "text": "Connectivity" }, {
                "text": "Wi-Fi 6E (802.11ax),
                Bluetooth 5.3 (aptX HD, AAC, SBC)",
            }] },
            { "cells": [{ "text": "Voice Assistants" }, {
                "text": "Amazon Alexa,
                Google Assistant (far-field 4-mic array)",
            }] },
            { "cells": [{ "text": "Battery" }, {
                "text": "5,
                000 mAh Li-Ion,
                18 hours playback,
                2.5 hours full charge",
            }] },
            { "cells": [{ "text": "Water Resistance" }, { "text": "IPX4 (splash-proof)" }] },
            { "cells": [{ "text": "Dimensions" }, { "text": "128mm diameter x 185mm height" }] },
            { "cells": [{ "text": "Weight" }, { "text": "1.2 kg (2.65 lbs)" }] },
            { "cells": [{ "text": "Colors" }, {
                "text": "Midnight Black,
                Arctic White,
                Sage Green",
            }] }
          ]
        },
        { "type": "separator" },
        {
            "type": "headline",
            "level": "h2",
            "text": "What'"'"'s in the Box",
        },
        { "type": "list", "variant": "unordered", "items": [
          { "text": "Apex SoundStage 360 speaker" },
          { "text": "USB-C charging cable (1.5m)" },
          { "text": "30W USB-C power adapter" },
          { "text": "Quick start guide and safety information" }
        ]},
        { "type": "separator" },
        { "type": "grid", "columns": [
          { "column_span": 6, "blocks": [
            {
                "type": "headline",
                "level": "h3",
                "text": "Sales & Distribution",
            },
            {
                "type": "paragraph",
                "markdown": "**Apex Audio Inc.**\n2400 Innovation Drive,
                Suite 800\nSan Jose,
                CA 95134\nUnited States",
            },
            { "type": "paragraph", "runs": [
              {
                  "text": "sales@apexaudio.com",
                  "font_weight": "bold",
              }
            ]}
          ]},
          { "column_span": 6, "blocks": [
            {
                "type": "headline",
                "level": "h3",
                "text": "Technical Support",
            },
            {
                "type": "paragraph",
                "markdown": "**Phone:** +1 (408) 555-0192\n**Email:** support@apexaudio.com\n**Web:** www.apexaudio.com/support",
            },
            {
              "type": "qr-code",
              "value": "https://www.apexaudio.com/products/soundstage-360",
              "width_in_pt": 72,
              "height_in_pt": 72,
              "fg_hex_color": "#000000",
              "bg_hex_color": "#FFFFFF"
            }
          ]}
        ]}
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: "Product Datasheet - Apex SoundStage 360",
      author: "Apex Audio Inc.",
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
          background_color: "#333333",
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
            top: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            right: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            bottom: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
            left: {
              color: "#CCCCCC",
              width_in_pt: 1.0,
            },
          },
          inner: {
            horizontal: {
              color: "#EEEEEE",
              width_in_pt: 0.5,
            },
            vertical: {
              color: "#EEEEEE",
              width_in_pt: 0.5,
            },
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
    header: [
      {
        type: "paragraph",
        runs: [{
          text: "Apex Audio Inc. — Product Datasheet",
          font_weight: "bold",
        }],
      },
    ],
    footer: [
      {
          type: "page-number",
          text_alignment: "center",
      },
    ],
    content: [
      {
          type: "headline",
          level: "h1",
          text: "Apex SoundStage 360",
      },
      {
          type: "paragraph",
          markdown: "The **Apex SoundStage 360** is a premium wireless smart speaker delivering immersive 360-degree audio with built-in voice assistant support,
          multi-room streaming,
          and audiophile-grade sound processing.",
      },
      { type: "grid", columns: [
        { column_span: 5, blocks: [
          {
            type: "image",
            buffer: "<base64-encoded-image-data>",
            width_in_pt: 200,
            height_in_pt: 200,
          },
        ]},
        { column_span: 7, blocks: [
          {
              type: "headline",
              level: "h2",
              text: "Key Features",
          },
          { type: "list", variant: "unordered", items: [
            { text: "360-degree omnidirectional sound projection" },
            { text: "Dual 40mm full-range drivers + passive bass radiator" },
            { text: "Wi-Fi 6E and Bluetooth 5.3 connectivity" },
            {
                text: "AirPlay 2,
                Chromecast,
                and Spotify Connect built in",
            },
            { text: "IPX4 splash resistance for indoor/outdoor use" },
            { text: "18-hour battery life with USB-C fast charging" },
          ]},
        ]},
      ]},
      { type: "separator" },
      {
          type: "headline",
          level: "h2",
          text: "Technical Specifications",
      },
      {
        type: "table",
        column_widths_in_percent: [35, 65],
        header: {
          cells: [
            { text: "Specification" },
            { text: "Details" },
          ],
        },
        rows: [
          { cells: [{ text: "Drivers" }, { text: "2 x 40mm full-range neodymium + 1 x 65mm passive bass radiator" }] },
          { cells: [{ text: "Frequency Response" }, { text: "55 Hz – 20 kHz (±3 dB)" }] },
          { cells: [{ text: "Peak Output" }, { text: "96 dB SPL @ 1m" }] },
          { cells: [{ text: "Connectivity" }, {
              text: "Wi-Fi 6E (802.11ax),
              Bluetooth 5.3 (aptX HD, AAC, SBC)",
          }] },
          { cells: [{ text: "Voice Assistants" }, {
              text: "Amazon Alexa,
              Google Assistant (far-field 4-mic array)",
          }] },
          { cells: [{ text: "Battery" }, {
              text: "5,
              000 mAh Li-Ion,
              18 hours playback,
              2.5 hours full charge",
          }] },
          { cells: [{ text: "Water Resistance" }, { text: "IPX4 (splash-proof)" }] },
          { cells: [{ text: "Dimensions" }, { text: "128mm diameter x 185mm height" }] },
          { cells: [{ text: "Weight" }, { text: "1.2 kg (2.65 lbs)" }] },
          { cells: [{ text: "Colors" }, {
              text: "Midnight Black,
              Arctic White,
              Sage Green",
          }] },
        ],
      },
      { type: "separator" },
      {
          type: "headline",
          level: "h2",
          text: "What's in the Box",
      },
      { type: "list", variant: "unordered", items: [
        { text: "Apex SoundStage 360 speaker" },
        { text: "USB-C charging cable (1.5m)" },
        { text: "30W USB-C power adapter" },
        { text: "Quick start guide and safety information" },
      ]},
      { type: "separator" },
      { type: "grid", columns: [
        { column_span: 6, blocks: [
          {
              type: "headline",
              level: "h3",
              text: "Sales & Distribution",
          },
          {
              type: "paragraph",
              markdown: "**Apex Audio Inc.**\n2400 Innovation Drive,
              Suite 800\nSan Jose,
              CA 95134\nUnited States",
          },
          { type: "paragraph", runs: [
            {
                text: "sales@apexaudio.com",
                font_weight: "bold",
            },
          ]},
        ]},
        { column_span: 6, blocks: [
          {
              type: "headline",
              level: "h3",
              text: "Technical Support",
          },
          {
              type: "paragraph",
              markdown: "**Phone:** +1 (408) 555-0192\n**Email:** support@apexaudio.com\n**Web:** www.apexaudio.com/support",
          },
          {
            type: "qr-code",
            value: "https://www.apexaudio.com/products/soundstage-360",
            width_in_pt: 72,
            height_in_pt: 72,
            fg_hex_color: "#000000",
            bg_hex_color: "#FFFFFF",
          },
        ]},
      ]},
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": "Product Datasheet - Apex SoundStage 360",
            "author": "Apex Audio Inc.",
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
                    "background_color": "#333333",
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
                        "top": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "right": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "bottom": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                        "left": {
                            "color": "#CCCCCC",
                            "width_in_pt": 1.0,
                        },
                    },
                    "inner": {
                        "horizontal": {
                            "color": "#EEEEEE",
                            "width_in_pt": 0.5,
                        },
                        "vertical": {
                            "color": "#EEEEEE",
                            "width_in_pt": 0.5,
                        },
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
        "header": [
            {
                "type": "paragraph",
                "runs": [{
                    "text": "Apex Audio Inc. — Product Datasheet",
                    "font_weight": "bold",
                }],
            },
        ],
        "footer": [
            {
                "type": "page-number",
                "text_alignment": "center",
            },
        ],
        "content": [
            {
                "type": "headline",
                "level": "h1",
                "text": "Apex SoundStage 360",
            },
            {
                "type": "paragraph",
                "markdown": "The **Apex SoundStage 360** is a premium wireless smart speaker delivering immersive 360-degree audio with built-in voice assistant support,
                multi-room streaming,
                and audiophile-grade sound processing.",
            },
            {"type": "grid", "columns": [
                {"column_span": 5, "blocks": [
                    {
                        "type": "image",
                        "buffer": "<base64-encoded-image-data>",
                        "width_in_pt": 200,
                        "height_in_pt": 200,
                    },
                ]},
                {"column_span": 7, "blocks": [
                    {
                        "type": "headline",
                        "level": "h2",
                        "text": "Key Features",
                    },
                    {"type": "list", "variant": "unordered", "items": [
                        {"text": "360-degree omnidirectional sound projection"},
                        {"text": "Dual 40mm full-range drivers + passive bass radiator"},
                        {"text": "Wi-Fi 6E and Bluetooth 5.3 connectivity"},
                        {
                            "text": "AirPlay 2,
                            Chromecast,
                            and Spotify Connect built in",
                        },
                        {"text": "IPX4 splash resistance for indoor/outdoor use"},
                        {"text": "18-hour battery life with USB-C fast charging"},
                    ]},
                ]},
            ]},
            {"type": "separator"},
            {
                "type": "headline",
                "level": "h2",
                "text": "Technical Specifications",
            },
            {
                "type": "table",
                "column_widths_in_percent": [35, 65],
                "header": {
                    "cells": [
                        {"text": "Specification"},
                        {"text": "Details"},
                    ],
                },
                "rows": [
                    {"cells": [{"text": "Drivers"}, {"text": "2 x 40mm full-range neodymium + 1 x 65mm passive bass radiator"}]},
                    {"cells": [{"text": "Frequency Response"}, {"text": "55 Hz – 20 kHz (±3 dB)"}]},
                    {"cells": [{"text": "Peak Output"}, {"text": "96 dB SPL @ 1m"}]},
                    {"cells": [{"text": "Connectivity"}, {
                        "text": "Wi-Fi 6E (802.11ax),
                        Bluetooth 5.3 (aptX HD, AAC, SBC)",
                    }]},
                    {"cells": [{"text": "Voice Assistants"}, {
                        "text": "Amazon Alexa,
                        Google Assistant (far-field 4-mic array)",
                    }]},
                    {"cells": [{"text": "Battery"}, {
                        "text": "5,
                        000 mAh Li-Ion,
                        18 hours playback,
                        2.5 hours full charge",
                    }]},
                    {"cells": [{"text": "Water Resistance"}, {"text": "IPX4 (splash-proof)"}]},
                    {"cells": [{"text": "Dimensions"}, {"text": "128mm diameter x 185mm height"}]},
                    {"cells": [{"text": "Weight"}, {"text": "1.2 kg (2.65 lbs)"}]},
                    {"cells": [{"text": "Colors"}, {
                        "text": "Midnight Black,
                        Arctic White,
                        Sage Green",
                    }]},
                ],
            },
            {"type": "separator"},
            {
                "type": "headline",
                "level": "h2",
                "text": "What's in the Box",
            },
            {"type": "list", "variant": "unordered", "items": [
                {"text": "Apex SoundStage 360 speaker"},
                {"text": "USB-C charging cable (1.5m)"},
                {"text": "30W USB-C power adapter"},
                {"text": "Quick start guide and safety information"},
            ]},
            {"type": "separator"},
            {"type": "grid", "columns": [
                {"column_span": 6, "blocks": [
                    {
                        "type": "headline",
                        "level": "h3",
                        "text": "Sales & Distribution",
                    },
                    {
                        "type": "paragraph",
                        "markdown": "**Apex Audio Inc.**\n2400 Innovation Drive,
                        Suite 800\nSan Jose,
                        CA 95134\nUnited States",
                    },
                    {"type": "paragraph", "runs": [
                        {
                            "text": "sales@apexaudio.com",
                            "font_weight": "bold",
                        },
                    ]},
                ]},
                {"column_span": 6, "blocks": [
                    {
                        "type": "headline",
                        "level": "h3",
                        "text": "Technical Support",
                    },
                    {
                        "type": "paragraph",
                        "markdown": "**Phone:** +1 (408) 555-0192\n**Email:** support@apexaudio.com\n**Web:** www.apexaudio.com/support",
                    },
                    {
                        "type": "qr-code",
                        "value": "https://www.apexaudio.com/products/soundstage-360",
                        "width_in_pt": 72,
                        "height_in_pt": 72,
                        "fg_hex_color": "#000000",
                        "bg_hex_color": "#FFFFFF",
                    },
                ]},
            ]},
        ],
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.GenerateDocument(il.GenerateDocumentRequest{
	Format: "pdf",
	Document: il.DocumentDefinition{
		Metadata: il.DocumentMetadata{
			Title:  "Product Datasheet - Apex SoundStage 360",
			Author: "Apex Audio Inc.",
		},
		Page: il.DocumentPage{
			Size:    il.DocPageSize{Preset: "A4"},
			Margins: il.DocMargins{
     TopInPt: 54,
     RightInPt: 54,
     BottomInPt: 54,
     LeftInPt: 54,
   },
		},
		Content: []il.ContentBlock{
			il.NewHeadlineBlock("h1", "Apex SoundStage 360"),
			il.NewParagraphBlock(),
			il.NewHeadlineBlock("h2", "Technical Specifications"),
			il.NewTableBlock([]il.TableRow{
				{Cells: []il.TableCell{
					{Text: "Drivers"},
					{Text: "2 x 40mm full-range neodymium + 1 x 65mm passive bass radiator"},
				}},
				{Cells: []il.TableCell{
					{Text: "Frequency Response"},
					{Text: "55 Hz – 20 kHz (±3 dB)"},
				}},
				{Cells: []il.TableCell{
					{Text: "Connectivity"},
					{
       Text: "Wi-Fi 6E (802.11ax),
       Bluetooth 5.3",
     },
				}},
			}),
			il.NewSeparatorBlock(),
			il.NewHeadlineBlock("h2", "What's in the Box"),
			il.NewListBlock("unordered", []il.ListItem{
				{Text: "Apex SoundStage 360 speaker"},
				{Text: "USB-C charging cable (1.5m)"},
				{Text: "30W USB-C power adapter"},
				{Text: "Quick start guide and safety information"},
			}),
		},
	},
	})
	if err != nil {
		panic(err)
	}

}
```

```n8n
{
  "name": "Generate Product Datasheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Generate Product Datasheet\n\nHardware manufacturers and product marketing teams use this recipe to generate a consistent datasheet for a product. Define product images, feature highlights, technical specifications, and contact details \u2014 producing a polished PDF ready for sales teams, distributors, and trade shows.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "d7559a3c-b6cd-42cb-a089-5f8f4890e331",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Generate Document\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
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
      "id": "dd906db1-411a-4a39-af1b-067ccf411cfd",
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
      "id": "75d53ef5-8536-43e0-a2ec-1285a34d8be2",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"Product Datasheet - Apex SoundStage 360\",\n    \"author\": \"Apex Audio Inc.\"\n  },\n  \"page\": {\n    \"size\": {\n      \"preset\": \"A4\"\n    },\n    \"margins\": {\n      \"top_in_pt\": 54,\n      \"right_in_pt\": 54,\n      \"bottom_in_pt\": 54,\n      \"left_in_pt\": 54\n    }\n  },\n  \"styles\": {\n    \"text\": {\n      \"font_family\": \"Helvetica\",\n      \"font_size_in_pt\": 11.0,\n      \"line_height\": 1.5,\n      \"color\": \"#333333\"\n    },\n    \"headline\": {\n      \"font_family\": \"Helvetica\",\n      \"font_size_in_pt\": 24.0,\n      \"color\": \"#111111\",\n      \"spacing_before_in_pt\": 12.0,\n      \"spacing_after_in_pt\": 6.0,\n      \"font_weight\": \"bold\"\n    },\n    \"link\": {\n      \"color\": \"#0066CC\",\n      \"is_underlined\": true\n    },\n    \"list\": {\n      \"text_style\": {\n        \"font_family\": \"Helvetica\",\n        \"font_size_in_pt\": 11.0,\n        \"line_height\": 1.5,\n        \"color\": \"#333333\"\n      },\n      \"marker_color\": \"#333333\",\n      \"marker_gap_in_pt\": 8.0\n    },\n    \"table\": {\n      \"header\": {\n        \"background_color\": \"#333333\",\n        \"text_color\": \"#FFFFFF\",\n        \"font_size_in_pt\": 11.0,\n        \"font_weight\": \"bold\"\n      },\n      \"body\": {\n        \"background_color\": \"#FFFFFF\",\n        \"text_color\": \"#333333\",\n        \"font_size_in_pt\": 11.0\n      },\n      \"border\": {\n        \"outer\": {\n          \"top\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"right\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"bottom\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          },\n          \"left\": {\n            \"color\": \"#CCCCCC\",\n            \"width_in_pt\": 1.0\n          }\n        },\n        \"inner\": {\n          \"horizontal\": {\n            \"color\": \"#EEEEEE\",\n            \"width_in_pt\": 0.5\n          },\n          \"vertical\": {\n            \"color\": \"#EEEEEE\",\n            \"width_in_pt\": 0.5\n          }\n        }\n      }\n    },\n    \"grid\": {\n      \"background_color\": \"#FFFFFF\",\n      \"border_color\": \"#CCCCCC\",\n      \"border_width_in_pt\": 0.0,\n      \"gap_in_pt\": 12.0\n    },\n    \"separator\": {\n      \"color\": \"#CCCCCC\",\n      \"thickness_in_pt\": 1.0,\n      \"spacing_before_in_pt\": 12.0,\n      \"spacing_after_in_pt\": 12.0\n    },\n    \"image\": {\n      \"border_color\": \"#000000\",\n      \"border_width_in_pt\": 0.0\n    }\n  },\n  \"header\": [\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        {\n          \"text\": \"Apex Audio Inc. \\u2014 Product Datasheet\",\n          \"font_weight\": \"bold\"\n        }\n      ]\n    }\n  ],\n  \"footer\": [\n    {\n      \"type\": \"page-number\",\n      \"text_alignment\": \"center\"\n    }\n  ],\n  \"content\": [\n    {\n      \"type\": \"headline\",\n      \"level\": \"h1\",\n      \"text\": \"Apex SoundStage 360\"\n    },\n    {\n      \"type\": \"paragraph\",\n      \"markdown\": \"The **Apex SoundStage 360** is a premium wireless smart speaker delivering immersive 360-degree audio with built-in voice assistant support,\\n            multi-room streaming,\\n            and audiophile-grade sound processing.\"\n    },\n    {\n      \"type\": \"grid\",\n      \"columns\": [\n        {\n          \"column_span\": 5,\n          \"blocks\": [\n            {\n              \"type\": \"image\",\n              \"buffer\": \"<base64-encoded-image-data>\",\n              \"width_in_pt\": 200,\n              \"height_in_pt\": 200\n            }\n          ]\n        },\n        {\n          \"column_span\": 7,\n          \"blocks\": [\n            {\n              \"type\": \"headline\",\n              \"level\": \"h2\",\n              \"text\": \"Key Features\"\n            },\n            {\n              \"type\": \"list\",\n              \"variant\": \"unordered\",\n              \"items\": [\n                {\n                  \"text\": \"360-degree omnidirectional sound projection\"\n                },\n                {\n                  \"text\": \"Dual 40mm full-range drivers + passive bass radiator\"\n                },\n                {\n                  \"text\": \"Wi-Fi 6E and Bluetooth 5.3 connectivity\"\n                },\n                {\n                  \"text\": \"AirPlay 2,\\n                  Chromecast,\\n                  and Spotify Connect built in\"\n                },\n                {\n                  \"text\": \"IPX4 splash resistance for indoor/outdoor use\"\n                },\n                {\n                  \"text\": \"18-hour battery life with USB-C fast charging\"\n                }\n              ]\n            }\n          ]\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"headline\",\n      \"level\": \"h2\",\n      \"text\": \"Technical Specifications\"\n    },\n    {\n      \"type\": \"table\",\n      \"column_widths_in_percent\": [\n        35,\n        65\n      ],\n      \"header\": {\n        \"cells\": [\n          {\n            \"text\": \"Specification\"\n          },\n          {\n            \"text\": \"Details\"\n          }\n        ]\n      },\n      \"rows\": [\n        {\n          \"cells\": [\n            {\n              \"text\": \"Drivers\"\n            },\n            {\n              \"text\": \"2 x 40mm full-range neodymium + 1 x 65mm passive bass radiator\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Frequency Response\"\n            },\n            {\n              \"text\": \"55 Hz \\u2013 20 kHz (\\u00b13 dB)\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Peak Output\"\n            },\n            {\n              \"text\": \"96 dB SPL @ 1m\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Connectivity\"\n            },\n            {\n              \"text\": \"Wi-Fi 6E (802.11ax),\\n                Bluetooth 5.3 (aptX HD, AAC, SBC)\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Voice Assistants\"\n            },\n            {\n              \"text\": \"Amazon Alexa,\\n                Google Assistant (far-field 4-mic array)\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Battery\"\n            },\n            {\n              \"text\": \"5,\\n                000 mAh Li-Ion,\\n                18 hours playback,\\n                2.5 hours full charge\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Water Resistance\"\n            },\n            {\n              \"text\": \"IPX4 (splash-proof)\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Dimensions\"\n            },\n            {\n              \"text\": \"128mm diameter x 185mm height\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Weight\"\n            },\n            {\n              \"text\": \"1.2 kg (2.65 lbs)\"\n            }\n          ]\n        },\n        {\n          \"cells\": [\n            {\n              \"text\": \"Colors\"\n            },\n            {\n              \"text\": \"Midnight Black,\\n                Arctic White,\\n                Sage Green\"\n            }\n          ]\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"headline\",\n      \"level\": \"h2\",\n      \"text\": \"What's in the Box\"\n    },\n    {\n      \"type\": \"list\",\n      \"variant\": \"unordered\",\n      \"items\": [\n        {\n          \"text\": \"Apex SoundStage 360 speaker\"\n        },\n        {\n          \"text\": \"USB-C charging cable (1.5m)\"\n        },\n        {\n          \"text\": \"30W USB-C power adapter\"\n        },\n        {\n          \"text\": \"Quick start guide and safety information\"\n        }\n      ]\n    },\n    {\n      \"type\": \"separator\"\n    },\n    {\n      \"type\": \"grid\",\n      \"columns\": [\n        {\n          \"column_span\": 6,\n          \"blocks\": [\n            {\n              \"type\": \"headline\",\n              \"level\": \"h3\",\n              \"text\": \"Sales & Distribution\"\n            },\n            {\n              \"type\": \"paragraph\",\n              \"markdown\": \"**Apex Audio Inc.**\\n2400 Innovation Drive,\\n                Suite 800\\nSan Jose,\\n                CA 95134\\nUnited States\"\n            },\n            {\n              \"type\": \"paragraph\",\n              \"runs\": [\n                {\n                  \"text\": \"sales@apexaudio.com\",\n                  \"font_weight\": \"bold\"\n                }\n              ]\n            }\n          ]\n        },\n        {\n          \"column_span\": 6,\n          \"blocks\": [\n            {\n              \"type\": \"headline\",\n              \"level\": \"h3\",\n              \"text\": \"Technical Support\"\n            },\n            {\n              \"type\": \"paragraph\",\n              \"markdown\": \"**Phone:** +1 (408) 555-0192\\n**Email:** support@apexaudio.com\\n**Web:** www.apexaudio.com/support\"\n            },\n            {\n              \"type\": \"qr-code\",\n              \"value\": \"https://www.apexaudio.com/products/soundstage-360\",\n              \"width_in_pt\": 72,\n              \"height_in_pt\": 72,\n              \"fg_hex_color\": \"#000000\",\n              \"bg_hex_color\": \"#FFFFFF\"\n            }\n          ]\n        }\n      ]\n    }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        500,
        300
      ],
      "id": "1dafcde3-4209-4683-841e-2122913dc766",
      "name": "Generate Document",
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
            "node": "Generate Document",
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
Generate a product datasheet PDF for [product name]. Use the generate_document tool with format "pdf" and these content blocks:

- Headline: [product name]
- Image: product photo from [image URL]
- Paragraph: product description and key features
- Table: technical specifications with columns for parameter and value
- Separator
- Paragraph: contact information and ordering details
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "JVBERi0xLjcKMSAwIG9iago8...",
    "mime_type": "application/pdf"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
