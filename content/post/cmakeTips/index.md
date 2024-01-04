+++
author = "Tao Shawn"
title = "CMake Tips"
date = "2024-01-04"
description = "Tips on using CMake"
tags = [
    "buildSystem",
]
categories = [
    "experience",
]
series = ["Themes Guide"]
aliases = ["CMake Tips"]
image = "images/CMakeLogo.svg"
+++

Notes on using cmake. 

Gonna explain and document follow a **PREP**( POINT REASON EXAMPLE POINT ) style.

## How to copy the runTime dll 
```cmake
add_custom_command(TARGET targetName POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy -t $<TARGET_FILE_DIR:targetName> $<TARGET_RUNTIME_DLLS:targetName>
    COMMAND_EXPAND_LISTS
)
```

For windows, a dynamic library consists of two files(xxx.lib file and xxx.dll). The .lib file is required when linking to corresponding exe and library. The .lib is useless during RUNTIME. The .dll file is required during RUNTIME, to run executable or run cmake test, we need to copy the .dll to the corresponding exe directory.

## How to add precompiled lib?

```cmake
add_library( libName
    [SHARED|STATIC] IMPORTED
)
# linux
set_property(TARGET libName PROPERTY
    IMPORTED_IMPLIB [pathToLibDir]/officialLibName.[a|so])

# windows
set_property(TARGET libName PROPERTY
    IMPORTED_IMPLIB [pathToLibDir]/officialLibName.lib)
set_property(TARGET libName PROPERTY
    IMPORTED_LOCATION [pathToLibDir]/officialDllName.dll)
```

Though vcpkg and linux package provide us with good and easy method to download package, which can be easy find through cmake `find_package`, the following occasion require to use other method to find a library and link to it:

- link to a precomiled lib or library not available through package manage system. </p>For instance, [Solace](https://solace.com/downloads/) library is not available through `vcpkg` or `apt-get`. The downloaded file will contain the lib and dll we need. </p> File tree:
    ```cmd
    solclient-[version]
        |---bin
            |---win32
                |---solclient.dll
            |---win64
                |---solclient.dll
        |---include
            |---solclient.h
            |---solclientMsg.h
        |---lib
            |---win32
                |---solclient.lib
            |---win64
                |---solclient.lib
    ```
    Take windows x64 build for example, the `CMakeLists.txt` will be:
    ```cmake
    add_library( SOLACE_LIB
        SHARED IMPORTED)
    set_property(TARGET SOLACE_LIB PROPERTY
        IMPORTED_IMPLIB .../solclient-[version]/${CMAKE_GENERATER_PLATFORM}/lib/solclient.lib)
    set_property(TARGET libName PROPERTY
        IMPORTED_LOCATION .../solclient-[version]/${CMAKE_GENERATER_PLATFORM}/lib/solclient.dll)

    target_include_directories( targetName
        [PRIVATE|PUBLIC|INTERFACE] 
        .../solclient-[version]/include)

    target_link_libraries(targetName
        SOLACE_LIB)
    ```

- `find_package` fail to find the correct dll. Take `zlib` for example, the following cmake steps work as expected.
    ```cmake
    find_library(ZLIB REQUIRED)
    ...
    target_link_libraries( targetName 
        ZLIB::ZLIB)
    ```
    However, when it comes to coping the dll, cmake will fail to find the correct one as zlib's dll name is not exactly `zlib.dll`( Release mode: zlib<font color=red>1</font>.dll Debug mode: zlib<font color=red>d1</font>.dll ).

    In this scenario, whether to switch from `find_package` to `add_library` is up to one's favor.
