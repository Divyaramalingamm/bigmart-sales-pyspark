# Big Mart Sales Analysis with PySpark on Databricks

Hands-on PySpark project where I clean, transform, and analyse the Big Mart sales dataset (8,523 rows) in Databricks, and save the result as a Delta table.

## Dataset

- **Name:** Big Mart Sales dataset
- **File:** included in this repository (add the file name here, e.g. `bigmart_sales.csv`)
- **Size:** 8,523 rows
- **Key columns:** `Item_Identifier`, `Item_Weight`, `Item_Fat_Content`, `Item_Visibility`, `Item_Type`, `Item_MRP`, `Outlet_Size`, `Outlet_Type`

## Tools Used

- PySpark
- Databricks (Free Edition)
- Delta Lake
- SQL

## What I Did

### 1. Reading data
- Read the CSV file into a DataFrame
- Defined the schema explicitly with `StructType` instead of relying on `inferSchema`

### 2. Data cleaning
- Checked null counts per column
- Handled nulls in `Item_Weight` with `fillna` (and compared with `dropna`, which removed about half of the rows)
- Checked duplicates with `dropDuplicates`
- Standardised text in `Item_Fat_Content` using `lower`, `initcap`, `regexp_replace`, and `when/otherwise`
- Fixed data types with `cast`

### 3. Transformations
- Created new columns with `withColumn`, `concat_ws`, and `lit`
- Split and exploded string columns with `split` and `explode`
- Worked with dates using `current_date`, `date_add`, `date_sub`, `datediff`, and `date_format`
- Sorted data on multiple columns

### 4. Aggregations and analysis
- `groupBy` with `agg` (sum, avg, count), rounded to whole numbers
- `pivot` to show Outlet_Type across columns
- `collect_list` and `collect_set` to gather values per group
- Window functions: `row_number`, `rank`, `dense_rank`, running totals, top N per group
- Joins: inner, left, and left anti

### 5. Saving results
- Registered DataFrames as temp views and queried them with SQL
- Wrote the cleaned data to a Delta table using write modes (`append`, `overwrite`)

## Key Findings

- **Missing weights:** 1,463 of the 8,523 rows (about 17%) had a null `Item_Weight`, so 7,060 rows had a weight.
- **Dropping nulls is too costly here:** `dropna()` cut the data from about 8k rows to about 4k, because a null in any column removes the whole row. Filling `Item_Weight` with `fillna` and a `subset` keeps all 8,523 rows.
- **No exact duplicates:** `dropDuplicates()` did not change the row count, so every row is a different item and outlet combination.
- **Data types matter:** `fillna` only fills columns whose type matches the value given, so string and numeric columns need separate fill values.

## How to Run

1. Download the dataset file from this repository.
2. Upload it to your Databricks workspace (or a volume).
3. Import the notebook `bigmart_pyspark.ipynb` into Databricks.
4. Update the file path in the first cell.
5. Run the cells from top to bottom.

## What I Learned

- Why defining a schema is faster and safer than inferring one
- Why `dropna()` can remove too much data, and how `fillna` with a `subset` avoids it
- That transformations return a new DataFrame, so the result must be assigned back
- How window functions keep every row while adding rankings and running totals

## Next Steps

- Build a Bronze, Silver, Gold pipeline with this data
- Add data quality checks and a rejected-rows table
- Schedule the pipeline as a Databricks Job

## Author

**Divya R**
