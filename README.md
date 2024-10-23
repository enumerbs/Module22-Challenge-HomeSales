# Module22-Challenge-HomeSales
Data Analytics Boot Camp - Module 22 - Big Data \
'Home Sales' Challenge

---

# Overview

Using a supplied dataset of house sales data, Spark SQL was used to run various queries, to answer questions such as *"What is the average price for a four bedroom house sold per year, rounded to two decimal places?"*.

The final question/Spark SQL query was used to compare the query execution timing for the following conditions:
1. Default - home sales dataset via the default PySpark file system / environment
1. Cached - in-memory cache (reduces scanning of the original files in future queries)
1. Partitioned - dataset partitioned, based on column value(s), on the file system.

The question used for performance comparisons was:
*What is the average price of a home per "view" rating, rounded to two decimal places, having an average home price greater than or equal to $350,000? Order by descending view rating.*

The corresponding Spark SQL was:
```
SELECT   view as ViewRating
        ,ROUND(AVG(price),2) as HouseAveragePrice
  FROM home_sales
 GROUP BY view
HAVING HouseAveragePrice >= 350000
 ORDER BY view DESC
 ```

# Results

### Query execution times

#### Local Spark environment

Default:&ensp;&ensp;&ensp; 2.089 seconds \
Cached:&ensp;&ensp;&ensp; 1.622 seconds \
Partitioned: 2.182 seconds

##### Online Spark environment

Default:&ensp;&ensp;&ensp; 1.212 seconds \
Cached:&ensp;&ensp;&ensp; 0.524 seconds \
Partitioned: 1.051 seconds

### Commentary

As expected, when the queries were run in the Online Spark environment they ran more quickly then in the Local Spark environment, as the local environment uses a single Spark node, whereas the online environment has access to multiple Spark nodes and so parallel processing.

The Cached queries ran the fastest in the Local and Online Spark environments, respectively.

The Partitioned cases gave roughly the same performance as the Default cases. Normally, we'd expect that the Partitioned query performance would be much better. However, in this example the particular PySpark query used for performance testing did not utilise the 'Partition By' column, so partitioning the dataset on disk provided little or no advantage compared with the unpartioned dataset.


# Implementation notes

The same queries were run using the following Jupyter Notebooks:
- ``Notebooks/Home_Sales.ipynb`` for the local Spark environment, and
- ``Notebooks/Home_Sales_colab.ipynb`` for the online environment.

The only significant difference between the Notebook implementations is the additional code to configure Spark for the online environment. However, there is no difference in the dataset or PySpark queries.

NOTE regarding 'Partition By': the activity instructions were to partition by the "date_built" field on the Parquet home sales dataset.

### Local Spark environment
- To get compatible versions of Spark and Hadoop binaries for Windows, the following were used:
    - **spark-3.0.0-bin-hadoop3.2** from the Apache Spark release archives ( https://archive.apache.org/dist/spark/ )
    - **hadoop-3.2.0** ``winutils.exe`` and ``hadoop.dll`` binaries for Hadoop on Windows from the 'cdarlint' GitHub repository ( https://github.com/cdarlint/winutils )
    - OpenJDK 11.0.16.1 (existing Java installation on Windows)

### Online Spark environment
- Google Colaboratory using Spark version 'spark-3.5.3'

# References

The following references were used in the development of the solution for this Challenge.

## Big Data

- Class notes/student activity files for 'Big Data', Monash University 'Data Analytics Boot Camp', including Jupyter Notebooks, were used as references and implementation guides.

## PySpark

- partitionBy method
    - https://www.geeksforgeeks.org/pyspark-partitionby-method/
    - https://sparkbyexamples.com/pyspark/pyspark-partitionby-example/

## Spark SQL

- Cache Table https://spark.apache.org/docs/latest/sql-ref-syntax-aux-cache-cache-table.html
