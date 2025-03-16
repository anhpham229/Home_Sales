# Home_Sales
Module 22
## Overview of the Analysis

In this analysis, the goal was to evaluate and determine key performance metrics for home sales data. The analysis compared different Spark optimizations, including data storage techniques, partitioning, and caching. Below is a detailed overview of the dataset, methodology, results, and conclusions drawn from the analysis.

## Dataset Overview

* Size: The dataset contains 33,287 entries.

* Columns: The dataset has 11 columns:
 - id, date, date_built, price, bedrooms, bathrooms, sqft_living, sqft_lot, floors, waterfront, view.
 - Columns used in the analysis: date, price, bedrooms, bathrooms, floors, view.
 - The date_built column is used for partitioning the dataset.

## Methodology

* Spark Optimization Techniques: In this analysis, the following Spark optimizations were implemented and compared:
 - Parquet Format: A columnar format that is efficient for both storage and query processing.
 - Partitioning: The data was partitioned based on the date_built column to optimize queries filtered by this field.
 - Caching: Caching the temporary table was used to improve the speed of subsequent queries by storing the data in memory.

* Process:
  - Data Reading: The dataset was read from a home_sales_data.csv file stored in an AWS S3 bucket and loaded into a PySpark DataFrame.
  - Temporary Table: A temporary table was created for efficient querying.
  - Run with parquet
  - Caching: The temporary table was cached to evaluate its impact on query performance.
  - Run with Caching
  - Partitioning: The dataset was partitioned by the date_built column, stored in Parquet format for efficient querying.
   - Run with Partitioning
  - Uncaching: After completing the operations, the temporary table was uncached to free up memory.

## Results

In the comparison of different Data Storage Optimization in Spark including Parquet format, Caching, and Partitioning - the results indicated the following:

* Parquet format: This approach, where the data is read directly from the S3 bucket and stored as parquet, was the fastest, with a processing time of 0.808 seconds. It demonstrates that reading and writing data in the efficient parquet format can minimize overhead, particularly for large datasets.

* Caching: Caching the temporary table in memory resulted in a slightly slower performance, with a time of 1.04 seconds. While caching can help speed up subsequent queries by holding the table in memory, it introduces a minor overhead when the data is large. The difference in time may depend on the amount of available memory and the complexity of the operations being performed.

* Partitioning: Partitioning by date_built took the longest, with a time of 1.4 seconds. Partitioning is useful for distributed systems, especially when querying large datasets over specific ranges of data (like dates), but it introduces some performance cost upfront during partition creation. However, it’s essential for managing large datasets and optimizing query performance in Spark, particularly when filtering on partitioned columns.

## Summary of Findings

- Parquet format was the most efficient for this dataset in terms of performance.
Caching might be useful for improving query times in the future, especially when working with repeated queries over the same dataset.
Partitioning is a powerful optimization for very large datasets, although it comes with an added upfront cost in terms of processing time.

## Conclusion
- From this analysis, Parquet format appears to be the most suitable approach for this dataset, as it minimizes the overhead of reading and writing data while maintaining efficient performance. Caching and Partitioning are valuable techniques depending on the specific needs of your queries and dataset size, but they may introduce additional overhead.