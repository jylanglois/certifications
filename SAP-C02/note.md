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