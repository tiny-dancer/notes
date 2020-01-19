# Kinesis Streams

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
- 3 Retries and then considered unsuccessfully processed and delivered to a specific bucket 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNzg1OTg4NjEsMTY1OTg2Mjc2OCwxMD
MxNTE2MTYsMjE1MzM3MzksMTg2MjY0ODg3N119
-->