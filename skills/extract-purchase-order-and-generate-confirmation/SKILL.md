---
name: extract-purchase-order-and-generate-confirmation
description: Extract line items and delivery details from a customer purchase order PDF, then generate a formatted order confirmation PDF ready to send back to the buyer.
---

# Extract a Purchase Order and Generate a Confirmation

B2B suppliers and manufacturers use this pipeline to automatically process inbound purchase orders and return a formatted confirmation PDF — acknowledging line items, delivery date, and total — without manual re-entry.

## APIs Used

Document Extraction (1 credit per page), Document Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract purchase order data
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "po-northstar-retail.pdf",
        "url": "https://example.com/purchase-orders/po-northstar-retail.pdf"
      }
    ],
    "schema": {
      "fields": [
        { "name": "customer_name", "type": "TEXT", "description": "Name of the customer or buying company" },
        { "name": "po_number", "type": "TEXT", "description": "Purchase order number" },
        { "name": "po_date", "type": "DATE", "description": "Date the PO was issued" },
        { "name": "requested_delivery_date", "type": "DATE", "description": "Requested delivery date" },
        { "name": "billing_address", "type": "TEXT", "description": "Customer billing address" },
        { "name": "shipping_address", "type": "TEXT", "description": "Delivery address" },
        {
          "name": "line_items",
          "type": "ARRAY",
          "description": "Individual line items on the purchase order",
          "fields": [
            { "name": "product_sku", "type": "TEXT", "description": "Product SKU or item code" },
            { "name": "product_name", "type": "TEXT", "description": "Product name or description" },
            { "name": "quantity", "type": "INTEGER", "description": "Quantity ordered" },
            { "name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Price per unit" },
            { "name": "line_total", "type": "CURRENCY_AMOUNT", "description": "Total for this line item" }
          ]
        },
        { "name": "currency", "type": "CURRENCY_CODE", "description": "Order currency" },
        { "name": "po_total", "type": "CURRENCY_AMOUNT", "description": "Total order value" }
      ]
    }
  }')

