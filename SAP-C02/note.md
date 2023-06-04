# Migration

## Storage Gateway

Bridge between on-premise data and cloud data

## Use cases

* disaster recovery
* backup & restore
* tiered storage
* on-premise cache & low-latency file access

## Types of Storage Gateway

* ### S3 File Gateway

  `(NFS or SMB client -> S3 File Gateway ) -> HTTPS -> ( S3 bucket )`

  The most recently used data is cached in the file gateway

* ### FSx File Gateway

  `(SMB Client -> Amazon FSx File Gateway) -> SMB (Amazon FSx)`

  Useful for local cache for frequently accessed data

* ### Volume Gateway

  `(Application Server -> ISCSI -> Volume Gateway) -> HTTPS -> (s3 Bucket -> Amazon EBS Snapshots)`

  Useful for backing up on prem server volume

  * #### Cached volumes

    Low latency access to most recent data

  * #### Stored volumes

    Entire dataset is on premise, scheduled backups to s3

* ### Tape Gateway

  `(Backup server -> Tape Drive -> Tape Gateway) -> HTTPS -> (Virtual Tapes s3 -> Archived Tapes Glacier)`

  With Tape Gateway, companies use the same processes but, in the cloud

# Storage

## AWS DataSync

### Particularities

* Move large amount of data to and from
* Preserve permissions and metadata (NFS POSIX, SMB)
* Can work one way or any way
* You can use an AWS snowcone device with agent preinstalled that will be shipped to AWS if not enough network capacity

### Type of migrations

* On premise or other cloud to AWS (need agent)
* AWS to AWS (no agent needed)

### Type of replication tasks schedules

* Hourly
* Daily
* Weekly

### Sources and Destinations

* #### Sources

  * On premise
  * Other cloud

* #### Destinations

  * NFS
  * SMB
  * HDFS
  * S3 API

# Compute & Load-Balancing

## AWS AppSync

It's a managed service that use GraphQL in the backend

### Particularities

* Make it easy for application to exactly get the data they need
* Can combine data from one or more sources
* Integrate with DynamoDB, Aurora, Elasticsearch, others
* Custom sources with AWS lambda
* Retrieve data in real time (websocket)

# Data Engineering

## Kinesis Data Stream

Kinesis is a managed "data streaming" is a service that can ingest data at scale in Realtime. Data is is automatically replicated synchronously to 3 AZ.

### It's great for ingesting:

* applications logs
* metrics
* iot
* clickstream

### Capacity mode

* On-demand (scales shards automatically)
* Provisioned (you manage the shards over time)

### Particularity

* Data retention is 24 hours by default, can go up to 365 days
* Ability to reprocess / replay data
* Multiple application can consume the same stream
* Real time processing with scale of throughput

### Consumer && Producer

* #### Producer

  * AWS SDK: simple producer
  * Kenesis Producer Library (KPL)
  * Kenesis Agent (base on KPL)

* #### Consumers

  * AWS SDK: simple consumer
  * KCL (support parallels consumer (checkpoint))
  * Lambda

### Provisioning  && Shards

Stream divided into Shards / Partitions

* #### Producer

  * 1 MB/s or 1000 messages/s at write PER SHARD
  * "ProvisionedThroughputException" otherwise

* #### Consumer Classic

  * 2 MB/s at read PER SHARD across all consumers
  * 5 API calls per second PER SHARD across all consumers

* #### Consumer Enhanced Fan-Out

  * 2MB/s at read PER SHARD, PER ENCHANCED CONSUMER
  * No API needed (push model)

## Kinesis Data Firehose

Load stream into S3, Redshift, Elasticsearch, Splunk in batch writes and near real time. The service scale automatically.

### Sources

* Kenesis Data Streams
* Amazon Cloudwatch (logs & Events)
* AWS Iot
* Kenesis Agent
* Client
* SDK/KPL

### Destinations

* Amazon S3
* Amazon Redshift
* Amazon OpenSearch
* Newrelic (3rd-party)
* Datadog (3rd-party)
* Splunk (3rd-party)
* HTTP Endpoint (Custom Destination)

### Failed Destination and source records before transformation (was not able to write data)

* S3 bucket

### Data Buffers

Data is accumaled in buffers

* #### Buffer Size (32 MB) when reached, data is flushed

* #### Buffer Time (1 minute) when reached, data is flushed

## Kinesis Data Analytic

Perform real time analytic on streams using SQL. It's serveries and scale automatically

### Possible sources

* Kinesis stream
* Kinesis firehose