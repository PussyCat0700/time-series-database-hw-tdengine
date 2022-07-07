# Insert Values If Tables Are Created.
 After Exectuting taosBenchmark in the previous section, we surely will have tables with device ID ranging from 0 to 9999. Therefore we will be having no problem inserting values to these tables right away. Note that we are inserting time series data directly into data collection point tables and timestamp is specified on the first column in the following code.
>INSERT INTO d1001 VALUES (now, 10.2, 219, 0.32);

>INSERT INTO d1001 VALUES (now, 10.15, 217, 0.33);

You will get a "Query OK" notice from taos shell after execution.
# What If Tables Are Not Created?

- ## Create A Table Before You Insert Data
We have learnt that a table can be associated with a super table to serve as a data collection point for fast aggregation queries among all tables related to this super table. Then of course, if we need to insert any data to a specific data collection point in our super table, first we'll need to create a table associated with this super table. 

A specific table needs to be created for each data collection point. Similar to RDBMS, table name and schema are required to create a table. Additionally, one or more tags can be created for each table. To create a table, a STable needs to be used as template and the values need to be specified for the tags. For example, for the meters the table can be created using below SQL statement.

> CREATE TABLE d1001 USING meters TAGS ("California.SanFrancisco", 2);

Then you can insert values to device ID d1001 as you wish!

>INSERT INTO d1001 VALUES (now, 10.2, 219, 0.32);

>INSERT INTO d1001 VALUES (now, 10.15, 217, 0.33);

- ## Insert Data On The Go

One thing I like about TDEngine is that you don't even have to create a data collection point table before inserting data to it. Instead, TDEngine can infer that you need to create and insert into that data collection point. Sounds amazing! And it really works with the following code here:

> INSERT INTO d1001 USING meters TAGS ("California.SanFrancisco", 2) VALUES (now, 10.2, 219, 0.32);

In the above SQL statement, a row with value (now, 10.2, 219, 0.32) will be inserted into table "d1001". If table "d1001" doesn't exist, it will be created automatically using STable "meters" as template with tag value "California.SanFrancisco", 2.

> INSERT INTO d1001 VALUES (now, 10.15, 217, 0.33);

In this way we don't have to create a data collection point before insertion. With super table name and tags specified creation and insertion can happen on the go with just one line of code. The table can be created automatically using the SQL statement above. Anyway, we also need to notice that nothing will happen if the table already exists.
