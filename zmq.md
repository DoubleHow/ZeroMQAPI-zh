#zmq(7)　　　　　　0MQ Manual - 0MQ/3.2.5

#Name

zmq – ØMQ 轻量级消息传输内核

#Synopsis

    #include <znq.h>

    cc [flags] files –lzmq [libraries]

#Description

ØMQ轻量级消息传输内核是一个从标准socket接口的扩展而来的链接库，这些接口通常是由一些专门的传送中间设备来提供。ØMQ提供了一个步消息传送、多模式消息传送、消息过滤（订阅）、对多种传输协议无缝接入的集合。

本文档呈现了ØMQ的概念，描述了ØMQ是怎样对标准socket进行抽象并为ØMQ库提供的函数提供了一个参考手册。

Context

在使用任何ØMQ库函数之前，必须创建一个ØMQ context。在结束应用程序时，必须销毁（删除）这个context。一下函数是用来对context进行操作的。

● 创建一个新的context

zmq_ctx_new(3)

● 操作context的属性

zmq_ctx_set(3) zmq_ctx_get(3)

● 删除一个context

zmq_ctx_destroy(3)

● 监视一个context

zmq_ctx_set_monitor(3)

以下被弃用的函数也可以用来创建和删除context

● 初始化一个context

zmq_init(3)

● 终结一个context

zmq_term(3)

Thread safety（线程安全）

ØMQ context 是线程安全的，可以根据需要尽可能任意的被多个线程共享，而不需要在调用端添加请求锁。

除非把一个socket的内存整体的从过一个线程移动到一个线程中，否则单个的socket不是线程安全的。事实上，这就意味着，应用程序可以在一个线程中用函数zmq_soc ket ()创建一个socket，然后把它传递给一个新创建线程，作为这个线程初始化时的一部分。比如，你可以把一个结构体作为一个参数传递给函数pthread_create() 。

Multiple contexts

在一个应用程序中可以有多个context共存。这样应用程序在对context直接进行操作的同时，还可以使用任意多其它库或者模块，这些模块本身也在像上文中提到的那样线程安全的使用ØMQ。

Messages

一个ØMQ的消息在应用程序之间或者在同一个程序的不同模块之间传输的是一个离散的消息单元。ØMQ消息没有内部的结构，并且从ØMQ自身的角度来看，消息被看作是不透明的二进制数据。

下面的函数用来对消息进行操作：

● 初始化一个消息

zmq_msg_init(3)  zmq_msg_init_size(3)  zmq_msg_init _data(3)

● 发送和接收消息

zmq_msg_send(3)  zmq_msg_recv(3)

● 释放消息

zmq_msg_close(3)

● 获取消息

zmq_msg_data(3)  zmq_msg_size(3)  zmq_msg_mor e(3)

● 设置消息属性

zmq_msg_get (3)  zmq_msg_set (3)

● 消息处理

zmq_msg_copy(3)  zmq_msg_move(3)

Sockets

ØMQ sockets 使用一种抽象的异步消息队列，这通过准确的基于socket类型语义方式实现。查询zmq_socket (3)函数可以看到都提供了哪些socket类型可使用。

以下提供的函数用来对socket进行操作：

● 创建一个socket

zmq_socket (3)

● 关闭一个socket

zmq_close(3)

● 操作socket属性

zmq_getsockopt (3)  zmq_setsockopt (3)

● 指定socket 消息流

zmq_bind(3)  zmq_connect (3)

● 发送与接收消息

zmq_msg_send(3)  zmq_msg_recv(3)  zmq_send(3)  zmq_recv(3)

Input/output multiplexing （多路输入/输出）

ØMQ为ØMQ socktes和标准socktes均提供了一种使应用程序进行多路输入/输出的机制。这种机制映射而来标准的poll()系统调用，在zmq_poll(3)中有更详细的描述。

Transports

一个ØMQ socket 可以使用多种不同的基础传输机制。每种传输机制适合于具体的使用目的，并且每种方式都有它自己的优点和缺点。

● 单一传输使用TCP

zmq_tcp(7)

● 可靠的多路广播传输使用 PGM

zmq_pgm(7)

● 进程内部的本地传输

zmq_ipc(7)

● 进程内部（线程内部）的本地传输

zmq_inproc(7)

Proxies

ØMQ提供了代理来创建扇入和扇出的拓扑方式。一个代理把frontend socket链接到backend socket，并且不透明的在两个sockte之间传输所有的消息。代理可以不透明的获取第三方的所有传输数据。你可以使用zmq_proxy(3) 在应用程序中使用代理。

Error handling

ØMQ库函数使用POSIX系统中的标准规定来获取错误信息。通常来说，这就意味着ØMQ库函数会返回NULL（如果是返回指针类型）或者返回一个负值（如果是返回int类型），而真正的错误值会存储在errno变量中。

在非POSIX系统中，开发者们可能会经历errno变量当前值的情况。zmq_errno()函数就是用来解决这样的问题的。更多的细节请查看zmq_errno(3)函数。

zmq_strerror()函数用来将ØMQ指定的错误值转换为字符串类型的信息，更多的细节请查看函数zmq_strerror(3)。

Miscellaneous

下面是一些其余的函数：

● 返回ØMQ 库的版本号

zmq_version(3)

Language bindings

ØMQ库提为各种语言提供了可供调用的接口。本文档描写的借口可以用来给C程序猿使用。本文的意图旨在，使用其它语言的ØMQ使用者们应该能够通过本文联系到他们自己的语言版本。

语言版本（C++、Python、PHP、Ruby、Java等等）被ØMQ社团的其它成员提供，你可以在ØMQ网站上找到他们。

Authors

This ØMQ manual  page was written by Martin Sustrik < sustrik@250bpm.com > , MartinLucina < martin@lucina.net > , and Pieter  Hintjens < ph@imatix.com > .

Resources

Main website:http://www.zeromq.org/

Report  bugs  to  the ØMQ development  mailing  list:< zeromq-dev@lists.zeromq.org>

Copying

Free use of this software isgranted under the terms of the GNU Lesser General  Public License

(LGPL). For details see the files COPYING and COPYING. LESSER included with the ØMQ distribution.

Website design and content is copyright (c) 2007-2012 iMatix Corporation. Contactus  for professional support. Site content licensed under the Creative Commons  Attribution-Share Alike 3.0 License . ØMQ is copyright (c) Copyright (c) 2007-2012 iMatix Corporation and Contributors . ØMQ is free software licensed under the  LGPL. ØMQ, ZeroMQ, and 0MQ are trade marks of iMatix Corporation. Terms of Use — Privacy Policy
