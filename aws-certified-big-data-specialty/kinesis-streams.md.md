# Domain 1 - Collection

## Kinesis Streams

> "Useful in any scenario where you are streaming large amounts of data that needs to be processed quickly and you have a requirement to build a custom application to process and analyze streaming data"
-acloudguru

## Core Concepts

### Shards

Uniquely identified groups of records in a stream

Single Shard Capacity:

- 1 MB/sec input
- 2 MB/sec output
- 5 transactions/sec for reads

### Records and it's componenents

#### Partition Key

#### Sequence Number

#### Data

### Data Consumers (KCL)

#### Aggregating

Combining multiple User Records into a single Stream Record
--> maximizing the usage of the 1mb Stream Record data limit

#### Collecting

Combining multiple Stream Records into a single http call (PutRecords)
--> minimizing http bandwidth

#### Dynamodb Checkpointing

10 read capacity units
10 write capacity units

Causes of "Provisioned throughput exceptions"
- Application does frequent checkpointing
- Stream has too many shards [for dynamo provisioned throughput]

> Consider adding more provisioned throughput to the dynamoDB table


### Stream Retention Period

- 1 day by default, configurable up to 7 days

### Data Producers

#### KPL

#### Kinesis Agent

#### Kinesis Streams API

## Emitting Data to AWS Services

### Consumer Destinations

### Use Cases

- **S3**: Archiving Data
- **DynamoDB**: Metrics
- **Elasticsearch**: Search and Index
- **Redshift**: Micro batch loading
- **EMR**:  Process and Analyze Data
- **Lambda**: Automate emitting data

### Kinesis Connector Library

- S3
- DynamoDB
- Elasticsearch
- Redshift
- EMR

> Exam wil not go into code level questions but good to understand how it architecturally works within AWS and why one would use 

### Lambda

Can automatically read records from a kinesis stream. process them and send the records S3, DynamoDB, Redshift or anywhere in the world.

> Exam will specifically target how to get data from kinesis data streams into redshift

## Amazon Kinesis Firehose

- Collects and loads streaming data in near real-time
- Load data into S3, Redshift, Elasticsearch and Splunk
- Use existing BI tools and dashboards
- Highly available and durable
- Fully managed
  - Scalability, sharing, and monitoring with zero administration
  - Minimize storage
  - Secure

### Loading Data into Kinesis Firehose

#### Amazon Kinesis Agent

- Web servers, log servers, database servers
- Download and install the agent
- Monitor files and sends to a delivery stream
- CloudWatch metrics
- Pre-process data
	- Multi-line records to single line
	- Convert from delimiter to JSON Format
	- Convert from log format to JSON Format

#### AWS SDK

- Java, .NET, Node.JS, Python, Ruby, Go, etc.
- PutRecord
- PutRecordBatch

### Transforming Data Using Lambda

Invoke a lamba function to transform

#### Data Transformation Flow

1. Buffers incoming data up to 3 MB or the buffering size specific in the delivery stream
2. Firehose invokes lambda function
3. Transformed data is sent  from lambda to firehose for buffering
4. Transformed data is delivered to destination when either the specified buffering size or interval is hit first

#### Parameters for Transformation

- **`recordId`**: Transformed record must have the same `recordId` prior to transformation
- **`result`**: Transformed record statuses:
  - `OK`: all good in the hood (successfully processed)
  - `Dropped`: intentionally dropped by processing logic (successfully processed)
  - `ProcessingFailed`: Failed (unsucessfully processed) 
- **`data`**: Transformed data payload after base64 encoding

#### Failure Handling

##### Transformations

