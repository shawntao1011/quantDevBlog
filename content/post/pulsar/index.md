+++
author = "Tao Shawn"
title = "pulsar"
date = "2024-01-02"
description = "My document on Pulsar"
tags = [
    "messaging",
    "cpp",
    "pulsar"
]
categories = [
    "c/cpp",
    "learning",
]
series = ["Themes Guide"]
aliases = ["Pulsar"]
image = "images/pulsar.png"
+++

My notes on building and using Pulsar on windows.

Pulsar git: [https://github.com/apache/pulsar-client-cpp.git](https://github.com/apache/pulsar-client-cpp.git)

Pulsar cpp client: [https://github.com/apache/pulsar-client-cpp.git](https://github.com/apache/pulsar-client-cpp.git)

## Build on windows

The official buidling guide is to RUN vcpkg <font color=red>first </font> 
```cmd
[Your path to vcpkg repo]\vcpkg.exe install --feature-flags=manifests --triplet x64-windows
```
and then run the following:
```
cmake \
 -B ./build \
 -A x64 \
 -DBUILD_TESTS=OFF \
 -DVCPKG_TRIPLET=x64-windows \
 -DCMAKE_BUILD_TYPE=Release \
 -S .
cmake --build ./build --config Release
```

However, this method will generate two vcpkg directories to manage, which are ${PROJECT_SOURCE_DIR} and ${CMAKE_CURRENT_BINARY_DIR}.


 