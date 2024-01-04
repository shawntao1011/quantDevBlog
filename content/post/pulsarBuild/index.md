+++
author = "Tao Shawn"
title = "pulsar build"
date = "2024-01-02"
description = "How to Pulsar"
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
image = "images/pulsarLogo.svg"
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
 -DCMAKE_TOOLCHAIN_FILE=[vcpkgPath]/vcpkg.cmake
 -S .
cmake --build ./build --config Release
```

However, this method will generate two vcpkg directories to manage, which are ${PROJECT_SOURCE_DIR} and ${CMAKE_CURRENT_BINARY_DIR}.

The above package install method is much like JS package managing method, which managing package dependency for each project. In favor of using Linux method, I recommend `vcpkg install` all the package under *vcpkg* directory. 

By default, if there is a `vcpkg.json` file under the ${PROJECT_SOURCE_DIR}, then vcpkg will use the MANIFEST mode. To clarify, vcpkg will check what package is available in ${VCPKG_INSTALLEd_DIR}, and install missing package under ${CMAKE_CURRENT_SOURCE_DIR}. 

