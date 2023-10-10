+++
author = "Tao"
title = "messaging solutions' performance for kdb arch"
date = "2023-09-27"
description = "performance compare on kafka, solace and tickerplant"
categories = [
    "messaging",
    "performance testing"
]
tags = [
    "kafka",
    "solace",
    "tickerplant"
]
image = "images/streaming.jpg"
+++

## test detail and environment
Test data is a txt file of 2 million rows of Json. The json are consist of jsonlized Ticks and Txns data.

## q tickerplant
![kdb_tickerplant_frame](images/kdb_tp_frame.png)

Tickerplant is on a remote server. Log writing is performed exactly on the same server(which means local). 

FH, RDB, RTS publish and subscribe data through KDB Q IPC.

### Performance Highlights

**Pub Speed:** </p> 1 million msgs 6 second

**Deserialize speed:**  </p> no need to deserialize

### feeder

{{< gist shawntao1011 df454f98c1078c73fb488234cab1c0fc >}}

### tp

{{< gist shawntao1011 76c03051815b23121346122a010fd79a >}}

### rdb

{{< gist shawntao1011 b572b865fd55baa366c900687bbe5f2a >}}

### run.cmd

{{< gist shawntao1011 76c03051815b23121346122a010fd79a >}}

## kafka

Kafka server is on the same host with tickerplant. 

PubSub is performed remotely in the same local network.

🐣 [kx kafka library](https://github.com/KxSystems/kafka)

### Performance Highlights

**Pub Speed:** </p> 1 million msgs 7.5 second

**Deserialize speed:**  </p> 1 million msgs 36 second

### feeder

{{< gist shawntao1011 627ee401bd560988f281e7fb08b5cd41 >}}

### kafka server

To download kafka, please take reference [kafka quick setup](https://kafka.apache.org/documentation.html#quickstart).

Zookeeper and a Kafka broker are initialized using the following commands.

zookeeper:
```
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```
The server need to <font color=red>add</font> *advertised.listeners=PLAINTEXT://your.host.name:9092*
```
bin\kafka-server-start.bat config\server.properties
```

### consumer

{{< gist shawntao1011 e9ba596c035820562fe9f6ed19501bb9 >}}

## solace

### feeder

### solace instant

### rdb

