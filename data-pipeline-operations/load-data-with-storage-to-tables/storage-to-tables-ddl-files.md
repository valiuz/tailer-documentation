---
description: >-
  This is the description of the DDL files used for a Storage to Tables data
  operation.
---

# Storage to Tables DDL files

You need to provide one DDL file for each database table that will be created. Each DDL file should be associated with a Markdown file for documentation purposes.

## 👁️‍🗨️ Example

```text
{
  "schema": [
    {
      "name": "COMPANYCODE",
      "type": "STRING",
      "description": "Company code: mostly Orsay."
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

## ⚙ Parameters

<table>
  <thead>
    <tr>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>schema</b>
        </p>
        <p>type: array</p>
        <p>mandatory</p>
      </td>
      <td style="text-align:left">
        <p></p>
        <p>GBQ table schema. It contains a list of fields corresponding to the number
          of columns found in the CSV.</p>
        <p>Each field described has three attributes:</p>
        <ul>
          <li><b>name</b>: Name of the field. Keep column names as in the input file.</li>
          <li><b>type</b>: Use only the STRING type at this stage to avoid cast errors.
            The type can modified with a transformation operation later on.</li>
          <li><b>description</b>: Text describing the meaning of the data.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

