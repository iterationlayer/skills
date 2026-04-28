---
name: extract-property-appraisal
description: Extract appraised value, property details, and comparable sales from a property appraisal report into structured JSON.
---

# Extract Property Appraisal

Mortgage lenders, real estate platforms, and appraisal management companies use this recipe to digitize appraisal reports. Upload an appraisal PDF and receive structured JSON with the appraised value, property details, and comparable sales — ready to feed into your loan origination system or valuation database.

## APIs Used

Document Extraction (1 credit per page)

## Prerequisites

You need an Iteration Layer API key. Get one at [platform.iterationlayer.com](https://platform.iterationlayer.com) — free trial credits included, no credit card required.

For full integration guidance (SDKs, auth, MCP, error handling), see the [Iteration Layer Integration Guide](https://iterationlayer.com/SKILL.md).

## Implementation


```bash
curl -X POST https://api.iterationlayer.com/document-extraction/v1/extract \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "files": [
      {
        "type": "url",
        "name": "appraisal.pdf",
        "url": "https://example.com/appraisals/2026-0042.pdf"
      }
    ],
    "schema": {
      "fields": [
        {
          "name": "subject_address",
          "type": "ADDRESS",
          "description": "Address of the subject property being appraised"
        },
        {
          "name": "appraised_value",
          "type": "CURRENCY_AMOUNT",
          "description": "Final appraised market value of the property"
        },
        {
          "name": "effective_date",
          "type": "DATE",
          "description": "Date of the appraisal"
        },
        {
          "name": "property_type",
          "type": "ENUM",
          "description": "Type of property",
          "values": ["single_family", "condo", "multi_family", "commercial", "land"],
          "max_selected": 1
        },
        {
          "name": "gross_living_area_sqft",
          "type": "INTEGER",
          "description": "Gross living area in square feet"
        },
        {
          "name": "year_built",
          "type": "INTEGER",
          "description": "Year the property was built"
        },
        {
          "name": "bedrooms",
          "type": "INTEGER",
          "description": "Number of bedrooms"
        },
        {
          "name": "bathrooms",
          "type": "DECIMAL",
          "description": "Number of bathrooms",
          "decimal_points": 1
        },
        {
          "name": "comparable_sales",
          "type": "ARRAY",
          "description": "Comparable sales used in the appraisal",
          "fields": [
            {
              "name": "address",
              "type": "ADDRESS",
              "description": "Address of the comparable property"
            },
            {
              "name": "sale_price",
              "type": "CURRENCY_AMOUNT",
              "description": "Sale price of the comparable"
            },
            {
              "name": "sale_date",
              "type": "DATE",
              "description": "Date of the comparable sale"
            },
            {
              "name": "gross_living_area_sqft",
              "type": "INTEGER",
              "description": "Gross living area of the comparable in square feet"
            }
          ]
        }
      ]
    }
  }'
```

```typescript
import { IterationLayer } from "iterationlayer";
const client = new IterationLayer({ apiKey: "YOUR_API_KEY" });

const result = await client.extractDocument({
  files: [
    {
      type: "url",
      name: "appraisal.pdf",
      url: "https://example.com/appraisals/2026-0042.pdf",
    },
  ],
  schema: {
    fields: [
      {
        name: "subject_address",
        type: "ADDRESS",
        description: "Address of the subject property being appraised",
      },
      {
        name: "appraised_value",
        type: "CURRENCY_AMOUNT",
        description: "Final appraised market value of the property",
      },
      {
        name: "effective_date",
        type: "DATE",
        description: "Date of the appraisal",
      },
      {
        name: "property_type",
        type: "ENUM",
        description: "Type of property",
        values: [
          "single_family",
          "condo",
          "multi_family",
          "commercial",
          "land",
        ],
        max_selected: 1,
      },
      {
        name: "gross_living_area_sqft",
        type: "INTEGER",
        description: "Gross living area in square feet",
      },
      {
        name: "year_built",
        type: "INTEGER",
        description: "Year the property was built",
      },
      {
        name: "bedrooms",
        type: "INTEGER",
        description: "Number of bedrooms",
      },
      {
        name: "bathrooms",
        type: "DECIMAL",
        description: "Number of bathrooms",
        decimal_points: 1,
      },
      {
        name: "comparable_sales",
        type: "ARRAY",
        description: "Comparable sales used in the appraisal",
        fields: [
          {
            name: "address",
            type: "ADDRESS",
            description: "Address of the comparable property",
          },
          {
            name: "sale_price",
            type: "CURRENCY_AMOUNT",
            description: "Sale price of the comparable",
          },
          {
            name: "sale_date",
            type: "DATE",
            description: "Date of the comparable sale",
          },
          {
            name: "gross_living_area_sqft",
            type: "INTEGER",
            description: "Gross living area of the comparable in square feet",
          },
        ],
      },
    ],
  },
});
```

```python
from iterationlayer import IterationLayer
client = IterationLayer(api_key="YOUR_API_KEY")

result = client.extract_document(
    files=[
        {
            "type": "url",
            "name": "appraisal.pdf",
            "url": "https://example.com/appraisals/2026-0042.pdf",
        }
    ],
    schema={
        "fields": [
            {
                "name": "subject_address",
                "type": "ADDRESS",
                "description": "Address of the subject property being appraised",
            },
            {
                "name": "appraised_value",
                "type": "CURRENCY_AMOUNT",
                "description": "Final appraised market value of the property",
            },
            {
                "name": "effective_date",
                "type": "DATE",
                "description": "Date of the appraisal",
            },
            {
                "name": "property_type",
                "type": "ENUM",
                "description": "Type of property",
                "values": ["single_family", "condo", "multi_family", "commercial", "land"],
                "max_selected": 1,
            },
            {
                "name": "gross_living_area_sqft",
                "type": "INTEGER",
                "description": "Gross living area in square feet",
            },
            {
                "name": "year_built",
                "type": "INTEGER",
                "description": "Year the property was built",
            },
            {
                "name": "bedrooms",
                "type": "INTEGER",
                "description": "Number of bedrooms",
            },
            {
                "name": "bathrooms",
                "type": "DECIMAL",
                "description": "Number of bathrooms",
                "decimal_points": 1,
            },
            {
                "name": "comparable_sales",
                "type": "ARRAY",
                "description": "Comparable sales used in the appraisal",
                "fields": [
                    {
                        "name": "address",
                        "type": "ADDRESS",
                        "description": "Address of the comparable property",
                    },
                    {
                        "name": "sale_price",
                        "type": "CURRENCY_AMOUNT",
                        "description": "Sale price of the comparable",
                    },
                    {
                        "name": "sale_date",
                        "type": "DATE",
                        "description": "Date of the comparable sale",
                    },
                    {
                        "name": "gross_living_area_sqft",
                        "type": "INTEGER",
                        "description": "Gross living area of the comparable in square feet",
                    },
                ],
            },
        ]
    },
)
```

```go
package main

import il "github.com/iterationlayer/sdk-go"

func main() {
	client := il.NewClient("YOUR_API_KEY")

	result, err := client.ExtractDocument(il.ExtractDocumentRequest{
		Files: []il.FileInput{
			il.NewFileFromURL("appraisal.pdf", "https://example.com/appraisals/2026-0042.pdf"),
		},
		Schema: il.ExtractionSchema{
			"subject_address": il.NewAddressFieldConfig(
				"subject_address", "Address of the subject property being appraised",
			),
			"appraised_value": il.NewCurrencyAmountFieldConfig(
				"appraised_value", "Final appraised market value of the property",
			),
			"effective_date": il.NewDateFieldConfig(
				"effective_date", "Date of the appraisal",
			),
			"property_type": il.NewEnumFieldConfig(
				"property_type", "Type of property",
				[]string{"single_family", "condo", "multi_family", "commercial", "land"},
			),
			"gross_living_area_sqft": il.NewIntegerFieldConfig(
				"gross_living_area_sqft", "Gross living area in square feet",
			),
			"year_built": il.NewIntegerFieldConfig(
				"year_built", "Year the property was built",
			),
			"bedrooms": il.NewIntegerFieldConfig(
				"bedrooms", "Number of bedrooms",
			),
			"bathrooms": il.NewDecimalFieldConfig(
				"bathrooms", "Number of bathrooms",
			),
			"comparable_sales": il.NewArrayFieldConfig(
				"comparable_sales", "Comparable sales used in the appraisal",
				[]il.FieldConfig{
					il.NewAddressFieldConfig("address", "Address of the comparable property"),
					il.NewCurrencyAmountFieldConfig("sale_price", "Sale price of the comparable"),
					il.NewDateFieldConfig("sale_date", "Date of the comparable sale"),
					il.NewIntegerFieldConfig("gross_living_area_sqft", "Gross living area of the comparable in square feet"),
				},
			),
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
  "name": "Extract Property Appraisal",
  "nodes": [
    {
      "parameters": {
        "content": "## Extract Property Appraisal\n\nMortgage lenders, real estate platforms, and appraisal management companies use this recipe to digitize appraisal reports. Upload an appraisal PDF and receive structured JSON with the appraised value, property details, and comparable sales \u2014 ready to feed into your loan origination system or valuation database.\n\n**Note:** This workflow uses the Iteration Layer community node (`n8n-nodes-iterationlayer`). Install it via Settings > Community Nodes on self-hosted n8n, or add it directly on n8n Cloud with Verified Community Nodes enabled.",
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
      "id": "18c9143e-4361-4a50-952b-dda14da53749",
      "name": "Overview"
    },
    {
      "parameters": {
        "content": "### Step 1: Extract Data\nResource: **Document Extraction**\n\nConfigure the Document Extraction parameters below, then connect your credentials.",
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
      "id": "163e4817-e901-4199-b290-9f0674a62a3d",
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
      "id": "e7f8a9b0-c1d2-3456-efab-567890123efa",
      "name": "Manual Trigger"
    },
    {
      "parameters": {
        "resource": "documentExtraction",
        "schemaInputMode": "rawJson",
        "schemaJson": "{\"fields\":[{\"name\":\"subject_address\",\"type\":\"ADDRESS\",\"description\":\"Address of the subject property being appraised\"},{\"name\":\"appraised_value\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Final appraised market value of the property\"},{\"name\":\"effective_date\",\"type\":\"DATE\",\"description\":\"Date of the appraisal\"},{\"name\":\"property_type\",\"type\":\"ENUM\",\"description\":\"Type of property\",\"values\":[\"single_family\",\"condo\",\"multi_family\",\"commercial\",\"land\"],\"max_selected\":1},{\"name\":\"gross_living_area_sqft\",\"type\":\"INTEGER\",\"description\":\"Gross living area in square feet\"},{\"name\":\"year_built\",\"type\":\"INTEGER\",\"description\":\"Year the property was built\"},{\"name\":\"bedrooms\",\"type\":\"INTEGER\",\"description\":\"Number of bedrooms\"},{\"name\":\"bathrooms\",\"type\":\"DECIMAL\",\"description\":\"Number of bathrooms\",\"decimal_points\":1},{\"name\":\"comparable_sales\",\"type\":\"ARRAY\",\"description\":\"Comparable sales used in the appraisal\",\"fields\":[{\"name\":\"address\",\"type\":\"ADDRESS\",\"description\":\"Address of the comparable property\"},{\"name\":\"sale_price\",\"type\":\"CURRENCY_AMOUNT\",\"description\":\"Sale price of the comparable\"},{\"name\":\"sale_date\",\"type\":\"DATE\",\"description\":\"Date of the comparable sale\"},{\"name\":\"gross_living_area_sqft\",\"type\":\"INTEGER\",\"description\":\"Gross living area of the comparable in square feet\"}]}]}",
        "files": {
          "fileValues": [
            {
              "fileInputMode": "url",
              "fileName": "appraisal.pdf",
              "fileUrl": "https://example.com/appraisals/2026-0042.pdf"
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
      "id": "f8a9b0c1-d2e3-4567-fabc-678901234fab",
      "name": "Extract Data",
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
            "node": "Extract Data",
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
Extract property appraisal data from the file at [file URL]. Use the extract_document tool with these fields:

- subject_address (ADDRESS): Address of the subject property being appraised
- appraised_value (CURRENCY_AMOUNT): Final appraised market value
- effective_date (DATE): Date of the appraisal
- property_type (ENUM): Type of property (single_family, condo, multi_family, commercial, land)
- gross_living_area_sqft (INTEGER): Gross living area in square feet
- year_built (INTEGER): Year the property was built
- bedrooms (INTEGER): Number of bedrooms
- bathrooms (DECIMAL): Number of bathrooms
- comparable_sales (ARRAY): Each with address (ADDRESS), sale_price (CURRENCY_AMOUNT), sale_date (DATE), gross_living_area_sqft (INTEGER)
```

### Response


```json
{
  "success": true,
  "data": {
    "subject_address": {
      "value": {
        "street": "412 Ridgewood Lane",
        "city": "Denver",
        "region": "CO",
        "postal_code": "80203",
        "country": "US"
      },
      "confidence": 0.98,
      "citations": ["412 Ridgewood Lane, Denver, CO 80203"]
    },
    "appraised_value": {
      "value": { "amount": "685000.00", "currency": "USD" },
      "confidence": 0.97,
      "citations": ["Market Value Opinion: $685,000"]
    },
    "effective_date": {
      "value": "2026-03-15",
      "confidence": 0.99,
      "citations": ["Effective Date: March 15, 2026"]
    },
    "property_type": {
      "value": ["single_family"],
      "confidence": 0.99,
      "citations": ["Single Family Residence"]
    },
    "gross_living_area_sqft": {
      "value": 2140,
      "confidence": 0.97,
      "citations": ["GLA: 2,140 sq ft"]
    },
    "year_built": {
      "value": 1998,
      "confidence": 0.99,
      "citations": ["Year Built: 1998"]
    },
    "bedrooms": {
      "value": 4,
      "confidence": 0.99,
      "citations": ["Bedrooms: 4"]
    },
    "bathrooms": {
      "value": 2.5,
      "confidence": 0.98,
      "citations": ["Bathrooms: 2.5"]
    },
    "comparable_sales": {
      "value": [
        {
          "address": {
            "value": {
              "street": "388 Ridgewood Lane",
              "city": "Denver",
              "region": "CO",
              "postal_code": "80203",
              "country": "US"
            },
            "confidence": 0.96,
            "citations": ["388 Ridgewood Lane, Denver, CO 80203"]
          },
          "sale_price": {
            "value": { "amount": "672000.00", "currency": "USD" },
            "confidence": 0.97,
            "citations": ["Sale Price: $672,000"]
          },
          "sale_date": {
            "value": "2026-01-20",
            "confidence": 0.98,
            "citations": ["Date of Sale: 01/20/2026"]
          },
          "gross_living_area_sqft": {
            "value": 2090,
            "confidence": 0.96,
            "citations": ["GLA: 2,090"]
          }
        }
      ],
      "confidence": 0.96,
      "citations": []
    }
  }
}
```


## Links

- [Integration guide](https://iterationlayer.com/SKILL.md)
- [Full documentation](https://iterationlayer.com/docs)
- [OpenAPI spec](https://api.iterationlayer.com/openapi.json)
- [Browse all recipes](https://iterationlayer.com/recipes)
