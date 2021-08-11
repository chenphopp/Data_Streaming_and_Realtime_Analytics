# Part 1 - Setup and Prerequisites

## Kafka overview

[Apache Kafka](https://kafka.apache.org) is a distributed streaming platform. It provides publish-subscribe APIs and can store and process streams of records at large scale.

Kafka is made up of the following components:
- <b> Broker (or server) </b> : One of more brokers form a Kafka cluster
- <b> Producer </b> : Client to send records into Kafka
- <b> Consumer </b> : Client to read records from Kafka
- <b> Admin </b> : Client to manage Kafka clusters
- <b> Connect </b> : Runtime for Sink and Source Connectors to stream data between external systems and Kafka
- <b> Streams </b> : Library to process streams of records in real time

![Kafka Platform](./kafka-platform.png)

You can learn more about Kafka in this introductory article,

"[What is Apache Kafka](https://developer.ibm.com/articles/an-introduction-to-apache-kafka/)" 

or in this conference presentation, 

"[Introducing Apache Kafka](https://developer.ibm.com/videos/an-introduction-to-apache-kafka/)."

## Prerequisites for this workshop

In order to complete this workshop, you need to have the following dependencies installed:

- [Java SDK](https://jdk.java.net/java-se-ri/10), Version 10 //Set Environment path
- [gradle](https://gradle.org/install/), Version 6 or above  //Set Environment path

## Downloading Apache Kafka

In this workshop, we will use the Kafka command line tools.

[Download](https://downloads.apache.org/kafka/2.7.0/kafka_2.13-2.7.0.tgz) binary package from the Apache Kafka website, uncompress it.

```sh
> tar -xzf kafka-2.13-2.7.0.tgz
> cd kafka-2.13-2.7.0
```

## Setup Kafka cluster
# Setting up Apache Kafka on a local machine

Follow these steps if you want to set up a 3-broker Kafka cluster on a single machine. This environment is not suitable for production, but it provides all the features necessary to complete this workshop.

## 1) Starting ZooKeeper

The first step is to get ZooKeeper running. This is necessary in order to start Kafka:

We can start ZooKeeper with the default configuration file, by running this command:

```sh
C:\kafka_2.13-2.7.0>bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```

We have now started a ZooKeeper ensemble consisting of a single server. Again, this is not suitable for production but it is enough to start a Kafka cluster.

## Configuring a local Kafka cluster

Kafka provides a default Kafka configuration file, `config/server.properties`. We will reuse this file and make a few changes.

1. Make the three copies of `config/server.properties` and named them as:
    - `config/server0.properties`
    - `config/server1.properties`
    - `config/server2.properties`

2. In each of three files:
    - Replace lines from 74 to 76 with:

    ```properties
    offsets.topic.replication.factor=3
    transaction.state.log.replication.factor=3
    transaction.state.log.min.isr=3
    ```
    - Replace line 21 with `broker.id=<BROKER_ID>`
    - Replace line 31 with `listeners=PLAINTEXT://:9<BROKER_ID>92`
    - Replace line 60 with `log.dirs=/tmp/kafka<BROKER_ID>-logs`
     
    `where <BROKER_ID>` is the number of the file name, 
     
    For example, `config/server2.properties` includes
    - `broker.id=2`
    - `listeners=PLAINTEXT://:9292`
    - `log.dirs=/tmp/kafka2-logs`  

## 2) Starting the Kafka cluster

Now that we have all the required configurations, let's start our brokers:

```sh
C:\kafka_2.13-2.7.0>bin\windows\kafka-server-start.bat .\config\server0.properties
```

Then, open a new terminal window and run:

```sh
C:\kafka_2.13-2.7.0>bin\windows\kafka-server-start.bat .\config\server1.properties
```

Then, open a new terminal window and run:
```sh
C:\kafka_2.13-2.7.0>bin\windows\kafka-server-start.bat .\config\server2.properties
```

Congratulations, you've now started your Kafka cluster!

```html
Note that if a server is off,
(1) close all terminals,
(2) delete all existing folders and files in "C:\tmp" and "C:\kafka_2.13-2.7.0\logs", and 
(3) run again.
```

## Menu
Continue to [Part 2](../part2/README.md)

Return to [Week02](../README.md)




