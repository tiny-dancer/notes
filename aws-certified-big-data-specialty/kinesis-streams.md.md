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


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3MzIxMzg1NiwtMjc1NDA2ODUxLC04Nj
Y3OTk2NjgsMTc0NTUyMzkzMiwyMDgzMjc1NTY4LC0xNTE1MzQ3
NjIsMTY5MTA1NjAzMCwxNjQzOTIwMDQyLDQ3NzEwNDc4NCwtNT
M2OTMzNDI5LDM0MTM1OTYyMyw2ODI0MjU0MCwxNTIyOTc4OTYx
LDQwMzE2MTIwNCwxNDU0MjAzNzExLDEwMjg4MTA4NjUsMTY1OT
g2Mjc2OCwxMDMxNTE2MTYsMjE1MzM3MzksMTg2MjY0ODg3N119

-->