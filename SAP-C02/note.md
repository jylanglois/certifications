# Security

## AWS Config

Help record configuration and changes overt time

## Particularities

* AWS Config is a per-region service
* Config data can be aggregated across regions and accounts

## AWS Config Rules

### Type of rules

* AWS managed config rules
* custom config rules (AWS lambda)

### Type of evaluations

* At each config changes
* Regular time intervals

## AWS WAF - Web Application Firewall

### Can be deployed on

* Application Load Balancer
* API Gateway
* Cloudfront
* AppSync

### Web ACL

* #### Rules

  Can include IP addresses, HTTP headers, HTTP body, or URI strings. Can be rate based.

* #### Managed Rules

  Library of over 190 managed rules

  * Baseline Rule Groups (general protection)
  * Use-case Specific Rule groups (protection for many AWS WAF use cases)
  * IP Reputation Rule Groups (block request based on source)
  * Bot control Managed Rule group (block and manage requests from bots)

* #### Rule Actions

  Count | Allow | Block | CAPTCHA

# Identity & Federation

## AWS Resource Access Manager (ram)

Share resource that you own with any account or within your Organization to avoid resource duplication.

### Resource that can be shared

* #### VPC Subnets 

  Must be from same organization and cannot share security group and default vpc. Participant will manage their own resources

* #### Managed prefix list

  A set of one or more CIDR blocks

  * Customer managed prefix list
  * AWS-Managed Prefix List

* #### AWS Transit gateway

* #### Route 53

* #### License Manager Configurations

* #### Aurora DB clusters

* #### ACM private autority

* #### etc...

# VPC

## Direct Connect

### Virtual Interface (VIF)

* #### Public VIF

  Connect to Public AWS endpoints (s3 buckets, ec2 service etc...)

* #### Private VIF

  connect to resources in your VPC (EC2 instances, ALB)

* #### Transit Virtual Interface

  connect to resources in a VPC using a Transit Gateway

### Link Aggregation Groups

Get increased speed and failover y summing up existing DX connection into logical one

### Connection Types

* #### Dedicated Connections (1Gbps, 10 Gbps, 100 Gbps)

  Request made to AWS first, then completed by aws Direct Connect Partners

* #### Hosted Connections (50 Mbps, 500 Mbps, 10 Gbps)

  Connection requests are made via AWS Direct Connect Partners and capacity can be added or removed on demand

### Direct Connect Gateway

If you want to setup a Direct Connect to one or more VPC in many different regions (same/cross account)

## VPN Cloud Hub

If you have multiple AWS Site-to-Site VPN connections, you can provide secure communication between site using the AWS VPN CloudHub that you can operate on a simple hub and spoke model that you can use with or without a VPC.

# Service Communication

## Amazon SQS

### Visibility Timeout

If a consumer fails to process a message within the Visibility Timeout the message goes back to the queue.

### Maximum Receives

We can set a threshold of how many times a message can go back to the queue.

### Dead Letter Queue (DLQ)

After the Maximum Receives threshold is exceeded, the message goes into a dead letter queue (DLQ)

* #### DLQ of a FIFO queue must also be a FIFO queue

* #### DLQ of a Standard queue must also be Standard queue

### Redrive to Source (DLQ)

Feature to help consume messages in the DLQ to understand what is wrong with them. When the code is fixed, we can redrive the message from the DLQ back into the source queue.

### Amazon SQS messages timers

Message timers let you specify an initial invisibility period for a message added to a queue

### Amazon SQS delay queues

If you create a delay queue, any message that you send to the queue remain invisible to consumers for the duration of the delay period

# Caching

## Cloudfront

* ### Signed URL

  Signed URL with expiration to control access to content in cloudfront. Good to restrict access to invidual files

* ### Signed cookies

  Allow you to control who can access your content when you don't want to change your current URLS or when you want to provide access to multiple restricted files.

## Amazon ElasticCache

### Caching strategies/strategies

* Application queries ElasticCache, if not available, get from RDS and store in ElasticCache (Lasy Load)
* Use it as a session store

### ElasticCache for Redis

* Multi AZ with Auto-Failover
* Support Read replicas to scale reads
* Can enabled persistent Data Durability with Read Only Files (AOF) and backup the data
* Look like a lot RDS

### ElasticCache for Memcache

* Multi-node for partitioning of data (sharding)
* Non persistent
* No backup and restore
* Multi-threaded architecture

# Databases

## AWS RDS

