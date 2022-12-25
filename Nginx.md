# Nginx

## 1 I/O模型

### 1.1 web服务介绍

#### 1.1.1 服务端I/O

I/O在计算机中指Input/Output， IOPS (Input/Output Per Second)即每秒的输入输出量(或读写次数)， 是衡量磁盘性能的主要指标之一。IOPS是指单位时间内系统能处理的I/O请求数量，一般以每秒处理的 I/O请求数量为单位，I/O请求通常为读或写数据操作请求。

一次完整的I/O是用户空间的进程数据与内核空间的内核数据的报文的完整交换，

但是由于内核空间与用户空间是严格隔离的，所以其数据交换过程中不能由用户空间的进程直接调用内核空间的内存数据，

而是需要经历一次从内核空间中的内存数据copy到用户空间的进程内存当中，所以简单说I/O就是把数据 从内核空间中的内存数据复制到用户空间中进程的内存当中。

Linux的I/O

* 磁盘I/O
* 网络I/O：一切皆文件，本质是对socket的读写

#### 1.1.2 磁盘I/O

磁盘I/O是进程向内核发起系统调用，请求磁盘上的某个资源比如是html 文件或者图片，然后内核通过 相应的驱动程序将目标文件加载到内核的内存空间，加载完成之后把数据从内核内存再复制给进程内 存，如果是比较大的数据也需要等待时间

```http
机械磁盘的寻道时间、旋转延迟和数据传输时间：
寻道时间：是指磁头移动到正确的磁道上所花费的时间，寻道时间越短则I/O处理就越快，目前磁盘的寻道时间一般在3-15毫秒左右。
旋转延迟：是指将磁盘片旋转到数据所在的扇区到磁头下面所花费的时间，旋转延迟取决于磁盘的转速，通常使用磁盘旋转一周所需要时间的1/2之一表示，比如7200转的磁盘平均训传延迟大约为60*1000/7200/2=4.17毫秒，公式的意思为 （每分钟60秒*1000毫秒每秒/7200转每分/2），如果是15000转的则为60*1000/15000/2=2毫秒。
数据传输时间：指的是读取到数据后传输数据的时间，主要取决于传输速率，这个值等于数据大小除以传输速率，目前的磁盘接口每秒的传输速度可以达到600MB，因此可以忽略不计。
常见的机械磁盘平均寻道时间值：
7200转/分的磁盘平均物理寻道时间：9毫秒
10000转/分的磁盘平均物理寻道时间：6毫秒
15000转/分的磁盘平均物理寻道时间：4毫秒
常见磁盘的平均延迟时间：
7200转的机械盘平均延迟：60*1000/7200/2 = 4.17ms
10000转的机械盘平均延迟：60*1000/10000/2 = 3ms
15000转的机械盘平均延迟：60*1000/15000/2 = 2ms
每秒最大IOPS的计算方法：
7200转的磁盘IOPS计算方式：1000毫秒/(9毫秒的寻道时间+4.17毫秒的平均旋转延迟时间)=1000/13.13=75.9 IOPS
10000转的磁盘的IOPS计算方式：1000毫秒/(6毫秒的寻道时间+3毫秒的平均旋转延迟时间)=1000/9=111IOPS
15000转的磁盘的IOPS计算方式：15000毫秒/(4毫秒的寻道时间+2毫秒的平均旋转延迟时间)=1000/6=166.6 IOPS
```

#### 1.1.3 网络I/O 

网络通信就是网络协议栈到用户空间进程的IO就是网络IO

网络I/O 处理过程

```http
获取请求数据，客户端与服务器建立连接发出请求，服务器接受请求（1-3）
构建响应，当服务器接收完请求，并在用户空间处理客户端的请求，直到构建响应完成（4）
返回数据，服务器将已构建好的响应再通过内核空间的网络 I/O 发还给客户端（5-7）
```

不论磁盘和网络I/O

```http
每次I/O，都要经由两个阶段：
第一步：将数据从文件先加载至内核内存空间（缓冲区），等待数据准备完成，时间较长
第二步：将数据从内核缓冲区复制到用户空间的进程的内存中，时间较短
```

### 1.2 I/O 模型

#### 1.2.1 I/O 模型相关概念

同步/异步：关注的是消息通信机制，即调用者在等待一件事情的处理结果时，被调用者是否提供完成状态的通知。

* 同步：synchronous，被调用者并不提供事件的处理结果相关的通知消息，需要调用者主动询问事情是否处理完成 
* 异步：asynchronous，被调用者通过状态、通知或回调机制主动通知调用者被调用者的运行状态

阻塞/非阻塞：关注调用者在等待结果返回之前所处的状态

* 阻塞：blocking，指IO操作需要彻底完成后才返回到用户空间，调用结果返回之前，调用者被挂 起，干不了别的事情。 
* 非阻塞：nonblocking，指IO操作被调用后立即返回给用户一个状态值，而无需等到IO操作彻底完 成，在最终的调用结果返回之前，调用者不会被挂起，可以去做别的事情。

#### 1.2.2 网络 I/O 模型

阻塞型、非阻塞型、复用型、信号驱动型、异步

##### 1.2.2.1 阻塞型 I/O 模型（blocking IO）

阻塞IO模型是最简单的I/O模型，用户线程在内核进行IO操作时被阻塞 

用户线程通过系统调用read发起I/O读操作，由用户空间转到内核空间。内核等到数据包到达后，然后将 接收的数据拷贝到用户空间，完成read操作 

用户需要等待read将数据读取到buffer后，才继续处理接收的数据。整个I/O请求的过程中，用户线程是 被阻塞的，这导致用户在发起IO请求时，不能做任何事情，对CPU的资源利用率不够 

优点：程序简单，在阻塞等待数据期间进程/线程挂起，基本不会占用 CPU 资源 

缺点：每个连接需要独立的进程/线程单独处理，当并发请求量大时为了维护程序，内存、线程切换开销 较大，apache 的preforck使用的是这种模式

```http
同步阻塞：程序向内核发送I/O请求后一直等待内核响应，如果内核处理请求的IO操作不能立即返回,则进程将一直等待并不再接受新的请求，并由进程轮询查看I/O是否完成，完成后进程将I/O结果返回给Client，在IO没有返回期间进程不能接受其他客户的请求，而且是有进程自己去查看I/O是否完成，这种方式简单，但是比较慢，用的比较少。
```

##### 1.2.2.2 非阻塞型 I/O 模型 (nonblocking IO)

用户线程发起IO请求时立即返回。但并未读取到任何数据，用户线程需要不断地发起IO请求，直到数据 到达后，才真正读取到数据，继续执行。即 “轮询”机制存在两个问题：如果有大量文件描述符都要等， 那么就得一个一个的read。这会带来大量的Context Switch（read是系统调用，每调用一次就得在用户 态和核心态切换一次）。轮询的时间不好把握。这里是要猜多久之后数据才能到。等待时间设的太长， 程序响应延迟就过大;设的太短，就会造成过于频繁的重试，干耗CPU而已，是比较浪费CPU的方式，一 般很少直接使用这种模型，而是在其他IO模型中使用非阻塞IO这一特性。

```http
非阻塞：程序向内核发送请I/O求后一直等待内核响应，如果内核处理请求的IO操作不能立即返回IO结果，进程将不再等待，而且继续处理其他请求，但是仍然需要进程隔一段时间就要查看内核I/O是否完成。
```

在设置连接为非阻塞时，当应用进程系统调用 recvfrom 没有数据返回时，内核会立即 返回一个 EWOULDBLOCK 错误，而不会一直阻塞到数据准备好。如上图在第四次调用时有一个数据报准备 好了，所以这时数据会被复制到 应用进程缓冲区 ，于是 recvfrom 成功返回数据

当一个应用进程这样循环调用 recvfrom 时，称之为轮询 polling 。这么做往往会耗费大量CPU时间， 实际使用很少

##### 1.2.2.3 多路复用 I/O 型(I/O multiplexing)

上面的模型中,每一个文件描述符对应的IO是由一个线程监控和处理 

多路复用IO指一个线程可以同时（实际是交替实现，即并发完成）监控和处理多个文件描述符对应各自 的IO，即复用同一个线程 

一个线程之所以能实现同时处理多个IO,是因为这个线程调用了内核中的SELECT,POLL或EPOLL等系统调 用，从而实现多路复用IO

I/O multiplexing 主要包括:select，poll，epoll三种系统调用，select/poll/epoll的好处就在于单个 process就可以同时处理多个网络连接的IO。 

它的基本原理就是select/poll/epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。 

当用户进程调用了select，那么整个进程会被block，而同时，kernel会“监视”所有select负责的socket， 当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从 kernel拷贝到用户进程。 

Apache prefork是此模式的select，worker是poll模式。

```http
IO多路复用（IO Multiplexing) ：是一种机制，程序注册一组socket文件描述符给操作系统，表示“我要监视这些fd是否有IO事件发生，有了就告诉程序处理”

IO多路复用一般和NIO一起使用的。NIO和IO多路复用是相对独立的。NIO仅仅是指IO API总是能立刻返回，不会被Blocking;而IO多路复用仅仅是操作系统提供的一种便利的通知机制。操作系统并不会强制这俩必须得一起用，可以只用IO多路复用 + BIO，这时还是当前线程被卡住。IO多路复用和NIO是要配合一起使用才有实际意义

IO多路复用是指内核一旦发现进程指定的一个或者多个IO条件准备读取，就通知该进程
多个连接共用一个等待机制，本模型会阻塞进程，但是进程是阻塞在select或者poll这两个系统调用上，而不是阻塞在真正的IO操作上

用户首先将需要进行IO操作添加到select中，同时等待select系统调用返回。当数据到达时，IO被激活，select函数返回。用户线程正式发起read请求，读取数据并继续执行
从流程上来看，使用select函数进行IO请求和同步阻塞模型没有太大的区别，甚至还多了添加监视IO，以及调用select函数的额外操作，效率更差。并且阻塞了两次，但是第一次阻塞在select上时，select可以监控多个IO上是否已有IO操作准备就绪，即可达到在同一个线程内同时处理多个IO请求的目的。而不像阻塞IO那种，一次只能监控一个IO

虽然上述方式允许单线程内处理多个IO请求，但是每个IO请求的过程还是阻塞的（在select函数上阻塞），平均时间甚至比同步阻塞IO模型还要长。如果用户线程只是注册自己需要的IO请求，然后去做自己的事情，等到数据到来时再进行处理，则可以提高CPU的利用率
IO多路复用是最常使用的IO模型，但是其异步程度还不够“彻底”，因它使用了会阻塞线程的select系统调用。因此IO多路复用只能称为异步阻塞IO模型，而非真正的异步IO
```

优缺点 

* 优点：可以基于一个阻塞对象，同时在多个描述符上等待就绪，而不是使用多个线程(每个文件描述 符一个线程)，这样可以大大节省系统资源 
* 缺点：当连接数较少时效率相比多线程+阻塞 I/O 模型效率较低，可能延迟更大，因为单个连接处 理需要 2 次系统调用，占用时间会有增加

IO多路复用适用如下场合： 

* 当客户端处理多个描述符时（一般是交互式输入和网络套接口），必须使用I/O复用 
* 当一个客户端同时处理多个套接字时，此情况可能的但很少出现 
* 当一个服务器既要处理监听套接字，又要处理已连接套接字，一般也要用到I/O复用 
* 当一个服务器即要处理TCP，又要处理UDP，一般要使用I/O复用 
* 当一个服务器要处理多个服务或多个协议，一般要使用I/O复用

##### 1.2.2.4 信号驱动式 I/O 模型 (signal-driven IO)

信号驱动I/O的意思就是进程现在不用傻等着，也不用去轮询。而是让内核在数据就绪时，发送信号通知进程。

调用的步骤是，通过系统调用 sigaction ，并注册一个信号处理的回调函数，该调用会立即返回，然后 主程序可以继续向下执行，当有I/O操作准备就绪,即内核数据就绪时，内核会为该进程产生一个 SIGIO 信号，并回调注册的信号回调函数，这样就可以在信号回调函数中系统调用 recvfrom 获取数据,将用户 进程所需要的数据从内核空间拷贝到用户空间

此模型的优势在于等待数据报到达期间进程不被阻塞。用户主程序可以继续执行，只要等待来自信号处 理函数的通知。 

在信号驱动式 I/O 模型中，应用程序使用套接口进行信号驱动 I/O，并安装一个信号处理函数，进程继 续运行并不阻塞 

当数据准备好时，进程会收到一个 SIGIO 信号，可以在信号处理函数中调用 I/O 操作函数处理数据。优 点：线程并没有在等待数据时被阻塞，内核直接返回调用接收信号，不影响进程继续处理其他请求因此 可以提高资源的利用率

缺点：信号 I/O 在大量 IO 操作时可能会因为信号队列溢出导致没法通知

```http
异步阻塞：程序进程向内核发送IO调用后，不用等待内核响应，可以继续接受其他请求，内核收到进程请求后进行的IO如果不能立即返回，就由内核等待结果，直到IO完成后内核再通知进程。
```

##### 1.2.2.5 异步 I/O 模型 (asynchronous IO)

异步I/O 与 信号驱动I/O最大区别在于，信号驱动是内核通知用户进程何时开始一个I/O操作，而异步I/O 是由内核通知用户进程I/O操作何时完成，两者有本质区别,相当于不用去饭店场吃饭，直接点个外卖， 把等待上菜的时间也给省了

相对于同步I/O，异步I/O不是顺序执行。用户进程进行aio_read系统调用之后，无论内核数据是否准备 好，都会直接返回给用户进程，然后用户态进程可以去做别的事情。等到socket数据准备好了，内核直 接复制数据给进程，然后从内核向进程发送通知。IO两个阶段，进程都是非阻塞的。

信号驱动IO当内核通知触发信号处理程序时，信号处理程序还需要阻塞在从内核空间缓冲区拷贝数据到 用户空间缓冲区这个阶段，而异步IO直接是在第二个阶段完成后，内核直接通知用户线程可以进行后续操作了

优点：异步 I/O 能够充分利用 DMA 特性，让 I/O 操作与计算重叠

缺点：要实现真正的异步 I/O，操作系统需要做大量的工作。目前 Windows 下通过 IOCP 实现了真正的 异步 I/O，在 Linux 系统下，Linux 2.6才引入，目前 AIO 并不完善，因此在 Linux 下实现高并发网络编 程时以 IO 复用模型模式+多线程任务的架构基本可以满足需求

Linux提供了AIO库函数实现异步，但是用的很少。目前有很多开源的异步IO库，例如libevent、libev、 libuv。

```http
异步非阻塞：程序进程向内核发送IO调用后，不用等待内核响应，可以继续接受其他请求，内核调用的IO如果不能立即返回，内核会继续处理其他事物，直到IO完成后将结果通知给内核，内核在将IO完成的结果返回给进程，期间进程可以接受新的请求，内核也可以处理新的事物，因此相互不影响，可以实现较大的同时并实现较高的IO复用，因此异步非阻塞使用最多的一种通信方式。
```

#### 1.2.3 五种 IO 对比

这五种 I/O 模型中，越往后，阻塞越少，理论上效率也是最优前四种属于同步 I/O，因为其中真正的 I/O  操作(recvfrom)将阻塞进程/线程，只有异步 I/O 模型才与 POSIX 定义的异步 I/O 相匹配

#### 1.2.4 I/O 的具体实现方式

##### 1.2.4.1 I/O常见实现

Nginx支持在多种不同的操作系统实现不同的事件驱动模型，但是其在不同的操作系统甚至是不同的系 统版本上面的实现方式不尽相同，主要有以下实现方式：

```http
1、select：
select库是在linux和windows平台都基本支持的 事件驱动模型库，并且在接口的定义也基本相同，只是部分参数的含义略有差异，最大并发限制1024，是最早期的事件驱动模型。
2、poll：
在Linux 的基本驱动模型，windows不支持此驱动模型，是select的升级版，取消了最大的并发限制，在编译nginx的时候可以使用--with-poll_module和--without-poll_module这两个指定是否编译select库。
3、epoll：
epoll是库是Nginx服务器支持的最高性能的事件驱动库之一，是公认的非常优秀的事件驱动模型，它和select和poll有很大的区别，epoll是poll的升级版，但是与poll有很大的区别.
epoll的处理方式是创建一个待处理的事件列表，然后把这个列表发给内核，返回的时候在去轮询检查这个表，以判断事件是否发生，epoll支持一个进程打开的最大事件描述符的上限是系统可以打开的文件的最大数，同时epoll库的I/O效率不随描述符数目增加而线性下降，因为它只会对内核上报的“活跃”的描述符进行操作。
4、kqueue：
用于支持BSD系列平台的高校事件驱动模型，主要用在FreeBSD 4.1及以上版本、OpenBSD 2.0级以上版本，NetBSD级以上版本及Mac OS X 平台上，该模型也是poll库的变种，因此和epoll没有本质上的区别，都是通过避免轮询操作提供效率。
5、Iocp：
Windows系统上的实现方式，对应第5种（异步I/O）模型。
6、rtsig：
不是一个常用事件驱动，最大队列1024，不是很常用
7、/dev/poll:
用于支持unix衍生平台的高效事件驱动模型，主要在Solaris 平台、HP/UX，该模型是sun公司在开发
Solaris系列平台的时候提出的用于完成事件驱动机制的方案，它使用了虚拟的/dev/poll设备，开发人员将要见识的文件描述符加入这个设备，然后通过ioctl()调用来获取事件通知，因此运行在以上系列平台的时候请使用/dev/poll事件驱动机制。
8、eventport：
该方案也是sun公司在开发Solaris的时候提出的事件驱动库，只是Solaris 10以上的版本，该驱动库看防止内核崩溃等情况的发生。
```

##### 1.2.4.2 常用I/O模型比较

|            | select                                           | poll                                             | epoll                                                        |
| ---------- | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------ |
| 操作方式   | 遍历                                             | 遍历                                             | 回调                                                         |
| 底层实现   | 数组                                             | 链表                                             | 哈希表                                                       |
| IO效率     | 每次调用都进行线性遍历，时间复杂度为O(n)         | 同左                                             | 事件通知方式，每当fd就绪，系统注册的回调函数就会被调用，将就绪的fd放到rd list里，时间复杂度为O(1) |
| 最大连接数 | 1024（X86）                2048（X64）           | 无上限                                           | 无上限                                                       |
| fd拷贝     | 每次调用select都需要把fd集合从用户态拷贝到内核态 | 每次调用poll，都需要把fd集合从用户态拷贝到内核态 | 调用epoll_ctl时拷贝进内核并保存，之后每次epoll_wait不拷贝    |

```http
1、Select：
POSIX所规定，目前几乎在所有的平台上支持，其良好跨平台支持也是它的一个优点，本质上是通过设置或者检查存放fd标志位的数据结构来进行下一步处理
缺点
单个进程能够监视的文件描述符的数量存在最大限制，在Linux上一般为1024，可以通过修改宏定义
FD_SETSIZE，再重新编译内核实现，但是这样也会造成效率的降低
单个进程可监视的fd数量被限制，默认是1024，修改此值需要重新编译内核
对socket是线性扫描，即采用轮询的方法，效率较低
select 采取了内存拷贝方法来实现内核将 FD 消息通知给用户空间，这样一个用来存放大量fd的数据结构，这样会使得用户空间和内核空间在传递该结构时复制开销大
2、poll：
本质上和select没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态
其没有最大连接数的限制，原因是它是基于链表来存储的
大量的fd的数组被整体复制于用户态和内核地址空间之间，而不管这样的复制是不是有意义
poll特点是“水平触发”，如果报告了fd后，没有被处理，那么下次poll时会再次报告该fd 
select是边缘触发即只通知一次
3、epoll：
在Linux 2.6内核中提出的select和poll的增强版本
支持水平触发LT和边缘触发ET，最大的特点在于边缘触发，它只告诉进程哪些fd刚刚变为就需态，并且只会通知一次
使用“事件”的就绪通知方式，通过epoll_ctl注册fd，一旦该fd就绪，内核就会采用类似callback的回调机制来激活该fd，epoll_wait便可以收到通知
优点:
没有最大并发连接的限制：能打开的FD的上限远大于1024(1G的内存能监听约10万个端口)，具体查看/proc/sys/fs/file-max，此值和系统内存大小相关
效率提升：非轮询的方式，不会随着FD数目的增加而效率下降;只有活跃可用的FD才会调用callback函数，
即epoll最大的优点就在于它只管理“活跃”的连接，而跟连接总数无关
内存拷贝，利用mmap(Memory Mapping)加速与内核空间的消息传递;即epoll使用mmap减少复制开销
```

总结：

```http
1、epoll只是一组API，比起select这种扫描全部的文件描述符，epoll只读取就绪的文件描述符，再加入基于事件的就绪通知机制，所以性能比较好
2、基于epoll的事件多路复用减少了进程间切换的次数，使得操作系统少做了相对于用户任务来说的无用功。
3、epoll比select等多路复用方式来说，减少了遍历循环及内存拷贝的工作量，因为活跃连接只占总并发连接的很小一部分。
```

### 1.3 零拷贝

#### 1.3.1 传统 Linux中 I/O 的问题

传统的 Linux 系统的标准 I/O 接口（read、write）是基于数据拷贝的，也就是数据都是 copy_to_user  或者 copy_from_user，这样做的好处是，通过中间缓存的机制，减少磁盘 I/O 的操作，但是坏处也很 明显，大量数据的拷贝，用户态和内核态的频繁切换，会消耗大量的 CPU 资源，严重影响数据传输的性 能，统计表明，在Linux协议栈中，数据包在内核态和用户态之间的拷贝所用的时间甚至占到了数据包整 个处理流程时间的57.1%

#### 1.3.2 什么是零拷贝

零拷贝就是上述问题的一个解决方案，通过尽量避免拷贝操作来缓解 CPU 的压力。零拷贝并没有真正做 到“0”拷贝，它更多是一种思想，很多的零拷贝技术都是基于这个思想去做的优化

#### 1.3.3 零拷页相关技术

*  MMAP ( Memory Mapping )

mmap()系统调用使得进程之间通过映射同一个普通文件实现共享内存。普通文件被映射到进程地址空间后，进程可以向访问普通内存一样对文件进行访问。 

mmap是一种内存映射文件的方法，即将一个文件或者其它对象映射到进程的地址空间，实现文件磁盘地址和进程虚拟地址空间中一段虚拟地址的一一对映关系。 

实现这样的映射关系后，进程就可以采用指针的方式读写操作这一段内存，而系统会自动回写脏页面到 对应的文件磁盘上，即完成了对文件的操作而不必再调用read,write等系统调用函数。相反，内核空间 对这段区域的修改也直接反映用户空间，从而可以实现不同进程间的文件共享。 

内存映射减少数据在用户空间和内核空间之间的拷贝操作,适合大量数据传输

* SENDFILE
* DMA 辅助的 SENDFILE

## 2 Nginx架构和安装

### 2.1 Nginx 概述

#### 2.1.1 Nginx介绍

Nginx官网：http://nginx.org

解决C1000K问题参考链接: http://www.ideawu.net/blog/archives/740.html

nginx的其它的二次发行版： 

* Tengine：由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添 加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到 了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。从2011年12月开 始，Tengine成为一个开源项目

  官网: http://tengine.taobao.org/ 

* OpenResty：基于 Nginx 与 Lua 语言的高性能 Web 平台， 章亦春团队开发，官网：http://openresty.org/cn/

#### 2.1.2 Nginx功能介绍

* 静态的web资源服务器html，图片，js，css，txt等静态资源 
* http/https协议的反向代理 
* 结合FastCGI/uWSGI/SCGI等协议反向代理动态资源请求 
* tcp/udp协议的请求转发（反向代理） 
* imap4/pop3协议的反向代理

#### 2.1.3 基础特性

* 模块化设计，较好的扩展性 
* 高可靠性 
* 支持热部署：不停机更新配置文件，升级版本，更换日志文件 
* 低内存消耗：10000个keep-alive连接模式下的非活动连接，仅需2.5M内存 
* event-driven，aio，mmap，sendfile

#### 2.1.4 Web 服务相关的功能

* 虚拟主机（server） 
* 支持 keep-alive 和管道连接(利用一个连接做多次请求) 
* 访问日志（支持基于日志缓冲提高其性能） 
* url rewirte 
* 路径别名 
* 基于IP及用户的访问控制 
* 支持速率限制及并发数限制 
* 重新配置和在线升级而无须中断客户的工作进程

### 2.2 Nginx 的架构和进程

#### 2.2.1 Nginx 架构

#### 2.2.2 Nginx 进程结构

web请求处理机制 

* 多进程方式：服务器每接收到一个客户端请求就有服务器的主进程生成一个子进程响应客户端，直 到用户关闭连接，这样的优势是处理速度快，子进程之间相互独立，但是如果访问过大会导致服务 器资源耗尽而无法提供请求。 
* 多线程方式：与多进程方式类似，但是每收到一个客户端请求会有服务进程派生出一个线程和此客 户端进行交互，一个线程的开销远远小于一个进程，因此多线程方式在很大程度减轻了web服务器 对系统资源的要求，但是多线程也有自己的缺点，即当多个线程位于同一个进程内工作的时候，可 以相互访问同样的内存地址空间，所以他们相互影响，一旦主进程挂掉则所有子线程都不能工作 了，IIS服务器使用了多线程的方式，需要间隔一段时间就重启一次才能稳定。

**Nginx是多进程组织模型，而且是一个由Master主进程和Worker工作进程组成。**

* 主进程(master process)的功能：

```http
对外接口：接收外部的操作（信号）
对内转发：根据外部的操作的不同，通过信号管理 Worker
监控：监控 worker 进程的运行状态，worker 进程异常终止后，自动重启 worker 进程
读取Nginx 配置文件并验证其有效性和正确性
建立、绑定和关闭socket连接
按照配置生成、管理和结束工作进程
接受外界指令，比如重启、升级及退出服务器等指令
不中断服务，实现平滑升级，重启服务并应用新的配置
开启日志文件，获取文件描述符
不中断服务，实现平滑升级，升级失败进行回滚处理
编译和处理perl脚本
```

* 工作进程（worker process）的功能：

