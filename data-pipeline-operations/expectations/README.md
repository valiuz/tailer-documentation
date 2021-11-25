# Expectations


### Expectations FashionDataExpectations
Stored Procedures for Data Quality


#### expect_everyday_increasing_since(dataset, tablename, value)
Expect a table to have a number of line continuously increasing since a predefined date

This procedure is a part of a specific set where daily/weekly/monthly execution is 
required. We store each iteration in a timely manner and assert that for each iteration
over time, we get a positive or null variation.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **value** (*DATE*) – The starting date to check for increase in value



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_everyday_since(dataset, tablename, column, start_date, exception, minimum)
Expect a table to have a date column to be filled with a minimum value for a everyday since a start date.

Check for a table to have a date type column to be present and have a minimum grouped lines for 
a specific period of time in a daily manner from the start_date (included) up to the current date. 
So if I enter 1/1/2021 for column date_to_check, 
it will count all lines grouped by date_to_check and assert that the count is not below minimum (included) and that all
dates are properly present. An Exception array can be provided to avoid an error where a date has no data. 
This test ensure daily continuity of data and is part of a freshness
test suite.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **start_date** (*STRING*) – The date to start the control


    * **exception** (*ARRAY*) – An array that contains dates that will not be checked


    * **minimum** (*INT64*) – The minimum amount of lines per date expected.



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_everymonth_since(dataset, tablename, column, start_date, exception, minimum)
Expect a table to have a date column to be filled with a minimum value for a every month since a start date.

Check for a table to have a date type column to be present and have a minimum grouped lines for 
a specific period of time in a monthly manner from the start_date (included) up to the current date. 
So if I enter 1/1/2021 for column date_to_check, 
it will count all lines grouped by date_to_check and assert that the count is not below minimum and that all
dates are properly present. An Exception array can be provided to avoid an error where a date has no data. 
A specific attention to the day used as it will
be the day of the month that will be repeated (so if I set first day a 5 January, it will be every 5th of the month).
This test ensure monthly continuity of data and is part of a freshness
test suite.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **start_date** (*STRING*) – The date to start the control


    * **exception** (*ARRAY*) – An array that contains dates that will not be checked


    * **minimum** (*INT64*) – The minimum amount of lines per date expected.



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_everyweek_since(dataset, tablename, column, start_date, exception, minimum)
Expect a table to have a date column to be filled with a minimum value for a everyweek since a start date.

Check for a table to have a date type column to be present and have a minimum grouped lines for 
a specific period of time in a weekly manner from the start_date (included) up to the current date. 
So if I enter 1/1/2021 for column date_to_check, 
it will count all lines grouped by date_to_check and assert that the count is not below minimum and that all
dates are properly present. An Exception array can be provided to avoid an error where a date has no data. 
A specific attention to the first day as it will
be the day of the week that will be repeated (so if I set first day a monday, it will be every monday).
This test ensure weekly continuity of data and is part of a freshness
test suite.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **start_date** (*STRING*) – The date to start the control


    * **exception** (*ARRAY*) – An array that contains dates that will not be checked


    * **minimum** (*INT64*) – The minimum amount of lines per date expected.



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_foreignkey(dataset, tablename, column, target_project, target_dataset, target_tablename, target_column, threshold)
Expect a table to have a column to be fully present into another table’s column

So we select source.column and target.column_target from source and “left outer join” to target.
Source and target column are joined on “source.column = target.column”.
Perfect fk means we have the very same number of pk on both side, delta means fk constraint is badly
formed.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **target_project** (*STRING*) – The target project of the foreign table


    * **target_dataset** (*STRING*) – The target dataset of the foreign table


    * **target_tablename** (*STRING*) – The foreign table name


    * **target_column** (*STRING*) – The the foreign key of the foreign table


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_not_null(dataset, tablename, column, threshold)
Expect a table to have a column to never be null

The target column must always contain a value.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name to check for value


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_null(dataset, tablename, column)
Expect a table to have a column to be fully null

The target column must not contain any value.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name to check for value



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_primarykey(dataset, tablename, column, target_project, target_dataset, target_tablename, target_column, threshold)
Expect a table to have a column to be fully present into another table’s column

So we select source.column and target.column_target from source and “left outer join” to target.
Source and target column are joined on “source.column = target.column”.
Perfect fk means we have the very same number of pk on both side, delta means fk constraint is badly
formed.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **target_project** (*STRING*) – The target project of the foreign table


    * **target_dataset** (*STRING*) – The target dataset of the foreign table


    * **target_tablename** (*STRING*) – The foreign table name


    * **target_column** (*STRING*) – The the foreign key of the foreign table


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_primarykey_named(dataset, tablename, column)
Expect a table to have a column to be behave like a primary key.

