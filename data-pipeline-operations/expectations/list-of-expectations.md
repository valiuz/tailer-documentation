# List of Expectations

### Fashion Data Expectations

* Key constraints
  * [primarykey\_named](list-of-expectations.md#primarykey\_named-dataset-tablename-column-threshold)
  * [primarykey](list-of-expectations.md#primarykey-dataset-tablename-threshold)
  * [foreignkey](list-of-expectations.md#foreignkey-dataset-tablename-column-target\_dataset-target\_tablename-target\_column-threshold)
* Temporal continuity
  * [everyday\_since](list-of-expectations.md#everyday\_since-dataset-tablename-column-start\_date-exception-minimum)
  * [everyday\_increasing\_since](list-of-expectations.md#everyday\_increasing\_since-dataset-tablename-value)
  * [everyweek\_since](list-of-expectations.md#everyweek\_since-dataset-tablename-column-start\_date-exception-minimum)
  * [everymonth\_since](list-of-expectations.md#everymonth\_since-dataset-tablename-column-start\_date-exception-minimum)
* Row count
  * [table\_count\_greater](list-of-expectations.md#table\_count\_greater-dataset-tablename-value)
  * [table\_count\_between](list-of-expectations.md#table\_count\_between-dataset-tablename-value)
  * [table\_count\_equal](list-of-expectations.md#table\_count\_equal-dataset-tablename-value)
  * [table\_count\_equal\_other\_table](list-of-expectations.md#table\_count\_equal\_other\_table-dataset-tablename-target\_dataset-target\_tablename-threshold)
* Column properties
  * [unique](list-of-expectations.md#unique-dataset-tablename-column)
  * [not\_null](list-of-expectations.md#not\_null-dataset-tablename-column-threshold)
  * [null](list-of-expectations.md#null-dataset-tablename-column-threshold)
  * [type](list-of-expectations.md#type-dataset-tablename-column-type)
  * [values\_to\_contain](list-of-expectations.md#values\_to\_contain-dataset-tablename-column-value-minimum-threshold)
  * [values\_to\_be\_between](list-of-expectations.md#values\_to\_be\_between-dataset-tablename-column-value-threshold)
  * [values\_to\_be\_in\_set](list-of-expectations.md#values\_to\_be\_in\_set-dataset-tablename-column-value-threshold)
  * [values\_to\_not\_be\_in\_set](list-of-expectations.md#values\_to\_not\_be\_in\_set-dataset-tablename-column-value-threshold)

### Key constraints

#### <mark style="color:purple;">primarykey\_named(dataset, tablename, column, threshold)</mark>

```sql
CALL `tailer-ai.expect.primarykey_named`('my-project.my_dataset', 'sales_details', 'PK_sales_details', 0);
CALL `tailer-ai.expect.primarykey_named`('my-project.my_dataset', 'sales_details', 'CONCAT(ticket_id, "-", line_number)', 0.0001);          
```

Expect a column in a table to respect a pseudo Primary Key constraint.

This procedure checks that every value of the specified column is not null and unique within the current table. This is enforced by counting the total number of rows within the table and comparing it to the number of distinct element in the column. A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count represents less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name, or an sql operation that creates a pseudo column that can be inserted into a count distinct
  * **threshold** (_FLOAT64_) – The threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">primarykey(dataset, tablename, threshold)</mark>

```sql
CALL `tailer-ai.expect.primarykey('my-project.my_dataset', 'sales_details', 0);
```

Expect a table to have a column that respects a pseudo Primary Key constraint.

This procedure looks for a column with a name that starts wiht 'PK' or with a desdcription that contains '#PK'. Then it checks that every value of this column is not null and unique within the current table. This is enforced by counting the total number of rows within the table and comparing it to the number of distinct element in the column. A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count represents less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **threshold** (_FLOAT64_) – The threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">foreignkey(dataset, tablename, column, target\_dataset, target\_tablename, target\_column, threshold)</mark>

```sql
CALL `tailer-ai.expect.foreignkey('my-project.my_dataset', 'sales_details', 'customer_id', 'my-project.my_dataset', 'customers', 'customer_id', 0.001);          
```

Expect a column in a table to respect a pseudo Foreign Key constraint.

This procedure checks that every non-null value in the column can be found in the values of the target column of the reference target table. A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count is less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **target\_dataset** (_STRING_) – The target dataset of the foreign table (and its GCP project)
  * **target\_tablename** (_STRING_) – The foreign table name
  * **target\_column** (_STRING_) – The foreign key column of the foreign table
  * **threshold** (_FLOAT64_) – The threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

### Temporal continuity

#### <mark style="color:purple;">everyday\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

```sql
CALL `tailer-ai.expect.everyday_since('my-project.my_dataset', 'sales_details', 'sale_date', DATE_SUB(current_date, interval 31 day), ['2022-01-01', '2021-12-25', cast(current_date as string)], 1000);          
```

Expect a table to have a minimum number of rows per day since a start date. An exception list can be provided to avoid an error when a date has no data for a good reason.

This procedure counts the number of rows of the specified table grouped by date. If a day between the specified start\_date and today is missing, or if a daily count is below minimum, then the test fails, except if the date is specified in the exception list.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY\<DATE>_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">everyday\_increasing\_since(dataset, tablename, value)</mark>

```sql
CALL `tailer-ai.expect.everyday_increasing_since`('my-gcp-project.my_dataset', 'products', cast('2021-11-01' as date));          
```

Expect a table to have a daily number of rows continuously increasing since a predefined date.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **value** (_DATE_) – The starting date to check for increase in value
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">everyweek\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

```sql
CALL `tailer-ai.expect.everyweek_since`('my-project.my_dataset', 'sales_details', 'sale_date', DATE_TRUNC(DATE_SUB(current_date, interval 2 month), week), ['2022-01-01', cast(current_date as string)], 1000);          
```

Expect a table to have a date column with a date every week since start\_date, and containing a minimum number of rows. An exception list can be provided to avoid an error when a date has no data for a good reason.

This procedure generates a date array containing the start\_date and the same day for every week until the current date. Then it counts the rows of the table grouped by date. If a day between the specified start date and today is missing, or if a daily count is below minimum, or if an extra date is in the table but does not fit in the monthly pattern, then the test fails, except if the date is specified in the exception list.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">everymonth\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

```sql
CALL `tailer-ai.expect.everymonth_since('my-project.my_dataset', 'sales_details', 'sale_date', DATE_TRUNC(DATE_SUB(current_date, interval 13 month), month), ['2022-01-01', cast(current_date as string)], 1000);          
```

Expect a table to have a date column with a date every month since start\_date, and containing a minimum number of rows. An exception list can be provided to avoid an error when a date has no data for a good reason.

This procedure generates a date array containing the start\_date and the same day for every month until the current date. Then it counts the rows of the table grouped by date. If a day between the specified start date and today is missing, or if a daily count is below minimum, or if an extra date is in the table but does not fit in the monthly pattern, then the test fails, except if the date is specified in the exception list.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY\<DATE>_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

### Row count

#### <mark style="color:purple;">table\_count\_greater(dataset, tablename, value)</mark>

```sql
CALL `tailer-ai.expect.table_count_greater`('my-project.my_dataset', 'stores', 1600, 0.01);          
```

Expect a table to have a count greater than or equal to a predefined value.

A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count represents less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **value** (_INT64_) – The value the table count must be greater to
  * **threshold** (_FLOAT64_) – The threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">table\_count\_between(dataset, tablename, value)</mark>

```sql
CALL `tailer-ai.expect.table_count_between`('my-project.my_dataset', 'customers', ['2000000', '300000']);           
```

Expect a table to have a number of rows to be between two values.

The values for the comparison must be provided as string and will be cast to integer during the assertion. The order in the array is important as we use the “between” predicat function to enforce this expectation: the lower value must be before the upper value.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **value** (_ARRAY\<STRING>_) – The array of values that will be used to check the table
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">table\_count\_equal(dataset, tablename, value)</mark>

```sql
CALL `tailer-ai.expect.table_count_equal`('my-project.my_dataset', 'stores', 500, 0.1);           
```

Expect a table to have a count equal to a predefined value.

A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count represents less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **value** (_INT64_) – the value the table count must be equal to
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">table\_count\_equal\_other\_table(dataset, tablename, target\_dataset, target\_tablename, threshold)</mark>

```sql
CALL `tailer-ai.expect.table_count_equal_other_table`('my-project.my_dataset', 'stores', 'my-project.my_other_dataset', 'stores', 0.01);          sql
```

Expect a table to have the same number of lines than another table.

A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count represents less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **target\_dataset** (_STRING_) – The target dataset
  * **target\_tablename** (_STRING_) – The target table name
  * **threshold** (_FLOAT64_) – The threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

### Column properties

#### <mark style="color:purple;">unique(dataset, tablename, column)</mark>

```sql
CALL `tailer-ai.expect.unique`('my-project.my_dataset', 'stores', 'store_id');          
```

Expect every value in the column to be unique.

This procedure checks that the number of distinct value of the specified column is equal to the total number of lines in the table. Null values are part of the process (so one line can be null but it must be the only one).

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">not\_null(dataset, tablename, column, threshold)</mark>

```sql
CALL `tailer-ai.expect.not_null`('my-project.my_dataset', 'sales_details', 'product_sku', 0.001);          
```

Expect a table to have a column to never be null.

This procedure counts the number of null in the specified column. A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count is less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name to check for value
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">null(dataset, tablename, column, threshold)</mark>

```sql
CALL `tailer-ai.expect.null`('my-project.my_dataset', 'logs', 'error_code', 0.05);          
```

Expect a table to have a column to be fully null.

This procedure counts the number of non-null values in the specified column. A threshold percentage can be provided, so the test is passed if the number of rejected rows divided by the table total row count is less than the threshold. Use 0 if no rejected row is allowed.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name to check for value
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">type(dataset, tablename, column, type)</mark>

```sql
CALL `tailer-ai.expect.type`('my-project.my_dataset', 'stores', 'store_id', 'INT64');           
```

Expect a table to have a column that can be casted as the predefined type with no error.

This procedure checks that a safe casted (to the wanted type) non-snull value will not be null. All BigQuery types are allowed (see BigQuery documentation [here](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types)).&#x20;

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its related GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **type** (_STRING_) – The type of the column to check
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">values\_to\_contain(dataset, tablename, column, value, minimum, threshold)</mark>

```sql
 CALL `tailer-ai.expect.values_to_contain`('my-project.my_dataset', 'sales', 'date', '2022-01-22', 1000, 0.01);           
```

Expect a table to have a column to contain a certain value at a certain minimum level with a threshold.

The authorized value type must be in a string (even for numeric values) as there is a safe\_cast to string in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (STRING) – The value of the predefined set
  * **minimum** (_INT64_) – The minimum value to have for the column
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure (as a percentage)
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">values\_to\_be\_between(dataset, tablename, column, value, threshold)</mark>

```sql
CALL `tailer-ai.expect.values_to_be_between`('my-project.my_dataset', 'sales', 'quantity' ,['-20','20'], 0.01);       
CALL `tailer-ai.expect.values_to_be_between`('my-project.my_dataset', 'sales', 'date', ['2015-01-01','2025-01-01'], 0);          
```

Expect a table to have a column to be between two values.

The authorized value type may be integer, float or dates to work properly. The between predicate requires the parameter to be included and in the proper order (for exemple for a set of date, the first date must be before the second date). A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_ARRAY_) – The array that contains the two values range
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">values\_to\_be\_in\_set(dataset, tablename, column, value, threshold)</mark>

```sql
CALL `tailer-ai.expect.values_to_be_between`('my-project.my_dataset', 'sales', 'type', ['1','2', '3', '5', '7', '9'], 0);            
```

Expect a table to have a column to be in a predefined set.

The authorized value type must be in an array as string as there is a cast in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_ARRAY_) – The values array of the predefined set
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.

#### <mark style="color:purple;">values\_to\_not\_be\_in\_set(dataset, tablename, column, value, threshold)</mark>

```sql
CALL `tailer-ai.expect.values_to_not_be_between`('my-project.my_dataset', 'sales', 'type', ['0', '6', '8'], 0);                   
```

Expect a table to have a column to NOT be in a predetermined set of values.

The not authorized value type must be in a array as string as there is a cast in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table (and its GCP project)
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_STRING_) – The target project of the foreign table
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    The last SQL job of the procedure returns a line as described [here](./#analyzing-raw-metrics). When this expectation is embedded in a table-to-table data operation, then this line is inserted into the tailer\_common.expectation\_results table in your GCP project.
