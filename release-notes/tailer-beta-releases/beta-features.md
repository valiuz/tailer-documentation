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

To test this new feature, please add the following attribute to any Storage To Table configuration and deploy it:

```json
"beta_mode": "20230227_STT_with_metadata"
```
