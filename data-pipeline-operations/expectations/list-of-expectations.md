# List of Expectations

### Fashion Data Expectations

Stored Procedures for Data Quality

#### <mark style="color:purple;">everyday\_increasing\_since(dataset, tablename, value)</mark>

```sql
call `tailer-ai.expect.everyday_increasing_since`(‘my-gcp-project.my_dataset’, ‘products’, cast(‘2021-11-01’ as date));
```

Expect a table to have a number of line continuously increasing since a predefined date

This procedure is a part of a specific set where daily/weekly/monthly execution is required. We store each iteration in a timely manner and assert that for each iteration over time, we get a positive or null variation.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **value** (_DATE_) – The starting date to check for increase in value
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">everyday\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

```sql
CALL `tailer-ai.expect.everyday_since`(‘my-project.my_dataset’, ‘sales_details’, ‘sale_date’, DATE_sub(current_date, interval 31 day), [‘2022-01-01’, ‘2021-12-25’], 1000);
```

Expect a table to have a minimum number of rows per day since a start date. An exception list can be provided to avoid an error where a date has no data for a good reason.

Count the rows of the table grouped by date. If a day between the specified start date and today is missing, or if a daily count is below minimum, then the test fails, except if the date is specified in the exception list. An exception array can be provided to avoid an error where a date has no data. This test ensure daily continuity of data and is part of a freshness test suite.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">everymonth\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

Expect a table to have a date column to be filled with a minimum value for a every month since a start date.

Check for a table to have a date type column to be present and have a minimum grouped lines for a specific period of time in a monthly manner from the start\_date (included) up to the current date. So if I enter 1/1/2021 for column date\_to\_check, it will count all lines grouped by date\_to\_check and assert that the count is not below minimum and that all dates are properly present. An Exception array can be provided to avoid an error where a date has no data. A specific attention to the day used as it will be the day of the month that will be repeated (so if I set first day a 5 January, it will be every 5th of the month). This test ensure monthly continuity of data and is part of a freshness test suite.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">everyweek\_since(dataset, tablename, column, start\_date, exception, minimum)</mark>

Expect a table to have a date column to be filled with a minimum value for a everyweek since a start date.

Check for a table to have a date type column to be present and have a minimum grouped lines for a specific period of time in a weekly manner from the start\_date (included) up to the current date. So if I enter 1/1/2021 for column date\_to\_check, it will count all lines grouped by date\_to\_check and assert that the count is not below minimum and that all dates are properly present. An Exception array can be provided to avoid an error where a date has no data. A specific attention to the first day as it will be the day of the week that will be repeated (so if I set first day a monday, it will be every monday). This test ensure weekly continuity of data and is part of a freshness test suite.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **start\_date** (_STRING_) – The date to start the control
  * **exception** (_ARRAY_) – An array that contains dates that will not be checked
  * **minimum** (_INT64_) – The minimum amount of lines per date expected.
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### f<mark style="color:purple;">oreignkey(dataset, tablename, column, target\_project, target\_dataset, target\_tablename, target\_column, threshold)</mark>

Expect a table to have a column to be fully present into another table’s column

So we select source.column and target.column\_target from source and “left outer join” to target. Source and target column are joined on “source.column = target.column”. Perfect fk means we have the very same number of pk on both side, delta means fk constraint is badly formed.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **target\_project** (_STRING_) – The target project of the foreign table
  * **target\_dataset** (_STRING_) – The target dataset of the foreign table
  * **target\_tablename** (_STRING_) – The foreign table name
  * **target\_column** (_STRING_) – The the foreign key of the foreign table
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">not\_null(dataset, tablename, column, threshold)</mark>

Expect a table to have a column to never be null

The target column must always contain a value.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name to check for value
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">null(dataset, tablename, column)</mark>

Expect a table to have a column to be fully null

The target column must not contain any value.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name to check for value
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">primarykey(dataset, tablename, column, target\_project, target\_dataset, target\_tablename, target\_column, threshold)</mark>

Expect a table to have a column to be fully present into another table’s column

So we select source.column and target.column\_target from source and “left outer join” to target. Source and target column are joined on “source.column = target.column”. Perfect fk means we have the very same number of pk on both side, delta means fk constraint is badly formed.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **target\_project** (_STRING_) – The target project of the foreign table
  * **target\_dataset** (_STRING_) – The target dataset of the foreign table
  * **target\_tablename** (_STRING_) – The foreign table name
  * **target\_column** (_STRING_) – The the foreign key of the foreign table
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">primarykey\_named(dataset, tablename, column)</mark>

Expect a table to have a column to be behave like a primary key.

To be a primary key a column must be not null and unique within the current table. This is enforced by counting the total number of lines within the table and comparing it to the number of distinct element in the column.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name (can be an sql operation)
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">primarykey\_threshold(dataset, tablename, column, threshold)</mark>