### Multi-AZ

Standby instance failover instance for failover in case of outage. The replication is synchronous.

### Patching

Booth primary and standby instance are upgraded at the same time.

### Read Replica

Read only copy that Increase Read Throughput. The replication is asynchronous (Eventual Consistency) and can be cross-region.

* #### Promoting

  You can promote a read replica to be a standalone database

## AWS Aurora

### Multi-AZ

By default, it's a multi AZ database.

### Patching

Aurora restart the database cluster, so you might experience a short period of unavailability before resuming use of your cluster.

### Read Replica

Up to 15 read replicas and you can get a reader endpoint to access them ALL. You can do cross region read replica. The data is synchronously replicated since it's replicated across several volumes.

* #### Promoting

  To increase availability, you can use Aurora Replica as failovers targets. That is, if the primary instance fails, an Aurora Replica is promoted to the primary instance.

### Endpoint Types

* #### Cluster Endpoint (Primary && Writer Endpoint (Read and Write))

* #### Reader Endpoint (Provide load-balancing)

* #### Custom Endpoint (Connect to subset of db instances)

# Identity & Federation

## Organization Unit

You can have OU within OU

* ### Root OU (contain management account)

* ### OU (contain member account)

## Permissions

When you create a member account the Organizations `OrganisationAccountAccessRole` is added to the member account 

## Feature Modes

* ### Consolidated billing

  * Consolidated Billing across all accounts - single payment method
  * Pricing benefits from aggregated usage (volume discount for EC2,S3...)
  * Reserved instances are shared (can be turned off) - (member account must have sharing turn on)

* ### All Feature

  * Includes consolidated billing features, SCP
  * Invited accounts must approve enabling all features
  * Ability to apply an SCP to prevent member accounts from leaving the org

## Service Control policies (SCP)

Define allowlist or blocklist IAM actions

## Particularities

* Does not apply to the Management Account
* SCP is applied to all the Users and Roles in the account, including Root user
* The SCP does not affect Service-Linked roles
* SCP must have an explicit Allow (does not allow anything by default)

## Policies

* ### Tag Policies

  Ensure consistent tags, audit tagged resources, maintain proper resources categorization

* ### AI Services Opt-Out Policies

  Certain AWS AI may use your content for continuous improvement of Amazon AI/ML services

* ### Backup Policies

  AWS Backup enables you to create Backup Plans that define how to backup your AWS resources

# Monitoring

## Send Logs to Cloudwatch

* Cloudwatch Logs Agent
* Cloudwatch Unified Agent
* SDK

## Export Cloudwatch logs

* ### S3 exports (not automated (up to 12 hors))

* ### Cloudwatch Subscriptions

  Filter the logs that you want and will sent data to the destination that you choose

  * #### Managed lambda function to elasticsearch (real time)

  * #### Custom lambda function

  * #### Kinesis Data firehose

  * #### Kinesis Data Streams

## X-Ray

Trace requests across your microservices (distributed systems)

## Integrations

* EC2 - install the X-Ray agent
* ECS - install the X-Ray agent or Docker container
* Beanstalk - agent is automatically installed if integration enabled
* API Gateway - helpful to debug errors (such as 504)
* Lambda

### SDK

The SDK is written in several language and send the trace event to the X-ray agent

# Migration

## AWS DMS

Quickly and securely migrate databases to AWS, resilient, self healing

### Particularities

* Muse use an EC2 instance with the DMS software installed to perform the replication tasks
* You can combine the tool with Database Migration Service (DMS)
* Support Full Load, Full Load + CDC, or CDC only

### Type of migration supported

* #### Homogeneous

  `Oracle -> Ocracle`

* #### Heterogeneous

  `Microsoft SQL -> Aurora`

### AWS Schema Conversion Tool (SCT)

Convert your Databases Schema from one engine to another

### Types of sources

* On premise
* EC2 instance
* Azure SQL Database
* Amazon RDS
* Amazon S3
* Document DB

### Targets

* On-premises
* EC2 instances
* Oracle
* MS SQL server
* PostgreSQL
* SAP
* OpenSearch Service (Only Target)
* Kinesis Data Streams (Only Target)
* Document DB (Only Target)

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

## AWS Snow Family

### Type of devices for migration

* #### Snowcone

  Small portable and good for harsh environments. You have the Snowcone with 8TB HDD and Snowcone SSD with 14 TB SSD

* #### Snowball Edge

  80 TB of hdd capacity for optimized storage and 42 for compute optimized. It's good for less then 10 petabyte migration

* #### Snowmobile

  Each snowmobile have 100 PB of capacity

### Type of device for edge computing

* Snowcone
* Snowball Edge

## VM migration services

### Application Discovery Service (DMS)

Plan migration projects by gathering information about on-premises data centers

* #### Agentless discovery

  An open Virtual Appliance (OVA) for vm inventory

* #### Agent-based discovery

  Agent that will discover system configuration, system performance, running processes

### Server Migration Service (SMS)

Lift and shift (rehost) solution which simplify migrating applications to AWS. (Continuous block level replication)

### Elastic Disaster Recovery (DRS)

Quickly and easily recover your physical, virtual and cloud based servers into AWS (Continuous block level replication)

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

## Amazon S3

### S3 Object Ownership (ACL)

S3 Object Ownership is an Amazon S3 bucket-level setting that you can use to control ownership uploaded to your bucket and to disable or enable access control lists (ACLs). By default, Object Ownership is et to the bucket owner enforced setting and all ACLs are disabled. When ACLs are disabled, the bucket owners own all the objects in the bucket and manages access to data exclusively using access management policies.

* #### ACLs disabled

  * `Bucket owner enforced` (default) (the bucket use policies to define access control)

* #### ACLs enabled

  * `Bucket owner preferred` (The bucket owner owns and has full control over new objects)
  * `Object writer` (The AWS account that uploads an ojbect owns the object, has full control over it, can can grant other users access to it through is ACL)

### S3 performance (Upload)

* #### Multi-Part upload

  Recommended for files > 100MB must use for files > 5GB. Can help parallelize uploads

* #### S3 Transfer Acceleration

  Use edge location to forward traffic to the target s3 bucket over a private connection

### S3 performance (Download)

* #### S3 Byte Range Fetches

  Parallelize GETs by requesting specific byte ranges. Better resilience in case of failure. It can also be utilized to retrieve only partial data.

### S3 Event Notifications

You can use Amazon s3 Event notifications feature to receive notifications when certain events happen in your S3 buckets

### S3 Select

With amazon s3 Select you can use structured query languages statement to filter the content of s3 object and retrieve only subset of data you need. You can use ScanRange to select non overlapping byte range for parallelizing.

### S3 Replication

* #### Cross Region Replication (CRR)

* #### Same Region Replication (SRR)

* #### Combine with Lifecycles Rules

* #### Replication Time Control

  Replicate most objects that you upload to Amazon s3 in seconds, and 99.99% of those objects within 15 minutes

## S3 Access Point

* Access Points contain a hostname, an AWS ARN, and an AWS IAM resource policy

# Compute & Load-Balancing

## AWS AppSync

It's a managed service that use GraphQL in the backend

### Particularities

* Make it easy for application to exactly get the data they need
* Can combine data from one or more sources
* Integrate with DynamoDB, Aurora, Elasticsearch, others
* Custom sources with AWS lambda
* Retrieve data in real time (websocket)

## Elastic Load Balancer

### Cross-zone load balancing

When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones

## AWS API Gateway

* ### Edge-Optimized (default)

  The requests are routed through the Cloud Front Edge location. The API gateway still live in one region

* ### Regional

  For client within the same region and could be manually combined with cloudfront (more control over caching strategies and the distribution)

* ### Private

  Can only be accessed from your VPC using and interface VPC endpoint (ENI)

###  Stages

Changes are deployed to stages (dev, test, prod). Stages can be rollback as a history of deployments is kept

### Caching API response

Caching reduces the number of calls made to the backend. Client can invalidate the cache with header (Cache-Control: max-age=0) or you can flush the entire cache.

### Logging

You can enable cloudwatch logging at the stage level and log full request and responses data. You can also enable xray on the api gateway.

### Authentification

* IAM based access (Pass iam credentials in headers)
* Lambda Authorizer (verify a custom OAuth, 3rd party authenfication)
* Cognition User Pools (client authenticate with cognito and pass the token to API gateway)

### Usage Plans && API keys

If you want to make an API available as an offering ($) to your customers.

* #### Usage plan

  Uses api keys to identify clients and meter access. You can configure throttling limits and quota limits that are enforced on invidual client

* #### API Keys

  Can use with usage plan to control access

## AWS Global Accelerator

Leverage the AWS internal network to route to your application and use 2 anycast static IP. Comparated to cloudfront, it support other protocol than http (udp && tcp) and bill a fixed hourly charge.

