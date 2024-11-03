# AWS Glue ETL Project: Data Pipeline from S3 to Snowflake

This project demonstrates a data pipeline in AWS Glue to extract data from an Amazon S3 bucket, transform it by adding a timestamp, and load it into S3 Bucket. This pipeline uses AWS Glue’s ETL capabilities, with PySpark handling transformations, and Amazon S3 as the storage source.

## Project Structure

- **AWS Glue Job**: Runs the ETL pipeline, processing data from the S3 source.
- **Transformations**: Adds a processing timestamp to track the ETL run.
- **Storage**: The processed data is stored in Amazon S3 in Parquet format.

## Prerequisites

- **AWS Account** with permissions for S3, Glue, and IAM.
- **Python & PySpark** installed locally (for testing) or configured within Glue ETL.
  
## Step Up 
The setup.yaml creates an s3 bucket, Glue IAM service role using AWS CloudFormation. 

After the code has been executed the following steps need to be executed on the console. 

1. Create a `rawData` data folder in the S3 bucket.  
2. Create a `processedData` folder in the S3 bucket.
3. Create a `scriptLocation` folder in the S3 bucket.
4. Create a `tmpDir` folder in the S3 bucket.
6. Upload source data into the `rawData` folder maintaining folder structure of customers,and orders.  

The S3 bucket should have the follow structure once set up; 

```
└── S3-Bucket-Name
    ├── athena
    ├── processedData
    ├── rawData
    │   ├── customers 
    │   │   └──  customers.csv 
    │   └── orders
    │       └── orders.csv 
    ├── scriptLocation    
    └──  tmpDir
```

## AWS Glue ETL Script

The primary ETL script `glue_etl_script.py` performs the following steps:

- **Data Extraction**: Reads raw data from S3 using AWS Glue Data Catalog.
- **Transformation**: Adds a `timestamp` column to mark the ETL run.
- **Load to S3**: Writes processed data back to S3 in Parquet format.

# Data 
Below is the schema for the table that wil be created in the Glue Data Catalog which includes a sample of the data.

**Customers**
| Customerid      | Firstname | Lastname| Fullname |
| ----------- | ----------- |-----------|-----------|
|  293 | Catherine                | Abel                   | Catherine Abel                 |
|  295 | Kim                      | Abercrombie            | Kim Abercrombie                |
|  297 | Humberto                 | Acevedo                | Humberto Acevedo               |

**Orders**

|  SalesOrderID |  SalesOrderDetailID |  OrderDate |  DueDate  | ShipDate | EmployeeID | CustomerID | SubTotal | TaxAmt | Freight | TotalDue | ProductID | OrderQty | UnitPrice | UnitPriceDiscount | LineTotal |
|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|---------------|
| 71782 | 110667 | 5/1/2014   | 5/13/2014  | 5/8/2014  | 276 |  293 |   33319.986 |  3182.8264 |  994.6333 | 37497.4457 | 714 |  3 |    29.994 |    0 |      89.982 |
| 44110 |   1732 | 8/1/2011   | 8/13/2011  | 8/8/2011  | 277 |  295 |  16667.3077 |  1600.6864 |  500.2145 |  18768.2086 | 765 |  2 |  419.4589 |    0 |    838.9178 |
| 44131 |   2005 | 8/1/2011   | 8/13/2011  | 8/8/2011  | 275 |  297 |  20514.2859 |  1966.5222 |  614.5382 |  23095.3463 | 709 |  6 |       5.7 |    0 |        34.2 |