Expect a table to have a column named or described as a PK to be unique with a defined tolerance

This procedures uses naming or tagging to detect the column that is likely to be a primary key. More precisely, it looks for “PK” in the name of the column or “PK” in the description of the column (the lookup is one on the metadata table). The threshold is a percentage of the total number of lines that can be wrong (1.0 means 1% and it means the assertion will fail if there is more than 1% of duplicated primary keys).

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">table\_count\_between(dataset, tablename, value)</mark>

Expect a table to have a number of lines to be between two values.

The values for the comparison must be provided as string and will be cast to integer during the assertion. The order in the array is important as we use the “between” predicat function to enforce this expectation.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **value** (_ARRAY_) – The array of values that will be used to check the table
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">table\_count\_equal(dataset, tablename, value)</mark>

Expect a table to have a count equal to a predefined value.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **value** (_INT64_) – the value the table count must be equal to
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">t</mark><mark style="color:blue;">a</mark><mark style="color:purple;">ble\_count\_equal\_other\_table(tablename, target\_dataset, target\_tablename, threshold)</mark>

Expect a table to have the same number of lines than another table.

The threshold value is compared at an absolute value of the source count.

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **target\_dataset** (_STRING_) – The target dataset of the foreign table
  * **target\_tablename** (_STRING_) – The foreign table name
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">table\_count\_greater(dataset, tablename, value)</mark>

Expect a table to have a count greater than or equal to a predefined value.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **value** (_INT64_) – the value the table count must be greater to
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">type(tablename, column, type)</mark>

Expect a table to have a column to be of a predefined type.

This procedure checks for the type of a column by ensuring that a casted (to the wanted type) non null value will not be null. Allow types are the one permitted by bigquery. -> Integer INT64 Numeric values without fractional components -> Floating point FLOAT64 Approximate numeric values with fractional components -> Numeric NUMERIC Exact numeric values with fractional components -> BigNumeric BIGNUMERIC Exact numeric values with fractional components -> Boolean BOOL TRUE or FALSE (case insensitive) -> String STRING Variable-length character (Unicode) data -> Bytes BYTES Variable-length binary data -> Date DATE A logical calendar date -> Date/Time DATETIME A year, month, day, hour, minute, second, and subsecond -> Time TIME A time, independent of a specific date -> Timestamp TIMESTAMP An absolute point in time, with microsecond precision -> Struct (Record) STRUCT Container of ordered fields each with a type (required) and field name (optional) -> Geography GEOGRAPHY

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **type** (_STRING_) – The type of the column to check
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">unique(tablename, column)</mark>

Expect a table to have a column to be unique per line

This procedure checks that the number of distinct value of the specified column is equal to the total number of lines in the table. Null values are part of the process (so one line can be null but it must be the only one).

* **Parameters**
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">values\_to\_be\_between(dataset, tablename, column, value, threshold)</mark>

Expect a table to have a column to be between two values.

The authorized value type may be integer, float or dates to work properly. The between predicate requires the parameter to be included and in the proper order (for exemple for a set of date, the first date must be before the second date). A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_ARRAY_) – The array that contains the two values range
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">values\_to\_be\_in\_set(dataset, tablename, column, value, threshold)</mark>

Expect a table to have a column to be in a predefined set.

The authorized value type must be in an array as string as there is a cast in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_ARRAY_) – The values array of the predefined set
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">values\_to\_contain(dataset, tablename, column, value, minimum, threshold)</mark>

Expect a table to have a column to contain a certain value at a certain minimum level with a threshold.

The authorized value type must be in a string as there is a safe\_cast to string in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** () – The values array of the predefined set
  * **minimum** (_INT64_) – The minimum value to have for the column
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure (as a percentage)
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.

#### <mark style="color:purple;">values\_to\_not\_be\_in\_set(dataset, tablename, column, value, threshold)</mark>

Expect a table to have a column to be fully present into another table’s column

Expect a table to have a column to NOT be in a predetermined set of values.

The not authorized value type must be in a array as string as there is a cast in the verification predicat. A threshold might be specified so marginal value might not trigger any assertion exception.

* **Parameters**
  * **project** (_STRING_) – The GCP project
  * **dataset** (_STRING_) – The dataset of the table
  * **tablename** (_STRING_) – The table name
  * **column** (_STRING_) – The column name
  * **value** (_STRING_) – The target project of the foreign table
  * **threshold** (_FLOAT64_) – the threshold to use to trigger an assertion failure
*   **Returns**

    nothing (the result is stored in expectation\_output table)
*   **Return type**

    nothing (expectation result sets are defined in the “Output” section)
*   **Raises**

    – An exception will be thrown if the assertion fails.
