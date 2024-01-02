+++
author = "Tao Shawn"
title = "torQ notes"
date = "2023-10-19"
description = "My notes on implementing torQ"
tags = [
    "deployment",
    "torQ",
]
categories = [
    "kdb",
    "experience",
]
series = ["Themes Guide"]
aliases = ["torQ logo"]
image = "images/torQlogo.png"
+++

Torq, mainly the <font color=blue>torq.q</font>, is a framework provided by Data Intellect. It implements some core functionality and utilities on top of kdb+, allowing developers to concentrate on the application business logic.

![TorQ Arch](images/TorQStructure.png)
<!--more-->

## Quick Deploy

For a minimized TorQ deployment with appliable business logic, the following components(processes) should be configured:

1. discovery

    "Processes use the discovery service to register their own availability, find other processes (by process type) and subscribe to receive updates for new process availability (by process type)."

    The following are some important facts of discovery service:

    1. The tickerplant process, which is the most important part of the whole pipeline system, is not (and shall not) registered or informed through discovery service.

    2. The raw discovery mainly focus on the following information
    ```
    q).servers.SERVERS
    procname        proctype    hpup                   w hits startp lastp                         endp attributes
    --------------------------------------------------------------------------------------------------------------
    discovery-1     discovery   :laptop-emhga05g:11001   0           2023.10.24D03:32:29.291568000      ()!()
    monitor-1       monitor     :laptop-emhga05g:11000   0           2023.10.24D03:32:30.117205000      ()!()
    tickerplant-mkt tickerplant :laptop-emhga05g:11002   0           2023.10.24D03:32:30.117205000      ()!()
    gateway-1       gateway     :laptop-emhga05g:11003   0           2023.10.24D03:32:30.117205000      ()!()
    rdb-1           rdb         :laptop-emhga05g:11004   0           2023.10.24D03:32:30.117205000      ()!()
    ```

2. monitor 

3. tickerplant

4. gateway

5. rdb