# Step 2: Generate the order confirmation PDF using extracted values
curl -X POST https://api.iterationlayer.com/document-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "pdf",
    "document": {
      "metadata": {
        "title": "Order Confirmation — PO-2026-0847",
        "author": "Meridian Manufacturing Co."
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
      "content": [
        { "type": "headline", "level": "h1", "text": "Meridian Manufacturing Co." },
        { "type": "paragraph", "runs": [{ "text": "1840 Industrial Parkway\nColumbus, OH 43201\norders@meridian-mfg.com" }] },
        { "type": "separator" },
        { "type": "headline", "level": "h2", "text": "Order Confirmation" },
        {
          "type": "paragraph",
          "runs": [
            { "text": "Order confirmed for: ", "font_weight": "bold" },
            { "text": "Northstar Retail Group\n" },
            { "text": "PO Number: ", "font_weight": "bold" },
            { "text": "PO-2026-0847\n" },
            { "text": "PO Date: ", "font_weight": "bold" },
            { "text": "2026-03-18\n" },
            { "text": "Estimated Delivery: ", "font_weight": "bold" },
            { "text": "2026-04-15" }
          ]
        },
        {
          "type": "table",
          "headers": [
            { "text": "SKU" },
            { "text": "Product" },
            { "text": "Qty" },
            { "text": "Unit Price" },
            { "text": "Line Total" }
          ],
          "rows": [
            [
              { "text": "SFT-GL-200" },
              { "text": "Industrial Safety Gloves" },
              { "text": "200" },
              { "text": "$4.50" },
              { "text": "$900.00" }
            ],
            [
              { "text": "SFT-VS-150" },
              { "text": "High-Vis Safety Vests" },
              { "text": "150" },
              { "text": "$12.00" },
              { "text": "$1,800.00" }
            ],
            [
              { "text": "SFT-BT-080" },
              { "text": "Steel-Toe Work Boots" },
              { "text": "80" },
              { "text": "$67.50" },
              { "text": "$5,400.00" }
            ]
          ]
        },
        { "type": "separator" },
        {
          "type": "paragraph",
          "runs": [
            { "text": "Order Total: USD $8,100.00", "font_weight": "bold" }
          ]
        },
        { "type": "paragraph", "runs": [{ "text": "Questions? Contact orders@meridian-mfg.com" }] }
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract purchase order data
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "po-northstar-retail.pdf",
      url: "https://example.com/purchase-orders/po-northstar-retail.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "customer_name",
        type: "TEXT",
        description: "Name of the customer or buying company",
      },
      {
        name: "po_number",
        type: "TEXT",
        description: "Purchase order number",
      },
      {
        name: "po_date",
        type: "DATE",
        description: "Date the PO was issued",
      },
      {
        name: "requested_delivery_date",
        type: "DATE",
        description: "Requested delivery date",
      },
      {
        name: "billing_address",
        type: "TEXT",
        description: "Customer billing address",
      },
      {
        name: "shipping_address",
        type: "TEXT",
        description: "Delivery address",
      },
      {
        name: "line_items",
        type: "ARRAY",
        description: "Individual line items on the purchase order",
        fields: [
          {
            name: "product_sku",
            type: "TEXT",
            description: "Product SKU or item code",
          },
          {
            name: "product_name",
            type: "TEXT",
            description: "Product name or description",
          },
          {
            name: "quantity",
            type: "INTEGER",
            description: "Quantity ordered",
          },
          {
            name: "unit_price",
            type: "CURRENCY_AMOUNT",
            description: "Price per unit",
          },
          {
            name: "line_total",
            type: "CURRENCY_AMOUNT",
            description: "Total for this line item",
          },
        ],
      },
      {
        name: "currency",
        type: "CURRENCY_CODE",
        description: "Order currency",
      },
      {
        name: "po_total",
        type: "CURRENCY_AMOUNT",
        description: "Total order value",
      },
    ],
  },
});

