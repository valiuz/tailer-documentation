# Beta features

## \[RELEASED] Storage To Table : automatic metadata

Automatic metadata feature will add specific columns during the ingestion process related to the inpput source.

The added columns are:

```python
tlr_ingestion_timestamp_utc (TIMESTAMP)
tlr_input_file_source_type (STRING)
tlr_input_file_name (STRING)
tlr_input_file_full_resource_name (STRING)
```

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

To test this new feature, please add the following attribute **"add\_tailer\_metadata": true** to any Storage To Table configuration and deploy it like this in the tables section :

```json
"tables": [
    {
    "table_name": "sales_details_test",
    "short_description": "Daily sales with tickets and tickets lines",
    "filename_template": "sales_{{FD_DATE}}-{{FD_DATE}}.csv",
    "ddl_mode": "file",
    "ddl_file": "sales_full_ddl.json",
    "add_tailer_metadata": true,
    "skip_leading_rows": 0
    }
]
```
