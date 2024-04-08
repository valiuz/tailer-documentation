# Beta features

## \[1.3.18] Table To Table : GBQ table delete task

New `delete_gbq_table` task type available.

Task configuration example:

```json
{
    "id": "delete_table_my_table",
    "task_type": "delete_gbq_table",
    "short_description": "Delete GBQ table : my_table",
    "bq_table": "my_table",
    "gcp_project_id": "myGcpProject",
    "bq_dataset": "myDataset",
}

```

Note that the partition decorator '$' will work with this task:

* time partitioned table name: `my_table$20240123`&#x20;
* integer partitioned table name: `my_table$27`

## \[1.3.18] Table To Table : copy table task write disposition mode

You can now specify the BigQuery write disposition in a copy table task.



```json
{
    "id": "copy_some_data",
    "task_type": "copy_gbq_table",
    "source_gcp_project_id": "mySourceGcpProject",
    "source_bq_dataset": "mySourceDataset",
    "source_bq_table": "tmp_stores",
    "destination_bq_table": "stores",
    "write_disposition": "WRITE_APPEND"
} 
```
