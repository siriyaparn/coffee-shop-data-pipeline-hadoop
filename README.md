# Data Engineering Project: Coffee Shop Data Pipeline with Hadoop
This project is from [Road to Data Engineer](https://school.datath.com/courses/road-to-data-engineer-2-0-2023) course. Road to Data Engineer is a course that provides fundamental to advanced knowledge in the Data Engineer track. We can apply this knowledge to build automated data pipelines.

## Project Overview
This project aims to create a data pipeline for a coffee shop using Hadoop ecosystem to process and calculate daily member card points for running campaigns and promotions. It also intends to send real-time data when a customer places an order, allowing the back office system to quickly check the latest order.

### Hadoop System Design
<img src="picture/hadoop-system-design.png" width=100% height=40%>

- Batch Layer: Handles offline data processing tasks, typically for historical customer data in this project.
- Speed Layer: Deals with real-time data processing, handling transaction data when a customer places an order.

## Batch Layer Workflow
### Data Collection
- Source System: Text files are generated in form of csv (customer.csv).
- CLI: The Hadoop command-line interface (CLI) is used to interact with the system.
- HDFS: Text files is stored in the Hadoop Distributed File System (HDFS).

### Data Processing
- SparkSQL: Data is loaded into SparkSQL for data cleansing.
- HIVE: Data is transformed and queried using Hive which is a data warehousing infrastructure built on top of Hadoop.

### Output
- Tables: Customer data is stored in Hive tables for further analysis or reporting.

## Speed Layer Workflow
### Data Collection
- Source System: Log files are generated from a source system containing transaction data when a customer places an order.
- Flume: Flume is used to collect, aggregate, and move log data into the Hadoop ecosystem.
- HDFS: Log data is stored in HDFS.
- HBASE: Data is stored in HBase, a NoSQL database optimized for fast read/write operations.

### Data Processing
- Spark Streaming: Real-time data is processed using Spark Streaming.
- HIVE: Data is transformed and queried using Hive.

### Output
- Tables: Transaction data is stored in Hive tables for further analysis or reporting.

## Automated workflow with Apache Oozie
- Join customer data and transaction data on the customer ID (id) to create a table suitable for calculating membership points.
- Set the scheduler to create and save a new table each day by running workflows daily.

## Getting Start
### Setting-up Google Compute Engine (Optional)
**1. Provision GCP Compute Engine for Docker**
- Machine configuration : `n2-standard-4(4vCPU, 16GB memory)`
- Boot disk : `Ubuntu 20.04 LTS, size 40GB`
- Allow HTTP, HTTPS traffic
  
**2. Setting-up Firewall Rule**
- Allow on match : `Allow`
- Source filter : `IPv4 ranges`
- Source IPv4 ranges : `0.0.0.0/0`
- Protocols and ports : specified > tcp > `7180,8888`

**3. Getting Started with Cloudera Hadoop via a Docker Container**
- Install Docker on Ubuntu 20.04 following steps 1-4 from Digital Ocean's Guide > [Here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)
- Once the installation is complete, use the command to pull the Docker image to your VM.
```sh
docker pull mikelemikelo/cloudera-spark:latest
```
- Run a container from the mikelemikelo/cloudera-spark:latest image with specific configurations.
```sh
docker run --hostname=quickstart.cloudera --privileged=true -it -p 8888:8888 -p
8080:8080 -p 7180:7180 -p 88:88/udp -p 88:88 mikelemikelo/cloudera-spark:latest /usr/bin/docker-quickstart-light
```
- Use the command to start Cloudera Manager.
```sh
sudo /home/cloudera/cloudera-manager --express && service ntpd start
```

- Use the Public/External IP of the VM to access Cloudera Manager on port `7180`, for example, `http://34.87.136.199:7180/`.
- Before starting the cluster, go to the Hive search on the left and set the default configuration to `None`.
- Remove any unused and unnecessary services, such as `Spark` and `KV Indexer`.
- Add `Flume` service
- Start the cluster and wait until the service reaches an `all-up-and-running` status.
- Log in to Cloudera HUE using the VM's Public/External IP on port `8888`, for example, `http://34.87.136.199:8888/`.
