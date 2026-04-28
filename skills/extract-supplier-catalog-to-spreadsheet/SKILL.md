---
name: extract-supplier-catalog-to-spreadsheet
description: Extract every product from a supplier catalog PDF — SKUs, names, prices, MOQs — and generate a ready-to-import XLSX in two API calls.
---

# Extract Supplier Catalog to Spreadsheet

E-commerce buyers and merchandising teams use this recipe to onboard new suppliers without manual data entry. Drop in a catalog PDF, extract every product into structured rows, and export a clean XLSX ready to import into Shopify, a PIM, or an internal inventory system.

## APIs Used

Document Extraction (1 credit per page), Sheet Generation (2 credits/request)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
# Step 1: Extract products from the supplier catalog
EXTRACTION=$(curl -s -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "catalog-spring-2026.pdf",
        "url": "https://example.com/suppliers/catalog-spring-2026.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "products",
          "type": "ARRAY",
          "description": "One entry per product in the catalog",
          "fields": [
            { "name": "sku", "type": "TEXT", "description": "Product SKU or item code" },
            { "name": "product_name", "type": "TEXT", "description": "Full product name" },
            { "name": "description", "type": "TEXT", "description": "Short product description" },
            { "name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Unit price" },
            { "name": "currency", "type": "CURRENCY_CODE", "description": "Price currency" },
            { "name": "moq", "type": "INTEGER", "description": "Minimum order quantity" },
            { "name": "category", "type": "TEXT", "description": "Product category" }
          ]
        }
      ]
    }
  }')

# Step 2: Generate the product import spreadsheet
curl -X POST https://api.iterationlayer.com/sheet-generation/v1/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "format": "xlsx",
    "styles": {
      "header": {
        "font_family": "Helvetica",
        "font_size_in_pt": 11,
        "is_bold": true,
        "background_color": "#0F4C81",
        "font_color": "#FFFFFF"
      },
      "body": {
        "font_family": "Helvetica",
        "font_size_in_pt": 11,
        "font_color": "#1A1A1A"
      }
    },
    "sheets": [
      {
      "name": "Product Import — Spring 2026",
      "columns": [
        { "name": "SKU", "width": 14 },
        { "name": "Product Name", "width": 32 },
        { "name": "Description", "width": 40 },
        { "name": "Category", "width": 18 },
        { "name": "Unit Price", "width": 14 },
        { "name": "Currency", "width": 10 },
        { "name": "MOQ", "width": 10 }
      ],
      "rows": [
        [
          { "value": "HG-MUG-CRM-12" },
          { "value": "Ceramic Mug — Cream White (350ml)" },
          { "value": "Glazed stoneware mug with matte exterior and gloss interior" },
          { "value": "Tableware" },
          { "value": 4.80, "format": "currency" },
          { "value": "USD" },
          { "value": 12, "format": "number" }
        ],
        [
          { "value": "HG-CUS-LIN-24" },
          { "value": "Linen Cushion Cover — Natural (50x50cm)" },
          { "value": "Pre-washed linen cover with invisible zip, natural undyed finish" },
          { "value": "Textiles" },
          { "value": 6.50, "format": "currency" },
          { "value": "USD" },
          { "value": 24, "format": "number" }
        ],
        [
          { "value": "HG-SRV-WD-06" },
          { "value": "Acacia Serving Board (38x22cm)" },
          { "value": "Hand-finished acacia wood board with juice groove and rope handle" },
          { "value": "Kitchen" },
          { "value": 9.20, "format": "currency" },
          { "value": "USD" },
          { "value": 6, "format": "number" }
        ],
        [
          { "value": "HG-CND-SOY-12" },
          { "value": "Soy Candle — Amber & Sandalwood (200g)" },
          { "value": "100% soy wax, cotton wick, 40-hour burn time, reusable glass jar" },
          { "value": "Home Fragrance" },
          { "value": 7.40, "format": "currency" },
          { "value": "USD" },
          { "value": 12, "format": "number" }
        ],
        [
          { "value": "HG-CAR-GLS-06" },
          { "value": "Borosilicate Glass Carafe (1L)" },
          { "value": "Heat-resistant borosilicate glass with beechwood stopper, dishwasher safe" },
          { "value": "Glassware" },
          { "value": 11.90, "format": "currency" },
          { "value": "USD" },
          { "value": 6, "format": "number" }
        ]
      ]
    }
    ]
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

