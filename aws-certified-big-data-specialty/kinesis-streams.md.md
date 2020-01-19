# Kinesis Streams

> "Useful in any scenario where you are streaming large amounts of data that needs to be processed quickly and you have a requirement to build a custom application to process and analyze streaming data"
-acloudguru

## Core Concepts

### Shards

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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk4NzU1MjU3MywxMDMxNTE2MTYsMjE1Mz
M3MzksMTg2MjY0ODg3N119
-->