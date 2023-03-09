---
layout: default
title: UHadoop
parent: Big Data
grand_parent: Public Cloud
permalink: /public-cloud/big-data/uhadoop/
nav_order: 2
---
# UHadoop - Managed Hadoop cluster
## Product Introduction
UHadoop is a hosting service for a big data processing system running on the SCloud platform. By running Hadoop and Spark on a cloud platform, users can easily analyze and process their own data using other peripheral systems in the Hadoop and Spark ecosystems (such as Apache Hive, Apache Pig, HBase, etc.).
## Product architecture diagram

![image](https://user-images.githubusercontent.com/124770063/224007640-550f58e3-f823-4d0a-bf94-98c426041658.png)

### HDFS
HDFS is deployed in HA mode by default, with two Name nodes deployed on master1 and master2 respectively, Data nodes distributed on all Core nodes, and Task not deploying Data nodes.

![image](https://user-images.githubusercontent.com/124770063/224007726-a843a6f7-4112-45c7-9c00-eb69a06d04be.png)

### Yarn
Yarn also adopts HA deployment by default, with 2 ResourceManagers deployed on master1 and master2 respectively, and Node managers distributed on all Core and Task nodes.

![image](https://user-images.githubusercontent.com/124770063/224007770-beaa9398-cc36-4543-8f60-2a8ff3ef421d.png)

### Hive
Hive currently only supports on yarn mode, and two Hive-MetaStores are deployed in master1 and master2, and connected to local mySQL, avoiding Hive service failures caused by the downtime of a single master node.

You can connect to Hive services through HiveCli or Beeline.

![image](https://user-images.githubusercontent.com/124770063/224007835-bd56e88d-f856-4987-8c1a-cf3a176384e4.png)

### HBase
HBase is deployed by HA by default, with two HMasters deployed on master1 and master2 respectively, and HRegionServer distributed on all Core nodes.

![image](https://user-images.githubusercontent.com/124770063/224007872-69bd46db-cf3d-4c5f-852b-143c7dad2224.png)

### Spark
Spark adopts the On Yarn pattern, please refer to the Spark Development Guide for details.

## Product features and advantages
### Convenient
Create clusters in minutes, without worrying about node allocation, deployment, and optimization; With rich examples and scenario tutorials, you can quickly get started and achieve your business goals.

### Use
- Automated deployment according to the selected hardware model (CPU, memory, disk), selected software combination and version;
- Users can request where their cluster is deployed based on the geographic location of themselves or the data source. Currently, UHADOOP supports regions such as North China, Guangzhou, and Hong Kong. It will be released to all regions supported by SCloud in the future.

### Elasticity
The cluster can be large or small, and supports dynamic scaling to effectively avoid waste of resources. Supports separation of compute and storage.

### Opening
Fully compatible with the open source community version of Hadoop/Spark, customers can use open source standard APIs to write jobs and migrate to the cloud without any modifications.

### Safe
User clusters are located in a dedicated virtual private network to achieve complete isolation of resources.

### Stable
Key components such as Hadoop, Spark, and HBase in the cluster support high availability features to ensure service availability.

## Port configuration

| Configuration name| UHadoop default configuration |
| -- | -- |
| yarn.resourcemanager.zk-address | localhost:2181 |
| yarn.resourcemanager.address.rm1 | master1:23140 |
| yarn.resourcemanager.address.rm2 | master2:23140 |
| yarn.resourcemanager.scheduler.address.rm1 | master1:23130 |
| yarn.resourcemanager.scheduler.address.rm2 | master2:23130 |
| yarn.resourcemanager.webapp.https.address.rm1 | master1:23189 |
| yarn.resourcemanager.webapp.https.address.rm2 | master2:23189 |
| yarn.resourcemanager.webapp.address.rm1 | master1:23188 |
| yarn.resourcemanager.webapp.address.rm2 | master2:23188 |
| yarn.resourcemanager.admin.address.rm1 | master1:23141 |
| yarn.resourcemanager.admin.address.rm2 | master2:23141 |
| yarn.resourcemanager.resource-tracker.address.rm1 | master1:23125 |
| yarn.resourcemanager.resource-tracker.address.rm2 | master2:23125 |
| yarn.nodemanager.localizer.address | 0.0.0.0:23344 |
| NM Webapp address | 0.0.0.0:23999 |
| zeppelin.server.port | master1:29090 |
| presto coordinator http-server.http.port | master1:28080 |
| Presto worker http-server.http.port (core, task node) | 28080 |
| mapreduce.shuffle.port | 23080 |
| mapreduce.jobhistory.address | 10020 |
| dfs.datanode.address | 50010 |
| dfs.datanode.http.address | 50075 |
| dfs.datanode.https.address | 50475 |
| dfs.datanode.ipc.address | 50020 |
| fs.defaultFS | 8020 |
| dfs.namenode.servicerpc-address | 8022 |
| dfs.namenode.http-address | 50070 |
| dfs.namenode.https-address | 50470 |
| dfs.namenode.secondary.http-address | 50090 |
| dfs.secondary.https.address | 50495 |
| dfs.namenode.shared.edits.dir | 8485 |
| dfs.journalnode.http-address | 8480 |
| dfs.journalnode.https-address | 8481 |
| DFSZKFailoverController | 8019 |
| hbase.master.port | 60000 |
| hbase.master.info.port | 60010 |
| hbase.regionserver.port | 60020 |
| hbase.zookeeper.property.clientPort | 2181 |
| hbase.zookeeper.peerport | 2888 |
| hbase.zookeeper.leaderport | 3888 |
| hbase.rest.port | 60050 |
| hive.server2.thrift.port | 10000 |
| Hive metastore | 9083 |
| Zookeeper ClientPort | 2181 |
| Zookeeper Peer | 2888, 3888 |
