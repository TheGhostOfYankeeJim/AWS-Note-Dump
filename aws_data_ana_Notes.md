# Data Analystics

## Athena 
Serverless Query Service to analyze data

Use standard SQL lang

lives in your S3 Bucket data

Supports CSV, JSON, ORC, AVRO, and Parquet

$5 per TB scanned

Commonly used with Amazon QuickSight for reporting/Dashboards.

Use Columnar data for cost-savings
(you only scan the columns you need)

Use Glue tp cpnvert your data to Parquet or ORC

Compress Data for smaller retrievals

Partition Data Sets in S3 for easy querying on "virtual" columns
Use larger files >128 MB for min overhead

Federated Query 
Can run queries across many times of databases. use data source connectors that can "translate" the query. YOu'd use Lambda predomitely for this. 

## RedShift

Based PostgreSQL but not for OLTP

OLAP - Online Analytial Processing
10x better performance and other data warehouses
Columnar storage of data // parallel query engine 

Provisioned Cluster // Severless Cluster 

SQL Interface for queries 

- Faster queirs joins aggregrations compared to Athena

Leader Node - Plans the work

Compute Node - Actual Does The Work

Provisoned mode, can choose instances and reserve instances for cost savings

Snapshots // DR

Has Multi-AZ for some clusters
You need to take Snapshots
Can restore a snapshot to a NEW cluster
8 hours every 5GB o a schedule. 

Redshift Spectrum 

Query data in S3 wihtout needing to load it. 
Query is submitted to thousands of redshift spectrum nodes

## OpenSearch (ElasticSearch)
Replaces ElasticSearch 
Search any field for even partial matches

OpenSearch as a coplement to another database

Managed Cluster

Serverless Cluster 
Does not Support SQL Natively (PLugin can enable this)

OpenSearch Dashboards

## EMR 
Elastic MapReduce 

Hadoop Clusters for "Big Data" 

Comes bundled with Apache Spark, HBASE, Presto, Flink
Auto scalling and integrated with Spot Instances

Master Node, managed cluster, health, long running
Core Node, run tasks and store data
Task Node, just runs tasks, usually a spot instance 

On-Demand // Reserved (1 year min) // Spot Instances 

## QuickSight
Serverless Machin Learning BI to create interactive dashboards

BI, Builidng visualations, ad-hoc analysis

SPICE engine, only works if you import data into it. 

Column Level Security, so users wont see all columns 

User and groups only exist in QuickSite not IAM. 

## GLUE

Managed Extract Transform and Load ETL service 

Fully Serverless Service 

How to convert data to Parquet Format
S3 -> import CSV into GLUE -> Make Parquet to output S3 Bucket -> Athena looks at this file easier

CATALOG of datasets

Sends data crawlers to DBs, the crawlers writes the metadata to the catalog. 

Glue Job Bookmarks - helps you stop reanalyzing old data

Data Brew = NOrmalize data using pre-built transformation 

Glue Studio GUI to create and run/monitor ETL jobs in GLUE

Glue Streaming ETL instead of batch jobs run as streaming jobs. 

## Lake Formation 

Data lake -> ALL DATA IN ONE PLACE
Can set up a data lake in DAYS
Discover, clean, transform, and ingest
Automates many complex manual steps
Cp,bome strictired ad unstructed data in the data lake 
Out of the box blueprints: how do I set this up in X. 
This is built ontop of AWS Glue

BIg thing is Centralized Permissions, i.e. you have so many places to enter permissions wrong. So this all sits in one spot for Row and Column protections. 

12 out of 14 questions right. 