const customerName = String(extraction["customer_name"].value);
const poNumber = String(extraction["po_number"].value);
const poDate = String(extraction["po_date"].value);
const requestedDeliveryDate = String(
  extraction["requested_delivery_date"].value,
);
const currency = String(extraction["currency"].value);
const poTotal = String(extraction["po_total"].value);
const lineItems = extraction["line_items"].value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate the order confirmation PDF using extracted values
const confirmation = await client.generateDocument({
  format: "pdf",
  document: {
    metadata: {
      title: `Order Confirmation — ${poNumber}`,
      author: "Meridian Manufacturing Co.",
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
    content: [
      { type: "headline", level: "h1", text: "Meridian Manufacturing Co." },
      {
        type: "paragraph",
        runs: [
          {
            text: "1840 Industrial Parkway\nColumbus, OH 43201\norders@meridian-mfg.com",
          },
        ],
      },
      { type: "separator" },
      { type: "headline", level: "h2", text: "Order Confirmation" },
      {
        type: "paragraph",
        runs: [
          { text: "Order confirmed for: ", font_weight: "bold" },
          { text: `${customerName}\n` },
          { text: "PO Number: ", font_weight: "bold" },
          { text: `${poNumber}\n` },
          { text: "PO Date: ", font_weight: "bold" },
          { text: `${poDate}\n` },
          { text: "Estimated Delivery: ", font_weight: "bold" },
          { text: requestedDeliveryDate },
        ],
      },
      {
        type: "table",
        headers: [
          { text: "SKU" },
          { text: "Product" },
          { text: "Qty" },
          { text: "Unit Price" },
          { text: "Line Total" },
        ],
        rows: lineItems.map((item) => [
          { text: String(item["product_sku"].value) },
          { text: String(item["product_name"].value) },
          { text: String(item["quantity"].value) },
          { text: String(item["unit_price"].value) },
          { text: String(item["line_total"].value) },
        ]),
      },
      { type: "separator" },
      {
        type: "paragraph",
        runs: [
          { text: `Order Total: ${currency} ${poTotal}`, font_weight: "bold" },
        ],
      },
      {
        type: "paragraph",
        runs: [{ text: "Questions? Contact orders@meridian-mfg.com" }],
      },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract purchase order data
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "po-northstar-retail.pdf",
            "url": "https://example.com/purchase-orders/po-northstar-retail.pdf",
        }
    ],
    schema={
        "fields": [
            {"name": "customer_name", "type": "TEXT", "description": "Name of the customer or buying company"},
            {"name": "po_number", "type": "TEXT", "description": "Purchase order number"},
            {"name": "po_date", "type": "DATE", "description": "Date the PO was issued"},
            {"name": "requested_delivery_date", "type": "DATE", "description": "Requested delivery date"},
            {"name": "billing_address", "type": "TEXT", "description": "Customer billing address"},
            {"name": "shipping_address", "type": "TEXT", "description": "Delivery address"},
            {
                "name": "line_items",
                "type": "ARRAY",
                "description": "Individual line items on the purchase order",
                "fields": [
                    {"name": "product_sku", "type": "TEXT", "description": "Product SKU or item code"},
                    {"name": "product_name", "type": "TEXT", "description": "Product name or description"},
                    {"name": "quantity", "type": "INTEGER", "description": "Quantity ordered"},
                    {"name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Price per unit"},
                    {"name": "line_total", "type": "CURRENCY_AMOUNT", "description": "Total for this line item"},
                ],
            },
            {"name": "currency", "type": "CURRENCY_CODE", "description": "Order currency"},
            {"name": "po_total", "type": "CURRENCY_AMOUNT", "description": "Total order value"},
        ]
    },
)

customer_name = extraction["customer_name"]["value"]
po_number = extraction["po_number"]["value"]
po_date = extraction["po_date"]["value"]
requested_delivery_date = extraction["requested_delivery_date"]["value"]
currency = extraction["currency"]["value"]
po_total = extraction["po_total"]["value"]
line_items = extraction["line_items"]["value"]

# Step 2: Generate the order confirmation PDF using extracted values
confirmation = client.generate_document(
    format="pdf",
    document={
        "metadata": {
            "title": f"Order Confirmation — {po_number}",
            "author": "Meridian Manufacturing Co.",
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
        "content": [
            {"type": "headline", "level": "h1", "text": "Meridian Manufacturing Co."},
            {"type": "paragraph", "runs": [{"text": "1840 Industrial Parkway\nColumbus, OH 43201\norders@meridian-mfg.com"}]},
            {"type": "separator"},
            {"type": "headline", "level": "h2", "text": "Order Confirmation"},
            {
                "type": "paragraph",
                "runs": [
                    {"text": "Order confirmed for: ", "font_weight": "bold"},
                    {"text": f"{customer_name}\n"},
                    {"text": "PO Number: ", "font_weight": "bold"},
                    {"text": f"{po_number}\n"},
                    {"text": "PO Date: ", "font_weight": "bold"},
                    {"text": f"{po_date}\n"},
                    {"text": "Estimated Delivery: ", "font_weight": "bold"},
                    {"text": requested_delivery_date},
                ],
            },
            {
                "type": "table",
                "headers": [
                    {"text": "SKU"},
                    {"text": "Product"},
                    {"text": "Qty"},
                    {"text": "Unit Price"},
                    {"text": "Line Total"},
                ],
                "rows": [
                    [
                        {"text": item["product_sku"]["value"]},
                        {"text": item["product_name"]["value"]},
                        {"text": str(item["quantity"]["value"])},
                        {"text": str(item["unit_price"]["value"])},
                        {"text": str(item["line_total"]["value"])},
                    ]
                    for item in line_items
                ],
            },
            {"type": "separator"},
            {
                "type": "paragraph",
                "runs": [{"text": f"Order Total: {currency} {po_total}", "font_weight": "bold"}],
            },
            {"type": "paragraph", "runs": [{"text": "Questions? Contact orders@meridian-mfg.com"}]},
        ],
    },
)
```

```go
package main

import (
	"fmt"

	il "github.com/iterationlayer/sdk-go"
)

func main() {
	client := il.NewClient("YOUR_API_KEY")

	// Step 1: Extract purchase order data
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("po-northstar-retail.pdf", "https://example.com/purchase-orders/po-northstar-retail.pdf"),
		},
		Schema: il.ExtractionSchema{
			"customer_name":           il.NewTextFieldConfig("customer_name", "Name of the customer or buying company"),
			"po_number":               il.NewTextFieldConfig("po_number", "Purchase order number"),
			"po_date":                 il.NewDateFieldConfig("po_date", "Date the PO was issued"),
			"requested_delivery_date": il.NewDateFieldConfig("requested_delivery_date", "Requested delivery date"),
			"billing_address":         il.NewTextFieldConfig("billing_address", "Customer billing address"),
			"shipping_address":        il.NewTextFieldConfig("shipping_address", "Delivery address"),
			"line_items": il.NewArrayFieldConfig(
				"line_items",
				"Individual line items on the purchase order",
				[]il.FieldConfig{
					il.NewTextFieldConfig("product_sku", "Product SKU or item code"),
					il.NewTextFieldConfig("product_name", "Product name or description"),
					il.NewIntegerFieldConfig("quantity", "Quantity ordered"),
					il.NewCurrencyAmountFieldConfig("unit_price", "Price per unit"),
					il.NewCurrencyAmountFieldConfig("line_total", "Total for this line item"),
				},
			),
			"currency": il.NewCurrencyCodeFieldConfig("currency", "Order currency"),
			"po_total": il.NewCurrencyAmountFieldConfig("po_total", "Total order value"),
		},
	})
	if err != nil {
		panic(err)
	}

	customerName := fmt.Sprintf("%v", (*extraction)["customer_name"].Value)
	poNumber := fmt.Sprintf("%v", (*extraction)["po_number"].Value)
	poDate := fmt.Sprintf("%v", (*extraction)["po_date"].Value)
	requestedDeliveryDate := fmt.Sprintf("%v", (*extraction)["requested_delivery_date"].Value)
	currency := fmt.Sprintf("%v", (*extraction)["currency"].Value)
	poTotal := fmt.Sprintf("%v", (*extraction)["po_total"].Value)

	// Step 2: Generate the order confirmation PDF using extracted values
	_, err = client.GenerateDocument(il.GenerateDocumentRequest{
		Format: "pdf",
		Document: il.DocumentDefinition{
			Metadata: il.DocumentMetadata{
				Title:  "Order Confirmation — " + poNumber,
				Author: "Meridian Manufacturing Co.",
			},
			Page: il.DocumentPage{
				Size:    il.DocPageSize{Preset: "A4"},
				Margins: il.DocMargins{TopInPt: 54, RightInPt: 54, BottomInPt: 54, LeftInPt: 54},
			},
			Content: []il.ContentBlock{
				il.ContentBlock{"type": "headline", "level": "h1", "text": "Meridian Manufacturing Co."},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "1840 Industrial Parkway\nColumbus, OH 43201\norders@meridian-mfg.com"}}},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{"type": "headline", "level": "h2", "text": "Order Confirmation"},
				il.ContentBlock{
					"type": "paragraph",
					"runs": []map[string]string{
						{"text": "Order confirmed for: ", "font_weight": "bold"},
						{"text": customerName + "\n"},
						{"text": "PO Number: ", "font_weight": "bold"},
						{"text": poNumber + "\n"},
						{"text": "PO Date: ", "font_weight": "bold"},
						{"text": poDate + "\n"},
						{"text": "Estimated Delivery: ", "font_weight": "bold"},
						{"text": requestedDeliveryDate},
					},
				},
				il.ContentBlock{
					"type": "table",
					"headers": []map[string]string{
						{"text": "SKU"},
						{"text": "Product"},
						{"text": "Qty"},
						{"text": "Unit Price"},
						{"text": "Line Total"},
					},
					"rows": [][]map[string]string{
						{{"text": "SFT-GL-200"}, {"text": "Industrial Safety Gloves"}, {"text": "200"}, {"text": "$4.50"}, {"text": "$900.00"}},
						{{"text": "SFT-VS-150"}, {"text": "High-Vis Safety Vests"}, {"text": "150"}, {"text": "$12.00"}, {"text": "$1,800.00"}},
						{{"text": "SFT-BT-080"}, {"text": "Steel-Toe Work Boots"}, {"text": "80"}, {"text": "$67.50"}, {"text": "$5,400.00"}},
					},
				},
				il.ContentBlock{"type": "separator"},
				il.ContentBlock{
					"type": "paragraph",
					"runs": []map[string]string{{"text": "Order Total: " + currency + " " + poTotal, "font_weight": "bold"}},
				},
				il.ContentBlock{"type": "paragraph", "runs": []map[string]string{{"text": "Questions? Contact orders@meridian-mfg.com"}}},
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
  "name": "Extract a Purchase Order and Generate a Confirmation",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract a Purchase Order and Generate a Confirmation\n\nB2B suppliers and manufacturers use this pipeline to automatically process inbound purchase orders and return a formatted confirmation PDF \u2014 acknowledging line items, delivery date, and total \u2014 without manual re-entry.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "acc63bdf-6459-43ef-8513-a8937f1b0a1d",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Purchase Order\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "b2954aa1-a811-433a-946f-3acefda1515b",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Order Confirmation\nResource: **Document Generation**\n\nConfigure the Document Generation parameters below, then connect your credentials.",
        "height": 160,
        "width": 300,
        "color": 6
      },
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        725,
        100
      ],
      "id": "8f523f93-f85d-47d8-a62e-395d8096e36d",
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
      "id": "a0b1c2d3-e4f5-6789-0abc-def012345678",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"customer_name\",\n    \"type\": \"TEXT\",\n    \"description\": \"Name of the customer or buying company\"\n  },\n  {\n    \"name\": \"po_number\",\n    \"type\": \"TEXT\",\n    \"description\": \"Purchase order number\"\n  },\n  {\n    \"name\": \"po_date\",\n    \"type\": \"DATE\",\n    \"description\": \"Date the PO was issued\"\n  },\n  {\n    \"name\": \"requested_delivery_date\",\n    \"type\": \"DATE\",\n    \"description\": \"Requested delivery date\"\n  },\n  {\n    \"name\": \"billing_address\",\n    \"type\": \"TEXT\",\n    \"description\": \"Customer billing address\"\n  },\n  {\n    \"name\": \"shipping_address\",\n    \"type\": \"TEXT\",\n    \"description\": \"Delivery address\"\n  },\n  {\n    \"name\": \"line_items\",\n    \"type\": \"ARRAY\",\n    \"description\": \"Individual line items on the purchase order\",\n    \"children\": [\n      {\n        \"name\": \"product_sku\",\n        \"type\": \"TEXT\",\n        \"description\": \"Product SKU or item code\"\n      },\n      {\n        \"name\": \"product_name\",\n        \"type\": \"TEXT\",\n        \"description\": \"Product name or description\"\n      },\n      {\n        \"name\": \"quantity\",\n        \"type\": \"INTEGER\",\n        \"description\": \"Quantity ordered\"\n      },\n      {\n        \"name\": \"unit_price\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Price per unit\"\n      },\n      {\n        \"name\": \"line_total\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Total for this line item\"\n      }\n    ]\n  },\n  {\n    \"name\": \"currency\",\n    \"type\": \"CURRENCY_CODE\",\n    \"description\": \"Order currency\"\n  },\n  {\n    \"name\": \"po_total\",\n    \"type\": \"CURRENCY_AMOUNT\",\n    \"description\": \"Total order value\"\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/purchase-orders/po-northstar-retail.pdf"
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
      "id": "b1c2d3e4-f5a6-7890-1bcd-ef0123456789",
      "name": "Extract Purchase Order",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "documentGeneration",
        "format": "pdf",
        "documentJson": "{\n  \"metadata\": {\n    \"title\": \"Order Confirmation \u2014 {{ $json.po_number }}\",\n    \"author\": \"Meridian Manufacturing Co.\"\n  },\n  \"page\": {\n    \"size\": { \"preset\": \"A4\" },\n    \"margins\": {\n      \"top_in_pt\": 54,\n      \"right_in_pt\": 54,\n      \"bottom_in_pt\": 54,\n      \"left_in_pt\": 54\n    }\n  },\n  \"content\": [\n    { \"type\": \"headline\", \"level\": \"h1\", \"text\": \"Meridian Manufacturing Co.\" },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"1840 Industrial Parkway\\nColumbus, OH 43201\\norders@meridian-mfg.com\" }] },\n    { \"type\": \"separator\" },\n    { \"type\": \"headline\", \"level\": \"h2\", \"text\": \"Order Confirmation\" },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [\n        { \"text\": \"Order confirmed for: \", \"font_weight\": \"bold\" },\n        { \"text\": \"{{ $json.customer_name }}\\n\" },\n        { \"text\": \"PO Number: \", \"font_weight\": \"bold\" },\n        { \"text\": \"{{ $json.po_number }}\\n\" },\n        { \"text\": \"PO Date: \", \"font_weight\": \"bold\" },\n        { \"text\": \"{{ $json.po_date }}\\n\" },\n        { \"text\": \"Estimated Delivery: \", \"font_weight\": \"bold\" },\n        { \"text\": \"{{ $json.requested_delivery_date }}\" }\n      ]\n    },\n    {\n      \"type\": \"table\",\n      \"headers\": [\n        { \"text\": \"SKU\" },\n        { \"text\": \"Product\" },\n        { \"text\": \"Qty\" },\n        { \"text\": \"Unit Price\" },\n        { \"text\": \"Line Total\" }\n      ],\n      \"rows\": \"{{ $json.line_items }}\"\n    },\n    { \"type\": \"separator\" },\n    {\n      \"type\": \"paragraph\",\n      \"runs\": [{ \"text\": \"Order Total: {{ $json.currency }} {{ $json.po_total }}\", \"font_weight\": \"bold\" }]\n    },\n    { \"type\": \"paragraph\", \"runs\": [{ \"text\": \"Questions? Contact orders@meridian-mfg.com\" }] }\n  ]\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "c2d3e4f5-a6b7-8901-2cde-f01234567890",
      "name": "Generate Order Confirmation",
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
            "node": "Extract Purchase Order",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Purchase Order": {
      "main": [
        [
          {
            "node": "Generate Order Confirmation",
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
Extract purchase order data from the file at [file URL], then generate an order confirmation PDF. Use the extract_document tool with these fields:

- customer_name (TEXT): Name of the customer or buying company
- po_number (TEXT): Purchase order number
- po_date (DATE): Date the PO was issued
- requested_delivery_date (DATE): Requested delivery date
- billing_address (TEXT): Customer billing address
- shipping_address (TEXT): Delivery address
- line_items (ARRAY): Each with product_sku (TEXT), product_name (TEXT), quantity (INTEGER), unit_price (CURRENCY_AMOUNT), line_total (CURRENCY_AMOUNT)
- currency (CURRENCY_CODE): Order currency
- po_total (CURRENCY_AMOUNT): Total order value

Then use the generate_document tool to create a PDF order confirmation from the extracted values.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "JVBERi0xLjQ...",
    "mime_type": "application/pdf"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
