+++
author = "Tao Shawn"
title = "network programming concepts"
date = "2023-11-17"
description = "My notes on learninng network programming"
tags = [
    "network programming",
    "c",
    "cpp",
]
categories = [
    "c/cpp",
    "learning",
]
series = ["Themes Guide"]
aliases = ["network programming concepts"]
image = "images/networkProgrammingConcepts.png"
+++

My notes on learning network programming. This post is mainly on concepts and examples from 

[Beej's Guide to Network Programming Using Internet Sockets](https://beej.us/guide/bgnet/)

The original Beej's guide only provide Unix/Linux source code examples. Windows example src, mentioned in Beej's guide, is provided on 

[Beej's Socket examples ported to windows](https://www.tallyhawk.net/WinsockExamples)

## Structs and functions

- addrinfo: one of the **first** thing to call when making a connection
```cpp
struct addrinfo {
    int         ai_flags;           // AI_PASSIVE, AI_CANONNAME, etc.
    int         ai_family;          // AF_INET, AF_INET6, AF_UNSPEC
    int         ai_socktype;        // SOCK_STREAM, SOCK_DGRAM
    int         ai_protocol;        // use 0 for "any"
    size_t      ai_addrlen;         // size of ai_addr in bytes
    struct      sockaddr *ai_addr;  // struct sockaddr_in or _in6
    char        *ai_canonname;      // full canonical hostname
    struct      addrinfo *ai_next;  // linked list, next node
};
```
*Note: the ai_next is a linked list to next node*

 where struct sockaddr is :
 ```cpp
 struct sockaddr {
    unsigned short  sa_family;      // address family, AF_xxx
    char            sa_data[14];    // 14 bytes of protocol address
};
 ```
The *sa_family* can be set to AF_INET (IPV4) or AF_INET6 (IPV6).

Since the *sa_data* is not going to be manually packed. Programmers created a parallel structure *sockaddr_in* to be used with IPv4. 

And here is the trick: a pointer to a struct *sockaddr_in*
```cpp
struct sockaddr_in addr = {...}
```
can be cast to a pointer to a struct sockaddr and *vice-versa*
```cpp
(const struct sockaddr*)&addr
```

So, even a function call requires "a pointer to a struct sockaddr", we can still work on *sockaddr_in* at the last minutes.

For instance:
```cpp
// bind socket fd 
struct sockaddr_in addr = {};
addr.sin_fmily = AF_INET;
addr.sin_port = noths(12345);
addr.sin_addr.s_addr = ntohl(0);

int res = bind(fd, (const sockadr*)&addr, sizeof(addr));
```

The struct *sockaddr_in*:
```cpp
// (IPv4 only--see struct sockaddr_in6 for IPv6)
struct sockaddr_in {
    short int           sin_family; // Address family, AF_INET
    unsigned short int  sin_port;   // Port number
    struct in_addr      sin_addr;   // Internet address
    unsigned char       sin_zero[8];// Same size as struct sockaddr
};

// Internet address (a structure for historical reasons)
struct in_addr {
    uint32_t    s_addr; // that's a 32-bit int (4 bytes)
};
```

## 

