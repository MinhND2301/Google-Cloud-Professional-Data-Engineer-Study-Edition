# Google Cloud Professional Data Engineer - Study Edition

This version is optimized for fast review and memorization.

- Read `Memorize` first, then check `Quick reason`.
- Open `Full explanation` only when you need depth.

**Total questions:** 278

---

## Q1

**Prompt:** Your company built a TensorFlow neutral-network model with a large number of neurons and layers. The model fits well for the training data. However, when tested against new data, it performs poorly. What method can you employ to address this?

**Options:**
- A) Threading
- B) Serialization
- C) Dropout Methods
- D) Dimensionality Reduction

**Memorize:** `C) Dropout Methods`

<details>
<summary>Full explanation</summary>

Answer is 
Dropout Methods

There are various ways to prevent overfitting when dealing with DNNs. In this post, we’ll review these techniques and then apply them specifically to TensorFlow models:

- Early Stopping

- L1 and L2 Regularization

- Dropout

- Max-Norm Regularization

- Data Augmentation

D is not correct here because it is used for normal ml models whereas dropout methods is used for neural networks.

Reference:

https://medium.com/mlreview/a-simple-deep-learning-model-for-stock-price-prediction-using-tensorflow-30505541d877

</details>

---

## Q2

**Prompt:** An external customer provides you with a daily dump of data from their database. The data flows into Google Cloud Storage GCS as comma-separated values (CSV) files. You want to analyze this data in Google BigQuery, but the data could have rows that are formatted incorrectly or corrupted. How should you build this pipeline?

**Options:**
- A) Use federated data sources, and check data in the SQL query.
- B) Enable BigQuery monitoring in Google Stackdriver and create an alert.
- C) Import the data into BigQuery using the gcloud CLI and set max_bad_records to 0.
- D) Run a Google Cloud Dataflow batch pipeline to import the data into BigQuery, and push errors to another dead-letter table for analysis.

**Memorize:** `D) Run a Google Cloud Dataflow batch pipeline to import the data into BigQuery, and push errors to another dead-letter table for analysis.`

<details>
<summary>Full explanation</summary>

Answer is 
Run a Google Cloud Dataflow batch pipeline to import the data into BigQuery, and push errors to another dead-letter table for analysis.

A. Use federated data sources, and check data in the SQL query. - WRONG (Because we are changing source itself, i.e. SQL, MySQL, PstgresSQL) instead of correcting the problem

B. Enable BigQuery monitoring in Google Stackdriver and create an alert. (WRONG - Because setting and creating an alert will not solve the corrupted data problem)

C. Import the data into BigQuery using the gcloud CLI and set max_bad_records to 0. (wrong - here we are saying set max_bad_records = 0 (i.e let's load all bad records into bi-query)

D. Run a Google Cloud Dataflow batch pipeline to import the data into BigQuery, and push errors to another dead-letter table for analysis. (CORRECT - Dataflow is used for this pupose only i.e transform the data and dead letter queue pupose is to write any invalid records - so that it can be analyzed later (rather than ignoring))

Reference:

https://cloud.google.com/blog/products/gcp/handling-invalid-inputs-in-dataflow

</details>

---

## Q3

**Prompt:** Your weather app queries a database every 15 minutes to get the current temperature. The frontend is powered by Google App Engine and server millions of users. How should you design the frontend to respond to a database failure?

**Options:**
- A) Issue a command to restart the database servers.
- B) Retry the query with exponential backoff, up to a cap of 15 minutes.
- C) Retry the query every second until it comes back online to minimize staleness of data.
- D) Reduce the query frequency to once every hour until the database comes back online.

**Memorize:** `B) Retry the query with exponential backoff, up to a cap of 15 minutes.`

<details>
<summary>Full explanation</summary>

Answer is 
Retry the query with exponential backoff, up to a cap of 15 minutes.

App engine create applications that use Cloud SQL database connections effectively. Below is what is written in google cloud documnetation.

If your application attempts to connect to the database and does not succeed, the database could be temporarily unavailable. In this case, sending too many simultaneous connection requests might waste additional database resources and increase the time needed to recover. Using exponential backoff prevents your application from sending an unresponsive number of connection requests when it can't connect to the database.

This retry only makes sense when first connecting, or when first grabbing a connection from the pool. If errors happen in the middle of a transaction, the application must do the retrying, and it must retry from the beginning of a transaction. So even if your pool is configured properly, the application might still see errors if connections are lost.

Reference:

https://cloud.google.com/sql/docs/mysql/manage-connections#backoff

</details>

---

## Q4

**Prompt:** You are building new real-time data warehouse for your company and will use Google BigQuery streaming inserts. There is no guarantee that data will only be sent in once but you do have a unique ID for each row of data and an event timestamp. You want to ensure that duplicates are not included while interactively querying data. Which query type should you use?

**Options:**
- A) Include ORDER BY DESK on timestamp column and LIMIT to 1.
- B) Use GROUP BY on the unique ID column and timestamp column and SUM on the values.
- C) Use the LAG window function with PARTITION by unique ID along with WHERE LAG IS NOT NULL.
- D) Use the ROW_NUMBER window function with PARTITION by unique ID along with WHERE row equals 1.

**Memorize:** `D) Use the ROW_NUMBER window function with PARTITION by unique ID along with WHERE row equals 1.`

<details>
<summary>Full explanation</summary>

Answer is 
Use the ROW_NUMBER window function with PARTITION by unique ID along with WHERE row equals 1.

Row Number equals 1 with partitioning will ensure only one record is fetched per partition

Reference:

https://www.youtube.com/watch?v=ysArdMImULo&list=PLQMsfKRZZviSLraRoqXulcMKFvIXQkHdA&index=3

</details>

---

## Q5

**Prompt:** You are designing a basket abandonment system for an ecommerce company. The system will send a message to a user based on these rules: • No interaction by the user on the site for 1 hour • Has added more than $30 worth of products to the basket • Has not completed a transaction You use Google Cloud Dataflow to process the data and decide if a message should be sent. How should you design the pipeline?

**Options:**
- A) Use a fixed-time window with a duration of 60 minutes.
- B) Use a sliding time window with a duration of 60 minutes.
- C) Use a session window with a gap time duration of 60 minutes.
- D) Use a global window with a time based trigger with a delay of 60 minutes.

**Memorize:** `C) Use a session window with a gap time duration of 60 minutes.`

<details>
<summary>Full explanation</summary>

Answer is 

There are 3 windowing concepts in dataflow and each can be used for below use case

1) Fixed window

2) Sliding window and

3) Session window.

Fixed window = any aggregation use cases, any batch analysis of data, relatively simple use cases.

Sliding window = Moving averages of data

Session window = user session data, click data and real time gaming analysis.

The question here is about user session data and hence session window.

Reference:

https://cloud.google.com/dataflow/docs/concepts/streaming-pipelines

</details>

---

## Q6

**Prompt:** Your company is migrating their 30-node Apache Hadoop cluster to the cloud. They want to re-use Hadoop jobs they have already created and minimize the management of the cluster as much as possible. They also want to be able to persist data beyond the life of the cluster. What should you do?

**Options:**
- A) Create a Google Cloud Dataflow job to process the data.
- B) Create a Google Cloud Dataproc cluster that uses persistent disks for HDFS.
- C) Create a Hadoop cluster on Google Compute Engine that uses persistent disks.
- D) Create a Cloud Dataproc cluster that uses the Google Cloud Storage connector.
- E) Create a Hadoop cluster on Google Compute Engine that uses Local SSD disks.

**Memorize:** `D) Create a Cloud Dataproc cluster that uses the Google Cloud Storage connector.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Cloud Dataproc cluster that uses the Google Cloud Storage connector.

Dataproc is used to migrate Hadoop and Spark jobs on GCP. Dataproc with GCS connected through Google Cloud Storage connector helps store data after the life of the cluster. When the job is high I/O intensive, then we need to create a small persistent disk.

</details>

---

## Q7

**Prompt:** Your company's on-premises Apache Hadoop servers are approaching end-of-life, and IT has decided to migrate the cluster to Google Cloud Dataproc. A like-for-like migration of the cluster would require 50 TB of Google Persistent Disk per node. The CIO is concerned about the cost of using that much block storage. You want to minimize the storage cost of the migration. What should you do?

**Options:**
- A) Put the data into Google Cloud Storage.
- B) Use preemptible virtual machines (VMs) for the Cloud Dataproc cluster.
- C) Tune the Cloud Dataproc cluster so that there is just enough disk for all data.
- D) Migrate some of the cold data into Google Cloud Storage, and keep only the hot data in Persistent Disk.

**Memorize:** `A) Put the data into Google Cloud Storage.`

<details>
<summary>Full explanation</summary>

Answer is 
Put the data into Google Cloud Storage.

A is correct because Google recommends using Cloud Storage instead of HDFS as it is much more cost effective especially when jobs aren't running.

B is not correct because this will decrease the compute cost but not the storage cost.

C is not correct because while this will reduce cost somewhat, it will not be as cost effective as using Cloud Storage.

D is not correct because while this will reduce cost somewhat, it will not be as cost effective as using Cloud Storage.

</details>

---

## Q8

**Prompt:** You are deploying 10,000 new Internet of Things devices to collect temperature data in your warehouses globally. You need to process, store and analyze these very large datasets in real time. What should you do?

**Options:**
- A) Send the data to Google Cloud Datastore and then export to BigQuery.
- B) Send the data to Google Cloud Pub/Sub, stream Cloud Pub/Sub to Google Cloud Dataflow, and store the data in Google BigQuery.
- C) Send the data to Cloud Storage and then spin up an Apache Hadoop cluster as needed in Google Cloud Dataproc whenever analysis is required.
- D) Export logs in batch to Google Cloud Storage and then spin up a Google Cloud SQL instance, import the data from Cloud Storage, and run an analysis as needed.

**Memorize:** `B) Send the data to Google Cloud Pub/Sub, stream Cloud Pub/Sub to Google Cloud Dataflow, and store the data in Google BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 
Send the data to Google Cloud Pub/Sub, stream Cloud Pub/Sub to Google Cloud Dataflow, and store the data in Google BigQuery.

You can use cloud data flow for both batch and streaming pipelines. Bigquery for analytics. Pub sub will be used to stream data into cloud data flow.

</details>

---

## Q9

**Prompt:** You are working on a sensitive project involving private user data. You have set up a project on Google Cloud Platform to house your work internally. An external consultant is going to assist with coding a complex transformation in a Google Cloud Dataflow pipeline for your project. How should you maintain users' privacy?

**Options:**
- A) Grant the consultant the Viewer role on the project.
- B) Grant the consultant the Cloud Dataflow Developer role on the project.
- C) Create a service account and allow the consultant to log on with it.
- D) Create an anonymized sample of the data for the consultant to work with in a different project.Most Voted

**Memorize:** `B) Grant the consultant the Cloud Dataflow Developer role on the project.`

<details>
<summary>Full explanation</summary>

Answer is 
Grant the consultant the Cloud Dataflow Developer role on the project.

The Dataflow developer role will not provide access to the underlying data.

Reference:

https://cloud.google.com/dataflow/docs/concepts/access-control#example_role_assignment

</details>

---

## Q10

**Prompt:** You work for an economic consulting firm that helps companies identify economic trends as they happen. As part of your analysis, you use Google BigQuery to correlate customer data with the average prices of the 100 most common goods sold, including bread, gasoline, milk, and others. The average prices of these goods are updated every 30 minutes. You want to make sure this data stays up to date so you can combine it with other data in BigQuery as cheaply as possible. What should you do?

**Options:**
- A) Load the data every 30 minutes into a new partitioned table in BigQuery.Most Voted
- B) Store and update the data in a regional Google Cloud Storage bucket and create a federated data source in BigQuery
- C) Store the data in Google Cloud Datastore. Use Google Cloud Dataflow to query BigQuery and combine the data programmatically with the data stored in Cloud Datastore
- D) Store the data in a file in a regional Google Cloud Storage bucket. Use Cloud Dataflow to query BigQuery and combine the data programmatically with the data stored in Google Cloud Storage.

**Memorize:** `B) Store and update the data in a regional Google Cloud Storage bucket and create a federated data source in BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
Store and update the data in a regional Google Cloud Storage bucket and create a federated data source in BigQuery

Use cases for external data sources include:

Loading and cleaning your data in one pass by querying the data from an external data source (a location external to BigQuery) and writing the cleaned result into BigQuery storage.

Having a small amount of frequently changing data that you join with other tables. As an external data source, the frequently changing data does not need to be reloaded every time it is updated.

Reference:

https://cloud.google.com/bigquery/external-data-sources

</details>

---

## Q11

**Prompt:** You are designing the database schema for a machine learning-based food ordering service that will predict what users want to eat. Here is some of the information you need to store: - The user profile: What the user likes and doesn't like to eat - The user account information: Name, address, preferred meal times - The order information: When orders are made, from where, to whom The database will be used to store all the transactional data of the product. You want to optimize the data schema. Which Google Cloud Platform product should you use?

**Options:**
- A) BigQueryMost Voted
- B) Cloud SQL
- C) Cloud Bigtable
- D) Cloud Datastore

**Memorize:** `B) Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud SQL

The database will be used to store all the transactional data of the product.

what we need is the database only for store transactional data, not for analysis and ML.

so the answer should be "the database that stores transactional data", which means, Cloud SQL.

if you want to analyze or do ML you just specify Cloud SQL as a federated data source.

A: it's good for analysis but it costs too much to input/output data frequently.

C: BigTable is not good for transactional data.

D: okay datastore supports transactions, but it is weaker than RDB, and also, in this case, the data schema has already defined , you should use RDB.

</details>

---

## Q12

**Prompt:** Your company produces 20,000 files every hour. Each data file is formatted as a comma separated values (CSV) file that is less than 4 KB. All files must be ingested on Google Cloud Platform before they can be processed. Your company site has a 200 ms latency to Google Cloud, and your Internet connection bandwidth is limited as 50 Mbps. You currently deploy a secure FTP (SFTP) server on a virtual machine in Google Compute Engine as the data ingestion point. A local SFTP client runs on a dedicated machine to transmit the CSV files as is. The goal is to make reports with data from the previous day available to the executives by 10:00 a.m. each day. This design is barely able to keep up with the current volume, even though the bandwidth utilization is rather low. You are told that due to seasonality, your company expects the number of files to double for the next three months. Which two actions should you take? (Choose two.)

**Options:**
- A) Introduce data compression for each file to increase the rate file of file transfer.
- B) Contact your internet service provider (ISP) to increase your maximum bandwidth to at least 100 Mbps.
- C) Redesign the data ingestion process to use gsutil tool to send the CSV files to a storage bucket in parallel.
- D) Assemble 1,000 files into a tape archive (TAR) file. Transmit the TAR files instead, and disassemble the CSV files in the cloud upon receiving them.
- E) Create an S3-compatible storage endpoint in your network, and use Google Cloud Storage Transfer Service to transfer on-premises data to the designated storage bucket.

**Memorize:** `C) Redesign the data ingestion process to use gsutil tool to send the CSV files to a storage bucket in parallel.`

<details>
<summary>Full explanation</summary>

Answers are;

A. Introduce data compression for each file to increase the rate file of file transfer.

C. Redesign the data ingestion process to use gsutil tool to send the CSV files to a storage bucket in parallel.

B - wrong (we need to provide solution without changing internet speed)

D - if we TAR 1000 files, its okay, but Volume is getting increased continously..How we define the number ?

E - Bandwidth already low, so storage Transfer service will not help here.

Follow these rules of thumb when deciding whether to use gsutil or Storage Transfer Service:

Transfer scenario Recommendation

Transferring from another cloud storage provider Use Storage Transfer Service.

Transferring less than 1 TB from on-premises Use gsutil.

Transferring more than 1 TB from on-premises Use Transfer service for on-premises data.

Transferring less than 1 TB from another Cloud Storage region Use gsutil.

Transferring more than 1 TB from another Cloud Storage region Use Storage Transfer Service.

Reference:

https://cloud.google.com/storage-transfer/docs/overview#gsutil

</details>

---

## Q13

**Prompt:** You are choosing a NoSQL database to handle telemetry data submitted from millions of Internet-of-Things (IoT) devices. The volume of data is growing at 100 TB per year, and each data entry has about 100 attributes. The data processing pipeline does not require atomicity, consistency, isolation, and durability (ACID). However, high availability and low latency are required. You need to analyze the data by querying against individual fields. Which three databases meet your requirements? (Choose three.)

**Options:**
- A) Redis
- B) HBase
- C) MySQL
- D) MongoDB
- E) Cassandra
- F) F. HDFS with Hive

**Memorize:** `E) Cassandra`

<details>
<summary>Full explanation</summary>

Answer is 
HBase, D. MongoDB, E. Cassandra

A. Redis - Redis is an in-memory non-relational key-value store. Redis is a great choice for implementing a highly available in-memory cache to decrease data access latency, increase throughput, and ease the load off your relational or NoSQL database and application. Since the question does not ask cache, A is discarded.

B. HBase - Meets reqs

C. MySQL - they do not need ACID, so not needed.

D. MongoDB - Meets reqs

E. Cassandra - Apache Cassandra is an open source NoSQL distributed database trusted by thousands of companies for scalability and high availability without compromising performance. Linear scalability and proven fault-tolerance on commodity hardware or cloud infrastructure make it the perfect platform for mission-critical data.

F. HDFS with Hive - Hive allows users to read, write, and manage petabytes of data using SQL. Hive is built on top of Apache Hadoop, which is an open-source framework used to efficiently store and process large datasets. As a result, Hive is closely integrated with Hadoop, and is designed to work quickly on petabytes of data. HIVE IS NOT A DATABSE.

</details>

---

## Q14

**Prompt:** You are using Google BigQuery as your data warehouse. Your users report that the following simple query is running very slowly, no matter when they run the query: SELECT country, state, city FROM [myproject:mydataset.mytable] GROUP BY country You check the query plan for the query and see the following output in the Read section of Stage:1: What is the most likely cause of the delay for this query?

**Options:**
- A) Users are running too many concurrent queries in the system
- B) The [myproject:mydataset.mytable] table has too many partitions
- C) Either the state or the city columns in the [myproject:mydataset.mytable] table have too many NULL values
- D) Most rows in the [myproject:mydataset.mytable] table have the same value in the country column, causing data skew

**Memorize:** `D) Most rows in the [myproject:mydataset.mytable] table have the same value in the country column, causing data skew`

<details>
<summary>Full explanation</summary>

Answer is 
Most rows in the [myproject:mydataset.mytable] table have the same value in the country column, causing data skew

Purple is reading, Blue is writing. so majority is reading.

Partition skew, sometimes called data skew, is when data is partitioned into very unequally sized partitions. This creates an imbalance in the amount of data sent between slots. You can't share partitions between slots, so if one partition is especially large, it can slow down, or even crash the slot that processes the oversized partition.

Reference:

https://cloud.google.com/bigquery/docs/best-practices-performance-patterns

</details>

---

## Q15

**Prompt:** Your globally distributed auction application allows users to bid on items. Occasionally, users place identical bids at nearly identical times, and different application servers process those bids. Each bid event contains the item, amount, user, and timestamp. You want to collate those bid events into a single location in real time to determine which user bid first. What should you do?

**Options:**
- A) Create a file on a shared file and have the application servers write all bid events to that file. Process the file with Apache Hadoop to identify which user bid first.
- B) Have each application server write the bid events to Cloud Pub/Sub as they occur. Push the events from Cloud Pub/Sub to a custom endpoint that writes the bid event information into Cloud SQL.Most Voted
- C) Set up a MySQL database for each application server to write bid events into. Periodically query each of those distributed MySQL databases and update a master MySQL database with bid event information.
- D) Have each application server write the bid events to Google Cloud Pub/Sub as they occur. Use a pull subscription to pull the bid events using Google Cloud Dataflow. Give the bid for each item to the user in the bid event that is processed first.

**Memorize:** `D) Have each application server write the bid events to Google Cloud Pub/Sub as they occur. Use a pull subscription to pull the bid events using Google Cloud Dataflow. Give the bid for each item to the user in the bid event that is processed first.`

<details>
<summary>Full explanation</summary>

Answer is 
Have each application server write the bid events to Google Cloud Pub/Sub as they occur. Use a pull subscription to pull the bid events using Google Cloud Dataflow. Give the bid for each item to the user in the bid event that is processed first.

The need is to collate the messages in real-time. We need to de-dupe the messages based on timestamp of when the event occurred. This can be done by publishing ot Pub-Sub and consuming via Dataflow.

Reference:

https://stackoverflow.com/questions/62997414/push-vs-pull-for-gcp-dataflow

</details>

---

## Q16

**Prompt:** Your organization has been collecting and analyzing data in Google BigQuery for 6 months. The majority of the data analyzed is placed in a time-partitioned table named events_partitioned. To reduce the cost of queries, your organization created a view called events, which queries only the last 14 days of data. The view is described in legacy SQL. Next month, existing applications will be connecting to BigQuery to read the events data via an ODBC connection. You need to ensure the applications can connect. Which two actions should you take? (Choose two.)

**Options:**
- A) Create a new view over events using standard SQL
- B) Create a new partitioned table using a standard SQL query
- C) Create a new view over events_partitioned using standard SQL
- D) Create a service account for the ODBC connection to use for authentication
- E) Create a Google Cloud Identity and Access Management (Cloud IAM) role for the ODBC connection and shared "events"

**Memorize:** `D) Create a service account for the ODBC connection to use for authentication`

<details>
<summary>Full explanation</summary>

Answers are;

C. Create a new view over events_partitioned using standard SQL

D. Create a service account for the ODBC connection to use for authentication

C = A standard SQL query cannot reference a view defined using legacy SQL syntax.

D = For the ODBC drivers is needed a service account which will get a standard Bigquery role.

Reference:

https://cloud.google.com/bigquery/docs/reference/standard-sql/migrating-from-legacy-sql#logical_views

</details>

---

## Q17

**Prompt:** Your analytics team wants to build a simple statistical model to determine which customers are most likely to work with your company again, based on a few different metrics. They want to run the model on Apache Spark, using data housed in Google Cloud Storage, and you have recommended using Google Cloud Dataproc to execute this job. Testing has shown that this workload can run in approximately 30 minutes on a 15-node cluster, outputting the results into Google BigQuery. The plan is to run this workload weekly. How should you optimize the cluster for cost?

**Options:**
- A) Migrate the workload to Google Cloud Dataflow
- B) Use pre-emptible virtual machines (VMs) for the cluster
- C) Use a higher-memory node so that the job runs faster
- D) Use SSDs on the worker nodes so that the job can run faster

**Memorize:** `B) Use pre-emptible virtual machines (VMs) for the cluster`

<details>
<summary>Full explanation</summary>

Answer is 
Use pre-emptible virtual machines (VMs) for the cluster

Hadoop/Spark jobs are run on Dataproc, and the pre-emptible machines cost 80% less

Reference:

https://cloud.google.com/dataproc/docs/concepts/compute/preemptible-vms

</details>

---

## Q18

**Prompt:** Your infrastructure includes a set of YouTube channels. You have been tasked with creating a process for sending the YouTube channel data to Google Cloud for analysis. You want to design a solution that allows your world-wide marketing teams to perform ANSI SQL and other types of analysis on up-to-date YouTube channels log data. How should you set up the log data transfer into Google Cloud?

**Options:**
- A) Use Storage Transfer Service to transfer the offsite backup files to a Cloud Storage Multi-Regional storage bucket as a final destination.
- B) Use Storage Transfer Service to transfer the offsite backup files to a Cloud Storage Regional bucket as a final destination.
- C) Use BigQuery Data Transfer Service to transfer the offsite backup files to a Cloud Storage Multi-Regional storage bucket as a final destination.Most Voted
- D) Use BigQuery Data Transfer Service to transfer the offsite backup files to a Cloud Storage Regional storage bucket as a final destination.

**Memorize:** `A) Use Storage Transfer Service to transfer the offsite backup files to a Cloud Storage Multi-Regional storage bucket as a final destination.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Storage Transfer Service to transfer the offsite backup files to a Cloud Storage Multi-Regional storage bucket as a final destination.

Destination is GCS and having multi-regional so A is the best option available.

Even since BigQuery Data Transfer Service initially supports Google application sources like Google Ads, Campaign Manager, Google Ad Manager and YouTube but it does not support destination anything other than bq data set

</details>

---

## Q19

**Prompt:** You are designing storage for very large text files for a data pipeline on Google Cloud. You want to support ANSI SQL queries. You also want to support compression and parallel load from the input locations using Google recommended practices. What should you do?

**Options:**
- A) Transform text files to compressed Avro using Cloud Dataflow. Use BigQuery for storage and query.
- B) Transform text files to compressed Avro using Cloud Dataflow. Use Cloud Storage and BigQuery permanent linked tables for query.
- C) Compress text files to gzip using the Grid Computing Tools. Use BigQuery for storage and query.
- D) Compress text files to gzip using the Grid Computing Tools. Use Cloud Storage, and then import into Cloud Bigtable for query.

**Memorize:** `B) Transform text files to compressed Avro using Cloud Dataflow. Use Cloud Storage and BigQuery permanent linked tables for query.`

<details>
<summary>Full explanation</summary>

Answer is 
Transform text files to compressed Avro using Cloud Dataflow. Use Cloud Storage and BigQuery permanent linked tables for query.

A and B are correct, but B is the best answer

The advantages of creating external tables are that they are fast to create so you skip the part of importing data and no additional monthly billing storage costs are accrued to your account since you only get charged for the data that is stored in the data lake, which is comparatively cheaper than storing it in BigQuery

A : Importing data into BigQuery may take more time compared to creating external tables on data. Additional storage costs by BigQuery is another issue which can be more expensive than Google Storage.

</details>

---

## Q20

**Prompt:** You are designing storage for 20 TB of text files as part of deploying a data pipeline on Google Cloud. Your input data is in CSV format. You want to minimize the cost of querying aggregate values for multiple users who will query the data in Cloud Storage with multiple engines. Which storage service and schema design should you use?

**Options:**
- A) Use Cloud Bigtable for storage. Install the HBase shell on a Compute Engine instance to query the Cloud Bigtable data.
- B) Use Cloud Bigtable for storage. Link as permanent tables in BigQuery for query.
- C) Use Cloud Storage for storage. Link as permanent tables in BigQuery for query.
- D) Use Cloud Storage for storage. Link as temporary tables in BigQuery for query.

**Memorize:** `C) Use Cloud Storage for storage. Link as permanent tables in BigQuery for query.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Storage for storage. Link as permanent tables in BigQuery for query.

BigQuery can access data in external sources, known as federated sources. Instead of first

loading data into BigQuery, you can create a reference to an external source. External

sources can be Cloud Bigtable, Cloud Storage, and Google Drive.

When accessing external data, you can create either permanent or temporary external

tables. Permanent tables are those that are created in a dataset and linked to an external

source. Dataset-level access controls can be applied to these tables. When you are using a

temporary table, a table is created in a special dataset and will be available for approxi-

mately 24 hours. Temporary tables are useful for one-time operations, such as loading data

into a data warehouse.

</details>

---

## Q21

**Prompt:** You are designing storage for two relational tables that are part of a 10-TB database on Google Cloud. You want to support transactions that scale horizontally. You also want to optimize data for range queries on non-key columns. What should you do?

**Options:**
- A) Use Cloud SQL for storage. Add secondary indexes to support query patterns.
- B) Use Cloud SQL for storage. Use Cloud Dataflow to transform data to support query patterns.
- C) Use Cloud Spanner for storage. Add secondary indexes to support query patterns.
- D) Use Cloud Spanner for storage. Use Cloud Dataflow to transform data to support query patterns.

**Memorize:** `C) Use Cloud Spanner for storage. Add secondary indexes to support query patterns.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Spanner for storage. Add secondary indexes to support query patterns.

Spanner allows transaction tables to scale horizontally and secondary indexes for range queries

Reference:

https://cloud.google.com/spanner/docs/secondary-indexes

</details>

---

## Q22

**Prompt:** Your financial services company is moving to cloud technology and wants to store 50 TB of financial time-series data in the cloud. This data is updated frequently and new data will be streaming in all the time. Your company also wants to move their existing Apache Hadoop jobs to the cloud to get insights into this data. Which product should they use to store the data?

**Options:**
- A) Cloud Bigtable
- B) Google BigQuery
- C) Google Cloud Storage
- D) Google Cloud Datastore

**Memorize:** `A) Cloud Bigtable`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Bigtable

Bigtable is GCP’s managed wide-column database. It is also a good option for migrat-

ing on-premises Hadoop HBase databases to a managed database because Bigtable has

an HBase interface.

Cloud Bigtable is a wide-column NoSQL database used for high-volume databases that

require low millisecond (ms) latency. Cloud Bigtable is used for IoT, time-series, finance,

and similar applications.

Reference:

https://cloud.google.com/blog/products/databases/getting-started-with-time-series-trend-predictions-using-gcp

</details>

---

## Q23

**Prompt:** You are responsible for writing your company's ETL pipelines to run on an Apache Hadoop cluster. The pipeline will require some checkpointing and splitting pipelines. Which method should you use to write the pipelines?

**Options:**
- A) PigLatin using Pig
- B) HiveQL using Hive
- C) Java using MapReduce
- D) Python using MapReduce

**Memorize:** `A) PigLatin using Pig`

<details>
<summary>Full explanation</summary>

Answer is 
PigLatin using Pig

Pig is scripting language which can be used for checkpointing and splitting pipelines

</details>

---

## Q24

**Prompt:** You need to migrate a 2TB relational database to Google Cloud Platform. You do not have the resources to significantly refactor the application that uses this database and cost to operate is of primary concern. Which service do you select for storing and serving your data?

**Options:**
- A) Cloud Spanner
- B) Cloud Bigtable
- C) Cloud Firestore
- D) Cloud SQL

**Memorize:** `D) Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud SQL

Cloud SQL supports MySQL 5.6 or 5.7, and provides up to 624 GB of RAM and 30 TB of data storage, with the option to automatically increase the storage size as needed.

Reference:

https://cloud.google.com/sql/docs/features

</details>

---

## Q25

**Prompt:** You are designing an Apache Beam pipeline to enrich data from Cloud Pub/Sub with static reference data from BigQuery. The reference data is small enough to fit in memory on a single worker. The pipeline should write enriched results to BigQuery for analysis. Which job type and transforms should this pipeline use?

**Options:**
- A) Batch job, PubSubIO, side-inputs
- B) Streaming job, PubSubIO, JdbcIO, side-outputs
- C) Streaming job, PubSubIO, BigQueryIO, side-inputs
- D) Streaming job, PubSubIO, BigQueryIO, side-outputs

**Memorize:** `C) Streaming job, PubSubIO, BigQueryIO, side-inputs`

<details>
<summary>Full explanation</summary>

Answer is 
Streaming job, PubSubIO, BigQueryIO, side-inputs

You need pubsubIO and BigQueryIO for streaming data and writing enriched data back to BigQuery. side-inputs are a way to enrich the data

Reference:

https://cloud.google.com/architecture/e-commerce/patterns/slow-updating-side-inputs

</details>

---

## Q26

**Prompt:** You want to analyze hundreds of thousands of social media posts daily at the lowest cost and with the fewest steps. You have the following requirements: - You will batch-load the posts once per day and run them through the Cloud Natural Language API. - You will extract topics and sentiment from the posts. - You must store the raw posts for archiving and reprocessing. - You will create dashboards to be shared with people both inside and outside your organization. You need to store both the data extracted from the API to perform analysis as well as the raw social media posts for historical archiving. What should you do?

**Options:**
- A) Store the social media posts and the data extracted from the API in BigQuery.
- B) Store the social media posts and the data extracted from the API in Cloud SQL.
- C) Store the raw social media posts in Cloud Storage, and write the data extracted from the API into BigQuery.
- D) Feed to social media posts into the API directly from the source, and write the extracted data from the API into BigQuery.

**Memorize:** `C) Store the raw social media posts in Cloud Storage, and write the data extracted from the API into BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 
Store the raw social media posts in Cloud Storage, and write the data extracted from the API into BigQuery.

Social media posts can images/videos which cannot be stored in bigquery

</details>

---

## Q27

**Prompt:** You want to automate execution of a multi-step data pipeline running on Google Cloud. The pipeline includes Cloud Dataproc and Cloud Dataflow jobs that have multiple dependencies on each other. You want to use managed services where possible, and the pipeline will run every day. Which tool should you use?

**Options:**
- A) cron
- B) Cloud Composer
- C) Cloud Scheduler
- D) Workflow Templates on Cloud Dataproc

**Memorize:** `B) Cloud Composer`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Composer

Cloud Composer is an Apache Airflow managed service, it serves well when orchestrating interdependent pipelines, and Cloud Scheduler is just a managed Cron service.

Reference:

https://stackoverflow.com/questions/59841146/cloud-composer-vs-cloud-scheduler

</details>

---

## Q28

**Prompt:** You work for a shipping company that uses handheld scanners to read shipping labels. Your company has strict data privacy standards that require scanners to only transmit recipients' personally identifiable information (PII) to analytics systems, which violates user privacy rules. You want to quickly build a scalable solution using cloud-native managed services to prevent exposure of PII to the analytics systems. What should you do?

**Options:**
- A) Create an authorized view in BigQuery to restrict access to tables with sensitive data.
- B) Install a third-party data validation tool on Compute Engine virtual machines to check the incoming data for sensitive information.
- C) Use Stackdriver logging to analyze the data passed through the total pipeline to identify transactions that may contain sensitive information.
- D) Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.

**Memorize:** `D) Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.`

<details>
<summary>Full explanation</summary>

Answer is 
Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.

Protection of sensitive data, like personally identifiable information (PII), is critical to your business. Deploy de-identification in migrations, data workloads, and real-time data collection and processing.

Reference:

https://cloud.google.com/dlp

</details>

---

## Q29

**Prompt:** You are a retailer that wants to integrate your online sales capabilities with different in-home assistants, such as Google Home. You need to interpret customer voice commands and issue an order to the backend systems. Which solutions should you choose?

**Options:**
- A) Cloud Speech-to-Text APIMost Voted
- B) Cloud Natural Language API
- C) Dialogflow Enterprise Edition
- D) Cloud AutoML Natural Language

**Memorize:** `C) Dialogflow Enterprise Edition`

<details>
<summary>Full explanation</summary>

Answer is 
Dialogflow Enterprise Edition

since we need to recognize both voice and intent

Reference:

https://cloud.google.com/blog/products/gcp/introducing-dialogflow-enterprise-edition-a-new-way-to-build-voice-and-text-conversational-apps

</details>

---

## Q30

**Prompt:** You are designing a data processing pipeline. The pipeline must be able to scale automatically as load increases. Messages must be processed at least once and must be ordered within windows of 1 hour. How should you design the solution?

**Options:**
- A) Use Apache Kafka for message ingestion and use Cloud Dataproc for streaming analysis.
- B) Use Apache Kafka for message ingestion and use Cloud Dataflow for streaming analysis.
- C) Use Cloud Pub/Sub for message ingestion and Cloud Dataproc for streaming analysis.
- D) Use Cloud Pub/Sub for message ingestion and Cloud Dataflow for streaming analysis.

**Memorize:** `D) Use Cloud Pub/Sub for message ingestion and Cloud Dataflow for streaming analysis.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Pub/Sub for message ingestion and Cloud Dataflow for streaming analysis.

Dataflow has autoscaling feature and pubsub is best solution

</details>

---

## Q31

**Prompt:** You need to set access to BigQuery for different departments within your company. Your solution should comply with the following requirements: - Each department should have access only to their data. - Each department will have one or more leads who need to be able to create and update tables and provide them to their team. - Each department has data analysts who need to be able to query but not modify data. How should you set access to the data in BigQuery?

**Options:**
- A) Create a dataset for each department. Assign the department leads the role of OWNER, and assign the data analysts the role of WRITER on their dataset.
- B) Create a dataset for each department. Assign the department leads the role of WRITER, and assign the data analysts the role of READER on their dataset.
- C) Create a table for each department. Assign the department leads the role of Owner, and assign the data analysts the role of Editor on the project the table is in.
- D) Create a table for each department. Assign the department leads the role of Editor, and assign the data analysts the role of Viewer on the project the table is in.

**Memorize:** `B) Create a dataset for each department. Assign the department leads the role of WRITER, and assign the data analysts the role of READER on their dataset.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a dataset for each department. Assign the department leads the role of WRITER, and assign the data analysts the role of READER on their dataset.

The permissions are required at dataset levels hence READER, WRITER & OWNER which are the primitive roles for dataset to be used.

Reference:

https://cloud.google.com/bigquery/docs/access-control-primitive-roles#dataset-primitive-roles

</details>

---

## Q32

**Prompt:** You decided to use Cloud Datastore to ingest vehicle telemetry data in real time. You want to build a storage system that will account for the long-term data growth, while keeping the costs low. You also want to create snapshots of the data periodically, so that you can make a point-in-time (PIT) recovery, or clone a copy of the data for Cloud Datastore in a different environment. You want to archive these snapshots for a long time. Which two methods can accomplish this? (Choose two.)

**Options:**
- A) Use managed export, and store the data in a Cloud Storage bucket using Nearline or Coldline class.
- B) Use managed export, and then import to Cloud Datastore in a separate project under a unique namespace reserved for that export.
- C) Use managed export, and then import the data into a BigQuery table created just for that export, and delete temporary export files.
- D) Write an application that uses Cloud Datastore client libraries to read all the entities. Treat each entity as a BigQuery table row via BigQuery streaming insert. Assign an export timestamp for each export, and attach it as an extra column for each row. Make sure that the BigQuery table is partitioned using the export timestamp column.
- E) Write an application that uses Cloud Datastore client libraries to read all the entities. Format the exported data into a JSON file. Apply compression before storing the data in Cloud Source Repositories.

**Memorize:** `B) Use managed export, and then import to Cloud Datastore in a separate project under a unique namespace reserved for that export.`

<details>
<summary>Full explanation</summary>

Answers are;

A. Use managed export, and store the data in a Cloud Storage bucket using Nearline or Coldline class.
B. Use managed export, and then import to Cloud Datastore in a separate project under a unique namespace reserved for that export.

Option A; Cheap storage and it is a supported meathod

https://cloud.google.com/datastore/docs/export-import-entities

Option B; Data exported from one Datastore mode database can be imported into another Datastore mode database, even one in another project.

https://cloud.google.com/datastore/docs/export-import-entities

</details>

---

## Q33

**Prompt:** You are designing a cloud-native historical data processing system to meet the following conditions: - The data being analyzed is in CSV, Avro, and PDF formats and will be accessed by multiple analysis tools including Cloud Dataproc, BigQuery, and Compute Engine. - A streaming data pipeline stores new data daily. - Peformance is not a factor in the solution. - The solution design should maximize availability. How should you design data storage for this solution?

**Options:**
- A) Create a Cloud Dataproc cluster with high availability. Store the data in HDFS, and peform analysis as needed.
- B) Store the data in BigQuery. Access the data using the BigQuery Connector on Cloud Dataproc and Compute Engine.
- C) Store the data in a regional Cloud Storage bucket. Access the bucket directly using Cloud Dataproc, BigQuery, and Compute Engine.
- D) Store the data in a multi-regional Cloud Storage bucket. Access the data directly using Cloud Dataproc, BigQuery, and Compute Engine.

**Memorize:** `D) Store the data in a multi-regional Cloud Storage bucket. Access the data directly using Cloud Dataproc, BigQuery, and Compute Engine.`

<details>
<summary>Full explanation</summary>

Answer is 
Store the data in a multi-regional Cloud Storage bucket. Access the data directly using Cloud Dataproc, BigQuery, and Compute Engine.

Multi-region increases high availability and pdf can be stored in gcs

</details>

---

## Q34

**Prompt:** Your United States-based company has created an application for assessing and responding to user actions. The primary table's data volume grows by 250,000 records per second. Many third parties use your application's APIs to build the functionality into their own frontend applications. Your application's APIs should comply with the following requirements: - Single global endpoint - ANSI SQL support - Consistent access to the most up-to-date data What should you do?

**Options:**
- A) Implement BigQuery with no region selected for storage or processing.
- B) Implement Cloud Spanner with the leader in North America and read-only replicas in Asia and Europe.
- C) Implement Cloud SQL for PostgreSQL with the master in Norht America and read replicas in Asia and Europe.
- D) Implement Cloud Bigtable with the primary cluster in North America and secondary clusters in Asia and Europe.

**Memorize:** `B) Implement Cloud Spanner with the leader in North America and read-only replicas in Asia and Europe.`

<details>
<summary>Full explanation</summary>

Answer is 
Implement Cloud Spanner with the leader in North America and read-only replicas in Asia and Europe.

Cloud Spanner has three types of replicas: read-write replicas, read-only replicas, and witness replicas. Bigquery cannot support 250K data ingestion/second , as ANSI SQL support is required , no other options left except Spanner.

</details>

---

## Q35

**Prompt:** You are building an application to share financial market data with consumers, who will receive data feeds. Data is collected from the markets in real time. Consumers will receive the data in the following ways: - Real-time event stream - ANSI SQL access to real-time stream and historical data - Batch historical exports Which solution should you use?

**Options:**
- A) Cloud Dataflow, Cloud SQL, Cloud Spanner
- B) Cloud Pub/Sub, Cloud Storage, BigQuery
- C) Cloud Dataproc, Cloud Dataflow, BigQuery
- D) Cloud Pub/Sub, Cloud Dataproc, Cloud SQL

**Memorize:** `B) Cloud Pub/Sub, Cloud Storage, BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Pub/Sub, Cloud Storage, BigQuery

It says the data is collected from the market, and the problem is that the methods are defined as requirements. Therefore, a close answer is B.

Real-time Event Stream: Cloud Pub/Sub is a managed messaging service that can handle real-time event streams efficiently. You can use Pub/Sub to ingest and publish real-time market data to consumers.

ANSI SQL Access: BigQuery supports ANSI SQL queries, making it suitable for both real-time and historical data analysis. You can stream data into BigQuery tables from Pub/Sub and provide ANSI SQL access to consumers.

Batch Historical Exports: Cloud Storage can be used for batch historical exports. You can export data from BigQuery to Cloud Storage in batch, making it available for consumers to download.

Reference:

https://cloud.google.com/solutions/processing-logs-at-scale-using-dataflow?hl=ja

https://cloud.google.com/bigquery/docs/write-api#:~:text=You%20can%20use%20the%20Storage,in%20a%20single%20atomic%20operation.

</details>

---

## Q36

**Prompt:** You are building a new data pipeline to share data between two different types of applications: jobs generators and job runners. Your solution must scale to accommodate increases in usage and must accommodate the addition of new applications without negatively affecting the performance of existing ones. What should you do?

**Options:**
- A) Create an API using App Engine to receive and send messages to the applications
- B) Use a Cloud Pub/Sub topic to publish jobs, and use subscriptions to execute them
- C) Create a table on Cloud SQL, and insert and delete rows with the job information
- D) Create a table on Cloud Spanner, and insert and delete rows with the job information

**Memorize:** `B) Use a Cloud Pub/Sub topic to publish jobs, and use subscriptions to execute them`

<details>
<summary>Full explanation</summary>

Answer is 
Use a Cloud Pub/Sub topic to publish jobs, and use subscriptions to execute them

Pub/sub will be used to streaming data between application

</details>

---

## Q37

**Prompt:** You need to move 2 PB of historical data from an on-premises storage appliance to Cloud Storage within six months, and your outbound network capacity is constrained to 20 Mb/sec. How should you migrate this data to Cloud Storage?

**Options:**
- A) Use Transfer Appliance to copy the data to Cloud Storage
- B) Use gsutil cp ""J to compress the content being uploaded to Cloud Storage
- C) Create a private URL for the historical data, and then use Storage Transfer Service to copy the data to Cloud Storage
- D) Use trickle or ionice along with gsutil cp to limit the amount of bandwidth gsutil utilizes to less than 20 Mb/sec so it does not interfere with the production traffic

**Memorize:** `A) Use Transfer Appliance to copy the data to Cloud Storage`

<details>
<summary>Full explanation</summary>

Answer is 
Use Transfer Appliance to copy the data to Cloud Storage

Huge amount of data with log network bandwidth, Transfer applicate is best for moving data over 100TB

</details>

---

## Q38

**Prompt:** You receive data files in CSV format monthly from a third party. You need to cleanse this data, but every third month the schema of the files changes. Your requirements for implementing these transformations include: - Executing the transformations on a schedule - Enabling non-developer analysts to modify transformations - Providing a graphical tool for designing transformations What should you do?

**Options:**
- A) Use Cloud Dataprep to build and maintain the transformation recipes, and execute them on a scheduled basis
- B) Load each month's CSV data into BigQuery, and write a SQL query to transform the data to a standard schema. Merge the transformed tables together with a SQL query
- C) Help the analysts write a Cloud Dataflow pipeline in Python to perform the transformation. The Python code should be stored in a revision control system and modified as the incoming data's schema changes
- D) Use Apache Spark on Cloud Dataproc to infer the schema of the CSV file before creating a Dataframe. Then implement the transformations in Spark SQL before writing the data out to Cloud Storage and loading into BigQuery

**Memorize:** `A) Use Cloud Dataprep to build and maintain the transformation recipes, and execute them on a scheduled basis`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Dataprep to build and maintain the transformation recipes, and execute them on a scheduled basis

Dataprep is used by non developers

</details>

---

## Q39

**Prompt:** You work for a shipping company that has distribution centers where packages move on delivery lines to route them properly. The company wants to add cameras to the delivery lines to detect and track any visual damage to the packages in transit. You need to create a way to automate the detection of damaged packages and flag them for human review in real time while the packages are in transit. Which solution should you choose?

**Options:**
- A) Use BigQuery machine learning to be able to train the model at scale, so you can analyze the packages in batches.
- B) Train an AutoML model on your corpus of images, and build an API around that model to integrate with the package tracking applications.
- C) Use the Cloud Vision API to detect for damage, and raise an alert through Cloud Functions. Integrate the package tracking applications with this function.Most Voted
- D) Use TensorFlow to create a model that is trained on your corpus of images. Create a Python notebook in Cloud Datalab that uses this model so you can analyze for damaged packages.

**Memorize:** `B) Train an AutoML model on your corpus of images, and build an API around that model to integrate with the package tracking applications.`

<details>
<summary>Full explanation</summary>

Answer is 
 B. Train an AutoML model on your corpus of images, and build an API around that model to integrate with the package tracking applications. 

For this scenario, where you need to automate the detection of damaged packages in real time while they are in transit, the most suitable solution among the provided options would be B.

Here's why this option is the most appropriate:

Real-Time Analysis: AutoML provides the capability to train a custom model specifically tailored to recognize patterns of damage in packages. This model can process images in real-time, which is essential in your scenario.

Integration with Existing Systems: By building an API around the AutoML model, you can seamlessly integrate this solution with your existing package tracking applications. This ensures that the system can flag damaged packages for human review efficiently.

Customization and Accuracy: Since the model is trained on your specific corpus of images, it can be more accurate in detecting damages relevant to your use case compared to pre-trained models.

Let's briefly consider why the other options are less suitable:

A. Use BigQuery machine learning: BigQuery is great for handling large-scale data analytics but is not optimized for real-time image processing or complex image recognition tasks like damage detection on packages.

C. Use the Cloud Vision API: While the Cloud Vision API is powerful for general image analysis, it might not be as effective for the specific task of detecting damage on packages, which requires a more customized approach.

D. Use TensorFlow in Cloud Datalab: While this is a viable option for creating a custom model, it might be more complex and time-consuming compared to using AutoML. Additionally, setting up a real-time analysis system through a Python notebook might not be as straightforward as an API integration.

</details>

---

## Q40

**Prompt:** You operate an IoT pipeline built around Apache Kafka that normally receives around 5000 messages per second. You want to use Google Cloud Platform to create an alert as soon as the moving average over 1 hour drops below 4000 messages per second. What should you do?

**Options:**
- A) Consume the stream of data in Cloud Dataflow using Kafka IO. Set a sliding time window of 1 hour every 5 minutes. Compute the average when the window closes, and send an alert if the average is less than 4000 messages.
- B) Consume the stream of data in Cloud Dataflow using Kafka IO. Set a fixed time window of 1 hour. Compute the average when the window closes, and send an alert if the average is less than 4000 messages.
- C) Use Kafka Connect to link your Kafka message queue to Cloud Pub/Sub. Use a Cloud Dataflow template to write your messages from Cloud Pub/Sub to Cloud Bigtable. Use Cloud Scheduler to run a script every hour that counts the number of rows created in Cloud Bigtable in the last hour. If that number falls below 4000, send an alert.
- D) Use Kafka Connect to link your Kafka message queue to Cloud Pub/Sub. Use a Cloud Dataflow template to write your messages from Cloud Pub/Sub to BigQuery. Use Cloud Scheduler to run a script every five minutes that counts the number of rows created in BigQuery in the last hour. If that number falls below 4000, send an alert.

**Memorize:** `A) Consume the stream of data in Cloud Dataflow using Kafka IO. Set a sliding time window of 1 hour every 5 minutes. Compute the average when the window closes, and send an alert if the average is less than 4000 messages.`

<details>
<summary>Full explanation</summary>

Answer is 
Consume the stream of data in Cloud Dataflow using Kafka IO. Set a sliding time window of 1 hour every 5 minutes. Compute the average when the window closes, and send an alert if the average is less than 4000 messages.

Kafka IO and Dataflow is a valid option for interconnect (needless where Kafka is located - On Prem/Google Cloud/Other cloud)

Sliding Window will help to calculate average.

</details>

---

## Q41

**Prompt:** You are planning to migrate your current on-premises Apache Hadoop deployment to the cloud. You need to ensure that the deployment is as fault-tolerant and cost-effective as possible for long-running batch jobs. You want to use a managed service. What should you do?

**Options:**
- A) Deploy a Cloud Dataproc cluster. Use a standard persistent disk and 50% preemptible workers. Store data in Cloud Storage, and change references in scripts from hdfs:// to gs://
- B) Deploy a Cloud Dataproc cluster. Use an SSD persistent disk and 50% preemptible workers. Store data in Cloud Storage, and change references in scripts from hdfs:// to gs://
- C) Install Hadoop and Spark on a 10-node Compute Engine instance group with standard instances. Install the Cloud Storage connector, and store the data in Cloud Storage. Change references in scripts from hdfs:// to gs://
- D) Install Hadoop and Spark on a 10-node Compute Engine instance group with preemptible instances. Store data in HDFS. Change references in scripts from hdfs:// to gs://

**Memorize:** `A) Deploy a Cloud Dataproc cluster. Use a standard persistent disk and 50% preemptible workers. Store data in Cloud Storage, and change references in scripts from hdfs:// to gs://`

<details>
<summary>Full explanation</summary>

Answer is 
Deploy a Cloud Dataproc cluster. Use a standard persistent disk and 50% preemptible workers. Store data in Cloud Storage, and change references in scripts from hdfs:// to gs://

Cloud Dataproc for Managed Cloud native application and HDD for cost-effective solution.

</details>

---

## Q42

**Prompt:** You need to choose a database for a new project that has the following requirements: - Fully managed - Able to automatically scale up - Transactionally consistent - Able to scale up to 6 TB - Able to be queried using SQL Which database do you choose?

**Options:**
- A) Cloud SQL
- B) Cloud Bigtable
- C) Cloud SpannerMost Voted
- D) Cloud Datastore

**Memorize:** `A) Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud SQL

It asks for scaling up which can be done in cloud sql, horizontal scaling is not possible in cloud sql

Automatic storage increase

If you enable this setting, Cloud SQL checks your available storage every 30 seconds. If the available storage falls below a threshold size, Cloud SQL automatically adds additional storage capacity. If the available storage repeatedly falls below the threshold size, Cloud SQL continues to add storage until it reaches the maximum of 30 TB.

</details>

---

## Q43

**Prompt:** What are two of the benefits of using denormalized data structures in BigQuery?

**Options:**
- A) Reduces the amount of data processed, reduces the amount of storage required
- B) Increases query speed, makes queries simpler
- C) Reduces the amount of storage required, increases query speed
- D) Reduces the amount of data processed, increases query speed

**Memorize:** `B) Increases query speed, makes queries simpler`

<details>
<summary>Full explanation</summary>

Answer is 
Increases query speed, makes queries simpler

Cannot be A or C because:

"Denormalized schemas aren't storage-optimal, but BigQuery's low cost of storage addresses concerns about storage inefficiency."

Cannot be D because the amount of data processed is the same.

As for why is it "simpler", I don't see it directly stated but it is hinted at: "Expressing records by using nested and repeated fields simplifies data load using JSON or Avro files." and "Expressing records using nested and repeated structures can provide a more natural representation of the underlying data."

Reference:

https://cloud.google.com/solutions/bigquery-data-warehouse

</details>

---

## Q44

**Prompt:** Which of the following are examples of hyperparameters? (Select 2 answers.)

**Options:**
- A) Number of hidden layers
- B) Number of nodes in each hidden layer
- C) Biases
- D) Weights

**Memorize:** `B) Number of nodes in each hidden layer`

<details>
<summary>Full explanation</summary>

Answers are;

A. Number of hidden layers

B. Number of nodes in each hidden layer

Hyperparamters are configuration variables and cannot change

Reference:

https://cloud.google.com/ai-platform/training/docs/hyperparameter-tuning-overview

</details>

---

## Q45

**Prompt:** Which of the following are feature engineering techniques? (Select 2 answers)

**Options:**
- A) Hidden feature layers
- B) Feature prioritization
- C) Crossed feature columns
- D) Bucketization of a continuous feature

**Memorize:** `D) Bucketization of a continuous feature`

<details>
<summary>Full explanation</summary>

Answer are;

C. Crossed feature columns

D. Bucketization of a continuous feature

Selecting and crafting the right set of feature columns is key to learning an effective model.

Bucketization is a process of dividing the entire range of a continuous feature into a set of consecutive bins/buckets, and then converting the original numerical feature into a bucket ID (as a categorical feature) depending on which bucket that value falls into.

Using each base feature column separately may not be enough to explain the data. To learn the differences between different feature combinations, we can add crossed feature columns to the model.

Reference:

https://cloud.google.com/solutions/machine-learning/ml-on-structured-data-model-2

</details>

---

## Q46

**Prompt:** You want to use a BigQuery table as a data sink. In which writing mode(s) can you use BigQuery as a sink?

**Options:**
- A) Both batch and streaming
- B) BigQuery cannot be used as a sink
- C) Only batch
- D) Only streaming

**Memorize:** `A) Both batch and streaming`

<details>
<summary>Full explanation</summary>

Answer is 
Both batch and streaming

When you apply a BigQueryIO.Write transform in batch mode to write to a single table, Dataflow invokes a BigQuery load job. When you apply a BigQueryIO.Write transform in streaming mode or in batch mode using a function to specify the destination table, Dataflow uses BigQuery's streaming inserts.

Reference:

https://cloud.google.com/dataflow/model/bigquery-io

</details>

---

## Q47

**Prompt:** You have a job that you want to cancel. It is a streaming pipeline, and you want to ensure that any data that is in-flight is processed and written to the output. Which of the following commands can you use on the Dataflow monitoring console to stop the pipeline job?

**Options:**
- A) Cancel
- B) Drain
- C) Stop
- D) Finish

**Memorize:** `B) Drain`

<details>
<summary>Full explanation</summary>

Answer is 
Drain

Using the Drain option to stop your job tells the Dataflow service to finish your job in its current state. Your job will immediately stop ingesting new data from input sources, but the Dataflow service will preserve any existing resources (such as worker instances) to finish processing and writing any buffered data in your pipeline.

Reference:

https://cloud.google.com/dataflow/pipelines/stopping-a-pipeline

</details>

---

## Q48

**Prompt:** Which of the following statements is NOT true regarding Bigtable access roles?

**Options:**
- A) Using IAM roles, you cannot give a user access to only one table in a project, rather than all tables in a project.
- B) To give a user access to only one table in a project, grant the user the Bigtable Editor role for that table.
- C) You can configure access control only at the project level.
- D) To give a user access to only one table in a project, you must configure access through your application.

**Memorize:** `B) To give a user access to only one table in a project, grant the user the Bigtable Editor role for that table.`

<details>
<summary>Full explanation</summary>

Answer is 
To give a user access to only one table in a project, grant the user the Bigtable Editor role for that table.

For Cloud Bigtable, you can configure access control at the project level. For example, you can grant the ability to:
Read from, but not write to, any table within the project.
Read from and write to any table within the project, but not manage instances.
Read from and write to any table within the project, and manage instances.

Reference:

https://cloud.google.com/bigtable/docs/access-control

</details>

---

## Q49

**Prompt:** What is the general recommendation when designing your row keys for a Cloud Bigtable schema?

**Options:**
- A) Include multiple time series values within the row key
- B) Keep the row keep as an 8 bit integer
- C) Keep your row key reasonably short
- D) Keep your row key as long as the field permits

**Memorize:** `C) Keep your row key reasonably short`

<details>
<summary>Full explanation</summary>

Answer is 

A general guide is to, keep your row keys reasonably short. Long row keys take up additional memory and storage and increase the time it takes to get responses from the Cloud Bigtable server.

Reference:

https://cloud.google.com/bigtable/docs/schema-design#row-keys

</details>

---

## Q50

**Prompt:** All Google Cloud Bigtable client requests go through a front-end server ______ they are sent to a Cloud Bigtable node.

**Options:**
- A) before
- B) after
- C) only if
- D) once

**Memorize:** `A) before`

<details>
<summary>Full explanation</summary>

Answer is 
before

In a Cloud Bigtable architecture all client requests go through a front-end server before they are sent to a Cloud Bigtable node.
The nodes are organized into a Cloud Bigtable cluster, which belongs to a Cloud Bigtable instance, which is a container for the cluster. Each node in the cluster handles a subset of the requests to the cluster.
When additional nodes are added to a cluster, you can increase the number of simultaneous requests that the cluster can handle, as well as the maximum throughput for the entire cluster.

Reference:

https://cloud.google.com/bigtable/docs/overview

</details>

---

## Q51

**Prompt:** In order to securely transfer web traffic data from your computer's web browser to the Cloud Dataproc cluster you should use a(n) _____.

**Options:**
- A) VPN connection
- B) Special browser
- C) SSH tunnel
- D) FTP connection

**Memorize:** `C) SSH tunnel`

<details>
<summary>Full explanation</summary>

Answer is 
SSH tunnel

To connect to the web interfaces, it is recommended to use an SSH tunnel to create a secure connection to the master node.

Reference:

https://cloud.google.com/dataproc/docs/concepts/cluster-web-interfaces#connecting_to_the_web_interfaces

</details>

---

## Q52

**Prompt:** The YARN ResourceManager and the HDFS NameNode interfaces are available on a Cloud Dataproc cluster ____.

**Options:**
- A) application node
- B) conditional node
- C) master node
- D) worker node

**Memorize:** `C) master node`

<details>
<summary>Full explanation</summary>

Answer is 
master node

The YARN ResourceManager and the HDFS NameNode interfaces are available on a Cloud Dataproc cluster master node. The cluster master-host-name is the name of your Cloud Dataproc cluster followed by an -m suffixfor example, if your cluster is named "my-cluster", the master-host-name would be "my-cluster-m".

Reference:

https://cloud.google.com/dataproc/docs/concepts/cluster-web-interfaces#interfaces

</details>

---

## Q53

**Prompt:** Cloud Dataproc charges you only for what you really use with _____ billing.

**Options:**
- A) month-by-month
- B) minute-by-minute
- C) week-by-week
- D) hour-by-hour
- E) second-by-second

**Memorize:** `E) second-by-second`

<details>
<summary>Full explanation</summary>

Answer is 
second-by-second

Although the pricing formula is expressed as an hourly rate, Dataproc is billed by the second, and all Dataproc clusters are billed in one-second clock-time increments, subject to a 1-minute minimum billing. Usage is stated in fractional hours (for example, 30 minutes is expressed as 0.5 hours) in order to apply hourly pricing to second-by-second use.

Reference:

https://cloud.google.com/dataproc/docs/concepts/overview

</details>

---

## Q54

**Prompt:** Scaling a Cloud Dataproc cluster typically involves ____.

**Options:**
- A) increasing or decreasing the number of worker nodes
- B) increasing or decreasing the number of master nodes
- C) moving memory to run more applications on a single node
- D) deleting applications from unused nodes periodically

**Memorize:** `A) increasing or decreasing the number of worker nodes`

<details>
<summary>Full explanation</summary>

Answer is 
increasing or decreasing the number of worker nodes

After creating a Cloud Dataproc cluster, you can scale the cluster by increasing or decreasing the number of worker nodes in the cluster at any time, even when jobs are running on the cluster. Cloud Dataproc clusters are typically scaled to:
1) increase the number of workers to make a job run faster
2) decrease the number of workers to save money
3) increase the number of nodes to expand available Hadoop Distributed Filesystem (HDFS) storage

Reference:

https://cloud.google.com/dataproc/docs/concepts/scaling-clusters

</details>

---

## Q55

**Prompt:** Dataproc clusters contain many configuration files. To update these files, you will need to use the --properties option. The format for the option is: file_prefix:property=_____.

**Options:**
- A) details
- B) value
- C) null
- D) id

**Memorize:** `B) value`

<details>
<summary>Full explanation</summary>

Answer is 
value

To make updating files and properties easy, the --properties command uses a special format to specify the configuration file and the property and value within the file that should be updated. The formatting is as follows: file_prefix:property=value.

Reference:

https://cloud.google.com/dataproc/docs/concepts/cluster-properties#formatting

</details>

---

## Q56

**Prompt:** Which action can a Cloud Dataproc Viewer perform?

**Options:**
- A) Submit a job.
- B) Create a cluster.
- C) Delete a cluster.
- D) List the jobs.

**Memorize:** `D) List the jobs.`

<details>
<summary>Full explanation</summary>

Answer is 
List the jobs.

A Cloud Dataproc Viewer is limited in its actions based on its role. A viewer can only list clusters, get cluster details, list jobs, get job details, list operations, and get operation details.

Reference:

https://cloud.google.com/dataproc/docs/concepts/iam#iam_roles_and_cloud_dataproc_operations_summary

</details>

---

## Q57

**Prompt:** Cloud Dataproc is a managed Apache Hadoop and Apache _____ service.

**Options:**
- A) Blaze
- B) Spark
- C) Fire
- D) Ignite

**Memorize:** `B) Spark`

<details>
<summary>Full explanation</summary>

Answer is 
Spark

Cloud Dataproc is a managed Apache Spark and Apache Hadoop service that lets you use open source data tools for batch processing, querying, streaming, and machine learning.

Reference:

https://cloud.google.com/dataproc/docs/

</details>

---

## Q58

**Prompt:** When using Cloud Dataproc clusters, you can access the YARN web interface by configuring a browser to connect through a ____ proxy.

**Options:**
- A) HTTPS
- B) VPN
- C) SOCKS
- D) HTTP

**Memorize:** `C) SOCKS`

<details>
<summary>Full explanation</summary>

Answer is 
SOCKS

When using Cloud Dataproc clusters, configure your browser to use the SOCKS proxy. The SOCKS proxy routes data intended for the Cloud Dataproc cluster through an SSH tunnel.

Reference:

https://cloud.google.com/dataproc/docs/concepts/cluster-web-interfaces#interfaces

</details>

---

## Q59

**Prompt:** Which of these rules apply when you add preemptible workers to a Dataproc cluster (select 2 answers)?

**Options:**
- A) Preemptible workers cannot use persistent disk.
- B) Preemptible workers cannot store data.
- C) If a preemptible worker is reclaimed, then a replacement worker must be added manually.
- D) A Dataproc cluster cannot have only preemptible workers.

**Memorize:** `D) A Dataproc cluster cannot have only preemptible workers.`

<details>
<summary>Full explanation</summary>

Answers are;

Preemptible workers cannot store data.

A Dataproc cluster cannot have only preemptible workers.

The following rules will apply when you use preemptible workers with a Cloud Dataproc cluster:
. Processing onlySince preemptibles can be reclaimed at any time, preemptible workers do not store data. Preemptibles added to a Cloud Dataproc cluster only function as processing nodes.
. No preemptible-only clustersTo ensure clusters do not lose all workers, Cloud Dataproc cannot create preemptible-only clusters.
. Persistent disk sizeAs a default, all preemptible workers are created with the smaller of 100GB or the primary worker boot disk size. This disk space is used for local caching of data and is not available through HDFS.
The managed group automatically re-adds workers lost due to reclamation as capacity permits.

Reference:

https://cloud.google.com/dataproc/docs/concepts/preemptible-vms

</details>

---

## Q60

**Prompt:** When creating a new Cloud Dataproc cluster with the projects.regions.clusters.create operation, these four values are required: project, region, name, and ____.

**Options:**
- A) zone
- B) node
- C) label
- D) type

**Memorize:** `A) zone`

<details>
<summary>Full explanation</summary>

Answer is 
zone

At a minimum, you must specify four values when creating a new cluster with the projects.regions.clusters.create operation:
The project in which the cluster will be created
The region to use -
The name of the cluster -
The zone in which the cluster will be created
You can specify many more details beyond these minimum requirements. For example, you can also specify the number of workers, whether preemptible compute should be used, and the network settings.

Reference:

https://cloud.google.com/dataproc/docs/tutorials/python-library-example#create_a_cluster

</details>

---

## Q61

**Prompt:** Which role must be assigned to a service account used by the virtual machines in a Dataproc cluster so they can execute jobs?

**Options:**
- A) Dataproc Worker
- B) Dataproc Viewer
- C) Dataproc Runner
- D) Dataproc Editor

**Memorize:** `A) Dataproc Worker`

<details>
<summary>Full explanation</summary>

Answer is 
Dataproc Worker

Service accounts used with Cloud Dataproc must have Dataproc/Dataproc Worker role (or have all the permissions granted by Dataproc Worker role).

Reference:

https://cloud.google.com/dataproc/docs/concepts/service-accounts#important_notes

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/service-accounts#service_account_requirements_and_limitations

</details>

---

## Q62

**Prompt:** What are the minimum permissions needed for a service account used with Google Dataproc?

**Options:**
- A) Execute to Google Cloud Storage; write to Google Cloud Logging
- B) Write to Google Cloud Storage; read to Google Cloud Logging
- C) Execute to Google Cloud Storage; execute to Google Cloud Logging
- D) Read and write to Google Cloud Storage; write to Google Cloud Logging

**Memorize:** `D) Read and write to Google Cloud Storage; write to Google Cloud Logging`

<details>
<summary>Full explanation</summary>

Answer is 
Read and write to Google Cloud Storage; write to Google Cloud Logging

Service accounts authenticate applications running on your virtual machine instances to other Google Cloud Platform services. For example, if you write an application that reads and writes files on Google Cloud Storage, it must first authenticate to the Google Cloud Storage API. At a minimum, service accounts used with Cloud Dataproc need permissions to read and write to Google Cloud Storage, and to write to Google Cloud Logging.

Reference:

https://cloud.google.com/dataproc/docs/concepts/service-accounts#important_notes

https://www.googleapis.com/auth/cloud.useraccounts.readonly

https://www.googleapis.com/auth/devstorage.read_write

https://www.googleapis.com/auth/logging.write

</details>

---

## Q63

**Prompt:** Which of the following job types are supported by Cloud Dataproc (select 3 answers)?

**Options:**
- A) Hive
- B) Pig
- C) YARN
- D) Spark

**Memorize:** `D) Spark`

<details>
<summary>Full explanation</summary>

Answers are; 
Hive, Pig and Spark

Cloud Dataproc provides out-of-the box and end-to-end support for many of the most popular job types, including Spark, Spark SQL, PySpark, MapReduce, Hive, and Pig jobs.

YARN is a resource manager.

Reference:

https://cloud.google.com/dataproc/docs/resources/faq#what_type_of_jobs_can_i_run

</details>

---

## Q64

**Prompt:** By default, which of the following windowing behavior does Dataflow apply to unbounded data sets?

**Options:**
- A) Windows at every 100 MB of data
- B) Single, Global Window
- C) Windows at every 1 minute
- D) Windows at every 10 minutes

**Memorize:** `B) Single, Global Window`

<details>
<summary>Full explanation</summary>

Answer is 
Single, Global Window

Dataflow's default windowing behavior is to assign all elements of a PCollection to a single, global window, even for unbounded PCollections

Reference:

https://cloud.google.com/dataflow/model/pcollection

</details>

---

## Q65

**Prompt:** Which of the following is not true about Dataflow pipelines?

**Options:**
- A) Pipelines are a set of operations
- B) Pipelines represent a data processing job
- C) Pipelines represent a directed graph of steps
- D) Pipelines can share data between instances

**Memorize:** `D) Pipelines can share data between instances`

<details>
<summary>Full explanation</summary>

Answer is 
Pipelines can share data between instances

In the Dataflow SDKs, a pipeline represents a data processing job. You build a pipeline by writing a program using a Dataflow SDK. A pipeline consists of a set of operations that can read a source of input data, transform that data, and write out the resulting output. The data and transforms in a pipeline are unique to, and owned by, that pipeline. While your program can create multiple pipelines, pipelines cannot share data or transforms.

Reference:

https://cloud.google.com/dataflow/model/pipelines

</details>

---

## Q66

**Prompt:** Which of the following IAM roles does your Compute Engine account require to be able to run pipeline jobs?

**Options:**
- A) dataflow.worker
- B) dataflow.compute
- C) dataflow.developer
- D) dataflow.viewer

**Memorize:** `A) dataflow.worker`

<details>
<summary>Full explanation</summary>

Answer is 
dataflow.worker

The dataflow.worker role provides the permissions necessary for a Compute Engine service account to execute work units for a Dataflow pipeline

Reference:

https://cloud.google.com/dataflow/access-control

</details>

---

## Q67

**Prompt:** You are developing a software application using Google's Dataflow SDK, and want to use conditional, for loops and other complex programming structures to create a branching pipeline. Which component will be used for the data processing operation?

**Options:**
- A) PCollection
- B) Transform
- C) Pipeline
- D) Sink API

**Memorize:** `B) Transform`

<details>
<summary>Full explanation</summary>

Answer is 
Transform

In Google Cloud, the Dataflow SDK provides a transform component. It is responsible for the data processing operation. You can use conditional, for loops, and other complex programming structure to create a branching pipeline.

Reference:

https://cloud.google.com/dataflow/model/programming-model

https://cloud.google.com/dataflow/docs/concepts/beam-programming-model#concepts

</details>

---

## Q68

**Prompt:** Which of the following is NOT true about Dataflow pipelines?

**Options:**
- A) Dataflow pipelines are tied to Dataflow, and cannot be run on any other runner
- B) Dataflow pipelines can consume data from other Google Cloud services
- C) Dataflow pipelines can be programmed in Java
- D) Dataflow pipelines use a unified programming model, so can work both with streaming and batch data sources

**Memorize:** `A) Dataflow pipelines are tied to Dataflow, and cannot be run on any other runner`

<details>
<summary>Full explanation</summary>

Answer is 
Dataflow pipelines are tied to Dataflow, and cannot be run on any other runner

Dataflow pipelines can also run on alternate runtimes like Spark and Flink, as they are built using the Apache Beam SDKs

Reference:

https://cloud.google.com/dataflow/

</details>

---

## Q69

**Prompt:** You want to process payment transactions in a point-of-sale application that will run on Google Cloud Platform. Your user base could grow exponentially, but you do not want to manage infrastructure scaling. Which Google database service should you use?

**Options:**
- A) Cloud SQLMost Voted
- B) BigQuery
- C) Cloud Bigtable
- D) Cloud Datastore

**Memorize:** `D) Cloud Datastore`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Datastore

It is a fully managed and serverless solution that allows for transactions and will autoscale (storage and compute) without the need to manage any infrastructure. 

A is wrong: Cloud SQL is fully a managed transactional DB, but only the storage grows automatically. As your user base increases, you will need to increase the CPU/memory of the instance, and to do that you must edit the instance manually (and the questions specifically says "you do not want to manage infrastructure scaling")

B is wrong: Bigquery is OLAP (for analytics). NoOps, fully managed, autoscales and allows transactions, but it is not designed for this use case.

C is wrong: Bigtable is a NoSQL database for massive writes, and to scale (storage and CPU) you must add nodes, so it is completely out of this use case.

Reference:

https://cloud.google.com/datastore/docs/concepts/overview

</details>

---

## Q70

**Prompt:** You work for a large bank that operates in locations throughout North America. You are setting up a data storage system that will handle bank account transactions. You require ACID compliance and the ability to access data with SQL.

**Options:**
- A) Store transaction data in Cloud Spanner. Enable stale reads to reduce latency.
- B) Store transaction in Cloud Spanner. Use locking read-write transactions.
- C) Store transaction data in BigQuery. Disabled the query cache to ensure consistency.
- D) Store transaction data in Cloud SQL. Use a federated query BigQuery for analysis.

**Memorize:** `B) Store transaction in Cloud Spanner. Use locking read-write transactions.`

<details>
<summary>Full explanation</summary>

Answer is 
Store transaction in Cloud Spanner. Use locking read-write transactions.

 

Since the banking transaction system requires ACID compliance and SQL access to the data, Cloud Spanner is the most appropriate solution. Unlike Cloud SQL, Cloud Spanner natively provides ACID transactions and horizontal scalability.

Enabling stale reads in Spanner (option A) would reduce data consistency, violating the ACID compliance requirement of banking transactions.

BigQuery (option C) does not natively support ACID transactions or SQL writes which are necessary for a banking transactions system.

Cloud SQL (option D) provides ACID compliance but does not scale horizontally like Cloud Spanner can to handle large transaction volumes.

By using Cloud Spanner and specifically locking read-write transactions, ACID compliance is ensured while providing fast, horizontally scalable SQL processing of banking transactions.

Reference:

https://cloud.google.com/blog/topics/developers-practitioners/your-google-cloud-database-options-explained

</details>

---

## Q71

**Prompt:** You work for a financial institution that lets customers register online. As new customers register, their user data is sent to Pub/Sub before being ingested into

BigQuery. For security reasons, you decide to redact your customers' Government issued Identification Number while allowing customer service representatives to view the original values when necessary.

**Options:**
- A) Use BigQuery's built-in AEAD encryption to encrypt the SSN column. Save the keys to a new table that is only viewable by permissioned users.
- B) Use BigQuery column-level security. Set the table permissions so that only members of the Customer Service user group can see the SSN column.
- C) Before loading the data into BigQuery, use Cloud Data Loss Prevention (DLP) to replace input values with a cryptographic hash.
- D) Before loading the data into BigQuery, use Cloud Data Loss Prevention (DLP) to replace input values with a cryptographic format-preserving encryption token.

**Memorize:** `D) Before loading the data into BigQuery, use Cloud Data Loss Prevention (DLP) to replace input values with a cryptographic format-preserving encryption token.`

<details>
<summary>Full explanation</summary>

Answer is 
Before loading the data into BigQuery, use Cloud Data Loss Prevention (DLP) to replace input values with a cryptographic format-preserving encryption token.

 

Before loading the data into BigQuery, use Cloud Data Loss Prevention (DLP) to replace input values with a cryptographic format-preserving encryption token.

The key reasons are:

- DLP allows redacting sensitive PII like SSNs before loading into BigQuery. This provides security by default for the raw SSN values.

- Using format-preserving encryption keeps the column format intact while still encrypting, allowing business logic relying on SSN format to continue functioning.

- The encrypted tokens can be reversed to view original SSNs when required, meeting the access requirement for customer service reps.

Option A does encrypt SSN but requires managing keys separately.

Option B relies on complex IAM policy changes instead of encrypting by default.

Doesn't truly redact the data. The SSN values are still stored in BigQuery, even if hidden from unauthorized users. A potential security breach could expose them.

Option C hashes irreversibly, preventing customer service reps from viewing original SSNs when required.

Therefore, using DLP format-preserving encryption before BigQuery ingestion balances both security and analytics requirements for SSN data.

Reference:

https://cloud.google.com/dlp/docs/classification-redaction

https://cloud.google.com/dlp/docs/transformations-reference

</details>

---

## Q72

**Prompt:** You need to deploy additional dependencies to all nodes of a Cloud Dataproc cluster at startup using an existing initialization action.

**Options:**
- A) Deploy the Cloud SQL Proxy on the Cloud Dataproc master
- B) Use an SSH tunnel to give the Cloud Dataproc cluster access to the Internet
- C) Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter
- D) Use Resource Manager to add the service account used by the Cloud Dataproc cluster to the Network User role

**Memorize:** `C) Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter`

<details>
<summary>Full explanation</summary>

Answer is 
Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter

 

If you create a Dataproc cluster with internal IP addresses only, attempts to access the Internet in an initialization action will fail unless you have configured routes to direct the traffic through a NAT or a VPN gateway. Without access to the Internet, you can enable Private Google Access, and place job dependencies in Cloud Storage; cluster nodes can download the dependencies from Cloud Storage from internal IPs.

Reference:

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/init-actions

</details>

---

## Q73

**Prompt:** You want to rebuild your batch pipeline for structured data on Google Cloud. You are using PySpark to conduct data transformations at scale, but your pipelines are taking over twelve hours to run. To expedite development and pipeline run time, you want to use a serverless tool and SOL syntax.

**Options:**
- A) Convert your PySpark commands into SparkSQL queries to transform the data, and then run your pipeline on Dataproc to write the data into BigQuery.
- B) Ingest your data into Cloud SQL, convert your PySpark commands into SparkSQL queries to transform the data, and then use federated quenes from BigQuery for machine learning.
- C) Ingest your data into BigQuery from Cloud Storage, convert your PySpark commands into BigQuery SQL queries to transform the data, and then write the transformations to a new table.
- D) Use Apache Beam Python SDK to build the transformation pipelines, and write the data into BigQuery.

**Memorize:** `C) Ingest your data into BigQuery from Cloud Storage, convert your PySpark commands into BigQuery SQL queries to transform the data, and then write the transformations to a new table.`

<details>
<summary>Full explanation</summary>

Answer is 
Ingest your data into BigQuery from Cloud Storage, convert your PySpark commands into BigQuery SQL queries to transform the data, and then write the transformations to a new table.

 

BigQuery SQL provides a fast, scalable, and serverless method for transforming structured data, easier to develop than PySpark.

Directly ingesting the raw Cloud Storage data into BigQuery avoids needing an intermediate processing cluster like Dataproc.

Transforming the data via BigQuery SQL queries will be faster than PySpark, especially since the data is already loaded into BigQuery.

Writing the transformed results to a new BigQuery table keeps the original raw data intact and provides a clean output.

So migrating to BigQuery SQL for transformations provides a fully managed serverless architecture that can significantly expedite development and reduce pipeline runtime versus PySpark. The ability to avoid clusters and conduct transformations completely within BigQuery is the most efficient approach here.

Reference:

https://cloud.google.com/dataproc-serverless/docs/overview

</details>

---

## Q74

**Prompt:** You are building a real-time prediction engine that streams files, which may contain PII (personal identifiable information) data, into Cloud Storage and eventually into BigQuery. You want to ensure that the sensitive data is masked but still maintains referential integrity, because names and emails are often used as join keys.

**Options:**
- A) Create a pseudonym by replacing the PII data with cryptogenic tokens, and store the non-tokenized data in a locked-down button.
- B) Redact all PII data, and store a version of the unredacted data in a locked-down bucket.
- C) Scan every table in BigQuery, and mask the data it finds that has PII.
- D) Create a pseudonym by replacing PII data with a cryptographic format-preserving token.

**Memorize:** `D) Create a pseudonym by replacing PII data with a cryptographic format-preserving token.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a pseudonym by replacing PII data with a cryptographic format-preserving token.

 

Format preserving encryption: An input value is replaced with a value that has been encrypted using the FPE-FFX encryption algorithm with a cryptographic key, and then prepended with a surrogate annotation, if specified. By design, both the character set and the length of the input value are preserved in the output value. Encrypted values can be re-identified using the original cryptographic key and the entire output value, including surrogate annotation.

Reference:

https://cloud.google.com/dlp/docs/pseudonymization#supported-methods

</details>

---

## Q75

**Prompt:** You need to give new website users a globally unique identifier (GUID) using a service that takes in data points and returns a GUID. This data is sourced from both internal and external systems via HTTP calls that you will make via microservices within your pipeline.

**Options:**
- A) Call out to the service via HTTP.
- B) Create the pipeline statically in the class definition.
- C) Create a new object in the startBundle method of DoFn.
- D) Batch the job into ten-second increments.

**Memorize:** `D) Batch the job into ten-second increments.`

<details>
<summary>Full explanation</summary>

Answer is 
Batch the job into ten-second increments.

 

Option D is the best approach to minimize backpressure in this scenario. By batching the jobs into 10-second increments, you can throttle the rate at which requests are made to the external GUID service. This prevents too many simultaneous requests from overloading the service.

Option A would not help with backpressure since it just makes synchronous HTTP requests as messages arrive. Similarly, options B and C don't provide any inherent batching or throttling mechanism.

Batching into time windows is a common strategy in stream processing to deal with high velocity data. The 10-second windows allow some buffering to happen, rather than making a call immediately for each message. This provides a natural throttling that can be tuned based on the external service's capacity.

To design a pipeline that minimizes backpressure, especially when dealing with tens of thousands of messages per second in a multi-threaded environment, it's important to consider how each option affects system performance and scalability. Let's examine each of your options:

A. Call out to the service via HTTP: Making HTTP calls to an external service for each message can introduce significant latency and backpressure, especially at high throughput. This is due to the overhead of establishing a connection, waiting for the response, and handling potential network delays or failures.

B. Create the pipeline statically in the class definition: While this approach can improve initialization time and reduce overhead during execution, it doesn't directly address the issue of backpressure caused by high message throughput.

C. Create a new object in the startBundle method of DoFn: This approach is typically used in Apache Beam to initialize resources before processing a bundle of elements. While it can optimize resource usage and performance within each bundle, it doesn't inherently solve the backpressure issue caused by high message rates.

D. Batch the job into ten-second increments: Batching messages can be an effective way to reduce backpressure. By grouping multiple messages into larger batches, you can reduce the frequency of external calls and distribute the processing load more evenly over time. This can lead to more efficient use of resources and potentially lower latency, as the system spends less time waiting on external services.

Given these considerations, option D (Batch the job into ten-second increments) seems to be the most effective strategy for minimizing backpressure in your scenario. By batching messages, you can reduce the strain on your pipeline and external services, making the system more resilient and scalable under high load. However, the exact batch size and interval should be fine-tuned based on the specific characteristics of your workload and the capabilities of the external systems you are interacting with.

Additionally, it's important to consider other strategies in conjunction with batching, such as implementing efficient error handling, load balancing, and potentially using asynchronous I/O for external HTTP calls to further optimize performance and minimize backpressure.

</details>

---

## Q76

**Prompt:** You are migrating your data warehouse to Google Cloud and decommissioning your on-premises data center. Because this is a priority for your company, you know that bandwidth will be made available for the initial data load to the cloud. The files being transferred are not large in number, but each file is 90 GB.

**Options:**
- A) Storage Transfer Service for the migration; Pub/Sub and Cloud Data Fusion for the real-time updates
- B) BigQuery Data Transfer Service for the migration; Pub/Sub and Dataproc for the real-time updates
- C) gsutil for the migration; Pub/Sub and Dataflow for the real-time updates
- D) gsutil for both the migration and the real-time updates

**Memorize:** `C) gsutil for the migration; Pub/Sub and Dataflow for the real-time updates`

<details>
<summary>Full explanation</summary>

Answer is 
gsutil for the migration; Pub/Sub and Dataflow for the real-time updates

 

Option C is the best choice given the large file sizes for the initial migration and the need for real-time updates after migration.

Specifically:

gsutil can transfer large files in parallel over multiple TCP connections to maximize bandwidth. This works well for the 90GB files during initial migration.

Pub/Sub allows real-time messaging of updates that can then be streamed into Cloud Dataflow. Dataflow provides scalable stream processing to handle transforming and writing those updates into BigQuery or other sinks.

Option A is incorrect because Storage Transfer Service is better for scheduled batch transfers, not ad hoc large migrations.

Option B is incorrect because BigQuery Data Transfer Service is more focused on scheduled replication jobs, not ad hoc migrations.

Option D would not work well for real-time updates after migration is complete.

Reference:

https://cloud.google.com/architecture/migration-to-google-cloud-transferring-your-large-datasets#gsutil_for_smaller_transfers_of_on-premises_data

</details>

---

## Q77

**Prompt:** You work for a shipping company that uses handheld scanners to read shipping labels. Your company has strict data privacy standards that require scanners to only transmit tracking numbers when events are sent to Kafka topics.

**Options:**
- A) Create an authorized view in BigQuery to restrict access to tables with sensitive data.
- B) Install a third-party data validation tool on Compute Engine virtual machines to check the incoming data for sensitive information.
- C) Use Cloud Logging to analyze the data passed through the total pipeline to identify transactions that may contain sensitive information.
- D) Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention (Cloud DLP) API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.

**Memorize:** `D) Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention (Cloud DLP) API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.`

<details>
<summary>Full explanation</summary>

Answer is 
Build a Cloud Function that reads the topics and makes a call to the Cloud Data Loss Prevention (Cloud DLP) API. Use the tagging and confidence levels to either pass or quarantine the data in a bucket for review.

 

DLP is required

</details>

---

## Q78

**Prompt:** You want to migrate an on-premises Hadoop system to Cloud Dataproc. Hive is the primary tool in use, and the data format is Optimized Row Columnar (ORC).

**Options:**
- A) Run the gsutil utility to transfer all ORC files from the Cloud Storage bucket to HDFS. Mount the Hive tables locally.
- B) Run the gsutil utility to transfer all ORC files from the Cloud Storage bucket to any node of the Dataproc cluster. Mount the Hive tables locally.
- C) Run the gsutil utility to transfer all ORC files from the Cloud Storage bucket to the master node of the Dataproc cluster. Then run the Hadoop utility to copy them do HDFS. Mount the Hive tables from HDFS.
- D) Leverage Cloud Storage connector for Hadoop to mount the ORC files as external Hive tables. Replicate external Hive tables to the native ones.
- E) Load the ORC files into BigQuery. Leverage BigQuery connector for Hadoop to mount the BigQuery tables as external Hive tables. Replicate external Hive tables to the native ones.

**Memorize:** `D) Leverage Cloud Storage connector for Hadoop to mount the ORC files as external Hive tables. Replicate external Hive tables to the native ones.`

<details>
<summary>Full explanation</summary>

Answers are;

C. Run the gsutil utility to transfer all ORC files from the Cloud Storage bucket to the master node of the Dataproc cluster. Then run the Hadoop utility to copy them do HDFS. Mount the Hive tables from HDFS.

D. Leverage Cloud Storage connector for Hadoop to mount the ORC files as external Hive tables. Replicate external Hive tables to the native ones.

 

I know it says to transfer all the files but with the options provided c is the best choice.

Explaination

A and B cannot be true as gsutil can copy data to master node and the to hdfs from master node.

C -> works

D->works Recommended by google. You can use the Cloud Storage connector for Hadoop to mount the ORC files stored in Cloud Storage as external Hive tables. This allows you to query the data without copying it to HDFS. You can replicate these external Hive tables to native Hive tables in Cloud Dataproc if needed.

E-> Will work but as the question says maximize performance this is not a case. As bigquery hadoop connecter stores all the BQ data to GCS as temp and then processes it to HDFS. As data is already in GCS we donot need to load it to bq and use a connector then unloads it back to GCS and then processes it.

Reference:

https://cloud.google.com/blog/products/data-analytics/new-release-of-cloud-storage-connector-for-hadoop-improving-performance-throughput-and-more

</details>

---

## Q79

**Prompt:** You are building a report-only data warehouse where the data is streamed into BigQuery via the streaming API. Following Google's best practices, you have both a staging and a production table for the data.

**Options:**
- A) Have a staging table that is an append-only model, and then update the production table every three hours with the changes written to staging.
- B) Have a staging table that is an append-only model, and then update the production table every ninety minutes with the changes written to staging.
- C) Have a staging table that moves the staged data over to the production table and deletes the contents of the staging table every three hours.
- D) Have a staging table that moves the staged data over to the production table and deletes the contents of the staging table every thirty minutes.

**Memorize:** `C) Have a staging table that moves the staged data over to the production table and deletes the contents of the staging table every three hours.`

<details>
<summary>Full explanation</summary>

Answer is 
Have a staging table that moves the staged data over to the production table and deletes the contents of the staging table every three hours.

 

Following common extract, transform, load (ETL) best practices, we used a staging table and a separate production table so that we could load data into the staging table without impacting users of the data. The design we created based on ETL best practices called for first deleting all the records from the staging table, loading the staging table, and then replacing the production table with the contents.

When using the streaming API, the BigQuery streaming buffer remains active for about 30 to 60 minutes or more after use, which means that you can’t delete or change data during that time. Since we used the streaming API, we scheduled the load every three hours to balance getting data into BigQuery quickly and being able to subsequently delete the data from the staging table during the load process.

Reference:

https://cloud.google.com/blog/products/data-analytics/moving-a-publishing-workflow-to-bigquery-for-new-data-insights

</details>

---

## Q80

**Prompt:** Your new customer has requested daily reports that show their net consumption of Google Cloud compute resources and who used the resources. You need to quickly and efficiently generate these daily reports.

**Options:**
- A) Do daily exports of Cloud Logging data to BigQuery. Create views filtering by project, log type, resource, and user.
- B) Filter data in Cloud Logging by project, resource, and user; then export the data in CSV format.
- C) Filter data in Cloud Logging by project, log type, resource, and user, then import the data into BigQuery.
- D) Export Cloud Logging data to Cloud Storage in CSV format. Cleanse the data using Dataprep, filtering by project, resource, and user.

**Memorize:** `A) Do daily exports of Cloud Logging data to BigQuery. Create views filtering by project, log type, resource, and user.`

<details>
<summary>Full explanation</summary>

Answer is 
Do daily exports of Cloud Logging data to BigQuery. Create views filtering by project, log type, resource, and user.

 

You cannot import custom or filtered billing criteria into BigQuery. There are three types of Cloud Billing data tables with a fixed schema that must further drilled-down via BigQuery views.

Reference:

https://cloud.google.com/billing/docs/how-to/export-data-bigquery#setup

</details>

---

## Q81

**Prompt:** The Development and External teams have the project viewer Identity and Access Management (IAM) role in a folder named Visualization. You want the

Development Team to be able to read data from both Cloud Storage and BigQuery, but the External Team should only be able to read data from BigQuery.

**Options:**
- A) Remove Cloud Storage IAM permissions to the External Team on the acme-raw-data project.Most Voted
- B) Create Virtual Private Cloud (VPC) firewall rules on the acme-raw-data project that deny all ingress traffic from the External Team CIDR range.
- C) Create a VPC Service Controls perimeter containing both projects and BigQuery as a restricted API. Add the External Team users to the perimeter's Access Level.
- D) Create a VPC Service Controls perimeter containing both projects and Cloud Storage as a restricted API. Add the Development Team users to the perimeter's Access Level.

**Memorize:** `D) Create a VPC Service Controls perimeter containing both projects and Cloud Storage as a restricted API. Add the Development Team users to the perimeter's Access Level.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a VPC Service Controls perimeter containing both projects and Cloud Storage as a restricted API. Add the Development Team users to the perimeter's Access Level.

 

Development team: needs to access both Cloud Storage and BQ -> therefore we put the Development team inside a perimeter so it can access both the Cloud Storage and the BQ

External team: allowed to access only BQ -> therefore we put Cloud Storage behind the restricted API and leave the external team outside of the perimeter, so it can access BQ, but is prohibited from accessing the Cloud Storage

"The grouping of GCP Project(s) and Service API(s) in the Service Perimeter result in restricting unauthorized access outside of the Service Perimeter to Service API endpoint(s) referencing resources inside of the Service Perimeter."

Reference:

https://scalesec.com/blog/vpc-service-controls-in-plain-english/

</details>

---

## Q82

**Prompt:** Your startup has a web application that currently serves customers out of a single region in Asia. You are targeting funding that will allow your startup to serve customers globally.

**Options:**
- A) Use Cloud Spanner to configure a single region instance initially, and then configure multi-region Cloud Spanner instances after securing funding.
- B) Use a Cloud SQL for PostgreSQL highly available instance first, and Bigtable with US, Europe, and Asia replication after securing funding.
- C) Use a Cloud SQL for PostgreSQL zonal instance first, and Bigtable with US, Europe, and Asia after securing funding.
- D) Use a Cloud SQL for PostgreSQL zonal instance first, and Cloud SQL for PostgreSQL with highly available configuration after securing funding.

**Memorize:** `A) Use Cloud Spanner to configure a single region instance initially, and then configure multi-region Cloud Spanner instances after securing funding.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Spanner to configure a single region instance initially, and then configure multi-region Cloud Spanner instances after securing funding.

 

When you create a Cloud Spanner instance, you must configure it as either regional (that is, all the resources are contained within a single Google Cloud region) or multi-region (that is, the resources span more than one region).

You can change the instance configuration to multi-regional (or global) at anytime.

Reference:

https://cloud.google.com/spanner/docs/jdbc-drivers

https://cloud.google.com/spanner/docs/instance-configurations#tradeoffs_regional_versus_multi-region_configurations

</details>

---

## Q83

**Prompt:** An aerospace company uses a proprietary data format to store its flight data.

**Options:**
- A) Write a shell script that triggers a Cloud Function that performs periodic ETL batch jobs on the new data source.
- B) Use a standard Dataflow pipeline to store the raw data in BigQuery, and then transform the format later when the data is used.
- C) Use Apache Hive to write a Dataproc job that streams the data into BigQuery in CSV format.
- D) Use an Apache Beam custom connector to write a Dataflow pipeline that streams the data into BigQuery in Avro format.

**Memorize:** `D) Use an Apache Beam custom connector to write a Dataflow pipeline that streams the data into BigQuery in Avro format.`

<details>
<summary>Full explanation</summary>

Answer is 
Use an Apache Beam custom connector to write a Dataflow pipeline that streams the data into BigQuery in Avro format.

 

The key reasons:

• Dataflow provides managed resource scaling for efficient stream processing

• Avro format has schema evolution capabilities and efficient serialization for flight telemetry data

• Apache Beam connectors avoid having to write much code to integrate proprietary data sources

• Streaming inserts data efficiently compared to periodic batch jobs

In contrast, option A uses Cloud Functions which lack native streaming capabilities. Option B stores data in less efficient JSON format. Option C uses Dataproc which requires manual cluster management.

So leveraging Dataflow + Avro + Beam provides the most efficient way to stream proprietary flight data into BigQuery while using minimal resources.

</details>

---

## Q84

**Prompt:** An online brokerage company requires a high volume trade processing architecture.

**Options:**
- A) Use a Pub/Sub push subscription to trigger a Cloud Function to pass the data to the Python API.
- B) Write an application hosted on a Compute Engine instance that makes a push subscription to the Pub/Sub topic.
- C) Write an application that makes a queue in a NoSQL database.
- D) Use Cloud Composer to subscribe to a Pub/Sub topic and call the Python API.

**Memorize:** `A) Use a Pub/Sub push subscription to trigger a Cloud Function to pass the data to the Python API.`

<details>
<summary>Full explanation</summary>

Answer is 
Use a Pub/Sub push subscription to trigger a Cloud Function to pass the data to the Python API.

 

Because trading platform requires securely transmission to queuing.

If you use cloud compose then we need some other job to trigger composer, would that be cloud composer api or cloud function.

Reference:

https://cloud.google.com/functions/docs/calling/pubsub#deployment

</details>

---

## Q85

**Prompt:** You have 15 TB of data in your on-premises data center that you want to transfer to Google Cloud.

**Options:**
- A) Use Cloud Scheduler to trigger the gsutil command. Use the -m parameter for optimal parallelism.
- B) Use Transfer Appliance to migrate your data into a Google Kubernetes Engine cluster, and then configure a weekly transfer job.
- C) Install Storage Transfer Service for on-premises data in your data center, and then configure a weekly transfer job.
- D) Install Storage Transfer Service for on-premises data on a Google Cloud virtual machine, and then configure a weekly transfer job.

**Memorize:** `C) Install Storage Transfer Service for on-premises data in your data center, and then configure a weekly transfer job.`

<details>
<summary>Full explanation</summary>

Answer is 
Install Storage Transfer Service for on-premises data in your data center, and then configure a weekly transfer job.

 

Like gsutil, Storage Transfer Service for on-premises data enables transfers from network file system (NFS) storage to Cloud Storage. Although gsutil can support small transfer sizes (up to 1 TB), Storage Transfer Service for on-premises data is designed for large-scale transfers (up to petabytes of data, billions of files).

Reference:

https://cloud.google.com/architecture/migration-to-google-cloud-transferring-your-large-datasets#storage-transfer-service-for-large-transfers-of-on-premises-data

</details>

---

## Q86

**Prompt:** You are using BigQuery and Data Studio to design a customer-facing dashboard that displays large quantities of aggregated data.

**Options:**
- A) Use BigQuery BI Engine with materialized views.
- B) Use BigQuery BI Engine with logical views.
- C) Use BigQuery BI Engine with streaming data.
- D) Use BigQuery BI Engine with authorized views.

**Memorize:** `A) Use BigQuery BI Engine with materialized views.`

<details>
<summary>Full explanation</summary>

Answer is 
Use BigQuery BI Engine with materialized views.

 

In BigQuery, materialized views are precomputed views that periodically cache the results of a query for increased performance and efficiency. BigQuery leverages precomputed results from materialized views and whenever possible reads only delta changes from the base tables to compute up-to-date results. Materialized views can be queried directly or can be used by the BigQuery optimizer to process queries to the base tables.

Queries that use materialized views are generally faster and consume fewer resources than queries that retrieve the same data only from the base tables. Materialized views can significantly improve the performance of workloads that have the characteristic of common and repeated queries.

Reference:

https://cloud.google.com/bigquery/docs/materialized-views-intro

</details>

---

## Q87

**Prompt:** Government regulations in the banking industry mandate the protection of clients' personally identifiable information (PII).

**Options:**
- A) Assign the required Identity and Access Management (IAM) roles to every employee, and create a single service account to access project resources.
- B) Use one service account to access a Cloud SQL database, and use separate service accounts for each human user.
- C) Use Cloud Storage to comply with major data protection standards. Use one service account shared by all users.
- D) Use Cloud Storage to comply with major data protection standards. Use multiple service accounts attached to IAM groups to grant the appropriate access to each group.

**Memorize:** `D) Use Cloud Storage to comply with major data protection standards. Use multiple service accounts attached to IAM groups to grant the appropriate access to each group.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Storage to comply with major data protection standards. Use multiple service accounts attached to IAM groups to grant the appropriate access to each group.

 

To align with Google's recommended practices for managing access to personally identifiable information (PII) in compliance with banking industry regulations, let's analyze the options:

A. Assign the required IAM roles to every employee, and create a single service account to access project resources: While assigning specific IAM roles to employees is a good practice for access control, using a single service account for all access to PII is not ideal. Service accounts should be used for applications and automated processes, not as a shared account for multiple users or employees.

B. Use one service account to access a Cloud SQL database, and use separate service accounts for each human user: Again, service accounts are intended for automated tasks or applications, not for individual human users. Assigning separate service accounts to each human user is not a recommended practice and does not align with the principle of least privilege.

C. Use Cloud Storage to comply with major data protection standards. Use one service account shared by all users: Using Cloud Storage can indeed help comply with data protection standards, especially when configured correctly with encryption and access controls. However, sharing a single service account among all users is not a best practice. It goes against the principle of least privilege and does not provide adequate granularity for access control.

D. Use Cloud Storage to comply with major data protection standards. Use multiple service accounts attached to IAM groups to grant the appropriate access to each group: This approach is more aligned with best practices. Using Cloud Storage can ensure compliance with data protection standards. Creating multiple service accounts, each with specific access controls attached to different IAM groups, allows for more granular and controlled access to PII. This setup adheres to the principle of least privilege, ensuring that each service (or group of services) only has access to the resources necessary for its function.

Based on these considerations, option D is the most appropriate choice. It ensures compliance with data protection standards, uses Cloud Storage for secure data management, and employs multiple service accounts tied to IAM groups for granular access control, aligning well with Google-recommended practices and regulatory requirements in the banking industry.

</details>

---

## Q88

**Prompt:** You create an important report for your large team in Google Data Studio 360. The report uses Google BigQuery as its data source. You notice that visualizations are not showing data that is less than 1 hour old. What should you do?

**Options:**
- A) Disable caching by editing the report settings.
- B) Disable caching in BigQuery by editing table details.
- C) Refresh your browser tab showing the visualizations.
- D) Clear your browser history for the past hour then reload the tab showing the virtualizations.

**Memorize:** `A) Disable caching by editing the report settings.`

<details>
<summary>Full explanation</summary>

Answer is 
Disable caching by editing the report settings.

Reference:

https://support.google.com/datastudio/answer/7020039?hl=en

</details>

---

## Q89

**Prompt:** Your startup has never implemented a formal security policy. Currently, everyone in the company has access to the datasets stored in Google BigQuery. Teams have freedom to use the service as they see fit, and they have not documented their use cases. You have been asked to secure the data warehouse. You need to discover what everyone is doing. What should you do first?

**Options:**
- A) Use Google Stackdriver Audit Logs to review data access.
- B) Get the identity and access management IIAM) policy of each table
- C) Use Stackdriver Monitoring to see the usage of BigQuery query slots.
- D) Use the Google Cloud Billing API to see what account the warehouse is being billed to.

**Memorize:** `A) Use Google Stackdriver Audit Logs to review data access.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Google Stackdriver Audit Logs to review data access.

'SLOTS' are just Virtual CPU used by BigQuery to process a query.

Reference:

https://cloud.google.com/bigquery/docs/slots

</details>

---

## Q90

**Prompt:** You have spent a few days loading data from comma-separated values (CSV) files into the Google BigQuery table CLICK_STREAM. The column DT stores the epoch time of click events. For convenience, you chose a simple schema where every field is treated as the STRING type. Now, you want to compute web session durations of users who visit your site, and you want to change its data type to the TIMESTAMP. You want to minimize the migration effort without making future queries computationally expensive. What should you do?

**Options:**
- A) Delete the table CLICK_STREAM, and then re-create it such that the column DT is of the TIMESTAMP type. Reload the data.
- B) Add a column TS of the TIMESTAMP type to the table CLICK_STREAM, and populate the numeric values from the column TS for each row. Reference the column TS instead of the column DT from now on.
- C) Create a view CLICK_STREAM_V, where strings from the column DT are cast into TIMESTAMP values. Reference the view CLICK_STREAM_V instead of the table CLICK_STREAM from now on.
- D) Add two columns to the table CLICK STREAM: TS of the TIMESTAMP type and IS_NEW of the BOOLEAN type. Reload all data in append mode. For each appended row, set the value of IS_NEW to true. For future queries, reference the column TS instead of the column DT, with the WHERE clause ensuring that the value of IS_NEW must be true.
- E) Construct a query to return every row of the table CLICK_STREAM, while using the built-in function to cast strings from the column DT into TIMESTAMP values. Run the query into a destination table NEW_CLICK_STREAM, in which the column TS is the TIMESTAMP type. Reference the table NEW_CLICK_STREAM instead of the table CLICK_STREAM from now on. In the future, new data is loaded into the table NEW_CLICK_STREAM.

**Memorize:** `E) Construct a query to return every row of the table CLICK_STREAM, while using the built-in function to cast strings from the column DT into TIMESTAMP values. Run the query into a destination table NEW_CLICK_STREAM, in which the column TS is the TIMESTAMP type. Reference the table NEW_CLICK_STREAM instead of the table CLICK_STREAM from now on. In the future, new data is loaded into the table NEW_CLICK_STREAM.`

<details>
<summary>Full explanation</summary>

Answer is 
E. Construct a query to return every row of the table CLICK_STREAM, while using the built-in function to cast strings from the column DT into TIMESTAMP values. Run the query into a destination table NEW_CLICK_STREAM, in which the column TS is the TIMESTAMP type. Reference the table NEW_CLICK_STREAM instead of the table CLICK_STREAM from now on. In the future, new data is loaded into the table NEW_CLICK_STREAM.

Creating a new table from existing table in BigQuery with new transformed column will be simple and will not involve and migration effort. Also future query performance will improve.

Reference:

https://cloud.google.com/bigquery/docs/manually-changing-schemas#changing_a_columns_data_type

</details>

---

## Q91

**Prompt:** You have Google Cloud Dataflow streaming pipeline running with a Google Cloud Pub/Sub subscription as the source. You need to make an update to the code that will make the new Cloud Dataflow pipeline incompatible with the current version. You do not want to lose any data when making this update. What should you do?

**Options:**
- A) Update the current pipeline and use the drain flag.
- B) Update the current pipeline and provide the transform mapping JSON object.
- C) Create a new pipeline that has the same Cloud Pub/Sub subscription and cancel the old pipeline.
- D) Create a new pipeline that has a new Cloud Pub/Sub subscription and cancel the old pipeline.

**Memorize:** `A) Update the current pipeline and use the drain flag.`

<details>
<summary>Full explanation</summary>

Answer is 
Update the current pipeline and use the drain flag.

Question mentions not to lose any data. All other options may lead to some data loss. If you want to prevent data loss as you bring down your streaming pipelines. the Dataflow pipeline can be stopped using the Drain option.

Drain options would cause Dataflow to stop any new processing, but would

also allow the existing processing to complete.

Reference:

https://cloud.google.com/dataflow/docs/guides/stopping-a-pipeline#drain

</details>

---

## Q92

**Prompt:** Your software uses a simple JSON format for all messages. These messages are published to Google Cloud Pub/Sub, then processed with Google Cloud

Dataflow to create a real-time dashboard for the CFO. During testing, you notice that some messages are missing in the dashboard. You check the logs, and all messages are being published to Cloud Pub/Sub successfully. What should you do next?

**Options:**
- A) Check the dashboard application to see if it is not displaying correctly.
- B) Run a fixed dataset through the Cloud Dataflow pipeline and analyze the output.
- C) Use Google Stackdriver Monitoring on Cloud Pub/Sub to find the missing messages.
- D) Switch Cloud Dataflow to pull messages from Cloud Pub/Sub instead of Cloud Pub/Sub pushing messages to Cloud Dataflow.

**Memorize:** `B) Run a fixed dataset through the Cloud Dataflow pipeline and analyze the output.`

<details>
<summary>Full explanation</summary>

Answer is 
Run a fixed dataset through the Cloud Dataflow pipeline and analyze the output.

Stack driver could tell us about performance but not logging of missing data.

Reference:

https://cloud.google.com/pubsub/docs/monitoring

</details>

---

## Q93

**Prompt:** You work for a large fast food restaurant chain with over 400,000 employees. You store employee information in Google BigQuery in a Users table consisting of a FirstName field and a LastName field. A member of IT is building an application and asks you to modify the schema and data in BigQuery so the application can query a FullName field consisting of the value of the FirstName field concatenated with a space, followed by the value of the LastName field for each employee. How can you make that data available while minimizing cost?

**Options:**
- A) Create a view in BigQuery that concatenates the FirstName and LastName field values to produce the FullName.Most Voted
- B) Add a new column called FullName to the Users table. Run an UPDATE statement that updates the FullName column for each user with the concatenation of the FirstName and LastName values.
- C) Create a Google Cloud Dataflow job that queries BigQuery for the entire Users table, concatenates the FirstName value and LastName value for each user, and loads the proper values for FirstName, LastName, and FullName into a new table in BigQuery.
- D) Use BigQuery to export the data for the table to a CSV file. Create a Google Cloud Dataproc job to process the CSV file and output a new CSV file containing the proper values for FirstName, LastName and FullName. Run a BigQuery load job to load the new CSV file into BigQuery.

**Memorize:** `C) Create a Google Cloud Dataflow job that queries BigQuery for the entire Users table, concatenates the FirstName value and LastName value for each user, and loads the proper values for FirstName, LastName, and FullName into a new table in BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Google Cloud Dataflow job that queries BigQuery for the entire Users table, concatenates the FirstName value and LastName value for each user, and loads the proper values for FirstName, LastName, and FullName into a new table in BigQuery.

There are two ways to manually rename a column:

Using a SQL query: choose this option if you are more concerned about simplicity and ease of use, and you are less concerned about costs.

Recreating the table: choose this option if you are more concerned about costs, and you are less concerned about simplicity and ease of use.

Similarily, if you need to create a new column, using Update query is not cost-effective. Creating a new table is cost-effective way.

Reference:

https://cloud.google.com/bigquery/docs/manually-changing-schemas?hl=en#changing_a_columns_name

</details>

---

## Q94

**Prompt:** You have enabled the free integration between Firebase Analytics and Google BigQuery. Firebase now automatically creates a new table daily in BigQuery in the format app_events_YYYYMMDD. You want to query all of the tables for the past 30 days in legacy SQL. What should you do?

**Options:**
- A) Use the TABLE_DATE_RANGE function
- B) Use the WHERE_PARTITIONTIME pseudo column
- C) Use WHERE date BETWEEN YYYY-MM-DD AND YYYY-MM-DD
- D) Use SELECT IF.(date >= YYYY-MM-DD AND date <= YYYY-MM-DD

**Memorize:** `A) Use the TABLE_DATE_RANGE function`

<details>
<summary>Full explanation</summary>

Answer is 
Use the TABLE_DATE_RANGE function

Legacy sql uses table date range whereas standard sql uses table_sufix for wildcard

Reference:

https://cloud.google.com/bigquery/docs/reference/legacy-sql#table-date-range

https://cloud.google.com/blog/products/gcp/using-bigquery-and-firebase-analytics-to-understand-your-mobile-app?hl=am

</details>

---

## Q95

**Prompt:** Your company is currently setting up data pipelines for their campaign. For all the Google Cloud Pub/Sub streaming data, one of the important business requirements is to be able to periodically identify the inputs and their timings during their campaign. Engineers have decided to use windowing and transformation in Google Cloud Dataflow for this purpose. However, when testing this feature, they find that the Cloud Dataflow job fails for the all streaming insert. What is the most likely cause of this problem?

**Options:**
- A) They have not assigned the timestamp, which causes the job to fail
- B) They have not set the triggers to accommodate the data coming in late, which causes the job to fail
- C) They have not applied a global windowing function, which causes the job to fail when the pipeline is created
- D) They have not applied a non-global windowing function, which causes the job to fail when the pipeline is created

**Memorize:** `D) They have not applied a non-global windowing function, which causes the job to fail when the pipeline is created`

<details>
<summary>Full explanation</summary>

Answer is 
They have not applied a non-global windowing function, which causes the job to fail when the pipeline is created

Caution: Beam’s default windowing behavior is to assign all elements of a PCollection to a single, global window and discard late data, even for unbounded PCollections. Before you use a grouping transform such as GroupByKey on an unbounded PCollection, you must do at least one of the following:

—->>>>>>Set a non-global windowing function. See Setting your PCollection’s windowing function.

Set a non-default trigger. This allows the global window to emit results under other conditions, since the default windowing behavior (waiting for all data to arrive) will never occur.

—->>>>If you don’t set a non-global windowing function or a non-default trigger for your unbounded PCollection and subsequently use a grouping transform such as GroupByKey or Combine, your pipeline will generate an error upon construction and your job will fail.

Reference:

https://beam.apache.org/documentation/programming-guide/#windowing

</details>

---

## Q96

**Prompt:** You architect a system to analyze seismic data. Your extract, transform, and load (ETL) process runs as a series of MapReduce jobs on an Apache Hadoop cluster. The ETL process takes days to process a data set because some steps are computationally expensive. Then you discover that a sensor calibration step has been omitted. How should you change your ETL process to carry out sensor calibration systematically in the future?

**Options:**
- A) Modify the transformMapReduce jobs to apply sensor calibration before they do anything else.
- B) Introduce a new MapReduce job to apply sensor calibration to raw data, and ensure all other MapReduce jobs are chained after this.
- C) Add sensor calibration data to the output of the ETL process, and document that all users need to apply sensor calibration themselves.
- D) Develop an algorithm through simulation to predict variance of data output from the last MapReduce job based on calibration factors, and apply the correction to all data.

**Memorize:** `B) Introduce a new MapReduce job to apply sensor calibration to raw data, and ensure all other MapReduce jobs are chained after this.`

<details>
<summary>Full explanation</summary>

Answer is 
Introduce a new MapReduce job to apply sensor calibration to raw data, and ensure all other MapReduce jobs are chained after this.

It is a cleaner approach with single job to handle the calibration before the data is used in the pipeline. Second, doing this step in later stages can be complex and maintenance of those jobs in the future will become challenging.

</details>

---

## Q97

**Prompt:** An online retailer has built their current application on Google App Engine. A new initiative at the company mandates that they extend their application to allow their customers to transact directly via the application. They need to manage their shopping transactions and analyze combined data from multiple datasets using a business intelligence (BI) tool. They want to use only a single database for this purpose. Which Google Cloud database should they choose?

**Options:**
- A) BigQuery
- B) Cloud SQL
- C) Cloud BigTable
- D) Cloud Datastore

**Memorize:** `B) Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud SQL

Cloud SQL supports transactions as well as analysis through a BI tool. Firestore/Datastore does not support SQL syntax typically needed to do analysis done by a BI tool. BigQuery is not suitable for transactional use case. BigTable does not support SQL.

</details>

---

## Q98

**Prompt:** You launched a new gaming app almost three years ago. You have been uploading log files from the previous day to a separate Google BigQuery table with the table name format LOGS_yyyymmdd. You have been using table wildcard functions to generate daily and monthly reports for all time ranges. Recently, you discovered that some queries that cover long date ranges are exceeding the limit of 1,000 tables and failing. How can you resolve this issue?

**Options:**
- A) Convert all daily log tables into date-partitioned tables
- B) Convert the sharded tables into a single partitioned table
- C) Enable query caching so you can cache data from previous months
- D) Create separate views to cover each month, and query from these views

**Memorize:** `B) Convert the sharded tables into a single partitioned table`

<details>
<summary>Full explanation</summary>

Answer is 
Convert the sharded tables into a single partitioned table

Google says that when you have multiple wildcard tables, best option is to shard it into single partitioned table. Time and cost efficient

Reference:

https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard

</details>

---

## Q99

**Prompt:** You are integrating one of your internal IT applications and Google BigQuery, so users can query BigQuery from the application's interface. You do not want individual users to authenticate to BigQuery and you do not want to give them access to the dataset. You need to securely access BigQuery from your IT application. What should you do?

**Options:**
- A) Create groups for your users and give those groups access to the dataset
- B) Integrate with a single sign-on (SSO) platform, and pass each user's credentials along with the query request
- C) Create a service account and grant dataset access to that account. Use the service account's private key to access the dataset
- D) Create a dummy user and grant dataset access to that user. Store the username and password for that user in a file on the files system, and use those credentials to access the BigQuery dataset

**Memorize:** `C) Create a service account and grant dataset access to that account. Use the service account's private key to access the dataset`

<details>
<summary>Full explanation</summary>

Answer is 
Create a service account and grant dataset access to that account. Use the service account's private key to access the dataset

Service Account are best option when granting access from tools/appllications

</details>

---

## Q100

**Prompt:** You are selecting services to write and transform JSON messages from Cloud Pub/Sub to BigQuery for a data pipeline on Google Cloud. You want to minimize service costs. You also want to monitor and accommodate input data volume that will vary in size with minimal manual intervention. What should you do?

**Options:**
- A) Use Cloud Dataproc to run your transformations. Monitor CPU utilization for the cluster. Resize the number of worker nodes in your cluster via the command line.
- B) Use Cloud Dataproc to run your transformations. Use the diagnose command to generate an operational output archive. Locate the bottleneck and adjust cluster resources.
- C) Use Cloud Dataflow to run your transformations. Monitor the job system lag with Stackdriver. Use the default autoscaling setting for worker instances.
- D) Use Cloud Dataflow to run your transformations. Monitor the total execution time for a sampling of jobs. Configure the job to use non-default Compute Engine machine types when needed.

**Memorize:** `C) Use Cloud Dataflow to run your transformations. Monitor the job system lag with Stackdriver. Use the default autoscaling setting for worker instances.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Dataflow to run your transformations. Monitor the job system lag with Stackdriver. Use the default autoscaling setting for worker instances.

Dataflow provides a cost-effective solution to perform transformations on the streaming data, with autoscaling provides scaling without any intervention. System lag with Stackdriver provides monitoring for the streaming data. With autoscaling enabled, the Cloud Dataflow service automatically chooses the appropriate number of worker instances required to run your job.

</details>

---

## Q101

**Prompt:** Government regulations in your industry mandate that you have to maintain an auditable record of access to certain types of data. Assuming that all expiring logs will be archived correctly, where should you store data that is subject to that mandate?

**Options:**
- A) Encrypted on Cloud Storage with user-supplied encryption keys. A separate decryption key will be given to each authorized user.
- B) In a BigQuery dataset that is viewable only by authorized personnel, with the Data Access log used to provide the auditability.Most Voted
- C) In Cloud SQL, with separate database user names to each user. The Cloud SQL Admin activity logs will be used to provide the auditability.
- D) In a bucket on Cloud Storage that is accessible only by an AppEngine service that collects user information and logs the access before providing a link to the bucket.

**Memorize:** `D) In a bucket on Cloud Storage that is accessible only by an AppEngine service that collects user information and logs the access before providing a link to the bucket.`

<details>
<summary>Full explanation</summary>

Answer is 
In a bucket on Cloud Storage that is accessible only by an AppEngine service that collects user information and logs the access before providing a link to the bucket.

Bigquery is not the best option here to store the auditable record of access to certain types of data, because to comply with the government regulations, businesses have to keep records of for up to 7 years. Cloud Storage is cheaper for file storages and can be used as external federated source and queried through big query.

</details>

---

## Q102

**Prompt:** After migrating ETL jobs to run on BigQuery, you need to verify that the output of the migrated jobs is the same as the output of the original. You've loaded a table containing the output of the original job and want to compare the contents with output from the migrated job to show that they are identical. The tables do not contain a primary key column that would enable you to join them together for comparison. What should you do?

**Options:**
- A) Select random samples from the tables using the RAND() function and compare the samples.
- B) Select random samples from the tables using the HASH() function and compare the samples.
- C) Use a Dataproc cluster and the BigQuery Hadoop connector to read the data from each table and calculate a hash from non-timestamp columns of the table after sorting. Compare the hashes of each table.
- D) Create stratified random samples using the OVER() function and compare equivalent samples from each table.Most Voted

**Memorize:** `C) Use a Dataproc cluster and the BigQuery Hadoop connector to read the data from each table and calculate a hash from non-timestamp columns of the table after sorting. Compare the hashes of each table.`

<details>
<summary>Full explanation</summary>

Answer is 
Use a Dataproc cluster and the BigQuery Hadoop connector to read the data from each table and calculate a hash from non-timestamp columns of the table after sorting. Compare the hashes of each table.

Using Cloud Storage with big data

Cloud Storage is a key part of storing and working with Big Data on Google Cloud. Examples include:

Loading data into BigQuery.

Using Dataproc, which automatically installs the HDFS-compatible Cloud Storage connector, enabling the use of Cloud Storage buckets in parallel with HDFS.

Using a bucket to hold staging files and temporary data for Dataflow pipelines.

For Dataflow, a Cloud Storage bucket is required. For BigQuery and Dataproc, using a Cloud Storage bucket is optional but recommended.

gsutil is a command-line tool that enables you to work with Cloud Storage buckets and objects easily and robustly, in particular in big data scenarios. For example, with gsutil you can copy many files in parallel with a single command, copy large files efficiently, calculate checksums on your data, and measure performance from your local computer to Cloud Storage.

</details>

---

## Q103

**Prompt:** You are a head of BI at a large enterprise company with multiple business units that each have different priorities and budgets. You use on-demand pricing for BigQuery with a quota of 2K concurrent on-demand slots per project. Users at your organization sometimes don't get slots to execute their query and you need to correct this. You'd like to avoid introducing new projects to your account. What should you do?

**Options:**
- A) Convert your batch BQ queries into interactive BQ queries.
- B) Create an additional project to overcome the 2K on-demand per-project quota.
- C) Switch to flat-rate pricing and establish a hierarchical priority model for your projects.
- D) Increase the amount of concurrent slots per project at the Quotas page at the Cloud Console.

**Memorize:** `C) Switch to flat-rate pricing and establish a hierarchical priority model for your projects.`

<details>
<summary>Full explanation</summary>

Answer is 
Switch to flat-rate pricing and establish a hierarchical priority model for your projects.

Fixed price doesn’t exhaust limits

Reference:

https://cloud.google.com/blog/products/gcp/busting-12-myths-about-bigquery

</details>

---

## Q104

**Prompt:** You have an Apache Kafka cluster on-prem with topics containing web application logs. You need to replicate the data to Google Cloud for analysis in BigQuery and Cloud Storage. The preferred replication method is mirroring to avoid deployment of Kafka Connect plugins. What should you do?

**Options:**
- A) Deploy a Kafka cluster on GCE VM Instances. Configure your on-prem cluster to mirror your topics to the cluster running in GCE. Use a Dataproc cluster or Dataflow job to read from Kafka and write to GCS.
- B) Deploy a Kafka cluster on GCE VM Instances with the PubSub Kafka connector configured as a Sink connector. Use a Dataproc cluster or Dataflow job to read from Kafka and write to GCS.
- C) Deploy the PubSub Kafka connector to your on-prem Kafka cluster and configure PubSub as a Source connector. Use a Dataflow job to read from PubSub and write to GCS.
- D) Deploy the PubSub Kafka connector to your on-prem Kafka cluster and configure PubSub as a Sink connector. Use a Dataflow job to read from PubSub and write to GCS.

**Memorize:** `A) Deploy a Kafka cluster on GCE VM Instances. Configure your on-prem cluster to mirror your topics to the cluster running in GCE. Use a Dataproc cluster or Dataflow job to read from Kafka and write to GCS.`

<details>
<summary>Full explanation</summary>

Answer is 
Deploy a Kafka cluster on GCE VM Instances. Configure your on-prem cluster to mirror your topics to the cluster running in GCE. Use a Dataproc cluster or Dataflow job to read from Kafka and write to GCS.

The solution specifically mentions mirroring and minimizing the use of Kafka Connect plugin.

Reference:

https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330

</details>

---

## Q105

**Prompt:** Your team is responsible for developing and maintaining ETLs in your company. One of your Dataflow jobs is failing because of some errors in the input data, and you need to improve reliability of the pipeline (incl. being able to reprocess all failing data). What should you do?

**Options:**
- A) Add a filtering step to skip these types of errors in the future, extract erroneous rows from logs.
- B) Add a try... catch block to your DoFn that transforms the data, extract erroneous rows from logs.
- C) Add a try... catch block to your DoFn that transforms the data, write erroneous rows to PubSub directly from the DoFn.
- D) Add a try... catch block to your DoFn that transforms the data, use a sideOutput to create a PCollection that can be stored to PubSub later.Most Voted

**Memorize:** `C) Add a try... catch block to your DoFn that transforms the data, write erroneous rows to PubSub directly from the DoFn.`

<details>
<summary>Full explanation</summary>

Answer is 
Add a try... catch block to your DoFn that transforms the data, write erroneous rows to PubSub directly from the DoFn.

The error records are directly written to PubSub from the DoFn (it’s equivalent in python).

You cannot directly write a PCollection to PubSub. You have to extract each record and write one at a time. Why do the additional work and why not write it using PubSubIO in the DoFn itself?

You can write the whole PCollection to Bigquery though, as explained in

Reference:

https://medium.com/google-cloud/dead-letter-queues-simple-implementation-strategy-for-cloud-pub-sub-80adf4a4a800

</details>

---

## Q106

**Prompt:** You're using Bigtable for a real-time application, and you have a heavy load that is a mix of read and writes. You've recently identified an additional use case and need to perform hourly an analytical job to calculate certain statistics across the whole database. You need to ensure both the reliability of your production application as well as the analytical workload. What should you do?

**Options:**
- A) Export Bigtable dump to GCS and run your analytical job on top of the exported files.
- B) Add a second cluster to an existing instance with a multi-cluster routing, use live-traffic app profile for your regular workload and batch-analytics profile for the analytics workload.Most Voted
- C) Add a second cluster to an existing instance with a single-cluster routing, use live-traffic app profile for your regular workload and batch-analytics profile for the analytics workload.
- D) Increase the size of your existing cluster twice and execute your analytics workload on your new resized cluster.

**Memorize:** `C) Add a second cluster to an existing instance with a single-cluster routing, use live-traffic app profile for your regular workload and batch-analytics profile for the analytics workload.`

<details>
<summary>Full explanation</summary>

Answer is 
Add a second cluster to an existing instance with a single-cluster routing, use live-traffic app profile for your regular workload and batch-analytics profile for the analytics workload.

You need to perform an hourly batch job on the cluster that already has high workload. in such cases, the best option is to replicate the cluster with single cluster routing. The original cluster can continue its read and writes. the replicated cluster can be used for analytical workload without impacting original cluster. Multi cluster routing is beneficial in cases where high availability is needed but requirement is only to isolate analytical workload from existing cluster.

Reference:

https://cloud.google.com/bigtable/docs/replication-overview#use-cases

</details>

---

## Q107

**Prompt:** You store historic data in Cloud Storage. You need to perform analytics on the historic data. You want to use a solution to detect invalid data entries and perform data transformations that will not require programming or knowledge of SQL. What should you do?

**Options:**
- A) Use Cloud Dataflow with Beam to detect errors and perform transformations.
- B) Use Cloud Dataprep with recipes to detect errors and perform transformations.
- C) Use Cloud Dataproc with a Hadoop job to detect errors and perform transformations.
- D) Use federated tables in BigQuery with queries to detect errors and perform transformations.

**Memorize:** `B) Use Cloud Dataprep with recipes to detect errors and perform transformations.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Dataprep with recipes to detect errors and perform transformations.

The keyword here is no programming skills required.

</details>

---

## Q108

**Prompt:** Your company needs to upload their historic data to Cloud Storage. The security rules don't allow access from external IPs to their on-premises resources. After an initial upload, they will add new data from existing on-premises applications every day. What should they do?

**Options:**
- A) Execute gsutil rsync from the on-premises servers.
- B) Use Cloud Dataflow and write the data to Cloud Storage.
- C) Write a job template in Cloud Dataproc to perform the data transfer.
- D) Install an FTP server on a Compute Engine VM to receive the files and move them to Cloud Storage.

**Memorize:** `A) Execute gsutil rsync from the on-premises servers.`

<details>
<summary>Full explanation</summary>

Answer is 
Execute gsutil rsync from the on-premises servers.

Dataflow is on cloud is external; "don't allow access from external IPs to their on-premises resources" so no dataflow.

Reference:

https://cloud.google.com/solutions/migration-to-google-cloud-transferring-your-large-datasets#options_available_from_google

</details>

---

## Q109

**Prompt:** You need to copy millions of sensitive patient records from a relational database to BigQuery. The total size of the database is 10 TB. You need to design a solution that is secure and time-efficient. What should you do?

**Options:**
- A) Export the records from the database as an Avro file. Upload the file to GCS using gsutil, and then load the Avro file into BigQuery using the BigQuery web UI in the GCP Console.Most Voted
- B) Export the records from the database as an Avro file. Copy the file onto a Transfer Appliance and send it to Google, and then load the Avro file into BigQuery using the BigQuery web UI in the GCP Console.
- C) Export the records from the database into a CSV file. Create a public URL for the CSV file, and then use Storage Transfer Service to move the file to Cloud Storage. Load the CSV file into BigQuery using the BigQuery web UI in the GCP Console.
- D) Export the records from the database as an Avro file. Create a public URL for the Avro file, and then use Storage Transfer Service to move the file to Cloud Storage. Load the Avro file into BigQuery using the BigQuery web UI in the GCP Console.

**Memorize:** `B) Export the records from the database as an Avro file. Copy the file onto a Transfer Appliance and send it to Google, and then load the Avro file into BigQuery using the BigQuery web UI in the GCP Console.`

<details>
<summary>Full explanation</summary>

Answer is 
Export the records from the database as an Avro file. Copy the file onto a Transfer Appliance and send it to Google, and then load the Avro file into BigQuery using the BigQuery web UI in the GCP Console.

You are transferring sensitive patient information, so C & D are ruled out. Choice comes down to A & B. Here it gets tricky. How to choose Transfer Appliance: (
https://cloud.google.com/transfer-appliance/docs/2.0/overview
)

Without knowing the bandwidth, it is not possible to determine whether the upload can be completed within 7 days, as recommended by Google. So the safest and most performant way is to use Transfer Appliance.

</details>

---

## Q110

**Prompt:** You used Cloud Dataprep to create a recipe on a sample of data in a BigQuery table. You want to reuse this recipe on a daily upload of data with the same schema, after the load job with variable execution time completes. What should you do?

**Options:**
- A) Create a cron schedule in Cloud Dataprep.
- B) Create an App Engine cron job to schedule the execution of the Cloud Dataprep job.
- C) Export the recipe as a Cloud Dataprep template, and create a job in Cloud Scheduler.
- D) Export the Cloud Dataprep job as a Cloud Dataflow template, and incorporate it into a Cloud Composer job.

**Memorize:** `D) Export the Cloud Dataprep job as a Cloud Dataflow template, and incorporate it into a Cloud Composer job.`

<details>
<summary>Full explanation</summary>

Answer is 
Export the Cloud Dataprep job as a Cloud Dataflow template, and incorporate it into a Cloud Composer job.

Dataprep can be run on Dataflow using template and cloud composer will create dependency on previous job

Reference:

https://cloud.google.com/blog/products/data-analytics/how-to-orchestrate-cloud-dataprep-jobs-using-cloud-composer

</details>

---

## Q111

**Prompt:** You have developed three data processing jobs. One executes a Cloud Dataflow pipeline that transforms data uploaded to Cloud Storage and writes results to BigQuery. The second ingests data from on-premises servers and uploads it to Cloud Storage. The third is a Cloud Dataflow pipeline that gets information from third-party data providers and uploads the information to Cloud Storage. You need to be able to schedule and monitor the execution of these three workflows and manually execute them when needed. What should you do?

**Options:**
- A) Create a Direct Acyclic Graph in Cloud Composer to schedule and monitor the jobs.
- B) Use Stackdriver Monitoring and set up an alert with a Webhook notification to trigger the jobs.
- C) Develop an App Engine application to schedule and request the status of the jobs using GCP API calls.
- D) Set up cron jobs in a Compute Engine instance to schedule and monitor the pipelines using GCP API calls.

**Memorize:** `A) Create a Direct Acyclic Graph in Cloud Composer to schedule and monitor the jobs.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Direct Acyclic Graph in Cloud Composer to schedule and monitor the jobs.

Cloud composer is used to schedule the interdependent jobs

</details>

---

## Q112

**Prompt:** You have Cloud Functions written in Node.js that pull messages from Cloud Pub/Sub and send the data to BigQuery. You observe that the message processing rate on the Pub/Sub topic is orders of magnitude higher than anticipated, but there is no error logged in Stackdriver Log Viewer. What are the two most likely causes of this problem? (Choose two.)

**Options:**
- A) Publisher throughput quota is too small.
- B) Total outstanding messages exceed the 10-MB maximum.
- C) Error handling in the subscriber code is not handling run-time errors properly.
- D) The subscriber code cannot keep up with the messages.
- E) The subscriber code does not acknowledge the messages that it pulls.

**Memorize:** `E) The subscriber code does not acknowledge the messages that it pulls.`

<details>
<summary>Full explanation</summary>

Answers are;

C. Error handling in the subscriber code is not handling run-time errors properly.

E. The subscriber code does not acknowledge the messages that it pulls.

By not acknowleding the pulled message, this result in it be putted back in Cloud Pub/Sub, meaning the messages accumulate instead of being consumed and removed from Pub/Sub. The same thing can happen ig the subscriber maintains the lease on the message it receives in case of an error. This reduces the overall rate of processing because messages get stuck on the first subscriber. Also, errors in Cloud Function do not show up in Stackdriver Log Viewer if they are not correctly handled.

A: No problem with publisher rate as the observed result is a higher number of messages and not a lower number.

B: if messages exceed the 10MB maximum, they cannot be published.

D: Cloud Functions automatically scales so they should be able to keep up.

</details>

---

## Q113

**Prompt:** Your company has a hybrid cloud initiative. You have a complex data pipeline that moves data between cloud provider services and leverages services from each of the cloud providers. Which cloud-native service should you use to orchestrate the entire pipeline?

**Options:**
- A) Cloud Dataflow
- B) Cloud Composer
- C) Cloud Dataprep
- D) Cloud Dataproc

**Memorize:** `B) Cloud Composer`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Composer

Hybrid and multi-cloud

Ease your transition to the cloud or maintain a hybrid data environment by orchestrating workflows that cross between on-premises and the public cloud. Create workflows that connect data, processing, and services across clouds to give you a unified data environment.

Reference:

https://cloud.google.com/composer#section-2

</details>

---

## Q114

**Prompt:** You use a dataset in BigQuery for analysis. You want to provide third-party companies with access to the same dataset. You need to keep the costs of data sharing low and ensure that the data is current. Which solution should you choose?

**Options:**
- A) Create an authorized view on the BigQuery table to control data access, and provide third-party companies with access to that view.
- B) Use Cloud Scheduler to export the data on a regular basis to Cloud Storage, and provide third-party companies with access to the bucket.
- C) Create a separate dataset in BigQuery that contains the relevant data to share, and provide third-party companies with access to the new dataset.
- D) Create a Cloud Dataflow job that reads the data in frequent time intervals, and writes it to the relevant BigQuery dataset or Cloud Storage bucket for third-party companies to use.

**Memorize:** `A) Create an authorized view on the BigQuery table to control data access, and provide third-party companies with access to that view.`

<details>
<summary>Full explanation</summary>

Answer is 
Create an authorized view on the BigQuery table to control data access, and provide third-party companies with access to that view.

By creating an authorized view one assures that the data is current and avoids taking more storage space (and cost) in order to share a dataset. B and D are not cost optimal and C does not guarantee that the data is kept updated

</details>

---

## Q115

**Prompt:** A shipping company has live package-tracking data that is sent to an Apache Kafka stream in real time. This is then loaded into BigQuery. Analysts in your company want to query the tracking data in BigQuery to analyze geospatial trends in the lifecycle of a package. The table was originally created with ingest-date partitioning. Over time, the query processing time has increased. You need to implement a change that would improve query performance in BigQuery. What should you do?

**Options:**
- A) Implement clustering in BigQuery on the ingest date column.
- B) Implement clustering in BigQuery on the package-tracking ID column.
- C) Tier older data onto Cloud Storage files, and leverage extended tables.
- D) Re-create the table using data partitioning on the package delivery date.

**Memorize:** `B) Implement clustering in BigQuery on the package-tracking ID column.`

<details>
<summary>Full explanation</summary>

Answer is 
Implement clustering in BigQuery on the package-tracking ID column.

In general, there are two typical usage patterns for clustering within a data warehouse:

Clustering on columns that have a very high number of distinct values, like userId or transactionId.

Clustering on multiple columns that are frequently used together. When clustering by multiple columns, the order of columns you specify is important. The order of the specified columns determines the sort order of the data. You can filter by any prefix of the clustering columns and get the benefits of clustering, like regionId, shopId and productId together; or regionId and shopId; or just regionId.

Reference:

https://cloud.google.com/blog/products/data-analytics/skip-the-maintenance-speed-up-queries-with-bigquerys-clustering

</details>

---

## Q116

**Prompt:** You are operating a Cloud Dataflow streaming pipeline. The pipeline aggregates events from a Cloud Pub/Sub subscription source, within a window, and sinks the resulting aggregation to a Cloud Storage bucket. The source has consistent throughput. You want to monitor an alert on behavior of the pipeline with Cloud

Stackdriver to ensure that it is processing data. Which Stackdriver alerts should you create?

**Options:**
- A) An alert based on a decrease of subscription/num_undelivered_messages for the source and a rate of change increase of instance/storage/ used_bytes for the destination
- B) An alert based on an increase of subscription/num_undelivered_messages for the source and a rate of change decrease of instance/storage/ used_bytes for the destination
- C) An alert based on a decrease of instance/storage/used_bytes for the source and a rate of change increase of subscription/ num_undelivered_messages for the destination
- D) An alert based on an increase of instance/storage/used_bytes for the source and a rate of change decrease of subscription/ num_undelivered_messages for the destination

**Memorize:** `B) An alert based on an increase of subscription/num_undelivered_messages for the source and a rate of change decrease of instance/storage/ used_bytes for the destination`

<details>
<summary>Full explanation</summary>

Answer is 
An alert based on an increase of subscription/num_undelivered_messages for the source and a rate of change decrease of instance/storage/ used_bytes for the destination

Need to create alert when pipeline data processing is not as per the expectation also need to monitor the storage

</details>

---

## Q117

**Prompt:** You currently have a single on-premises Kafka cluster in a data center in the us-east region that is responsible for ingesting messages from IoT devices globally.

Because large parts of globe have poor internet connectivity, messages sometimes batch at the edge, come in all at once, and cause a spike in load on your Kafka cluster. This is becoming difficult to manage and prohibitively expensive. What is the Google-recommended cloud native architecture for this scenario?

**Options:**
- A) Edge TPUs as sensor devices for storing and transmitting the messages.
- B) Cloud Dataflow connected to the Kafka cluster to scale the processing of incoming messages.
- C) An IoT gateway connected to Cloud Pub/Sub, with Cloud Dataflow to read and process the messages from Cloud Pub/Sub.
- D) A Kafka cluster virtualized on Compute Engine in us-east with Cloud Load Balancing to connect to the devices around the world.

**Memorize:** `C) An IoT gateway connected to Cloud Pub/Sub, with Cloud Dataflow to read and process the messages from Cloud Pub/Sub.`

<details>
<summary>Full explanation</summary>

Answer is 
An IoT gateway connected to Cloud Pub/Sub, with Cloud Dataflow to read and process the messages from Cloud Pub/Sub.

Alterative to Kafka in google cloud native service is Pub/Sub and Dataflow punched with Pub/Sub is the google recommended option

</details>

---

## Q118

**Prompt:** You have a petabyte of analytics data and need to design a storage and processing platform for it. You must be able to perform data warehouse-style analytics on the data in Google Cloud and expose the dataset as files for batch analysis tools in other cloud providers. What should you do?

**Options:**
- A) Store and process the entire dataset in BigQuery.
- B) Store and process the entire dataset in Cloud Bigtable.
- C) Store the full dataset in BigQuery, and store a compressed copy of the data in a Cloud Storage bucket.
- D) Store the warm data as files in Cloud Storage, and store the active data in BigQuery. Keep this ratio as 80% warm and 20% active.

**Memorize:** `C) Store the full dataset in BigQuery, and store a compressed copy of the data in a Cloud Storage bucket.`

<details>
<summary>Full explanation</summary>

Answer is 
Store the full dataset in BigQuery, and store a compressed copy of the data in a Cloud Storage bucket.

BigQuery for analytics processing and Cloud Storage for exposing the data as files

</details>

---

## Q119

**Prompt:** You use BigQuery as your centralized analytics platform. New data is loaded every day, and an ETL pipeline modifies the original data and prepares it for the final users. This ETL pipeline is regularly modified and can generate errors, but sometimes the errors are detected only after 2 weeks. You need to provide a method to recover from these errors, and your backups should be optimized for storage costs. How should you organize your data in BigQuery and store your backups?

**Options:**
- A) Organize your data in a single table, export, and compress and store the BigQuery data in Cloud Storage.
- B) Organize your data in separate tables for each month, and export, compress, and store the data in Cloud Storage.
- C) Organize your data in separate tables for each month, and duplicate your data on a separate dataset in BigQuery.
- D) Organize your data in separate tables for each month, and use snapshot decorators to restore the table to a time prior to the corruption.

**Memorize:** `B) Organize your data in separate tables for each month, and export, compress, and store the data in Cloud Storage.`

<details>
<summary>Full explanation</summary>

Answer is 
Organize your data in separate tables for each month, and export, compress, and store the data in Cloud Storage.

Store your data in different tables for specific time periods. This method ensures that you will need to restore only a subset of data to a new table, rather than a whole dataset.

Reference:

https://cloud.google.com/architecture/dr-scenarios-for-data#managed-database-services-on-gcp

</details>

---

## Q120

**Prompt:** You are building a new application that you need to collect data from in a scalable way. Data arrives continuously from the application throughout the day, and you expect to generate approximately 150 GB of JSON data per day by the end of the year. Your requirements are: - Decoupling producer from consumer - Space and cost-efficient storage of the raw ingested data, which is to be stored indefinitely - Near real-time SQL query - Maintain at least 2 years of historical data, which will be queried with SQL Which pipeline should you use to meet these requirements?

**Options:**
- A) Create an application that provides an API. Write a tool to poll the API and write data to Cloud Storage as gzipped JSON files.
- B) Create an application that writes to a Cloud SQL database to store the data. Set up periodic exports of the database to write to Cloud Storage and load into BigQuery.
- C) Create an application that publishes events to Cloud Pub/Sub, and create Spark jobs on Cloud Dataproc to convert the JSON data to Avro format, stored on HDFS on Persistent Disk.
- D) Create an application that publishes events to Cloud Pub/Sub, and create a Cloud Dataflow pipeline that transforms the JSON event payloads to Avro, writing the data to Cloud Storage and BigQuery.

**Memorize:** `D) Create an application that publishes events to Cloud Pub/Sub, and create a Cloud Dataflow pipeline that transforms the JSON event payloads to Avro, writing the data to Cloud Storage and BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 
Create an application that publishes events to Cloud Pub/Sub, and create a Cloud Dataflow pipeline that transforms the JSON event payloads to Avro, writing the data to Cloud Storage and BigQuery.

Because we have to be able to query over historical 2 years data only BigQuery address this issue and because we have lots of input data we have to use Dataflow for processing.

</details>

---

## Q121

**Prompt:** You have several Spark jobs that run on a Cloud Dataproc cluster on a schedule. Some of the jobs run in sequence, and some of the jobs run concurrently. You need to automate this process. What should you do?

**Options:**
- A) Create a Cloud Dataproc Workflow Template
- B) Create an initialization action to execute the jobs
- C) Create a Directed Acyclic Graph in Cloud Composer
- D) Create a Bash script that uses the Cloud SDK to create a cluster, execute jobs, and then tear down the cluster

**Memorize:** `C) Create a Directed Acyclic Graph in Cloud Composer`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Directed Acyclic Graph in Cloud Composer

Wokflow template is a template with no scheduling capabilities. For scheduling Composer will required to be used.

</details>

---

## Q122

**Prompt:** Data Analysts in your company have the Cloud IAM Owner role assigned to them in their projects to allow them to work with multiple GCP products in their projects. Your organization requires that all BigQuery data access logs be retained for 6 months. You need to ensure that only audit personnel in your company can access the data access logs for all projects. What should you do?

**Options:**
- A) Enable data access logs in each Data Analyst's project. Restrict access to Stackdriver Logging via Cloud IAM roles.
- B) Export the data access logs via a project-level export sink to a Cloud Storage bucket in the Data Analysts' projects. Restrict access to the Cloud Storage bucket.
- C) Export the data access logs via a project-level export sink to a Cloud Storage bucket in a newly created projects for audit logs. Restrict access to the project with the exported logs.
- D) Export the data access logs via an aggregated export sink to a Cloud Storage bucket in a newly created project for audit logs. Restrict access to the project that contains the exported logs.

**Memorize:** `D) Export the data access logs via an aggregated export sink to a Cloud Storage bucket in a newly created project for audit logs. Restrict access to the project that contains the exported logs.`

<details>
<summary>Full explanation</summary>

Answer is 
Export the data access logs via an aggregated export sink to a Cloud Storage bucket in a newly created project for audit logs. Restrict access to the project that contains the exported logs.

Aggregated log sink will create a single sink for all projects, the destination can be a google cloud storage, pub/sub topic, bigquery table or a cloud logging bucket. without aggregated sink this will be required to be done for each project individually which will be cumbersome.

Reference:

https://cloud.google.com/logging/docs/export/aggregated_sinks

</details>

---

## Q123

**Prompt:** You are operating a streaming Cloud Dataflow pipeline. Your engineers have a new version of the pipeline with a different windowing algorithm and triggering strategy. You want to update the running pipeline with the new version. You want to ensure that no data is lost during the update. What should you do?

**Options:**
- A) Update the Cloud Dataflow pipeline inflight by passing the --update option with the --jobName set to the existing job name
- B) Update the Cloud Dataflow pipeline inflight by passing the --update option with the --jobName set to a new unique job name
- C) Stop the Cloud Dataflow pipeline with the Cancel option. Create a new Cloud Dataflow job with the updated code
- D) Stop the Cloud Dataflow pipeline with the Drain option. Create a new Cloud Dataflow job with the updated code

**Memorize:** `D) Stop the Cloud Dataflow pipeline with the Drain option. Create a new Cloud Dataflow job with the updated code`

<details>
<summary>Full explanation</summary>

Answer is 
Stop the Cloud Dataflow pipeline with the Drain option. Create a new Cloud Dataflow job with the updated code

The pipe algorithm change considered as a major code change. So the job should be drained first then start a new job.

Reference:

https://cloud.google.com/dataflow/docs/guides/updating-a-pipeline

</details>

---

## Q124

**Prompt:** You are implementing several batch jobs that must be executed on a schedule. These jobs have many interdependent steps that must be executed in a specific order. Portions of the jobs involve executing shell scripts, running Hadoop jobs, and running queries in BigQuery. The jobs are expected to run for many minutes up to several hours. If the steps fail, they must be retried a fixed number of times. Which service should you use to manage the execution of these jobs?

**Options:**
- A) Cloud Scheduler
- B) Cloud Dataflow
- C) Cloud Functions
- D) Cloud Composer

**Memorize:** `D) Cloud Composer`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Composer 

All interdependent tasks need to be run through cloud composer whereas small/adhoc tasks need to be run via scheduler.

Cloud Scheduler is a fully managed enterprise-grade cron job scheduler.

Reference:

https://cloud.google.com/scheduler

</details>

---

## Q125

**Prompt:** You want to build a managed Hadoop system as your data lake. The data transformation process is composed of a series of Hadoop jobs executed in sequence. To accomplish the design of separating storage from compute, you decided to use the Cloud Storage connector to store all input data, output data, and intermediary data. However, you noticed that one Hadoop job runs very slowly with Cloud Dataproc, when compared with the on-premises bare-metal Hadoop environment (8-core nodes with 100-GB RAM). Analysis shows that this particular Hadoop job is disk I/O intensive. You want to resolve the issue. What should you do?

**Options:**
- A) Allocate sufficient memory to the Hadoop cluster, so that the intermediary data of that particular Hadoop job can be held in memory
- B) Allocate sufficient persistent disk space to the Hadoop cluster, and store the intermediate data of that particular Hadoop job on native HDFS
- C) Allocate more CPU cores of the virtual machine instances of the Hadoop cluster so that the networking bandwidth for each instance can scale up
- D) Allocate additional network interface card (NIC), and configure link aggregation in the operating system to use the combined throughput when working with Cloud Storage

**Memorize:** `B) Allocate sufficient persistent disk space to the Hadoop cluster, and store the intermediate data of that particular Hadoop job on native HDFS`

<details>
<summary>Full explanation</summary>

Answer is 
 B. Allocate sufficient persistent disk space to the Hadoop cluster, and store the intermediate data of that particular Hadoop job on native HDFS 

Local HDFS storage is a good option if:

Your jobs require a lot of metadata operations—for example, you have thousands of partitions and directories, and each file size is relatively small.

You modify the HDFS data frequently or you rename directories. (Cloud Storage objects are immutable, so renaming a directory is an expensive operation because it consists of copying all objects to a new key and deleting them afterwards.)

You heavily use the append operation on HDFS files.

You have workloads that involve heavy I/O. For example, you have a lot of partitioned writes, such as the following:

spark.read().write.partitionBy(...).parquet("gs://")

You have I/O workloads that are especially sensitive to latency. For example, you require single-digit millisecond latency per storage operation.

</details>

---

## Q126

**Prompt:** You need to deploy additional dependencies to all of a Cloud Dataproc cluster at startup using an existing initialization action. Company security policies require that Cloud Dataproc nodes do not have access to the Internet so public initialization actions cannot fetch resources. What should you do?

**Options:**
- A) Deploy the Cloud SQL Proxy on the Cloud Dataproc master
- B) Use an SSH tunnel to give the Cloud Dataproc cluster access to the Internet
- C) Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter
- D) Use Resource Manager to add the service account used by the Cloud Dataproc cluster to the Network User role

**Memorize:** `C) Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter`

<details>
<summary>Full explanation</summary>

Answer is 
Copy all dependencies to a Cloud Storage bucket within your VPC security perimeter

If you create a Dataproc cluster with internal IP addresses only, attempts to access the Internet in an initialization action will fail unless you have configured routes to direct the traffic through a NAT or a VPN gateway. Without access to the Internet, you can enable Private Google Access, and place job dependencies in Cloud Storage; cluster nodes can download the dependencies from Cloud Storage from internal IPs.

Reference:

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/init-actions

</details>

---

## Q127

**Prompt:** You work for a mid-sized enterprise that needs to move its operational system transaction data from an on-premises database to GCP. The database is about 20

TB in size. Which database should you choose?

**Options:**
- A) Cloud SQL
- B) Cloud Bigtable
- C) Cloud SpannerMost Voted
- D) Cloud Datastore

**Memorize:** `A) Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud SQL

Cloud SQL can store upto 30 TB.

Reference:

https://cloud.google.com/sql/docs/quotas#:~:text=Cloud%20SQL%20storage%20limits&text=Up%20to%2030%2C720%20GB%2C%20depending,for%20PostgreSQL%20or%20SQL%20Server.

</details>

---

## Q128

**Prompt:** You have data pipelines running on BigQuery, Cloud Dataflow, and Cloud Dataproc. You need to perform health checks and monitor their behavior, and then notify the team managing the pipelines if they fail. You also need to be able to work across multiple projects. Your preference is to use managed products of features of the platform. What should you do?

**Options:**
- A) Export the information to Cloud Stackdriver, and set up an Alerting policy
- B) Run a Virtual Machine in Compute Engine with Airflow, and export the information to Stackdriver
- C) Export the logs to BigQuery, and set up App Engine to read that information and send emails if you find a failure in the logs
- D) Develop an App Engine application to consume logs using GCP API calls, and send emails if you find a failure in the logs

**Memorize:** `A) Export the information to Cloud Stackdriver, and set up an Alerting policy`

<details>
<summary>Full explanation</summary>

Answer is 
Export the information to Cloud Stackdriver, and set up an Alerting policy

Monitoring does not only provide you with access to Dataflow-related metrics, but also lets you to create alerting policies and dashboards so you can chart time series of metrics and choose to be notified when these metrics reach specified values.

</details>

---

## Q129

**Prompt:** Suppose you have a table that includes a nested column called "city" inside a column called "person", but when you try to submit the following query in BigQuery, it gives you an error. SELECT person FROM `project1.example.table1` WHERE city = "London" How would you correct the error?

**Options:**
- A) Add ", UNNEST(person)" before the WHERE clause.
- B) Change "city" to "person.city".
- C) Change "person" to "city.person".
- D) Add ", UNNEST(city)" before the WHERE clause.

**Memorize:** `B) Change "city" to "person.city".`

<details>
<summary>Full explanation</summary>

Answer is 
Change "city" to "person.city".

The qestion is about nest nor repeated. Nested doesn't need unnest, while repeated do.

WITH
table1 AS (
SELECT
STRUCT('Elvis Presley' AS name,
'Buenos Aires' AS city,
"town1" AS town) AS person
UNION ALL
SELECT
STRUCT('Johnny Depp',
'London',
'Paris'))
SELECT
person
FROM
table1
WHERE
person.city="London"

Reference:

https://cloud.google.com/bigquery/docs/nested-repeated

</details>

---

## Q130

**Prompt:** Which of these statements about exporting data from BigQuery is false?

**Options:**
- A) To export more than 1 GB of data, you need to put a wildcard in the destination filename.
- B) The only supported export destination is Google Cloud Storage.
- C) Data can only be exported in JSON or Avro format.
- D) The only compression option available is GZIP.

**Memorize:** `C) Data can only be exported in JSON or Avro format.`

<details>
<summary>Full explanation</summary>

Answer is 
Data can only be exported in JSON or Avro format.

When you export data from BigQuery, note the following:

You cannot export table data to a local file, to Google Sheets, or to Google Drive. The only supported export location is Cloud Storage. For information on saving query results, see Downloading and saving query results.

You can export up to 1 GB of table data to a single file. If you are exporting more than 1 GB of data, use a wildcard to export the data into multiple files. When you export data to multiple files, the size of the files will vary.

You cannot export nested and repeated data in CSV format. Nested and repeated data is supported for Avro and JSON exports.

When you export data in JSON format, INT64 (integer) data types are encoded as JSON strings to preserve 64-bit precision when the data is read by other systems.

You cannot export data from multiple tables in a single export job.

You cannot choose a compression type other than GZIP when you export data using the Cloud Console or the classic BigQuery web UI.

Reference:

https://cloud.google.com/bigquery/docs/exporting-data#export_limitations

</details>

---

## Q131

**Prompt:** What are all of the BigQuery operations that Google charges for?

**Options:**
- A) Storage, queries, and streaming inserts
- B) Storage, queries, and loading data from a file
- C) Storage, queries, and exporting data
- D) Queries and streaming inserts

**Memorize:** `A) Storage, queries, and streaming inserts`

<details>
<summary>Full explanation</summary>

Answer is 
Storage, queries, and streaming inserts

All are charged, no charges for exporting and loading data in same region

</details>

---

## Q132

**Prompt:** Which of these statements about BigQuery caching is true?

**Options:**
- A) By default, a query's results are not cached.
- B) BigQuery caches query results for 48 hours.
- C) Query results are cached even if you specify a destination table.
- D) There is no charge for a query that retrieves its results from cache.

**Memorize:** `D) There is no charge for a query that retrieves its results from cache.`

<details>
<summary>Full explanation</summary>

Answer is 
There is no charge for a query that retrieves its results from cache.

A. By default, a query's results are not cached. (False)

B. BigQuery caches query results for 48 hours. (False - 24 hours)

C. Query results are cached even if you specify a destination table. False

When a destination table is specified in the job configuration, the Cloud Console, the bq command-line tool, or the API, the query results are not cached. 

https://cloud.google.com/bigquery/docs/cached-results#cache-exceptions

D. There is no charge for a query that retrieves its results from cache. (True)

Reference:

https://cloud.google.com/bigquery/docs/cached-results#pricing_and_quotas

</details>

---

## Q133

**Prompt:** Which of these sources can you not load data into BigQuery from?

**Options:**
- A) File upload
- B) Google Drive
- C) Google Cloud Storage
- D) Google Cloud SQL

**Memorize:** `D) Google Cloud SQL`

<details>
<summary>Full explanation</summary>

Answer is 
Google Cloud SQL

Only Cloud sql is not available to be used for loading data directly.

On console we can see that we can use file, bigtable, drive and GS.

</details>

---

## Q134

**Prompt:** How would you query specific partitions in a BigQuery table?

**Options:**
- A) Use the DAY column in the WHERE clause
- B) Use the EXTRACT(DAY) clause
- C) Use the __PARTITIONTIME pseudo-column in the WHERE clause
- D) Use DATE BETWEEN in the WHERE clause

**Memorize:** `C) Use the __PARTITIONTIME pseudo-column in the WHERE clause`

<details>
<summary>Full explanation</summary>

Answer is 
Use the __PARTITIONTIME pseudo-column in the WHERE clause

Logical partition column is queried using _partitiontime

Reference:

https://cloud.google.com/bigquery/docs/partitioned-tables#ingestion_time

</details>

---

## Q135

**Prompt:** Which SQL keyword can be used to reduce the number of columns processed by BigQuery?

**Options:**
- A) BETWEEN
- B) WHERE
- C) SELECT
- D) LIMIT

**Memorize:** `C) SELECT`

<details>
<summary>Full explanation</summary>

Answer is 
SELECT

Select can be used to only select few columns which will reduce query time and cost

</details>

---

## Q136

**Prompt:** To give a user read permission for only the first three columns of a table, which access control method would you use?

**Options:**
- A) Primitive role
- B) Predefined role
- C) Authorized view
- D) It's not possible to give access to only the first three columns of a table.

**Memorize:** `C) Authorized view`

<details>
<summary>Full explanation</summary>

Answer is 
Authorized view

Authorized views are used to provided restricted access
Reference:

https://cloud.google.com/iam/docs/understanding-roles

https://cloud.google.com/bigquery/docs/authorized-views

</details>

---

## Q137

**Prompt:** What are two methods that can be used to denormalize tables in BigQuery?

**Options:**
- A) 1) Split table into multiple tables; 2) Use a partitioned table
- B) 1) Join tables into one table; 2) Use nested repeated fields
- C) 1) Use a partitioned table; 2) Join tables into one table
- D) 1) Use nested repeated fields; 2) Use a partitioned table

**Memorize:** `B) 1) Join tables into one table; 2) Use nested repeated fields`

<details>
<summary>Full explanation</summary>

Answer is 
1) Join tables into one table; 2) Use nested repeated fields

Denormalization says join tables to create one table and nested repeated fields to make query run faster
Reference:

https://medium.com/@guillaumelbr13/how-to-denormalize-tables-in-bigquery-cd1677c0aeab

</details>

---

## Q138

**Prompt:** Which of these is not a supported method of putting data into a partitioned table?

**Options:**
- A) If you have existing data in a separate file for each day, then create a partitioned table and upload each file into the appropriate partition.
- B) Run a query to get the records for a specific day from an existing table and for the destination table, specify a partitioned table ending with the day in the format "$YYYYMMDD".
- C) Create a partitioned table and stream new records to it every day.
- D) Use ORDER BY to put a table's rows into chronological order and then change the table's type to "Partitioned".

**Memorize:** `D) Use ORDER BY to put a table's rows into chronological order and then change the table's type to "Partitioned".`

<details>
<summary>Full explanation</summary>

Answer is 
Use ORDER BY to put a table's rows into chronological order and then change the table's type to "Partitioned".

Once table is created, you cannot change it partitioned

</details>

---

## Q139

**Prompt:** Which of these numbers are adjusted by a neural network as it learns from a training dataset (select 2 answers)?

**Options:**
- A) Weights
- B) Biases
- C) Continuous features
- D) Input values

**Memorize:** `B) Biases`

<details>
<summary>Full explanation</summary>

Answers are;
 
Weights

B. Biases

The two are adjust to create a perfect model

</details>

---

## Q140

**Prompt:** Which TensorFlow function can you use to configure a categorical column if you don't know all of the possible values for that column?

**Options:**
- A) categorical_column_with_vocabulary_list
- B) categorical_column_with_hash_bucket
- C) categorical_column_with_unknown_values
- D) sparse_column_with_keys

**Memorize:** `B) categorical_column_with_hash_bucket`

<details>
<summary>Full explanation</summary>

Answer is 
categorical_column_with_hash_bucket

Vocabulary list is used when column values are known(incremental values are added as categorical columns starting from 0) and hash_bucket is used when you don’t know the values.

Reference:

https://www.tensorflow.org/tutorials/structured_data/feature_columns

</details>

---

## Q141

**Prompt:** Which Cloud Dataflow / Beam feature should you use to aggregate data in an unbounded data source every hour based on the time when the data entered the pipeline?

**Options:**
- A) An hourly watermark
- B) An event time trigger
- C) The with Allowed Lateness method
- D) A processing time trigger

**Memorize:** `D) A processing time trigger`

<details>
<summary>Full explanation</summary>

Answer is 
A processing time trigger

When collecting and grouping data into windows, Beam uses triggers to determine when to emit the aggregated results of each window.
Processing time triggers. These triggers operate on the processing time  the time when the data element is processed at any given stage in the pipeline.
Event time triggers. These triggers operate on the event time, as indicated by the timestamp on each data element. Beams default trigger is event time-based.

Reference:

https://beam.apache.org/documentation/programming-guide/#triggers

</details>

---

## Q142

**Prompt:** You are planning to use Google's Dataflow SDK to analyze customer data such as displayed below. Your project requirement is to extract only the customer name from the data source and then write to an output PCollection. Tom,555 X street - Tim,553 Y street - Sam, 111 Z street - Which operation is best suited for the above data processing requirement?

**Options:**
- A) ParDo
- B) Sink API
- C) Source API
- D) Data extraction

**Memorize:** `A) ParDo`

<details>
<summary>Full explanation</summary>

Answer is 
ParDo

In Google Cloud dataflow SDK, you can use the ParDo to extract only a customer name of each element in your PCollection.

Reference:

https://cloud.google.com/dataflow/model/par-do

</details>

---

## Q143

**Prompt:** Does Dataflow process batch data pipelines or streaming data pipelines?

**Options:**
- A) Only Batch Data Pipelines
- B) Both Batch and Streaming Data Pipelines
- C) Only Streaming Data Pipelines
- D) None of the above

**Memorize:** `B) Both Batch and Streaming Data Pipelines`

<details>
<summary>Full explanation</summary>

Answer is 
Both Batch and Streaming Data Pipelines

Dataflow is a unified processing model, and can execute both streaming and batch data pipelines

Reference:

https://cloud.google.com/dataflow/

</details>

---

## Q144

**Prompt:** The _________ for Cloud Bigtable makes it possible to use Cloud Bigtable in a Cloud Dataflow pipeline.

**Options:**
- A) Cloud Dataflow connector
- B) DataFlow SDK
- C) BiqQuery API
- D) BigQuery Data Transfer Service

**Memorize:** `A) Cloud Dataflow connector`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Dataflow connector

The Cloud Dataflow connector for Cloud Bigtable makes it possible to use Cloud Bigtable in a Cloud Dataflow pipeline. You can use the connector for both batch and streaming operations.

Reference:

https://cloud.google.com/bigtable/docs/dataflow-hbase

https://cloud.google.com/bigtable/docs/hbase-dataflow-java

</details>

---

## Q145

**Prompt:** The Dataflow SDKs have been recently transitioned into which Apache service?

**Options:**
- A) Apache Spark
- B) Apache Hadoop
- C) Apache Kafka
- D) Apache Beam

**Memorize:** `D) Apache Beam`

<details>
<summary>Full explanation</summary>

Answer is 
Apache Beam

Dataflow SDKs are being transitioned to Apache Beam, as per the latest Google directive

Reference:

https://cloud.google.com/dataflow/docs/

</details>

---

## Q146

**Prompt:** Which Java SDK class can you use to run your Dataflow programs locally?

**Options:**
- A) LocalRunner
- B) DirectPipelineRunner
- C) MachineRunner
- D) LocalPipelineRunner

**Memorize:** `B) DirectPipelineRunner`

<details>
<summary>Full explanation</summary>

Answer is 
DirectPipelineRunner

DirectPipelineRunner allows you to execute operations in the pipeline directly, without any optimization. Useful for small local execution and tests

Reference:

https://cloud.google.com/dataflow/java-sdk/JavaDoc/com/google/cloud/dataflow/sdk/runners/DirectPipelineRunner

https://beam.apache.org/documentation/runners/direct/

https://beam.apache.org/documentation/runners/direct/

</details>

---

## Q147

**Prompt:** Which of the following is NOT one of the three main types of triggers that Dataflow supports?

**Options:**
- A) Trigger based on element size in bytes
- B) Trigger that is a combination of other triggers
- C) Trigger based on element count
- D) Trigger based on time

**Memorize:** `A) Trigger based on element size in bytes`

<details>
<summary>Full explanation</summary>

Answer is 
Trigger based on element size in bytes

There are three major kinds of triggers that Dataflow supports: 1. Time-based triggers 2. Data-driven triggers. You can set a trigger to emit results from a window when that window has received a certain number of data elements. 3. Composite triggers. These triggers combine multiple time-based or data-driven triggers in some logical way

Reference:

https://cloud.google.com/dataflow/model/triggers

</details>

---

## Q148

**Prompt:** What Dataflow concept determines when a Window's contents should be output based on certain criteria being met?

**Options:**
- A) Sessions
- B) OutputCriteria
- C) Windows
- D) Triggers

**Memorize:** `D) Triggers`

<details>
<summary>Full explanation</summary>

Answer is 
Triggers

Triggers control when the elements for a specific key and window are output. As elements arrive, they are put into one or more windows by a Window transform and its associated WindowFn, and then passed to the associated Trigger to determine if the Windows contents should be output.

Reference:

https://cloud.google.com/dataflow/java-sdk/JavaDoc/com/google/cloud/dataflow/sdk/transforms/windowing/Trigger

</details>

---

## Q149

**Prompt:** When running a pipeline that has a BigQuery source, on your local machine, you continue to get permission denied errors. What could be the reason for that?

**Options:**
- A) Your gcloud does not have access to the BigQuery resources
- B) BigQuery cannot be accessed from local machines
- C) You are missing gcloud on your machine
- D) Pipelines cannot be run locally

**Memorize:** `A) Your gcloud does not have access to the BigQuery resources`

<details>
<summary>Full explanation</summary>

Answer is 
Your gcloud does not have access to the BigQuery resources

When reading from a Dataflow source or writing to a Dataflow sink using DirectPipelineRunner, the Cloud Platform account that you configured with the gcloud executable will need access to the corresponding source/sink

Reference:

https://cloud.google.com/dataflow/java-sdk/JavaDoc/com/google/cloud/dataflow/sdk/runners/DirectPipelineRunner

</details>

---

## Q150

**Prompt:** You are deploying a new storage system for your mobile application, which is a media streaming service. You decide the best fit is Google Cloud Datastore. You have entities with multiple properties, some of which can take on multiple values. For example, in the entity 'Movie' the property 'actors' and the property

'tags' have multiple values but the property 'date released' does not. A typical query would ask for all movies with actor= ordered by date_released or all movies with tag=Comedy ordered by date_released.How should you avoid a combinatorial explosion in the number of indexes?Manually configure the index in your index config as follows:Manually configure the index in your index config as follows:Set the following in your entity options: exclude_from_indexes = 'actors, tags'Set the following in your entity options: exclude_from_indexes = 'date_published'Click here for the answerDiscussReportAnswer isManually configure the index in your index configYou can circumvent the exploding index by manually configuring an index in your index configuration file.Reference:https://cloud.google.com/datastore/docs/concepts/indexes#index_limits< Previous PageNext Page >Quick access to all questions in this exam1-1011-2021-3031-4041-5051-6061-7071-8081-9091-100101-110111-120121-130131-140141-150151-160161-170171-180181-190191-200201-210211-220221-230231-240241-250251-260261-270271-280Back to top© 2024 Pass n Exam, Inc. ·Contact

**Options:**
- A) Manually configure the index in your index config as follows:
- B) Manually configure the index in your index config as follows:
- C) Set the following in your entity options: exclude_from_indexes = 'actors, tags'
- D) Set the following in your entity options: exclude_from_indexes = 'date_published'

**Memorize:** `A) Manually configure the index in your index config as follows:`

<details>
<summary>Full explanation</summary>

Answer is 
Manually configure the index in your index config

 

You can circumvent the exploding index by manually configuring an index in your index configuration file.

Reference:

https://cloud.google.com/datastore/docs/concepts/indexes#index_limits

</details>

---

## Q151

**Prompt:** You are updating the code for a subscriber to a Pub/Sub feed. You are concerned that upon deployment the subscriber may erroneously acknowledge messages, leading to message loss. Your subscriber is not set up to retain acknowledged messages.

**Options:**
- A) Set up the Pub/Sub emulator on your local machine. Validate the behavior of your new subscriber logic before deploying it to production.
- B) Create a Pub/Sub snapshot before deploying new subscriber code. Use a Seek operation to re-deliver messages that became available after the snapshot was created.
- C) Use Cloud Build for your deployment. If an error occurs after deployment, use a Seek operation to locate a timestamp logged by Cloud Build at the start of the deployment.
- D) Enable dead-lettering on the Pub/Sub topic to capture messages that aren't successfully acknowledged. If an error occurs after deployment, re-deliver any messages captured by the dead-letter queue.

**Memorize:** `B) Create a Pub/Sub snapshot before deploying new subscriber code. Use a Seek operation to re-deliver messages that became available after the snapshot was created.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Pub/Sub snapshot before deploying new subscriber code. Use a Seek operation to re-deliver messages that became available after the snapshot was created.

 

According to the second reference in the list below, a concern with deploying new subscriber code is that the new executable may erroneously acknowledge messages, leading to message loss. Incorporating snapshots into your deployment process gives you a way to recover from bugs in new subscriber code.

Answer cannot be C because To seek to a timestamp, you must first configure the subscription to retain acknowledged messages using retain-acked-messages. If retain-acked-messages is set, Pub/Sub retains acknowledged messages for 7 days.

References:

https://cloud.google.com/pubsub/docs/replay-message

https://cloud.google.com/pubsub/docs/replay-overview#seek_use_cases

</details>

---

## Q152

**Prompt:** You work for a large real estate firm and are preparing 6 TB of home sales data to be used for machine learning. You will use SQL to transform the data and use

BigQuery ML to create a machine learning model. You plan to use the model for predictions against a raw dataset that has not been transformed.

**Options:**
- A) When creating your model, use BigQuery's TRANSFORM clause to define preprocessing steps. At prediction time, use BigQuery's ML.EVALUATE clause without specifying any transformations on the raw input data.
- B) When creating your model, use BigQuery's TRANSFORM clause to define preprocessing steps. Before requesting predictions, use a saved query to transform your raw input data, and then use ML.EVALUATE.
- C) Use a BigQuery view to define your preprocessing logic. When creating your model, use the view as your model training data. At prediction time, use BigQuery's ML.EVALUATE clause without specifying any transformations on the raw input data.
- D) Preprocess all data using Dataflow. At prediction time, use BigQuery's ML.EVALUATE clause without specifying any further transformations on the input data.

**Memorize:** `A) When creating your model, use BigQuery's TRANSFORM clause to define preprocessing steps. At prediction time, use BigQuery's ML.EVALUATE clause without specifying any transformations on the raw input data.`

<details>
<summary>Full explanation</summary>

Answer is 
When creating your model, use BigQuery's TRANSFORM clause to define preprocessing steps. At prediction time, use BigQuery's ML.EVALUATE clause without specifying any transformations on the raw input data.

 

Using the TRANSFORM clause, you can specify all preprocessing during model creation. The preprocessing is automatically applied during the prediction and evaluation phases of machine learning.

Reference:

https://cloud.google.com/bigquery-ml/docs/bigqueryml-transform

</details>

---

## Q153

**Prompt:** You are analyzing the price of a company's stock. Every 5 seconds, you need to compute a moving average of the past 30 seconds' worth of data. You are reading data from Pub/Sub and using DataFlow to conduct the analysis.

**Options:**
- A) Use a fixed window with a duration of 5 seconds. Emit results by setting the following trigger: AfterProcessingTime.pastFirstElementInPane().plusDelayOf (Duration.standardSeconds(30))
- B) Use a fixed window with a duration of 30 seconds. Emit results by setting the following trigger: AfterWatermark.pastEndOfWindow().plusDelayOf (Duration.standardSeconds(5))
- C) Use a sliding window with a duration of 5 seconds. Emit results by setting the following trigger: AfterProcessingTime.pastFirstElementInPane().plusDelayOf (Duration.standardSeconds(30))
- D) Use a sliding window with a duration of 30 seconds and a period of 5 seconds. Emit results by setting the following trigger: AfterWatermark.pastEndOfWindow ()

**Memorize:** `D) Use a sliding window with a duration of 30 seconds and a period of 5 seconds. Emit results by setting the following trigger: AfterWatermark.pastEndOfWindow ()`

<details>
<summary>Full explanation</summary>

Answer is 
Use a sliding window with a duration of 30 seconds and a period of 5 seconds. Emit results by setting the following trigger: AfterWatermark.pastEndOfWindow ()

 

Sliding Window: Since you need to compute a moving average of the past 30 seconds' worth of data every 5 seconds, a sliding window is appropriate. A sliding window allows overlapping intervals and is well-suited for computing rolling aggregates.

Window Duration: The window duration should be set to 30 seconds to cover the required 30 seconds' worth of data for the moving average calculation.

Window Period: The window period or sliding interval should be set to 5 seconds to move the window every 5 seconds and recalculate the moving average with the latest data.

Trigger: The trigger should be set to AfterWatermark.pastEndOfWindow() to emit the computed moving average results when the watermark advances past the end of the window. This ensures that all data within the window is considered before emitting the result.

Reference:

https://cloud.google.com/dataflow/docs/concepts/streaming-pipelines#hopping-windows

</details>

---

## Q154

**Prompt:** You are designing a pipeline that publishes application events to a Pub/Sub topic. Although message ordering is not important, you need to be able to aggregate events across disjoint hourly intervals before loading the results to BigQuery for analysis.

**Options:**
- A) Create a Cloud Function to perform the necessary data processing that executes using the Pub/Sub trigger every time a new message is published to the topic.
- B) Schedule a Cloud Function to run hourly, pulling all available messages from the Pub/Sub topic and performing the necessary aggregations.
- C) Schedule a batch Dataflow job to run hourly, pulling all available messages from the Pub/Sub topic and performing the necessary aggregations.
- D) Create a streaming Dataflow job that reads continually from the Pub/Sub topic and performs the necessary aggregations using tumbling windows.

**Memorize:** `D) Create a streaming Dataflow job that reads continually from the Pub/Sub topic and performs the necessary aggregations using tumbling windows.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a streaming Dataflow job that reads continually from the Pub/Sub topic and performs the necessary aggregations using tumbling windows.

 

TUMBLE=> fixed windows.

HOP=> sliding windows.

SESSION=> session windows.

A streaming Dataflow job is the best way to process and load data from Pub/Sub to BigQuery in real time. This is because streaming Dataflow jobs can scale to handle large volumes of data, and they can perform aggregations using tumbling windows.

Reference:

https://cloud.google.com/dataflow/docs/concepts/streaming-pipelines#tumbling-windows

</details>

---

## Q155

**Prompt:** You are migrating an application that tracks library books and information about each book, such as author or year published, from an on-premises data warehouse to BigQuery.

**Options:**
- A) Keep the schema the same, maintain the different tables for the book and each of the attributes, and query as you are doing today.
- B) Create a table that is wide and includes a column for each attribute, including the author's first name, last name, date of birth, etc.
- C) Create a table that includes information about the books and authors, but nest the author fields inside the author column.
- D) Keep the schema the same, create a view that joins all of the tables, and always query the view.

**Memorize:** `C) Create a table that includes information about the books and authors, but nest the author fields inside the author column.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a table that includes information about the books and authors, but nest the author fields inside the author column.

 

Use nested and repeated fields to denormalize data storage which will increase query performance. BigQuery doesn't require a completely flat denormalization. You can use nested and repeated fields to maintain relationships.

Reference:

https://cloud.google.com/bigquery/docs/best-practices-performance-nested

</details>

---

## Q156

**Prompt:** You are using Bigtable to persist and serve stock market data for each of the major indices. To serve the trading application, you need to access only the most recent stock prices that are streaming in.

**Options:**
- A) Create one unique table for all of the indices, and then use the index and timestamp as the row key design.
- B) Create one unique table for all of the indices, and then use a reverse timestamp as the row key design.
- C) For each index, have a separate table and use a timestamp as the row key design.
- D) For each index, have a separate table and use a reverse timestamp as the row key design.

**Memorize:** `B) Create one unique table for all of the indices, and then use a reverse timestamp as the row key design.`

<details>
<summary>Full explanation</summary>

Answer is 
Create one unique table for all of the indices, and then use a reverse timestamp as the row key design.

 

If you usually retrieve the most recent records first, you can use a reversed timestamp in the row key by subtracting the timestamp from your programming language's maximum value for long integers (in Java, java.lang.Long.MAX_VALUE). With a reversed timestamp, the records will be ordered from most recent to least recent.

Reference:

https://cloud.google.com/bigtable/docs/schema-design#time-based

</details>

---

## Q157

**Prompt:** Your company is in the process of migrating its on-premises data warehousing solutions to BigQuery.

**Options:**
- A) Perform a DML INSERT, UPDATE, or DELETE to replicate each individual CDC record in real time directly on the reporting table.
- B) Insert each new CDC record and corresponding operation type to a staging table in real time.
- C) Periodically DELETE outdated records from the reporting table.
- D) Periodically use a DML MERGE to perform several DML INSERT, UPDATE, and DELETE operations at the same time on the reporting table.
- E) Insert each new CDC record and corresponding operation type in real time to the reporting table, and use a materialized view to expose only the newest version of each unique record.

**Memorize:** `D) Periodically use a DML MERGE to perform several DML INSERT, UPDATE, and DELETE operations at the same time on the reporting table.`

<details>
<summary>Full explanation</summary>

Answers are;

B. Insert each new CDC record and corresponding operation type to a staging table in real time.

D. Periodically use a DML MERGE to perform several DML INSERT, UPDATE, and DELETE operations at the same time on the reporting table.

 

Delta tables contain all change events for a particular table since the initial load. Having all change events available can be valuable for identifying trends, the state of the entities that a table represents at a particular moment, or change frequency.

The best way to merge data frequently and consistently is to use a MERGE statement, which lets you combine multiple INSERT, UPDATE, and DELETE statements into a single atomic operation.

Reference:

https://cloud.google.com/architecture/database-replication-to-bigquery-using-change-data-capture#overview_of_cdc_data_replication

</details>

---

## Q158

**Prompt:** You are loading CSV files from Cloud Storage to BigQuery. The files have known data quality issues, including mismatched data types, such as STRINGs and

INT64s in the same column, and inconsistent formatting of values such as phone numbers or addresses. You need to create the data pipeline to maintain data quality and perform the required cleansing and transformation. What should you do?

**Options:**
- A) Use Data Fusion to transform the data before loading it into BigQuery.
- B) Use Data Fusion to convert the CSV files to a self-describing data format, such as AVRO, before loading the data to BigQuery.
- C) Load the CSV files into a staging table with the desired schema, perform the transformations with SQL, and then write the results to the final destination table.
- D) Create a table with the desired schema, load the CSV files into the table, and perform the transformations in place using SQL.

**Memorize:** `A) Use Data Fusion to transform the data before loading it into BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Data Fusion to transform the data before loading it into BigQuery.

 

Data Fusion's advantages:

Visual interface: Offers a user-friendly interface for designing data pipelines without extensive coding, making it accessible to a wider range of users.

Built-in transformations: Includes a wide range of pre-built transformations to handle common data quality issues, such as:

Data type conversions

Data cleansing (e.g., removing invalid characters, correcting formatting)

Data validation (e.g., checking for missing values, enforcing constraints)

Data enrichment (e.g., adding derived fields, joining with other datasets)

Custom transformations: Allows for custom transformations using SQL or Java code for more complex cleaning tasks.

Scalability: Can handle large datasets efficiently, making it suitable for processing CSV files with potential data quality issues.

Integration with BigQuery: Integrates seamlessly with BigQuery, allowing for direct loading of transformed data.

Why other options are less suitable:

B. Converting to AVRO: While AVRO is a self-describing format, it doesn't inherently address data quality issues. Transformations would still be needed, and Data Fusion provides a more comprehensive environment for this.

C. Staging table: Requires manual SQL transformations, which can be time-consuming and error-prone for large datasets with complex data quality issues.

D. Transformations in place: Modifying data directly in the destination table can lead to data loss or corruption if errors occur. It's generally safer to keep raw data intact and perform transformations separately.

By using Data Fusion, you can create a robust and efficient data pipeline that addresses data quality issues upfront, ensuring that only clean and consistent data is loaded into BigQuery for accurate analysis and insights.

Reference:

https://cloud.google.com/data-fusion/docs/concepts/overview#:~:text=The%20Cloud%20Data%20Fusion%20web%20UI%20lets%20you%20to%20build%20scalable%20data%20integration%20solutions%20to%20clean%2C%20prepare%2C%20blend%2C%20transfer%2C%20and%20transform%20data%2C%20without%20having%20to%20manage%20the%20infrastructure
.

</details>

---

## Q159

**Prompt:** You are building a model to make clothing recommendations. You know a user's fashion preference is likely to change over time, so you build a data pipeline to stream new data back to the model as it becomes available. How should you use this data to train the model?

**Options:**
- A) Continuously retrain the model on just the new data.
- B) Continuously retrain the model on a combination of existing data and the new data.
- C) Train on the existing data while using the new data as your test set.
- D) Train on the new data while using the existing data as your test set.

**Memorize:** `B) Continuously retrain the model on a combination of existing data and the new data.`

<details>
<summary>Full explanation</summary>

Answer is 
Continuously retrain the model on a combination of existing data and the new data.

In Recommendation System - Matrix stored in database - which store the users rating and other details like how much time user spent on that page.We generally recommend on basis of matrix. And In production that matrix updated (or model get retrained once a day or once a week) - Because there could be new users rating. or we can say new data.

So model retrained fully i.e. on Existing Data + New Data => and generate a new matrix (user/item matrix)

Reference:

https://docs.aws.amazon.com/machine-learning/latest/dg/retraining-models-on-new-data.html

</details>

---

## Q160

**Prompt:** You are creating a model to predict housing prices. Due to budget constraints, you must run it on a single resource-constrained virtual machine. Which learning algorithm should you use?

**Options:**
- A) Linear regression
- B) Logistic classification
- C) Recurrent neural network
- D) Feedforward neural network

**Memorize:** `A) Linear regression`

<details>
<summary>Full explanation</summary>

Answer is 
Linear regression

A tip here to decide when a liner regression should be used or logistics regression needs to be used. If you are forecasting that is the values in the column that you are predicting is numeric, it is always liner regression. If you are classifying, that is buy or no buy, yes or no, you will be using logistics regression.

Reference:

https://towardsdatascience.com/predicting-house-prices-with-linear-regression-machine-learning-from-scratch-part-ii-47a0238aeac1

</details>

---

## Q161

**Prompt:** You need to store and analyze social media postings in Google BigQuery at a rate of 10,000 messages per minute in near real-time. Initially, design the application to use streaming inserts for individual postings. Your application also performs data aggregations right after the streaming inserts. You discover that the queries after streaming inserts do not exhibit strong consistency, and reports from the queries might miss in-flight data. How can you adjust your application design?

**Options:**
- A) Re-write the application to load accumulated data every 2 minutes.
- B) Convert the streaming insert code to batch load for individual messages.
- C) Load the original message to Google Cloud SQL, and export the table every hour to BigQuery via streaming inserts.
- D) Estimate the average latency for data availability after streaming inserts, and always run queries after waiting twice as long.

**Memorize:** `D) Estimate the average latency for data availability after streaming inserts, and always run queries after waiting twice as long.`

<details>
<summary>Full explanation</summary>

Answer is 
Estimate the average latency for data availability after streaming inserts, and always run queries after waiting twice as long.

The data is first comes to buffer and then written to Storage. If we are running queries in buffer we will face above mentioned issues. If we wait for the bigquery to write the data to storage then we won’t face the issue. So We need to wait till it’s written tio storage

</details>

---

## Q162

**Prompt:** Business owners at your company have given you a database of bank transactions. Each row contains the user ID, transaction type, transaction location, and transaction amount. They ask you to investigate what type of machine learning can be applied to the data. Which three machine learning applications can you use? (Choose three.)

**Options:**
- A) Supervised learning to determine which transactions are most likely to be fraudulent.
- B) Unsupervised learning to determine which transactions are most likely to be fraudulent.
- C) Clustering to divide the transactions into N categories based on feature similarity.
- D) Supervised learning to predict the location of a transaction.
- E) Reinforcement learning to predict the location of a transaction.
- F) F. Unsupervised learning to predict the location of a transaction.

**Memorize:** `D) Supervised learning to predict the location of a transaction.`

<details>
<summary>Full explanation</summary>

Answer is 
B-C-D

Fraud is not a feature, so unsupervised, location is given so supervised, Clustering can be done looking at the done with same features.

Say the model predict a location, guessing US or Sweden are both wrong when the answer is Canada. But US is closer, the distance from the correct location can be used to calculate a reward. Through reinforcement learning (E) the model could guess a location with better accuracy than supervised (D).

</details>

---

## Q163

**Prompt:** Your company has hired a new data scientist who wants to perform complicated analyses across very large datasets stored in Google Cloud Storage and in a Cassandra cluster on Google Compute Engine. The scientist primarily wants to create labelled data sets for machine learning projects, along with some visualization tasks. She reports that her laptop is not powerful enough to perform her tasks and it is slowing her down. You want to help her perform her tasks. What should you do?

**Options:**
- A) Run a local version of Jupiter on the laptop.
- B) Grant the user access to Google Cloud Shell.
- C) Host a visualization tool on a VM on Google Compute Engine.
- D) Deploy Google Cloud Datalab to a virtual machine (VM) on Google Compute Engine.

**Memorize:** `D) Deploy Google Cloud Datalab to a virtual machine (VM) on Google Compute Engine.`

<details>
<summary>Full explanation</summary>

Answer is 
Deploy Google Cloud Datalab to a virtual machine (VM) on Google Compute Engine.

Cloud Datalab is **packaged as a container and run in a VM (Virtual Machine) instance.**

VM creation**, running the container in that VM, and establishing a connection from your browser to the Cloud Datalab container, which allows you to open existing Cloud Datalab notebooks and create new notebooks**. Read through the introductory notebooks in the `/docs/intro` directory to get a sense of how a notebook is organized and executed.

Cloud Datalab uses **notebooks instead of the text files containing code. Notebooks bring together code, documentation written as markdown, and the results of code execution—whether as text, image or, HTML/JavaScript**.

Reference:

https://cloud.google.com/datalab/docs/quickstarts

</details>

---

## Q164

**Prompt:** You are building a model to predict whether or not it will rain on a given day. You have thousands of input features and want to see if you can improve training speed by removing some features while having a minimum effect on model accuracy. What can you do?

**Options:**
- A) Eliminate features that are highly correlated to the output labels.
- B) Combine highly co-dependent features into one representative feature.
- C) Instead of feeding in each feature individually, average their values in batches of 3.
- D) Remove the features that have null values for more than 50% of the training records.

**Memorize:** `B) Combine highly co-dependent features into one representative feature.`

<details>
<summary>Full explanation</summary>

Answer is 
Combine highly co-dependent features into one representative feature.

A: correlated to output means that feature can contribute a lot to the model. so not a good idea.

C: you need to run with almost same number, but you will iterate twice, once for averaging and second time to feed the averaged value.

D: removing features even if it 50% nulls is not good idea, unless you prove that it is not at all correlated to output. But this is nowhere so can remove.

</details>

---

## Q165

**Prompt:** Your company is performing data preprocessing for a learning algorithm in Google Cloud Dataflow. Numerous data logs are being are being generated during this step, and the team wants to analyze them. Due to the dynamic nature of the campaign, the data is growing exponentially every hour.

The data scientists have written the following code to read the data for a new key features in the logs. BigQueryIO.Read -

.named("ReadLogData")

.from("clouddataflow-readonly:samples.log_data") You want to improve the performance of this data read. What should you do?

**Options:**
- A) Specify the TableReference object in the code.
- B) Use .fromQuery operation to read specific fields from the table.
- C) Use of both the Google BigQuery TableSchema and TableFieldSchema classes.
- D) Call a transform that returns TableRow objects, where each element in the PCollection represents a single row in the table.

**Memorize:** `B) Use .fromQuery operation to read specific fields from the table.`

<details>
<summary>Full explanation</summary>

Answer is 
Use .fromQuery operation to read specific fields from the table.

BigQueryIO.read.from() directly reads the whole table from BigQuery. This function exports the whole table to temporary files in Google Cloud Storage, where it will later be read from. This requires almost no computation, as it only performs an export job, and later Dataflow reads from GCS (not from BigQuery).

BigQueryIO.read.fromQuery() executes a query and then reads the results received after the query execution. Therefore, this function is more time-consuming, given that it requires that a query is first executed (which will incur in the corresponding economic and computational costs).

Reference:

https://cloud.google.com/bigquery/docs/best-practices-costs#avoid_select_

</details>

---

## Q166

**Prompt:** Your company is streaming real-time sensor data from their factory floor into Bigtable and they have noticed extremely poor performance. How should the row key be redesigned to improve Bigtable performance on queries that populate real-time dashboards?

**Options:**
- A) Use a row key of the form <timestamp>.
- B) Use a row key of the form.
- C) Use a row key of the form#.
- D) Use a row key of the form >#<sensorid>#<timestamp>.

**Memorize:** `D) Use a row key of the form >#<sensorid>#<timestamp>.`

<details>
<summary>Full explanation</summary>

Answer is 
Use a row key of the form >#
#
.

Best practices of bigtable states that rowkey should not be only timestamp or have timestamp at starting. It’s better to have sensorid and timestamp as rowkey.

Reference:

https://cloud.google.com/bigtable/docs/schema-design

</details>

---

## Q167

**Prompt:** You are training a spam classifier. You notice that you are overfitting the training data. Which three actions can you take to resolve this problem? (Choose three.)

**Options:**
- A) Get more training examples
- B) Reduce the number of training examples
- C) Use a smaller set of features
- D) Use a larger set of features
- E) Increase the regularization parameters
- F) F. Decrease the regularization parameters

**Memorize:** `E) Increase the regularization parameters`

<details>
<summary>Full explanation</summary>

Answers are;

A. Get more training examples

C. Use a smaller set of features

E. Increase the regularization parameters

Prevent overfitting: less variables, regularisation, early ending on the training

Reference:

https://cloud.google.com/bigquery-ml/docs/preventing-overfitting

</details>

---

## Q168

**Prompt:** You have some data, which is shown in the graphic below. The two dimensions are X and Y, and the shade of each dot represents what class it is. You want to classify this data accurately using a linear algorithm. To do this you need to add a synthetic feature. What should the value of that feature be?

**Options:**
- A) X^2+Y^2
- B) X^2
- C) Y^2
- D) cos(X)

**Memorize:** `A) X^2+Y^2`

<details>
<summary>Full explanation</summary>

Answer is 
X^2+Y^2

Reference:

https://medium.com/@sachinkun21/using-a-linear-model-to-deal-with-nonlinear-dataset-c6ed0f7f3f51

</details>

---

## Q169

**Prompt:** You are building a data pipeline on Google Cloud. You need to prepare data using a casual method for a machine-learning process. You want to support a logistic regression model. You also need to monitor and adjust for null values, which must remain real-valued and cannot be removed. What should you do?

**Options:**
- A) Use Cloud Dataprep to find null values in sample source data. Convert all nulls to "˜none' using a Cloud Dataproc job.
- B) Use Cloud Dataprep to find null values in sample source data. Convert all nulls to 0 using a Cloud Dataprep job.
- C) Use Cloud Dataflow to find null values in sample source data. Convert all nulls to "˜none' using a Cloud Dataprep job.
- D) Use Cloud Dataflow to find null values in sample source data. Convert all nulls to 0 using a custom script.

**Memorize:** `B) Use Cloud Dataprep to find null values in sample source data. Convert all nulls to 0 using a Cloud Dataprep job.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Dataprep to find null values in sample source data. Convert all nulls to 0 using a Cloud Dataprep job.

Key phrases are "casual method", "need to replace null with real values", "logistic regression". Logistic regression works on numbers. Null need to be replaced with a number. And Cloud dataprep is best casual tool out of given options.

</details>

---

## Q170

**Prompt:** You are developing an application that uses a recommendation engine on Google Cloud. Your solution should display new videos to customers based on past views. Your solution needs to generate labels for the entities in videos that the customer has viewed. Your design must be able to provide very fast filtering suggestions based on data from other customer preferences on several TB of data. What should you do?

**Options:**
- A) Build and train a complex classification model with Spark MLlib to generate labels and filter the results. Deploy the models using Cloud Dataproc. Call the model from your application.
- B) Build and train a classification model with Spark MLlib to generate labels. Build and train a second classification model with Spark MLlib to filter results to match customer preferences. Deploy the models using Cloud Dataproc. Call the models from your application.
- C) Build an application that calls the Cloud Video Intelligence API to generate labels. Store data in Cloud Bigtable, and filter the predicted labels to match the user's viewing history to generate preferences.
- D) Build an application that calls the Cloud Video Intelligence API to generate labels. Store data in Cloud SQL, and join and filter the predicted labels to match the user's viewing history to generate preferences.

**Memorize:** `C) Build an application that calls the Cloud Video Intelligence API to generate labels. Store data in Cloud Bigtable, and filter the predicted labels to match the user's viewing history to generate preferences.`

<details>
<summary>Full explanation</summary>

Answer is 
Build an application that calls the Cloud Video Intelligence API to generate labels. Store data in Cloud Bigtable, and filter the predicted labels to match the user's viewing history to generate preferences.

1. Rather than building a new model - it is better to use Google provide APIs, here - Google Video Intelligence.

So option A and B rules out

2. Between SQL and Bigtable - Bigtable is the better option as Bigtable support row-key filtering. Joining the filters is not required.

Reference:

https://cloud.google.com/video-intelligence/docs/feature-label-detection

</details>

---

## Q171

**Prompt:** You are developing an application on Google Cloud that will automatically generate subject labels for users' blog posts. You are under competitive pressure to add this feature quickly, and you have no additional developer resources. No one on your team has experience with machine learning. What should you do?

**Options:**
- A) Call the Cloud Natural Language API from your application. Process the generated Entity Analysis as labels.
- B) Call the Cloud Natural Language API from your application. Process the generated Sentiment Analysis as labels.
- C) Build and train a text classification model using TensorFlow. Deploy the model using Cloud Machine Learning Engine. Call the model from your application and process the results as labels.
- D) Build and train a text classification model using TensorFlow. Deploy the model using a Kubernetes Engine cluster. Call the model from your application and process the results as labels.

**Memorize:** `A) Call the Cloud Natural Language API from your application. Process the generated Entity Analysis as labels.`

<details>
<summary>Full explanation</summary>

Answer is 
Call the Cloud Natural Language API from your application. Process the generated Entity Analysis as labels.

Entity analysis -> Identify entities within documents receipts, invoices, and contracts and label them by types such as date, person, contact information, organization, location, events, products, and media.

Sentiment analysis -> Understand the overall opinion, feeling, or attitude sentiment expressed in a block of text.

</details>

---

## Q172

**Prompt:** You're training a model to predict housing prices based on an available dataset with real estate properties. Your plan is to train a fully connected neural net, and you've discovered that the dataset contains latitude and longitude of the property. Real estate professionals have told you that the location of the property is highly influential on price, so you'd like to engineer a feature that incorporates this physical dependency. What should you do?

**Options:**
- A) Provide latitude and longitude as input vectors to your neural net.
- B) Create a numeric column from a feature cross of latitude and longitude.
- C) Create a feature cross of latitude and longitude, bucketize at the minute level and use L1 regularization during optimization.
- D) Create a feature cross of latitude and longitude, bucketize it at the minute level and use L2 regularization during optimization.

**Memorize:** `C) Create a feature cross of latitude and longitude, bucketize at the minute level and use L1 regularization during optimization.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a feature cross of latitude and longitude, bucketize at the minute level and use L1 regularization during optimization.

Use L1 regularization when you need to assign greater importance to more influential features. It shrinks less important feature to 0.

L2 regularization performs better when all input features influence the output & all with the weights are of equal size.

Reference:

https://developers.google.com/machine-learning/crash-course/regularization-for-sparsity/l1-regularization

</details>

---

## Q173

**Prompt:** You work for a bank. You have a labelled dataset that contains information on already granted loan application and whether these applications have been defaulted. You have been asked to train a model to predict default rates for credit applicants. What should you do?

**Options:**
- A) Increase the size of the dataset by collecting additional data.
- B) Train a linear regression to predict a credit default risk score.
- C) Remove the bias from the data and collect applications that have been declined loans.Most Voted
- D) Match loan applicants with their social profiles to enable feature engineering.

**Memorize:** `D) Match loan applicants with their social profiles to enable feature engineering.`

<details>
<summary>Full explanation</summary>

Answer is 
Match loan applicants with their social profiles to enable feature engineering.

Don't think its B because you don't have the defaulted rate in hand only defaulted or not. it's classification. you have to do feature engineering to prepare data for Linear regression first.

</details>

---

## Q174

**Prompt:** You are creating a new pipeline in Google Cloud to stream IoT data from Cloud Pub/Sub through Cloud Dataflow to BigQuery. While previewing the data, you notice that roughly 2% of the data appears to be corrupt. You need to modify the Cloud Dataflow pipeline to filter out this corrupt data. What should you do?

**Options:**
- A) Add a SideInput that returns a Boolean if the element is corrupt.
- B) Add a ParDo transform in Cloud Dataflow to discard corrupt elements.
- C) Add a Partition transform in Cloud Dataflow to separate valid data from corrupt data.
- D) Add a GroupByKey transform in Cloud Dataflow to group all of the valid data together and discard the rest.

**Memorize:** `B) Add a ParDo transform in Cloud Dataflow to discard corrupt elements.`

<details>
<summary>Full explanation</summary>

Answer is 
Add a ParDo transform in Cloud Dataflow to discard corrupt elements.

ParDo is used to do transformation and create side output

Reference:

https://beam.apache.org/documentation/programming-guide/

</details>

---

## Q175

**Prompt:** You need to create a data pipeline that copies time-series transaction data so that it can be queried from within BigQuery by your data science team for analysis. Every hour, thousands of transactions are updated with a new status. The size of the intitial dataset is 1.5 PB, and it will grow by 3 TB per day. The data is heavily structured, and your data science team will build machine learning models based on this data. You want to maximize performance and usability for your data science team. Which two strategies should you adopt? (Choose two.)

**Options:**
- A) Denormalize the data as must as possible.
- B) Preserve the structure of the data as much as possible.
- C) Use BigQuery UPDATE to further reduce the size of the dataset.
- D) Develop a data pipeline where status updates are appended to BigQuery instead of updated.
- E) Copy a daily snapshot of transaction data to Cloud Storage and store it as an Avro file. Use BigQuery's support for external data sources to query.

**Memorize:** `D) Develop a data pipeline where status updates are appended to BigQuery instead of updated.`

<details>
<summary>Full explanation</summary>

Answers are;

A. Denormalize the data as must as possible.

D. Develop a data pipeline where status updates are appended to BigQuery instead of updated.

Using BigQuery as an OLTP store is considered an anti-pattern. Because OLTP stores have a high volume of updates and deletes, they are a mismatch for the data warehouse use case. To decide which storage option best fits your use case, review the Cloud storage products table.

BigQuery is built for scale and can scale out as the size of the warehouse grows, so there is no need to delete older data. By keeping the entire history, you can deliver more insight on your business. If the storage cost is a concern, you can take advantage of BigQuery's long term storage pricing by archiving older data and using it for special analysis when the need arises. If you still have good reasons for dropping older data, you can use BigQuery's native support for date-partitioned tables and partition expiration. In other words, BigQuery can automatically delete older data.

Reference:

https://cloud.google.com/solutions/bigquery-data-warehouse#handling_change

</details>

---

## Q176

**Prompt:** You work for a manufacturing company that sources up to 750 different components, each from a different supplier. You've collected a labeled dataset that has on average 1000 examples for each unique component. Your team wants to implement an app to help warehouse workers recognize incoming components based on a photo of the component. You want to implement the first working version of this app (as Proof-Of-Concept) within a few working days. What should you do?

**Options:**
- A) Use Cloud Vision AutoML with the existing dataset.Most Voted
- B) Use Cloud Vision AutoML, but reduce your dataset twice.
- C) Use Cloud Vision API by providing custom labels as recognition hints.
- D) Train your own image recognition model leveraging transfer learning techniques.

**Memorize:** `B) Use Cloud Vision AutoML, but reduce your dataset twice.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Vision AutoML, but reduce your dataset twice.

POC is a small-scale experiments

</details>

---

## Q177

**Prompt:** You are working on a niche product in the image recognition domain. Your team has developed a model that is dominated by custom C++ TensorFlow ops your team has implemented. These ops are used inside your main training loop and are performing bulky matrix multiplications. It currently takes up to several days to train a model. You want to decrease this time significantly and keep the cost low by using an accelerator on Google Cloud. What should you do?

**Options:**
- A) Use Cloud TPUs without any additional adjustment to your code.
- B) Use Cloud TPUs after implementing GPU kernel support for your customs ops.
- C) Use Cloud GPUs after implementing GPU kernel support for your customs ops.
- D) Stay on CPUs, and increase the size of the cluster you're training your model on.

**Memorize:** `C) Use Cloud GPUs after implementing GPU kernel support for your customs ops.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud GPUs after implementing GPU kernel support for your customs ops.

TPU support Models with no custom TensorFlow operations inside the main training loop so Option-A and B are eliminated as question says that 'These ops are used inside your main training loop'

Now choices remain 'C' & 'D'. CPU is for Simple models that do not take long to train. Since question says that currently its taking up to several days to train a model and hence existing infra may be CPU and taking so many days. GPUs are for "Models with a significant number of custom TensorFlow operations that must run at least partially on CPUs" as question says that model is dominated by TensorFlow ops leading to correct option as 'C'

Reference:

https://cloud.google.com/tpu/docs/tpus

https://www.tensorflow.org/guide/create_op#gpu_kernels

</details>

---

## Q178

**Prompt:** You work on a regression problem in a natural language processing domain, and you have 100M labeled exmaples in your dataset. You have randomly shuffled your data and split your dataset into train and test samples (in a 90/10 ratio). After you trained the neural network and evaluated your model on a test set, you discover that the root-mean-squared error (RMSE) of your model is twice as high on the train set as on the test set. How should you improve the performance of your model?

**Options:**
- A) Increase the share of the test sample in the train-test split.
- B) Try to collect more data and increase the size of your dataset.
- C) Try out regularization techniques (e.g., dropout of batch normalization) to avoid overfitting.
- D) Increase the complexity of your model by, e.g., introducing an additional layer or increase sizing the size of vocabularies or n-grams used.Most Voted

**Memorize:** `C) Try out regularization techniques (e.g., dropout of batch normalization) to avoid overfitting.`

<details>
<summary>Full explanation</summary>

Answer is 
Try out regularization techniques (e.g., dropout of batch normalization) to avoid overfitting.

"Training error is small and test error is big" is an indication of overfitting.

</details>

---

## Q179

**Prompt:** A data scientist has created a BigQuery ML model and asks you to create an ML pipeline to serve predictions. You have a REST API application with the requirement to serve predictions for an individual user ID with latency under 100 milliseconds. You use the following query to generate predictions: SELECT predicted_label, user_id FROM ML.PREDICT (MODEL "dataset.model', table user_features). How should you create the ML pipeline?

**Options:**
- A) Add a WHERE clause to the query, and grant the BigQuery Data Viewer role to the application service account.Most Voted
- B) Create an Authorized View with the provided query. Share the dataset that contains the view with the application service account.
- C) Create a Cloud Dataflow pipeline using BigQueryIO to read results from the query. Grant the Dataflow Worker role to the application service account.
- D) Create a Cloud Dataflow pipeline using BigQueryIO to read predictions for all users from the query. Write the results to Cloud Bigtable using BigtableIO. Grant the Bigtable Reader role to the application service account so that the application can read predictions for individual users from Cloud Bigtable.

**Memorize:** `D) Create a Cloud Dataflow pipeline using BigQueryIO to read predictions for all users from the query. Write the results to Cloud Bigtable using BigtableIO. Grant the Bigtable Reader role to the application service account so that the application can read predictions for individual users from Cloud Bigtable.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Cloud Dataflow pipeline using BigQueryIO to read predictions for all users from the query. Write the results to Cloud Bigtable using BigtableIO. Grant the Bigtable Reader role to the application service account so that the application can read predictions for individual users from Cloud Bigtable.

The key reason for pick D is the 100ms requirement. Bigtable provides lowest latency

</details>

---

## Q180

**Prompt:** You work for an advertising company, and you've developed a Spark ML model to predict click-through rates at advertisement blocks. You've been developing everything at your on-premises data center, and now your company is migrating to Google Cloud. Your data center will be closing soon, so a rapid lift-and-shift migration is necessary. However, the data you've been using will be migrated to migrated to BigQuery. You periodically retrain your Spark ML models, so you need to migrate existing training pipelines to Google Cloud. What should you do?

**Options:**
- A) Use Cloud ML Engine for training existing Spark ML models
- B) Rewrite your models on TensorFlow, and start using Cloud ML Engine
- C) Use Cloud Dataproc for training existing Spark ML models, but start reading data directly from BigQuery
- D) Spin up a Spark cluster on Compute Engine, and train Spark ML models on the data exported from BigQuery

**Memorize:** `C) Use Cloud Dataproc for training existing Spark ML models, but start reading data directly from BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Dataproc for training existing Spark ML models, but start reading data directly from BigQuery

Use Cloud Dataproc for training existing Spark ML models, but start reading data directly from BigQuery

</details>

---

## Q181

**Prompt:** You work for a global shipping company. You want to train a model on 40 TB of data to predict which ships in each geographic region are likely to cause delivery delays on any given day. The model will be based on multiple attributes collected from multiple sources. Telemetry data, including location in GeoJSON format, will be pulled from each ship and loaded every hour. You want to have a dashboard that shows how many and which ships are likely to cause delays within a region. You want to use a storage solution that has native functionality for prediction and geospatial processing. Which storage solution should you use?

**Options:**
- A) BigQuery
- B) Cloud Bigtable
- C) Cloud Datastore
- D) Cloud SQL for PostgreSQL

**Memorize:** `A) BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
BigQuery

Geospatial and ML functionality is with bigquery

</details>

---

## Q182

**Prompt:** Your team is working on a binary classification problem. You have trained a support vector machine (SVM) classifier with default parameters, and received an area under the Curve (AUC) of 0.87 on the validation set. You want to increase the AUC of the model. What should you do?

**Options:**
- A) Perform hyperparameter tuning
- B) Train a classifier with deep neural networks, because neural networks would always beat SVMs
- C) Deploy the model and measure the real-world AUC; it's always higher because of generalization
- D) Scale predictions you get out of the model (tune a scaling factor as a hyperparameter) in order to get the highest AUC

**Memorize:** `A) Perform hyperparameter tuning`

<details>
<summary>Full explanation</summary>

Answer is 
Perform hyperparameter tuning

AUC is scale-invariant. It measures how well predictions are ranked, rather than their absolute values.

Reference:

https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc?hl=en

</details>

---

## Q183

**Prompt:** You need to choose a database to store time series CPU and memory usage for millions of computers. You need to store this data in one-second interval samples. Analysts will be performing real-time, ad hoc analytics against the database. You want to avoid being charged for every query executed and ensure that the schema design will allow for future growth of the dataset. Which database and data model should you choose?

**Options:**
- A) Create a table in BigQuery, and append the new samples for CPU and memory to the table
- B) Create a wide table in BigQuery, create a column for the sample value at each second, and update the row with the interval for each second
- C) Create a narrow table in Cloud Bigtable with a row key that combines the Computer Engine computer identifier with the sample time at each second
- D) Create a wide table in Cloud Bigtable with a row key that combines the computer identifier with the sample time at each minute, and combine the values for each second as column data.

**Memorize:** `C) Create a narrow table in Cloud Bigtable with a row key that combines the Computer Engine computer identifier with the sample time at each second`

<details>
<summary>Full explanation</summary>

Answer is 
Create a narrow table in Cloud Bigtable with a row key that combines the Computer Engine computer identifier with the sample time at each second

A tall and narrow table has a small number of events per row, which could be just one event, whereas a short and wide table has a large number of events per row. As explained in a moment, tall and narrow tables are best suited for time-series data.

For time series, you should generally use tall and narrow tables. This is for two reasons: Storing one event per row makes it easier to run queries against your data. Storing many events per row makes it more likely that the total row size will exceed the recommended maximum (see Rows can be big but are not infinite).

Reference:

https://cloud.google.com/bigtable/docs/schema-design-time-series#patterns_for_row_key_design

</details>

---

## Q184

**Prompt:** Why do you need to split a machine learning dataset into training data and test data?

**Options:**
- A) So you can try two different sets of features
- B) To make sure your model is generalized for more than just the training data
- C) To allow you to create unit tests in your code
- D) So you can use one dataset for a wide model and one for a deep model

**Memorize:** `B) To make sure your model is generalized for more than just the training data`

<details>
<summary>Full explanation</summary>

Answer is 
To make sure your model is generalized for more than just the training data

Splitting data ensures that the model created works with test data (similar features)

</details>

---

## Q185

**Prompt:** The CUSTOM tier for Cloud Machine Learning Engine allows you to specify the number of which types of cluster nodes?

**Options:**
- A) Workers
- B) Masters, workers, and parameter servers
- C) Workers and parameter servers
- D) Parameter servers

**Memorize:** `C) Workers and parameter servers`

<details>
<summary>Full explanation</summary>

Answer is 
Workers and parameter servers

Workers and parameter servers. You can't choose the number of master node.

Reference:

https://cloud.google.com/ai-platform/training/docs/machine-types#scale_tiers

</details>

---

## Q186

**Prompt:** Which software libraries are supported by Cloud Machine Learning Engine?

**Options:**
- A) Theano and TensorFlow
- B) Theano and Torch
- C) TensorFlow
- D) TensorFlow and TorchMost Voted

**Memorize:** `C) TensorFlow`

<details>
<summary>Full explanation</summary>

Answer is 
TensorFlow

Cloud Machine learning supports Tensflow, scikit=learn & XGBoost

Reference

https://cloud.google.com/ai-platform/docs/ml-solutions-overview#code_your_model

https://cloud.google.com/ai-platform/training/docs/overview

</details>

---

## Q187

**Prompt:** Which of the following statements about the Wide & Deep Learning model are true? (Select 2 answers.)

**Options:**
- A) The wide model is used for memorization, while the deep model is used for generalization.
- B) A good use for the wide and deep model is a recommender system.
- C) The wide model is used for generalization, while the deep model is used for memorization.
- D) A good use for the wide and deep model is a small-scale linear regression problem.

**Memorize:** `B) A good use for the wide and deep model is a recommender system.`

<details>
<summary>Full explanation</summary>

Answers are;
The wide model is used for memorization, while the deep model is used for generalization.

B. A good use for the wide and deep model is a recommender system.

Wide model is used for memorization and deep model is used for generalization to make model think like human, both needs to be used to create a recommender system like search.

Reference:

https://ai.googleblog.com/2016/06/wide-deep-learning-better-together-with.html

</details>

---

## Q188

**Prompt:** To run a TensorFlow training job on your own computer using Cloud Machine Learning Engine, what would your command start with?

**Options:**
- A) gcloud ml-engine local train
- B) gcloud ml-engine jobs submit training
- C) gcloud ml-engine jobs submit training local
- D) You can't run a TensorFlow program on your own computer using Cloud ML Engine .

**Memorize:** `A) gcloud ml-engine local train`

<details>
<summary>Full explanation</summary>

Answer is 
gcloud ml-engine local train

Train model in production like environment using Cloud ML engine

Reference:

https://cloud.google.com/sdk/gcloud/reference/ml-engine/local/train

</details>

---

## Q189

**Prompt:** If you want to create a machine learning model that predicts the price of a particular stock based on its recent price history, what type of estimator should you use?

**Options:**
- A) Unsupervised learning
- B) Regressor
- C) Classifier
- D) Clustering estimator

**Memorize:** `B) Regressor`

<details>
<summary>Full explanation</summary>

Answer is 
Regressor

Regression is the supervised learning task to model and predict numerical value

</details>

---

## Q190

**Prompt:** Suppose you have a dataset of images that are each labeled as to whether or not they contain a human face. To create a neural network that recognizes human faces in images using this labeled dataset, what approach would likely be the most effective?

**Options:**
- A) Use K-means Clustering to detect faces in the pixels.
- B) Use feature engineering to add features for eyes, noses, and mouths to the input data.
- C) Use deep learning by creating a neural network with multiple hidden layers to automatically detect features of faces.
- D) Build a neural network with an input layer of pixels, a hidden layer, and an output layer with two categories.

**Memorize:** `C) Use deep learning by creating a neural network with multiple hidden layers to automatically detect features of faces.`

<details>
<summary>Full explanation</summary>

Answer is 
Use deep learning by creating a neural network with multiple hidden layers to automatically detect features of faces.

Multiple layer to detect face

</details>

---

## Q191

**Prompt:** What are two of the characteristics of using online prediction rather than batch prediction?

**Options:**
- A) It is optimized to handle a high volume of data instances in a job and to run more complex models.
- B) Predictions are returned in the response message.
- C) Predictions are written to output files in a Cloud Storage location that you specify.
- D) It is optimized to minimize the latency of serving predictions.

**Memorize:** `D) It is optimized to minimize the latency of serving predictions.`

<details>
<summary>Full explanation</summary>

Answers are; 

B. Predictions are returned in the response message.

D. It is optimized to minimize the latency of serving predictions.

Realtime, quick response and low latency

Reference:

https://cloud.google.com/ai-platform/prediction/docs/online-vs-batch-prediction

</details>

---

## Q192

**Prompt:** Which of these are examples of a value in a sparse vector? (Select 2 answers.)

**Options:**
- A) [0, 5, 0, 0, 0, 0]
- B) [0, 0, 0, 1, 0, 0, 1]
- C) [0, 1]
- D) [1, 0, 0, 0, 0, 0, 0]

**Memorize:** `D) [1, 0, 0, 0, 0, 0, 0]`

<details>
<summary>Full explanation</summary>

Answers are; 

C. [0, 1]

D. [1, 0, 0, 0, 0, 0, 0]

Sparse vector contains only 0 and 1, whereas only one 1

</details>

---

## Q193

**Prompt:** How can you get a neural network to learn about relationships between categories in a categorical feature?

**Options:**
- A) Create a multi-hot column
- B) Create a one-hot column
- C) Create a hash bucket
- D) Create an embedding column

**Memorize:** `D) Create an embedding column`

<details>
<summary>Full explanation</summary>

Answer is 
Create an embedding column

References:

https://machinelearningmastery.com/how-to-prepare-categorical-data-for-deep-learning-in-python/

https://www.studyblue.com/notes/note/n/data-engineer-practice-exam/deck/21261127

</details>

---

## Q194

**Prompt:** Which Google Cloud Platform service is an alternative to Hadoop with Hive?

**Options:**
- A) Cloud Dataflow
- B) Cloud Bigtable
- C) BigQuery
- D) Cloud Datastore

**Memorize:** `C) BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
BigQuery

Apache Hive is a data warehouse software project built on top of Apache Hadoop for providing data summarization, query, and analysis.
Google BigQuery is an enterprise data warehouse.

Reference:

https://en.wikipedia.org/wiki/Apache_Hive

</details>

---

## Q195

**Prompt:** You want to use a database of information about tissue samples to classify future tissue samples as either normal or mutated. You are evaluating an unsupervised anomaly detection method for classifying the tissue samples. Which two characteristic support this method? (Choose two.)

**Options:**
- A) There are very few occurrences of mutations relative to normal samples.
- B) There are roughly equal occurrences of both normal and mutated samples in the database.
- C) You expect future mutations to have different features from the mutated samples in the database.
- D) You expect future mutations to have similar features to the mutated samples in the database.
- E) You already have labels for which samples are mutated and which are normal in the database.

**Memorize:** `D) You expect future mutations to have similar features to the mutated samples in the database.`

<details>
<summary>Full explanation</summary>

Answer is 
A and D

A: In any anomaly detection algorithm it is assumed a priori that you have much more "normal" samples than mutated ones, so that you can model normal patterns and detect patterns that are "off" that normal pattern. For that you will always need the no. of normal samples to be much bigger than the no. of mutated samples.

D: You expect future mutations to have similar features to the mutated samples in the database." - in other words: Expect future anomalies to be similar to the anomalies that we already have in database

</details>

---

## Q196

**Prompt:** You are working on a linear regression model on BigQuery ML to predict a customer's likelihood of purchasing your company's products. Your model uses a city name variable as a key predictive component. In order to train and serve the model, your data must be organized in columns. You want to prepare your data using the least amount of coding while maintaining the predictable variables.

**Options:**
- A) Create a new view with BigQuery that does not include a column with city information.
- B) Use SQL in BigQuery to transform the state column using a one-hot encoding method, and make each city a column with binary values.
- C) Use TensorFlow to create a categorical variable with a vocabulary list. Create the vocabulary file and upload that as part of your model to BigQuery ML.
- D) Use Cloud Data Fusion to assign each city to a region that is labeled as 1, 2, 3, 4, or 5, and then use that number to represent the city in the model.

**Memorize:** `B) Use SQL in BigQuery to transform the state column using a one-hot encoding method, and make each city a column with binary values.`

<details>
<summary>Full explanation</summary>

Answer is 
Use SQL in BigQuery to transform the state column using a one-hot encoding method, and make each city a column with binary values.

 

One-hot encoding is a common technique used to handle categorical data in machine learning. This approach will transform the city name variable into a series of binary columns, one for each city. Each row will have a "1" in the column corresponding to the city it represents and "0" in all other city columns. This method is effective for linear regression models as it enables the model to use city data as a series of numeric, binary variables. BigQuery supports SQL operations that can easily implement one-hot encoding, thus minimizing the amount of coding required and efficiently preparing the data for the model.

A removes the city information completely, losing a key predictive component.

C requires additional coding and infrastructure with TensorFlow and vocabulary files outside of what BigQuery already provides.

D transforms the distinct city values into numeric regions, losing granularity of the city data.

By using SQL within BigQuery to one-hot encode cities into multiple yes/no columns, the city data is maintained and formatted appropriately for the BigQuery ML linear regression model with minimal additional coding. This aligns with the requirements stated in the question.

Reference:

https://cloud.google.com/bigquery/docs/auto-preprocessing#one_hot_encoding

</details>

---

## Q197

**Prompt:** You work for a large financial institution that is planning to use Dialogflow to create a chatbot for the company's mobile app. You have reviewed old chat logs and tagged each conversation for intent based on each customer's stated intention for contacting customer service.

**Options:**
- A) Automate the 10 intents that cover 70% of the requests so that live agents can handle more complicated requests.
- B) Automate the more complicated requests first because those require more of the agents' time.
- C) Automate a blend of the shortest and longest intents to be representative of all intents.
- D) Automate intents in places where common words such as 'payment' appear only once so the software isn't confused.

**Memorize:** `A) Automate the 10 intents that cover 70% of the requests so that live agents can handle more complicated requests.`

<details>
<summary>Full explanation</summary>

Answer is 
Automate the 10 intents that cover 70% of the requests so that live agents can handle more complicated requests.

 

If your agent will be large or complex, start by building a dialog that only addresses the top level requests. Once the basic structure is established, iterate on the conversation paths to ensure you're covering all of the possible routes an end-user may take.

Therefore, you should initally automate the 70 % of the requests that are simpler before automating the more complicated ones.

Reference:

https://cloud.google.com/dialogflow/cx/docs/concept/agent-design#build-iteratively

</details>

---

## Q198

**Prompt:** You work on a regression problem in a natural language processing domain, and you have 100M labeled examples in your dataset. You have randomly shuffled your data and split your dataset into train and test samples (in a 90/10 ratio). After you trained the neural network and evaluated your model on a test set, you discover that the root-mean-squared error (RMSE) of your model is twice as high on the train set as on the test set.

**Options:**
- A) Increase the share of the test sample in the train-test split.
- B) Try to collect more data and increase the size of your dataset.
- C) Try out regularization techniques (e.g., dropout of batch normalization) to avoid overfitting.
- D) Increase the complexity of your model by, e.g., introducing an additional layer or increase sizing the size of vocabularies or n-grams used.

**Memorize:** `D) Increase the complexity of your model by, e.g., introducing an additional layer or increase sizing the size of vocabularies or n-grams used.`

<details>
<summary>Full explanation</summary>

Answer is 
Increase the complexity of your model by, e.g., introducing an additional layer or increase sizing the size of vocabularies or n-grams used.

 

"root-mean-squared error (RMSE) of your model is twice as high on the train set as on the test set." means the RMSE of training set is two time of RMSE of test set, which indicates the training is not as good as test, then underfitting.

Reference:

https://stats.stackexchange.com/questions/497050/how-big-a-difference-for-test-train-rmse-is-considered-as-overfit#:~:text=RMSE%20of%20test%20%3C%20RMSE%20of
,is%20always%20overfit%20or%20underfit.

</details>

---

## Q199

**Prompt:** You are developing a new deep learning model that predicts a customer's likelihood to buy on your ecommerce site.

**Options:**
- A) Increase the size of the training dataset, and increase the number of input features.
- B) Increase the size of the training dataset, and decrease the number of input features.
- C) Reduce the size of the training dataset, and increase the number of input features.
- D) Reduce the size of the training dataset, and decrease the number of input features.

**Memorize:** `B) Increase the size of the training dataset, and decrease the number of input features.`

<details>
<summary>Full explanation</summary>

Answer is 
Increase the size of the training dataset, and decrease the number of input features.

 

There 2 parts and they are relevant to each other

1. Overfit is fixed by decreasing the number of input features (select only essential features)

2. Accuracy is improved by increasing the amount of training data examples.

Reference:

https://docs.aws.amazon.com/machine-learning/latest/dg/model-fit-underfitting-vs-overfitting.html

</details>

---

## Q200

**Prompt:** You are implementing a chatbot to help an online retailer streamline their customer service. The chatbot must be able to respond to both text and voice inquiries.

**Options:**
- A) Use the Cloud Speech-to-Text API to build a Python application in App Engine.
- B) Use the Cloud Speech-to-Text API to build a Python application in a Compute Engine instance.
- C) Use Dialogflow for simple queries and the Cloud Speech-to-Text API for complex queries.
- D) Use Dialogflow to implement the chatbot, defining the intents based on the most common queries collected.

**Memorize:** `D) Use Dialogflow to implement the chatbot, defining the intents based on the most common queries collected.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Dialogflow to implement the chatbot, defining the intents based on the most common queries collected.

 

Dialogflow is a conversational AI platform that allows for easy implementation of chatbots without needing to code. It has built-in integration for both text and voice input via APIs like Cloud Speech-to-Text. Defining intents and entity types allows you to map common queries and keywords to responses. This would provide a low/no-code way to quickly build and iteratively improve the chatbot capabilities.

Option A and B would require more heavy coding to handle speech input/output. Option C still requires coding the complex query handling. Only option D leverages the full capabilities of Dialogflow to enable no-code chatbot development and ongoing improvements as more conversational data is collected. Hence, option D is the best approach given the requirements.

Reference:

https://cloud.google.com/dialogflow/es/docs/how/detect-intent-tts#:~:text=Dialogflow%20can%20use%20Cloud%20Text
,to%2Dspeech%2C%20or%20TTS.

</details>

---

## Q201

**Prompt:** You designed a database for patient records as a pilot project to cover a few hundred patients in three clinics. Your design used a single database table to represent all patients and their visits, and you used self-joins to generate reports. The server resource utilization was at 50%. Since then, the scope of the project has expanded. The database must now store 100 times more patient records. You can no longer run the reports, because they either take too long or they encounter errors with insufficient compute resources. How should you adjust the database design?

**Options:**
- A) Add capacity (memory and disk space) to the database server by the order of 200.
- B) Shard the tables into smaller ones based on date ranges, and only generate reports with prespecified date ranges.
- C) Normalize the master patient-record table into the patient table and the visits table, and create other necessary tables to avoid self-join.
- D) Partition the table into smaller tables, with one for each clinic. Run queries against the smaller table pairs, and use unions for consolidated reports.

**Memorize:** `C) Normalize the master patient-record table into the patient table and the visits table, and create other necessary tables to avoid self-join.`

<details>
<summary>Full explanation</summary>

Answer is 
Normalize the master patient-record table into the patient table and the visits table, and create other necessary tables to avoid self-join.

C is correct because this option provides the least amount of inconvenience over using pre-specified date ranges or one table per clinic while also increasing performance due to avoiding self-joins.

Reference:

https://cloud.google.com/bigquery/docs/best-practices-performance-patterns

</details>

---

## Q202

**Prompt:** Your company is using WILDCARD tables to query data across multiple tables with similar names. The SQL statement is currently failing with the following error: # Syntax error : Expected end of statement but got "-" at [4:11] SELECT age

FROM bigquery-public-data.noaa_gsod.gsod

WHERE

  age != 99

  AND_TABLE_SUFFIX = "˜1929'

ORDER BY age DESC Which table name will make the SQL statement work correctly?

**Options:**
- A) "˜bigquery-public-data.noaa_gsod.gsod"˜
- B) bigquery-public-data.noaa_gsod.gsod*
- C) "˜bigquery-public-data.noaa_gsod.gsod'*
- D) `bigquery-public-data.noaa_gsod.gsod*`

**Memorize:** `D) `bigquery-public-data.noaa_gsod.gsod*``

<details>
<summary>Full explanation</summary>

Answer is 

 it follows the correct wildcard syntax of enclosing the table name in backticks and including the * wildcard character.

</details>

---

## Q203

**Prompt:** Your company is in a highly regulated industry. One of your requirements is to ensure individual users have access only to the minimum amount of information required to do their jobs. You want to enforce this requirement with Google BigQuery. Which three approaches can you take? (Choose three.)

**Options:**
- A) Disable writes to certain tables.
- B) Restrict access to tables by role.
- C) Ensure that the data is encrypted at all times.
- D) Restrict BigQuery API access to approved users.
- E) Segregate data across multiple tables or databases.
- F) Use Google Stackdriver Audit Logging to determine policy violations.

**Memorize:** `E) Segregate data across multiple tables or databases.`

<details>
<summary>Full explanation</summary>

Answer is 

B. Restrict access to tables by role.

D. Restrict BigQuery API access to approved users.

E. Segregate data across multiple tables or databases.

B. Restrict access to tables by role: This approach can be used to control access to tables based on user roles. Access controls can be set at the project, dataset, and table level, and roles can be customized to provide granular access controls to different groups of users.

D. Restrict BigQuery API access to approved users: This approach involves using IAM (Identity and Access Management) to control access to the BigQuery API. Access can be granted or revoked at the project or dataset level, and policies can be customized to control access based on user roles, IP addresses, and other factors.

E. Segregate data across multiple tables or databases: This approach involves breaking down large datasets into smaller, more manageable tables or databases. This helps to ensure that individual users have access only to the minimum amount of information required to do their jobs, and reduces the risk of data breaches or policy violations.

Option A is incorrect because disabling writes to certain tables would prevent users from updating the data which is not in line with the goal of providing access to the minimum amount of information required to do their jobs.

Option C is incorrect because while data encryption is important for security it doesn't specifically help with providing users access to the minimum amount of information required to do their jobs.

Option F is incorrect because while Google Stackdriver Audit Logging can help to determine policy violations it does not help to enforce the access controls and segregation of data.

</details>

---

## Q204

**Prompt:** Your company handles data processing for a number of different clients. Each client prefers to use their own suite of analytics tools, with some allowing direct query access via Google BigQuery. You need to secure the data so that clients cannot see each other's data. You want to ensure appropriate access to the data. Which three steps should you take? (Choose three.)

**Options:**
- A) Load data into different partitions.
- B) Load data into a different dataset for each client.
- C) Put each client's BigQuery dataset into a different table.
- D) Restrict a client's dataset to approved users.
- E) Only allow a service account to access the datasets.
- F) F. Use the appropriate identity and access management (IAM) roles for each client's users.

**Memorize:** `F) F. Use the appropriate identity and access management (IAM) roles for each client's users.`

<details>
<summary>Full explanation</summary>

Answer is 
B-D-F

</details>

---

## Q205

**Prompt:** You want to use Google Stackdriver Logging to monitor Google BigQuery usage. You need an instant notification to be sent to your monitoring tool when new data is appended to a certain table using an insert job, but you do not want to receive notifications for other tables. What should you do?

**Options:**
- A) Make a call to the Stackdriver API to list all logs, and apply an advanced filter.
- B) In the Stackdriver logging admin interface, and enable a log sink export to BigQuery.
- C) In the Stackdriver logging admin interface, enable a log sink export to Google Cloud Pub/Sub, and subscribe to the topic from your monitoring tool.
- D) Using the Stackdriver API, create a project sink with advanced log filter to export to Pub/Sub, and subscribe to the topic from your monitoring tool.

**Memorize:** `D) Using the Stackdriver API, create a project sink with advanced log filter to export to Pub/Sub, and subscribe to the topic from your monitoring tool.`

<details>
<summary>Full explanation</summary>

Answer is 
Using the Stackdriver API, create a project sink with advanced log filter to export to Pub/Sub, and subscribe to the topic from your monitoring tool.

Using a Logging sink, you can build an event-driven system to detect and respond to log events in real time. Cloud Logging can help you to build this event-driven architecture through its integration with Cloud Pub/Sub and a serverless computing service such as Cloud Functions or Cloud Run.

However as we need to create an alert only for certain table we will need to go with advanced log queries to filter only required log.

Reference:

https://cloud.google.com/blog/products/management-tools/automate-your-response-to-a-cloud-logging-event

</details>

---

## Q206

**Prompt:** Your company's customer and order databases are often under heavy load. This makes performing analytics against them difficult without harming operations. The databases are in a MySQL cluster, with nightly backups taken using mysqldump. You want to perform analytics with minimal impact on operations. What should you do?

**Options:**
- A) Add a node to the MySQL cluster and build an OLAP cube there.
- B) Use an ETL tool to load the data from MySQL into Google BigQuery.Most Voted
- C) Connect an on-premises Apache Hadoop cluster to MySQL and perform ETL.
- D) Mount the backups to Google Cloud SQL, and then process the data using Google Cloud Dataproc.

**Memorize:** `D) Mount the backups to Google Cloud SQL, and then process the data using Google Cloud Dataproc.`

<details>
<summary>Full explanation</summary>

Answer is 
Mount the backups to Google Cloud SQL, and then process the data using Google Cloud Dataproc.

A: OLAP on MySQL performs poorly.

B: ETL consumes lot of MySQL resources, to read the data, as per question MySQL is under pressure already.

C: Similar to B.

D: By mounting backup can avoid reading from MySQL, data freshness is not an issue as per the question (and is not mention in the question).

Reference:

https://cloud.google.com/blog/products/data-analytics/genomics-data-analytics-with-cloud-pt2

</details>

---

## Q207

**Prompt:** Your company is running their first dynamic campaign, serving different offers by analyzing real-time data during the holiday season. The data scientists are collecting terabytes of data that rapidly grows every hour during their 30-day campaign. They are using Google Cloud Dataflow to preprocess the data and collect the feature (signals) data that is needed for the machine learning model in Google Cloud Bigtable. The team is observing suboptimal performance with reads and writes of their initial load of 10 TB of data. They want to improve this performance while minimizing cost. What should they do?

**Options:**
- A) Redefine the schema by evenly distributing reads and writes across the row space of the table.
- B) The performance issue should be resolved over time as the site of the BigDate cluster is increased.
- C) Redesign the schema to use a single row key to identify values that need to be updated frequently in the cluster.
- D) Redesign the schema to use row keys based on numeric IDs that increase sequentially per user viewing the offers.

**Memorize:** `A) Redefine the schema by evenly distributing reads and writes across the row space of the table.`

<details>
<summary>Full explanation</summary>

Answer is 
Redefine the schema by evenly distributing reads and writes across the row space of the table.

If you find that you're reading and writing only a small number of rows, you might need to redesign your schema so that reads and writes are more evenly distributed.

Reference:

https://cloud.google.com/bigtable/docs/performance#troubleshooting

</details>

---

## Q208

**Prompt:** Your company has recently grown rapidly and now ingesting data at a significantly higher rate than it was previously. You manage the daily batch MapReduce analytics jobs in Apache Hadoop. However, the recent increase in data has meant the batch jobs are falling behind. You were asked to recommend ways the development team could increase the responsiveness of the analytics without increasing costs. What should you recommend they do?

**Options:**
- A) Rewrite the job in Pig.
- B) Rewrite the job in Apache Spark.
- C) Increase the size of the Hadoop cluster.
- D) Decrease the size of the Hadoop cluster but also rewrite the job in Hive.

**Memorize:** `B) Rewrite the job in Apache Spark.`

<details>
<summary>Full explanation</summary>

Answer is 
Rewrite the job in Apache Spark.

The objective is to not increase the cost at the sametime do the analyitics required. Mapreduce jobs are not efficient and fast as spark so it will avoid failing the jobs.

</details>

---

## Q209

**Prompt:** You work for a manufacturing plant that batches application log files together into a single log file once a day at 2:00 AM. You have written a Google Cloud Dataflow job to process that log file. You need to make sure the log file in processed once per day as inexpensively as possible. What should you do?

**Options:**
- A) Change the processing job to use Google Cloud Dataproc instead.
- B) Manually start the Cloud Dataflow job each morning when you get into the office.
- C) Create a cron job with Google App Engine Cron Service to run the Cloud Dataflow job.
- D) Configure the Cloud Dataflow job as a streaming job so that it processes the log data immediately.

**Memorize:** `C) Create a cron job with Google App Engine Cron Service to run the Cloud Dataflow job.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a cron job with Google App Engine Cron Service to run the Cloud Dataflow job.

Scheduler for adhoc jobs – 3 jobs free and $0.10 per job

Reference:

https://cloud.google.com/appengine/docs/flexible/nodejs/scheduling-jobs-with-cron-yaml

</details>

---

## Q210

**Prompt:** Your company is loading comma-separated values (CSV) files into Google BigQuery. The data is fully imported successfully; however, the imported data is not matching byte-to-byte to the source file. What is the most likely cause of this problem?

**Options:**
- A) The CSV data loaded in BigQuery is not flagged as CSV.
- B) The CSV data has invalid rows that were skipped on import.
- C) The CSV data loaded in BigQuery is not using BigQuery's default encoding.
- D) The CSV data has not gone through an ETL phase before loading into BigQuery.

**Memorize:** `C) The CSV data loaded in BigQuery is not using BigQuery's default encoding.`

<details>
<summary>Full explanation</summary>

Answer is 
The CSV data loaded in BigQuery is not using BigQuery's default encoding.

If you don't specify an encoding, or if you specify UTF-8 encoding when the CSV file is not UTF-8 encoded, BigQuery attempts to convert the data to UTF-8. Generally, your data will be loaded successfully, but it may not match byte-for-byte what you expect.

Reference:

https://cloud.google.com/bigquery/docs/loading-data-cloud-storage-csv#details_of_loading_csv_data

</details>

---

## Q211

**Prompt:** You are implementing security best practices on your data pipeline. Currently, you are manually executing jobs as the Project Owner. You want to automate these jobs by taking nightly batch files containing non-public information from Google Cloud Storage, processing them with a Spark Scala job on a Google Cloud Dataproc cluster, and depositing the results into Google BigQuery. How should you securely run this workload?

**Options:**
- A) Restrict the Google Cloud Storage bucket so only you can see the files
- B) Grant the Project Owner role to a service account, and run the job with it
- C) Use a service account with the ability to read the batch files and to write to BigQuery
- D) Use a user account with the Project Viewer role on the Cloud Dataproc cluster to read the batch files and write to BigQuery

**Memorize:** `C) Use a service account with the ability to read the batch files and to write to BigQuery`

<details>
<summary>Full explanation</summary>

Answer is 
Use a service account with the ability to read the batch files and to write to BigQuery

As service account invoked to read the data into GCS and write to BQ once transformed via Data Proc. Assumes DataProc can inherit SA authorisation to perform transform and propagate.

Reference:

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/service-accounts#dataproc_service_accounts_2

</details>

---

## Q212

**Prompt:** Your company receives both batch- and stream-based event data. You want to process the data using Google Cloud Dataflow over a predictable time period. However, you realize that in some instances data can arrive late or out of order. How should you design your Cloud Dataflow pipeline to handle data that is late or out of order?

**Options:**
- A) Set a single global window to capture all the data.
- B) Set sliding windows to capture all the lagged data.
- C) Use watermarks and timestamps to capture the lagged data.
- D) Ensure every datasource type (stream or batch) has a timestamp, and use the timestamps to define the logic for lagged data.

**Memorize:** `C) Use watermarks and timestamps to capture the lagged data.`

<details>
<summary>Full explanation</summary>

Answer is 
Use watermarks and timestamps to capture the lagged data.

A watermark is a threshold that indicates when Dataflow expects all of the data in a window to have arrived. If new data arrives with a timestamp that's in the window but older than the watermark, the data is considered late data.

</details>

---

## Q213

**Prompt:** You set up a streaming data insert into a Redis cluster via a Kafka cluster. Both clusters are running on Compute Engine instances. You need to encrypt data at rest with encryption keys that you can create, rotate, and destroy as needed. What should you do?

**Options:**
- A) Create a dedicated service account, and use encryption at rest to reference your data stored in your Compute Engine cluster instances as part of your API service calls.
- B) Create encryption keys in Cloud Key Management Service. Use those keys to encrypt your data in all of the Compute Engine cluster instances.
- C) Create encryption keys locally. Upload your encryption keys to Cloud Key Management Service. Use those keys to encrypt your data in all of the Compute Engine cluster instances.
- D) Create encryption keys in Cloud Key Management Service. Reference those keys in your API service calls when accessing the data in your Compute Engine cluster instances.

**Memorize:** `B) Create encryption keys in Cloud Key Management Service. Use those keys to encrypt your data in all of the Compute Engine cluster instances.`

<details>
<summary>Full explanation</summary>

Answer is 
Create encryption keys in Cloud Key Management Service. Use those keys to encrypt your data in all of the Compute Engine cluster instances.

KMS stored on cloud helps in creating, rotating and destroying keys as needed and also it can be used to encrypt compute engines.

Reference:

https://cloud.google.com/security/encryption/customer-supplied-encryption-keys

</details>

---

## Q214

**Prompt:** An organization maintains a Google BigQuery dataset that contains tables with user-level data. They want to expose aggregates of this data to other Google Cloud projects, while still controlling access to the user-level data. Additionally, they need to minimize their overall storage cost and ensure the analysis cost for other projects is assigned to those projects. What should they do?

**Options:**
- A) Create and share an authorized view that provides the aggregate results.Most Voted
- B) Create and share a new dataset and view that provides the aggregate results.
- C) Create and share a new dataset and table that contains the aggregate results.
- D) Create dataViewer Identity and Access Management (IAM) roles on the dataset to enable sharing.

**Memorize:** `B) Create and share a new dataset and view that provides the aggregate results.`

<details>
<summary>Full explanation</summary>

Answer is 
Create and share a new dataset and view that provides the aggregate results.

Control access on a view we need to store in a separate dataset

Reference:

https://cloud.google.com/bigquery/docs/share-access-views#create_the_view_in_the_new_dataset

</details>

---

## Q215

**Prompt:** Your neural network model is taking days to train. You want to increase the training speed. What can you do?

**Options:**
- A) Subsample your test dataset.
- B) Subsample your training dataset.
- C) Increase the number of input features to your model.
- D) Increase the number of layers in your neural network.

**Memorize:** `B) Subsample your training dataset.`

<details>
<summary>Full explanation</summary>

Answer is 
Subsample your training dataset.

Subsampling is the method to increase the training speed

</details>

---

## Q216

**Prompt:** Your company maintains a hybrid deployment with GCP, where analytics are performed on your anonymized customer data. The data are imported to Cloud

Storage from your data center through parallel uploads to a data transfer server running on GCP. Management informs you that the daily transfers take too long and have asked you to fix the problem. You want to maximize transfer speeds. Which action should you take?

**Options:**
- A) Increase the CPU size on your server.
- B) Increase the size of the Google Persistent Disk on your server.
- C) Increase your network bandwidth from your datacenter to GCP.
- D) Increase your network bandwidth from Compute Engine to Cloud Storage.

**Memorize:** `C) Increase your network bandwidth from your datacenter to GCP.`

<details>
<summary>Full explanation</summary>

Answer is 
Increase your network bandwidth from your datacenter to GCP.

Speed of data transfer depends on Bandwidth

Few things in computing highlight the hardware limitations of networks as transferring large amounts of data. Typically you can transfer 1 GB in eight seconds over a 1 Gbps network. If you scale that up to a huge dataset (for example, 100 TB), the transfer time is 12 days. Transferring huge datasets can test the limits of your infrastructure and potentially cause problems for your business.

</details>

---

## Q217

**Prompt:** You've migrated a Hadoop job from an on-prem cluster to dataproc and GCS. Your Spark job is a complicated analytical workload that consists of many shuffing operations and initial data are parquet files (on average 200-400 MB size each). You see some degradation in performance after the migration to Dataproc, so you'd like to optimize for it. You need to keep in mind that your organization is very cost-sensitive, so you'd like to continue using Dataproc on preemptibles (with 2 non-preemptible workers only) for this workload. What should you do?

**Options:**
- A) Increase the size of your parquet files to ensure them to be 1 GB minimum.
- B) Switch to TFRecords formats (appr. 200MB per file) instead of parquet files.
- C) Switch from HDDs to SSDs, copy initial data from GCS to HDFS, run the Spark job and copy results back to GCS.
- D) Switch from HDDs to SSDs, override the preemptible VMs configuration to increase the boot disk size.

**Memorize:** `D) Switch from HDDs to SSDs, override the preemptible VMs configuration to increase the boot disk size.`

<details>
<summary>Full explanation</summary>

Answer is 
Switch from HDDs to SSDs, override the preemptible VMs configuration to increase the boot disk size.

From GCP Documentation:

1) "As a default, preemptible VMs are created with a smaller boot disk size, and you might want to override this configuration if you are running shuffle-heavy workloads"

2) If you perform many shuffling operations or partitioned writes, switch to SSDs to boost performance.

Reference:

https://cloud.google.com/architecture/hadoop/migrating-apache-spark-jobs-to-cloud-dataproc#optimize_performance

</details>

---

## Q218

**Prompt:** You have a data pipeline that writes data to Cloud Bigtable using well-designed row keys. You want to monitor your pipeline to determine when to increase the size of you Cloud Bigtable cluster. Which two actions can you take to accomplish this? (Choose two.)

**Options:**
- A) Review Key Visualizer metrics. Increase the size of the Cloud Bigtable cluster when the Read pressure index is above 100.
- B) Review Key Visualizer metrics. Increase the size of the Cloud Bigtable cluster when the Write pressure index is above 100.
- C) Monitor the latency of write operations. Increase the size of the Cloud Bigtable cluster when there is a sustained increase in write latency.
- D) Monitor storage utilization. Increase the size of the Cloud Bigtable cluster when utilization increases above 70% of max capacity.
- E) Monitor latency of read operations. Increase the size of the Cloud Bigtable cluster of read operations take longer than 100 ms.

**Memorize:** `D) Monitor storage utilization. Increase the size of the Cloud Bigtable cluster when utilization increases above 70% of max capacity.`

<details>
<summary>Full explanation</summary>

Answers are;

C. Monitor the latency of write operations. Increase the size of the Cloud Bigtable cluster when there is a sustained increase in write latency.

D. Monitor storage utilization. Increase the size of the Cloud Bigtable cluster when utilization increases above 70% of max capacity.

C –> Adding more nodes to a cluster (not replication) can improve the write performance

https://cloud.google.com/bigtable/docs/performance

D –> since Google recommends adding nodes when storage utilization is > 70%

https://cloud.google.com/bigtable/docs/modifying-instance#nodes

</details>

---

## Q219

**Prompt:** You have a query that filters a BigQuery table using a WHERE clause on timestamp and ID columns. By using bq query -dry_run you learn that the query triggers a full scan of the table, even though the filter on timestamp and ID select a tiny fraction of the overall data. You want to reduce the amount of data scanned by BigQuery with minimal changes to existing SQL queries. What should you do?

**Options:**
- A) Create a separate table for each ID.
- B) Use the LIMIT keyword to reduce the number of rows returned.
- C) Recreate the table with a partitioning column and clustering column.
- D) Use the bq query - -maximum_bytes_billed flag to restrict the number of bytes billed.

**Memorize:** `C) Recreate the table with a partitioning column and clustering column.`

<details>
<summary>Full explanation</summary>

Answer is 
Recreate the table with a partitioning column and clustering column.

LIMIT keyword is applied only at the end, i.e., only to limit the results already calculated. Therefore, a full table scan will have already happened. The where clause on the other hand would provide the desired filtering depending on the case.

</details>

---

## Q220

**Prompt:** You have a requirement to insert minute-resolution data from 50,000 sensors into a BigQuery table. You expect significant growth in data volume and need the data to be available within 1 minute of ingestion for real-time analysis of aggregated trends. What should you do?

**Options:**
- A) Use bq load to load a batch of sensor data every 60 seconds.
- B) Use a Cloud Dataflow pipeline to stream data into the BigQuery table.
- C) Use the INSERT statement to insert a batch of data every 60 seconds.
- D) Use the MERGE statement to apply updates in batch every 60 seconds.

**Memorize:** `B) Use a Cloud Dataflow pipeline to stream data into the BigQuery table.`

<details>
<summary>Full explanation</summary>

Answer is 
Use a Cloud Dataflow pipeline to stream data into the BigQuery table.

You need a pipeline because this type of operation can be easily parallelized, as the ingestion can be divided between into chunks (PCollections) and handled by many workers.

</details>

---

## Q221

**Prompt:** You need to create a near real-time inventory dashboard that reads the main inventory tables in your BigQuery data warehouse. Historical inventory data is stored as inventory balances by item and location. You have several thousand updates to inventory every hour. You want to maximize performance of the dashboard and ensure that the data is accurate. What should you do?

**Options:**
- A) Leverage BigQuery UPDATE statements to update the inventory balances as they are changing.
- B) Partition the inventory balance table by item to reduce the amount of data scanned with each inventory update.
- C) Use the BigQuery streaming the stream changes into a daily inventory movement table. Calculate balances in a view that joins it to the historical inventory balance table. Update the inventory balance table nightly.
- D) Use the BigQuery bulk loader to batch load inventory changes into a daily inventory movement table. Calculate balances in a view that joins it to the historical inventory balance table. Update the inventory balance table nightly.

**Memorize:** `C) Use the BigQuery streaming the stream changes into a daily inventory movement table. Calculate balances in a view that joins it to the historical inventory balance table. Update the inventory balance table nightly.`

<details>
<summary>Full explanation</summary>

Answer is 
Use the BigQuery streaming the stream changes into a daily inventory movement table. Calculate balances in a view that joins it to the historical inventory balance table. Update the inventory balance table nightly.

A : Not correct, due to constraint of 1500 updates / table / day

B : Not correct. Same as A, constraint of number of updates to BQ table / day

C : Streamed data is available for real-time analysis within a few seconds of the first streaming insertion into a table (in the table "Daily Inventory Movement Table). For the near real-time dashboard , balances are calculated in a view that joins "Daily Inventory Movement Table " and "Historical Inventory Balance Table" . Night process updates Historical Inventory Balance Table

D : Not Correct, as it will meet the requirement of near real time

</details>

---

## Q222

**Prompt:** You have a data stored in BigQuery. The data in the BigQuery dataset must be highly available. You need to define a storage, backup, and recovery strategy of this data that minimizes cost. How should you configure the BigQuery table?

**Options:**
- A) Set the BigQuery dataset to be regional. In the event of an emergency, use a point-in-time snapshot to recover the data.
- B) Set the BigQuery dataset to be regional. Create a scheduled query to make copies of the data to tables suffixed with the time of the backup. In the event of an emergency, use the backup copy of the table.
- C) Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.
- D) Set the BigQuery dataset to be multi-regional. Create a scheduled query to make copies of the data to tables suffixed with the time of the backup. In the event of an emergency, use the backup copy of the table.

**Memorize:** `C) Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.`

<details>
<summary>Full explanation</summary>

Answer is 
Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.

Highly available = multi-regional:

https://cloud.google.com/bigquery/docs/locations

Recovery strategy of this data that minimizes cost = point-in-time snapshot:

https://cloud.google.com/solutions/bigquery-data-warehouse#backup-and-recovery

</details>

---

## Q223

**Prompt:** You are managing a Cloud Dataproc cluster. You need to make a job run faster while minimizing costs, without losing work in progress on your clusters. What should you do?

**Options:**
- A) Increase the cluster size with more non-preemptible workers.
- B) Increase the cluster size with preemptible worker nodes, and configure them to forcefully decommission.
- C) Increase the cluster size with preemptible worker nodes, and use Cloud Stackdriver to trigger a script to preserve work.
- D) Increase the cluster size with preemptible worker nodes, and configure them to use graceful decommissioning.

**Memorize:** `D) Increase the cluster size with preemptible worker nodes, and configure them to use graceful decommissioning.`

<details>
<summary>Full explanation</summary>

Answer is 
Increase the cluster size with preemptible worker nodes, and configure them to use graceful decommissioning.

Graceful decommissioning will ensure that the data is processed by worker before it is removed by Yarn

Reference:

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/scaling-clusters

</details>

---

## Q224

**Prompt:** You have historical data covering the last three years in BigQuery and a data pipeline that delivers new data to BigQuery daily. You have noticed that when the Data Science team runs a query filtered on a date column and limited to 30-90 days of data, the query scans the entire table. You also noticed that your bill is increasing more quickly than you expected. You want to resolve the issue as cost-effectively as possible while maintaining the ability to conduct SQL queries. What should you do?

**Options:**
- A) Re-create the tables using DDL. Partition the tables by a column containing a TIMESTAMP or DATE Type.
- B) Recommend that the Data Science team export the table to a CSV file on Cloud Storage and use Cloud Datalab to explore the data by reading the files directly.
- C) Modify your pipeline to maintain the last 30""90 days of data in one table and the longer history in a different table to minimize full table scans over the entire history.
- D) Write an Apache Beam pipeline that creates a BigQuery table per day. Recommend that the Data Science team use wildcards on the table name suffixes to select the data they need.

**Memorize:** `A) Re-create the tables using DDL. Partition the tables by a column containing a TIMESTAMP or DATE Type.`

<details>
<summary>Full explanation</summary>

Answer is 
Re-create the tables using DDL. Partition the tables by a column containing a TIMESTAMP or DATE Type.

With partitions the performance will improve for selecting 30-90 days data. Also the storage cost will reduce as the old partitions (not updated in last 90 days) will qualify for Long-Term storage rates.

</details>

---

## Q225

**Prompt:** You operate a logistics company, and you want to improve event delivery reliability for vehicle-based sensors. You operate small data centers around the world to capture these events, but leased lines that provide connectivity from your event collection infrastructure to your event processing infrastructure are unreliable, with unpredictable latency. You want to address this issue in the most cost-effective way. What should you do?

**Options:**
- A) Deploy small Kafka clusters in your data centers to buffer events.
- B) Have the data acquisition devices publish data to Cloud Pub/Sub.
- C) Establish a Cloud Interconnect between all remote data centers and Google.
- D) Write a Cloud Dataflow pipeline that aggregates all data in session windows.

**Memorize:** `B) Have the data acquisition devices publish data to Cloud Pub/Sub.`

<details>
<summary>Full explanation</summary>

Answer is 
Have the data acquisition devices publish data to Cloud Pub/Sub.

The most cost effective (cheapest) way is to use PubSub. It can handle messages with high latency.

</details>

---

## Q226

**Prompt:** You operate a database that stores stock trades and an application that retrieves average stock price for a given company over an adjustable window of time. The data is stored in Cloud Bigtable where the datetime of the stock trade is the beginning of the row key. Your application has thousands of concurrent users, and you notice that performance is starting to degrade as more stocks are added. What should you do to improve the performance of your application?

**Options:**
- A) Change the row key syntax in your Cloud Bigtable table to begin with the stock symbol.
- B) Change the row key syntax in your Cloud Bigtable table to begin with a random number per second.
- C) Change the data pipeline to use BigQuery for storing stock trades, and update your application.
- D) Use Cloud Dataflow to write summary of each day's stock trades to an Avro file on Cloud Storage. Update your application to read from Cloud Storage and Cloud Bigtable to compute the responses.

**Memorize:** `A) Change the row key syntax in your Cloud Bigtable table to begin with the stock symbol.`

<details>
<summary>Full explanation</summary>

Answer is 
Change the row key syntax in your Cloud Bigtable table to begin with the stock symbol.

Having EXCHANGE and SYMBOL in the leading positions in the row key will naturally distribute activity.

Reference:

https://cloud.google.com/bigtable/docs/schema-design-time-series

</details>

---

## Q227

**Prompt:** As your organization expands its usage of GCP, many teams have started to create their own projects. Projects are further multiplied to accommodate different stages of deployments and target audiences. Each project requires unique access control configurations. The central IT team needs to have access to all projects. Furthermore, data from Cloud Storage buckets and BigQuery datasets must be shared for use in other projects in an ad hoc way. You want to simplify access control management by minimizing the number of policies. Which two steps should you take? (Choose two.)

**Options:**
- A) Use Cloud Deployment Manager to automate access provision.
- B) Introduce resource hierarchy to leverage access control policy inheritance.
- C) Create distinct groups for various teams, and specify groups in Cloud IAM policies.
- D) Only use service accounts when sharing data for Cloud Storage buckets and BigQuery datasets.
- E) For each Cloud Storage bucket or BigQuery dataset, decide which projects need access. Find all the active members who have access to these projects, and create a Cloud IAM policy to grant access to all these users.

**Memorize:** `C) Create distinct groups for various teams, and specify groups in Cloud IAM policies.`

<details>
<summary>Full explanation</summary>

Answers are;

B. Introduce resource hierarchy to leverage access control policy inheritance.

C. Create distinct groups for various teams, and specify groups in Cloud IAM policies.

Google suggests that we should provide access by following google hierarchy and groups for users with similar roles

</details>

---

## Q228

**Prompt:** You are running a pipeline in Cloud Dataflow that receives messages from a Cloud Pub/Sub topic and writes the results to a BigQuery dataset in the EU.

Currently, your pipeline is located in europe-west4 and has a maximum of 3 workers, instance type n1-standard-1. You notice that during peak periods, your pipeline is struggling to process records in a timely fashion, when all 3 workers are at maximum CPU utilization. Which two actions can you take to increase performance of your pipeline? (Choose two.)

**Options:**
- A) Increase the number of max workers
- B) Use a larger instance type for your Cloud Dataflow workers
- C) Change the zone of your Cloud Dataflow pipeline to run in us-central1
- D) Create a temporary table in Cloud Bigtable that will act as a buffer for new data. Create a new step in your pipeline to write to this table first, and then create a new pipeline to write from Cloud Bigtable to BigQuery
- E) Create a temporary table in Cloud Spanner that will act as a buffer for new data. Create a new step in your pipeline to write to this table first, and then create a new pipeline to write from Cloud Spanner to BigQuery

**Memorize:** `B) Use a larger instance type for your Cloud Dataflow workers`

<details>
<summary>Full explanation</summary>

Answers are;

A. Increase the number of max workers

B. Use a larger instance type for your Cloud Dataflow workers

With autoscaling enabled, the Dataflow service does not allow user control of the exact number of worker instances allocated to your job. You might still cap the number of workers by specifying the --max_num_workers option when you run your pipeline. Here as per question CAP is 3, So we can change that CAP.

For batch jobs, the default machine type is n1-standard-1. For streaming jobs, the default machine type for Streaming Engine-enabled jobs is n1-standard-2 and the default machine type for non-Streaming Engine jobs is n1-standard-4. When using the default machine types, the Dataflow service can therefore allocate up to 4000 cores per job. If you need more cores for your job, you can select a larger machine type.

</details>

---

## Q229

**Prompt:** You have a data pipeline with a Cloud Dataflow job that aggregates and writes time series metrics to Cloud Bigtable. This data feeds a dashboard used by thousands of users across the organization. You need to support additional concurrent users and reduce the amount of time required to write the data. Which two actions should you take? (Choose two.)

**Options:**
- A) Configure your Cloud Dataflow pipeline to use local execution
- B) Increase the maximum number of Cloud Dataflow workers by setting maxNumWorkers in PipelineOptions
- C) Increase the number of nodes in the Cloud Bigtable cluster
- D) Modify your Cloud Dataflow pipeline to use the Flatten transform before writing to Cloud Bigtable
- E) Modify your Cloud Dataflow pipeline to use the CoGroupByKey transform before writing to Cloud Bigtable

**Memorize:** `C) Increase the number of nodes in the Cloud Bigtable cluster`

<details>
<summary>Full explanation</summary>

Answers are;

B. Increase the maximum number of Cloud Dataflow workers by setting maxNumWorkers in PipelineOptions

C. Increase the number of nodes in the Cloud Bigtable cluster

The maximum number of Compute Engine instances to be made available to your pipeline during execution. Note that this can be higher than the initial number of workers (specified by num_workers to allow your job to scale up, automatically or otherwise.

Adding nodes to the original cluster: You can add 3 nodes to the cluster, for a total of 6 nodes. The write throughput for the instance doubles, but the instance's data is available in only one zone:

Reference:

https://cloud.google.com/bigtable/docs/performance#performance-write-throughput

https://cloud.google.com/dataflow/docs/guides/specifying-exec-params#setting-other-cloud-pipeline-options

</details>

---

## Q230

**Prompt:** You need to create a new transaction table in Cloud Spanner that stores product sales data. You are deciding what to use as a primary key. From a performance perspective, which strategy should you choose?

**Options:**
- A) The current epoch time
- B) A concatenation of the product name and the current epoch time
- C) A random universally unique identifier number (version 4 UUID)
- D) The original order identification number from the sales system, which is a monotonically increasing integer

**Memorize:** `C) A random universally unique identifier number (version 4 UUID)`

<details>
<summary>Full explanation</summary>

Answer is 
A random universally unique identifier number (version 4 UUID)

Reference:

https://cloud.google.com/spanner/docs/schema-and-data-model#choosing_a_primary_key

</details>

---

## Q231

**Prompt:** Each analytics team in your organization is running BigQuery jobs in their own projects. You want to enable each team to monitor slot usage within their projects.

What should you do?

**Options:**
- A) Create a Stackdriver Monitoring dashboard based on the BigQuery metric query/scanned_bytes
- B) Create a Stackdriver Monitoring dashboard based on the BigQuery metric slots/allocated_for_project
- C) Create a log export for each project, capture the BigQuery job execution logs, create a custom metric based on the totalSlotMs, and create a Stackdriver Monitoring dashboard based on the custom metric
- D) Create an aggregated log export at the organization level, capture the BigQuery job execution logs, create a custom metric based on the totalSlotMs, and create a Stackdriver Monitoring dashboard based on the custom metric

**Memorize:** `B) Create a Stackdriver Monitoring dashboard based on the BigQuery metric slots/allocated_for_project`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Stackdriver Monitoring dashboard based on the BigQuery metric slots/allocated_for_project

Number of slots allocated to the project at any time. This can also be thought of as the number of slots being utilized by that project.

Reference:

https://cloud.google.com/bigquery/docs/monitoring#slots-available

</details>

---

## Q232

**Prompt:** You are migrating your data warehouse to BigQuery. You have migrated all of your data into tables in a dataset. Multiple users from your organization will be using the data. They should only see certain tables based on their team membership. How should you set user permissions?

**Options:**
- A) Assign the users/groups data viewer access at the table level for each table
- B) Create SQL views for each team in the same dataset in which the data resides, and assign the users/groups data viewer access to the SQL views
- C) Create authorized views for each team in the same dataset in which the data resides, and assign the users/groups data viewer access to the authorized viewsMost Voted
- D) Create authorized views for each team in datasets created for each team. Assign the authorized views data viewer access to the dataset in which the data resides. Assign the users/groups data viewer access to the datasets in which the authorized views reside

**Memorize:** `A) Assign the users/groups data viewer access at the table level for each table`

<details>
<summary>Full explanation</summary>

Answer is 
 A. Assign the users/groups data viewer access at the table level for each table 

it is feasible to provide table level access to user by allowing user to query single table and no other table will be visible to user in same dataset.

Reference:

https://cloud.google.com/bigquery/docs/table-access-controls-intro#permissions

https://cloud.google.com/bigquery/docs/table-access-controls-intro#example_use_case

</details>

---

## Q233

**Prompt:** You plan to deploy Cloud SQL using MySQL. You need to ensure high availability in the event of a zone failure. What should you do?

**Options:**
- A) Create a Cloud SQL instance in one zone, and create a failover replica in another zone within the same region.
- B) Create a Cloud SQL instance in one zone, and create a read replica in another zone within the same region.
- C) Create a Cloud SQL instance in one zone, and configure an external read replica in a zone in a different region.
- D) Create a Cloud SQL instance in a region, and configure automatic backup to a Cloud Storage bucket in the same region.

**Memorize:** `A) Create a Cloud SQL instance in one zone, and create a failover replica in another zone within the same region.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Cloud SQL instance in one zone, and create a failover replica in another zone within the same region.

The HA (High Availability) configuration, sometimes called a cluster, provides data redundancy. A Cloud SQL instance configured for HA is also called a regional instance and is located in a primary and secondary zone within the configured region. Within a regional instance, the configuration is made up of a primary instance and a standby instance.
Reference:

https://cloud.google.com/sql/docs/mysql/high-availability

</details>

---

## Q234

**Prompt:** You want to archive data in Cloud Storage. Because some data is very sensitive, you want to use the "Trust No One" (TNO) approach to encrypt your data to prevent the cloud provider staff from decrypting your data. What should you do?

**Options:**
- A) Use gcloud kms keys create to create a symmetric key. Then use gcloud kms encrypt to encrypt each archival file with the key and unique additional authenticated data (AAD). Use gsutil cp to upload each encrypted file to the Cloud Storage bucket, and keep the AAD outside of Google Cloud.
- B) Use gcloud kms keys create to create a symmetric key. Then use gcloud kms encrypt to encrypt each archival file with the key. Use gsutil cp to upload each encrypted file to the Cloud Storage bucket. Manually destroy the key previously used for encryption, and rotate the key once.
- C) Specify customer-supplied encryption key (CSEK) in the .boto configuration file. Use gsutil cp to upload each archival file to the Cloud Storage bucket. Save the CSEK in Cloud Memorystore as permanent storage of the secret.
- D) Specify customer-supplied encryption key (CSEK) in the .boto configuration file. Use gsutil cp to upload each archival file to the Cloud Storage bucket. Save the CSEK in a different project that only the security team can access.

**Memorize:** `A) Use gcloud kms keys create to create a symmetric key. Then use gcloud kms encrypt to encrypt each archival file with the key and unique additional authenticated data (AAD). Use gsutil cp to upload each encrypted file to the Cloud Storage bucket, and keep the AAD outside of Google Cloud.`

<details>
<summary>Full explanation</summary>

Answer is 
Use gcloud kms keys create to create a symmetric key. Then use gcloud kms encrypt to encrypt each archival file with the key and unique additional authenticated data (AAD). Use gsutil cp to upload each encrypted file to the Cloud Storage bucket, and keep the AAD outside of Google Cloud.

AAD is used to decrypt the data so better to keep it outside GCP for safety

</details>

---

## Q235

**Prompt:** Which of the following is not possible using primitive roles?

**Options:**
- A) Give a user viewer access to BigQuery and owner access to Google Compute Engine instances.Most Voted
- B) Give UserA owner access and UserB editor access for all datasets in a project.
- C) Give a user access to view all datasets in a project, but not run queries on them.
- D) Give GroupA owner access and GroupB editor access for all datasets in a project.

**Memorize:** `C) Give a user access to view all datasets in a project, but not run queries on them.`

<details>
<summary>Full explanation</summary>

Answer is 
Give a user access to view all datasets in a project, but not run queries on them.

Reference:

https://cloud.google.com/bigquery/docs/access-control#bigquery.dataViewer

</details>

---

## Q236

**Prompt:** Which of the following statements about Legacy SQL and Standard SQL is not true?

**Options:**
- A) Standard SQL is the preferred query language for BigQuery.
- B) If you write a query in Legacy SQL, it might generate an error if you try to run it with Standard SQL.
- C) One difference between the two query languages is how you specify fully-qualified table names (i.e. table names that include their associated project name).
- D) You need to set a query language for each dataset and the default is Standard SQL.

**Memorize:** `D) You need to set a query language for each dataset and the default is Standard SQL.`

<details>
<summary>Full explanation</summary>

Answer is 
You need to set a query language for each dataset and the default is Standard SQL.

Query language is not set at dataset level. It depends on from where we are trying to access Bigquery tables. Webs default is leagacy whereas Cloud console is Standard.

</details>

---

## Q237

**Prompt:** Which methods can be used to reduce the number of rows processed by BigQuery?

**Options:**
- A) Splitting tables into multiple tables; putting data in partitions
- B) Splitting tables into multiple tables; putting data in partitions; using the LIMIT clause
- C) Putting data in partitions; using the LIMIT clause
- D) Splitting tables into multiple tables; using the LIMIT clause

**Memorize:** `A) Splitting tables into multiple tables; putting data in partitions`

<details>
<summary>Full explanation</summary>

Answer is 
Splitting tables into multiple tables; putting data in partitions

Others options are with Limit, limit cannot stop from checking all the rows in bigquery

</details>

---

## Q238

**Prompt:** If a dataset contains rows with individual people and columns for year of birth, country, and income, how many of the columns are continuous and how many are categorical?

**Options:**
- A) 1 continuous and 2 categorical
- B) 3 categorical
- C) 3 continuous
- D) 2 continuous and 1 categorical

**Memorize:** `D) 2 continuous and 1 categorical`

<details>
<summary>Full explanation</summary>

Answer is 
2 continuous and 1 categorical

Year can be any value, income can be any value, so continuous, and country is categorical as values are finite

</details>

---

## Q239

**Prompt:** What is the recommended action to do in order to switch between SSD and HDD storage for your Google Cloud Bigtable instance?

**Options:**
- A) create a third instance and sync the data from the two storage types via batch jobs
- B) export the data from the existing instance and import the data into a new instance
- C) run parallel instances where one is HDD and the other is SDD
- D) the selection is final and you must resume using the same storage type

**Memorize:** `B) export the data from the existing instance and import the data into a new instance`

<details>
<summary>Full explanation</summary>

Answer is 
export the data from the existing instance and import the data into a new instance

When you create a Cloud Bigtable instance and cluster, your choice of SSD or HDD storage for the cluster is permanent. You cannot use the Google Cloud
Platform Console to change the type of storage that is used for the cluster.
If you need to convert an existing HDD cluster to SSD, or vice-versa, you can export the data from the existing instance and import the data into a new instance.
Alternatively, you can write -
a Cloud Dataflow or Hadoop MapReduce job that copies the data from one instance to another.

Reference:

https://cloud.google.com/bigtable/docs/choosing-ssd-hdd

</details>

---

## Q240

**Prompt:** What is the HBase Shell for Cloud Bigtable?

**Options:**
- A) The HBase shell is a GUI based interface that performs administrative tasks, such as creating and deleting tables.
- B) The HBase shell is a command-line tool that performs administrative tasks, such as creating and deleting tables.
- C) The HBase shell is a hypervisor based shell that performs administrative tasks, such as creating and deleting new virtualized instances.
- D) The HBase shell is a command-line tool that performs only user account management functions to grant access to Cloud Bigtable instances.

**Memorize:** `B) The HBase shell is a command-line tool that performs administrative tasks, such as creating and deleting tables.`

<details>
<summary>Full explanation</summary>

Answer is 

The HBase shell is a command-line tool that performs administrative tasks, such as creating and deleting tables. The Cloud Bigtable HBase client for Java makes it possible to use the HBase shell to connect to Cloud Bigtable.

Reference:

https://cloud.google.com/bigtable/docs/installing-hbase-shell

</details>

---

## Q241

**Prompt:** Google Cloud Bigtable indexes a single value in each row. This value is called the _______.

**Options:**
- A) primary key
- B) unique key
- C) row key
- D) master key

**Memorize:** `C) row key`

<details>
<summary>Full explanation</summary>

Answer is 
row key

Cloud Bigtable is a sparsely populated table that can scale to billions of rows and thousands of columns, allowing you to store terabytes or even petabytes of data.
A single value in each row is indexed; this value is known as the row key.

Reference:

https://cloud.google.com/bigtable/docs/overview

</details>

---

## Q242

**Prompt:** Cloud Bigtable is a recommended option for storing very large amounts of ____________________________?

**Options:**
- A) multi-keyed data with very high latency
- B) multi-keyed data with very low latency
- C) single-keyed data with very low latency
- D) single-keyed data with very high latency

**Memorize:** `C) single-keyed data with very low latency`

<details>
<summary>Full explanation</summary>

Answer is 
single-keyed data with very low latency

Cloud Bigtable is a sparsely populated table that can scale to billions of rows and thousands of columns, allowing you to store terabytes or even petabytes of data.
A single value in each row is indexed; this value is known as the row key. Cloud Bigtable is ideal for storing very large amounts of single-keyed data with very low latency. It supports high read and write throughput at low latency, and it is an ideal data source for MapReduce operations.

Reference:

https://cloud.google.com/bigtable/docs/overview#storage-model

</details>

---

## Q243

**Prompt:** When you store data in Cloud Bigtable, what is the recommended minimum amount of stored data?

**Options:**
- A) 500 TB
- B) 1 GB
- C) 1 TB
- D) 500 GB

**Memorize:** `C) 1 TB`

<details>
<summary>Full explanation</summary>

Answer is 
1 TB

Cloud Bigtable is not a relational database. It does not support SQL queries, joins, or multi-row transactions. It is not a good solution for less than 1 TB of data.

Reference:

https://cloud.google.com/bigtable/docs/overview#title_short_and_other_storage_options

</details>

---

## Q244

**Prompt:** Which of the following is NOT a valid use case to select HDD (hard disk drives) as the storage for Google Cloud Bigtable?

**Options:**
- A) You expect to store at least 10 TB of data.
- B) You will mostly run batch workloads with scans and writes, rather than frequently executing random reads of a small number of rows.
- C) You need to integrate with Google BigQuery.
- D) You will not use the data to back a user-facing or latency-sensitive application.

**Memorize:** `C) You need to integrate with Google BigQuery.`

<details>
<summary>Full explanation</summary>

Answer is 

HDD storage is suitable for use cases that meet the following criteria:

You expect to store at least 10 TB of data.

You will not use the data to back a user-facing or latency-sensitive application.

Your workload falls into one of the following categories:

Batch workloads with scans and writes, and no more than occasional random reads of a small number of rows or point reads.

Data archival, where you write very large amounts of data and rarely read that data.

Reference:

https://cloud.google.com/bigtable/docs/choosing-ssd-hdd#use-cases-hdd

</details>

---

## Q245

**Prompt:** When you design a Google Cloud Bigtable schema it is recommended that you _________.

**Options:**
- A) Avoid schema designs that are based on NoSQL concepts
- B) Create schema designs that are based on a relational database design
- C) Avoid schema designs that require atomicity across rows
- D) Create schema designs that require atomicity across rows

**Memorize:** `C) Avoid schema designs that require atomicity across rows`

<details>
<summary>Full explanation</summary>

Answer is 
Avoid schema designs that require atomicity across rows

All operations are atomic at the row level. For example, if you update two rows in a table, it's possible that one row will be updated successfully and the other update will fail. Avoid schema designs that require atomicity across rows.

Reference:

https://cloud.google.com/bigtable/docs/schema-design#row-keys

</details>

---

## Q246

**Prompt:** Which is the preferred method to use to avoid hotspotting in time series data in Bigtable?

**Options:**
- A) Field promotion
- B) Randomization
- C) Salting
- D) Hashing

**Memorize:** `A) Field promotion`

<details>
<summary>Full explanation</summary>

Answer is 
Field promotion

By default, prefer field promotion. Field promotion avoids hotspotting in almost all cases, and it tends to make it easier to design a row key that facilitates queries.

Reference:

https://cloud.google.com/bigtable/docs/schema-design-time-series#ensure_that_your_row_key_avoids_hotspotting

</details>

---

## Q247

**Prompt:** Which is not a valid reason for poor Cloud Bigtable performance?

**Options:**
- A) The workload isn't appropriate for Cloud Bigtable.
- B) The table's schema is not designed correctly.
- C) The Cloud Bigtable cluster has too many nodes.
- D) There are issues with the network connection.

**Memorize:** `C) The Cloud Bigtable cluster has too many nodes.`

<details>
<summary>Full explanation</summary>

Answer is 
The Cloud Bigtable cluster has too many nodes.

The Cloud Bigtable cluster doesn't have enough nodes. If your Cloud Bigtable cluster is overloaded, adding more nodes can improve performance. Use the monitoring tools to check whether the cluster is overloaded.

Reference:

https://cloud.google.com/bigtable/docs/performance

</details>

---

## Q248

**Prompt:** When a Cloud Bigtable node fails, ____ is lost.

**Options:**
- A) all data
- B) no data
- C) the last transaction
- D) the time dimension

**Memorize:** `B) no data`

<details>
<summary>Full explanation</summary>

Answer is 
no data

A Cloud Bigtable table is sharded into blocks of contiguous rows, called tablets, to help balance the workload of queries. Tablets are stored on Colossus, Google's file system, in SSTable format. Each tablet is associated with a specific Cloud Bigtable node.
Data is never stored in Cloud Bigtable nodes themselves; each node has pointers to a set of tablets that are stored on Colossus. As a result:
Rebalancing tablets from one node to another is very fast, because the actual data is not copied. Cloud Bigtable simply updates the pointers for each node.
Recovery from the failure of a Cloud Bigtable node is very fast, because only metadata needs to be migrated to the replacement node.
When a Cloud Bigtable node fails, no data is lost

Reference:

https://cloud.google.com/bigtable/docs/overview

</details>

---

## Q249

**Prompt:** Which row keys are likely to cause a disproportionate number of reads and/or writes on a particular node in a Bigtable cluster (select 2 answers)?

**Options:**
- A) A sequential numeric ID
- B) A timestamp followed by a stock symbol
- C) A non-sequential numeric ID
- D) A stock symbol followed by a timestamp

**Memorize:** `B) A timestamp followed by a stock symbol`

<details>
<summary>Full explanation</summary>

Answers are;

A sequential numeric ID

A timestamp followed by a stock symbol

Using a timestamp as the first element of a row key can cause a variety of problems.
In brief, when a row key for a time series includes a timestamp, all of your writes will target a single node; fill that node; and then move onto the next node in the cluster, resulting in hotspotting.
Suppose your system assigns a numeric ID to each of your application's users. You might be tempted to use the user's numeric ID as the row key for your table.
However, since new users are more likely to be active users, this approach is likely to push most of your traffic to a small number of nodes.

Reference:

https://cloud.google.com/bigtable/docs/schema-design-time-series#ensure_that_your_row_key_avoids_hotspotting

</details>

---

## Q250

**Prompt:** For the best possible performance, what is the recommended zone for your Compute Engine instance and Cloud Bigtable instance?

**Options:**
- A) Have the Compute Engine instance in the furthest zone from the Cloud Bigtable instance.
- B) Have both the Compute Engine instance and the Cloud Bigtable instance to be in different zones.
- C) Have both the Compute Engine instance and the Cloud Bigtable instance to be in the same zone.
- D) Have the Cloud Bigtable instance to be in the same zone as all of the consumers of your data.

**Memorize:** `C) Have both the Compute Engine instance and the Cloud Bigtable instance to be in the same zone.`

<details>
<summary>Full explanation</summary>

Answer is 
Have both the Compute Engine instance and the Cloud Bigtable instance to be in the same zone.

It is recommended to create your Compute Engine instance in the same zone as your Cloud Bigtable instance for the best possible performance,
If it's not possible to create a instance in the same zone, you should create your instance in another zone within the same region. For example, if your Cloud
Bigtable instance is located in us-central1-b, you could create your instance in us-central1-f. This change may result in several milliseconds of additional latency for each Cloud Bigtable request.
It is recommended to avoid creating your Compute Engine instance in a different region from your Cloud Bigtable instance, which can add hundreds of milliseconds of latency to each Cloud Bigtable request.

Reference:

https://cloud.google.com/bigtable/docs/creating-compute-instance

</details>

---

## Q251

**Prompt:** Which of these is NOT a way to customize the software on Dataproc cluster instances?

**Options:**
- A) Set initialization actions
- B) Modify configuration files using cluster properties
- C) Configure the cluster using Cloud Deployment Manager
- D) Log into the master node and make changes from thereMost Voted

**Memorize:** `C) Configure the cluster using Cloud Deployment Manager`

<details>
<summary>Full explanation</summary>

Answer is 
Configure the cluster using Cloud Deployment Manager

You can access the master node of the cluster by clicking the SSH button next to it in the Cloud Console.
You can easily use the --properties option of the dataproc command in the Google Cloud SDK to modify many common configuration files when creating a cluster.
When creating a Cloud Dataproc cluster, you can specify initialization actions in executables and/or scripts that Cloud Dataproc will run on all nodes in your Cloud
Dataproc cluster immediately after the cluster is set up.

Reference:

https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/cluster-properties

</details>

---

## Q252

**Prompt:** You work for a car manufacturer and have set up a data pipeline using Google Cloud Pub/Sub to capture anomalous sensor events. You are using a push subscription in Cloud Pub/Sub that calls a custom HTTPS endpoint that you have created to take action of these anomalous events as they occur. Your custom HTTPS endpoint keeps getting an inordinate amount of duplicate messages. What is the most likely cause of these duplicate messages?

**Options:**
- A) The message body for the sensor event is too large.
- B) Your custom endpoint has an out-of-date SSL certificate.
- C) The Cloud Pub/Sub topic has too many messages published to it.
- D) Your custom endpoint is not acknowledging messages within the acknowledgement deadline.

**Memorize:** `D) Your custom endpoint is not acknowledging messages within the acknowledgement deadline.`

<details>
<summary>Full explanation</summary>

Answer is 
Your custom endpoint is not acknowledging messages within the acknowledgement deadline.

The custom endpoint is not acknowledging the message, that is the reason for Pub/Sub to send the message again and again.

A managed certificate for HTTPS connections is automatically issued and renewed when you map a service to a custom domain.

Reference:

https://cloud.google.com/run/docs/mapping-custom-domains

</details>

---

## Q253

**Prompt:** Your company uses a proprietary system to send inventory data every 6 hours to a data ingestion service in the cloud. Transmitted data includes a payload of several fields and the timestamp of the transmission. If there are any concerns about a transmission, the system re-transmits the data. How should you deduplicate the data most efficiency?

**Options:**
- A) Assign global unique identifiers (GUID) to each data entry.
- B) Compute the hash value of each data entry, and compare it with all historical data.
- C) Store each data entry as the primary key in a separate database and apply an index.
- D) Maintain a database table to store the hash value and other metadata for each data entry.

**Memorize:** `A) Assign global unique identifiers (GUID) to each data entry.`

<details>
<summary>Full explanation</summary>

Answer is 
Assign global unique identifiers (GUID) to each data entry.

Answer "D" is not as efficient or error-proof due to two reasons

1. You need to calculate hash at sender as well as at receiver end to do the comparison. Waste of computing power.

2. Even if we discount the computing power, we should note that the system is sending inventory information. Two messages sent at different can denote same inventory level (and thus have same hash). Adding sender time stamp to hash will defeat the purpose of using hash as now retried messages will have different timestamp and a different hash. 

if timestamp is used as message creation timestamp than that can also be used as a UUID.

</details>

---

## Q254

**Prompt:** The marketing team at your organization provides regular updates of a segment of your customer dataset. The marketing team has given you a CSV with 1 million records that must be updated in BigQuery. When you use the UPDATE statement in BigQuery, you receive a quotaExceeded error.

**Options:**
- A) Reduce the number of records updated each day to stay within the BigQuery UPDATE DML statement limit.
- B) Increase the BigQuery UPDATE DML statement limit in the Quota management section of the Google Cloud Platform Console.
- C) Split the source CSV file into smaller CSV files in Cloud Storage to reduce the number of BigQuery UPDATE DML statements per BigQuery job.
- D) Import the new records from the CSV file into a new BigQuery table. Create a BigQuery job that merges the new records with the existing records and writes the results to a new BigQuery table.

**Memorize:** `D) Import the new records from the CSV file into a new BigQuery table. Create a BigQuery job that merges the new records with the existing records and writes the results to a new BigQuery table.`

<details>
<summary>Full explanation</summary>

Answer is 
Import the new records from the CSV file into a new BigQuery table. Create a BigQuery job that merges the new records with the existing records and writes the results to a new BigQuery table.

 

BigQuery DML statements have no quota limits.

However, DML statements are counted toward the maximum number of table operations per day and partition modifications per day. DML statements will not fail due to these limits.

In addition, DML statements are subject to the maximum rate of table metadata update operations. If you exceed this limit, retry the operation using exponential backoff between retries.

Reference:

https://cloud.google.com/bigquery/quotas#data-manipulation-language-statements

</details>

---

## Q255

**Prompt:** You have data pipelines running on BigQuery, Dataflow, and Dataproc. You need to perform health checks and monitor their behavior, and then notify the team managing the pipelines if they fail. You also need to be able to work across multiple projects. Your preference is to use managed products or features of the platform.

**Options:**
- A) Export the information to Cloud Monitoring, and set up an Alerting policy
- B) Run a Virtual Machine in Compute Engine with Airflow, and export the information to Cloud Monitoring
- C) Export the logs to BigQuery, and set up App Engine to read that information and send emails if you find a failure in the logs
- D) Develop an App Engine application to consume logs using GCP API calls, and send emails if you find a failure in the logs

**Memorize:** `A) Export the information to Cloud Monitoring, and set up an Alerting policy`

<details>
<summary>Full explanation</summary>

Answer is 
Export the information to Cloud Monitoring, and set up an Alerting policy

 

Cloud Monitoring (formerly known as Stackdriver) is a fully managed monitoring service provided by GCP, which can collect metrics, logs, and other telemetry data from various GCP services, including BigQuery, Dataflow, and Dataproc.

 
 

Alerting Policies: Cloud Monitoring allows you to define alerting policies based on specific conditions or thresholds, such as pipeline failures, latency spikes, or other custom metrics. When these conditions are met, Cloud Monitoring can trigger notifications (e.g., emails) to alert the team managing the pipelines.

 
 

Cross-Project Monitoring: Cloud Monitoring supports monitoring resources across multiple GCP projects, making it suitable for your requirement to monitor pipelines in multiple projects.

 
 

Managed Solution: Cloud Monitoring is a managed service, reducing the operational overhead compared to running your own virtual machine instances or building custom solutions.

</details>

---

## Q256

**Prompt:** Your company currently runs a large on-premises cluster using Spark, Hive, and HDFS in a colocation facility. The cluster is designed to accommodate peak usage on the system; however, many jobs are batch in nature, and usage of the cluster fluctuates quite dramatically. Your company is eager to move to the cloud to reduce the overhead associated with on-premises infrastructure and maintenance and to benefit from the cost savings. They are also hoping to modernize their existing infrastructure to use more serverless offerings in order to take advantage of the cloud. Because of the timing of their contract renewal with the colocation facility, they have only 2 months for their initial migration.

**Options:**
- A) Migrate the workloads to Dataproc plus HDFS; modernize later.
- B) Migrate the workloads to Dataproc plus Cloud Storage; modernize later.
- C) Migrate the Spark workload to Dataproc plus HDFS, and modernize the Hive workload for BigQuery.
- D) Modernize the Spark workload for Dataflow and the Hive workload for BigQuery.

**Memorize:** `B) Migrate the workloads to Dataproc plus Cloud Storage; modernize later.`

<details>
<summary>Full explanation</summary>

Answer is 
Migrate the workloads to Dataproc plus Cloud Storage; modernize later.

 

Based on the time constraint of 2 months and the goal to maximize cost savings, we would recommend this option. The key reasons are:

- Dataproc provides a fast, native migration path from on-prem Spark and Hive to the cloud. This allows meeting the 2 month timeline.

- Using Cloud Storage instead of HDFS avoids managing clusters for variable workloads and provides cost savings.

- Further optimizations and modernization to serverless (Dataflow, BigQuery) can happen incrementally later without time pressure.

Option A still requires managing HDFS.

Option C and D require full modernization of workloads in 2 months which is likely infeasible.

Therefore, migrating to Dataproc with Cloud Storage fast tracks the migration within 2 months while realizing immediate cost savings, enabling the flexibility to iteratively modernize and optimize the workloads over time.

Reference:

https://cloud.google.com/bigquery/docs/migration/hive#data_migration

https://cloud.google.com/architecture/hadoop/migrating-apache-spark-jobs-to-cloud-dataproc#overview

</details>

---

## Q257

**Prompt:** You are migrating a table to BigQuery and are deciding on the data model. Your table stores information related to purchases made across several store locations and includes information like the time of the transaction, items purchased, the store ID, and the city and state in which the store is located. You frequently query this table to see how many of each item were sold over the past 30 days and to look at purchasing trends by state, city, and individual store.

**Options:**
- A) Partition by transaction time; cluster by state first, then city, then store ID.
- B) Partition by transaction time; cluster by store ID first, then city, then state.
- C) Top-level cluster by state first, then city, then store ID.
- D) Top-level cluster by store ID first, then city, then state.

**Memorize:** `A) Partition by transaction time; cluster by state first, then city, then store ID.`

<details>
<summary>Full explanation</summary>

Answer is 
Partition by transaction time; cluster by state first, then city, then store ID.

 

A partitioned table is a special table that is divided into segments, called partitions, that make it easier to manage and query your data. By dividing a large table into smaller partitions, you can improve query performance, and you can control costs by reducing the number of bytes read by a query.

You can partition BigQuery tables by:

- Time-unit column: Tables are partitioned based on a TIMESTAMP, DATE, or DATETIME column in the table.

Clustered tables in BigQuery are tables that have a user-defined column sort order using clustered columns. Clustered tables can improve query performance and reduce query costs.

Reference:

https://cloud.google.com/bigquery/docs/partitioned-tables

https://cloud.google.com/bigquery/docs/clustered-tables

</details>

---

## Q258

**Prompt:** Your company is implementing a data warehouse using BigQuery, and you have been tasked with designing the data model.

**Options:**
- A) Denormalize the data.
- B) Shard the data by customer ID.
- C) Materialize the dimensional data in views.
- D) Partition the data by transaction date.

**Memorize:** `D) Partition the data by transaction date.`

<details>
<summary>Full explanation</summary>

Answer is 
Partition the data by transaction date.

 

BigQuery supports partitioned tables, where the data is divided into smaller, manageable portions based on a chosen column (e.g., transaction date). By partitioning the data based on the transaction date, BigQuery can efficiently query only the relevant partitions that contain data for the past 30 days, reducing the amount of data that needs to be scanned.Partitioning does not increase storage costs. It organizes existing data in a more structured manner, allowing for better query performance without any additional storage expenses.

Reference:

https://cloud.google.com/bigquery/docs/partitioned-tables

</details>

---

## Q259

**Prompt:** You have uploaded 5 years of log data to Cloud Storage. A user reported that some data points in the log data are outside of their expected ranges, which indicates errors.

**Options:**
- A) Import the data from Cloud Storage into BigQuery. Create a new BigQuery table, and skip the rows with errors.
- B) Create a Compute Engine instance and create a new copy of the data in Cloud Storage. Skip the rows with errors.
- C) Create a Dataflow workflow that reads the data from Cloud Storage, checks for values outside the expected range, sets the value to an appropriate default, and writes the updated records to a new dataset in Cloud Storage.
- D) Create a Dataflow workflow that reads the data from Cloud Storage, checks for values outside the expected range, sets the value to an appropriate default, and writes the updated records to the same dataset in Cloud Storage.

**Memorize:** `C) Create a Dataflow workflow that reads the data from Cloud Storage, checks for values outside the expected range, sets the value to an appropriate default, and writes the updated records to a new dataset in Cloud Storage.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a Dataflow workflow that reads the data from Cloud Storage, checks for values outside the expected range, sets the value to an appropriate default, and writes the updated records to a new dataset in Cloud Storage.

 

Option A would remove data which may be needed for compliance reasons. Keeping the original data is preferred.

Option B makes a copy of the data but still removes potentially useful records. Additional storage costs would be incurred as well.

Option C uses Dataflow to clean the data by setting out of range values while keeping the original data intact. The fixed records are written to a new location for further analysis. This meets the requirements.

Option D writes the fixed data back to the original location, overwriting the original data. This would violate the compliance needs to keep the original data untouched.

So option C leverages Dataflow to properly clean the data while preserving the original data for compliance, at reasonable operational costs. This best achieves the stated requirements.

</details>

---

## Q260

**Prompt:** You are testing a Dataflow pipeline to ingest and transform text files.

**Options:**
- A) Switch to compressed Avro files.
- B) Reduce the batch size.
- C) Retry records that throw an error.
- D) Use CoGroupByKey instead of the SideInput.

**Memorize:** `D) Use CoGroupByKey instead of the SideInput.`

<details>
<summary>Full explanation</summary>

Answer is 
Use CoGroupByKey instead of the SideInput.

 

Here's why this approach is beneficial:

1. Efficiency in Handling Large Datasets: SideInputs are not optimal for large datasets because they require that the entire dataset be available to each worker. This can lead to performance bottlenecks, especially if the dataset is large. CoGroupByKey, on the other hand, is more efficient for joining large datasets because it groups elements by key and allows the pipeline to process each key-group separately.

2. Scalability: CoGroupByKey is more scalable than SideInputs for large-scale data processing. It distributes the workload more evenly across the Dataflow workers, which can significantly improve the performance of your pipeline.

3. Better Resource Utilization: By using CoGroupByKey, the Dataflow job can make better use of its resources, as it doesn't need to replicate the entire dataset to each worker. This results in faster processing times and better overall efficiency.

The other options may not be as effective:

A (Switch to compressed Avro files): While Avro is a good format for certain types of data processing, simply changing the file format from gzip to Avro may not address the underlying issue causing the delay, especially if the problem is related to the way data is being joined or processed.

B (Reduce the batch size): Reducing the batch size could potentially increase overhead and might not significantly improve the processing time, especially if the bottleneck is due to the method of data joining.

C (Retry records that throw an error): Retrying errors could be useful in certain contexts, but it's unlikely to speed up the pipeline if the delay is due to inefficiencies in data processing methods like the use of SideInputs.

Reference:

https://cloud.google.com/architecture/building-production-ready-data-pipelines-using-dataflow-developing-and-testing#choose_correctly_between_side_inputs_or_cogroupbykey_for_joins

</details>

---

## Q261

**Prompt:** You have a data stored in BigQuery. The data in the BigQuery dataset must be highly available. You need to define a storage, backup, and recovery strategy of this data that minimizes cost.

**Options:**
- A) Set the BigQuery dataset to be regional. In the event of an emergency, use a point-in-time snapshot to recover the data.
- B) Set the BigQuery dataset to be regional. Create a scheduled query to make copies of the data to tables suffixed with the time of the backup. In the event of an emergency, use the backup copy of the table.
- C) Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.
- D) Set the BigQuery dataset to be multi-regional. Create a scheduled query to make copies of the data to tables suffixed with the time of the backup. In the event of an emergency, use the backup copy of the table.

**Memorize:** `C) Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.`

<details>
<summary>Full explanation</summary>

Answer is 
Set the BigQuery dataset to be multi-regional. In the event of an emergency, use a point-in-time snapshot to recover the data.

 

A BigQuery table snapshot preserves the contents of a table (called the base table) at a particular time. You can save a snapshot of a current table, or create a snapshot of a table as it was at any time in the past seven days. A table snapshot can have an expiration; when the configured amount of time has passed since the table snapshot was created, BigQuery deletes the table snapshot. You can query a table snapshot as you would a standard table. Table snapshots are read-only, but you can create (restore) a standard table from a table snapshot, and then you can modify the restored table.

Reference:

https://cloud.google.com/bigquery/docs/table-snapshots-intro#table_snapshots

</details>

---

## Q262

**Prompt:** You issue a new batch job to Dataflow. The job starts successfully, processes a few elements, and then suddenly fails and shuts down.

**Options:**
- A) Job validation
- B) Exceptions in worker code
- C) Graph or pipeline construction
- D) Insufficient permissions

**Memorize:** `B) Exceptions in worker code`

<details>
<summary>Full explanation</summary>

Answer is 
Exceptions in worker code

 

The most likely cause of the errors you're experiencing in Dataflow, particularly if they are related to a particular DoFn (Dataflow's parallel processing operation), is; Exceptions in worker code.

When a Dataflow job processes a few elements successfully before failing, it suggests that the overall job setup, permissions, and pipeline graph are likely correct, as the job was able to start and initially process data. However, if it fails during execution and the errors are associated with a specific DoFn, this points towards issues in the code that executes within the workers. This could include:

1. Runtime exceptions in the code logic of the DoFn.

2. Issues handling specific data elements that might not be correctly managed by the DoFn code (e.g., unexpected data formats, null values, etc.).

3. Resource constraints or timeouts if the DoFn performs operations that are resource-intensive or long-running.

To resolve these issues, you should:

1. Inspect the stack traces and error messages in the Dataflow monitoring interface for details on the exception.

2. Test the DoFn with a variety of data inputs, especially edge cases, to ensure robust error handling.

3. Review the resource usage and performance characteristics of the DoFn if the issue is related to resource constraints.

Reference:

https://cloud.google.com/dataflow/docs/guides/troubleshooting-your-pipeline#detect_an_exception_in_worker_code

</details>

---

## Q263

**Prompt:** You need to migrate 1 PB of data from an on-premises data center to Google Cloud. Data transfer time during the migration should take only a few hours. You want to follow Google-recommended practices to facilitate the large data transfer over a secure connection.

**Options:**
- A) Establish a Cloud Interconnect connection between the on-premises data center and Google Cloud, and then use the Storage Transfer Service.
- B) Use a Transfer Appliance and have engineers manually encrypt, decrypt, and verify the data.
- C) Establish a Cloud VPN connection, start gcloud compute scp jobs in parallel, and run checksums to verify the data.
- D) Reduce the data into 3 TB batches, transfer the data using gsutil, and run checksums to verify the data.

**Memorize:** `A) Establish a Cloud Interconnect connection between the on-premises data center and Google Cloud, and then use the Storage Transfer Service.`

<details>
<summary>Full explanation</summary>

Answer is 
Establish a Cloud Interconnect connection between the on-premises data center and Google Cloud, and then use the Storage Transfer Service.

 

Cloud Interconnect provides a dedicated private connection between on-prem and Google Cloud for high bandwidth (up to 100 Gbps) and low latency. This facilitates large, fast data transfers.

Storage Transfer Service supports parallel data transfers over Cloud Interconnect. It can transfer petabyte-scale datasets faster by transferring objects in parallel.

Storage Transfer Service uses HTTPS encryption in transit and at rest by default for secure data transfers.

It follows Google-recommended practices for large data migrations vs ad hoc methods like gsutil or scp.

The other options would take too long for a 1 PB transfer (VPN capped at 3 Gbps, manual transfers) or introduce extra steps like batching and checksums. Cloud Interconnect + Storage Transfer is the recommended Google solution.

Reference:

https://cloud.google.com/storage-transfer/docs/transfer-options#:~:text=Transferring%20more%20than%201%20TB%20from%20on%2Dpremises

</details>

---

## Q264

**Prompt:** Your company wants to be able to retrieve large result sets of medical information from your current system, which has over 10 TBs in the database, and store the data in new tables for further query.

**Options:**
- A) Use Cloud SQL, but first organize the data into tables. Use JOIN in queries to retrieve data.
- B) Use BigQuery as a data warehouse. Set output destinations for caching large queries.
- C) Use a MySQL cluster installed on a Compute Engine managed instance group for scalability.
- D) Use Cloud Spanner to replicate the data across regions. Normalize the data in a series of tables.

**Memorize:** `B) Use BigQuery as a data warehouse. Set output destinations for caching large queries.`

<details>
<summary>Full explanation</summary>

Answer is 
Use BigQuery as a data warehouse. Set output destinations for caching large queries.

 

The key reasons why BigQuery fits the requirements:

It is a fully managed data warehouse built to scale to handle massive datasets and perform fast SQL analytics

It has a low maintenance architecture with no infrastructure to manage

SQL capabilities allow easy querying of the medical data

Output destinations allow configurable caching for fast retrieval of large result sets

It provides a very cost-effective solution for these large scale analytics use cases

In contrast, Cloud Spanner and Cloud SQL would not scale as cost effectively for 10TB+ data volumes. Self-managed MySQL on Compute Engine also requires more maintenance. Hence, leveraging BigQuery as a fully managed data warehouse is the optimal solution here.

Reference:

https://cloud.google.com/bigquery/docs/query-overview

</details>

---

## Q265

**Prompt:** You are designing a system that requires an ACID-compliant database. You must ensure that the system requires minimal human intervention in case of a failure.

**Options:**
- A) Configure a Cloud SQL for MySQL instance with point-in-time recovery enabled.
- B) Configure a Cloud SQL for PostgreSQL instance with high availability enabled.
- C) Configure a Bigtable instance with more than one cluster.
- D) Configure a BigQuery table with a multi-region configuration.

**Memorize:** `B) Configure a Cloud SQL for PostgreSQL instance with high availability enabled.`

<details>
<summary>Full explanation</summary>

Answer is 
Configure a Cloud SQL for PostgreSQL instance with high availability enabled.

 

Key reasons:

Cloud SQL for PostgreSQL provides full ACID compliance, unlike Bigtable which provides only atomicity and consistency guarantees.

Enabling high availability removes the need for manual failover as Cloud SQL will automatically failover to a standby replica if the leader instance goes down.

Point-in-time recovery in MySQL requires manual intervention to restore data if needed.

BigQuery does not provide transactional guarantees required for an ACID database.

Therefore, a Cloud SQL for PostgreSQL instance with high availability meets the ACID and minimal intervention requirements best. The automatic failover will ensure availability and uptime without administrative effort.

Reference:

https://cloud.google.com/sql/docs/postgres/high-availability#HA-configuration

</details>

---

## Q266

**Prompt:** You are implementing workflow pipeline scheduling using open source-based tools and Google Kubernetes Engine (GKE). You want to use a Google managed service to simplify and automate the task.

**Options:**
- A) Use Dataflow for your workflow pipelines. Use Cloud Run triggers for scheduling.
- B) Use Dataflow for your workflow pipelines. Use shell scripts to schedule workflows.
- C) Use Cloud Composer in a Shared VPC configuration. Place the Cloud Composer resources in the host project.
- D) Use Cloud Composer in a Shared VPC configuration. Place the Cloud Composer resources in the service project.

**Memorize:** `D) Use Cloud Composer in a Shared VPC configuration. Place the Cloud Composer resources in the service project.`

<details>
<summary>Full explanation</summary>

Answer is 
Use Cloud Composer in a Shared VPC configuration. Place the Cloud Composer resources in the service project.

 

Shared VPC requires that you designate a host project to which networks and subnetworks belong and a service project, which is attached to the host project. When Cloud Composer participates in a Shared VPC, the Cloud Composer environment is in the service project.

Reference:

https://cloud.google.com/composer/docs/how-to/managing/configuring-shared-vpc

</details>

---

## Q267

**Prompt:** Flowlogistic is a leading logistics and supply chain provider. They help businesses throughout the world manage their resources and transport them to their final destination. The company has grown rapidly, expanding their offerings to include rail, truck, aircraft, and oceanic shipping. Company Background The company started as a regional trucking company, and then expanded into other logistics market. Because they have not updated their infrastructure, managing and tracking orders and shipments has become a bottleneck. To improve operations, Flowlogistic developed proprietary technology for tracking shipments in real time at the parcel level. However, they are unable to deploy it because their technology stack, based on Apache Kafka, cannot support the processing volume. In addition, Flowlogistic wants to further analyze their orders and shipments to determine how best to deploy their resources. Solution Concept Flowlogistic wants to implement two concepts using the cloud: - Use their proprietary technology in a real-time inventory-tracking system that indicates the location of their loads - Perform analytics on all their orders and shipment logs, which contain both structured and unstructured data, to determine how best to deploy resources, which markets to expand info. They also want to use predictive analytics to learn earlier when a shipment will be delayed. Existing Technical Environment Flowlogistic architecture resides in a single data center: - Databases 8 physical servers in 2 clusters • SQL Server "" user data, inventory, static data 3 physical servers • Cassandra "" metadata, tracking messages 10 Kafka servers "" tracking message aggregation and batch insert - Application servers "" customer front end, middleware for order/customs 60 virtual machines across 20 physical servers • Tomcat "" Java services • Nginx "" static content • Batch servers - Storage appliances • iSCSI for virtual machine (VM) hosts • Fibre Channel storage area network (FC SAN) "" SQL server storage • Network-attached storage (NAS) image storage, logs, backups - 10 Apache Hadoop /Spark servers • Core Data Lake • Data analysis workloads - 20 miscellaneous servers • Jenkins, monitoring, bastion hosts Business Requirements - Build a reliable and reproducible environment with scaled panty of production. - Aggregate data in a centralized Data Lake for analysis - Use historical data to perform predictive analytics on future shipments - Accurately track every shipment worldwide using proprietary technology - Improve business agility and speed of innovation through rapid provisioning of new resources - Analyze and optimize architecture for performance in the cloud - Migrate fully to the cloud if all other requirements are met Technical Requirements - Handle both streaming and batch data - Migrate existing Hadoop workloads - Ensure architecture is scalable and elastic to meet the changing demands of the company. - Use managed services whenever possible - Encrypt data flight and at rest - Connect a VPN between the production data center and cloud environment SEO Statement We have grown so quickly that our inability to upgrade our infrastructure is really hampering further growth and efficiency. We are efficient at moving shipments around the world, but we are inefficient at moving data around.

We need to organize our information so we can more easily understand where our customers are and what they are shipping. CTO Statement IT has never been a priority for us, so as our data has grown, we have not invested enough in our technology. I have a good staff to manage IT, but they are so busy managing our infrastructure that I cannot get them to do the things that really matter, such as organizing our data, building the analytics, and figuring out how to implement the CFO' s tracking technology. CFO Statement Part of our competitive advantage is that we penalize ourselves for late shipments and deliveries. Knowing where out shipments are at all times has a direct correlation to our bottom line and profitability. Additionally, I don't want to commit capital to building out a server environment. Flowlogistic wants to use Google BigQuery as their primary analysis system, but they still have Apache Hadoop and Spark workloads that they cannot move to BigQuery. Flowlogistic does not know how to store the data that is common to both workloads. What should they do?

**Options:**
- A) Store the common data in BigQuery as partitioned tables.
- B) Store the common data in BigQuery and expose authorized views.
- C) Store the common data encoded as Avro in Google Cloud Storage.
- D) Store he common data in the HDFS storage for a Google Cloud Dataproc cluster.

**Memorize:** `C) Store the common data encoded as Avro in Google Cloud Storage.`

<details>
<summary>Full explanation</summary>

Answer is 
Store the common data encoded as Avro in Google Cloud Storage.

Though data proc has connectors for cloud storage, Bigtable, and BigQuery, using a big query connector is a little more work compared to cloud storage and big table.

The best thing while moving apache Hadoop and spark jobs to data proc is using cloud storage instead of HDFS.

Your data in ORC, Parquet, Avro, or any other format will be used by different clusters or jobs, and you need data persistence if the cluster terminates.

We can just replace HDFS:// with gs://.

Even big query can read data in avro format.

The best way to store data common to both workloads is cloud storage.

Reference:

https://cloud.google.com/solutions/streaming-avro-records-into-bigquery-using-dataflow

</details>

---

## Q268

**Prompt:** Flowlogistic is a leading logistics and supply chain provider. They help businesses throughout the world manage their resources and transport them to their final destination. The company has grown rapidly, expanding their offerings to include rail, truck, aircraft, and oceanic shipping. Company Background The company started as a regional trucking company, and then expanded into other logistics market. Because they have not updated their infrastructure, managing and tracking orders and shipments has become a bottleneck. To improve operations, Flowlogistic developed proprietary technology for tracking shipments in real time at the parcel level. However, they are unable to deploy it because their technology stack, based on Apache Kafka, cannot support the processing volume. In addition, Flowlogistic wants to further analyze their orders and shipments to determine how best to deploy their resources. Solution Concept Flowlogistic wants to implement two concepts using the cloud: - Use their proprietary technology in a real-time inventory-tracking system that indicates the location of their loads - Perform analytics on all their orders and shipment logs, which contain both structured and unstructured data, to determine how best to deploy resources, which markets to expand info. They also want to use predictive analytics to learn earlier when a shipment will be delayed. Existing Technical Environment Flowlogistic architecture resides in a single data center: - Databases 8 physical servers in 2 clusters • SQL Server "" user data, inventory, static data 3 physical servers • Cassandra "" metadata, tracking messages 10 Kafka servers "" tracking message aggregation and batch insert - Application servers "" customer front end, middleware for order/customs 60 virtual machines across 20 physical servers • Tomcat "" Java services • Nginx "" static content • Batch servers - Storage appliances • iSCSI for virtual machine (VM) hosts • Fibre Channel storage area network (FC SAN) "" SQL server storage • Network-attached storage (NAS) image storage, logs, backups - 10 Apache Hadoop /Spark servers • Core Data Lake • Data analysis workloads - 20 miscellaneous servers • Jenkins, monitoring, bastion hosts Business Requirements - Build a reliable and reproducible environment with scaled panty of production. - Aggregate data in a centralized Data Lake for analysis - Use historical data to perform predictive analytics on future shipments - Accurately track every shipment worldwide using proprietary technology - Improve business agility and speed of innovation through rapid provisioning of new resources - Analyze and optimize architecture for performance in the cloud - Migrate fully to the cloud if all other requirements are met Technical Requirements - Handle both streaming and batch data - Migrate existing Hadoop workloads - Ensure architecture is scalable and elastic to meet the changing demands of the company. - Use managed services whenever possible - Encrypt data flight and at rest - Connect a VPN between the production data center and cloud environment SEO Statement We have grown so quickly that our inability to upgrade our infrastructure is really hampering further growth and efficiency. We are efficient at moving shipments around the world, but we are inefficient at moving data around.

We need to organize our information so we can more easily understand where our customers are and what they are shipping. CTO Statement IT has never been a priority for us, so as our data has grown, we have not invested enough in our technology. I have a good staff to manage IT, but they are so busy managing our infrastructure that I cannot get them to do the things that really matter, such as organizing our data, building the analytics, and figuring out how to implement the CFO' s tracking technology. CFO Statement Part of our competitive advantage is that we penalize ourselves for late shipments and deliveries. Knowing where out shipments are at all times has a direct correlation to our bottom line and profitability. Additionally, I don't want to commit capital to building out a server environment. Flowlogistic's management has determined that the current Apache Kafka servers cannot handle the data volume for their real-time inventory tracking system. You need to build a new system on Google Cloud Platform (GCP) that will feed the proprietary tracking software. The system must be able to ingest data from a variety of global sources, process and query in real-time, and store the data reliably. Which combination of GCP products should you choose?

**Options:**
- A) Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage
- B) Cloud Pub/Sub, Cloud Dataflow, and Local SSD
- C) Cloud Pub/Sub, Cloud SQL, and Cloud Storage
- D) Cloud Load Balancing, Cloud Dataflow, and Cloud Storage

**Memorize:** `A) Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage

Cloud pub sub for real time processing, cloud data flow for real time streaming data processing and loading data into cloud storage.

Reference:

https://codelabs.developers.google.com/codelabs/cpb104-pubsub/#0

</details>

---

## Q269

**Prompt:** Flowlogistic is a leading logistics and supply chain provider. They help businesses throughout the world manage their resources and transport them to their final destination. The company has grown rapidly, expanding their offerings to include rail, truck, aircraft, and oceanic shipping. Company Background The company started as a regional trucking company, and then expanded into other logistics market. Because they have not updated their infrastructure, managing and tracking orders and shipments has become a bottleneck. To improve operations, Flowlogistic developed proprietary technology for tracking shipments in real time at the parcel level. However, they are unable to deploy it because their technology stack, based on Apache Kafka, cannot support the processing volume. In addition, Flowlogistic wants to further analyze their orders and shipments to determine how best to deploy their resources. Solution Concept Flowlogistic wants to implement two concepts using the cloud: - Use their proprietary technology in a real-time inventory-tracking system that indicates the location of their loads - Perform analytics on all their orders and shipment logs, which contain both structured and unstructured data, to determine how best to deploy resources, which markets to expand info. They also want to use predictive analytics to learn earlier when a shipment will be delayed. Existing Technical Environment Flowlogistic architecture resides in a single data center: - Databases 8 physical servers in 2 clusters • SQL Server "" user data, inventory, static data 3 physical servers • Cassandra "" metadata, tracking messages 10 Kafka servers "" tracking message aggregation and batch insert - Application servers "" customer front end, middleware for order/customs 60 virtual machines across 20 physical servers • Tomcat "" Java services • Nginx "" static content • Batch servers - Storage appliances • iSCSI for virtual machine (VM) hosts • Fibre Channel storage area network (FC SAN) "" SQL server storage • Network-attached storage (NAS) image storage, logs, backups - 10 Apache Hadoop /Spark servers • Core Data Lake • Data analysis workloads - 20 miscellaneous servers • Jenkins, monitoring, bastion hosts Business Requirements - Build a reliable and reproducible environment with scaled panty of production. - Aggregate data in a centralized Data Lake for analysis - Use historical data to perform predictive analytics on future shipments - Accurately track every shipment worldwide using proprietary technology - Improve business agility and speed of innovation through rapid provisioning of new resources - Analyze and optimize architecture for performance in the cloud - Migrate fully to the cloud if all other requirements are met Technical Requirements - Handle both streaming and batch data - Migrate existing Hadoop workloads - Ensure architecture is scalable and elastic to meet the changing demands of the company. - Use managed services whenever possible - Encrypt data flight and at rest - Connect a VPN between the production data center and cloud environment SEO Statement We have grown so quickly that our inability to upgrade our infrastructure is really hampering further growth and efficiency. We are efficient at moving shipments around the world, but we are inefficient at moving data around.

We need to organize our information so we can more easily understand where our customers are and what they are shipping. CTO Statement IT has never been a priority for us, so as our data has grown, we have not invested enough in our technology. I have a good staff to manage IT, but they are so busy managing our infrastructure that I cannot get them to do the things that really matter, such as organizing our data, building the analytics, and figuring out how to implement the CFO' s tracking technology. CFO Statement Part of our competitive advantage is that we penalize ourselves for late shipments and deliveries. Knowing where out shipments are at all times has a direct correlation to our bottom line and profitability. Additionally, I don't want to commit capital to building out a server environment. Flowlogistic's CEO wants to gain rapid insight into their customer base so his sales team can be better informed in the field. This team is not very technical, so they've purchased a visualization tool to simplify the creation of BigQuery reports. However, they've been overwhelmed by all the data in the table, and are spending a lot of money on queries trying to find the data they need. You want to solve their problem in the most cost-effective way. What should you do?

**Options:**
- A) Export the data into a Google Sheet for virtualization.
- B) Create an additional table with only the necessary columns.
- C) Create a view on the table to present to the virtualization tool.
- D) Create identity and access management (IAM) roles on the appropriate columns, so only they appear in a query.

**Memorize:** `C) Create a view on the table to present to the virtualization tool.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a view on the table to present to the virtualization tool.

A logical view can be created with only the required columns which is required for visualization. B is not the right option as you will create a table and make it static. What happens when the original data is updated. This new table will not have the latest data and hence view is the best possible option here.

</details>

---

## Q270

**Prompt:** Flowlogistic is a leading logistics and supply chain provider. They help businesses throughout the world manage their resources and transport them to their final destination. The company has grown rapidly, expanding their offerings to include rail, truck, aircraft, and oceanic shipping. Company Background The company started as a regional trucking company, and then expanded into other logistics market. Because they have not updated their infrastructure, managing and tracking orders and shipments has become a bottleneck. To improve operations, Flowlogistic developed proprietary technology for tracking shipments in real time at the parcel level. However, they are unable to deploy it because their technology stack, based on Apache Kafka, cannot support the processing volume. In addition, Flowlogistic wants to further analyze their orders and shipments to determine how best to deploy their resources. Solution Concept Flowlogistic wants to implement two concepts using the cloud: - Use their proprietary technology in a real-time inventory-tracking system that indicates the location of their loads - Perform analytics on all their orders and shipment logs, which contain both structured and unstructured data, to determine how best to deploy resources, which markets to expand info. They also want to use predictive analytics to learn earlier when a shipment will be delayed. Existing Technical Environment Flowlogistic architecture resides in a single data center: - Databases 8 physical servers in 2 clusters • SQL Server "" user data, inventory, static data 3 physical servers • Cassandra "" metadata, tracking messages 10 Kafka servers "" tracking message aggregation and batch insert - Application servers "" customer front end, middleware for order/customs 60 virtual machines across 20 physical servers • Tomcat "" Java services • Nginx "" static content • Batch servers - Storage appliances • iSCSI for virtual machine (VM) hosts • Fibre Channel storage area network (FC SAN) "" SQL server storage • Network-attached storage (NAS) image storage, logs, backups - 10 Apache Hadoop /Spark servers • Core Data Lake • Data analysis workloads - 20 miscellaneous servers • Jenkins, monitoring, bastion hosts Business Requirements - Build a reliable and reproducible environment with scaled panty of production. - Aggregate data in a centralized Data Lake for analysis - Use historical data to perform predictive analytics on future shipments - Accurately track every shipment worldwide using proprietary technology - Improve business agility and speed of innovation through rapid provisioning of new resources - Analyze and optimize architecture for performance in the cloud - Migrate fully to the cloud if all other requirements are met Technical Requirements - Handle both streaming and batch data - Migrate existing Hadoop workloads - Ensure architecture is scalable and elastic to meet the changing demands of the company. - Use managed services whenever possible - Encrypt data flight and at rest - Connect a VPN between the production data center and cloud environment SEO Statement We have grown so quickly that our inability to upgrade our infrastructure is really hampering further growth and efficiency. We are efficient at moving shipments around the world, but we are inefficient at moving data around.

We need to organize our information so we can more easily understand where our customers are and what they are shipping. CTO Statement IT has never been a priority for us, so as our data has grown, we have not invested enough in our technology. I have a good staff to manage IT, but they are so busy managing our infrastructure that I cannot get them to do the things that really matter, such as organizing our data, building the analytics, and figuring out how to implement the CFO' s tracking technology. CFO Statement Part of our competitive advantage is that we penalize ourselves for late shipments and deliveries. Knowing where out shipments are at all times has a direct correlation to our bottom line and profitability. Additionally, I don't want to commit capital to building out a server environment. Flowlogistic is rolling out their real-time inventory tracking system. The tracking devices will all send package-tracking messages, which will now go to a single

Google Cloud Pub/Sub topic instead of the Apache Kafka cluster. A subscriber application will then process the messages for real-time reporting and store them in Google BigQuery for historical analysis. You want to ensure the package data can be analyzed over time. Which approach should you take?

**Options:**
- A) Attach the timestamp on each message in the Cloud Pub/Sub subscriber application as they are received.
- B) Attach the timestamp and Package ID on the outbound message from each publisher device as they are sent to Clod Pub/Sub.
- C) Use the NOW () function in BigQuery to record the event's time.
- D) Use the automatically generated timestamp from Cloud Pub/Sub to order the data.

**Memorize:** `B) Attach the timestamp and Package ID on the outbound message from each publisher device as they are sent to Clod Pub/Sub.`

<details>
<summary>Full explanation</summary>

Answer is 
Attach the timestamp and Package ID on the outbound message from each publisher device as they are sent to Clod Pub/Sub.

A. There is no indication that the application can do this. Moreover, due to networking problems, it is possible that Pub/Sub doesn't receive messages in order. This will analysis difficult.

B. This makes sure that you have access to publishing timestamp which provides you with the correct ordering of messages.

C. If timestamps are already messed up, BigQuery will get wrong results anyways.

D. The timestamp we are interested in is when the data was produced by the publisher, not when it was received by Pub/Sub.

</details>

---

## Q271

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments "" development/test, staging, and production "" to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. MJTelco's Google Cloud Dataflow pipeline is now ready to start receiving data from the 50,000 installations. You want to allow Cloud Dataflow to scale its compute power up as required. Which Cloud Dataflow pipeline configuration setting should you update?

**Options:**
- A) The zone
- B) The number of workers
- C) The disk size per worker
- D) The maximum number of workers

**Memorize:** `D) The maximum number of workers`

<details>
<summary>Full explanation</summary>

Answer is 
The maximum number of workers

Scalability is directly corerlated to max number of workers, size determines the speed of functioning.

Reference:

https://cloud.google.com/dataflow/docs/guides/specifying-exec-params

</details>

---

## Q272

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. You need to compose visualizations for operations teams with the following requirements: - The report must include telemetry data from all 50,000 installations for the most resent 6 weeks (sampling once every minute). - The report must not be more than 3 hours delayed from live data. - The actionable report should only show suboptimal links. - Most suboptimal links should be sorted to the top. - Suboptimal links can be grouped and filtered by regional geography. - User response time to load the report must be <5 seconds. Which approach meets the requirements?

**Options:**
- A) Load the data into Google Sheets, use formulas to calculate a metric, and use filters/sorting to show only suboptimal links in a table.
- B) Load the data into Google BigQuery tables, write Google Apps Script that queries the data, calculates the metric, and shows only suboptimal rows in a table in Google Sheets.
- C) Load the data into Google Cloud Datastore tables, write a Google App Engine Application that queries all rows, applies a function to derive the metric, and then renders results in a table using the Google charts and visualization API.
- D) Load the data into Google BigQuery tables, write a Google Data Studio 360 report that connects to your data, calculates a metric, and then uses a filter expression to show only suboptimal rows in a table.

**Memorize:** `D) Load the data into Google BigQuery tables, write a Google Data Studio 360 report that connects to your data, calculates a metric, and then uses a filter expression to show only suboptimal rows in a table.`

<details>
<summary>Full explanation</summary>

Answer is 
Load the data into Google BigQuery tables, write a Google Data Studio 360 report that connects to your data, calculates a metric, and then uses a filter expression to show only suboptimal rows in a table.

Data studio is best to accelerate data exploration and analysis.

Also once the data is loaded in big query the data can be easily visualized in data studio.

</details>

---

## Q273

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. You create a new report for your large team in Google Data Studio 360. The report uses Google BigQuery as its data source. It is company policy to ensure employees can view only the data associated with their region, so you create and populate a table for each region. You need to enforce the regional access policy to the data. Which two actions should you take? (Choose two.)

**Options:**
- A) Ensure all the tables are included in global dataset.
- B) Ensure each table is included in a dataset for a region.
- C) Adjust the settings for each table to allow a related region-based security group view access.
- D) Adjust the settings for each view to allow a related region-based security group view access.
- E) Adjust the settings for each dataset to allow a related region-based security group view access.

**Memorize:** `C) Adjust the settings for each table to allow a related region-based security group view access.`

<details>
<summary>Full explanation</summary>

Answers are; 

B. Ensure each table is included in a dataset for a region.

C. Adjust the settings for each table to allow a related region-based security group view access.

BigQuery come with table level access control. Since we can have table-level access and each region represents a table, B & C is correct answer.

Reference:

https://cloud.google.com/blog/products/data-analytics/introducing-table-level-access-controls-in-bigquery

</details>

---

## Q274

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. MJTelco needs you to create a schema in Google Bigtable that will allow for the historical analysis of the last 2 years of records. Each record that comes in is sent every 15 minutes, and contains a unique identifier of the device and a data record. The most common query is for all the data for a given device for a given day. Which schema should you use?

**Options:**
- A) Rowkey: date#device_id Column data: data_point
- B) Rowkey: date Column data: device_id, data_point
- C) Rowkey: device_id Column data: date, data_pointMost Voted
- D) Rowkey: data_point Column data: device_id, date
- E) Rowkey: date#data_point Column data: device_id

**Memorize:** `A) Rowkey: date#device_id Column data: data_point`

<details>
<summary>Full explanation</summary>

Answer is 
Rowkey: date#device_id Column data: data_point

From the question "Most common query is all data for a given device for a given day". so A, B, C Rowkey will work to query. But B and C will cause larger Rows hence poor performance. Also, considering granularity of date & device_id RowKey with "date#device_id" performs better.

</details>

---

## Q275

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. MJTelco is building a custom interface to share data. They have these requirements: 1. They need to do aggregations over their petabyte-scale datasets. 2. They need to scan specific time range rows with a very fast response time (milliseconds). Which combination of Google Cloud Platform products should you recommend?

**Options:**
- A) Cloud Datastore and Cloud Bigtable
- B) Cloud Bigtable and Cloud SQL
- C) BigQuery and Cloud Bigtable
- D) BigQuery and Cloud Storage

**Memorize:** `C) BigQuery and Cloud Bigtable`

<details>
<summary>Full explanation</summary>

Answer is 
BigQuery and Cloud Bigtable

They need to do aggregations over their petabyte-scale datasets: Bigquery

They need to scan specific time range rows with a very fast response time (milliseconds): Bigtable

</details>

---

## Q276

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. You need to compose visualization for operations teams with the following requirements: - Telemetry must include data from all 50,000 installations for the most recent 6 weeks (sampling once every minute) - The report must not be more than 3 hours delayed from live data. - The actionable report should only show suboptimal links. - Most suboptimal links should be sorted to the top. - Suboptimal links can be grouped and filtered by regional geography. - User response time to load the report must be <5 seconds. You create a data source to store the last 6 weeks of data, and create visualizations that allow viewers to see multiple date ranges, distinct geographic regions, and unique installation types. You always show the latest data without any changes to your visualizations. You want to avoid creating and updating new visualizations each month. What should you do?

**Options:**
- A) Look through the current data and compose a series of charts and tables, one for each possible combination of criteria.
- B) Look through the current data and compose a small set of generalized charts and tables bound to criteria filters that allow value selection.
- C) Export the data to a spreadsheet, compose a series of charts and tables, one for each possible combination of criteria, and spread them across multiple tabs.
- D) Load the data into relational database tables, write a Google App Engine application that queries all rows, summarizes the data across each criteria, and then renders results using the Google Charts and visualization API.

**Memorize:** `B) Look through the current data and compose a small set of generalized charts and tables bound to criteria filters that allow value selection.`

<details>
<summary>Full explanation</summary>

Answer is 
Look through the current data and compose a small set of generalized charts and tables bound to criteria filters that allow value selection.

</details>

---

## Q277

**Prompt:** MJTelco Case Study Company Overview MJTelco is a startup that plans to build networks in rapidly growing, underserved markets around the world. The company has patents for innovative optical communications hardware. Based on these patents, they can create many reliable, high-speed backbone links with inexpensive hardware. Company Background Founded by experienced telecom executives, MJTelco uses technologies originally developed to overcome communications challenges in space. Fundamental to their operation, they need to create a distributed data infrastructure that drives real-time analysis and incorporates machine learning to continuously optimize their topologies. Because their hardware is inexpensive, they plan to overdeploy the network allowing them to account for the impact of dynamic regional politics on location availability and cost. Their management and operations teams are situated all around the globe creating many-to-many relationship between data consumers and provides in their system. After careful consideration, they decided public cloud is the perfect environment to support their needs. Solution Concept MJTelco is running a successful proof-of-concept (PoC) project in its labs. They have two primary needs: - Scale and harden their PoC to support significantly more data flows generated when they ramp to more than 50,000 installations. - Refine their machine-learning cycles to verify and improve the dynamic models they use to control topology definition. MJTelco will also use three separate operating environments; " development/test, staging, and production " to meet the needs of running experiments, deploying new features, and serving production customers. Business Requirements - Scale up their production environment with minimal cost, instantiating resources when and where needed in an unpredictable, distributed telecom user community. - Ensure security of their proprietary data to protect their leading-edge machine learning and analysis. - Provide reliable and timely access to data for analysis from distributed research workers - Maintain isolated environments that support rapid iteration of their machine-learning models without affecting their customers. Technical Requirements - Ensure secure and efficient transport and storage of telemetry data - Rapidly scale instances to support between 10,000 and 100,000 data providers with multiple flows each. - Allow analysis and presentation against data tables tracking up to 2 years of data storing approximately 100m records/day - Support rapid iteration of monitoring infrastructure focused on awareness of data pipeline problems both in telemetry flows and in production learning cycles. CEO Statement Our business model relies on our patents, analytics and dynamic machine learning. Our inexpensive hardware is organized to be highly reliable, which gives us cost advantages. We need to quickly stabilize our large distributed data pipelines to meet our reliability and capacity commitments. CTO Statement Our public cloud services must operate as advertised. We need resources that scale and keep our data secure. We also need environments in which our data scientists can carefully study and quickly adapt our models. Because we rely on automation to process our data, we also need our development and test environments to work as we iterate. CFO Statement The project is too large for us to maintain the hardware and software required for the data and analysis. Also, we cannot afford to staff an operations team to monitor so many data feeds, so we will rely on automation and infrastructure. Google Cloud's machine learning will allow our quantitative researchers to work on our high-value problems instead of problems with our data pipelines. Given the record streams MJTelco is interested in ingesting per day, they are concerned about the cost of Google BigQuery increasing. MJTelco asks you to provide a design solution. They require a single large data table called tracking_table. Additionally, they want to minimize the cost of daily queries while performing fine-grained analysis of each day's events. They also want to use streaming ingestion. What should you do?

**Options:**
- A) Create a table called tracking_table and include a DATE column.
- B) Create a partitioned table called tracking_table and include a TIMESTAMP column.
- C) Create sharded tables for each day following the pattern tracking_table_YYYYMMDD.
- D) Create a table called tracking_table with a TIMESTAMP column to represent the day.

**Memorize:** `B) Create a partitioned table called tracking_table and include a TIMESTAMP column.`

<details>
<summary>Full explanation</summary>

Answer is 
Create a partitioned table called tracking_table and include a TIMESTAMP column.

Partitioned Table for Faster Query and Low cost (because it will process less data)

Reference:

https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard

</details>

---

## Q278

**Prompt:** Flowlogistic Case Study Company Overview Flowlogistic is a leading logistics and supply chain provider. They help businesses throughout the world manage their resources and transport them to their final destination. The company has grown rapidly, expanding their offerings to include rail, truck, aircraft, and oceanic shipping. Company Background The company started as a regional trucking company, and then expanded into other logistics market. Because they have not updated their infrastructure, managing and tracking orders and shipments has become a bottleneck. To improve operations, Flowlogistic developed proprietary technology for tracking shipments in real time at the parcel level. However, they are unable to deploy it because their technology stack, based on Apache Kafka, cannot support the processing volume. In addition, Flowlogistic wants to further analyze their orders and shipments to determine how best to deploy their resources. Solution Concept Flowlogistic wants to implement two concepts using the cloud: - Use their proprietary technology in a real-time inventory-tracking system that indicates the location of their loads - Perform analytics on all their orders and shipment logs, which contain both structured and unstructured data, to determine how best to deploy resources, which markets to expand info. They also want to use predictive analytics to learn earlier when a shipment will be delayed. Existing Technical Environment Flowlogistic architecture resides in a single data center: Databases - 8 physical servers in 2 clusters - SQL Server; user data, inventory, static data - 3 physical servers - Cassandra "" metadata, tracking messages - 10 Kafka servers "" tracking message aggregation and batch insert Application servers; customer front end, middleware for order/customs 60 virtual machines across 20 physical servers • Tomcat "" Java services • Nginx "" static content • Batch servers - Storage appliances • iSCSI for virtual machine (VM) hosts • Fibre Channel storage area network (FC SAN) "" SQL server storage • Network-attached storage (NAS) image storage, logs, backups - 10 Apache Hadoop /Spark servers • Core Data Lake • Data analysis workloads - 20 miscellaneous servers • Jenkins, monitoring, bastion hosts Business Requirements - Build a reliable and reproducible environment with scaled panty of production. - Aggregate data in a centralized Data Lake for analysis - Use historical data to perform predictive analytics on future shipments - Accurately track every shipment worldwide using proprietary technology - Improve business agility and speed of innovation through rapid provisioning of new resources - Analyze and optimize architecture for performance in the cloud - Migrate fully to the cloud if all other requirements are met Technical Requirements - Handle both streaming and batch data - Migrate existing Hadoop workloads - Ensure architecture is scalable and elastic to meet the changing demands of the company. - Use managed services whenever possible - Encrypt data flight and at rest - Connect a VPN between the production data center and cloud environment SEO Statement We have grown so quickly that our inability to upgrade our infrastructure is really hampering further growth and efficiency. We are efficient at moving shipments around the world, but we are inefficient at moving data around.

We need to organize our information so we can more easily understand where our customers are and what they are shipping. CTO Statement IT has never been a priority for us, so as our data has grown, we have not invested enough in our technology. I have a good staff to manage IT, but they are so busy managing our infrastructure that I cannot get them to do the things that really matter, such as organizing our data, building the analytics, and figuring out how to implement the CFO' s tracking technology. CFO Statement Part of our competitive advantage is that we penalize ourselves for late shipments and deliveries. Knowing where out shipments are at all times has a direct correlation to our bottom line and profitability. Additionally, I don't want to commit capital to building out a server environment. Flowlogistic's management has determined that the current Apache Kafka servers cannot handle the data volume for their real-time inventory tracking system. You need to build a new system on Google Cloud Platform (GCP) that will feed the proprietary tracking software. The system must be able to ingest data from a variety of global sources, process and query in real-time, and store the data reliably. Which combination of GCP products should you choose?

**Options:**
- A) Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage
- B) Cloud Pub/Sub, Cloud Dataflow, and Local SSD
- C) Cloud Pub/Sub, Cloud SQL, and Cloud Storage
- D) Cloud Load Balancing, Cloud Dataflow, and Cloud Storage
- E) Cloud Dataflow, Cloud SQL, and Cloud Storage

**Memorize:** `A) Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage`

<details>
<summary>Full explanation</summary>

Answer is 
Cloud Pub/Sub, Cloud Dataflow, and Cloud Storage

Kafka --> replace by PubSub, Streaming then Dataflow, store data reliably and not mention any other condition then Cloud Storage

</details>

---