// Step 1: Extract products from the supplier catalog
const extraction = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "catalog-spring-2026.pdf",
      url: "https://example.com/suppliers/catalog-spring-2026.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "products",
        type: "ARRAY",
        description: "One entry per product in the catalog",
        fields: [
          {
            name: "sku",
            type: "TEXT",
            description: "Product SKU or item code",
          },
          {
            name: "product_name",
            type: "TEXT",
            description: "Full product name",
          },
          {
            name: "description",
            type: "TEXT",
            description: "Short product description",
          },
          {
            name: "unit_price",
            type: "CURRENCY_AMOUNT",
            description: "Unit price",
          },
          {
            name: "currency",
            type: "CURRENCY_CODE",
            description: "Price currency",
          },
          {
            name: "moq",
            type: "INTEGER",
            description: "Minimum order quantity",
          },
          { name: "category", type: "TEXT", description: "Product category" },
        ],
      },
    ],
  },
});

const products = extraction["products"].value as Array<
  Record<string, { value: unknown }>
>;

// Step 2: Generate the product import spreadsheet
const sheet = await client.generateSheet({
  format: "xlsx",
  styles: {
    header: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      is_bold: true,
      background_color: "#0F4C81",
      font_color: "#FFFFFF",
    },
    body: {
      font_family: "Helvetica",
      font_size_in_pt: 11,
      font_color: "#1A1A1A",
    },
  },
  sheets: [
    {
      name: "Product Import — Spring 2026",
      columns: [
        { name: "SKU", width: 14 },
        { name: "Product Name", width: 32 },
        { name: "Description", width: 40 },
        { name: "Category", width: 18 },
        { name: "Unit Price", width: 14 },
        { name: "Currency", width: 10 },
        { name: "MOQ", width: 10 },
      ],
      rows: products.map((product) => [
        { value: product["sku"].value },
        { value: product["product_name"].value },
        { value: product["description"].value },
        { value: product["category"].value },
        {
          value: (product["unit_price"].value as { amount: string }).amount,
          format: "currency",
        },
        { value: product["currency"].value },
        { value: product["moq"].value, format: "number" },
      ]),
    },
  ],
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

# Step 1: Extract products from the supplier catalog
extraction = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "catalog-spring-2026.pdf",
            "url": "https://example.com/suppliers/catalog-spring-2026.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "products",
                "type": "ARRAY",
                "description": "One entry per product in the catalog",
                "fields": [
                    {"name": "sku", "type": "TEXT", "description": "Product SKU or item code"},
                    {"name": "product_name", "type": "TEXT", "description": "Full product name"},
                    {"name": "description", "type": "TEXT", "description": "Short product description"},
                    {"name": "unit_price", "type": "CURRENCY_AMOUNT", "description": "Unit price"},
                    {"name": "currency", "type": "CURRENCY_CODE", "description": "Price currency"},
                    {"name": "moq", "type": "INTEGER", "description": "Minimum order quantity"},
                    {"name": "category", "type": "TEXT", "description": "Product category"},
                ],
            }
        ]
    },
)

products = extraction["products"]["value"]