```http
所有 Worker 进程都是平等的
实际处理：网络请求，由 Worker 进程处理
Worker进程数量：一般设置为核心数，充分利用CPU资源，同时避免进程数量过多，导致进程竞争CPU资源，增加上下文切换的损耗
接受处理客户的请求
将请求依次送入各个功能模块进行处理
I/O调用，获取响应数据
与后端服务器通信，接收后端服务器的处理结果
缓存数据，访问缓存索引，查询和调用缓存数据
发送请求结果，响应客户的请求
接收主程序指令，比如重启、升级和退出等
```

#### 2.2.3 Nginx 进程间通信

工作进程是由主进程生成的，主进程使用fork()函数，在Nginx服务器启动过程中主进程根据配置文件决定启动工作进程的数量

然后建立一张全局的工作表用于存放当前未退出的所有的工作进程

主进程生成工作进程后会将新生成的工作进程加入到工作进程表中，并建立一个单向的管道并将其传递给工作进程

该管道与普通的管道不同，它是由主进程指向工作进程的单向通道，包含了主进程向工作进程发出的指令、工作进程ID、工作进程在工作进程表中的索引和必要的文件描述符等信息。

主进程与外界通过信号机制进行通信，当接收到需要处理的信号时，它通过管道向相关的工作进程发送正确的指令，每个工作进程都有能力捕获管道中的可读事件，当管道中有可读事件的时候，工作进程就 会从管道中读取并解析指令，然后采取相应的执行动作，这样就完成了主进程与工作进程的交互。

```http
1、worker进程之间的通信原理基本上和主进程与worker进程之间的通信是一样的，只要worker进程之间能够取得彼此的信息，建立管道即可通信，但是由于worker进程之间是完全隔离的，因此一个进程想要知道另外一个进程的状态信息,就只能通过主进程来实现。
2、为了实现worker进程之间的交互，master进程在生成worker进程之后，在worker进程表中进行遍历，将该新进程的PID以及针对该进程建立的管道句柄传递给worker进程中的其他进程，为worker进程之间的通信做准备，当worker进程1向worker进程2发送指令的时候，首先在master进程给它的其他worker进程工作信息中找到2的进程PID，然后将正确的指令写入指向进程2的管道，worker进程2捕获到管道中的事件后，解析指令并进行相关操作，这样就完成了worker进程之间的通信。
3、另worker进程可以通过共享内存来通讯的，比如upstream中的zone，或者limit_req、limit_conn中的zone等。操作系统提供了共享内存机制
```

#### 2.2.4 Nginx 启动和 HTTP 连接建立

* Nginx 启动时，Master 进程，加载配置文件 
* Master 进程，初始化监听的 socket 
* Master 进程，fork 出多个 Worker 进程 
* Worker 进程，竞争新的连接，获胜方通过三次握手，建立 Socket 连接，并处理请求

#### 2.2.5 HTTP处理过程

### 2.3 Nginx 模块介绍

nginx 有多种模块 

* 核心模块：是 Nginx 服务器正常运行必不可少的模块，提供错误日志记录 、配置文件解析 、事件驱动机制 、进程管理等核心功能 
* 标准HTTP模块：提供 HTTP 协议解析相关的功能，比如： 端口配置 、 网页编码设置 、 HTTP响应 头设置 等等 
* 可选HTTP模块：主要用于扩展标准的 HTTP 功能，让 Nginx 能处理一些特殊的服务，比如： Flash  多媒体传输 、解析 GeoIP 请求、 网络传输压缩 、 安全协议 SSL 支持等 
* 邮件服务模块：主要用于支持 Nginx 的邮件服务 ，包括对 POP3协议、 IMAP 协议和 SMTP协议的支持 
* Stream服务模块: 实现反向代理功能,包括TCP协议代理 
* 第三方模块：是为了扩展 Nginx 服务器应用，完成开发者自定义功能，比如： Json 支持、 Lua 支持等

nginx高度模块化，但其模块早期不支持DSO机制;1.9.11版本支持动态装载和卸载

模块分类：

```bash
核心模块：core module
标准模块：
 HTTP 模块： ngx_http_*
 HTTP Core modules   #默认功能
 HTTP Optional modules #需编译时指定
 Mail 模块: ngx_mail_*
 Stream 模块 ngx_stream_*
第三方模块
```

### 2.4 Nginx 安装

#### 2.4.1 基于yum安装 Nginx

```bash
# https://nginx.org/en/linux_packages.html#RHEL 官网以提供repo配置
[root@10 ~]# cat /etc/yum.repos.d/nginx.repo 
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
[root@10 ~]# yum install epel-release
[root@10 ~]# yum info nginx
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.njupt.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.njupt.edu.cn
 * updates: mirrors.njupt.edu.cn
nginx-stable                                                                                                 | 2.9 kB  00:00:00     
nginx-stable/7/x86_64/primary_db                                                                             |  80 kB  00:00:00     
Available Packages
Name        : nginx
Arch        : x86_64
Epoch       : 1
Version     : 1.22.1
Release     : 1.el7.ngx
Size        : 797 k
Repo        : nginx-stable/7/x86_64
Summary     : High performance web server
URL         : https://nginx.org/
License     : 2-clause BSD-like license
Description : nginx [engine x] is an HTTP and reverse proxy server, as well as
            : a mail proxy server.
[root@10 ~]# yum -y install nginx
[root@10 ~]# nginx -v
nginx version: nginx/1.22.1
[root@10 ~]# rpm -q nginx
nginx-1.22.1-1.el7.ngx.x86_64
[root@10 ~]# rpm -qi nginx
Name        : nginx
Epoch       : 1
Version     : 1.22.1
Release     : 1.el7.ngx
Architecture: x86_64
Install Date: Wed 21 Dec 2022 10:21:28 AM CST
Group       : System Environment/Daemons
Size        : 2917123
License     : 2-clause BSD-like license
Signature   : RSA/SHA256, Wed 19 Oct 2022 06:58:05 PM CST, Key ID abf5bd827bd9bf62
Source RPM  : nginx-1.22.1-1.el7.ngx.src.rpm
Build Date  : Wed 19 Oct 2022 06:48:30 PM CST
Build Host  : ip-10-1-17-124.eu-central-1.compute.internal
Relocations : (not relocatable)
Vendor      : NGINX Packaging <nginx-packaging@f5.com>
URL         : https://nginx.org/
Summary     : High performance web server
Description :
nginx [engine x] is an HTTP and reverse proxy server, as well as
a mail proxy server.
[root@centos8 ~]#rpm -ql nginx

#带有自动日志切割功能
[root@centos8 ~]#cat /etc/logrotate.d/nginx 
/var/log/nginx/*log {
   create 0664 nginx root
   daily
   rotate 10
   missingok
   notifempty
   compress
   sharedscripts
   postrotate
       /bin/kill -USR1 `cat /run/nginx.pid 2>/dev/null` 2>/dev/null || true
   endscript
}

# nginx 程序用法帮助
[root@10 logrotate.d]# nginx -h
nginx version: nginx/1.22.1
Usage: nginx [-?hvVtTq] [-s signal] [-p prefix]
             [-e filename] [-c filename] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit		
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /etc/nginx/)
  -e filename   : set error log file (default: /var/log/nginx/error.log)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
  
# nginx -g "daemon off;"     守护进程关闭，前台执行

[root@10 ~]# systemctl enable --now nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
[root@10 ~]# ps aux |grep nginx
root       4282  0.0  0.0  49060  1164 ?        Ss   10:54   0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
nginx      4283  0.0  0.0  49448  1900 ?        S    10:54   0:00 nginx: worker process
nginx      4284  0.0  0.0  49448  1900 ?        S    10:54   0:00 nginx: worker process
root       4286  0.0  0.0 112808   968 pts/0    S+   10:54   0:00 grep --color=auto nginx

# nginx 验证
[root@10 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@10 ~]# nginx -V
nginx version: nginx/1.22.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'

# nginx 启动文件
[root@10 system]# pwd
/usr/lib/systemd/system
[root@10 system]# cat nginx.service 
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/sh -c "/bin/kill -s HUP $(/bin/cat /var/run/nginx.pid)"
ExecStop=/bin/sh -c "/bin/kill -s TERM $(/bin/cat /var/run/nginx.pid)"

[Install]
WantedBy=multi-user.target
```

#### 2.4.2 Nginx编译安装

* 编译器介绍

```http
源码安装需要提前准备标准的编译器
GCC的全称是（GNU Compiler collection），其有GNU开发，并以GPL即LGPL许可，是自由的类UNIX即苹果电脑Mac OS X操作系统的标准编译器，因为GCC原本只能处理C语言，所以原名为GNU C语言编译器，后来得到快速发展，可以处理C++,Fortranpascal，objective-C，java以及Ada等其他语言，此外还需要Automake工具，以完成自动创建Makefile的工作，Nginx的一些模块需要依赖第三方库，比如:pcre（支持rewrite），zlib（支持gzip模块）和openssl（支持ssl模块）等。
```

* 编译安装Nginx

```bash
[root@10 ~]# yum -y install gcc pcre-devel openssl-devel zlib-devel wget
[root@10 ~]# useradd -s /sbin/nologin nginx
[root@10 ~]# cd /usr/local/src/
[root@10 src]# wget http://nginx.org/download/nginx-1.20.2.tar.gz
--2022-12-21 12:38:25--  http://nginx.org/download/nginx-1.20.2.tar.gz
Resolving nginx.org (nginx.org)... 3.125.197.172, 52.58.199.22, 2a05:d014:edb:5704::6, ...
Connecting to nginx.org (nginx.org)|3.125.197.172|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1062124 (1.0M) [application/octet-stream]
Saving to: ‘nginx-1.20.2.tar.gz’

100%[==========================================================================================>] 1,062,124    617KB/s   in 1.7s   

2022-12-21 12:38:27 (617 KB/s) - ‘nginx-1.20.2.tar.gz’ saved [1062124/1062124]
[root@10 src]# tar xvf nginx-1.20.2.tar.gz
[root@10 src]# cd nginx-1.20.2
[root@10 nginx-1.20.2]# ll
total 792
drwxr-xr-x. 6 nginx nginx   4096 Dec 21 12:39 auto
-rw-r--r--. 1 nginx nginx 312251 Nov 16  2021 CHANGES
-rw-r--r--. 1 nginx nginx 476577 Nov 16  2021 CHANGES.ru
drwxr-xr-x. 2 nginx nginx    168 Dec 21 12:39 conf
-rwxr-xr-x. 1 nginx nginx   2590 Nov 16  2021 configure
drwxr-xr-x. 4 nginx nginx     72 Dec 21 12:39 contrib
drwxr-xr-x. 2 nginx nginx     40 Dec 21 12:39 html
-rw-r--r--. 1 nginx nginx   1397 Nov 16  2021 LICENSE
drwxr-xr-x. 2 nginx nginx     21 Dec 21 12:39 man
-rw-r--r--. 1 nginx nginx     49 Nov 16  2021 README
drwxr-xr-x. 9 nginx nginx     91 Dec 21 12:39 src
[root@10 nginx-1.20.2]# ./configure --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module
[root@10 nginx-1.20.2]# make && make install
[root@10 nginx]# chown -R nginx:nginx /apps/nginx
```

* nginx安装完毕之后，生成四个目录

```bash
[root@10 sbin]# ll /apps/nginx/
total 4
drwxr-xr-x. 2 nginx nginx 4096 Dec 21 12:46 conf
drwxr-xr-x. 2 nginx nginx   40 Dec 21 12:46 html
drwxr-xr-x. 2 nginx nginx    6 Dec 21 12:46 logs
drwxr-xr-x. 2 nginx nginx   19 Dec 21 12:46 sbin

1 conf：保存nginx所有的配置文件，其中nginx.conf是nginx服务器的最核心最主要的配置文件，其他的.conf则是用来配置nginx相关的功能的，例如fastcgi功能使用的是fastcgi.conf和fastcgi_params两个文件，配置文件一般都有一个样板配置文件，是以.default为后缀，使用时可将其复制并将default后缀去掉即可。
2 html：目录中保存了nginx服务器的web文件，但是可以更改为其他目录保存web文件,另外还有一个50x的web文件是默认的错误页面提示页面。
3 logs：用来保存nginx服务器的访问日志错误日志等日志，logs目录可以放在其他路径，比如/var/logs/nginx里面。
4 sbin：保存nginx二进制启动脚本，可以接受不同的参数以实现不同的功能。
```

*  验证版本及编译参数

```bash
[root@10 sbin]# ./nginx -v
nginx version: nginx/1.20.2
[root@10 sbin]# ln -s /apps/nginx/sbin/nginx /usr/sbin/
[root@10 sbin]# nginx -v
nginx version: nginx/1.20.2
# 查看编译参数
[root@10 conf]# nginx -V
nginx version: nginx/1.20.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module
```

* nginx启动和停止

```bash
[root@10 conf]# nginx
[root@10 conf]# ss -ntl
State      Recv-Q Send-Q                     Local Address:Port                                    Peer Address:Port              
LISTEN     0      128                                    *:80                                                 *:*                  
LISTEN     0      128                                    *:22                                                 *:*                  
LISTEN     0      128                                 [::]:22                                              [::]:*                  
[root@10 conf]# nginx -s stop
[root@10 conf]# ss -ntl
State      Recv-Q Send-Q                     Local Address:Port                                    Peer Address:Port              
LISTEN     0      128                                    *:22                                                 *:*                  
LISTEN     0      128                                 [::]:22                                              [::]:*   
```

* Nginx 自启动文件

```bash
# 编辑service文件
[root@10 nginx]# cat /usr/lib/systemd/system/nginx.service 
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/apps/nginx/run/nginx.pid
ExecStart=/apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
ExecReload=/bin/sh -c "/bin/kill -s HUP $(/bin/cat /apps/nginx/run/nginx.pid)"
ExecStop=/bin/sh -c "/bin/kill -s TERM $(/bin/cat /apps/nginx/run/nginx.pid)"

[Install]
WantedBy=multi-user.target
# 创建pid目录
[root@10 nginx]# mkdir -vp /apps/nginx/run/
mkdir: created directory ‘/apps/nginx/run/’
# 修改配置文件
[root@10 nginx]# vim /apps/nginx/conf/nginx.conf
pid        /apps/nginx/run/nginx.pid;

# 验证nginx自启动文件
[root@10 nginx]# systemctl daemon-reload
[root@10 nginx]# systemctl enable --now nginx
[root@10 nginx]# systemctl status nginx.service
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2022-12-21 13:47:12 CST; 2s ago
     Docs: http://nginx.org/en/docs/
  Process: 17356 ExecStart=/apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf (code=exited, status=0/SUCCESS)
 Main PID: 17357 (nginx)
   CGroup: /system.slice/nginx.service
           ├─17357 nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
           └─17358 nginx: worker process

Dec 21 13:47:12 10.0.0.113 systemd[1]: Starting nginx - high performance web server...
Dec 21 13:47:12 10.0.0.113 systemd[1]: Can't open PID file /apps/nginx/run/nginx.pid (yet?) after start: No such file or directory
Dec 21 13:47:12 10.0.0.113 systemd[1]: Started nginx - high performance web server.
[root@10 nginx]# ll /apps/nginx/run/
total 4
-rw-r--r--. 1 root root 6 Dec 21 13:47 nginx.pid
```

#### 2.4.3 安装nginx脚本

```bash
#!/bin/bash
#***********************************************
SRC_DIR=/usr/local/src
NGINX_URL=http://nginx.org/download/
NGINX_VERSION=nginx-1.20.2
TAR=.tar.gz
NGINX_INSTALL_DIR=/apps/nginx

CPUS=`lscpu |awk '/^CPU\(s\)/{print $2}'`

color () {
    RES_COL=60
    MOVE_TO_COL="echo -en \\003[${RES_COL}G"
    SETCOLOR_SUCCESS="echo -en \\033[1;32m"
    SETCOLOR_FAILURE="echo -en \\033[1;31m"
    SETCOLOR_WARNING="echo -en \\033[1;33m"
    SETCOLOR_NORMAL="echo -en \E[0m"
    echo -n "$1" && $MOVE_TO_COLOR
    echo -n "["
    if [ $2 = "success" -o $2 = "0" ] ;then
        ${SETCOLOR_SUCCESS}
        echo -n $" OK "
    elif [ $2 = "failure" - o $2 = "1" ] ;then
        ${SETCOLOR_FAILURE}
        echo -n $"FAILED"
    else 
        ${SETCOLOR_WARNING}
        echo -n $"WARNING"
    fi
    ${SETCOLOR_NORMAL}
    echo -n "]"
    echo
}

os_type () {
    awk -F'[ "]' '/^NAME/{print $2}' /etc/os-release
}

os_version () {
    awk -F '"' '/^VERSION_ID/{print $2}' /etc/os-release
}

check () {
    [ -e ${NGINX_INSTALL_DIR} ] && { color "Nginx already install，please uninstall..." 1; exit; }
    cd ${SRC_DIR}
    if [ -e ${NGINX_VERSION}${TAR} ];then
        color "relevant files are ready..." 0
    else
        color "Start download nginx source package..."
        wget ${NGINX_URL}${NGINX_VERSION}${TAR}
	[ $? -ne 0 ] && { color "download ${NGINX_VERSION}${TAR} failed" 1; exit; }
    fi
}

install () {
    color "Start install nginx..." 0
    if id nginx &> /dev/null;then
        color "User nginx already exists..." 1
    else
        useradd -s /sbin/nologin -r nginx
        color "Create user nginx..." 0
    fi
    
    color "Start intall nginx dependent packages..."
    if [ `os_type` == "CentOS" -a `os_version` == "8" ] ;then
        yum -y -q install make gcc-c++ libtool pcre pcre-devel zlib zlib-devel openssl openssl-devel perl-ExtUtils-Embed
    elif [ `os_type` == "CentOS" -a `os_version` == "7" ] ;then
        yum -y -q install make gcc pcre-devel openssl-devel zlib-devel perl-ExtUtils-Embed
    else
        apt update &> /dev/null
        apt -y install make gcc libpcre3 libpcre3-dev openssl libssl-dev zlib1g-dev &> /dev/null
    fi

    cd ${SRC_DIR}
    tar xf ${NGINX_VERSION}${TAR}
    NGINX_DIR=`echo ${NGINX_VERSION}${TAR} | sed -nr 's/^(.*[0-9]).*/\1/p'`
    cd ${NGINX_DIR}
    ./configure --prefix=${NGINX_INSTALL_DIR} --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module
    make -j $CPUS && make install
    [ $? -eq 0 ] && color "Nginx make install success..." 0 || { color "Nginx make install failed,exit..." 1; exit; }
    echo "PATH=${NGINX_INSTALL_DIR}/sbin:${PATH}" > /etc/profile.d/nginx.sh
    cat > /usr/lib/systemd/system/nginx.service <<EOF
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target
[Service]
Type=forking
PIDFile=${NGINX_INSTALL_DIR}/logs/nginx.pid
ExecStartPre=/bin/rm -f ${NGINX_INSTALL_DIR}/logs/nginx.pid
ExecStartPre=${NGINX_INSTALL_DIR}/sbin/nginx -t
ExecStart=${NGINX_INSTALL_DIR}/sbin/nginx
ExecReload=/bin/kill -s HUP \$MAINPID
KillSignal=SIGQUIT
LimitNOFILE=100000
TimeoutStopSec=5
KillMode=process
PrivateTmp=true
[Install]
WantedBy=multi-user.target
EOF
    systemctl daemon-reload
    systemctl enable --now nginx
    systemctl is-active nginx &> /dev/null || { color "Nginx start failed,exit!" 1; exit; }
    color "Nginx install complete..." 0
}

check
install
```

#### 2.4.4 playbook一键安装nginx

```yaml
---
- hosts: webservers
  remote_user: root
  gather_facts: yes
  vars:
    nginx_verison: nginx-1.18.0
    suffix: .tar.gz
    down_dir: /usr/local/src/
    ins_dir: /apps/nginx/
    user: nginx
    group: nginx
  tasks:
    - name: install packages on centos8
      yum:
        name:
          - gcc
          - make
          - pcre-devel
          - openssl-devel
          - zlib-devel
        state: present
      when: 
        - ansible_facts['distribution'] == "CentOS"
        - ansible_facts['distribution_major_version'] == "8"
    - name: install packages on centos7
      yum:
        name:
          - gcc
          - make
          - pcre-devel
          - openssl-devel
          - zlib-devel
          - perl-ExtUtils-Embed
        state: present
      when: 
        - ansible_facts['distribution'] == "CentOS"
        - ansible_facts['distribution_major_version'] == "7"
    - name: install packages on ubuntu18.04
      apt:
        name:
          - gcc
          - make
          - libpcre3
          - libpcre3-dev
          - openssl
          - libssl-dev
          - zlib1g-dev
        state: present
      when:
        - ansible_facts['distribution'] == "Ubuntu"
        - ansible_facts['distribution_major_version'] == "18"
    - name: group "{{ group }}"
      group: name={{ group }} state=present system=yes
    - name: user "{{ user }}"
      user:
        name: "{{ user }}"
        shell: /sbin/nologin
        system: yes
        group: "{{ group }}"
        home: "/home/{{ user }}"
        create_home: no
    - name: unarchive
      unarchive:
        src: "http://nginx.org/download/{{ nginx_verison }}{{ suffix }}"
        dest: "{{ down_dir }}"
        remote_src: yes
    - name: configure
      shell: ./configure \
                --prefix={{ ins_dir }} \
                --user={{ user }} --group={{ group }} \
                --with-http_ssl_module \
                --with-http_realip_module \
                --with-http_v2_module \
                --with-http_stub_status_module \
                --with-http_gzip_static_module \
                --with-pcre \
                --with-stream \
                --with-stream_ssl_module \
                --with-stream_realip_module
      args:
        chdir: "{{ down_dir }}{{ nginx_verison }}/"
    - name: make
      shell: make -j "{{ ansible_processor_vcpus }}" && make install
      args:
        chdir: "{{ down_dir }}{{ nginx_verison }}/"
    - name: link
      file:
        src: "{{ ins_dir }}sbin/nginx"
        dest: /usr/sbin/nginx
        state: link
    - name: service file
      copy: content='[Unit]\nDescription=nginx - high performance web server\nDocumentation=http://nginx.org/en/docs/\nAfter=network-online.target remote-fs.target nss-lookup.target\nWants=network-online.target\n\n[Service]\nType=forking\nPIDFile={{ ins_dir }}run/nginx.pid\nExecStart=/usr/sbin/nginx -c {{ ins_dir }}conf//nginx.conf\nExecReload=/bin/sh -c "/bin/kill -s HUP $(/bin/cat {{ ins_dir }}run/nginx.pid)"\nExecStop=/bin/sh -c "/bin/kill -s TERM $(/bin/cat {{ ins_dir }}run/nginx.pid)"\nLimitNOFILE=100000\n[Install]\nWantedBy=multi-user.target' dest=/lib/systemd/system/nginx.service
    - name: change configure pid location and change worker_processes
      lineinfile:
        path: "{{ item.path }}"
        regexp: "{{ item.regexp }}"
        line: "{{ item.line }}"
      with_items:
        - { path: "{{ ins_dir }}conf/nginx.conf",regexp: '^worker_processes',line: "worker_processes {{ ansible_processor_vcpus }};" }
        - { path: "{{ ins_dir }}conf/nginx.conf",regexp: '^#pid',line: 'pid     run/nginx.pid;' }
    - name: create dir run and modify directory permission
      file: 
        path: "{{ ins_dir }}run"
        owner: "{{ user }}"
        group: "{{ group }}"
        state: directory
    - name: start serivce
      service: 
        name: nginx
        state: started
        enabled: yes
```

### 2.5 Nginx 命令和信号

#### 2.5.1 Nginx命令

```http
nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]
```

选项说明：

```http
帮助: -? -h
使用指定的配置文件: -c
指定配置指令:-g
指定运行目录:-p
测试配置文件是否有语法错误:-t -T
打印nginx的版本信息、编译信息等:-v -V
发送信号: -s 示例: nginx -s reload
```

信号说明：

```http
立刻停止服务:stop,相当于信号SIGTERM,SIGINT
优雅的停止服务:quit,相当于信号SIGQUIT
平滑重启，重新加载配置文件: reload,相当于信号SIGHUP
重新开始记录日志文件:reopen,相当于信号SIGUSR1,在切割日志时用途较大
平滑升级可执行程序:发送信号SIGUSR2,在升级版本时使用
优雅的停止工作进程:发送信号SIGWINCH,在升级版本时使用
```

#### 2.5.2 quit实现worker进程关闭

* 设置定时器: worker_shutdown_timeout
* 关闭监听句柄
* 关闭空闲连接
* 在循环中等待全部连接关闭
* 退出Nginx所以进程

