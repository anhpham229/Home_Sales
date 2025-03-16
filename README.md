# Home_Sales
Module 22
## Overview of the Analysis

In this analysis, the goal was to determine key metrics about home sales data. Below is a detailed overview of the dataset, methodology, and results of the analysis.

## Dataset Overview

* Size: The dataset contains 33,287 entries.

* Columns: 11 columns with id, date, date_built, price, bedrooms, bathrooms, sqft_living, sqft_lot, floors, waterfront and view.
Columns to be use in the analysis: date, price, bedrooms, bathrooms, floors and view. Columns date_built is use for partition.

## Methodology

* Optimizing Spark: Data Storage, Partitioning and Catching was used to compare running time so we can determine which on will be the most suitable metric to use.

* Process:
  - Data Reading: The data was read from a home_sales_data.csv file from AWS S3 bucket location into a PySpark DataFrame.
  - Data Preprocessing: Binned the CLASSIFICATION and APPLICATION_TYPE columns to group less frequent values.
  - Create a temporary table
  - Cat the temporary table
  - Partition by the "date_built" field on the formatted parquet home sales data.
  - Uncache the temporary table

## Results

In the comparison of different Spark optimizations—Data Storage, Caching, and Partitioning—the results indicated the following:

Data Storage: This approach, where the data is read directly from the S3 bucket and stored as parquet, was the fastest, with a processing time of 0.808 seconds. It demonstrates that reading and writing data in the efficient parquet format can minimize overhead, particularly for large datasets.

Caching: Caching the temporary table in memory resulted in a slightly slower performance, with a time of 1.04 seconds. While caching can help speed up subsequent queries by holding the table in memory, it introduces a minor overhead when the data is large. The difference in time may depend on the amount of available memory and the complexity of the operations being performed.

Partitioning: Partitioning by date_built took the longest, with a time of 1.4 seconds. Partitioning is useful for distributed systems, especially when querying large datasets over specific ranges of data (like dates), but it introduces some performance cost upfront during partition creation. However, it’s essential for managing large datasets and optimizing query performance in Spark, particularly when filtering on partitioned columns.

## Summary of Findings

Data Storage was the most efficient for this dataset in terms of performance.
Caching might be useful for improving query times in the future, especially when working with repeated queries over the same dataset.
Partitioning is a powerful optimization for very large datasets, although it comes with an added upfront cost in terms of processing time.

## Conclusion
From this analysis, Data Storage appears to be the most suitable approach for this dataset, as it minimizes the overhead of reading and writing data while maintaining efficient performance. Caching and Partitioning are valuable techniques depending on the specific needs of your queries and dataset size, but they may introduce additional overhead.