- 3 Retries and then considered unsuccessfully processed and delivered to a specific ``processing_failed` bucket 
- When doing data transformation, can configure a back up bucket where all raw logs are stored

##### Destinations

- **S3**
  - retries delivery for up to 24 hours
- **Redshift**
  - Retry duration is 0-7200 seconds from S3
  - Skips S3 objects
  - Manifest than manual backfill
 - **Elasticsearch**:
   - Retry duration is 0-7200 seconds
   - Skips index request
   - Skipped documented delivered to S3 Bucket, `elasticsearch_failed`
   - Manual backfill

#### Exam

- Firehose can load streaming data into S3, Redshift and Elasticsearch
- How is lambda used with Firehose
- Key concepts

### Amazon SQS

> Exam: Need to understand where you would SQS and where you would use Kinesis Streams.

- `256kb` per message
- Any application can store in the queue
- Any application can retrieve from the queue
- `>256KB` can be managed using SQS extended client library, which uses S3
- Ensures delivery of a message at least once
- Supports multiple readers and writers
- Single queue can be shared my many components
- FIFO queues are supported
- A queue can be created in any region
- Messages can be retained in any region
- Messages can be retained in queues for up to 14 days
- Messages can be sent and read simulatenously
- Long polling eliminates extraneous polling

Queues can provide a convenient mechanism to determine the load in an application

#### Exam

SQS vs Kinesis Streams
- SQS is a message queue service allowing you to move data between distributed components of your applications without losing messages or requiring each application component to be available at all times
- Kinesis streams is for evaluating data in near real time

SQS use cases

- parallel fan out processing (ie. image upload, fans out to image recognization, thumbnail generation, and meta data reader

Kinesis Stream use cases

- fast log data data feed intake and processing 
- real-time metrics and reporting
- realtimedata analytics
- complex stream processing

### AWS IoT

Managed cloud platform allowing devices to interact securrely with applications and other devices.

IoT goes hand-in-hand with Big Data

- IoT devices produce data
- Analyze streaming data in real-time
- Process and store the data

IoT services can integrate with CloudWatch, DynamoDB, Elasticsearch, Kinesis Firehose, Kinesis Streams, Kinesis Firehose, Lambda, S3, SNS, and SQS.

> Exam: IoT service can create rule actions for kinesis streams, firehose, lambda, dynamoDB, and lambda

#### Authentication

- Each connected device requires a X.509
  - Upload your own CSR or CA
- AWS IoT uses IAM Policies for users, groups and roles
- Cognito identities for mobile applications

Cognito Identity
  - Allows you to use your own idp
  - login with amazon, facebook, google, twitter
  - OpenID providers, SAML identity Provider
  - use cognito user pool itself (no sso/federation)

> Exam: Cognito identities can be used to authenticate against IoT, specifcally mobile users. User pools can be used instead of third party authenticators and can scale to millions of users

#### Authorization

AWS IoT policies and IAM policies to control operations

#### Rule Engine

- Provide a thing the ability to interact with the AWS IoT service and other services
- Rules enable you to transform messages and route them to various AWS services
- Messages are transformed using a SQL based syntax
- Based on the rule,  a rule action is triggered and that rule action kicks off the delivery of message to other AWS services

#### Exam

- [ ] Authentication and Authorization
- [ ] Pay special attention to how Cognito works with AWS IoT
- [ ] Device gateway, device registry and device shadow
- [ ] Know what the rules engine does
- [ ] Know what a rule action does
- [ ] Rule actions for: cloudWatch, DynamoDB, Elasticsearch, Kinesis Firehose, Kinesis Streams, Kinesis Firehose, Lambda, S3, SNS, and SQS.
### AWS Data Pipeline

- Helps you process and move data between AWS compute and storage services and on-premise daa sources
- Create an ETL workflow to automate processing of data scheduled intervals, and then delete the resources

> Exam: Data pipeline comes up under backing up and restoration of data into other regions

Data Pipeline can execute on premise.

- **Datanode**: End of destination for your data
- **Activity**: an action that data pipeline initates on your behalf as part of a pipeline
- **Precondition**: A readiness check that can be optionally associated with a data source or activity
		-Must complete successfully before any activity consuming the data source are launched
		- Common use cases:
			- Check for existance of resources, such as s3 key, dynamodb table, etc
- 
#### Exam

- Web service that helps you reliably process and move data betweend different  aws compute and storage services
- can be integrated with on-premise environments
- data pipeline will provision and terminate resources as, and when, required
- pipeline components include datanode, activity, precondition and schedule
- A lot of data pipelines functionality has been replaced by lambda

### Getting Data into AWS

#### Direct Connect

Connects a private data center directly to AWS datacenter.

##### Use Cases

- Ensure back ups and archives through storage gateway are consistent
- Consistently send GB+ files 

#### Snowball

Physically ship data to AWS, use for _substantial_ amounts of data (TBs and PBs)

#### Snowball Edge

Storage Optimized - Block and S3-compatibile object storage, 24 vCPUs
Compute 

#### Snowmobile

Can transfer up to 100PB per snowmobile 

#### Storage Gateway

- Allows on-premise applications to use cloud storage
- Backup/archiving, DR, stoage and migration
- Applications connect through a vm or hardware gateway appliance
- S3, Glacier, EBS and AWS Backup
- Optimized data transfer mechanism, bandwidth management, efficient data transfer (includes local data cache for low latency)

#### S3 Transfer Acceleration

- Uploading to a centralized bucket from various parts of the world
- Transferring GBs - TBs of data across continents
- Transfer accerleration uses cloudfront
- Data routed from edge location to S3 over an optimized network path

#### Database Migration Service

- Source databases remain operational/minimize downtime
- DMS supports migration to various open source and commercial databases
- Homogenous/Heterogenous migrations
- One-time migrations/continuous replication to AWS

#### Exam

- **Direct Connect**: Increased bandwith throughput and consistent network performance
- **Snowball**: TB and PB scale solution
- **Snowball Edge**: Inconsistent connectvity/move data to AWS (TB and PB scale)
- **Snowmobile**: PB and EB scale solution
- **Stoage Gateway**:  On-premise application data to AWS, backups, files that need to be accessed by Redshift, EMR, etc.
- **S3 Transfer Acceleration**: No proximity to a region with S3
- **DMS**: One-time migrations/continuous replication to AWS

# Domain 2 - Storage

## Glacier

- Keep all your data at a much lower cost
	- Can use object lifecycle policies to automate this process
- Compliance requirement to keep your data
- Vault Lock
	- Deploy and enforce controls for a vault with a vault lock policy
	- Policies
		- Locked from edit
		- Policies cannot be changed after locking 
		- Enforce compliance controls
		- Created using IAM

### Vault Lock

1. Initiate Vault Lock
	2. Attaches a vault lock policy to your vault
	3. The Lock is set to an `InProgress` state and a lock id returned
	4. 24 hours to validate the lock (expires after 24 hours if not validated)
2. Complete Vault Lock
	3. `InProgress` to `Locked` state (cannot be changed once in `Locked` state)

### Exam

- Vault lock controls (prevent deletion, time-based retention)
- Implement using IAM policies

## DynamoDB Intro

A fully managed, NoSQL database service

Use Cases:
- Mobile
- Web
- Gaming
- IoT
- Live online voting
- Session Management
- S3 Object Meta-data

Capacity:

- Write Capacity Unit (WCU): Number of 1 KB blocks per second
- Read Capacity Units (RCU): Number of 4 KB blocks per second
- Supports Eventually (default) or strongly consistent reads
	- Eventually consistent reads leads to lower cost and/or higher throughput
  
## DynamoDB Partitions

- A partition can handle 3000 RCU and 1000 WCU

> ! There is a capacity and performance relationship to the number of partitions
  
 - Design tables and applications to avoid I/O "hot spots"/"hot keys"
 - When `>10GB`or `>3000 RCU` or `>1000 WCU` required, a new partition is added and the data is spread between them over time  

So how is the data distributed?
- Based on its partition key (HASH)
Each partition key is:
- limited to 10 GB data
- limited to 3000 RCU, 1000 WCU
- Be careful increasing and decreasing WCU/RCU, partitions will be added automatically but never removed

### Key Concepts

- Be aware of the underlying storage infrastracture - partitions
- Be aware of what influenes the number of partitions
  - Capacity (every 10 GB)
  - Performance (every 3,000 RCU or 1,000 WCU)
 - Partitions increase, but do not decrease
 - Be aware that table performance is split across partitions
 - True performance in based on performance allocated, key structure and time and key distribution of reads and writes

### Exam

- Questions are varied 
  - RCU, WCU
  - DynamoDB streams

- Understand the key concepts on partitions
- Understand how RCU and WRU impact performance
- Understand how partition key choices impact performance
- Understand sort key selection

## DynamoDB Global and Local Secondary Indexes

Two primary retrieval options, `SCAN` and `QUERY`

### Local Secondary Indexes

Contains Partition, Sort and new Sort
- Optional projected values

Any data written to the table is copied async to any LSIs (assume eventual consistency with Local Secondary Indexes)

Shares RCU and WCU with the table

A LSI is a sparse index. An index will only have an ITEM, if the index sort key attribute is contained in the table item (row).
- Known as a sparse index

#### Storage and performance considerations with LSI's

Any non key values by deafult are not stored in a LSI
- If you query an attribute which is NOT projected, you are charged for the entire ITEM cost from pulling it from the main table

> Item Collections: Limit the size of data storable for a partition key to 10GB, Only apply to tables with a LSI

### Global Secondary Indexes

- Has the ability to have it's own set of RCU and WCU definitions
- GSI updates are written async, ie. GSIs are eventually consistent

### Exam

- Know the fundamentals of LSIs and GSIs
- [Scaling Writes on DynamoDB Tables with Global Secondary Indexes](https://aws.amazon.com/blogs/big-data/scaling-writes-on-amazon-dynamodb-tables-with-global-secondary-indexes/)
- [Improving Data Access with Secondary Indexes](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html)

### Streams and Replication

#### Use Cases
- Replication
- Triggers
- DR: a database table in one AWS region, replicating to another AWS region for DR failover
- A lambda function triggered when items are added, performing analytics
- Lambda function sending emails, etc
- Large distributed applications with users worldwide: using a multi-master model

#### Replication

- Utilizing Lambda with the dynamodb streams

#### Exam

- Understanding the capabilitiy and use-cases of streams is important
- Knowing that streams + lambda allow traditional DB style triggers will help you answer a number of questions
- Understanding the type of stream views:
  - `KEYS_ONLY`
  - `NEW_IMAGE`
  - `OLD_IMAGE`
  - `NEW_AND_OLD_IMAGE`
 - Cross-region replication utilizing dynamodb streams 

### Performance

#### Size Formula

Partitions = gb size / 10

#### Performance Formula

Partitions = (desired RCU / 3000) + (desired WCU / 1000)

We need to ensure our key selection  parallelizes reading across partitions to maximize the distributed WCU/RCU across partitions

#### Key Selection

Attribute should have:

- Many distinct values
- A uniform wite pattern across all partition key values
- Uniform temporal write pattern across time
- If any of the above aren't possible with a exsting value, you should consider a sythnetic/created/hybrid value
- Shouldn't mix HOT and COL key values within a table

#### Key Sharding

Manual (better but not perfect)

- Prefix/Suffix partition key with a number to shard values across partitions
  - ie.  `dskfksdmf12` ⇒ `dskfksdmf12_01`

- Performance is split across partitions
- Number of unique partitions impacts performance
- Performance (WCU/RCU) is scoped to a partition

#### Bursting

SQS could be used a pre-write buffer


#### Exam

Fully understand all the concepts
- Partitions: what they are, how they work, and how to caculate their number
- How partitions influence performance together with partition keys
- Best practice for partition keys and how to structure data based on key-space and load
- Global indexes and how to select keys, and how this selction impacts the index and the table

Questions

> Which operation/feature or service would you use to locate all items in a table with a particular sort key value? (Choose 2)

Local secondary indexes can't be used: they only allow an alternative sort key, and query can only work against 1 partition key, with a single or range of sort. Global secondary indexes will allow a new index with the sort key as a partition key, and query will work. Scan will allow it, but is very inefficient. GetItem wont work: it needs a single P-KEY and S-KEY.


Each shard can support up to 5 transactions per second for reads, and up to 1,000 records per second for writes.

# Domain 3 - Processing

## EMR

Elastic Map Reduce

- Managed cluster platform
- Process and analyze large amounts of data
- MapReduce
- Using big data frameworks and open-source projects:
  - Apache Hadoop
  - Apache Spark
  - Presto
  - Apache HBase

### Use Cases

- Log Processing/Analytics
- Extract, Transform and Load (ETL)
- Clickstream Analysis
- Machine Learning

## Apache Hadoop

Hadoop software library is a framework

- Distributed processing of large data sets across clusters of computers using simple programming models
- Can scale up from single servers to thousands of machines, each offering local computation and storage
- The library itself is designed to detect and handle failures at the application layer

## EMR

### Storage

#### Instance Store

- Local storage, ephemeral 
	- High I/O performance
	- High IOPS at low cost
	- D2 and I3
#### EBS FOR HDFS

- volumes DO NOT PERSIST after cluster termination

####  EMR File System

- An implementation of HDFS which allows clusters to store data on S3
- Uses data directly on S3 without ingesting into HDFS
- Reliability, durability and scalability of S3
- Resize and terminate EMR clusters without losing data

> EMRFS with HDFS: Copy data from S3 to HDFS using S3DistCP

####  EMRFS and Consistent View
- S3 - Read after write consistency for new put requests
- S3 - Eventual consistency for overwrite of put and delete requests
- Listing after quick write may be incomplete
- For example an ETL pipeline that depends on list as input to subsequent steps
- Consistent View
	- Uses metadata in dynamodb to ensure EMR interaction is consistent while using EMRFS / S3


### EMR Operations


Scaling down: graceful shrink

- Keep replicaton factor in mind, you have less core nodes than the replication factor

Autoscaling

> Cannot add a auto scaling role after an EMR cluster has been created


### Hue

- Have an overall understanding of the benefits and advantages of Hue

### Apache Hive on EMR

Data Warehouse infra built on top of Hadoop

- Summarize, Query, and Analyze very large data sets
- Use a SQL-like interface
- Hive is useful for non-java programmers
	- Hadoop is implemented in Java
	- Hive provides a SQL abstraction

#### Use Cases

- Process and analyze logs
- Join very large tables
- Batch jobs (oozie)
- Ad-hoc interactive queries
 
#### S3

- Read and write data from and to S3
- EMRFS
- Partitioning in Hive
- Partitioning supported with S3 (EMRFS)
	- Time based data or source based with date

#### DynamoDB

EMR dynamodb connector:
- Join hive and dynamodb tables
- query data in dynamodb table
- copy data from a dynamodb table into HDFS and vice versa
- copy data from dynamodb to s3
- copy data from s3 to dynamodb


### HBase on EMR

Massively scalable, distributed big data store in the Hadoop system.

- Integrates with Hadoop, Hive, and Phoenix so you can combine massively parallel analytics with fast data access

#### Use Cases

- Ad tech (click stream analysis)
- Content (Facebook, Spotify)
- Financial Data (FINRA)

**When should you use HBase?**

- Large amounts of data - 100s of GBs to PBs
- High write throughput and updates rates
- NoSQL, flexible schema
- Fast access to data, random and real-time
- Fault-tolerance in a non-relational environment

**When NOT to use HBase**

- Transactional applications
- Relational database type features
- Small amounts of data

**HBase vs Redshift**

- HBase is more valuable for high write/update throughput and near real-time lookups (over fast changing data)
- Redshift is more valuable for OLAP (large complex queries, JOINS, aggregations)  

#### Exam
- What is HBase?
- When to use HBase and when NOT to
- Compared to DynamoDB and Redshift
- HBase integration with Hadoop, Hive, Pheonix, HDFS and EMRFS

### Presto On EMR

- Open-source in-memory distributed fast SQL query engine
- Run interactive analytic queries against a variety of data sources with sizes ranging from GBs to PBs
- Faster than hive

#### Advantages

- Query different types of data sources from relational data-sources, NoSQL database, frameworks like Hive to stream processing platforms like Kafka
- High concurrency, run thousands of queries per day (sub-second to minutes)
- In-memory processing help avoid unnecssary I/O, leading to low latency
- Does not need an interpreter layer like Hive does (Tez)

#### What NOT to use Presto for

- Not a database and not designed for OLTP
- Joining very large (100m plus rows) requires optimization
  - Use Hive instead
 - Batch processing

#### Exam

- High-level understanding of Presto
- Know where Presto should and should not be used

### Apache Spark on EMR

> Exam: Important topic 

- Fast engine for processing large amounts of data
- Run in-memory
- Run on disk
- Very popular in big data ecosystem, primarily because of performance

#### Use Cases

- Interactive Analytics
	- Faster than running queries in Hive
	- Flexibility in terms of languages (Scala, Python, etc)
	- Run queries against live data (Spark 2.0)
		- Structured Streaming
- Stream Processing
	- Disparate data sources
	- Data in small sizes
- Machine Learning
	- Repeated queries at scale against data sets
	- Train machine learning algorithms
	- Machine Learning Libary (MLlib)
		- Recommendation Engines
		- Fraud Detection
		- Customer Segmentation
		- Security
- Data Integration
	- ETL
	- Reduce time and cost taken to complete ETL processes

#### When NOT to use Spark

- Not a database and not designed for OLTP
- Batch processing
	- consider hive for very large batch jobs
- Avoid for large multi-user reporting environments with high concurrency
	- Run ETL in Spark and copying the data to a typical reporting database
	- Run batch jobs in Hive instead

#### Spark Core

General execution engine

- Dispatch and scheduling of tasks
- Memory management
- Supports APIs for Scala, Python, SQL, R and Java
- Supports the following APIs to access data

#### Spark SQL

> Exam: Question heavy

- Runs low-latency, interactive SQL queries against structured data
- RDD and DataFrame APIs to access variety of data sources using Scala, Python, Java, R or SQL
- Avro, Parquet, ORC and JSON
- Join across data sources
- Supports querying Hive tables using HiveQL
- JDBC/ODBC with existing tables (query and copy data into existing data sources)

#### Spark Streaming

> Exam: Question heavy

> "Spark streaming is an extension of the core Spark API that enables scalable, high through, fault tolerant stream proecssing of live-data streams"
> - Apache Spark documentation

#### MLlib

- Spark's scalable marchine learning library
- MLlib for out-of-the-box scalable, distributed machine learning algorithms

#### GraphX

- Spark's API for graphs and graph-parallel computation
- Iteractively build and transform graph structured data at scale
- Supports a number of graph algorithms

#### Cluster Managers

- Driver program coordinates processes
- Drive program connects to Cluster Manager
- Spark aqcquires executors
- Spark sends application code to executors
- Driver program sends tasks to the executors to run
- Does not requre Hadoop, can use standalone scheduler
- By default uses Hadoop YARN
	- Kerberos authentication
	- Dynamic allocation of executors
	- Dynamic sharing and central configuration of resources across various engines 

### Spark on EMR in the AWS Ecosystem

- Kinesis Streams
- Redshift
- DynamoDB

#### Spark Streamining and Kinesis Streams

- High-level abstraction called DStreams, which are a continuous stream of data
- DStreams can be created from input data streams like Kinesis Streams
- DStreams are a collection of RDDs
- Transformations are applied to the RDDs
- Results published to HDFS, databases or dashboards

Valuable blog posts for the exam:
- [https://aws.amazon.com/blogs/big-data/analyze-your-data-on-amazon-dynamodb-with-apache-spark/](https://aws.amazon.com/blogs/big-data/analyze-your-data-on-amazon-dynamodb-with-apache-spark/)
- [https://aws.amazon.com/blogs/big-data/optimize-spark-streaming-to-efficiently-process-amazon-kinesis-streams/](https://aws.amazon.com/blogs/big-data/optimize-spark-streaming-to-efficiently-process-amazon-kinesis-streams/)
- [https://aws.amazon.com/blogs/big-data/analyze-realtime-data-from-amazon-kinesis-streams-using-zeppelin-and-spark-streaming/](https://aws.amazon.com/blogs/big-data/analyze-realtime-data-from-amazon-kinesis-streams-using-zeppelin-and-spark-streaming/)

#### Spark and Redshift

1. Spark for extraction, transformation and load (ETL) of large amounts of data
- Hive tables in HDFS or S3, text files (csv), parquet, etc
- ETL in Spark gives you a performance benefit
2. Redshift for analysis

- Databrick's Spark-Redshift library
	- Reads data from redshift and can write back to Redshift by loading into Spark SQL DataFrames
		- Executes the Redshift UNLOAD command to copy data to a S3 Bucket

Valuable blog post for the exam:
- [https://aws.amazon.com/blogs/big-data/powering-amazon-redshift-analytics-with-apache-spark-and-amazon-machine-learning/](https://aws.amazon.com/blogs/big-data/powering-amazon-redshift-analytics-with-apache-spark-and-amazon-machine-learning/)

#### Spark and DynamoDB

Valuable blog post for the exam:
- [https://aws.amazon.com/blogs/big-data/using-spark-sql-for-etl/](https://aws.amazon.com/blogs/big-data/using-spark-sql-for-etl/)


#### Exam

- Know how Spark Streaming and Kinesis Streams work together
- High level understanding of how Spark integrations with Redshift and DynamoDB

### EMR File Storage and Compression

#### Files in HDFS and S3

- **HDFS**: data files are split into chucks automatically by Hadoop
- **S3**: Hadoop will split the data by reading the files in multiple HTTP range requests

#### Compression

| Algorithm | Splittable? | Compression Ratio | Speed     |
|-----------|-------------|-------------------|-----------|
| GZIP      | No          | High              | Medium    |
| bzip2     | Yes         | Very high         | Slow      |
| LZO       | Yes         | Low               | Fast      |
| Snappy    | No          | Low               | Very fast |

- Better performance when less data is transferred between S3, mappers and reducers
- Less network traffic between S3 and EMR
- Reduce storage and networking costs

#### File Formats

- **Text**: csv, tsv
- **Parquet**:  Columnar-oriented file format
- **ORC**: Optimized Row Columnar file format
- **Sequence**: Flat files consisting of binary key/value pairs
- **Avro**: Data serialization framework

#### File Sizes

- GZIP files are not splittable, keep them in the 1-2 GB range
- Avoid smaller files (100 MB or less), plan for fewer larger files
- S3DistCP can be used to combine small files into larger files
	- An extension of DistCP
	- S3DistCp can be used to copy data between S3 buckets or S3 to HDFS or HDFS to S3
- S3DistCp can be added as a step in EMR

#### Exam

- Study the compression algorithms table


### AWS Lambda in the AWS Big Data Ecosystem

- Function as a Service product (FaaS) 
- Billed only for the number of milli-seconds you use
- Isolated and stateless
- Persistence - your responsibility
- Event-driven
	- S3 Put
	- DynamoDB change 
- Integrates with other AWS Services
- Invoked via API Gateway (HTTP)

#### Limits

Ephemeral disk capacity: `512MB`
Number of processes and threads: 1,024
Max execution duration: 900 seconds

#### Exam

- Understanding of Lambda
- Integration with AWS Big Data services
- References
	- [https://aws.amazon.com/blogs/big-data/using-aws-lambda-for-event-driven-data-processing-pipelines/](https://aws.amazon.com/blogs/big-data/using-aws-lambda-for-event-driven-data-processing-pipelines/)
	- [https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html)
	- [https://aws.amazon.com/blogs/compute/amazon-kinesis-firehose-data-transformation-with-aws-lambda/](https://aws.amazon.com/blogs/compute/amazon-kinesis-firehose-data-transformation-with-aws-lambda/)
	- [https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html](https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html)

### HCatalog

Allows you to access the Hive metastore tables with:
- Pig
- SparkSQL
- Custom MapReduce applications

### AWS Glue

Fully managed ETL service
- Categorize, clean and enrich your data
- Move the data between various data stores
- Data catalog
- Serverless

#### Use Cases

- Querying data in S3
- Clean, normalize and enrich data into a Date Warehouse (informatica)
- Data catalog
	- Can crawl from:
		- Redshift
		- S3
		- RDS
		- DynamoDB
		- DBs in EC2
- Event-driven ETL Pipelines

#### When to use Glue vs other services

- Data Pipeline
	- Heterogenious jobs (Hive, Pig)
	- Control over where code runs and access to those environments (EC2, EMR...)
- EMR
	- Flexibility
	- More maintenance
	- Monitoring ETL jobs not as easy
- Data Migration Service
	- Database migration and replication
- Kinesis Data Analytics
	- Run SQL against incoming data
	- No real ETL service

--> Can Data Catalog with Glue
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMTAwNzU2ODgsNzIwNzEzMzM5LC0zNz
AxNDM4MjcsMjEzMTYwNjg3NywtMzczMjMxNjAzLC02OTMwMDUx
OCwxNjA3NTEwODEwLC0yMDQxMzM0NzY4LDEyMDY5OTg1OTEsLT
E2Njg3ODA3NjYsNDIzMTE0MjIyLC00MTY3NjI2ODAsLTE5ODA2
MTM2NzEsLTc2MjA1NDI3NSwzMjYzNjAzMzMsLTE1NTcxNDYzMz
gsLTc5NjczOTcxOSw1MDIzNjI5MDUsLTUyODI1NTA1MywxNjI4
NzQwNDE5XX0=
-->