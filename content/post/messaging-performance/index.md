+++
author = "Tao"
title = "messaging solutions' performance for kdb arch"
date = "2023-09-27"
description = "performance compare on kafka, solace and tickerplant"
categories = [
    "messaging",
    "proofOfConcept"
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

sample(one json one row):
```
"{
    "date":"yyyy-mm-ddThh:mm:ss.sssssssss",
    "sym":"xxxxxx.xx",
    "time":"yyyy-mm-ddThh:mm:ss.sssssssss",
    "ticker":"xxxxxx",
    "side":"N",
    "px":0,
    "size":xxx,
    "index":xxxxxx,
    "askId":0,
    "bidId":xxxxxx,
    "ordType":"0",
    "func":"C"
}",
"{
    "date":"yyyy-mm-ddThh:mm:ss.sssssssss",
    "sym":"xxxxxx.xx",
    "time":"yyyy-mm-ddThh:mm:ss.sssssssss",
    "open":xx.xx,
    "high":xx.xx,
    "low":xx.xx,
    "preclose":xx.xx,
    "px":xx.xx,
    "match":xxxx,
    "volume":xxxxxx,
    "amt":xxxxxxx,
    "asks":[xx.89,xx.9,xx.91,xx.93,xx.94,xx.95,xx.96,xx.97,xx.99,xx],
    "avgAsk":xx.xx,
    "askSizes":[4800,75300,300,1600,5200,5000,5100,300,3800,104446],"totalAskSize":xxxxxx,
    "bids":[xx.88,xx.87,xx.86,xx.85,xx.84,xx.83,xx.82,xx.81,xx.8,xx.79],
    "avgBid":xx.xx,
    "bidSizes":[2500,1800,500,5900,6500,3600,2000,6200,18700,4000],"totalBidSize":xxxxxx,
    "presettle":null,
    "settle":null
}"
```

## q tickerplant
![kdb_tickerplant_frame](images/kdb_tp_frame.png)

Tickerplant is on a remote server. Log writing is performed exactly on the same server(which means local). 

FH, RDB, RTS publish and subscribe data through KDB Q IPC.

### Performance Highlights

**Pub Speed:** </p> 1 million msgs 6 second

**Deserialize**  </p> no need to deserialize

**Replay**  </p> 1 million msgs 10 second

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

**Deserialize:**  </p> 1 million msgs 36 second

**Consume Speed:** </p> 1 million msgs total 50 second, include deserialize and upd to table.

**Replay performance** </p> performance is exactly the same as consumer.

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
bin\windows\kafka-server-start.bat config\server.properties
```

### consumer

{{< gist shawntao1011 e9ba596c035820562fe9f6ed19501bb9 >}}

### replay

replay in kafka requires to change the client config. The other is exactly the same as start a consumer.
```
client: .kfk.Consumer[
 ....
    (`group.id;`new port number);
    //(`auto.offset.reset;`latest)
    (`auto.offset.reset;`earliest)
 ];
```

## solace

🐣 [solace container guide](https://solace.com/products/event-broker/software/getting-started/)

🐥 [solace kx library](https://github.com/KxSystems/solace)

### Performance Highlights

**Pub Speed:** </p> 1 million msgs 55 second

**Deserialize:**  </p> 1 million msgs 36 second

**Consume Speed:** </p> 1 million msgs total 1min45s, include deserialize and upd to table.

**Replay performance** </p> 1 millon msgs 150 seconds

### producer

{{< gist shawntao1011 ace523e6a136828e27c0226491737109 >}}

### solace instant
```
docker run -d -p 8080:8080 -p 55555:55555 -p 8008:8008 -p 1883:1883 -p 8000:8000 -p 5672:5672 -p 9000:9000 -p 2222:2222 --shm-size=2g --env username_admin_globalaccesslevel=admin --env username_admin_password=admin --name=solace solace/solace-pubsub-standard
```

### consumer

{{< gist shawntao1011 051079edf1fdbe54595617dd4539cbd4 >}}

### replay

{{< youtube HuYgF_IsfXw >}}

