---
description: 网络编程之Socket
---

# Socket

## Socket基础

`socket`是比较低级的模块，python提供了一些对`socket`的封装，使得使用`socket`更加容易。

这里主要记录对`socket`模块的学习。

### Creating 创建

**socket.socket(family=AF\_INET, type=SOCK\_STREAM, proto=0, fileno=None)**

* **family：address family 地址族**
  * 该参数传入地址族类型，一般`AF_INET`，`AF_INET6`比较常用。
* **type：socket type 套接字类型**
  * 该参数传入套接字类型，一般`SOCK_STREAM (the default)`，`SOCK_DGRAM`比较常用。
* **proto：protocol number 协议号**
  * 该参数一般为零，也可忽略不传。

```
Create a new socket using the given address family, socket type and protocol number. 
    The address family should be AF_INET (the default), AF_INET6, AF_UNIX, AF_CAN or AF_RDS. 
    The socket type should be SOCK_STREAM (the default), SOCK_DGRAM, SOCK_RAW or perhaps one of the other SOCK_ constants. 
    The protocol number is usually zero and may be omitted or in the case where the address family is AF_CAN the protocol should be one of CAN_RAW or CAN_BCM. 
    If fileno is specified, the other arguments are ignored, causing the socket with the specified file descriptor to return. Unlike socket.fromfd(), fileno will return the same socket and not a duplicate. This may help close a detached socket using socket.close().
```

> 3.4版本之后返回的套接字不可被继承。
>
> Changed in version 3.4: The returned socket is now non-inheritable.

### Constants 常量

```
socket.AF_UNIX
socket.AF_INET：Ipv4网络协议
socket.AF_INET6：Ipv6网络协议

    These constants represent the address (and protocol) families, used for the first argument to socket(). If the AF_UNIX constant is not defined then this protocol is unsupported. More constants may be available depending on the system.
------------------------------------------------------------------------------------------------
socket.SOCK_STREAM：提供面向连接的稳定数据传输，使用的是TCP协议
socket.SOCK_DGRAM：无保障的面向消息的socket，基于UDP协议
socket.SOCK_RAW
socket.SOCK_RDM
socket.SOCK_SEQPACKET

    These constants represent the socket types, used for the second argument to socket(). More constants may be available depending on the system. (Only SOCK_STREAM and SOCK_DGRAM appear to be generally useful.)
------------------------------------------------------------------------------------------------
socket.SOCK_CLOEXEC
socket.SOCK_NONBLOCK

    These two constants, if defined, can be combined with the socket types and allow you to set some flags atomically (thus avoiding possible race conditions and the need for separate calls).
```

## Socket详解

### Socket Server

* **.bind(address)**
  *   绑定socket对象的地址。该对象必须是还未绑定的socket对象。

      ```
      Bind the socket to address. The socket must not already be bound. (The format of address depends on the address family)
      ```
* **.listen(\[backlog])**：
  *   启用服务器接受连接。指定系统在拒绝新连接之前将允许的未接受连接的数量。如果未指定，则选择默认的合理值。

      ```
      Enable a server to accept connections. If backlog is specified, it must be at least 0 (if it is lower, it is set to 0); it specifies the number of unaccepted connections that the system will allow before refusing new connections. If not specified, a default reasonable value is chosen.
      ```
* **.accept()**
  *   接收连接，套接字必须绑定到地址并监听连接。返回值是一个元组`(conn, address)`，`conn`是一个新的`socket`对象，可用于发送和接收信息，`address`是绑定的另一端的套接字地址。

      ```
      Accept a connection. The socket must be bound to an address and listening for connections. The return value is a pair (conn, address) where conn is a new socket object usable to send and receive data on the connection, and address is the address bound to the socket on the other end of the connection. 
      ```

### Socket Client

* **.connect(address)**：
  *   通过`address`连接一个远程的`socket`服务器。

      ```
      Connect to a remote socket at address. (The format of address depends on the address family)

      If the connection is interrupted by a signal, the method waits until the connection completes, or raise a socket.timeout on timeout, if the signal handler doesn’t raise an exception and the socket is blocking or has a timeout. For non-blocking sockets, the method raises an InterruptedError exception if the connection is interrupted by a signal (or the exception raised by the signal handler).
      ```

### Socket

* ### **.recv(**_**bufsize**_**\[, **_**flags**_**])** ：
* **.recvfrom(**_**bufsize**_**\[, **_**flags**_**])** ：
* **.recvmsg(**_**bufsize**_**\[, **_**ancbufsize**_**\[, **_**flags**_**]])** ：
* **.send(**_**bytes**_**\[, **_**flags**_**])** ：
* **.sendto(**_**bytes**_**, **_**flags**_**, **_**address**_**)** ：
* **.sendmsg(**_**buffers**_**\[, **_**ancdata**_**\[, **_**flags**_**\[, **_**address**_**]]])** ：
* **.close()** ：