# Step 2: Generate the product import spreadsheet
sheet = client.generate_sheet(
    format="xlsx",
    styles={
        "header": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "is_bold": True,
            "background_color": "#0F4C81",
            "font_color": "#FFFFFF",
        },
        "body": {
            "font_family": "Helvetica",
            "font_size_in_pt": 11,
            "font_color": "#1A1A1A",
        },
    },
    sheets=[
        {
            "name": "Product Import — Spring 2026",
            "columns": [
                {"name": "SKU", "width": 14},
                {"name": "Product Name", "width": 32},
                {"name": "Description", "width": 40},
                {"name": "Category", "width": 18},
                {"name": "Unit Price", "width": 14},
                {"name": "Currency", "width": 10},
                {"name": "MOQ", "width": 10},
            ],
            "rows": [
                [
                    {"value": product["sku"]["value"]},
                    {"value": product["product_name"]["value"]},
                    {"value": product["description"]["value"]},
                    {"value": product["category"]["value"]},
                    {"value": product["unit_price"]["value"]["amount"], "format": "currency"},
                    {"value": product["currency"]["value"]},
                    {"value": product["moq"]["value"], "format": "number"},
                ]
                for product in products
            ],
        }
    ],
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

	// Step 1: Extract products from the supplier catalog
	extraction, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL(
				"catalog-spring-2026.pdf",
				"https://example.com/suppliers/catalog-spring-2026.pdf",
			),
		},
		Schema: il.ExtractionSchema{
			"products": il.NewArrayFieldConfig(
				"products",
				"One entry per product in the catalog",
				[]il.FieldConfig{
					il.NewTextFieldConfig("sku", "Product SKU or item code"),
					il.NewTextFieldConfig("product_name", "Full product name"),
					il.NewTextFieldConfig("description", "Short product description"),
					il.NewCurrencyAmountFieldConfig("unit_price", "Unit price"),
					il.NewCurrencyCodeFieldConfig("currency", "Price currency"),
					il.NewIntegerFieldConfig("moq", "Minimum order quantity"),
					il.NewTextFieldConfig("category", "Product category"),
				},
			),
		},
	})
	if err != nil {
		panic(err)
	}

	rawProducts, _ := (*extraction)["products"].Value.([]interface{})

	sheetRows := make([]il.SheetRow, 0, len(rawProducts))
	for _, rawProduct := range rawProducts {
		product, _ := rawProduct.(map[string]interface{})
		getSV := func(key string) string {
			if field, ok := product[key].(map[string]interface{}); ok {
				return fmt.Sprintf("%v", field["value"])
			}
			return ""
		}
		unitPrice := ""
		if field, ok := product["unit_price"].(map[string]interface{}); ok {
			if amountMap, ok := field["value"].(map[string]interface{}); ok {
				unitPrice = fmt.Sprintf("%v", amountMap["amount"])
			}
		}
		sheetRows = append(sheetRows, il.SheetRow{
			{Value: getSV("sku")},
			{Value: getSV("product_name")},
			{Value: getSV("description")},
			{Value: getSV("category")},
			{Value: unitPrice, Format: "currency"},
			{Value: getSV("currency")},
			{Value: getSV("moq"), Format: "number"},
		})
	}

	// Step 2: Generate the product import spreadsheet
	result, err := client.GenerateSheet(il.GenerateSheetRequest{
		Format: "xlsx",
		Styles: &il.SheetStyles{
			Header: &il.CellStyle{
				FontFamily:      "Helvetica",
				FontSizeInPt:    11,
				IsBold:          true,
				BackgroundColor: "#0F4C81",
				FontColor:       "#FFFFFF",
			},
			Body: &il.CellStyle{
				FontFamily:   "Helvetica",
				FontSizeInPt: 11,
				FontColor:    "#1A1A1A",
			},
		},
		Sheets: []il.Sheet{
			{
				Name: "Product Import — Spring 2026",
				Columns: []il.SheetColumn{
					{Name: "SKU", Width: 14},
					{Name: "Product Name", Width: 32},
					{Name: "Description", Width: 40},
					{Name: "Category", Width: 18},
					{Name: "Unit Price", Width: 14},
					{Name: "Currency", Width: 10},
					{Name: "MOQ", Width: 10},
				},
				Rows: sheetRows,
			},
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
  "name": "Extract Supplier Catalog to Spreadsheet",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Supplier Catalog to Spreadsheet\n\nE-commerce buyers and merchandising teams use this recipe to onboard new suppliers without manual data entry. Drop in a catalog PDF, extract every product into structured rows, and export a clean XLSX ready to import into Shopify, a PIM, or an internal inventory system.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "8c2f7dc4-ce1b-44ef-bd76-70126efdfc59",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Product Catalog\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "df465266-3879-48da-8da6-86e539e9cb5e",
      "name": "Step 1 Note"
    },
    {
      "parameters": {
        "content": "### Step 2: Generate Product Import Sheet\nResource: **Sheet Generation**\n\nConfigure the Sheet Generation parameters below, then connect your credentials.",
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
      "id": "d82b8787-116a-4f22-86ed-3a7298886e56",
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
      "id": "d9e0f1a2-b3c4-5678-9d34-678901234567",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "[\n  {\n    \"name\": \"products\",\n    \"type\": \"ARRAY\",\n    \"description\": \"One entry per product in the catalog\",\n    \"children\": [\n      {\n        \"name\": \"sku\",\n        \"type\": \"TEXT\",\n        \"description\": \"Product SKU or item code\"\n      },\n      {\n        \"name\": \"product_name\",\n        \"type\": \"TEXT\",\n        \"description\": \"Full product name\"\n      },\n      {\n        \"name\": \"description\",\n        \"type\": \"TEXT\",\n        \"description\": \"Short product description\"\n      },\n      {\n        \"name\": \"unit_price\",\n        \"type\": \"CURRENCY_AMOUNT\",\n        \"description\": \"Unit price\"\n      },\n      {\n        \"name\": \"currency\",\n        \"type\": \"CURRENCY_CODE\",\n        \"description\": \"Price currency\"\n      },\n      {\n        \"name\": \"moq\",\n        \"type\": \"INTEGER\",\n        \"description\": \"Minimum order quantity\"\n      },\n      {\n        \"name\": \"category\",\n        \"type\": \"TEXT\",\n        \"description\": \"Product category\"\n      }\n    ]\n  }\n]",
        "files": {
          "fileValues": [
            {
              "inputMode": "url",
              "fileUrl": "https://example.com/suppliers/catalog-spring-2026.pdf"
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
      "id": "e0f1a2b3-c4d5-6789-0e45-789012345678",
      "name": "Extract Product Catalog",
      "credentials": {
        "iterationLayerApi": {
          "id": "1",
          "name": "Iteration Layer API"
        }
      }
    },
    {
      "parameters": {
        "resource": "sheetGeneration",
        "sheetFormat": "xlsx",
        "sheetsJson": "[\n  {\n    \"name\": \"Product Import \u2014 Spring 2026\",\n    \"columns\": [\n      { \"name\": \"SKU\", \"width\": 14 },\n      { \"name\": \"Product Name\", \"width\": 32 },\n      { \"name\": \"Description\", \"width\": 40 },\n      { \"name\": \"Category\", \"width\": 18 },\n      { \"name\": \"Unit Price\", \"width\": 14 },\n      { \"name\": \"Currency\", \"width\": 10 },\n      { \"name\": \"MOQ\", \"width\": 10 }\n    ],\n    \"rows\": \"{{ $json.products }}\"\n  }\n]",
        "sheetStylesJson": "{\n  \"header\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"is_bold\": true,\n    \"background_color\": \"#0F4C81\",\n    \"font_color\": \"#FFFFFF\"\n  },\n  \"body\": {\n    \"font_family\": \"Helvetica\",\n    \"font_size_in_pt\": 11,\n    \"font_color\": \"#1A1A1A\"\n  }\n}"
      },
      "type": "n8n-nodes-iterationlayer.iterationLayer",
      "typeVersion": 1,
      "position": [
        750,
        300
      ],
      "id": "f1a2b3c4-d5e6-7890-1f56-890123456789",
      "name": "Generate Product Import Sheet",
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
            "node": "Extract Product Catalog",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Extract Product Catalog": {
      "main": [
        [
          {
            "node": "Generate Product Import Sheet",
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
Extract products from the supplier catalog at [file URL], then generate a product import spreadsheet. Use the extract_document tool with these fields:

- products (ARRAY): One entry per product in the catalog, each with:
  - sku (TEXT): Product SKU or item code
  - product_name (TEXT): Full product name
  - description (TEXT): Short product description
  - unit_price (CURRENCY_AMOUNT): Unit price
  - currency (CURRENCY_CODE): Price currency
  - moq (INTEGER): Minimum order quantity
  - category (TEXT): Product category

Then use the generate_sheet tool to create an XLSX product import spreadsheet with the extracted rows.
```

### Response


```json
{
  "success": true,
  "data": {
    "buffer": "UEsDBBQAAAAIAA...",
    "mime_type": "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
