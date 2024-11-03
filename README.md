# Hands On AWS Glue Course

# Introduction 

This GitHub Repo contains the code alongside explanations for this [youtube video](https://youtu.be/ZvJSaioPYyo). 

The youtube video is a tutoral through the updated AWS Glue Service on the AWS Console UI. The video and rep will cover; 
- What is AWS Glue? 
- Why do we Use AWS Glue? 
- Setup Work For The Tutorial
- AWS Glue Data Catalog
- AWS Glue Databases 
- AWS Glue Tables 
- Partitions in AWS
- AWS Glue Crawlers 
- AWS Glue Connections 
- AWS Glue ETL 
- AWS Glue Data Quality 
- AWS Glue Data Brew
- AWS Glue Triggers
- AWS Glue Workflows 

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

# Step Up 
The setup-code.yaml contains code to be executed using Amazon CloudFormation. The code creates an s3 bucket, Glue IAM service role and athena working group that will use throughout the course. 

After the code has been executed the following steps need to be executed on the console. 

1. Create a `rawData` data folder in the S3 bucket.  
2. Create a `processedData` folder in the S3 bucket.
3. Create a `scriptLocation` folder in the S3 bucket.
4. Create a `tmpDir` folder in the S3 bucket.
5. Create a `athena` folder in the S3 bucket.
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

# What is AWS Glue?  
AWS Glue is a serverless data integration service that makes it easier to discover, prepare, and combine data for analytics, machine learning (ML), and application development. AWS Glue provides all the capabilities needed for data integration, so you can start analysing your data and putting it to use in minutes instead of months. AWS Glue provides both visual and code-based interfaces to make data integration easier. Users can more easily find and access data using the AWS Glue Data Catalog. Data engineers and ETL (extract, transform, and load) developers can visually create, run, and monitor ETL workflows in a few steps in AWS Glue Studio. Data analysts and data scientists can use AWS Glue DataBrew to visually enrich, clean, and normalise data without writing code

# Why do we Use AWS Glue?  
AWS Glue offers a fully manged serverless ETL tool. This removes the overhead, and barriers to entry, when there is a requirement for an ETL Service in AWS. 

# AWS Glue Data Catalog 
The AWS Glue Data Catalog is your persistent technical metadata store. It is a managed service that you can use to store, annotate, and share metadata in the AWS Cloud.

# AWS Glue Databases 
A set of associated Data Catalog table definitions organized into a logical group.

# AWS Glue Tables
The metadata definition that represents your data. The data resides in its original store. This is just a representation of the schema.

# Partitions in AWS
Folders where data is stored on S3, which are physical entities, are mapped to partitions, which are logical entities i.e. Columns in the Glue table.

# AWS Glue Connections
A Data Catalog object that contains the properties that are required to connect to a particular data store. Glue Connections can be used to connect to RDS, Redshift, S3, and other datastores. The connections can be used repeatedly throughout ETL code to avoid hard coding connection string details into scripts. 

Supported Connections; 
- Amazon DocumentDB
- Amazon OpenSearch Service, for use with AWS Glue for Spark.
- Amazon Redshift
- Azure Cosmos, for use of Azure Cosmos DB for NoSQL with AWS Glue ETL jobs
- Azure SQL, for use with AWS Glue for Spark.
- Google BigQuery, for use with AWS Glue for Spark.
- JDBC
- Kafka
- MongoDB
- MongoDB Atlas
- Salesforce
- SAP HANA, for use with AWS Glue for Spark.
- Snowflake, for use with AWS Glue for Spark.
- Teradata Vantage, when using AWS Glue for Spark.
- Vertica, for use with AWS Glue for Spark.
- Various Amazon Relational Database Service (Amazon RDS) offerings.
- Network (designates a connection to a data source that is in an Amazon Virtual Private Cloud (Amazon VPC))
- Aurora (supported if the native JDBC driver is being used. Not all driver features can be leveraged)

# AWS Glue Crawler 
You can use an AWS Glue crawler to populate the AWS Glue Data Catalog with databases and tables. This is the primary method used by most AWS Glue users. A crawler can crawl multiple data stores in a single run. Upon completion, the crawler creates or updates one or more tables in your Data Catalog. Extract, transform, and load (ETL) jobs that you define in AWS Glue use these Data Catalog tables as sources and targets. The ETL job reads from and writes to the data stores that are specified in the source and target Data Catalog tables.


# AWS Glue ETL 
An AWS Glue job encapsulates a script that connects to your source data, processes it, and then writes it out to your data target. Typically, a job runs extract, transform, and load (ETL) scripts. Jobs can run scripts designed for Apache Spark and Ray runtime environments. Jobs can also run general-purpose Python scripts (Python shell jobs.) AWS Glue triggers can start jobs based on a schedule or event, or on demand. You can monitor job runs to understand runtime metrics such as completion status, duration, and start time.

You can use scripts that AWS Glue generates or you can provide your own. With a source schema and target location or schema, the AWS Glue Studio code generator can automatically create an Apache Spark API (PySpark) script. You can use this script as a starting point and edit it to meet your goals.

AWS Glue can write output files in several data formats. Each job type may support different output formats. For some data formats, common compression formats can be written.

# Creators

**Johnny Chivers**

- <https://github.com/johnny-chivers/>

# Useful Links

- [youtube video](https://youtu.be/ZvJSaioPYyo) 
- [website](https://www.johnnychivers.co.uk)
- [buy me a coffee](https://www.buymeacoffee.com/johnnychivers)

Enjoy :metal: