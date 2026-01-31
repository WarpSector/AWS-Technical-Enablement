# Domain 3: Technology
# (3E: Amazon Database Services)

### Executive Summary

## Types of Databases
   * #### Relational Databases
     * Organized by tables and rows.
     * Has a rigid schema (MySQL).
       * SQL is used to define the structure of the data and its elements.
       * SQL provides the tools to insert, update, delete, and querying data within the database table.
     * Typically scaled vertically.
     * Supports complex queries and joins.
     * **Examples:** Amazon RDS, MySQL, PostgresSQL, Oracle, IBM DB2.
   * #### Non-Relational Databases
     * Varied data storage models.
     * Flexible schema (NoSQL).
       * Data is stored in **key-value pairs**, columns, documents, or graphs.
       * There is no rigid schema so attributes can be missing or have different data types.
     * Scales horizontally.
     * Unstructured, simple language that supports any kind of schema.
     * **Examples:** Amazon DynamoDB, MongoDB, Redis, Neo4j.
   * #### Graph Databases
     * Designed to store, manage, and navigate relationships in data (e.g., Facebook).
     * Graph databases use:
       * Nodes to represent entities.
       * Edges to represent relationships.
       * Properties to store information about nodes and edges.
     * **Example:** Amazon Neptune.  

## Use Cases for Databases
   * #### Operational/Transactional
     * Online Transactional Processing (OLTP).
     * Production databases that process transactions (e.g., adding customer records, checking stock availability, etc.).
     * Short transactions, simple queries.
     * **Relational Examples:** Amazon RDS, My SQL, Oracle, IBM DB2.
     * **Non-Relational Examples:** MongoDB, Cassandra, Neo4j.
   * #### Analytical
     * Online Analytics Processing (OLAP).
     * The source data for OLAPs comes from OLTPs.
     * Data warehouse - typically separated from customer facing databases - data is extracted for decision-making.
     * Long transactions, complex queries.
     * **Relational Examples:** Amazon Redshift, Teradata, HP Vertica.
     * **Non-Relational Examples:** Amazon Elastic Map Reduce (EMR).
   * #### Amazon RDS and Amazon Redshift
     * Amazon RDS is Amazon's relational database service that ingests OLTP data (re: takes in transactional data).
     * RDS OLTP data is then copied into Amazon Redshift (Amazon's data warehouse service).
     * OLAP can then be performed on the OLTP data from RDS when copied into Redshift (performs data analysis, e.g., looking for trends in the data, etc.).
   * #### Amazon Databases
     * **Database on EC2:** full control over the database and instance, can load third-party databases that are not available in Amazon RDS.
     * **Amazon RDS:** traditional relational database, data is well-formed and structured (includes MySQL, PostgreSQL, Oracle, etc.).
     * **Amazon DynamoDB:** NoSQL database, dynamic scaling, useful for high I/O needs and in-memory (RAM) performance.
     * **Amazon Redshift:** data warehouse for large volumes of aggregated data.
     * **Amazon ElastiCache:** fast temporary storage for small amounts of data, in-memory (RAM) database.    

 ## Data Partitions vs. Data Shards
   * #### Data Partitions
     * Breaking a data table into separate pieces on a single EC2 instance (vertical scaling required).
   * #### Data Shards
     * Breaking a data table into separate pieces across multiple EC2 instances (horizontal scaling enabled).  
 
 ## Amazon Relational Database (RDS)
   * #### Overview
     * **Managed** relational database.
     * Used for transactional data processing (OLTP).
     * Runs on Amazon EC2 instances and stored in EBS block volumes.
   * #### Choice of DB Engines with RDS
     * **Amazon Aurora:** Amazon's proprietary relational database, MySQL & PostgrSQL compatibility, scalable, and very cost-effective (this is the important one to remember!).
     * MySQL
     * PostgreSQL
     * Oracle
     * Microsoft SQL    
   * #### Scaling
     * RDs is typically **scaled up** - you must stop and restart the scaled up instance.
     * Vertical scaling is required for enhanced **write** performance.
     * RDS can be **scaled out** using **RDS Read Replica**, which is a read-only copy of the RDS database that handles **read** requests while the original DB handles write requests - these can be **scaled out.**
   * #### Availability
     * You can create a **RDS Standby** copy in another AZ so you have multi-AZ deployment, which is essential for disaster recovery.
     * **RDS Standbys** are not used for scalability like Read Replicas are - their sole purpose is for failover and disaster recovery.   

## Amazon Aurora
   * #### Overview
     * AWS database offering in the Amazon RDS family.
     * MySQL and PostgreSQL compatiblity only.
     * 5x faster than MySQL and 3x faster than PostgreSQL.
   * #### Architecture
     * Aurora is a **regional** service. 
     * Aurora Primary sits in an AZ and serves as the primary for writes, while Aurora Replicas are spread across multple AZs to serve reads (up to 15 Replicas in a Region!).
     * Fault tolerance: Aurora is spread across 3 AZs.
     * **Single Logical Volume:** Data store volume is spread across multiple AZs in the Region.
     * Aurora has a serverless on-demand auto-scaling configuration available.
   * #### Amazon Limitless
     * A new feature that allows a single SQL DB scale into millions of transactions per second!  

## Amazon DynamoDB
  * #### Overview
    * Fully managed NoSQL database service.
    * Fully serverless.
    * Flexible data schema, key/value and document store.
    * Low latency (within milliseconds) access to data.
    * Horizontally scalable at the push of a button.
    * On-demand back-up and restore available.
  * #### Architecture 
    * Data is stored in partitions, which are replicated across multiple AZs (fault tolerant).
    * Highly available 99.99%, 99.999% for Global Tables.
    * Global Tables are a fully managed, mutli-region, and mutli-master solution that writes and reads to both Regions and the data is replicated in both tables.
    
## Amazon Redshift
  * #### Overview
    * Fully managed SQL-based data warehouse that stores transactional data loaded from Amazon RDS (OLTP).
    * Data can be analyzed using business intelligence (BI) tools (OLAP).
    * Redshift uses EC2 instances (it is not serverless).
    * Redshift always keeps three (3) copies of your data.
    * Redshift provides continous incremental backups.
   
## Amazon Elastic Map Reduce (EMR)
  * #### Overview
    * EMR provides a managed service implementing **Hadoop** - a framework for big data.
    * EMR is used for **big data** management and intelligence.
    * EMR can perform **extract, transform, and loading (ETL)** of big data.
    * EMR can perform ETL from a number of different DBs: RDS, Redshift, DynamoDB, S3, S3 Glacier (**Yes - S3 can also be used as a data lake to store data!**), Hadoop, Spark, etc. 

## Amazon ElastiCache
  * #### Overview
    * Fully managed service for open-source DBs: **Redis** and **Memcached**.
    * ElastiCache is a key-value pair store.
    * ElastiCache is a high performance, low latency in-memory (RAM) DB store.
  * #### Architecture
    * ElastiCache is put "in front" of the database.
    * It works with EC2 instances, so **not** serverless.
    * The EC2 instance writes to RDS --> ElastiCache is in-between the EC2 instance and RDS --> RDS caches data into ElastiCache --> When a read request comes in from the EC2 instance ("cache hit"), it reads from ElastiCache instead of RDS.  

## Amazon MemoryDB for Redis
  * #### MemoryDB for Redis vs. ElastiCache
    * The key thing to remember here is one is **ephemeral** in-line memory (ElastiCache) and the other is **durable** (MemoryDB for Redis).
    * **Amazon ElastiCache:** Caching. This is for ephemeral data. If the server loses power, the data is gone. ElastiCache is primarily used to speed up RDS (by handling cache hits from the attached EC2 instance).
    * **Amazon Memory DB for Redis:** Primary Database. This is for durable data. It stores everything in RAM for speed, but it also writes to a distributed transaction log across multiple AZs. 

## Amazon Athena
  * #### Overview
    * Athena can query data using SQL in data lakes like S3.
    * Athena can also query data to other sources (such as AWS CloudTrail) via a Lambda function.
    * Athena uses a managed data catalog (like AWS Glue) to store information and schemas about databases.
    * Data can be CSV, JSON, ORC, TSV, or Parquet.
  * #### Optimizing Athena
    * Partition your data
    * Bucket your data
    * Optimize file sizes

## AWS Glue
  * #### Overview
    * Fully managed ETL service.
    * Used for preparing data for analytics (OLAP).
    * AWS Glue runs ETL on Apache Spark.
    * AWS Glue discovers data and stores the associated metadata in the **AWS Glue Data Catalog** (which can then be leveraged by services like Amazon Athena when running queries).
    * Works with data lakes (e.g. S3), data warehouses (e.g. Redshift), and data stores (e.g. RDS).
   
## Amazon Kinesis
  * #### Overview
    * Kinesis is associated with **streaming data** (such as IoT sensors, gaming, stock tickers, geospatical data, etc.).
    * Kinesis ingests data into data streams and then feeds into other Kinesis services.
  * #### Amazon Kinesis Data Analytics  
    * Data producers send data into shards for up to 7 days.
    * Consumers process the data and store in another database.
    * Lambda functions can store the data into other data stores or services such as DynamoDB.
  * #### Amazon Kinesis Firehose
    * No shards, completely automated and easily scalable.
    * Saves data directly into another service like S3 or Redshift.

## Amazon OpenSearch Service
  * #### Overview
    * Also known as **ElasticSearch** (you may see "ElasticSearch" and "OpenSearch" used interchangeably).
    * Fully managed service for searching, visualize, and analyzing text as well as unstructured data (search and analytics suite).
    * Petabyte scale and secure, deployable to VPC, and integrates with IAM.
    * Supports SQL syntax queries.
    * Integrates with open-tools.
    * Ultrawarm and cold storage available.
    * Enables encryption at rest and transit.
  * #### Architecture
    * Deployed in Amazon VPC as clusters allowing for secure intra-VPC communications.
        * NOTE: Limitations of deploying in VPCs:
            * Cannot launch in a VPC that has tenancy (meaning you cannot launch this service on **Dedicated Hosts** and **Dedicated Instances**).
            * Because OpenSearch is a managed service, AWS handles the provisioning of the underlying servers. Since you don't have control over the physical placement of those specific "managed" servers, they can't be pinned to your specific Dedicated Host hardware.  
    * VPN connection required if connecting to public internet.
    * Nodes and replicas are deployable across multiple AZs making it HA and scalable.
    * Not serverless - uses EC2 instances and scales in and out.
    * Backs up using EBS snapshots.
    * Deployed as clusters which are managed by APIs, CLI, or via AWS Management Console.
    * OpenSearch ingests data from different sources, which can include Amazon Kinesis Firehose, LogStash, or ElasticSearch Open API.
    * Data ingested into OpenSearch can then be searched and/or analyzed by pointing open source tools like Kibana Dashboard at OpenSearch.
  * #### The ELK Stack
    * "ELK" = ElasticSearch + Logstash + Kibana (essentially the architecture of OpenSearch).
    * Logstash (Ingest) --> ElasticSearch (Load) <-- Kibana Dashboard (Analyze)
    * The ELK Stack is used to aggregate logs from systems and applications to create visualizations for:
        * Troubleshooting
        * Visualize app and/or infrastructure monitoring data
        * Security analytics
  * #### Access Control
    * Resource-Based policies (domain access policies)
    * Identity-Based policies (IAM permissions and roles)
    * IP-Based policies (restrict access to one or more IPs or entire CIDR blocks)
    * Fine-Grained access control (role-based access control, HTTP access control, and other security policies)
    * Authentication-Based control
  * #### Best Practices     
    * Deploy across three (3) AZs for high availability.
    * Use three (3) dedicated nodes.
    * Configure at least one replica for each index.
    * Apply restrictive resource-based policies for access control (or use fine-grained access control).
    * Enable node-to-node (transit) encryption and encryption at rest.

## AWS Data Exchange
  * #### Overview
    * AWS Data Exchange is a platform that allows you to securely use third-party data and data products.
    * AWS Data Exchange provides access to 3,500+ data sets from 300+ third-party providers.
  * #### Architecture
    * Data Producers and Providers publish their data sets to the AWS Data Exchange and AWS Cloud customers can subscribe to these data sets/providers.
        * 3rd Party Data Set/Provider/Producer --> AWS Data Exchange <-- AWS User subs to their choice of set/provider via a Lambda function that calls the API endpoint of the 3rd party data set/provider.
  * #### Use Cases
    * **Business Intelligence:** enhances BI and analytics.
    * **Machine Learning:** create more effective ML models.
    * **AWS Data Exchange for AWS Lake Formation:** subscribe directly to the 3rd party's S3 data lake.
    * **Data APIs:** leverages AWS IAM credentialing and AWS SDKs to call data from hundreds of 3rd party providers.
    * **Data Files:** automatically export new or updated data to your S3 buckets.
    * **Data Tables:** discover and subscribe to 3rd party data and directly query the data in Amazon Redshift.   

## Amazon Managed Streaming Service for Apache Kafka (MSK)
  * #### Overview
    * Amazon MSK is a fully managed service that allows you to build and runs apps that use Apache Kafka to process streaming data.
    * Amazon MSK is used for ingesting and processing streaming data in real-time.
    * Includes Kafka clusters, broker nodes, and zookeeper nodes.
    * **NOTE:** Just remember for the exam that MSK works with Kafka - you don't need a deep dive into Kafka.
   
## Amazon Data Pipeline
  * #### Overview
    * Processes and moves data between different AWS compute and storage services.
    * Saves results to S3, RDS, DnyamoDB, and EMR.
   
## Amazon Quicksight
  * #### Overview
    * Business Intelligence (BI) service.
    * Creates and publishes interactive dashboards for ML-powered insights.

 ## Amazon Neptune
  * #### Overview
    * Fully managed graph database service.

## Amazon DocumentDB
  * #### Overview
    * Fully managed document database service (non-relational).
    * Supports MongoDB workloads.
 
## Amazon QLDB
  * #### Overview
    * Fully managed ledger database for immutable change history.

## Amazon Managed Blockchain
  * #### Overview
    * Fully managed service for joining public and private networks using Hyperledge Fabric and Ethereum.
       