```bash
# 验证
[root@localhost html]# pwd
/apps/nginx/html
[root@localhost html]# dd if=/dev/zero of=test.img bs=1M count=2048			#dd命令创建大文件test.img
2048+0 records in
2048+0 records out
2147483648 bytes (2.1 GB) copied, 3.15316 s, 681 MB/s
[root@localhost html]# ll
total 2097160
-rw-r--r--. 1 nginx nginx        494 Dec 14 11:39 50x.html
-rw-r--r--. 1 nginx nginx        612 Dec 14 11:39 index.html
-rw-r--r--. 1 root  root  2147483648 Dec 14 16:43 test.img
[root@localhost html]# ps -aux |grep nginx
root      32689  0.0  0.0 118704  1092 pts/0    S+   16:23   0:00 man man/nginx.8
root      32724  0.0  0.1  46352  2028 ?        Ss   16:24   0:00 nginx: master process nginx
nginx     32731  0.0  0.1  46804  2044 ?        S    16:26   0:00 nginx: worker process
nginx     32732  0.0  0.1  46804  2044 ?        S    16:26   0:00 nginx: worker process
nginx     32733  0.0  0.1  46804  2280 ?        S    16:26   0:00 nginx: worker process
nginx     32734  0.0  0.1  46804  2044 ?        S    16:26   0:00 nginx: worker process
nginx     32735  0.0  0.1  46804  2044 ?        S    16:26   0:00 nginx: worker process
nginx     32736  0.0  0.1  46804  2280 ?        S    16:26   0:00 nginx: worker process
root      32867  0.0  0.0 112812   976 pts/1    S+   16:46   0:00 grep --color=auto nginx
[root@lzy-ng2 ~]# wget http://10.0.0.109/test.img
--2022-12-14 16:48:10--  http://10.0.0.109/test.img
Connecting to 10.0.0.109:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2147483648 (2.0G) [application/octet-stream]
Saving to: ‘test.img’

100%[==============================================================================================================>] 2,147,483,648  384MB/s   in 5.2s   

2022-12-14 16:48:16 (397 MB/s) - ‘test.img’ saved [2147483648/2147483648]
[root@localhost html]# nginx -s quit					#wget 下载时quit，会等待进程工作结束后关闭
[root@localhost html]# ps -aux |grep nginx
root      32689  0.0  0.0 118704  1048 pts/0    S+   16:23   0:00 man man/nginx.8
root      32724  0.0  0.1  46352  2016 ?        Ss   16:24   0:00 nginx: master process nginx
nginx     32731  0.1  0.1  46804  2268 ?        R    16:26   0:02 nginx: worker process is shutting down
root      32956  0.0  0.0 112812   980 pts/1    S+   16:48   0:00 grep --color=auto nginx
[root@localhost html]# ps -aux |grep nginx
root      32689  0.0  0.0 118704   828 pts/0    S+   16:23   0:00 man man/nginx.8
root      32958  0.0  0.0 112812   980 pts/1    S+   16:48   0:00 grep --color=auto nginx

[root@localhost html]# nginx -s stop
[root@localhost html]# ps -aux |grep nginx
root      32689  0.0  0.0 118704   828 pts/0    S+   16:23   0:00 man man/nginx.8
root      32980  0.0  0.0 112812   976 pts/1    S+   16:50   0:00 grep --color=auto nginx
[root@lzy-ng2 ~]# wget http://10.0.0.109/test.img					# 使用stop，连接进程会直接关闭
--2022-12-14 16:50:51--  http://10.0.0.109/test.img
Connecting to 10.0.0.109:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2147483648 (2.0G) [application/octet-stream]
Saving to: ‘test.img’
41% [==============================================>                               ] 901,812,981  471MB/s   in 1.8s 
2022-12-14 16:50:53 (471 MB/s) - Connection closed at byte 901812981. Retrying.
--2022-12-14 16:50:54--  (try: 2)  http://10.0.0.109/test.img
Connecting to 10.0.0.109:80... failed: Connection refused.
```

#### 2.5.3 reload 流程

利用 reload 可以实现平滑修改配置并生效

* 向master进程发送HUP信号(reload命令)
* master进程校验配置语法是否正确
* master进程打开新的监听端口
* master进程用新配置启动新的worker子进程
* master进程向老worker子进程发送QUIT信号，老的worker对已建立连接继续处理，处理完才会优 雅退出未关闭的worker旧进程不会处理新来的请求
* 老worker进程关闭监听句柄，处理完当前连接后结束进程

### 2.6 平滑升级和回滚

* 平滑升级流程
  * 将旧Nginx二进制文件换成新Nginx程序文件（注意先备份)
  * 向master进程发送USR2信号
  * master进程修改pid文件名加上后缀.oldbin,成为nginx.pid.oldbin
  * master进程用新Nginx文件启动新master进程成为旧master的子进程,系统中将有新旧两个Nginx 主进程共同提供Web服务,当前新的请求仍然由旧Nginx的worker进程进行处理,将新生成的master 进程的PID存放至新生成的pid文件nginx.pid
  * 向旧的Nginx服务进程发送WINCH信号，使旧的Nginx worker进程平滑停止
  * 向旧master进程发送QUIT信号，关闭老master，并删除Nginx.pid.oldbin文件
  * 如果发现升级有问题,可以回滚∶向老master发送HUP，向新master发送QUIT
* 平滑升级和回滚案例

```bash
#下载最新稳定版
[root@centos8 ~]#wget http://nginx.org/download/nginx-1.20.1.tar.gz
[root@centos8 ~]#tar xvf nginx-1.20.1.tar.gz
[root@centos8 ~]#cd nginx-1.20.1

#查看当前使用的版本及编译选项。结果如下：
[root@centos8 nginx-1.20.1]#/apps/nginx/sbin/nginx -V   
nginx version: nginx/1.18.0
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC) 
built with OpenSSL 1.1.1g FIPS  21 Apr 2020
TLS SNI support enabled
configure arguments: --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream
--with-stream_ssl_module --with-stream_realip_module
#configure arguments后面是以前编译时的参数。现在编译使用一样的参数

#开始编译新版本
[root@centos8 nginx-1.20.1]#./configure --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module

#只要make无需要make install
[root@centos8 nginx-1.20.1]#make
[root@centos8 nginx-1.20.1]#objs/nginx -v
nginx version: nginx/1.20.1

#查看两个版本
[root@centos8 nginx-1.20.1]#ll objs/nginx /apps/nginx/sbin/nginx
-rwxr-xr-x 1 nginx nginx 7591096 Jun  7 16:28 /apps/nginx/sbin/nginx
-rwxr-xr-x 1 root root  7723272 Jun  7 17:27 objs/nginx

#把之前的旧版的nginx命令备份
[root@centos8 nginx-1.20.1]#mv /apps/nginx/sbin/nginx 
/usr/local/nginx/sbin/nginx.old

#把新版本的nginx命令复制过去
[root@centos8 nginx-1.20.1]#cp ./objs/nginx /apps/nginx/sbin/

#检测一下有没有问题
[root@centos8 nginx-1.20.1]#/apps/nginx/sbin/nginx -t

#USR2 平滑升级可执行程序,将存储有旧版本主进程PID的文件重命名为nginx.pid.oldbin，并启动新的nginx
#此时两个master的进程都在运行,只是旧的master不在监听,由新的master监听80
#此时Nginx开启一个新的master进程，这个master进程会生成新的worker进程，这就是升级后的Nginx进程，此时老的进程不会自动退出，但是当接收到新的请求不作处理而是交给新的进程处理。

#前提就是你在启动nginx时使用的是nginx二进制文件的绝对路径，而不是直接在命令行中输入”nginx”的方式启动的nginx服务，不通过绝对路径启动的方式通常是为了方便，配置了nginx相关的环境变量，如果没有通过绝对路径启动nginx，那么当你向nginx进程发送更新的信号时，nginx进程可能会无法找到新的二进制程序（由于没有找新版本的二进制程序，所以没有任何反应），有可能会在更新时，在日志中发现如下错误：
2022/12/14 17:53:05 [alert] 36292#0: execve() failed while executing new binary process "nginx" (2: No such file or directory)

[root@centos8 nginx-1.20.1]#kill -USR2 `cat /apps/nginx/run/nginx.pid`		

#可以看到两个master,新的master是旧版master的子进程,并生成新版的worker进程
[root@lzy-ng1 nginx-1.22.1]# ps -auxf |grep nginx
root      36323  0.0  0.0 112812   980 pts/1    S+   18:00   0:00          \_ grep --color=auto nginx
root      36310  0.0  0.0  46224  1344 ?        Ss   17:59   0:00 nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
nginx     36311  0.0  0.1  46600  1932 ?        S    17:59   0:00  \_ nginx: worker process
nginx     36312  0.0  0.1  46600  1932 ?        S    17:59   0:00  \_ nginx: worker process
root      36319  0.0  0.1  46228  3376 ?        S    18:00   0:00  \_ nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
nginx     36320  0.0  0.1  46680  1916 ?        S    18:00   0:00      \_ nginx: worker process
nginx     36321  0.0  0.1  46680  1916 ?        S    18:00   0:00      \_ nginx: worker process
[root@lzy-ng1 nginx-1.22.1]# ll /apps/nginx/run/
total 8
-rw-r--r--. 1 root root 6 Dec 14 18:00 nginx.pid
-rw-r--r--. 1 root root 6 Dec 14 17:59 nginx.pid.oldbin
[root@lzy-ng1 nginx-1.22.1]# cat /apps/nginx/run/nginx.pid			#新的master进程pid
36319
[root@lzy-ng1 nginx-1.22.1]# cat /apps/nginx/run/nginx.pid.oldbin    #旧的master进程pid
36310

[root@lzy-ng1 nginx-1.22.1]# lsof -i:80					#通过wget在lzy-ng2主机下载ng1的文件，pid36321建立工作连接
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   36310  root    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36311 nginx    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36312 nginx    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36319  root    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36320 nginx    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36321 nginx    6u  IPv4  77390      0t0  TCP *:http (LISTEN)
nginx   36321 nginx    7u  IPv4  78677      0t0  TCP lzy-ng1:http->lzy-ng1:55804 (ESTABLISHED)
wget    36371  root    3u  IPv4  78676      0t0  TCP lzy-ng1:55804->lzy-ng1:http (ESTABLISHED)

#先关闭旧nginx的worker进程,而不关闭nginx主进程方便回滚
#向原Nginx主进程发送WINCH信号，它会逐步关闭旗下的工作进程（主进程不退出），这时所有请求都会由新版Nginx处理
[root@centos8 nginx-1.20.1]#kill -WINCH `cat /apps/nginx/run/nginx.pid.oldbin`

#如果旧版worker进程有用户的请求,会一直等待处理完后才会关闭
[root@lzy-ng1 nginx-1.22.1]# ps -auxf |grep nginx
root      36290  0.0  0.0 108096   616 pts/0    S+   17:52   0:00  |       \_ tail -200f /apps/nginx/logs/error.log
root      36401  0.0  0.0 112812   980 pts/1    S+   18:16   0:00          \_ grep --color=auto nginx
root      36310  0.0  0.0  46224  1344 ?        Ss   17:59   0:00 nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
root      36319  0.0  0.1  46228  3376 ?        S    18:00   0:00  \_ nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
nginx     36320  0.0  0.1  46680  1916 ?        S    18:00   0:00      \_ nginx: worker process
nginx     36321  0.0  0.1  46680  2168 ?        S    18:00   0:00      \_ nginx: worker process


[root@lzy-ng1 nginx-1.22.1]# pstree -lnp |grep nginx
           `-nginx(36310)---nginx(36319)-+-nginx(36320)
                                         `-nginx(36321)

#经过一段时间测试，新版本服务没问题，最后退出老的master
[root@centos8 nginx-1.20.1]#kill -QUIT `cat /apps/nginx/run/nginx.pid.oldbin` 

#查看版本是不是已经是新版了
[root@centos8 nginx-1.20.1]#nginx -v
nginx version: nginx/1.20.1
[root@centos8 nginx-1.20.1]#curl -I 127.0.0.1
HTTP/1.1 200 OK
Server: nginx/1.20.1
Date: Mon, 07 Jun 2021 09:48:48 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Mon, 07 Jun 2021 08:28:12 GMT
Connection: keep-alive
ETag: "60bdd89c-264"
Accept-Ranges: bytes

#回滚
#如果升级的版本发现问题需要回滚,可以重新拉起旧版本的worker
[root@centos8 nginx-1.20.1]#kill -HUP `cat /apps/nginx/run/nginx.pid.oldbin`
[root@lzy-ng1 nginx-1.22.1]# ps -auxf |grep nginx
root      36290  0.0  0.0 108096   616 pts/0    S+   17:52   0:00  |       \_ tail -200f /apps/nginx/logs/error.log
root      36415  0.0  0.0 112812   976 pts/1    S+   18:20   0:00          \_ grep --color=auto nginx
root      36310  0.0  0.0  46224  1344 ?        Ss   17:59   0:00 nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
root      36319  0.0  0.1  46228  3376 ?        S    18:00   0:00  \_ nginx: master process /apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
nginx     36320  0.0  0.1  46680  1916 ?        S    18:00   0:00  |   \_ nginx: worker process
nginx     36321  0.0  0.1  46680  2168 ?        S    18:00   0:00  |   \_ nginx: worker process
nginx     36412  0.0  0.1  46600  1924 ?        S    18:20   0:00  \_ nginx: worker process
nginx     36413  0.0  0.1  46600  1928 ?        S    18:20   0:00  \_ nginx: worker process
[root@centos8 nginx-1.20.1]#pstree -p |grep nginx
           |-nginx(8814)-+-nginx(12014)-+-nginx(12015)
           |             |              `-nginx(12016)
           |             |-nginx(12090)
           |             `-nginx(12091)
           
#最后关闭新版的master 
[root@centos8 nginx-1.20.1]#kill -QUIT `cat /apps/nginx/run/nginx.pid`

#相关博客
https://www.zsythink.net/archives/3260
```

## 3 Nginx配置

### 3.1 配置文件说明

Nginx 官方帮助文档

```http
http://nginx.org/en/docs/
```

Nginx的配置文件的组成部分： 