## Route 53

### Routing Policies

* #### Simple

  Route to a single resource and can return multiple value in the same record

* #### Weighted

  Control the % of the request that go to each specific resource

* #### Latency based

  Redirect to the resource that has the least latency close to us

* #### Failover (Active-Passive)

  Use health check to do a failover

* #### Geolocation

  This routing is based on user location

* #### Geoproximity

  Route traffic to your resources based on the geographic location of users and resources

* #### Traffic flow

  Simplify the process of creating and maintaining records in large and complex configurations

* #### Multi-Value

  Return multiples values, but only healthy one

* #### IP Based Routing

  Routing is based on clients IP address. You provide a list of CIDRS for your clients

### Resolvers Endpoints 

* #### Inbound Endpoint

  DNS resolvers on your network can forward DNS queries to Route 53 Resolver

* #### Outbound Endpoint

  Route 53 resolver conditionally forward DNS queries to your DNS resolvers

# Data Engineering

## Amazon Redshift

Redshift is based on PostgreSQL, but not used for OLTP. It's OLAP - online analytical processing (analytic and data warehousing)

### Multi-AZ

Some type of clusters support multi AZ.

### Snapshots & DR

* Snapshot are point in time backups of a cluster stored in s3

* Snapshot are incremental

* You can restore a snapshot into a new cluster

* You can configure redshift to automatically copy snapshots of cluster ton an other region. You can use `snapshot copy grant` to use the KMS key in a other region to encrypt the snapshot and copy it to an other region.

### 2 types of snapshots

* Automated: every 8 hours, every 5GB, or on a schedule with a set retention

* Manual: snapshot is retained until you delete it

### Redshift Spectrum

Query data that is already in s3 without loading it

### Redshift Workload Management (WLM)

Enable you to flexibly manage queries priorities within workloads. Route queries to appropriate queues at runtime.

* ### Automatic (WLM)

  Queues and resources managed by redshift

* ### Manual  (WLM)

  Queues and resources managed by you

### Redshift Concurrency Scaling

Enable to provide consistently fast performance with virtually unlimited users and queries

## Amazon Athena

Serverless Query service to analyze data stored in Amazon S3

### Supported formats

* CSV
* JSON
* ORC
* AVRO
* Parquet

### Performance improvements

* Use columnar data for cost-savings (less scan)  (Parquet or ORC)
* Can use Glue to convert your data to Parquet or ORC
* Compress data for smaller retrevial
* Use larger files
* Partition your data

### Federated Queries

Allow you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises) Those query use Data source that run on AWS lambda to run federated queries

## Amazon Document DB

MongoDB is used to store, query, and index JSON data. The service is similar to AWS aurora

### Type of instances

* primary
* replica

## Amazon timestream

A fully managed serverless database for timeseries data

### Particularities

* Much better and efficient than sql database for timeseries data
* Built-in time series analytics functions

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

## Amazon QuickSight

Serveries machine machine learning powered business intelligence service to create interactive dashboard

### Supported AWS sources

* RDS
* Aurora
* Athena
* Redshift
* OpenSearch
* S3
* Spice (directly in memory quicksight)

### Supported third party sources

* jira
* On premises database databases (JDBC)
* import (csv, xlsx, json etc...)

### Security Model

* Users (Standard versions)
* Groups (enterprise version)

# IOT Services

### IOT data analytic

Colect and process data for analytic with 4 component to configure:

* ### Chanel

  Store your raw data

* ### Pipeline

  Use to peform transformation on the raw data

* ### Datastore

  Processed data

* ### Dataset

  Query data

### AWS IoT Core

Managed cloud service that let your iot device talk to AWS managed service or your cloud application.

### AWS IoT Device Defender

A fully managed service that help you secure your fleet of iot device. It audit your iot resources against security best practice. It also let your applying mitigations

### AWS IoT Device Management 

Enable to see and manage your device through a single dashboard. It allow you to index your device and run query for searching and run jobs

###  AWS IoT Events

Is a fully managed iot service that makes it easy to detect and respond to events from IOT sensors end application

### AWS IoT Greengrass

Seamlessly extends AWS to devices so they can act locally on the data they generate, while still using the cloud for management, analytics, and durable storage

### AWS IoT SiteWise

Collect data and generate real time KPIS & metrics to make better data driven decision. It's store data as time series and allow to create dashboard to visualize live and historical data.



