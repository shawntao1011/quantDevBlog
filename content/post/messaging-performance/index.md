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

### feeder

{{< gist shawntao1011 df454f98c1078c73fb488234cab1c0fc >}}

### tp

{{< gist shawntao1011 76c03051815b23121346122a010fd79a >}}

### rdb

{{< gist shawntao1011 b572b865fd55baa366c900687bbe5f2a >}}

### run.cmd

{{< gist shawntao1011 76c03051815b23121346122a010fd79a >}}

## kafka

### feeder

### kafka server

### rdb

## solace

### feeder

### solace instant

### rdb

