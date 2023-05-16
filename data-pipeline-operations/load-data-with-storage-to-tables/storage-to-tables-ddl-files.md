---
description: >-
  This is the description of the DDL files used for a Storage to Tables data
  operation.
---

# Storage to Tables DDL files

You need to provide one DDL file for each database table that will be created. Each DDL file should be associated with a Markdown file for documentation purposes.

## 👁️‍🗨️ Example

```json
{
  "schema": [
    {
      "name": "COMPANYCODE",
      "type": "STRING",
      "description": "Company code."
    },
    {
      "name": "STOREID",
      "type": "STRING",
      "description": "ID of the store. Foreign key to stores table."
    },
    {
      "name": "STOREDESC",
      "type": "STRING",
      "description": "Store name."
    },
    {
      "name": "STORETYPE",
      "type": "STRING",
      "description": "Store type."
    }
  ]
}
```

## :gear: Parameters

| Parameter                                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>schema</strong></p><p>type: array</p><p>mandatory</p> | <p>GBQ table schema. It contains a list of fields corresponding to the number of columns found in the CSV.</p><p>Each field described has three attributes:</p><ul><li><strong>name</strong>: Name of the field. Keep column names as in the input file.</li><li><strong>type</strong>: Use only the STRING type at this stage to avoid cast errors. The type can modified with a transformation operation later on.</li><li><strong>description</strong>: Text describing the meaning of the data.</li></ul> |