* 主配置文件：nginx.conf 
* 子配置文件: include conf.d/*.conf 
* fastcgi， uwsgi，scgi 等协议相关的配置文件 
* mime.types：支持的mime类型，MIME(Multipurpose Internet Mail Extensions)多用途互联网邮 件扩展类型，MIME消息能包含文本、图像、音频、视频以及其他应用程序专用的数据，是设定某 种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动 使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。
*  MIME参考文档：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types

nginx 配置文件格式说明:

```http
配置文件由指令与指令块构成
每条指令以;分号结尾，指令与值之间以空格符号分隔
可以将多条指令放在同一行,用分号分隔即可,但可读性差,不推荐
指令块以{ }大括号将多条指令组织在一起,且可以嵌套指令块
include语句允许组合多个配置文件以提升可维护性
使用#符号添加注释，提高可读性
使用$符号使用变量
部分指令的参数支持正则表达式
```

Nginx 主配置文件的配置指令方式：

```
directive value [value2 ...];
注意
(1) 指令必须以分号结尾
(2) 支持使用配置变量
 内建变量：由Nginx模块引入，可直接引用
 自定义变量：由用户使用set命令定义,格式: set variable_name value;
 引用变量：$variable_name
```

主配置文件结构：四部分

```bash
main block：主配置段，即全局配置段，对http,mail都有效
#事件驱动相关的配置
event {
 ...
}   
#http/https 协议相关配置段
http {
 ...
}          
#默认配置文件不包括下面两个块
#mail 协议相关配置段
mail {
 ...
}    
#stream 服务器相关配置段
stream {
 ...
} 
```

默认的nginx.conf 配置文件格式说明:

```bash
#全局配置端，对全局生效，主要设置nginx的启动用户/组，启动的工作进程数量，工作模式，Nginx的PID路径，日志路径等。
user nginx nginx;
worker_processes  1;   #启动工作进程数数量
#events设置快，主要影响nginx服务器与用户的网络连接，比如是否允许同时接受多个网络连接，使用哪种事件驱动模型处理请求，每个工作进程可以同时支持的最大连接数，是否开启对多工作进程下的网络连接进行序列化等。
events { 
     worker_connections  1024;   #设置单个nginx工作进程可以接受的最大并发，作为web服务器的时候最大并发数为worker_connections * worker_processes，作为反向代理的时候为(worker_connections * worker_processes)/2
}
#http块是Nginx服务器配置中的重要部分，缓存、代理和日志格式定义等绝大多数功能和第三方模块都可以在这设置，http块可以包含多个server块，而一个server块中又可以包含多个location块，server块可以配置文件引入、MIME-Type定义、日志自定义、是否启用sendfile、连接超时时间和单个链接的请求上限等。
http { 
   include       mime.types;
   default_type application/octet-stream;
   sendfile       on; #作为web服务器的时候打开sendfile加快静态文件传输，指定是否使用sendfile系统调用来传输文件,sendfile系统调用在两个文件描述符之间直接传递数据(完全在内核中操作)，从而避免了数据在内核缓冲区和用户缓冲区之间的拷贝，操作效率很高，被称之为零拷贝，硬盘 >> kernel buffer (快速拷贝到kernelsocket buffer) >>协议栈。
   keepalive_timeout  65;  #长连接超时时间，单位是秒
   #设置一个虚拟机主机，可以包含自己的全局快，同时也可以包含多个location模块。比如本虚拟机监听的端口、本虚拟机的名称和IP配置，多个server 可以使用一个端口，比如都使用80端口提供web服务
   server {
       listen       80;  			#配置server监听的端口
       server_name localhost; 	#本server的名称，当访问此名称的时候nginx会调用当前serevr内部的配置进程匹配。
       #location其实是server的一个指令，为nginx服务器提供比较多而且灵活的指令，都是在location中体现的，主要是基于nginx接受到的请求字符串，对用户请求的UIL进行匹配，并对特定的指令进行处理，包括地址重定向、数据缓存和应答控制等功能都是在这部分实现，另外很多第三方模块的配置也是在location模块中配置。
       location / { 
           root   html; #相当于默认页面的目录名称，默认是安装目录的相对路径，可以使用绝对路径配置。
           index index.html index.htm; #默认的页面文件名称
       }
       error_page   500 502 503 504 /50x.html; #错误页面的文件名称
	   #location处理对应的不同错误码的页面定义到/50x.html，这个跟对应其server中定义的目录下。
       location = /50x.html { 
           root   html;  #定义默认页面所在的目录
       }
   }
    
#和邮件相关的配置
mail {
               ...
       }         mail 协议相关配置段
#tcp代理配置，1.9版本以上支持
stream {
               ...
       }       stream 服务器相关配置段
#导入其他路径的配置文件
include /apps/nginx/conf.d/*.conf
}
```

### 3.2 全局配置

Main 全局配置段常见的配置指令分类

* 正常运行必备的配置
* 优化性能相关的配置
* 用于调试及定位问题相关的配置
* 事件驱动相关的配置

**全局配置说明**：

```bash
user nginx nginx; #启动Nginx工作进程的用户和组
worker_processes [number | auto]; #启动Nginx工作进程的数量,一般设为和CPU核心数相同
#ab工具，模拟请求调用的工具
[root@lzy-ng2 ~]# while : ;do ab -c 1000 -n 10000 http://10.0.0.113/; sleep 1;done

worker_cpu_affinity 00000001 00000010 00000100 00001000 | auto ; #将Nginx工作进程绑定到指定的CPU核心，默认Nginx是不进行进程绑定的，绑定并不是意味着当前nginx进程独占以一核心CPU，但是可以保证此进程不会运行在其他核心上，这就极大减少了nginx的工作进程在不同的cpu核心上的来回跳转，减少了CPU对进程的资源分配与回收以及内存管理等，因此可以有效的提升nginx服务器的性能。
CPU MASK: 00000001：0号CPU
          00000010：1号CPU
		 10000000：7号CPU
#示例:
worker_cpu_affinity 0001 0010 0100 1000;第0号---第3号CPU
worker_cpu_affinity 0101 1010; 
#示例
worker_processes  4;
worker_cpu_affinity 00000010 00001000 00100000 10000000;

#auto 绑定CPU
#The special value auto (1.9.10) allows binding worker processes automatically to 
available CPUs:
worker_processes auto; 
worker_cpu_affinity auto;
#The optional mask parameter can be used to limit the CPUs available for 
automatic binding:
worker_cpu_affinity auto 01010101;
#错误日志记录配置，语法：error_log file [debug | info | notice | warn | error | crit | alert | emerg]
#error_log logs/error.log;
#error_log logs/error.log notice;
error_log /apps/nginx/logs/error.log error; 
#pid文件保存路径
pid       /apps/nginx/logs/nginx.pid;
worker_priority 0; 			#工作进程优先级，-20~19
[root@centos8 ~]# watch -n1 'ps -axo pid,cmd,nice | grep nginx' #验证进程优先级

worker_rlimit_nofile 65536;  #所有worker进程能打开的文件数量上限,包括:Nginx的所有连接（例如与代理服务器的连接等），而不仅仅是与客户端的连接,另一个考虑因素是实际的并发连接数不能超过系统级别的最大打开文件数的限制.最好与ulimit -n 或者limits.conf的值保持一致,
#配合修改pam限制
[root@centos8 ~]#cat /etc/security/limits.conf
*               soft   nofile          1000000
*               hard   nofile          1000000
daemon off;  #前台运行Nginx服务用于测试、docker等环境。
master_process off|on; #是否开启Nginx的master-worker工作模式，仅用于开发调试场景,默认为on
events {
    worker_connections  65536;  #设置单个工作进程的最大并发连接数
    use epoll; #使用epoll事件驱动，Nginx支持众多的事件驱动，比如:select、poll、epoll，只能设置在events模块中设置。
    accept_mutex on; #on为同一时刻一个请求轮流由work进程处理,而防止被同时唤醒所有worker,避免多个睡眠进程被唤醒的设置，默认为off，新请求会唤醒所有worker进程,此过程也称为"惊群"，因此nginx刚安装完以后要进行适当的优化。建议设置为on
    multi_accept on; #on时Nginx服务器的每个工作进程可以同时接受多个新的网络连接，此指令默认为off，即默认为一个工作进程只能一次接受一个新的网络连接，打开后几个同时接受多个。建议设置为on
}
```

### 3.3 http 配置块

http 协议相关的配置结构

```bash
http {
	...
	...  		#各server的公共配置
	server {    #每个server用于定义一个虚拟主机,第一个server为默认虚拟服务器
		...
	}
	server {     
		...
		server_name   	#虚拟主机名
		root     		#主目录
		alias     		#路径别名
		location [OPERATOR] URL {     #指定URL的特性
			...
			if CONDITION {
				...
			}
		}
	}
}
```

http 协议配置说明

```bash
http {
	include       mime.types; #导入支持的文件类型,是相对于/apps/nginx/conf的目录
	default_type application/octet-stream; #除mime.types中文件类型外,设置其它文件默认类型，访问其它类型时会提示下载不匹配的类型文件
#日志配置部分
    #log_format main '$remote_addr - $remote_user [$time_local] "$request" '
    #                 '$status $body_bytes_sent "$http_referer" '
    #                 '"$http_user_agent" "$http_x_forwarded_for"';
    #access_log logs/access.log main;
#自定义优化参数
   	sendfile       on; 
    #tcp_nopush     on; #在开启了sendfile的情况下，合并请求后统一发送给客户端,必须开启sendfile
    #tcp_nodelay   off; #在开启了keepalived模式下的连接是否启用TCP_NODELAY选项，当为off时，延迟0.2s发送，默认On时，不延迟发送，立即发送用户响应报文。
    #keepalive_timeout 0;
    keepalive_timeout  65 65; #设置会话保持时间,第二个值为响应首部:keep-Alived:timeout=65,可以和第一个值不同
    #gzip on; #开启文件压缩
    server {
        listen       80; 		#设置监听地址和端口
        server_name localhost; 	#设置server name，可以以空格隔开写多个并支持正则表达式，如:*.magedu.com www.magedu.* ~^www\d+\.magedu\.com$ default_server 
        #charset koi8-r; #设置编码格式，默认是俄语格式，建议改为utf-8
        #access_log logs/host.access.log main;
        location / {
           root   html;						#网页存放路径，编译安装的相对路径，此处表示/apps/nginx/html
           index index.html index.htm;
        }
        #error_page 404             /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504 /50x.html; #定义错误页面
        location = /50x.html {
           root   html;
        }
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ { #以http的方式转发php请求到指定web服务器
        #   proxy_pass   http://127.0.0.1;
        #}
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ { #以fastcgi的方式转发php请求到php处理
        #   root           html;
        #   fastcgi_pass   127.0.0.1:9000;
        #   fastcgi_index index.php;
        #   fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name;
        #   include       fastcgi_params;
        #}
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #拒绝web形式访问指定文件，如很多的网站都是通过.htaccess文件来改变自己的重定向等功能。
        #location ~ /\.ht { 
        #   deny all;
        #}
        location ~ /passwd.html {
            deny all;
       }
    }
    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server { 								#自定义虚拟server
    #   listen       8000;
    #   listen       somename:8080;
    #   server_name somename alias another.alias;
    #   location / { 
    #       root   html;
    #       index index.html index.htm; 	#指定默认网页文件，此指令由ngx_http_index_module模块提供
    #   }
    #}
    # HTTPS server
    #
    #server { 								#https服务器配置
    #   listen       443 ssl;
    #   server_name localhost;
    #   ssl_certificate     cert.pem;
    #   ssl_certificate_key cert.key;
    #   ssl_session_cache   shared:SSL:1m;
    #   ssl_session_timeout 5m;
    #   ssl_ciphers HIGH:!aNULL:!MD5;
    #   ssl_prefer_server_ciphers on;
    #   location / {
    #       root   html;
    #       index index.html index.htm;
    #   }
    #}
```

#### 3.3.1 MIME

```bash
#在响应报文中将指定的文件扩展名映射至MIME对应的类型
include           /etc/nginx/mime.types;
default_type     application/octet-stream;     #除mime.types中的类型外，指定其它文件的默认MIME类型，浏览器一般会提示下载
types {
    text/html html;
    image/gif gif;
    image/jpeg jpg;
}
#MIME参考文档：
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types
```

#### 3.3.2 指定响应报文server首部

```bash
#是否在响应报文中的Content-Type显示指定的字符集，默认off不显示
charset charset | off;
#示例
charset utf-8;
#是否在响应报文的Server首部显示nginx版本
server_tokens on | off | build | string;
```

范例: 修改server字段

```bash
如果想自定义响应报文的nginx版本信息，需要修改源码文件，重新编译
如果server_tokens on，修改 src/core/nginx.h 修改第13-14行，如下示例
#define NGINX_VERSION     "1.68.9"
#define NGINX_VER         "wanginx/" NGINX_VERSION
如果server_tokens off，修改 src/http/ngx_http_header_filter_module.c 第49行，如下示例：
static char ngx_http_server_string[] = "Server: nginx" CRLF;
把其中的nginx改为自己想要的文字即可,如：wanginx

#范例: 修改 Server 头部信息
[root@centos8 ~]#vim /usr/local/src/nginx-1.18.0/src/core/nginx.h 
#define NGINX_VERSION     "1.68.9"
#define NGINX_VER         "wanginx/" NGINX_VERSION  
[root@centos8 ~]#vim nginx-1.18.0/src/http/ngx_http_header_filter_module.c
static u_char ngx_http_server_string[] = "Server: magedu-nginx" CRLF; 
[root@centos7 ~]#curl -I www.magedu.org
HTTP/1.1 200 OK
Server: wanginx/1.68.9
Date: Thu, 24 Sep 2020 07:44:05 GMT
Content-Type: text/html
Content-Length: 8
Last-Modified: Wed, 23 Sep 2020 14:39:21 GMT
Connection: keep-alive
ETag: "5f6b5e19-8"
Accept-Ranges: bytes
[root@centos8 ~]#vim /apps/nginx/conf/conf.d/pc.conf 
server {
     ......
     server_tokens off; 
     ......
     
[root@centos7 ~]#curl -I www.magedu.org
HTTP/1.1 200 OK
Server: magedu-nginx
Date: Thu, 24 Sep 2020 07:44:59 GMT
Content-Type: text/html
Content-Length: 8
Last-Modified: Wed, 23 Sep 2020 14:39:21 GMT
Connection: keep-alive
ETag: "5f6b5e19-8"
Accept-Ranges: bytes
```

### 3.4 核心配置示例

基于不同的IP、不同的端口以及不用得域名实现不同的虚拟主机，依赖于核心模块 ngx_http_core_module实现。

#### 3.4.1 新建一个 PC web 站点

```bash
#定义子配置文件路径
[root@centos8 ~]# mkdir /apps/nginx/conf/conf.d
[root@centos8 ~]# vim /apps/nginx/conf/nginx.conf
http {
 ......
 include /apps/nginx/conf/conf.d/*.conf; #在配置文件的最后面添加此行,注意不要放在最前面,会导致前面的命令无法生效
}
#创建PC网站配置
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/pc.conf
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   root /data/nginx/html/pc;
 }
}
[root@centos8 ~]# mkdir -p /data/nginx/html/pc 
[root@centos8 ~]# echo "pc web" > /data/nginx/html/pc/index.html
[root@centos8 ~]# systemctl   reload nginx
#访问测试
```

#### 3.4.2 新建一个 Mobile web 站点

```bash
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/mobile.conf
server {
 listen 80;
 server_name m.magedu.org;  #指定第二个站点名称
 location / {
   root /data/nginx/html/mobile;
 }
}
[root@centos8 ~]# mkdir -p /data/nginx/html/mobile  
[root@centos8 ~]# echo "mobile web" >> /data/nginx/html/mobile/index.html
[root@centos8 ~]# systemctl reload nginx
```

#### 3.4.3 root 与 alias

root：指定web的家目录，在定义location的时候，文件的绝对路径等于 root+location

```bash
#范例
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   root /data/nginx/html/pc;
 }
 location /about {
   root /opt/html; #必须要在html目录中创建一个名为about的目录才可以访问，否则报错。
 }
}
[root@centos8 ~]# mkdir -p /opt/html/about
[root@centos8 ~]# echo about > /opt/html/about/index.html
#重启Nginx并访问测试
```

alias：定义路径别名，会把访问的路径重新定义到其指定的路径,文档映射的另一种机制;仅能用于 location上下文,此指令使用较少

```bash
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   root /data/nginx/html/pc;
 }
 location /about { #注意about后不要加/ , 使用alias的时候uri后面如果加了斜杠,则下面的路径配置必须加斜杠，否则403
   alias /opt/html/about; #当访问about的时候，会显示alias定义的/opt/html/about里面的内容。
 }
}
#重启Nginx并访问测试
```

注意：location中使用root指令和alias指令的意义不同

```http
root 给定的路径对应于location中的/uri 左侧的/
alias 给定的路径对应于location中的/uri 的完整路径
```

#### 3.4.4 location 的详细使用

在一个server中location配置段可存在多个，用于实现从uri到文件系统的路径映射；ngnix会根据用户请 求的URI来检查定义的所有location，按一定的优先级找出一个最佳匹配，而后应用其配置 

在没有使用正则表达式的时候，nginx会先在server中的多个location选取匹配度最高的一个uri，uri是 用户请求的字符串，即域名后面的web文件路径，然后使用该location模块中的正则url和字符串，如果 匹配成功就结束搜索，并使用此location处理此请求。

location 官方帮助：http://nginx.org/en/docs/http/ngx_http_core_module.html#location

```bash
#语法规则：
location [ = | ~ | ~* | ^~ ] uri { ... }
=   #用于标准uri前，需要请求字串与uri精确匹配，大小敏感,如果匹配成功就停止向下匹配并立即处理请求
^~  #用于标准uri前，表示包含正则表达式,并且匹配以指定的正则表达式开头,对URI的最左边部分做匹配检查，不区分字符大小写
~   #用于标准uri前，表示包含正则表达式,并且区分大小写
~*  #用于标准uri前，表示包含正则表达式,并且不区分大写
不带符号 #匹配起始于此uri的所有的uri
\   #用于标准uri前，表示包含正则表达式并且转义字符。可以将 . * ?等转义为普通符号

#匹配优先级从高到低：
=, ^~, ~/~*, 不带符号
```

```bash
#官方范例
location = / {
   [ configuration A ]
}
location / {
   [ configuration B ]
}
location /documents/ {
   [ configuration C ]
}
location ^~ /images/ {
   [ configuration D ]
}
location ~* \.(gif|jpg|jpeg)$ {
   [ configuration E ]
}
The “/” request will match configuration A(?), the “/index.html” request will 
match configuration B,
the “/documents/document.html” request will match configuration C, the 
“/images/1.gif” request will match configuration D, and the “/documents/1.jpg” 
request will match configuration E.
```

##### 3.4.4.1 匹配案例-精确匹配

在server部分使用location配置一个web界面，例如：当访问nginx 服务器的/logo.jpg的时候要显示指定 html文件的内容 

精确匹配一般用于匹配组织的logo等相对固定的URL,匹配优先级最高

```bash
范例: 精确匹配 logo
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/pc.conf 
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   root /data/nginx/html/pc;
 }
 location = /logo.jpg {
   root /data/nginx/images;
   index index.html;
 }
}
#上传logo.jpg图片到/data/nginx/images，重启Nginx并访问测试
#访问测试：http://www.magedu.org/logo.jpg
```

##### 3.4.4.2  匹配案例-区分大小写

~ 实现区分大小写的模糊匹配. 以下范例中, 如果访问uri中包含大写字母的JPG，则以下location匹配 Ax.jpg条件不成功，因为 ~ 区分大小写，当用户的请求被执行匹配时发现location中定义的是小写的 jpg，本次访问的uri匹配失败，后续要么继续往下匹配其他的location（如果有），要么报错给客户端

```bash
location ~ /A.?\.jpg { #匹配字母A开头的jpg图片，后面?表示A后面零次或一个字符
   index index.html;
   root /data/nginx/html/image;
}
  
#重启Nginx并访问测试
#将只能访问以小写字符的jpg图片，不能识别大写的JPG结尾的图片
```

##### 3.4.4.3 匹配案例-不区分大小写

~* 用来对用户请求的uri做模糊匹配，uri中无论都是大写、都是小写或者大小写混合，此模式也都会匹 配，通常使用此模式匹配用户request中的静态资源并继续做下一步操作,此方式使用较多 

注意: 此方式中,对于Linux文件系统上的文件仍然是区分大小写的,如果磁盘文件不存在,仍会提示404 

```bash
#正则表达式匹配：
 location ~* /A.?\.jpg {
   index index.html;
   root /opt/nginx/html/image;
 }
#重启Nginx并访问测试
对于不区分大小写的location，则可以访问任意大小写结尾的图片文件,如区分大小写则只能访问Aa.jpg此类文件，不区分大小写则可以访问除了aa.jpg以外,还有其它的资源比如Aa.JPG、aA.jPG这样的混合名称文件，但是还同时也要求nginx服务器的资源目录有相应的文件，比如:必须有Aa.JPG,aA.jPG这样文件存在。
```

##### 3.4.4.4 匹配案例-URI开始

```bash
location ^~ /images {
   root /data/nginx/;
   index index.html;
 }
location /api {
   alias /data/nginx/api;
   index index.html;
}  
#重启Nginx并访问测试，实现效果是访问/images和/app返回不同的结果
[root@centos8 ~]# curl http://www.magedu.org/images/
images
[root@centos8 ~]# curl http://www.magedu.org/api/
api web
```

##### 3.4.4.5 匹配案例-文件名后缀

```bash
[root@centos8 ~]# mkdir /data/nginx/static
#上传一个图片到/data/nginx/static
 location ~* \.(gif|jpg|jpeg|bmp|png|tiff|tif|ico|wmf|js|css)$ {
   root /data/nginx/static;
   index index.html;
 }
#重启Nginx并访问测试 
```

##### 3.4.4.6 匹配案例-优先级

```bash
location = /1.jpg {
   root /data/nginx/static1;
   index index.html;
 }
 location /1.jpg {
   root /data/nginx/static2;
   index index.html;
}
 location ~* \.(gif|jpg|jpeg|bmp|png|tiff|tif|ico|wmf|js)$ {
   root /data/nginx/static3;
   index index.html;
 }
[root@centos8 ~]# mkdir -p /data/nginx/static{1,2,3}
#上传图片到 /data/nginx/static{1,2,3} 并重启nginx访问测试
#匹配优先级：=, ^~, ～/～*，/
#location优先级：
(location =) > (location ^~ 路径) > (location ~,~* 正则顺序) > (location 完整路径) > (location 部分起始路径) > (/)
```

##### 3.4.4.7 生产使用案例

```bash
#直接匹配网站根会加速Nginx访问处理
location = /index.html {
   ......;
}
location / {
   ......;
}
#静态资源配置方法1
location ^~ /static/ {
   ......;
}
#静态资源配置方法2,应用较多
location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ {
   ......;
}
#多应用配置
location ~* /app1 {
     ......;
}
location ~* /app2 {
     ......;
}
```

#### 3.4.5 Nginx 四层访问控制

访问控制基于模块ngx_http_access_module实现，可以通过匹配客户端源IP地址进行限制 

注意: 如果能在防火墙设备控制,最好就不要在nginx上配置,可以更好的节约资源

官方帮助: http://nginx.org/en/docs/http/ngx_http_access_module.html

```bash
# 范例
location = /login/ {
   root /data/nginx/html/pc;
   allow 10.0.0.0/24;
   deny all;
 }
location /about {
   alias /data/nginx/html/pc;
   index index.html;
   deny  192.168.1.1;
   allow 192.168.1.0/24;
   allow 10.1.1.0/16;
   allow 2001:0db8::/32;
   deny all;  #按先小范围到大范围排序
 }
```

#### 3.4.6 Nginx 账户认证功能

由 ngx_http_auth_basic_module 模块提供此功能

官方帮助：http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html

```bash
#CentOS安装包
[root@centos8 ~]#yum -y install httpd-tools
#Ubuntu安装包
[root@Ubuntu ~]#apt -y install apache2-utils
#创建用户
#-b 非交互式方式提交密码
[root@centos8 ~]# htpasswd -cb /apps/nginx/conf/.htpasswd user1 123456
Adding password for user user1
[root@centos8 ~]# htpasswd -b /apps/nginx/conf/.htpasswd user2 123456
Adding password for user user2
[root@centos8 ~]# tail /apps/nginx/conf/.htpasswd 
user1:$apr1$Rjm0u2Kr$VHvkAIc5OYg.3ZoaGwaGq/
user2:$apr1$nIqnxoJB$LR9W1DTJT.viDJhXa6wHv.
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf
 location = /login/ {
   root /data/nginx/html/pc;
   index index.html;
   auth_basic           "login password";
   auth_basic_user_file /apps/nginx/conf/.htpasswd;
 }
#重启Nginx并访问测试
[root@centos6 ~]#curl http://user1:123456@www.magedu.org/login/
login page
[root@centos6 ~]#curl -u user2:123456 www.magedu.org/login/
login page
```

#### 3.4.7 自定义错误页面

自定义错误页，同时也可以用指定的响应状态码进行响应, 可用位置：http, server, location, if in  location

```bash
error_page code ... [=[response]] uri;
```

```
#范例
listen 80;
server_name www.magedu.org;
error_page  500 502 503 504 /error.html;
location = /error.html {
   root /data/nginx/html;
}
#重启nginx并访问不存在的页面进行测试

#范例: 自定义错误页面
error_page 404 /40x.html;
location = /40x.html {
 root /data/html/ ;
}

#范例: 如果404,就转到主页
#404转为302
#error_page 404 /index.html;
error_page  404  =302 /index.html; 
error_page 500 502 503 504 /50x.html;
     location = /50x.html {
}
```

#### 3.4.8 自定义错误日志

可以自定义错误日志

```bash
Syntax: error_log file [level];
Default: 
error_log logs/error.log error;
Context: main, http, mail, stream, server, location
level: debug, info, notice, warn, error, crit, alert, emerg
```

```bash
# 范例
[root@centos8 ~]# mkdir /data/nginx/logs
 listen 80;
 server_name www.magedu.org;
 error_page  500 502 503 504 404 /error.html; 
 access_log /apps/nginx/logs/magedu-org_access.log; 
 error_log /apps/nginx/logs/magedu-org_error.log; #定义错误日志
 location = /error.html {
   root   html;
 }
#重启nginx并访问不存在的页面进行测试并验证是在指定目录生成新的日志文件
```

#### 3.4.9 检测文件是否存在

try_files会按顺序检查文件是否存在，返回第一个找到的文件或文件夹（结尾加斜线表示为文件夹），如 果所有文件或文件夹都找不到，会进行一个内部重定向到最后一个参数。只有最后一个参数可以引起一 个内部重定向，之前的参数只设置内部URI的指向。最后一个参数是回退URI且必须存在，否则会出现内部500错误。

```bash
# 语法格式
Syntax: try_files file ... uri;
try_files file ... =code;
Default: —
Context: server, location

# 范例：如果不存在页面, 就转到default.html页面
location / {
   root /data/nginx/html/pc;
   index index.html;
   try_files $uri  $uri.html $uri/index.html /about/default.html;
    #try_files $uri $uri/index.html $uri.html =489;
}
[root@centos8 ~]# echo "default page" >> /data/nginx/html/pc/about/default.html
#重启nginx并测试，当访问到http://www.magedu.org/about/xx.html等不存在的uri会显示default.html，如果是自定义的状态码则会显示在返回数据的状态码中
#注释default.html行,启用上面489响应码的那一行,在其生效后再观察结果
location / {
   root /data/nginx/html/pc;
   index index.html;
    #try_files $uri $uri.html $uri/index.html /about/default.html;
   try_files $uri $uri/index.html $uri.html =489;
}
[root@centos8 ~]# curl -I http://www.magedu.org/about/xx.html
HTTP/1.1 489 #489就是自定义的状态返回码
Server: nginx
Date: Thu, 21 Feb 2019 00:11:40 GMT
Content-Length: 0
Connection: keep-alive
Keep-Alive: timeout=65
```

#### 3.4.10 长连接配置

```bash
keepalive_timeout timeout [header_timeout];  #设定保持连接超时时长，0表示禁止长连接，默认为75s，通常配置在http字段作为站点全局配置
keepalive_requests number;  #在一次长连接上所允许请求的资源的最大数量，默认为100次,建议适当调大,比如:500

# 范例
keepalive_requests 3;
keepalive_timeout  65 60;
#开启长连接后，返回客户端的会话保持时间为60s，单次长连接累计请求达到指定次数请求或65秒就会被断
开，第二个数字60为发送给客户端应答报文头部中显示的超时时间设置为60s：如不设置客户端将不显示超时
时间。
Keep-Alive:timeout=60  #浏览器收到的服务器返回的报文
#如果设置为0表示关闭会话保持功能，将如下显示：
Connection:close  #浏览器收到的服务器返回的报文
#使用命令测试：
[root@centos8 ~]# telnet www.magedu.org 80
Trying 10.0.0.8...
Connected to www.magedu.org.
Escape character is '^]'.
GET / HTTP/1.1
HOST: www.magedu.org
#Response Headers(响应头信息)：
HTTP/1.1 200 OK
Server: nginx/1.18.0
Date: Thu, 24 Sep 2020 04:35:35 GMT
Content-Type: text/html
Content-Length: 7
Last-Modified: Wed, 23 Sep 2020 14:39:21 GMT
Connection: keep-alive
Keep-Alive: timeout=60
ETag: "5c8a6b3a-7"
Accept-Ranges: bytes
#页面内容
pc web
```

#### 3.4.11 作为下载服务器配置

ngx_http_autoindex_module 模块处理以斜杠字符 "/" 结尾的请求，并生成目录列表,可以做为下载服务 配置使用

```http
官方文档
http://nginx.org/en/docs/http/ngx_http_autoindex_module.html
```

```bash
# 相关指令
autoindex on | off;#自动文件索引功能，默为off
autoindex_exact_size on | off;  #计算文件确切大小（单位bytes），off 显示大概大小（单位K、M)，默认on
autoindex_localtime on | off ; #显示本机时间而非GMT(格林威治)时间，默认off
autoindex_format html | xml | json | jsonp; #显示索引的页面文件风格，默认html
limit_rate rate; #限制响应客户端传输速率(除GET和HEAD以外的所有方法)，单位B/s,即bytes/second，默认值0,表示无限制,此指令由ngx_http_core_module提供
set $limit_rate 4k; #也可以通变量限速,单位B/s,同时设置,此项优级高.Rate limit can also be set in the $limit_rate variable, however, since version 1.17.0, this method is not recommended:

# 范例: 实现下载站点
#注意:download不需要index.html文件
[root@centos8 ~]# mkdir -p /data/nginx/html/pc/download
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf
 location /download {
   autoindex on;    #自动索引功能
   autoindex_exact_size   on; #计算文件确切大小（单位bytes），此为默认值,off只显示大概大小（单位kb、mb、gb） 
   autoindex_localtime   on; #on表示显示本机时间而非GMT(格林威治)时间,默为为off显示GMT时间
   limit_rate 1024k;   #限速,默认不限速
   root /data/nginx/html/pc;
 }

[root@centos8 ~]# cp /root/anaconda-ks.cfg /data/nginx/html/pc/download/
#重启Nginx并访问测试下载页面

```

#### 3.4.12 作为上传服务器

以下指令控制上传数据

```bash
client_max_body_size 1m; 		#设置允许客户端上传单个文件的最大值，默认值为1m,上传文件超过此值会出413错误
client_body_buffer_size size; 	#用于接收每个客户端请求报文的body部分的缓冲区大小;默认16k;超出此大小时，其将被暂存到磁盘上的由client_body_temp_path指令所定义的位置
client_body_temp_path path [level1 [level2 [level3]]];		#设定存储客户端请求报文的body部分的临时存储路径及子目录结构和数量，目录名为16进制的数字，使用hash之后的值从后往前截取1位、2位、2位作为目录名
[root@centos8 ~]# md5sum /data/nginx/html/pc/index.html 
95f6f65f498c74938064851b1bb 96 3d 4 /data/nginx/html/pc/index.html
1级目录占1位16进制，即2^4=16个目录  0-f
2级目录占2位16进制，即2^8=256个目录 00-ff
3级目录占2位16进制，即2^8=256个目录 00-ff
#配置示例：
client_max_body_size 100m; #如果太大，上传时会出现下图的413错误,注意:如果php上传,还需要修改php.ini的相关配置
client_body_buffer_size 1024k;
client_body_temp_path   /apps/nginx/client_body_temp/ 1 2 2; #上传时,Nginx会自动创建相关目录

#上传超过nginx指定client_max_body_size值会出413错误
[root@wang-liyun-pc ~]# tail /apps/nginx/logs/nginx.access.log
125.41.184.117 - - [27/Sep/2020:00:09:00 +0800] "POST /wp-admin/async-upload.php 
HTTP/1.1" 413 578 "http://www.wangxiaochun.com/wp-admin/post-new.php"
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/85.0.4183.121 Safari/537.36 Edg/85.0.564.63" "-"

#上传文件后,会自动生成相关目录
[root@wang-liyun-pc ~]# tree /apps/nginx/client_body_temp/
/apps/nginx/client_body_temp/
├── 5
│   └── 00
│       └── 00
└── 6
   └── 00
       └── 00
```

#### 3.4.13  其他配置

```bash
keepalive_disable none | browser ...;  
#对哪种浏览器禁用长连接

limit_except method ... { ... }，仅用于location
#禁止客户端使用除了指定的请求方法之外的其它方法,如果使用会出现403错误
method:GET, HEAD, POST, PUT, DELETE，MKCOL, COPY, MOVE, OPTIONS, PROPFIND, 
PROPPATCH, LOCK, UNLOCK, PATCH
limit_except GET {
 allow 192.168.0.0/24;
 allow 10.0.0.1;
 deny all;
}
#除了GET和HEAD 之外其它方法仅允许192.168.1.0/24网段主机使用
[root@centos8 ~]# mkdir /data/nginx/html/pc/upload
[root@centos8 ~]# echo "upload" > /data/nginx/html/pc/upload/index.html
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf
location /upload {
       root /data/nginx/html/pc;
       index index.html;
       limit_except GET {
           allow  10.0.0.6;
           deny all;
       }
}
  
#重启Nginx并进行测试上传文件
[root@centos8 ~]# systemctl restart nginx
[root@centos6 ~]# curl -XPUT /etc/issue http://www.magedu.org/about
curl: (3) <url> malformed
<html>
<head><title>405 Not Allowed</title></head> #Nginx已经允许,但是程序未支持上传功能
<body bgcolor="white">
<center><h1>405 Not Allowed</h1></center>
<hr><center>nginx</center>
</body>
</html>
[root@centos8 ~]# curl -XPUT /etc/issue http://www.magedu.org/upload
curl: (3) <url> malformed
<html>
<head><title>403 Forbidden</title></head> #Nginx拒绝上传
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

```bash
aio on | off  #是否启用asynchronous file I/O(AIO)功能，需要编译开启 --with-file-aio
#linux 2.6以上内核提供以下几个系统调用来支持aio：
1、SYS_io_setup：建立aio 的context
2、SYS_io_submit: 提交I/O操作请求
3、SYS_io_getevents：获取已完成的I/O事件
4、SYS_io_cancel：取消I/O操作请求
5、SYS_io_destroy：毁销aio的context
```

```bash
directio size | off; #操作完全和aio相反，aio是读取文件而directio是写文件到磁盘，启用直接I/O，默认为关闭，当文件大于等于给定大小时，例如:directio 4m，同步（直接）写磁盘，而非写缓存。
```

```bash
open_file_cache off;  #是否缓存打开过的文件信息
open_file_cache max=N [inactive=time];
#nginx可以缓存以下三种信息：
(1) 文件元数据：文件的描述符、文件大小和最近一次的修改时间
(2) 打开的目录结构
(3) 没有找到的或者没有权限访问的文件的相关信息 
max=N：#可缓存的缓存项上限数量;达到上限后会使用LRU(Least recently used，最近最少使用)算法实现管理
inactive=time：#缓存项的非活动时长，在此处指定的时长内未被命中的或命中的次数少于open_file_cache_min_uses指令所指定的次数的缓存项即为非活动项，将被删除
open_file_cache_valid time; #缓存项有效性的检查验证频率，默认值为60s 
open_file_cache_errors on | off; #是否缓存查找时发生错误的文件一类的信息,默认值为off
open_file_cache_min_uses number; #open_file_cache指令的inactive参数指定的时长内，至少被命中此处指定的次数方可被归类为活动项,默认值为1 
```

```bash
# 范例
open_file_cache max=10000 inactive=60s; #最大缓存10000个文件，非活动数据超时时长60s
open_file_cache_valid   60s;  #每间隔60s检查一下缓存数据有效性
open_file_cache_min_uses 5; #60秒内至少被命中访问5次才被标记为活动数据
open_file_cache_errors   on; #缓存错误信息
```

## 4 Nginx高级配置

### 4.1 Nginx 状态页

基于nginx 模块 ngx_http_stub_status_module 实现，在编译安装nginx的时候需要添加编译参数 -- with-http_stub_status_module，否则配置完成之后监测会是提示语法错误

注意: 状态页显示的是整个服务器的状态,而非虚拟主机的状态

```bash
#配置示例：
location /nginx_status {
   stub_status;
   auth_basic           "auth login";
   auth_basic_user_file /apps/nginx/conf/.htpasswd;
   allow 192.168.0.0/16;
   allow 127.0.0.1;
   deny all;
 }
#状态页用于输出nginx的基本状态信息：
#输出信息示例：
Active connections: 291
server accepts handled requests
 16630948 16630948 31070465
 上面三个数字分别对应accepts,handled,requests三个值
Reading: 6 Writing: 179 Waiting: 106
Active connections： #当前处于活动状态的客户端连接数，包括连接等待空闲连接数=reading+writing+waiting
accepts：#统计总值，Nginx自启动后已经接受的客户端请求连接的总数。
handled：#统计总值，Nginx自启动后已经处理完成的客户端请求连接总数，通常等于accepts，除非有因worker_connections限制等被拒绝的连接
requests：#统计总值，Nginx自启动后客户端发来的总的请求数。
Reading：#当前状态，正在读取客户端请求报文首部的连接的连接数,数值越大,说明排队现象严重,性能不足
Writing：#当前状态，正在向客户端发送响应报文过程中的连接数,数值越大,说明访问量很大
Waiting：#当前状态，正在等待客户端发出请求的空闲连接数，开启 keep-alive的情况下,这个值等于active – (reading+writing)
```

范例:分析网站当前访问量

```
[root@centos7 ~]#curl http://wang:123456@www.wangxiaochun.com/nginx_status 2> 
/dev/null |awk '/Reading/{print $2,$4,$6}'
0 1 15
```

### 4.2 Nginx 第三方模块

第三模块是对nginx 的功能扩展，第三方模块需要在编译安装Nginx 的时候使用参数--addmodule=PATH指定路径添加，有的模块是由公司的开发人员针对业务需求定制开发的，有的模块是开 源爱好者开发好之后上传到github进行开源的模块，nginx的第三方模块需要从源码重新编译进行支持

比如:开源的echo模块 https://github.com/openresty/echo-nginx-module

```bash
[root@centos8 ~]# systemctl stop nginx
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf 
 location /main {
     index index.html;
     default_type text/html;
     echo "hello world,main-->";
     echo $remote_addr ;
     echo_reset_timer;   #将计时器开始时间重置为当前时间
     echo_location /sub1;
     echo_location /sub2;
     echo "took $echo_timer_elapsed sec for total.";
 }
 location /sub1 {
     echo_sleep 1;
     echo sub1;
 }
 location /sub2 {
     echo_sleep 1;
     echo sub2;
 }
[root@centos8 ~]# /apps/nginx/sbin/nginx -t
nginx: [emerg] unknown directive "echo_reset_timer" in
/apps/nginx/conf/conf.d/pc.conf:86
nginx: configuration file /apps/nginx/conf/nginx.conf test failed
#解决以上报错问题
[root@centos8 ~]# cd /usr/local/src
[root@centos8 src]# yum install git -y
[root@centos8 src]# git clone https://github.com/openresty/echo-nginx-module.git
[root@centos8 src]# cd nginx-1.18.0/
[root@centos8 src]# ./configure \
--prefix=/apps/nginx \
--user=nginx --group=nginx \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-pcre \
--with-stream \
--with-stream_ssl_module \
--with-stream_realip_module \
--with-http_perl_module \
--add-module=/usr/local/src/echo-nginx-module  #指定模块源代码路径
[root@centos8 src]# make && make install
#确认语法检测通过
[root@centos8 ~]# /apps/nginx/sbin/nginx -t
nginx: the configuration file /apps/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /apps/nginx/conf/nginx.conf test is successful
#重启nginx访问测试
[root@centos8 ~]# systemctl restart nginx
[root@centos8 ~]#nginx -V
nginx version: wanginx/1.68.9
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC) 
built with OpenSSL 1.1.1c FIPS  28 May 2019
TLS SNI support enabled
configure arguments: --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream
--with-stream_ssl_module --with-stream_realip_module --add-module=/usr/local/src/echo-nginx-module
#测试查看结果
[root@centos7 ~]#curl http://www.magedu.org/main
hello world,main-->
10.0.0.7
sub1
sub2
took 2.003 sec for total.
```

### 4.3 Nginx 变量使用

nginx的变量可以在配置文件中引用，作为功能判断或者日志等场景使用 

变量可以分为内置变量和自定义变量 

内置变量是由nginx模块自带，通过变量可以获取到众多的与客户端访问相关的值。

#### 4.3.1 内置变量

官方文档：http://nginx.org/en/docs/varindex.html

* 常用内置变量

```bash
$remote_addr; 
#存放了客户端的地址，注意是客户端的公网IP
$proxy_add_x_forwarded_for
#此变量表示将客户端IP追加请求报文中X-Forwarded-For首部字段,多个IP之间用逗号分隔,如果请求中没有X-Forwarded-For,就使用$remote_addr
the “X-Forwarded-For” client request header field with the $remote_addr variable 
appended to it, separated by a comma. If the “X-Forwarded-For” field is not 
present in the client request header, the $proxy_add_x_forwarded_for variable is 
equal to the $remote_addr variable.
$args; 
#变量中存放了URL中的所有参数，例如:http://www.magedu.org/main/index.do?id=20190221&partner=search
#返回结果为: id=20190221&partner=search
$is_args
#如果有参数为? 否则为空
“?” if a request line has arguments, or an empty string otherwise
$document_root; 
#保存了针对当前资源的请求的系统根目录,例如:/apps/nginx/html。
$document_uri;
#保存了当前请求中不包含参数的URI，注意是不包含请求的指令，比如:http://www.magedu.org/main/index.do?id=20190221&partner=search会被定义为/main/index.do 
#返回结果为:/main/index.do
$host; 
#存放了请求的host名称
limit_rate 10240;
echo $limit_rate;
#如果nginx服务器使用limit_rate配置了显示网络速率，则会显示，如果没有设置， 则显示0
$remote_port;
#客户端请求Nginx服务器时随机打开的端口，这是每个客户端自己的端口
$remote_user;
#已经经过Auth Basic Module验证的用户名
$request_body_file;
#做反向代理时发给后端服务器的本地资源的名称
$request_method;
#请求资源的方式，GET/PUT/DELETE等
$request_filename;
#当前请求的资源文件的磁盘路径，由root或alias指令与URI请求生成的文件绝对路径，如:/apps/nginx/html/main/index.html
$request_uri;
#包含请求参数的原始URI，不包含主机名，相当于:$document_uri?$args,例如：/main/index.do?id=20190221&partner=search 
$scheme;
#请求的协议，例如:http，https,ftp等
$server_protocol;
#保存了客户端请求资源使用的协议的版本，例如:HTTP/1.0，HTTP/1.1，HTTP/2.0等
$server_addr;
#保存了服务器的IP地址
$server_name;
#请求的服务器的主机名
$server_port;
#请求的服务器的端口号
$http_user_agent;
#客户端浏览器的详细信息
$http_cookie;
#客户端的所有cookie信息
$cookie_<name>
#name为任意请求报文首部字部cookie的key名
$http_<name>
#name为任意请求报文首部字段,表示记录请求报文的首部字段，ame的对应的首部字段名需要为小写，如果有横线需要替换为下划线
arbitrary request header field; the last part of a variable name is the field 
name converted to lower case with dashes replaced by underscores #用下划线代替横线
#示例: 
echo $http_user_agent; 
echo $http_host;
$sent_http_<name>
#name为响应报文的首部字段，name的对应的首部字段名需要为小写，如果有横线需要替换为下划线,此变量有问题
echo $sent_http_server;
$arg_<name>
#此变量存放了URL中的指定参数，name为请求url中指定的参数
echo $arg_id;
```

#### 4.3.2 自定义变量

假如需要自定义变量名称和值，使用指令set $variable value;

语法格式：

```bash
Syntax: set $variable value;
Default: —
Context: server, location, if
```

```bash
# 范例
set $name magedu;
echo $name;
set $my_port $server_port;
echo $my_port;
echo "$server_name:$server_port";
#输出信息如下
[root@centos6 ~]#curl www.magedu.org/main
magedu
80
www.magedu.org:80
```

### 4.4 Nginx 自定义访问日志

访问日志是记录客户端即用户的具体请求内容信息，而在全局配置模块中的error_log是记录nginx服务 器运行时的日志保存路径和记录日志的level，因此两者是不同的，而且Nginx的错误日志一般只有一 个，但是访问日志可以在不同server中定义多个，定义一个日志需要使用access_log指定日志的保存路 径，使用log_format指定日志的格式，格式中定义要保存的具体日志内容。

访问日志由 ngx_http_log_module 模块实现

官方帮助文档: http://nginx.org/en/docs/http/ngx_http_log_module.html

语法格式：

```bash
Syntax: access_log path [format [buffer=size] [gzip[=level]] [flush=time] 
[if=condition]];
access_log off; #关闭访问日志
Default: 
access_log logs/access.log combined;
Context: http, server, location, if in location, limit_except
```

#### 4.4.1 自定义默认格式日志

如果是要保留日志的源格式，只是添加相应的日志内容，则配置如下：

```bash
#注意:此指令只支持http块,不支持server块
log_format nginx_format1  '$remote_addr - $remote_user [$time_local] "$request" 
'
                      '$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"'
'$server_name:$server_port';
#注意:此指令一定要在放在log_format命令后
access_log logs/access.log nginx_format1;
#重启nginx并访问测试日志格式
==> /apps/nginx/logs/access.log <==
10.0.0.1 - - [22/Feb/2019:08:44:14 +0800] "GET /favicon.ico HTTP/1.1" 404 162 "-
" "Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:65.0) Gecko/2
0100101 Firefox/65.0" "-"www.magedu.org:80
```

#### 4.4.2 自定义json格式日志

Nginx 的默认访问日志记录内容相对比较单一，默认的格式也不方便后期做日志统计分析，生产环境中 通常将nginx日志转换为json日志，然后配合使用ELK做日志收集,统计和分析。

```bash
   log_format access_json '{"@timestamp":"$time_iso8601",'
        '"host":"$server_addr",'
        '"clientip":"$remote_addr",'
        '"size":$body_bytes_sent,'
        '"responsetime":$request_time,' #总的处理时间
        '"upstreamtime":"$upstream_response_time",'
        '"upstreamhost":"$upstream_addr",'   #后端应用服务器处理时间
        '"http_host":"$host",'
        '"uri":"$uri",'
        '"xff":"$http_x_forwarded_for",'
        '"referer":"$http_referer",'
        '"tcp_xff":"$proxy_protocol_addr",'
        '"http_user_agent":"$http_user_agent",'
        '"status":"$status"}';
     access_log /apps/nginx/logs/access_json.log access_json;
     
#重启Nginx并访问测试日志格式,参考链接:http://json.cn/
{"@timestamp":"2019-02-
22T08:55:32+08:00","host":"10.0.0.8","clientip":"10.0.0.1","size":162,"responsetime":0.000,"upstreamtime":"-","upstreamhost":"-","http_host":"www.magedu.org","uri":"/favicon.ico","xff":"-","referer":"-
","tcp_xff":"","http_user_agent":"Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:65.0) Gecko/20100101 Firefox/65.0","status":"404"}
```

#### 4.4.3 json 格式的日志访问统计

```bash
#python3
[root@centos8 ~]#dnf -y install python3
[root@centos8 ~]#cat log.py
#!/usr/bin/env python3
#coding:utf-8
status_200= []
status_404= []
with open("access_json.log") as f:
    for line in f.readlines():
        line = eval(line)
        if line.get("status") == "200":
            status_200.append(line.get)
        elif line.get("status") == "404":
            status_404.append(line.get)
        else:
            print("状态码 ERROR")
        print((line.get("clientip")))
f.close()
print("状态码200的有--:",len(status_200))
print("状态码404的有--:",len(status_404))
#python2
[root@centos8 ~]#dnf -y install python2
[root@centos8 ~]#cat log.py
#!/usr/bin/env python
#coding:utf-8
status_200= []
status_404= []
with open("access_json.log") as f:
    for line in f.readlines():
        line = eval(line)
        if line.get("status") == "200":
            status_200.append(line.get)
        elif line.get("status") == "404":
            status_404.append(line.get)
        else:
            print("状态码 ERROR")
        print(line.get("clientip"))
f.close()
print "状态码200的有--:",len(status_200)
print "状态码404的有--:",len(status_404)
#保存日志文件到指定路径并进测试：
[root@centos7 ~]# python nginx_json.py 
状态码200的有--: 1910
状态码404的有--: 13
    
    
#转换python2语法到python3
[root@centos8 ~]#pip3 install 2to3
[root@centos8 ~]#2to3 -w log.py
```

### 4.5 Nginx 压缩功能

Nginx支持对指定类型的文件进行压缩然后再传输给客户端，而且压缩还可以设置压缩比例，压缩后的文件大小将比源文件显著变小，这样有助于降低出口带宽的利用率，降低企业的IT支出，不过会占用相应的CPU资源。

Nginx对文件的压缩功能是依赖于模块 ngx_http_gzip_module,默认是内置模块 

官方文档： https://nginx.org/en/docs/http/ngx_http_gzip_module.html 

配置指令如下：

```bash
#启用或禁用gzip压缩，默认关闭
gzip on | off; 
#压缩比由低到高从1到9，默认为1
gzip_comp_level level;
#禁用IE6 gzip功能
gzip_disable "MSIE [1-6]\."; 
#gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k; 
#启用压缩功能时，协议的最小版本，默认HTTP/1.1
gzip_http_version 1.0 | 1.1; 
#指定Nginx服务需要向服务器申请的缓存空间的个数和大小,平台不同,默认:32 4k或者16 8k;
gzip_buffers number size;  
#指明仅对哪些类型的资源执行压缩操作;默认为gzip_types text/html，不用显示指定，否则出错
gzip_types mime-type ...; 
#如果启用压缩，是否在响应报文首部插入“Vary: Accept-Encoding”,一般建议打开
gzip_vary on | off; 
#预压缩，即直接从磁盘找到对应文件的gz后缀的式的压缩文件返回给用户，无需消耗服务器CPU
#注意: 来自于ngx_http_gzip_static_module模块
gzip_static on | off;
#重启nginx并进行访问测试压缩功能
[root@centos8 ~]# cp /apps/nginx/logs/access.log /data/nginx/html/pc/m.txt
[root@centos8 ~]# echo "test" > /data/nginx/html/pc/test.html #小于1k的文件测试是否会压缩
[root@centos8 ~]# vim /apps/nginx/conf/nginx.conf
gzip on;
gzip_comp_level 5;
gzip_min_length 1k;
gzip_types text/plain application/javascript application/x-javascript text/css 
application/xml text/javascript application/x-httpd-php image/gif image/png;   
gzip_vary on;
#重启Nginx并访问测试：
[root@centos8 ~]# curl --head --compressed http://www.magedu.org/test.html
HTTP/1.1 200 OK
Server: nginx
Date: Fri, 22 Feb 2019 01:52:23 GMT
Content-Type: text/html
Last-Modified: Thu, 21 Feb 2019 10:31:18 GMT
Connection: keep-alive
Keep-Alive: timeout=65
Vary: Accept-Encoding
ETag: W/"5c6e7df6-171109"
Content-Encoding: gzip #压缩传输
#验证不压缩访问的文件大小：
```

### 4.6 https 功能

Web网站的登录页面通常都会使用https加密传输的，加密数据以保障数据的安全，HTTPS能够加密信 息，以免敏感信息被第三方获取，所以很多银行网站或电子邮箱等等安全级别较高的服务都会采用 HTTPS协议，HTTPS其实是有两部分组成：HTTP + SSL / TLS，也就是在HTTP上又加了一层处理加密信 息的模块。服务端和客户端的信息传输都会通过TLS进行加密，所以传输的数据都是加密后的数据。

```http
https 实现过程如下：
1.客户端发起HTTPS请求：
客户端访问某个web端的https地址，一般都是443端口
2.服务端的配置：
采用https协议的服务器必须要有一套证书，可以通过一些组织申请，也可以自己制作，目前国内很多网站都自己做的，当你访问一个网站的时候提示证书不可信任就表示证书是自己做的，证书就是一个公钥和私钥匙，就像一把锁和钥匙，正常情况下只有你的钥匙可以打开你的锁，你可以把这个送给别人让他锁住一个箱子，里面放满了钱或秘密，别人不知道里面放了什么而且别人也打不开，只有你的钥匙是可以打开的。
3.传送证书：
服务端给客户端传递证书，其实就是公钥，里面包含了很多信息，例如证书得到颁发机构、过期时间等等。
4.客户端解析证书：
这部分工作是有客户端完成的，首先回验证公钥的有效性，比如颁发机构、过期时间等等，如果发现异常则会弹出一个警告框提示证书可能存在问题，如果证书没有问题就生成一个随机值，然后用证书对该随机值进行加密，就像2步骤所说把随机值锁起来，不让别人看到。
5.传送4步骤的加密数据：
就是将用证书加密后的随机值传递给服务器，目的就是为了让服务器得到这个随机值，以后客户端和服务端的通信就可以通过这个随机值进行加密解密了。
6.服务端解密信息：
服务端用私钥解密5步骤加密后的随机值之后，得到了客户端传过来的随机值(私钥)，然后把内容通过该值进行对称加密，对称加密就是将信息和私钥通过算法混合在一起，这样除非你知道私钥，不然是无法获取其内部的内容，而正好客户端和服务端都知道这个私钥，所以只要机密算法够复杂就可以保证数据的安全性。
7.传输加密后的信息:
服务端将用私钥加密后的数据传递给客户端，在客户端可以被还原出原数据内容。
8.客户端解密信息：
客户端用之前生成的私钥获解密服务端传递过来的数据，由于数据一直是加密的，因此即使第三方获取到数据也无法知道其详细内容。
```

#### 4.6.1 https 配置参数

nginx 的https 功能基于模块ngx_http_ssl_module实现，因此如果是编译安装的nginx要使用参数 ngx_http_ssl_module开启ssl功能，但是作为nginx的核心功能，yum安装的nginx默认就是开启的，编 译安装的nginx需要指定编译参数--with-http_ssl_module开启

官方文档：https://nginx.org/en/docs/http/ngx_http_ssl_module.html

配置参数如下：

```bash
ssl on | off;   
#为指定的虚拟主机配置是否启用ssl功能，此功能在1.15.0废弃，使用listen [ssl]替代
listen 443 ssl;
ssl_certificate /path/to/file;
#指向包含当前虚拟主机和CA的两个证书信息的文件，一般是crt文件
ssl_certificate_key /path/to/file;
#当前虚拟主机使用的私钥文件，一般是key文件
ssl_protocols [SSLv2] [SSLv3] [TLSv1] [TLSv1.1] [TLSv1.2]; 
#支持ssl协议版本，早期为ssl现在是TLS，默认为后三个
ssl_session_cache off | none | [builtin[:size]] [shared:name:size];
#配置ssl缓存
   off： #关闭缓存
   none:  #通知客户端支持ssl session cache，但实际不支持
   builtin[:size]：#使用OpenSSL内建缓存，为每worker进程私有
   [shared:name:size]：#在各worker之间使用一个共享的缓存，需要定义一个缓存名称和缓存空间大小，一兆可以存储4000个会话信息，多个虚拟主机可以使用相同的缓存名称
ssl_session_timeout time;
#客户端连接可以复用ssl session cache中缓存的有效时长，默认5m 
```

#### 4.6.2 自签名证书

```bash
#自签名CA证书
[root@centos8 ~]# cd /apps/nginx/
[root@centos8 nginx]# mkdir certs
[root@centos8 nginx]# cd certs/
[root@centos8 nginx]# openssl req -newkey rsa:4096 -nodes -sha256 -keyout ca.key -x509 -days 3650 -out ca.crt #自签名CA证书
Generating a 4096 bit RSA private key
.................++
.....
Country Name (2 letter code) [XX]:CN #国家代码
State or Province Name (full name) []:BeiJing   #省份
Locality Name (eg, city) [Default City]:Beijing #城市名称
Organization Name (eg, company) [Default Company Ltd]:magedu.Ltd #公司名称
Organizational Unit Name (eg, section) []:magedu #部门
Common Name (eg, your name or your server's hostname) []:ca.magedu.org #通用名称
Email Address []: #邮箱
[root@centos8 certs]# ll ca.crt 
-rw-r--r-- 1 root root 2118 Feb 22 12:10 ca.crt
#自制key和csr文件
[root@centos8 certs]# openssl req -newkey rsa:4096 -nodes -sha256 -keyout www.magedu.org.key     -out www.magedu.org.csr 
Generating a 4096 bit RSA private key
........................................................................++
......
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:BeiJing
Locality Name (eg, city) [Default City]:BeiJing
Organization Name (eg, company) [Default Company Ltd]:magedu.org
Organizational Unit Name (eg, section) []:magedu.org
Common Name (eg, your name or your server's hostname) []:www.magedu.org
Email Address []:2973707860@qq.com
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:      
An optional company name []:
[root@centos8 certs]# ll
total 16
-rw-r--r-- 1 root root 2118 Feb 22 12:10 ca.crt
-rw-r--r-- 1 root root 3272 Feb 22 12:10 ca.key
-rw-r--r-- 1 root root 1760 Feb 22 12:18 www.magedu.org.csr
-rw-r--r-- 1 root root 3272 Feb 22 12:18 www.magedu.org.key
#签发证书
[root@centos8 certs]# openssl x509 -req -days 3650 -in www.magedu.org.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out www.magedu.org.crt
#验证证书内容
[root@centos8 certs]# openssl x509 -in www.magedu.org.crt -noout -text 
Certificate:
   Data:
       Version: 1 (0x0)
       Serial Number:
           bb:76:ea:fe:f4:04:ac:06
   Signature Algorithm: sha256WithRSAEncryption
       Issuer: C=CN, ST=BeiJing, L=Beijing, O=magedu.Ltd, OU=magedu, 
CN=magedu.ca/emailAddress=2973707860@qq.com
       Validity
           Not Before: Feb 22 06:14:03 2019 GMT
           Not After : Feb 22 06:14:03 2020 GMT
       Subject: C=CN, ST=BeiJing, L=BeiJing, O=magedu.org, OU=magedu.org, 
CN=www.magedu.org/emailAddress=2973707860@qq.com
       Subject Public Key Info:
           Public Key Algorithm: rsaEncryption
               Public-Key: (4096 bit)
                
#合并CA和服务器证书成一个文件,注意服务器证书在前
[root@centos8 certs]#cat www.magedu.org.crt ca.crt > www.magedu.org.pem
```

#### 4.6.3 https 配置

```bash
server {
 listen 80;
 listen 443 ssl;
 ssl_certificate /apps/nginx/certs/www.magedu.org.pem;
 ssl_certificate_key /apps/nginx/certs/www.magedu.org.key;
 ssl_session_cache shared:sslcache:20m;
 ssl_session_timeout 10m;
 root /data/nginx/html; 
}
#重启Nginx并访问验证
```

#### 4.6.4 实现多域名 https

Nginx 支持基于单个IP实现多域名的功能，并且还支持单IP多域名的基础之上实现HTTPS，其实是基于 Nginx的 SNI（Server Name Indication）功能实现，SNI是为了解决一个Nginx服务器内使用一个IP绑定 多个域名和证书的功能，其具体功能是客户端在连接到服务器建立SSL链接之前先发送要访问站点的域名 （Hostname），这样服务器再根据这个域名返回给客户端一个合适的证书。

```bash
[root@lzy_ng1 ~]# nginx -V
nginx version: nginx/1.20.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled							#SNI功能
configure arguments: --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module
```

```bash
#制作key和csr文件
[root@centos8 certs]# openssl req -newkey rsa:4096 -nodes -sha256 -keyout 
m.magedu.org.key -out m.magedu.org.csr
Generating a 4096 bit RSA private key
..........
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:BeiJing
Locality Name (eg, city) [Default City]:BeiJing
Organization Name (eg, company) [Default Company Ltd]:magedu
Organizational Unit Name (eg, section) []:magedu
Common Name (eg, your name or your server's hostname) []:m.magedu.org
Email Address []:2973707860@qq.com
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
#签名证书
[root@centos8 certs]# openssl x509 -req -days 3650 -in m.magedu.org.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out m.magedu.org.crt
#验证证书内容
[root@centos8 certs]# openssl x509 -in m.magedu.org.crt -noout -text 
Certificate:
   Data:
       Version: 1 (0x0)
       Serial Number:
           bb:76:ea:fe:f4:04:ac:07
   Signature Algorithm: sha256WithRSAEncryption
       Issuer: C=CN, ST=BeiJing, L=Beijing, O=magedu.Ltd, OU=magedu, 
CN=magedu.ca
       Validity
           Not Before: Feb 22 13:50:43 2019 GMT
           Not After : Feb 19 13:50:43 2029 GMT
       Subject: C=CN, ST=BeiJing, L=BeiJing, O=magedu, OU=magedu, 
CN=m.magedu.org/emailAddress=2973707860@qq.com
       Subject Public Key Info:
           Public Key Algorithm: rsaEncryption
               Public-Key: (4096 bit)
..............
#合并证书文件
[root@centos8 certs]#cat m.magedu.org.crt ca.crt > m.magedu.org.pem
#Nginx 配置
[root@centos8 certs]# cat /apps/nginx/conf/conf.d/mobile.conf 
server {
 listen 80 default_server;
 server_name m.magedu.org;
 rewrite ^(.*)$ https://$server_name$1 permanent;
}
server {
 listen 443 ssl;
 server_name m.magedu.org;
 ssl_certificate /apps/nginx/certs/m.magedu.org.pem;
 ssl_certificate_key /apps/nginx/certs/m.magedu.org.key;
 ssl_session_cache shared:sslcache:20m;
 ssl_session_timeout 10m;
 location / {
   root "/data/nginx/html/mobile";
 }
location /mobile_status {
   stub_status;
 }
}
```

#### 4.6.5 实现 HSTS

官方文档：https://www.nginx.com/blog/http-strict-transport-security-hsts-and-nginx/

注意: 配置rewrite才能实现http跳转到https

```bash
server {
   listen 443 ssl;
   server_name www.magedu.org;
   add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"
always;
   location / {
       if ( $scheme = http ) {
         rewrite ^/(.*)$ https://www.magedu.org/$1 redirect;                   
                            
       }
   .....
   }
}
```

```bash
#范例
[root@centos8 ~]#vim /apps/nginx/conf/conf.d/pc.conf
server {
 listen 80;
 listen 443 ssl;
 ssl_certificate /apps/nginx/conf/conf.d/www.magedu.org.crt;
 ssl_certificate_key /apps/nginx/conf/conf.d/www.magedu.org.key;
 ssl_session_cache shared:sslcache:20m;
 ssl_session_timeout 10m;
 server_name www.magedu.org;
 error_log /apps/nginx/logs/magedu.org_error.log notice;  
 access_log /apps/nginx/logs/magedu.org_access.log main;
 add_header Strict-Transport-Security "max-age=31536000; includeSubDomains"
always;
 location / {
   root /data/nginx/html/pc;
   if ( $scheme = http ) {
         rewrite ^/(.*)$ https://www.magedu.org/$1 redirect;                   
                            
   }
 }
[root@centos8 ~]#systemctl restart nginx
[root@centos7 ~]#curl -ikL https://www.magedu.org
HTTP/1.1 200 OK
Server: nginx/1.18.0
Date: Thu, 08 Oct 2020 15:29:56 GMT
Content-Type: text/html
Content-Length: 7
Last-Modified: Sat, 26 Sep 2020 01:18:32 GMT
Connection: keep-alive
ETag: "5f6e96e8-7"
Strict-Transport-Security: max-age=31536000; includeSubDomains
Accept-Ranges: bytes
pc web
```

### 4.7 关于 favicon.ico

favicon.ico 文件是浏览器收藏网址时显示的图标，当客户端使用浏览器问页面时，浏览器会自己主动发 起请求获取页面的favicon.ico文件，但是当浏览器请求的favicon.ico文件不存在时，服务器会记录404日 志，而且浏览器也会显示404报错

```bash
#方法一：服务器不记录访问日志：
location = /favicon.ico {
   log_not_found off;
   access_log off;
}
#方法二：将图标保存到指定目录访问：
#location ~ ^/favicon\.ico$ {
location = /favicon.ico {
     root   /data/nginx/html/pc/images;
     expires 365d;  #设置文件过期时间
}
```

### 4.8 升级 OpenSSL 版本

OpenSSL程序库当前广泛用于实现互联网的传输层安全（TLS）协议。心脏出血（Heartbleed），也简 称为心血漏洞，是一个出现在加密程序库OpenSSL的安全漏洞，此漏洞于2012年被引入了软件中， 2014年4月首次向公众披露。只要使用的是存在缺陷的OpenSSL实例，无论是服务器还是客户端，都可 能因此而受到攻击。此问题的原因是在实现TLS的心跳扩展时没有对输入进行适当验证（缺少边界检 查），因此漏洞的名称来源于“心跳”（heartbeat）。该程序错误属于缓冲区过读，即可以读取的数据比 应该允许读取的还多。

范例: 升级OpenSSL解决安全漏洞

```bash
#准备OpenSSL源码包：
[root@centos8 ~]#cd /usr/local/src
[root@centos8 src]#wget https://www.openssl.org/source/openssl-1.1.1h.tar.gz
[root@centos8 src]#tar xvf openssl-1.1.1h.tar.gz
#编译安装Nginx并制定新版本OpenSSL路径：
[root@centos8 ~]#cd /usr/local/src/nginx-1.18.0/
[root@centos8 nginx-1.18.0]#./configure --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream --with-stream_ssl_module --with-stream_realip_module --add-module=/usr/local/src/echo-nginx-module --with-openssl=/usr/local/src/openssl-1.1.1h
[root@centos8 nginx-1.18.0]#make && make install
#验证并启动Nginx：
[root@centos8 ~]#nginx -t 
nginx: the configuration file /apps/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /apps/nginx/conf/nginx.conf test is successful
[root@centos8 ~]#systemctl restart nginx
[root@centos8 ~]#nginx -V
nginx version: wanginx/1.68.9
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC) 
built with OpenSSL 1.1.1h  22 Sep 2020
TLS SNI support enabled
configure arguments: --prefix=/apps/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_stub_status_module --with-http_gzip_static_module --with-pcre --with-stream
--with-stream_ssl_module --with-stream_realip_module --add-module=/usr/local/src/echo-nginx-module --with-openssl=/usr/local/src/openssl-1.1.1h
```

## 5 Nginx Rewrite相关

## 6 Nginx反向代理

反向代理：reverse proxy，指的是代理外网用户的请求到内部的指定的服务器，并将数据返回给用户的 一种方式，这是用的比较多的一种方式。

Nginx 除了可以在企业提供高性能的web服务之外，另外还可以将 nginx 本身不具备的请求通过某种预 定义的协议转发至其它服务器处理，不同的协议就是Nginx服务器与其他服务器进行通信的一种规范， 主要在不同的场景使用以下模块实现不同的功能

```bash
ngx_http_proxy_module： #将客户端的请求以http协议转发至指定服务器进行处理
ngx_http_upstream_module #用于定义为proxy_pass,fastcgi_pass,uwsgi_pass等指令引用的后端服务器分组
ngx_stream_proxy_module：#将客户端的请求以tcp协议转发至指定服务器处理
ngx_http_fastcgi_module：#将客户端对php的请求以fastcgi协议转发至指定服务器助理
ngx_http_uwsgi_module： #将客户端对Python的请求以uwsgi协议转发至指定服务器处理
```

### 6.1 http 反向代理

官方文档： https://nginx.org/en/docs/http/ngx_http_proxy_module.html

#### 6.1.1 http 协议反向代理

##### 6.1.1.1 反向代理配置参数

```bash
#官方文档：https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass 
proxy_pass; 
#用来设置将客户端请求转发给的后端服务器的主机，可以是主机名(将转发至后端服务做为主机头首部)、IP
地址：端口的方式
#也可以代理到预先设置的主机群组，需要模块ngx_http_upstream_module支持
#示例:
 location /web {
   index index.html;
   proxy_pass http://10.0.0.18:8080; #8080后面无uri,即无 / 符号,需要将location后面url 附加到proxy_pass指定的url后面,此行为类似于root
    #proxy_pass指定的uri不带斜线将访问的/web,等于访问后端服务器http://10.0.0.18:8080/web/index.html，即后端服务器配置的站点根目录要有web目录才可以被访问
    # http://nginx/web/index.html ==> http://10.0.0.18:8080/web/index.html
   proxy_pass http://10.0.0.18:8080/;   #8080后面有uri,即有 / 符号,相当于置换,即访问/web时实际返回proxy_pass后面uri内容.此行为类似于alias 
    #proxy_pass指定的uri带斜线，等于访问后端服务器的http://10.0.0.18:8080/index.html 内容返回给客户端
 }  # http://nginx/web/index.html ==> http://10.0.0.18:8080
    
#重启Nginx测试访问效果：
#curl -L http://www.magedu.org/web
#如果location定义其uri时使用了正则表达式模式(包括~,~*,但不包括^~)，则proxy_pass之后必须不能
使用uri; 即不能有/ ,用户请求时传递的uri将直接附加至后端服务器之后
server {
 ...
 server_name HOSTNAME;
 location ~|~* /uri/ {
 proxy_pass http://host:port; #proxy_pass后面的url 不能加/
 }
 ...
 }
 http://HOSTNAME/uri/ --> http://host/uri/
```

```bash
proxy_hide_header field;
#用于nginx作为反向代理的时候，在返回给客户端http响应时，隐藏后端服务器相应头部的信息，可以设置在http,server或location块
#示例: 隐藏后端服务器ETag首部字段
 location /web {
   index index.html;
   proxy_pass http://10.0.0.18:8080/; 
   proxy_hide_header ETag;
 }
```

```bash
proxy_pass_header field;
#默认nginx在响应报文中不传递后端服务器的首部字段Date, Server, X-Pad, X-Accel等参数，如果要传递的话则要使用 proxy_pass_header field声明将后端服务器返回的值传递给客户端
#field 首部字段大小不敏感

#示例:透传后端服务器的Server和Date首部给客户端,同时不再响应报中显示前端服务器的Server字段
proxy_pass_header Server;
proxy_pass_header Date;
```

```bash
proxy_pass_request_body on | off; 
#是否向后端服务器发送HTTP实体部分,可以设置在http,server或location块，默认即为开启
```

```bash
proxy_pass_request_headers on | off; 
#是否将客户端的请求头部转发给后端服务器，可以设置在http,server或location块，默认即为开启
```

```bash
proxy_set_header; 
#可更改或添加客户端的请求头部信息内容并转发至后端服务器，比如在后端服务器想要获取客户端的真实IP的时候，就要更改每一个报文的头部

#示例: 
#proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
$proxy_add_x_forwarded_for
the “X-Forwarded-For” client request header field with the $remote_addr variable 
appended to it, separated by a comma. If the “X-Forwarded-For” field is not 
present in the client request header, the $proxy_add_x_forwarded_for variable is 
equal to the $remote_addr variable.

proxy_set_header X-Real-IP  $remote_addr;  
#添加HOST到报文头部，如果客户端为NAT上网那么其值为客户端的共用的公网IP地址，常用于在日之中记录客户端的真实IP地址。
#在后端httpd服务器修改配置,添加日志记录X-Forwarded-For字段
	LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%{X-Real-IP}i\"" combined 
```

```bash
#在后端服务器查看日志
[root@centos8 ~]#tail /var/log/httpd/access_log   -f
10.0.0.8 - - [09/Oct/2020:21:50:57 +0800] "HEAD /static/index.html HTTP/1.0" 200
- "-" "curl/7.29.0" "10.0.0.7"
```

```bash
proxy_connect_timeout time;
#配置nginx服务器与后端服务器尝试建立连接的超时时间，默认为60秒，用法如下：
proxy_connect_timeout 6s; 
#60s为自定义nginx与后端服务器建立连接的超时时间,超时会返回客户端504响应码

proxy_read_timeout time;
#配置nginx服务器向后端服务器或服务器组发起read请求后，等待的超时时间，默认60s
proxy_send_timeout time; 
#配置nginx项后端服务器或服务器组发起write请求后，等待的超时 时间，默认60s
```

```bash
proxy_http_version 1.0; 
#用于设置nginx提供代理服务的HTTP协议的版本，默认http 1.0
```

```bash
proxy_ignore_client_abort off; 
#当客户端网络中断请求时，nginx服务器中断其对后端服务器的请求。即如果此项设置为on开启，则服务器会忽略客户端中断并一直等着代理服务执行返回，如果设置为off，则客户端中断后Nginx也会中断客户端请求并立即记录499日志，默认为off。
```

```bash
proxy_headers_hash_bucket_size 128;
#当配置了 proxy_hide_header和proxy_set_header的时候，用于设置nginx保存HTTP报文头的hash表的上限
proxy_headers_hash_max_size 512; 
#设置proxy_headers_hash_bucket_size的最大可用空间
server_namse_hash_bucket_size 512; 
#server_name hash表申请空间大小
server_names_hash_max_size  512; 
#设置服务器名称hash表的上限大小
proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | 
http_503 | http_504;
#当一台后端朋务器出错,超时,无效首部,500等时,切换至下一个后端服务器提供服务
```

##### 6.1.1.2 实战案例: 反向代理单台 web 服务器

要求：将用户对域 www.magedu.org 的请求转发给后端服务器处理

```bash
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/pc.conf
server {
 listen 80;
 server_name www.magedu.org;
 location / {
     proxy_pass http://10.0.0.18/; # http://10.0.0.18/ 最后的 / 可加或不加
 }
}
#重启Nginx 并访问测试
```

如果后端服务器无法连接((比如:iptables -AINPUT -s nginx_ip -j REJECT或者systemctl stop httpd)), 会 出现下面提示

（502 Bad Gateway）

默认在1分钟内后端服务器无法响应(比如:iptables -AINPUT -s nginx_ip -j DROP),会显示下面的504超时 提示

（504 Gateway Time-out）

##### 6.1.1.3 实战案例: 指定 location 实现反向代理

* 针对指定的 location

```bash
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   index index.html index.php;
   root /data/nginx/html/pc;
   }
 location /static {
   #proxy_pass http://10.0.0.18:80/; #注意有后面的/， 表示置换
   proxy_pass http://10.0.0.18:80;  #后面没有 / , 表示附加
 }
}
#后端web服务器必须要有相对于的访问URL
[root@centos8 ~]# mkdir /var/www/html/static
[root@centos8 ~]# echo "web1 page for apache" > /var/www/html/index.html
[root@centos8 ~]# echo "web2 page for apache" > /var/www/html/static/index.html
#重启Nginx并访问测试：
[root@centos8 ~]# curl -L http://www.magedu.org/
pc web
[root@centos8 ~]# curl -L http://www.magedu.org/static
web2 page for apache
#Apache的访问日志：
[root@centos8 ~]# tail -f /var/log/httpd/access_log 
10.0.0.8 - - [04/Mar/2019:18:52:00 +0800] "GET /static/ HTTP/1.1" 200 21 "-" "curl/7.29.0"
```

* 针对特定的资源实现代理

```bash
# mg.cn/img_convert/47d07bfe7fa1b9e1befbc16dc8541647.png
[root@centos8 ~]#vim /apps/nginx/conf/conf.d/pc.conf
server {
......
 location ~ \.(jpe?g|png|bmp|gif)$ {
   proxy_pass http://10.0.0.18;     #如果加/ 语法出错                             
                                         
 }
}
#如果 http://10.0.0.18/ 有 / 语法出错    
[root@centos8 ~]#nginx -t
nginx: [emerg] "proxy_pass" cannot have URI part in location given by regular expression, or inside named location, or inside "if" statement, or inside "limit_except" block in /apps/nginx/conf/conf.d/pc.conf:19
nginx: configuration file /apps/nginx/conf/nginx.conf test failed
```

##### 6.1.1.4 反向代理示例: 缓存功能

缓存功能默认关闭状态,需要先动配置才能启用

```bash
proxy_cache zone_name | off; 默认off
#指明调用的缓存，或关闭缓存机制;Context:http, server, location
#zone_name 表示缓存的名称.需要由proxy_cache_path事先定义
```

```bash
proxy_cache_key string;
#缓存中用于“键”的内容，默认值：proxy_cache_key $scheme$proxy_host$request_uri;
```

```bash
proxy_cache_valid [code ...] time;
#定义对特定响应码的响应内容的缓存时长，定义在http{...}中
	示例:
	proxy_cache_valid 200 302 10m;
	proxy_cache_valid 404 1m;
```

```bash
proxy_cache_path;
#定义可用于proxy功能的缓存;Context:http 
proxy_cache_path path [levels=levels] [use_temp_path=on|off] 
keys_zone=zone_name:size [inactive=time] [max_size=size] [manager_files=number] 
[manager_sleep=time] [manager_threshold=time] [loader_files=number] 
[loader_sleep=time] [loader_threshold=time] [purger=on|off] 
[purger_files=number] [purger_sleep=time] [purger_threshold=time];
#示例：在http配置定义缓存信息
proxy_cache_path /var/cache/nginx/proxy_cache #定义缓存保存路径，proxy_cache会自动创
建
   levels=1:2:2 #定义缓存目录结构层次，1:2:2可以生成2^4x2^8x2^8=2^20=1048576个目录
   keys_zone=proxycache:20m #指内存中缓存的大小，主要用于存放key和metadata（如：使用次
数）,一般1M可存放8000个左右的key
   inactive=120s  #缓存有效时间  
   max_size=10g; #最大磁盘占用空间，磁盘存入文件内容的缓存空间最大值
#调用缓存功能，需要定义在相应的配置段，如server{...};或者location等
proxy_cache proxycache;
proxy_cache_key $request_uri; #对指定的数据进行MD5的运算做为缓存的key
proxy_cache_valid 200 302 301 10m; #指定的状态码返回的数据缓存多长时间
proxy_cache_valid any 1m;   #除指定的状态码返回的数据以外的缓存多长时间,必须设置,否则不会缓存
```

```bash
proxy_cache_use_stale error | timeout | invalid_header | updating | http_500 | 
http_502 | http_503 | http_504 | http_403 | http_404 | off ; #默认是off
#在被代理的后端服务器出现哪种情况下，可直接使用过期的缓存响应客户端

#示例
proxy_cache_use_stale error http_502 http_503;
```

```bash
proxy_cache_methods GET | HEAD | POST ...;
#对哪些客户端请求方法对应的响应进行缓存，GET和HEAD方法总是被缓存
```

扩展知识：清理缓存

```http
方法1: rm -rf 缓存目录
方法2: 第三方扩展模块ngx_cache_purge 
```

* 非缓存场景压测

```bash
#准备后端httpd服务器
[root@centos8 app1]# pwd
/var/www/html/static
[root@centos8 static]# cat /var/log/messages > ./log.html #准备测试页面
[root@centos8 ~]# ab -n 2000 -c200 http://www.magedu.org/static/log.html
Concurrency Level:      200
Time taken for tests:   15.363 seconds
Complete requests:      2000
Failed requests:        0
Write errors:           0
Total transferred:      1486898000 bytes
HTML transferred:       1486316000 bytes
Requests per second:    130.18 [#/sec] (mean)
Time per request:       1536.342 [ms] (mean)
Time per request:       7.682 [ms] (mean, across all concurrent requests)
Transfer rate:          94513.36 [Kbytes/sec] received
```

*  准备缓存配置

```bash
[root@centos8 ~]# vim /apps/nginx/conf/nginx.conf
proxy_cache_path /data/nginx/proxycache   levels=1:1:1  keys_zone=proxycache:20m 
inactive=120s  max_size=1g; #配置在nginx.conf http配置段
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf
 location /static {  #要缓存的URL 或者放在server配置项对所有URL都进行缓存
   proxy_pass http://10.0.0.18:80;
   proxy_cache proxycache;
   proxy_cache_key $request_uri;
   #proxy_cache_key $host$uri$is_args$args;
   proxy_cache_valid 200 302 301 10m;
   proxy_cache_valid any 5m;  #必须指定哪些响应码的缓存
   proxy_set_header clientip $remote_addr;
 }
[root@centos8 ~]# systemctl restart nginx
#/data/nginx/proxycache/ 目录会自动生成
[root@centos8 ~]#ll /data/nginx/proxycache/ -d
drwx------ 2 nginx root 6 Oct 10 11:30 /data/nginx/proxycache/
[root@centos8 ~]#tree /data/nginx/proxycache/
/data/nginx/proxycache/
0 directories, 0 files
```

* 访问并验证缓存文件

```bash
#访问web并验证缓存目录
[root@centos8 ~]# curl http://www.magedu.org/static/log.html
[root@centos8 ~]# ab -n 2000 -c200 http://www.magedu.org/static/log.html    
Concurrency Level:      200
Time taken for tests:   8.165 seconds
Complete requests:      2000
Failed requests:        0
Write errors:           0
Total transferred:      1486898000 bytes
HTML transferred:       1486316000 bytes
Requests per second:    244.96 [#/sec] (mean) #性能提高近一倍
Time per request:       816.476 [ms] (mean)
Time per request:       4.082 [ms] (mean, across all concurrent requests)
Transfer rate:          177843.44 [Kbytes/sec] received
    
#验证缓存目录结构及文件大小   
[root@centos8 ~]#tree /data/nginx/proxycache/
/data/nginx/proxycache/
└── d
   └── b
       └── e
           └── a971fce2cfaae636d54b5121d7a74ebd
3 directories, 1 file
[root@centos8 ~]#ll -h   
/data/nginx/proxycache/d/b/e/a971fce2cfaae636d54b5121d7a74ebd 
-rw------- 1 nginx nginx 727K Oct 10 11:33 
/data/nginx/proxycache/d/b/e/a971fce2cfaae636d54b5121d7a74ebd
#验证文件内容：
[root@centos8 ~]#file 
/data/nginx/proxycache/d/b/e/a971fce2cfaae636d54b5121d7a74ebd 
/data/nginx/proxycache/d/b/e/a971fce2cfaae636d54b5121d7a74ebd: Hitachi SH bigendian COFF object file, not stripped, 0 section, symbol offset=0x813a815f
[root@centos8 ~]#head -n20 
/data/nginx/proxycache/d/b/e/a971fce2cfaae636d54b5121d7a74ebd
#会在文件首部添加相应码
:_
 )_q,_棁gs"b56f6-5b14894970c60"
KEY: /static/log.html
HTTP/1.1 200 OK
Date: Sat, 10 Oct 2020 03:37:21 GMT
Server: Apache/2.4.37 (centos)
Last-Modified: Sat, 10 Oct 2020 03:22:52 GMT
ETag: "b56f6-5b14894970c60"
Accept-Ranges: bytes
Content-Length: 743158
Connection: close
Content-Type: text/html; charset=UTF-8
Jun 16 06:29:40 10 kernel: Linux version 4.18.0-193.el8.x86_64 
(mockbuild@kbuilder.bsys.centos.org) (gcc version 8.3.1 20191121 (Red Hat 8.3.1-
5) (GCC)) #1 SMP Fri May 8 10:59:10 UTC 2020
Jun 16 06:29:40 10 kernel: Command line: BOOT_IMAGE=(hd0,msdos1)/vmlinuz-4.18.0-
193.el8.x86_64 root=UUID=acf9bd1f-caae-4e28-87be-e53afec61347 ro 
resume=UUID=409e36d2-ac5e-423f-ad78-9b12db4576bd rhgb quiet
Jun 16 06:29:40 10 kernel: Disabled fast string operations
Jun 16 06:29:40 10 kernel: x86/fpu: Supporting XSAVE feature 0x001: 'x87 
floating point registers'
Jun 16 06:29:40 10 kernel: x86/fpu: Supporting XSAVE feature 0x002: 'SSE 
registers'
Jun 16 06:29:40 10 kernel: x86/fpu: Supporting XSAVE feature 0x004: 'AVX 
registers'
Jun 16 06:29:40 10 kernel: x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]: 
256
Jun 16 06:29:40 10 kernel: x86/fpu: Enabled xstate features 0x7, context size is 
832 bytes, using 'standard' format.
```

##### 6.1.1.5 添加响应报文的头部信息

nginx基于模块ngx_http_headers_module可以实现对后端服务器响应给客户端的报文中添加指定的响应首部字段 

参考链接: https://nginx.org/en/docs/http/ngx_http_headers_module.html

```bash
Syntax: add_header name value [always];
Default: —
Context: http, server, location, if in location
#添加响应报文的自定义首部：
add_header name value [always]; 
#示例:
add_header X-Via   $server_addr; #当前nginx主机的IP
add_header X-Cache $upstream_cache_status; #是否缓存命中
add_header X-Accel $server_name; #客户访问的FQDN
#添加自定义响应信息的尾部，使用较少,1.13.2版后支持 
add_trailer name value [always];
```

* Nginx 配置

```bash
 location /static {
   proxy_pass http://10.0.0.101:80/;
   proxy_cache proxycache;
   proxy_cache_key $request_uri;
   proxy_cache_valid 200 302 301 1h;
   proxy_cache_valid any 1m;
    
   proxy_set_header clientip $remote_addr;
   add_header X-Via  $server_addr;
   add_header X-Cache $upstream_cache_status;
   add_header X-Accel $server_name;
 }
```

* 验证头部信息

```bash
[root@centos7 ~]#curl http://www.magedu.org/static/log.html -I
HTTP/1.1 200 OK
Date: Sat, 10 Oct 2020 03:42:31 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 743158
Connection: keep-alive
Vary: Accept-Encoding
Server: Apache/2.4.37 (centos)
Last-Modified: Sat, 10 Oct 2020 03:22:52 GMT
ETag: "b56f6-5b14894970c60"
X-Via: 10.0.0.8
X-Cache: MISS #第一次无缓存 
X-Accel: www.magedu.org
Accept-Ranges: bytes
[root@centos7 ~]#curl http://www.magedu.org/static/log.html -I
HTTP/1.1 200 OK
Date: Sat, 10 Oct 2020 03:42:38 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 743158
Connection: keep-alive
Vary: Accept-Encoding
Server: Apache/2.4.37 (centos)
Last-Modified: Sat, 10 Oct 2020 03:22:52 GMT
ETag: "b56f6-5b14894970c60"
X-Via: 10.0.0.8
X-Cache: HIT                 #第二次命令缓存
X-Accel: www.magedu.org
Accept-Ranges: bytes
```

##### 6.1.1.6 实现反向代理客户端 IP 透传

* 一级代理实现客户端IP透传

```bash
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/pc.conf
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   index index.html index.php;
   root /data/nginx/html/pc;
   proxy_pass http://10.0.0.18;
    #proxy_set_header X-Real-IP $remote_addr;                   #只添加客户端IP到请求报文头部,转发至后端服务器
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; #添加客户端IP和反向代理服务器IP到请求报文头部
 }
}
#重启nginx
[root@centos8 ~]#systemctl restart nginx
#后端Apache配置：
[root@centos8 ~]#vim /etc/httpd/conf/httpd.conf
 LogFormat "%{X-Forwarded-For}i %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%
{User-Agent}i\"" combined
#重启apache访问web界面并验证apache日志
 [root@centos8 ~]#cat /var/log/httpd/access_log
10.0.0.1 10.0.0.8 - - [05/Mar/2019:00:40:46 +0800] "GET / HTTP/1.0" 200 19 "-"
"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/72.0.3626.119 Safari/537.36"
#Nginx配置：
[root@centos8 conf.d]# cat /apps/nginx/conf/nginx.conf
"$http_x_forwarded_for"' #默认日志格式就有此配置
#重启nginx访问web界面并验证日志格式：
10.0.0.8 - - [04/Mar/2019:16:40:51 +0800] "GET / HTTP/1.0" 200 24 "-"
"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) 
Chrome/72.0.3626.119 Safari/537.36" "10.0.0.1"
```

* 多级代理实现客户端 IP 透传

```bash
#第一个代理服务器
[root@nginx1 ~]#vim /apps/nginx/conf/nginx.conf 
#开启日志格式,记录x_forwarded_for
http {
   include       mime.types;
   default_type application/octet-stream;
   proxy_cache_path /data/nginx/proxycache   levels=1:1:1 
keys_zone=proxycache:20m inactive=120s  max_size=1g;
   log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
   access_log logs/access.log main;
#定义反向代理
[root@nginx1 ~]#vim /apps/nginx/conf/conf.d/pc.conf 
server {
   location / {
   proxy_pass http://10.0.0.18;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;                 
                              }
.......   
}
#第二个代理服务器
[root@nginx2 ~]#vim /apps/nginx/conf/nginx.conf 
#开启日志格式,记录x_forwarded_for
http {
   include       mime.types;
   default_type application/octet-stream;
   proxy_cache_path /data/nginx/proxycache   levels=1:1:1 
keys_zone=proxycache:20m inactive=120s  max_size=1g;
   log_format main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" "$http_x_forwarded_for"';
   access_log logs/access.log main;
#定义反向代理
[root@nginx2 ~]#vim /etc/nginx/nginx.conf
 server {
       location / {
         proxy_pass http://10.0.0.28;
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;           
                            
       }
       .....
}
#在第一个proxy上面查看日志
[root@nginx1 ~]#tail /apps/nginx/logs/access.log -f
10.0.0.7 - - [11/Oct/2020:14:37:00 +0800] "GET /index.html HTTP/1.1" 200 11 "-"
"curl/7.58.0" "-"
#在第二个proxy上面查看日志
[root@nginx2 ~]#tail /apps/nginx/logs/access.log -f
10.0.0.8 - - [11/Oct/2020:14:37:00 +0800] "GET /index.html HTTP/1.0" 200 11 "-"
"curl/7.58.0" "10.0.0.7"
#后端服务器配置日志格式
[root@centos8 ~]#vim /etc/httpd/conf/httpd.conf
LogFormat "\"%{x-Forwarded-For}i\" %h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%
{User-Agent}i\"" testlog
CustomLog "logs/access_log" testlog
#测试访问
[root@centos7 ~]#curl www.magedu.org/index.html
<h1> web site on 10.0.0.28 </h1>
#后端服务器查看日志
[root@centos8 ~]#tail -f /var/log/httpd/access_log
"10.0.0.7, 10.0.0.8"  10.0.0.18 - - [11/Oct/2020:14:37:00 +0800] "GET 
/index.html HTTP/1.0" 200 34 "-" "curl/7.29.0"
```

#### 6.1.2 http 反向代理负载均衡

官方文档： https://nginx.org/en/docs/http/ngx_http_upstream_module.html

##### 6.1.2.1 http upstream配置参数

```bash
#自定义一组服务器，配置在http块内
upstream name { 
 server .....
 ......
}
#示例
upstream backend {
   server backend1.example.com weight=5;
   server 127.0.0.1:8080       max_fails=3 fail_timeout=30s;
   server unix:/tmp/backend3;
   server backup1.example.com backup;
}
server address [parameters];
#配置一个后端web服务器，配置在upstream内，至少要有一个server服务器配置。
#server支持的parameters如下：
weight=number #设置权重，默认为1,实现类似于LVS中的WRR,WLC等
max_conns=number  #给当前后端server设置最大活动链接数，默认为0表示没有限制
max_fails=number  #后端服务器的下线条件,当客户端访问时,对本次调度选中的后端服务器连续进行检测
多少次,如果都失败就标记为不可用,默认为1次,当客户端访问时,才会利用TCP触发对探测后端服务器健康性
检查,而非周期性的探测
fail_timeout=time #后端服务器的上线条件,对已经检测到处于不可用的后端服务器,每隔此时间间隔再次
进行检测是否恢复可用，如果发现可用,则将后端服务器参与调度,默认为10秒
backup  #设置为备份服务器，当所有后端服务器不可用时,才会启用此备用服务器
down    #标记为down状态,可以平滑下线后端服务器
resolve #当server定义的是主机名的时候，当A记录发生变化会自动应用新IP而不用重启Nginx
```

```bash
hash KEY [consistent];
#基于指定请求报文中首部字段或者URI等key做hash计算，使用consistent参数，将使用ketama一致性hash算法，适用于后端是Cache服务器（如varnish）时使用，consistent定义使用一致性hash运算，一致性hash基于取模运算
hash $request_uri consistent; #基于用户请求的uri做hash
hash $cookie_sessionid  #基于cookie中的sessionid这个key进行hash调度,实现会话绑定
```

```bash
ip_hash;
#源地址hash调度方法，基于的客户端的remote_addr(源地址IPv4的前24位或整个IPv6地址)做hash计算，以实现会话保持
```

```bash
least_conn;
#最少连接调度算法，优先将客户端请求调度到当前连接最少的后端服务器,相当于LVS中的WLC
```

##### 6.1.2.2 反向代理示例: 后端多台 web服务器

环境说明：

```bash
10.0.0.100  #Nginx 代理服务器
10.0.0.101  #后端web A，Apache部署
10.0.0.102  #后端web B，Apache部署
```

* 部署后端 Apache服务器

```bash
[root@centos8 ~]# yum install httpd -y
[root@centos8 ~]# echo "web1 10.0.0.101" > /var/www/html/index.html
[root@centos8 ~]# systemctl enable --now httpd
[root@centos8 ~]# yum install httpd -y
[root@centos8 ~]# echo "web2 10.0.0.102" >> /var/www/html/index.html
[root@centos8 ~]# systemctl enable --now httpd
#访问测试
[root@centos8 ~]# curl http://10.0.0.101
web1 10.0.0.101
[root@centos8 ~]# curl http://10.0.0.102
web2 10.0.0.102
```

* 配置 nginx 反向代理

```bash
# 注意: 本节实验过程中先关闭缓存
[root@centos8 ~]# cat /apps/nginx/conf/conf.d/pc.conf
http {
 upstream webserver {
  #hash $request_uri consistent;
  #hash $cookie_sessionid
  #ip_hash;
  #least_conn;
 server 10.0.0.101:80 weight=1 fail_timeout=5s max_fails=3; #后端服务器状态监测
 server 10.0.0.102:80 weight=1 fail_timeout=5s max_fails=3;
  #server 127.0.0.1:80 weight=1 fail_timeout=5s max_fails=3 backup;
 }
server {
 listen 80;
 server_name www.magedu.org;
 location / {
   index index.html index.php;
   root /data/nginx/html/pc;
   }
 location /web {
   index index.html;
   proxy_pass http://webserver/;
   proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | 
http_503 | http_504;
 }
}
#重启Nginx 并访问测试
[root@centos8 ~]# curl   http://www.magedu.org/web
web1 10.0.0.101
[root@centos8 ~]# curl http://www.magedu.org/web
web2 10.0.0.102
#关闭10.0.0.101和10.0.0.102，测试nginx backup服务器可用性：
[root@centos8 ~]# while true;do curl http://www.magedu.org/web;sleep 1;done
```

##### 6.1.2.3 基于Cookie 实现会话绑定

```bash
[root@centos8 ~]#vim /apps/nginx/conf/nginx.conf
http {
 upstream websrvs {
 hash $cookie_hello;  #hello是cookie的key的名称
 server 10.0.0.101:80 weight=2;
 server 10.0.0.102:80 weight-1;
 
 }
}
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf
server {
   location /{
 #proxy_cache mycache;
 proxy_pass http://websrvs;
   }
}
#测试
[root@centos8 ~]#curl -b hello=wang http://www.magedu.org/
```

### 6.2  Nginx 四层负载均衡

Nginx在1.9.0版本开始支持tcp模式的负载均衡，在1.9.13版本开始支持udp协议的负载，udp主要用于 DNS的域名解析，其配置方式和指令和http 代理类似，其基于ngx_stream_proxy_module模块实现tcp 负载，另外基于模块ngx_stream_upstream_module实现后端服务器分组转发、权重分配、状态监测、 调度算法等高级功能。

如果编译安装,需要指定 --with-stream 选项才能支持ngx_stream_proxy_module模块

官方文档： 

https://nginx.org/en/docs/stream/ngx_stream_proxy_module.html

http://nginx.org/en/docs/stream/ngx_stream_upstream_module.html

#### 6.2.1 tcp负载均衡配置参数

```bash
stream { #定义stream相关的服务；Context:main
   upstream backend { #定义后端服务器
       hash $remote_addr consistent; #定义调度算法
       server backend1.example.com:12345 weight=5; #定义具体server
       server 127.0.0.1:12345      max_fails=3 fail_timeout=30s;
       server unix:/tmp/backend3;
   }
   upstream dns {  #定义后端服务器
       server 10.0.0.1:53535;  #定义具体server
       server dns.example.com:53;
   }
   server { #定义server
       listen 12345; #监听IP:PORT
       proxy_connect_timeout 1s; #连接超时时间
       proxy_timeout 3s; #转发超时时间
       proxy_pass backend; #转发到具体服务器组
   }
   server {
       listen 127.0.0.1:53 udp reuseport;
       proxy_timeout 20s;
       proxy_pass dns;
   }
   server {
       listen [::1]:12345;
       proxy_pass unix:/tmp/stream.socket;
   }
}
```

#### 6.2.2 负载均衡实例 : Redis

* 后端服务器安装 redis

```bash
#安装两台redis服务器
[root@centos8 ~]# yum -y install redis 
[root@centos8 ~]# sed -i '/^bind /c bind 0.0.0.0' /etc/redis.conf
[root@centos8 ~]# systemctl enable --now redis
[root@centos8 ~]# ss -tnl | grep 6379
LISTEN     0      128         *:6379                     *:*
```

* nginx 配置

```bash
[root@centos8 ~]# vim /apps/nginx/conf/nginx.conf
include /apps/nginx/conf/tcp/tcp.conf; #注意此处的include与http模块平级
[root@centos8 ~]# mkdir /apps/nginx/conf/tcp
[root@centos8 ~]# cat /apps/nginx/conf/tcp/tcp.conf
stream {
 upstream redis_server {
    #hash $remote_addr consistent;
   server 10.0.0.18:6379 max_fails=3 fail_timeout=30s;
   server 10.0.0.28:6379 max_fails=3 fail_timeout=30s;
 }
 server {
   listen 10.0.0.8:6379;
   proxy_connect_timeout 3s;
   proxy_timeout 3s;
   proxy_pass redis_server;
 }
}
#重启nginx并访问测试
[root@centos8 ~]# systemctl restart nginx
[root@centos8 ~]# ss -tnl | grep 6379
LISTEN     0      128    10.0.0.8:6379                     *:*  
#测试通过nginx 负载连接redis：
[root@centos8 ~]#redis-cli -h 10.0.0.8 set name wang 
OK
[root@centos8 ~]#redis-cli -h 10.0.0.8 get name
(nil)
[root@centos8 ~]#redis-cli -h 10.0.0.8 get name
"wang"
```

#### 6.2.3 负载均衡实例: MySQL

* 后端服务器安装 MySQL

```bash
#先修改后端两个服务器的主机名,方便测试
[root@centos8 ~]#hostnamectl set-hostname mysql-server1.magedu.org
[root@centos8 ~]#hostnamectl set-hostname mysql-server2.magedu.org
#安装MySQL服务
[root@centos8 ~]# yum -y install mariadb-server 
[root@centos8 ~]# systemctl enable --now mariadb
MariaDB [(none)]> create user wang@'10.0.0.%' identified by 'magedu';
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> exit
```

* nginx配置

```bash
[root@centos8 ~]# cat /apps/nginx/conf/tcp/tcp.conf 
stream {
   upstream redis_server {
     server 10.0.0.18:6379 max_fails=3 fail_timeout=30s;
     server 10.0.0.28:6379 max_fails=3 fail_timeout=30s;
      #server 10.0.0.28:6379 max_fails=3 fail_timeout=30s backup;
   }
   upstream mysql_server {
     least_conn;
     server 10.0.0.18:3306 max_fails=3 fail_timeout=30s;
   }
###################################################################
 server {
   listen 10.0.0.8:3306;
   proxy_connect_timeout 6s;
   proxy_timeout 15s;
   proxy_pass mysql_server;
   }
 server {
   listen 10.0.0.8:6379;
   proxy_connect_timeout 3s;
   proxy_timeout 3s;
   proxy_pass redis_server;
   }
}
#重启nginx并访问测试：
[root@centos8 ~]# systemctl restart nginx
#测试通过nginx负载连接MySQL：
[root@centos8 ~]#mysql -uwang -pmagedu -h10.0.0.8 -e 'show variables like "hostname"'
+---------------+-------------------------+
| Variable_name | Value                   |
+---------------+-------------------------+
| hostname     | mysql-server1.magedu.org |
+---------------+-------------------------+
[root@centos8 ~]#mysql -uwang -pmagedu -h10.0.0.8 -e 'show variables like "hostname"'
+---------------+-------------------------+
| Variable_name | Value                   |
+---------------+-------------------------+
| hostname     | mysql-server2.magedu.org |
+---------------+-------------------------+
#在10.0.0.28停止MySQL服务
[root@centos8 ~]#systemctl stop mysqld
#再次测试访问,只会看到mysql-server1.magedu.org进行响应
[root@centos8 ~]#mysql -uwang -pmagedu -h10.0.0.8 -e 'show variables like "hostname"'
+---------------+-------------------------+
| Variable_name | Value                   |
+---------------+-------------------------+
| hostname     | mysql-server1.magedu.org |
+---------------+-------------------------+
```

#### 6.2.4 udp 负载均衡实例: DNS

```bash
stream { 
 upstream dns_servers { 
 server 10.0.0.7:53; 
 server 10.0.0.17:53; 
   } 
   server { 
   listen 53 udp; 
   proxy_pass dns_servers; 
   proxy_timeout 1s; 
   proxy_responses 1;  #使用UDP协议时，设置代理服务器响应客户端期望的数据报文数。该值
作为会话的终止条件
   error_log logs/dns.log; 
   } 
}
```

### 6.3  实现 FastCGI

CGI的由来： 最早的Web服务器只能简单地响应浏览器发来的HTTP请求，并将存储在服务器上的HTML文件返回给浏 览器，也就是静态html文件，但是后期随着网站功能增多网站开发也越来越复杂，以至于出现动态技 术，比如像php(1995年)、java(1995)、python(1991)语言开发的网站，但是nginx/apache服务器并不 能直接运行 php、java这样的文件，apache实现的方式是打补丁，但是nginx缺通过与第三方基于协议 实现，即通过某种特定协议将客户端请求转发给第三方服务处理，**第三方服务器会新建新的进程处理用户的请求，处理完成后返回数据给Nginx并回收进程**，最后nginx在返回给客户端，那这个约定就是通用 网关接口(common gateway interface，简称CGI)，CGI（协议） 是web服务器和外部应用程序之间的 接口标准，是cgi程序和web服务器之间传递信息的标准化接口。

为什么会有FastCGI？ 

CGI协议虽然解决了语言解析器和 Web Server 之间通讯的问题，但是它的效率很低，因为 Web Server 每收到一个请求都会创建一个CGI进程，PHP解析器都会解析php.ini文件，初始化环境，请求结束的时 候再关闭进程，对于每一个创建的CGI进程都会执行这些操作，所以效率很低，而FastCGI是用来提高 CGI性能的，FastCGI每次处理完请求之后不会关闭掉进程，而是保留这个进程，使这个进程可以处理多 个请求。这样的话每个请求都不用再重新创建一个进程了，大大提升了处理效率。

什么是PHP-FPM？ 

PHP-FPM(FastCGI Process Manager：FastCGI进程管理器)是一个实现了Fastcgi的程序，并且提供进程 管理的功能。进程包括master进程和worker进程。master进程只有一个，负责监听端口，接受来自 web server的请求。worker进程一般会有多个，每个进程中会嵌入一个PHP解析器，进行PHP代码的处理。

#### 6.3.1 FastCGI配置指令

Nginx基于模块ngx_http_fastcgi_module实现通过fastcgi协议将指定的客户端请求转发至php-fpm处 理，其配置指令如下：

```bash
fastcgi_pass address:port;
#转发请求到后端服务器，address为后端的fastcgi server的地址，可用位置：location, if in location
fastcgi_index name;
#fastcgi默认的主页资源，示例：fastcgi_index index.php;
fastcgi_param parameter value [if_not_empty];
#设置传递给FastCGI服务器的参数值，可以是文本，变量或组合，可用于将Nginx的内置变量赋值给自定义key
fastcgi_param REMOTE_ADDR        $remote_addr; #客户端源IP
fastcgi_param REMOTE_PORT        $remote_port; #客户端源端口
fastcgi_param SERVER_ADDR        $server_addr; #请求的服务器IP地址
fastcgi_param SERVER_PORT        $server_port; #请求的服务器端口
fastcgi_param SERVER_NAME        $server_name; #请求的server name
Nginx默认配置示例：
   location ~ \.php$ {
     root           /scripts;
     fastcgi_pass   127.0.0.1:9000;
     fastcgi_index index.php;
     fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name; #默认脚本路径
     #fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
     include       fastcgi_params;    #此文件默认系统已提供,存放的相对路径为prefix/conf
   }
```

fastcgi 缓存定义指令：注意使用fastcgi缓存, 可能会导致源代码更新失败,生产慎用

```bash
fastcgi_cache_path path [levels=levels] [use_temp_path=on|off] 
keys_zone=name:size [inactive=time] [max_size=size] [manager_files=number] 
[manager_sleep=time] [manager_threshold=time] [loader_files=number] 
[loader_sleep=time] [loader_threshold=time] [purger=on|off] 
[purger_files=number] [purger_sleep=time] [purger_threshold=time];
#定义fastcgi的缓存;
	path   #缓存位置为磁盘上的文件系统路径
	max_size=size #磁盘path路径中用于缓存数据的缓存空间上限
	levels=levels：缓存目录的层级数量，以及每一级的目录数量，levels=ONE:TWO:THREE，示例：leves=1:2:2
	keys_zone=name:size #设置缓存名称及k/v映射的内存空间的名称及大小
	inactive=time #缓存有效时间，默认10分钟，需要在指定时间满足fastcgi_cache_min_uses 次数被视为活动缓存。
```

缓存调用指令：

```bash
fastcgi_cache zone | off; 
#调用指定的缓存空间来缓存数据，可用位置：http, server, location
fastcgi_cache_key string; 
#定义用作缓存项的key的字符串，示例：fastcgi_cache_key $request_uri;
fastcgi_cache_methods GET | HEAD | POST ...; 
#为哪些请求方法使用缓存 
fastcgi_cache_min_uses number; 
#缓存空间中的缓存项在inactive定义的非活动时间内至少要被访问到此处所指定的次数方可被认作活动项
fastcgi_keep_conn on | off;  
#收到后端服务器响应后，fastcgi服务器是否关闭连接，建议启用长连接
fastcgi_cache_valid [code ...] time; 
#不同的响应码各自的缓存时长
fastcgi_hide_header field; #隐藏响应头指定信息
fastcgi_pass_header field; #返回响应头指定信息，默认不会将Status、X-Accel-...返回
```

#### 6.3.2 FastCGI实战案例 : Nginx与php-fpm在同一服务器

php安装可以通过yum或者编译安装，使用yum安装相对比较简单，编译安装更方便自定义参数或选项。

* php 环境准备

使用base源自带的php版本

```bash
#yum安装默认版本php和相关APP依赖的包
[root@centos8 ~]# yum -y install php-fpm php-mysqlnd php-json #默认版本
#或者安装清华的php源
[root@centos7 ~]# yum install 
https://mirrors.tuna.tsinghua.edu.cn/remi/enterprise/remi-release-7.rpm
[root@centos8 ~]# systemctl enable --now php-fpm
[root@centos8 ~]# ps -ef | grep php-fpm
root       4925      1  0 17:13 ?        00:00:00 php-fpm: master process 
(/etc/php-fpm.conf)
apache     4927   4925  0 17:13 ?        00:00:00 php-fpm: pool www
apache     4928   4925  0 17:13 ?        00:00:00 php-fpm: pool www
apache     4929   4925  0 17:13 ?        00:00:00 php-fpm: pool www
apache     4930   4925  0 17:13 ?        00:00:00 php-fpm: pool www
apache     4931   4925  0 17:13 ?        00:00:00 php-fpm: pool www
root       4933   3235  0 17:13 pts/0    00:00:00 grep --color=auto php-fpm
```

* php相关配置优化

```bash
[root@centos8 ~]# grep "^[a-Z]" /etc/php-fpm.conf 
include=/etc/php-fpm.d/*.conf
pid = /run/php-fpm/php-fpm.pid
error_log = /var/log/php-fpm/error.log
daemonize = yes #是否后台启动
[root@centos8 ~]#grep -Ev '^;.*$|^ *$' /etc/php-fpm.d/www.conf 
[www]
user = nginx
group = nginx
listen = /run/php-fpm/www.sock   #指定使用UDS,或者使用下面形式
;listen = 127.0.0.1:9000  #监听地址及IP
listen.acl_users = apache,nginx
listen.allowed_clients = 127.0.0.1
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
pm.status_path = /pm_status  #修改此行
ping.path = /ping            #修改此行
ping.response = ping-pong    #修改此行
slowlog = /var/log/php-fpm/www-slow.log  #慢日志路径
php_admin_value[error_log] = /var/log/php-fpm/www-error.log #错误日志
php_admin_flag[log_errors] = on
php_value[session.save_handler] = files  #phpsession保存方式及路径
php_value[session.save_path]    = /var/lib/php/session #当时使用file保存session的文
件路径
php_value[soap.wsdl_cache_dir]  = /var/lib/php/wsdlcache
#修改配置文件后记得重启php-fpm
[root@centos8 ~]# systemctl restart php-fpm
```

* 准备php测试页面

```bash
[root@centos8 ~]# mkdir -p /data/php
[root@centos8 ~]# cat /data/php/index.php #php测试页面
<?php
phpinfo();
?>
```

* Nginx配置转发

Nginx安装完成之后默认生成了与fastcgi的相关配置文件，一般保存在nginx的安装路径的conf目录当 中，比如/apps/nginx/conf/fastcgi.conf、/apps/nginx/conf/fastcgi_params。

```bash
[root@centos8 ~]# vim /apps/nginx/conf/conf.d/pc.conf #在指定文件配置fastcgi
 location ~ \.php$|pm_status|ping {
   root           /data/php; #下面的$document_root调用此行的root指令指定的目录
   fastcgi_pass   unix:/run/php-fpm/www.sock;
    #fastcgi_pass   127.0.0.1:9000;
    fastcgi_index index.php;
    #fastcgi_param SCRIPT_FILENAME /data/php$fastcgi_script_name;
    #如果SCRIPT_FILENAME是上面的绝对路径则可以省略root /data/php;
    fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include       fastcgi_params;
 }
#重启Nginx并访问web测试  
[root@centos8 ~]# systemctl restart nginx
```

* 访问验证php测试页面

```bash
#常见的错误：
File not found.  #路径不对
502  #php-fpm处理超时、服务停止运行等原因导致的无法连接或请求超时
```

* php-fpm 的运行状态页面

访问配置文件里面指定的路径，会返回php-fpm的当前运行状态。

```
# Nginx配置：
location ~ ^/(ping|pm_status)$ {
   include fastcgi_params;
   fastcgi_pass unix:/run/php-fpm/www.sock;
    #fastcgi_pass 127.0.0.1:9000;
   fastcgi_param PATH_TRANSLATED $document_root$fastcgi_script_name;
}
#重启Nginx并测试：
```

```bash
[root@centos8 ~]#curl http://www.magedu.org/ping
ping-pong
[root@centos8 ~]#curl http://www.magedu.org/pm_status
pool:                 www
process manager:     dynamic
start time:           10/Oct/2020:00:04:43 +0800
start since:          155
accepted conn:        5
listen queue:         0
max listen queue:     0
listen queue len:     0
idle processes:       4
active processes:     1
total processes:      5
max active processes: 1
max children reached: 0
slow requests:        0
[root@centos8 ~]# curl http://www.magedu.org/pm_status?full
[root@centos8 ~]# curl http://www.magedu.org/pm_status?html
[root@centos8 ~]# curl http://www.magedu.org/pm_status?json
```

#### 6.3.3 FastCGI实战案例 : Nginx与php不在同一个服务器

nginx会处理静态请求，但是会转发动态请求到后端指定的php-fpm服务器，因此php代码需要放在后端的php-fpm服务器，即静态页面放在Nginx服务器上,而动态页面放在后端php-fpm服务器，通常情况下，一般都是采用在同一个服务器

* yum安装较新版本php-fpm

```bash
[root@centos8 ~]#dnf -y install php-fpm php-mysqlnd php-json
#验证安装路径：
[root@centos8 ~]#rpm -ql php-fpm
/etc/httpd/conf.d/php.conf
/etc/logrotate.d/php-fpm
/etc/nginx/conf.d/php-fpm.conf
/etc/nginx/default.d/php.conf
/etc/php-fpm.conf
/etc/php-fpm.d
/etc/php-fpm.d/www.conf
/etc/systemd/system/php-fpm.service.d
/run/php-fpm
/usr/lib/.build-id
/usr/lib/.build-id/bf
/usr/lib/.build-id/bf/8d6b2bca109fb20f8037d72220670f2d6cc954
/usr/lib/systemd/system/httpd.service.d/php-fpm.conf
/usr/lib/systemd/system/nginx.service.d/php-fpm.conf
/usr/lib/systemd/system/php-fpm.service
/usr/sbin/php-fpm
/usr/share/doc/php-fpm
/usr/share/doc/php-fpm/php-fpm.conf.default
/usr/share/doc/php-fpm/www.conf.default
/usr/share/fpm
/usr/share/fpm/status.html
/usr/share/licenses/php-fpm
/usr/share/licenses/php-fpm/fpm_LICENSE
/usr/share/man/man8/php-fpm.8.gz
/var/lib/php/opcache
/var/lib/php/session
/var/lib/php/wsdlcache
/var/log/php-fpm
```

* 修改php-fpm监听配置

```bash
#php-fpm默认监听在127.0.0.1的9000端口，也就是无法远程连接，因此要做相应的修改。
#修改监听配置
[root@centos8 ~]#vim /etc/php-fpm.d/www.conf 
;listen = /run/php-fpm/www.sock #注释此行
listen = 9000  #修改此行,指定监听端口
;listen.allowed_clients = 127.0.0.1  #注释此行
```

* 准备php测试页面

```bash
#准备php数据目录
[root@centos8 ~]# mkdir -p /data/php 
[root@centos8 ~]# vim /data/php/index.php
<?php
 phpinfo();
?>
```

* 启动并验证php-fpm

```bash
#启动php-fpm
[root@centos8 ~]#systemctl enable --now php-fpm.service
#验证php-fpm进程及端口：
[root@centos8 ~]#ps -ef | grep php-fpm
root        1506       1  0 23:24 ?        00:00:00 php-fpm: master process 
(/etc/php-fpm.conf)
apache      1507    1506  0 23:24 ?        00:00:00 php-fpm: pool www
apache      1508    1506  0 23:24 ?        00:00:00 php-fpm: pool www
apache      1509    1506  0 23:24 ?        00:00:00 php-fpm: pool www
apache      1510    1506  0 23:24 ?        00:00:00 php-fpm: pool www
apache      1511    1506  0 23:24 ?        00:00:00 php-fpm: pool www
root        1517    1102  0 23:25 pts/0    00:00:00 grep --color=auto php-fpm
[root@centos8 ~]#ss -tnl
State             Recv-Q           Send-Q   Local Address:Port Peer 
Address:Port           
LISTEN            0                 100           127.0.0.1:25         0.0.0.0:* 
             
LISTEN            0                 128             0.0.0.0:22         0.0.0.0:* 
             
LISTEN            0                 100               [::1]:25           [::]:* 
             
LISTEN            0                 128                   *:9000             *:* 
             
LISTEN            0                 128               [::]:22           [::]:* 
  
[root@centos8 ~]#lsof -i :9000
COMMAND PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
php-fpm 1506   root   9u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
php-fpm 1507 apache   11u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
php-fpm 1508 apache   11u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
php-fpm 1509 apache   11u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
php-fpm 1510 apache   11u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
php-fpm 1511 apache   11u IPv6  33898     0t0 TCP *:cslistener (LISTEN)
```

* Nginx配置转发

```bash
[root@centos8 ~]#vi /apps/nginx/conf/conf.d/pc.conf 
 location ~ \.php$ {
   root           /data/php;
   fastcgi_pass   10.0.0.18:9000;
   fastcgi_index index.php;
    #fastcgi_param SCRIPT_FILENAME /data/php$fastcgi_script_name;
   fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
   include       fastcgi_params;
 }
 location ~ ^/(ping|pm_status)$ {
   include fastcgi_params;
   fastcgi_pass 10.0.0.18:9000;
   fastcgi_param PATH_TRANSLATED $document_root$fastcgi_script_name;
   }
#重启nginx
[root@centos8 ~]# systemctl restart nginx
```

* 访问验证php测试页面

#### 6.3.4 实现动静分离

要求：将客户端对除php以外的资源的访问转发至后端服务器 10.0.0.28上

*  配置 nginx 实现反向代理的动静分离

```bash
[root@centos8 ~]#vi /apps/nginx/conf/conf.d/pc.conf 
 location / {
   proxy_pass http://10.0.0.28;
   index index.html;
 }
 location ~ \.php$ {
   root /data/php;
   fastcgi_pass   10.0.0.18:9000;
   fastcgi_index index.php;
    #fastcgi_param SCRIPT_FILENAME /data/php$fastcgi_script_name;
   fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
   include       fastcgi_params;
 } 
```

* 准备后端 httpd 服务器

```bash
#在后端服务器10.0.0.28上安装httpd服务
[root@centos8 ~]#dnf -y install httpd
[root@centos8 ~]#systemctl enable --now httpd
[root@centos8 ~]#mkdir /var/www/html/images
[root@centos8 ~]#wget -O /var/www/html/images/magedu.jpg 
http://www.magedu.com/wp-content/uploads/2019/05/2019052306372726.jpg
```

#### 6.3.5 项目实战 : 利用 LNMP 实现WordPress站点搭建

LNMP项目实战环境说明

```http
L：Linux（CentOS7）https://mirrors.aliyun.com/centos/7/isos/x86_64/
N：Nginx（1.18.0） https://nginx.org/en/download.html
M：MySQL（8.0.19） https://dev.mysql.com/downloads/mysql/
P：PHP（7.4.10） http://php.net/downloads.php
Wordpress（5.4.2）：https://cn.wordpress.org/download/
#部署规划：
10.0.0.7：Nginx php-fpm 运行web服务
10.0.0.17：运行MySQL数据库,Redis服务
```

##### 6.3.5.1 部署数据库

在10.0.0.17主机部署MySQL服务

* 二进制部署MySQL数据库

```bash
[root@centos7 ~]#ll 
total 473728
-rw-r--r--  1 root root      2433 Sep  8 23:09 install_mysql5.7or8.0_for_centos.sh
-rw-r--r--  1 root root 485074552 Aug 22 23:40 mysql-8.0.19-linux-glibc2.12-x86_64.tar.xz
#一键安装脚本
[root@centos7 ~]#cat install_mysql5.7or8.0_for_centos.sh 
#!/bin/bash
#
#********************************************************************
#Author: wangxiaochun
#QQ: 29308620
#Date: 2020-02-12
#FileName： install_mysql5.7_for_centos.sh
#URL: http://www.magedu.com
#Description： The test script
#Copyright (C): 2020 All rights reserved
#********************************************************************
#MySQL Download URL: https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.29-
linux-glibc2.12-x86_64.tar.gz
. /etc/init.d/functions 
SRC_DIR=`pwd`
MYSQL='mysql-8.0.19-linux-glibc2.12-x86_64.tar.xz'
COLOR='echo -e \E[01;31m'
END='\E[0m'
MYSQL_ROOT_PASSWORD=magedu
check (){
if [ $UID -ne 0 ]; then
 action "当前用户不是root,安装失败" false
  exit 1
fi
cd  $SRC_DIR
if [ !  -e $MYSQL ];then
        $COLOR"缺少${MYSQL}文件"$END
 $COLOR"请将相关软件放在${SRC_DIR}目录下"$END
        exit
elif [ -e /usr/local/mysql ];then
       action "数据库已存在，安装失败" false
        exit
else
 return
fi
} 
install_mysql(){
    $COLOR"开始安装MySQL数据库..."$END
 yum  -y -q install libaio numactl-libs   libaio &> /dev/null
    cd $SRC_DIR
    tar xf $MYSQL -C /usr/local/
    MYSQL_DIR=`echo $MYSQL| sed -nr 's/^(.*[0-9]).*/\1/p'`
    ln -s /usr/local/$MYSQL_DIR /usr/local/mysql
    chown -R root.root /usr/local/mysql/
    id mysql &> /dev/null || { useradd -s /sbin/nologin -r mysql ; action "创建mysql用户"; }
        
    echo 'PATH=/usr/local/mysql/bin/:$PATH' > /etc/profile.d/mysql.sh
   . /etc/profile.d/mysql.sh
 ln -s /usr/local/mysql/bin/* /usr/bin/
    cat > /etc/my.cnf <<-EOF
[mysqld]
server-id=`hostname -I|cut -d. -f4`
log-bin
datadir=/data/mysql
socket=/data/mysql/mysql.sock                                                   
                                                
log-error=/data/mysql/mysql.log
pid-file=/data/mysql/mysql.pid
[client]
socket=/data/mysql/mysql.sock
EOF
   mysqld --initialize --user=mysql --datadir=/data/mysql 
   cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
   chkconfig --add mysqld
   chkconfig mysqld on
   service mysqld start
   [ $? -ne 0 ] && { $COLOR"数据库启动失败，退出!"$END;exit; }
   MYSQL_OLDPASSWORD=`awk '/A temporary password/{print $NF}' /data/mysql/mysql.log`
   mysqladmin  -uroot -p$MYSQL_OLDPASSWORD password $MYSQL_ROOT_PASSWORD &>/dev/null
   action "数据库安装完成"
}
check
install_mysql
#运行脚本安装数据库
```

* 创建wordpress数据库和用户并授权

```bash
[root@centos7 ~]#mysql -uroot -pmagedu
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.19 MySQL Community Server - GPL
Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> create database wordpress;
Query OK, 1 row affected (0.01 sec)
mysql> create user wordpress@'10.0.0.%' identified by '123456';
Query OK, 0 rows affected (0.01 sec)
mysql> grant all on wordpress.* to wordpress@'10.0.0.%';
Query OK, 0 rows affected (0.01 sec)
```

* 验证MySQL账户权限

```bash
#在WordPress服务器使用授权的MySQL账户远程登录测试权限
[root@centos7 ~]#mysql -uwordpress -p123456 -h10.0.0.17 
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.0.19 MySQL Community Server - GPL
Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| wordpress          |
+--------------------+
2 rows in set (0.00 sec)
```

##### 6.3.5.2 部署PHP

在10.0.0.7主机部署php-fpm服务

* 编译安装 php

```bash
[root@centos7 ~]#yum -y install gcc openssl-devel libxml2-devel bzip2-devel 
libmcrypt-devel sqlite-devel oniguruma-devel
[root@centos7 ~]#cd /usr/local/src
[root@centos7 src]#wget https://www.php.net/distributions/php-7.4.11.tar.xz
[root@centos7 src]#ll -h php-7.4.11.tar.xz 
-rw-r--r-- 1 root root 9.9M Sep 29 18:40 php-7.4.11.tar.xz
[root@centos7 php-7.4.11]#./configure --prefix=/apps/php74 --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-openssl   --with-zlib --with-config-file-path=/etc --with-config-file-scan-dir=/etc/php.d --enable-mbstring --enable-xml --enable-sockets --enable-fpm --enable-maintainer-zts --disable-fileinfo
......
config.status: creating main/php_config.h
config.status: executing default commands
+--------------------------------------------------------------------+
| License:                                                           |
| This software is subject to the PHP License, available in this     |
| distribution in the file LICENSE. By continuing this installation |
| process, you are bound by the terms of this license agreement.     |
| If you do not agree with the terms of this license, you must abort |
| the installation process at this point.                           |
+--------------------------------------------------------------------+
Thank you for using PHP.
[root@centos7 php-7.4.11]#make -j 8 && make install
```

* 准备 php 配置文件

```bash
#生成配置文件
[root@centos7 php-7.4.11]#cp /usr/local/src/php-7.4.11/php.ini-production 
/etc/php.ini
[root@centos7 php-7.4.11]#cd /apps/php74/etc
[root@centos7 etc]#cp php-fpm.conf.default php-fpm.conf
[root@centos7 php-7.4.11]#cd php-fpm.d/
[root@centos7 php-fpm.d]#cp www.conf.default www.conf
[root@centos7 php-fpm.d]#vim www.conf
[root@centos7 php-fpm.d]#grep '^[^;]' www.conf
[www]
user = www
group = www
listen = 127.0.0.1:9000
pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
pm.status_path = /pm_status
ping.path = /ping
access.log = log/$pool.access.log
slowlog = log/$pool.log.slow
#创建用户
[root@centos7 php-fpm.d]#useradd -r -s /sbin/nologin www
#创建访问日志文件路径
[root@centos7 php-fpm.d]#mkdir /apps/php74/log
```

* 启动并验证 php-fpm 服务

```bash
[root@centos7 ~]#/apps/php74/sbin/php-fpm -t
[22-Oct-2020 10:55:19] NOTICE: configuration file /apps/php74/etc/php-fpm.conf 
test is successful
[root@centos7 ~]#cp /usr/local/src/php-7.4.11/sapi/fpm/php-fpm.service 
/usr/lib/systemd/system/
             
[root@centos7 ~]#systemctl daemon-reload
[root@centos7 ~]#systemctl enable --now php-fpm
Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service 
to /usr/lib/systemd/system/php-fpm.service.
[root@centos7 php-7.4.11]#ss -ntl
State     Recv-Q Send-Q                     Local Address:Port                 
                    Peer Address:Port           
LISTEN     0      128                                     *:22                   
                              *:*             
LISTEN     0      100                             127.0.0.1:25                   
                              *:*             
LISTEN     0      128                             127.0.0.1:9000                 
                              *:*             
LISTEN     0      128                                 [::]:22                   
                            [::]:*             
LISTEN     0      100                                 [::1]:25                   
                            [::]:*  
[root@centos7 ~]#pstree -p |grep php
           |-php-fpm(16331)-+-php-fpm(16332)
           |                `-php-fpm(16333)
[root@centos7 ~]#ps -ef |grep php
root      16331      1  0 12:46 ?        00:00:00 php-fpm: master process 
(/apps/php74/etc/php-fpm.conf)
www       16332  16331  0 12:46 ?        00:00:00 php-fpm: pool www
www       16333  16331  0 12:46 ?        00:00:00 php-fpm: pool www
root      16403   1095  0 13:12 pts/0    00:00:00 grep --color=auto php
```

##### 6.3.5.3 部署 Nginx

在10.0.0.7主机部署nginx服务

* 编译安装 nginx

```bash
[root@centos7 ~]#yum -y install gcc pcre-devel openssl-devel zlib-devel
[root@centos7 ~]#cd /usr/local/src/
[root@centos7 src]#wget http://nginx.org/download/nginx-1.18.0.tar.gz
[root@centos7 src]#tar xf nginx-1.18.0.tar.gz 
[root@centos7 src]#cd nginx-1.18.0/
[root@centos7 nginx-1.18.0]#./configure --prefix=/apps/nginx \
--user=www \
--group=www \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-pcre \
--with-stream \
--with-stream_ssl_module \
--with-stream_realip_module
[root@centos7 nginx-1.18.0]#make && make install
```

* 准备服务文件并启动 nginx

```bash
[root@centos8 ~]#vim /usr/lib/systemd/system/nginx.service 
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target
[Service]
Type=forking
PIDFile=/apps/nginx/run/nginx.pid
ExecStart=/apps/nginx/sbin/nginx -c /apps/nginx/conf/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID
[Install]
WantedBy=multi-user.target
#创建目录
[root@centos8 ~]#mkdir /apps/nginx/run/
#修改配置文件
[root@centos8 ~]#vim /apps/nginx/conf/nginx.conf
pid   /apps/nginx/run/nginx.pid;
[root@centos7 ~]#systemctl daemon-reload
[root@centos7 ~]#systemctl enable --now nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to 
/usr/lib/systemd/system/nginx.service.
[root@centos7 ~]#ss -ntl
State     Recv-Q Send-Q       Local Address:Port   Peer Address:Port          
LISTEN     0      128                     *:22                 *:*             
LISTEN     0      100              127.0.0.1:25                 *:*             
LISTEN     0      128              127.0.0.1:9000               *:*             
LISTEN     0      128                     *:80                 *:*             
LISTEN     0      128                   [::]:22             [::]:*             
LISTEN     0      100                 [::1]:25             [::]:* 
```

* 配置 Nginx 支持 fastcgi

```bash
[root@centos7 ~]#vim /apps/nginx/conf/nginx.conf
[root@centos7 ~]#grep -Ev '#|^$' /apps/nginx/conf/nginx.conf
worker_processes  1;
pid       /apps/nginx/run/nginx.pid;
events {
   worker_connections  1024;
}
http {
   include       mime.types;
   default_type application/octet-stream;
   sendfile       on;
   keepalive_timeout  65;
   server {
       listen       80;
       server_name www.magedu.org;  #指定主机名
       location / {
           root   /data/nginx/wordpress;  #指定数据目录
           index index.php index.html index.htm;          #指定默认主页
       }
       error_page   500 502 503 504 /50x.html;
       location = /50x.html {
           root   html;
       }
       location ~ \.php$ {                         #实现php-fpm
           root           /data/nginx/wordpress;
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
           include       fastcgi_params;
       }
       location ~ ^/(ping|pm_status)$ {            #实现状态页
           include fastcgi_params;
           fastcgi_pass 127.0.0.1:9000;
           fastcgi_param PATH_TRANSLATED $document_root$fastcgi_script_name;
       } 
   }
}
[root@centos7 ~]# /apps/nginx/sbin/nginx -t
nginx: the configuration file /apps/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /apps/nginx/conf/nginx.conf test is successful
[root@centos7 ~]#systemctl reload nginx
```

* 准备 php 测试页

```bash
[root@centos7 ~]#mkdir -p /data/nginx/wordpress
[root@centos7 ~]#vim /data/nginx/wordpress/test.php
[root@centos7 ~]#cat /data/nginx/wordpress/test.php
<?php
phpinfo();
?>
```

*  验证 php 测试页

##### 6.3.5.4 部署 WordPress

在10.0.0.7主机部署 wordpress

* 准备 WordPress 文件

```bash
[root@centos7 ~]#tar xf wordpress-5.4.2-zh_CN.tar.gz
[root@centos7 ~]#cp -r wordpress/* /data/nginx/wordpress
[root@centos7 ~]#chown -R www.www /data/nginx/wordpress/
```

* 初始化web页面

```http
打开浏览器访问下面链接
http://www.magedu.org/
```

* 登录后台管理界面并发表文章
* 验证发表的文章

```bash
#可以看到上传的图片
[root@centos7 ~]#tree /data/nginx/wordpress/wp-content/uploads/
/data/nginx/wordpress/wp-content/uploads/
└── 2020
   └── 10
       └── magedu.jpg
2 directories, 1 file
```

* 配置允许上传大文件

```bash
#注意:默认只支持1M以下文件上传,要利用php程序上传大图片,还需要修改下面三项配置,最大上传由三项值
的最小值决定
#直接上传大于1M文件,会出现下面413错误
[root@centos7 ~]#tail -f /apps/nginx/logs/access.log
10.0.0.1 - - [27/Nov/2020:12:21:16 +0800] "POST /wp-admin/async-upload.php 
HTTP/1.1" 413 585 "http://10.0.0.7/wp-admin/upload.php" "Mozilla/5.0 (Windows NT 
10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.67 
Safari/537.36 Edg/87.0.664.47"
#nginx上传文件大小限制
[root@centos7 ~]#vim /apps/nginx/conf/nginx.conf
server {
       client_max_body_size 10m; #默认值为1M
.....
#php上传文件大小限制
[root@centos7 ~]#vim /etc/php.ini
post_max_size = 30M   #默认值为8M
upload_max_filesize = 20M  #默认值为2M
[root@centos7 ~]#systemctl restart nginx php-fpm
```

* 安全加固

```bash
[root@centos7 ~]#vim /apps/nginx/conf/nginx.conf
[root@centos7 ~]#grep -Ev '#|^$' /apps/nginx/conf/nginx.conf
worker_processes  1;
pid       /apps/nginx/run/nginx.pid;
events {
   worker_connections  1024;
}
http {
   include       mime.types;
   default_type application/octet-stream;
   sendfile       on;  
   keepalive_timeout  65;
   server {
       listen       80;
       server_name www.magedu.org;
       server_tokens off; #添加此行
       location / {
           root   /data/nginx/wordpress;
           index index.php index.html index.htm;
       }
       error_page   500 502 503 504 /50x.html;
       location = /50x.html {
           root   html;
       }
       location ~ \.php$ {
           root           /data/nginx/wordpress;
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
           include       fastcgi_params;
           fastcgi_hide_header X-Powered-By;  #添加此行
       }
       location ~ ^/(ping|pm_status)$ {
           include fastcgi_params;
           fastcgi_pass 127.0.0.1:9000;
           fastcgi_param PATH_TRANSLATED $document_root$fastcgi_script_name;
       } 
   }
}
[root@centos7 ~]#systemctl reload nginx
```

* 配置 php 开启 opcache 加速

在10.0.0.7主机进行以下修改配置

```bash
#编辑php.ini配置文件
[root@centos7 ~]#vim /etc/php.ini
[opcache]
; Determines if Zend OPCache is enabled
zend_extension=opcache.so                                                       
                            opcache.enable=1
.....
[root@centos7 ~]#systemctl restart php-fpm
#访问测试页确认开启opcache加速
```

##### 6.3.5.5 PHP 扩展session模块支持redis

PECL是 PHP 扩展的存储库，提供用于下载和开发 PHP 扩展的所有已知扩展和托管功能的目录 

官方链接: http://pecl.php.net/package-stats.php 

github: https://github.com/phpredis/phpredis 

github安装文档: https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown 

开始在 PHP 中使用 Redis 前， 需要确保已经安装了 redis 服务及 PHP redis 驱动， 

PHP redis 驱动下载地址为:https://github.com/phpredis/phpredis/releases

* 编译安装phpredis模块

在10.0.0.7主机进行以下编译安装phpredis模块

```bash
[root@centos7 ~]#cd /usr/local/src
[root@centos7 src]#ls
[root@centos7 src]#wget http://pecl.php.net/get/redis-5.3.1.tgz
[root@centos7 src]#tar xf redis-5.3.1.tgz 
[root@centos7 src]#cd redis-5.3.1/
[root@centos7 redis-5.3.1]#ls
arrays.markdown   config.m4   INSTALL.markdown README.markdown     redis.c     
      redis_sentinel.c   sentinel_library.h
cluster_library.c config.w32 liblzf           redis_array.c       
redis_cluster.c   redis_sentinel.h   sentinel.markdown
cluster_library.h COPYING     library.c         redis_array.h       
redis_cluster.h   redis_session.c     tests
cluster.markdown   crc16.h     library.h         redis_array_impl.c 
redis_commands.c redis_session.h
common.h           CREDITS     php_redis.h       redis_array_impl.h 
redis_commands.h sentinel_library.c
#如果是yum安装php,需要执行yum -y install php-cli php-devel
#以下为编译安装php的对应方式
[root@centos7 redis-5.3.1]#/apps/php74/bin/phpize 
Configuring for:
PHP Api Version:         20190902
Zend Module Api No:      20190902
Zend Extension Api No:   320190902
Cannot find autoconf. Please check your autoconf installation and the #报错提示
$PHP_AUTOCONF environment variable. Then, rerun this script.
[root@centos7 redis-5.3.1]#yum -y install autoconf
#重新执行成功
[root@centos7 redis-5.3.1]#/apps/php74/bin/phpize 
Configuring for:
PHP Api Version:         20190902
Zend Module Api No:      20190902
Zend Extension Api No:   320190902
#查看生成configure脚本
[root@centos7 redis-5.3.1]#ls 
arrays.markdown   common.h     COPYING           library.h           
redis_array_impl.h redis_sentinel.c   sentinel_library.h
autom4te.cache     config.h.in   crc16.h           php_redis.h         redis.c   
          redis_sentinel.h   sentinel.markdown
build             config.m4     CREDITS           README.markdown     
redis_cluster.c     redis_session.c     tests
cluster_library.c configure     INSTALL.markdown redis_array.c       
redis_cluster.h     redis_session.h
cluster_library.h configure.ac liblzf           redis_array.h       
redis_commands.c   run-tests.php
cluster.markdown   config.w32   library.c         redis_array_impl.c 
redis_commands.h   sentinel_library.c
#如果是基于yum安装php,无需指定--with-php-config
[root@centos7 redis-5.3.1]#./configure --with-php-config=/apps/php74/bin/php-config
[root@centos7 redis-5.3.1]#make -j 8 && make install
.....
See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
Build complete.
Don't forget to run 'make test'.
Installing shared extensions:     /apps/php74/lib/php/extensions/no-debug-zts20190902/
#验证redis模块
#yum安装php,模块文件默认存放在 /usr/lib64/php/modules/redis.so
[root@centos7 redis-5.3.1]#ll /apps/php74/lib/php/extensions/no-debug-zts20190902/
total 9584
-rwxr-xr-x 1 root root 4647668 Oct 22 10:44 opcache.a
-rwxr-xr-x 1 root root 2509416 Oct 22 10:44 opcache.so
-rwxr-xr-x 1 root root 2651320 Oct 22 12:10 redis.so
```

* 编辑php配置文件支持redis

在10.0.0.7主机进行以下修改配置

```bash
#编辑php.ini配置文件，扩展redis.so模块
[root@centos7 ~]#vim /etc/php.ini
.....
#extension=/apps/php74/lib/php/extensions/no-debug-zts-20190902/redis.so
extension=redis.so    #文件最后一行添加此行,路径可省略
[root@centos7 ~]#systemctl restart php-fpm
```

* 验证加载 redis 模块

访问测试页确认redis模块开启

* 安装和配置 redis 服务

在10.0.0.17主机进行安装redis 服务

```bash
[root@centos7 ~]#yum -y install redis
[root@centos7 ~]#vim /etc/redis.conf
bind 0.0.0.0
requirepass 123456
[root@centos7 ~]#systemctl enable --now redis
[root@centos7 ~]#ss -tnl
State     Recv-Q Send-Q                     Local Address:Port                 Peer Address:Port     
LISTEN     0      128                                     *:22                   *:*             
LISTEN     0      100                             127.0.0.1:25                   *:*             
LISTEN     0      128                                     *:6379                 *:*             
LISTEN     0      128                                 [::]:22                   [::]:*             
LISTEN     0      100                                 [::1]:25                   [::]:*             
LISTEN     0      70                                   [::]:33060               [::]:*             
LISTEN     0      128                                 [::]:3306                  [::]:*
```

* 配置 php 支持 redis 保存 session

```bash
#在10.0.0.7主机配置php的session保存在redis服务
[root@centos7 ~]#vim /etc/php.ini
[Session]
; Handler used to store/retrieve data.
; http://php.net/session.save-handler
session.save_handler = redis
session.save_path = "tcp://10.0.0.17:6379?auth=123456"  
[root@centos7 ~]#systemctl restart php-fpm
```

* 准备 php实现 session 的测试页面

```bash
#在10.0.0.7主机进行准备相关文件
[root@centos7 ~]#cat /data/nginx/wordpress/session.php
<?php
session_start();
//redis用session_id作为key 并且是以string的形式存储
$redisKey = 'PHPREDIS_SESSION:' . session_id();
// SESSION 赋值测试
$_SESSION['message'] = "Hello, I'm in redis";
$_SESSION['arr'] = [1, 2, 3, 4, 5, 6];
echo $_SESSION["message"] , "<br/>";
echo "Redis key =   " . $redisKey . "<br/>";
echo "以下是从Redis获取的数据", "<br/>";
// 取数据'
$redis = new Redis();
$redis->connect('10.0.0.17', 6379);
$redis->auth('123456');
echo $redis->get($redisKey);
?>
```

* 访问 web 页面测试实现session保存在redis服务
* redis 主机验证 session 数据

```bash
#在10.0.0.17主机进行验证
[root@centos7 ~]#redis-cli -h 10.0.0.17 -a 123456
10.0.0.17:6379> keys *
1) "PHPREDIS_SESSION:ad3nujfqfvcltkg2ml4snsf72h"
10.0.0.17:6379> get PHPREDIS_SESSION:ad3nujfqfvcltkg2ml4snsf72h
"message|s:19:\"Hello, I'm in redis\";arr|a:6:
{i:0;i:1;i:1;i:2;i:2;i:3;i:3;i:4;i:4;i:5;i:5;i:6;}"
```

## 7 Nginx二次开发

## 8 系统参数优化

默认的Linux内核参数考虑的是最通用场景，不符合用于支持高并发访问的Web服务器的定义，根据业 务特点来进行调整，当Nginx作为静态web内容服务器、反向代理或者提供压缩服务器的服务器时，内 核参数的调整都是不同的，此处针对最通用的、使Nginx支持更多并发请求的TCP网络参数做简单的配置

### 8.1 优化内核参数

修改/etc/sysctl.conf 

```bash
fs.file-max = 1000000
#表示单个进程较大可以打开的句柄数
net.ipv4.tcp_tw_reuse = 1
#参数设置为 1 ，表示允许将TIME_WAIT状态的socket重新用于新的TCP链接，这对于服务器来说意义重大，因为总有大量TIME_WAIT状态的链接存在
net.ipv4.tcp_keepalive_time = 600
#当keepalive启动时，TCP发送keepalive消息的频度;默认是2小时，将其设置为10分钟，可更快的清理无效链接
net.ipv4.tcp_fin_timeout = 30
#当服务器主动关闭链接时，socket保持在FIN_WAIT_2状态的较大时间
net.ipv4.tcp_max_tw_buckets = 5000
#表示操作系统允许TIME_WAIT套接字数量的较大值，如超过此值，TIME_WAIT套接字将立刻被清除并打印警告信息,默认为8000，过多的TIME_WAIT套接字会使Web服务器变慢
net.ipv4.ip_local_port_range = 1024 65000
#定义UDP和TCP链接的本地端口的取值范围
net.ipv4.tcp_rmem = 10240 87380 12582912
#定义了TCP接受缓存的最小值、默认值、较大值
net.ipv4.tcp_wmem = 10240 87380 12582912
#定义TCP发送缓存的最小值、默认值、较大值
net.core.netdev_max_backlog = 8096
#当网卡接收数据包的速度大于内核处理速度时，会有一个列队保存这些数据包。这个参数表示该列队的较大值

net.core.rmem_default = 6291456
#表示内核套接字接受缓存区默认大小
net.core.wmem_default = 6291456
#表示内核套接字发送缓存区默认大小
net.core.rmem_max = 12582912
#表示内核套接字接受缓存区较大大小
net.core.wmem_max = 12582912
#表示内核套接字发送缓存区较大大小
注意：以上的四个参数，需要根据业务逻辑和实际的硬件成本来综合考虑

net.ipv4.tcp_syncookies = 1
#与性能无关。用于解决TCP的SYN攻击
net.ipv4.tcp_max_syn_backlog = 8192
#这个参数表示TCP三次握手建立阶段接受SYN请求列队的较大长度，默认1024，将其设置的大一些可使出现Nginx繁忙来不及accept新连接时，Linux不至于丢失客户端发起的链接请求
net.ipv4.tcp_tw_recycle = 1
#这个参数用于设置启用timewait快速回收
net.core.somaxconn=262114
#选项默认值是128，这个参数用于调节系统同时发起的TCP连接数，在高并发的请求中，默认的值可能会导致链接超时或者重传，因此需要结合高并发请求数来调节此值。
net.ipv4.tcp_max_orphans=262114
#选项用于设定系统中最多有多少个TCP套接字不被关联到任何一个用户文件句柄上。如果超过这个数字，孤立链接将立即被复位并输出警告信息。这个限制指示为了防止简单的DOS攻击，不用过分依靠这个限制甚至认为的减小这个值，更多的情况是增加这个值
```

### 8.2 PAM 资源限制优化

```bash
在/etc/security/limits.conf 最后增加：
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535
```

