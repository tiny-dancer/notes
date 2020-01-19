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

IoT services can integrate with 
- elasticsearch
- kinesis firehose
- kinesis stream
- dynamodb
- machine learning

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
- [ ] Rule actions for:
  - dynamodb
  - lamdba
  - kinesis firehose
  - kinesis streams
  - machine learning

### AWS Data Pipeline

- Helps you process and move data between AWS compute and storage services and on-premise daa sources
- Create an ETL workflow to automate processing of data scheduled intervals, and then delete the resources

> Exam: Data pipeline comes up under backing up and restoration of data into other regions

Data Pipeline can execute on premise.

- **Datanode**: End of destination for your data
- **Activity**: an action that data pipeline initates on your behalf as part of a pipeline
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk4MjUyMzQ3MCw2ODI0MjU0MCwxNTIyOT
c4OTYxLDQwMzE2MTIwNCwxNDU0MjAzNzExLDEwMjg4MTA4NjUs
MTY1OTg2Mjc2OCwxMDMxNTE2MTYsMjE1MzM3MzksMTg2MjY0OD
g3N119
-->