To be a primary key a column must be not null and unique within the current table.
This is enforced by counting the total number of lines within the table and 
comparing it to the number of distinct element in the column.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name (can be an sql operation)



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_primarykey_threshold(dataset, tablename, column, threshold)
Expect a table to have a column named or described as a PK to be unique with a defined tolerance

This procedures uses naming or tagging to detect the column that is likely to be a primary key.
More precisely, it looks for “PK” in the name of the column or “PK” in the description of the column (the lookup is one on the metadata table).
The threshold is a percentage of the total number of lines that can be wrong (1.0 means 1% and it means
the assertion will fail if there is more than 1% of duplicated primary keys).


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_table_count_between(dataset, tablename, value)
Expect a table to have a number of lines to be between two values.

The values for the comparison must be provided as string and will be
cast to integer during the assertion. The order in the array is important
as we use the “between” predicat function to enforce this expectation.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **value** (*ARRAY<string>*) – The array of values that will be used to check the table



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_table_count_equal(dataset, tablename, value)
Expect a table to have a count equal to a predefined value.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **value** (*INT64*) – the value the table count must be equal to



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_table_count_equal_other_table(tablename, target_dataset, target_tablename, threshold)
Expect a table to have the same number of lines than another table.

The threshold value is compared at an absolute value of the source count.


* **Parameters**

    
    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **target_dataset** (*STRING*) – The target dataset of the foreign table


    * **target_tablename** (*STRING*) – The foreign table name


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_table_count_greater(dataset, tablename, value)
Expect a table to have a count greater than or equal to a predefined value.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **value** (*INT64*) – the value the table count must be greater to



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_type(tablename, column, type)
Expect a table to have a column to be of a predefined type.

This procedure checks for the type of a column by ensuring that a casted (to the wanted type) 
non null value will not be null. Allow types are the one permitted by bigquery.
-> Integer INT64 Numeric values without fractional components
-> Floating point  FLOAT64 Approximate numeric values with fractional components
-> Numeric NUMERIC Exact numeric values with fractional components
-> BigNumeric  BIGNUMERIC  Exact numeric values with fractional components
-> Boolean BOOL  TRUE or FALSE (case insensitive)
-> String  STRING  Variable-length character (Unicode) data
-> Bytes BYTES Variable-length binary data
-> Date  DATE  A logical calendar date
-> Date/Time DATETIME  A year, month, day, hour, minute, second, and subsecond
-> Time  TIME  A time, independent of a specific date
-> Timestamp TIMESTAMP An absolute point in time, with microsecond precision
-> Struct (Record) STRUCT  Container of ordered fields each with a type (required) and field name (optional)
-> Geography GEOGRAPHY


* **Parameters**

    
    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **type** (*STRING*) – The type of the column to check



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_unique(tablename, column)
Expect a table to have a column to be unique per line

This procedure checks that the number of distinct value of the specified column is equal to the total number of lines in the table.
Null values are part of the process (so one line can be null but it must be the only one).


* **Parameters**

    
    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_values_to_be_between(dataset, tablename, column, value, threshold)
Expect a table to have a column to be between two values.

The authorized value type may be integer, float or dates to work properly. The between predicate
requires the parameter to be included and in the proper order (for exemple for a set of date, the first
date must be before the second date).
A threshold might be specified so marginal value might not trigger any assertion exception.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **value** (*ARRAY<string>*) – The array that contains the two values range


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_values_to_be_in_set(dataset, tablename, column, value, threshold)
Expect a table to have a column to be in a predefined set.

The authorized value type must be in a array as string as there is a cast in the verification predicat.
A threshold might be specified so marginal value might not trigger any assertion exception.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **value** (*ARRAY<STRING>*) – The values array of the predefined set


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.



#### expect_values_to_not_be_in_set(dataset, tablename, column, value, threshold)
Expect a table to have a column to be fully present into another table’s column

Expect a table to have a column to NOT be in a predetermined set of values.

The not authorized value type must be in a array as string as there is a cast in the verification predicat.
A threshold might be specified so marginal value might not trigger any assertion exception.


* **Parameters**

    
    * **project** (*STRING*) – The GCP project


    * **dataset** (*STRING*) – The dataset of the table


    * **tablename** (*STRING*) – The table name


    * **column** (*STRING*) – The column name


    * **value** (*STRING*) – The target project of the foreign table


    * **threshold** (*FLOAT64*) – the threshold to use to trigger an assertion failure



* **Returns**

    nothing (the result is stored in expectation_output table)



* **Return type**

    nothing (expectation result sets are defined in the “Output” section)



* **Raises**

    **<Exception>** – An exception will be thrown if the assertion fails.
