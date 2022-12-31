# Redis

## 1 缓存技术

缓存cache 是为了调节速度不一致的两个或多个不同的物质的速度，置于中间,可以实现速度较快的一方 加速访问速度较慢的一方的作用，比如CPU的一级、二级缓存是保存了CPU最近经常访问的数据，内存是保存CPU经常访问硬盘的数据，而且硬盘也有大小不一的缓存，甚至是物理服务器的raid 卡有也缓存，都是为了起到加速CPU访问硬盘数据的目的，因为CPU的速度太快了，CPU需要的数据由于硬盘往往不能在短时间内满足CPU的需求，因此CPU缓存、内存、Raid 卡缓存以及硬盘缓存就在一定程度上满足了CPU的数据需求，即CPU 从缓存读取数据,从而可以大幅提高CPU的工作效率。

参考资料：http://www.sohu.com/a/246498483_468626

### 1.1 缓存 cache

#### 1.1.1 buffer与cache

buffer：缓冲,也叫写缓冲，一般用于写操作，可以将数据先写入内存在写入磁盘，buffer 一般用于写缓 冲，用于解决不同介质的速度不一致的缓冲，先将数据临时写入到里自己最近的地方，以提高写入速 度，CPU会把数据先写到内存的磁盘缓冲区，然后应用就认为数据已经写入完成，然后由内核在后续的 时间再写入磁盘，所以服务器突然断电会丢失内存中的部分数据。

cache：缓存,也叫读缓存，一般用于读操作，CPU读文件从内存读，如果内存没有,就先从硬盘读到内存 再读到CPU，将需要频繁读取的数据放在里自己最近的缓存区域，下次读取的时候即可快速读取。

向 /proc/sys/vm/drop_caches 写入相应的修改值，会清理缓存。建议先执行sync（sync 命令将所有未 写的系统缓冲区写到磁盘中，包含已修改的 i-node、已延迟的块 I/O 和读写映射文件）。执行echo 1、 2、3 至 /proc/sys/vm/drop_caches, 达到不同的清理目的

如果因为是应用有像内存泄露、溢出的问题时，从swap的使用情况是可以比较快速可以判断的，但通过 执行free 反而比较难查看。但核心并不会因为内存泄露等问题并没有快速清空buffer或cache（默认值 是0），生产也不应该随便去改变此值。

一般情况下，应用在系统上稳定运行了，free值也会保持在一个稳定值的。当发生内存不足、应用获取 不到可用内存、OOM错误等问题时，还是更应该去分析应用方面的原因，否则，清空buffer，强制腾出 free的大小，可能只是把问题给暂时屏蔽了。

排除内存不足的情况外，除非是在软件开发阶段，需要临时清掉buffer，以判断应用的内存使用情况； 或应用已经不再提供支持，即使应用对内存的时候确实有问题，而且无法避免的情况下，才考虑定时清 空buffer。

说明: man 5 proc 

```bash
[root@centos8 ~]#man proc
......
/proc/sys/vm/drop_caches (since Linux 2.6.16)
Writing to this file causes the kernel to drop clean caches, dentries, and 
inodes from memory, causing that memory to become free. This can be useful for
memory management testing and performing reproducible filesystem 
benchmarks.Because writing to this file causes the benefits of caching to be 
lost, it can degrade overall system performance.
To free pagecache, use:
echo 1 > /proc/sys/vm/drop_caches
To free dentries and inodes, use:
echo 2 > /proc/sys/vm/drop_caches
To free pagecache, dentries and inodes, use:
echo 3 > /proc/sys/vm/drop_caches
Because writing to this file is a nondestructive operation and dirty objects 
are not freeable, the user should run sync(1) first.
```

```bash
#范例: 清理缓存
[root@centos8 ~]#cat /proc/sys/vm/drop_caches
0
[root@centos8 ~]#free -h
             total       used       free     shared buff/cache   available
Mem:         952Mi       110Mi       84Mi       0.0Ki       757Mi       697Mi
Swap:         2.0Gi       66Mi       1.9Gi
[root@centos8 ~]#echo 3 > /proc/sys/vm/drop_caches
[root@centos8 ~]#free -h
             total       used       free     shared buff/cache   available
Mem:         952Mi       109Mi       794Mi       0.0Ki       48Mi       749Mi
Swap:         2.0Gi       66Mi       1.9Gi
```

#### 1.1.2 缓存保存位置及分层结构

互联网应用领域,提到缓存为王 

* 用户层: 浏览器DNS缓存,应用程序DNS缓存,操作系统DNS缓存客户端 
* 代理层: CDN,反向代理缓存 
* Web层: 解释器Opcache,Web服务器缓存 
* 应用层: 页面静态化 
* 数据层: 分布式缓存,数据库 
* 系统层: 操作系统cache 
* 物理层: 磁盘cache, Raid Cache

#### 1.1.3 cache 的特性

自动过期：给缓存的数据加上有效时间，超出时间后自动过期删除 

强制过期：源网站更新图片后CDN是不会更新的，需要强制是图片缓存过期 ，通过CDN管理后台实现 

命中率：即缓存的读取命中率

### 1.2 用户层缓存

#### 1.2.1 DNS缓存

浏览器的DNS缓存默认为60秒，即60秒之内在访问同一个域名就不在进行DNS解析 

范例: 查看chrome浏览器的DNS缓存

 注意: 此方式新版的chrome 基于安全原因不再显示缓存信息

```http
chrome://net-internals/#dns
```

#### 1.2.2 火狐浏览器缓存

地址栏输入下面指令可以看firefox的缓存信息

```http
about:cache
```

#### 1.2.3 前端技术 dns-prefetch

HTML5的新特性, 可以在html文件中进行DNS的 Prefetch

#### 1.2.4 浏览器缓存过期机制

##### 1.2.4.1 最后修改时间 last-modified

Last-Modified 是 Http 响应Header 中的资源的最后修改时间，如果带有 Last-Modified ，下一次发送 Http 请求时，将会自动携带此时间值的 If-modified-since 的 Http请求Header 。如果没有过期，将会 收到 304 的响应，从缓存中读取。 

浏览器发起请求后,WEB服务器会将服务器端的文件的最后修改时间和本地浏览器缓存中的文件时间相 比，如果没有发生变化就返回给浏览器304的状态码，表示没有发生变化，然后浏览器就使用的本地的 缓存展示资源,如果变化,则重服务器发送变化的新资源 

此方式也需要向服务器发送请求才可以使用缓存

##### 1.2.4.2 Etag 标记

Etag 是 Http 响应报文Header 字段, 代表资源的标签，在服务器端生成。如果响应报文中的资源带有 Etag ，下一次发送请求时会自动携带 If-None-Match 首部字段指定此Etag的值，服务器判断如果 Etag  没有变化将收到 304 的响应，从缓存中读取。

基于Etag标记是否一致做判断页面是否发生过变化，Nginx 可以使用指令 etag on来实现。

Etag 在使用时要注意相同资源在多台 后端的 Web 服务器的 Etag 的一致性

此方式也需要向服务器发送请求才可以使用缓存

##### 1.2.4.3 过期时间 expires 和 Cache-Control

以上两种都需要发送请求，即不管资源是否过期都要发送请求进行协商，这样会消耗不必要的时间，因 此有了缓存的过期时间 

Expire 是 Http响应Header, 代表资源的过期时间，由服务器段设置。如果带有 Expire ，则在 Expire 过 期前不会发生 Http 请求，直接从缓存中读取。但当用户执行 F5 强制刷新例外 

即第一次请求资源时,响应报文带有资源的过期时间，默认为30天，当前此方式使用的比较多，但是无法 保证客户的时间都是准确并且一致的，因此会加入一个最大生存周期，使用用户本地的时间计算缓存数 据是否超过多少天，下面的过期时间Expires:为2028年，但是缓存的最大生存周期Cache-Control:  max-age=315360000，转换为以天单位, 即等于3650天即10年，过期时间如下：

##### 1.2.3.4 混合使用和缓存刷新

通常 Last-Modified,Etag,Expire 是一起混合使用的 

* 特别是 Last-Modified 和 Expire 经常一起使用，因为 Expire 可以让浏览器完全不发起 Http 请 求，而当浏览器强制 F5 的时候又有 Last-Modified ，这样就很好的达到了浏览器段缓存的效果。 
* Etag 和 Expire 一起使用时，先判断 Expire ，如果已经过期，再发起 Http 请求，如果 Etag 变化 了，则返回新资源的 200 响应。如果 Etag 没有变化,则返回 304 的缓存响应。 
* Last-Modified,Etag,Expires 三个同时使用时。先判断 Expire ，如果过期,则然后发送 Http 请求， 服务器先判断 last-modified ，再判断 Etag ，必须都没有过期，才能返回 304 响应。

缓存刷新 

* 第一次访问,获取最新数据,返回 200响应码
* 鼠标点击第二次访问 (Cache),输入地址后回车,浏览器对所有没有过期的内容直接使用本地缓存。 
* F5或点刷新按钮, 会向服务器发送请求缓存协商信息,last-modified和etag会有影响,但expires本地 过期时间不受影响,无变化返回304 
* 按Ctrl+F5强制刷新,所有缓存不再使用,直接连接服务器,获取最新数据,返回200响应码

#### 1.2.5 cookie 和 session

Cookie是访问某些网站以后在本地存储的一些网站相关的信息，下次再访问的时候减少一些步骤,比如加密后的账户名密码等信息 

Cookies是服务器在客户端浏览器上存储的小段文本并随每一个请求发送至同一个服务器，是一种实现 客户端保持状态的方案。 

session称为会话信息，位于web服务器上，主要负责访问者与网站之间的交互，当浏览器请求http地址 时，可以基于之前的session实现会话保持、session共享等。

### 1.3 CDN 缓存

#### 1.3.1 什么是CDN

内容分发网络（Content Delivery Network，CDN）是建立并覆盖在承载网上，由不同区域的服务器组成的分布式网络。将源站资源缓存到全国各地的边缘服务器，利用全球调度系统使用户能够就近获取， 有效降低访问延迟，降低源站压力,提升服务可用性。

**常见的CDN服务商** 

百度CDN：https://cloud.baidu.com/product/cdn.html 

阿里CDN：https://www.aliyun.com/product/cdn?spm=5176.8269123.416540.50.728y8n 

腾讯CDN：https://www.qcloud.com/product/cdn 

腾讯云CDN收费介绍：https://cloud.tencent.com/document/product/228/2949

#### 1.3.2 用户请求CDN流程

1. 用户向 www.test.com 下的某图片资源（如：1.jpg）发起请求，会先向 Local DNS 发起域名解析 请求。 

2. 当 Local DNS 解析 www.test.com 时，会发现已经配置了 CNAME  www.test.com.cdn.dnsv1.com ，解析请求会发送至 Tencent DNS（GSLB），GSLB 为腾讯云自主研发的调度体系，会为请求分配最佳节点 IP。 
2. Local DNS 获取 Tencent DNS 返回的解析 IP。 
2. 用户获取解析 IP。 
2. 用户向获取的 IP 发起对资源 1.jpg 的访问请求。 
2. 若该 IP 对应的节点缓存有 1.jpg，则会将数据直接返回给用户（10），此时请求结束。若该节点未 缓存 1.jpg，则节点会向业务源站发起对 1.jpg 的请求（6、7、8），获取资源后，结合用户自定义 配置的缓存策略，将资源缓存至节点（9），并返回给用户（10），此时请求结束

#### 1.3.3 利用 302 实现转发请求重定向至最优服务器集群

因为中国网络较为复杂，依赖DNS就近解析的调度，仍然会存在部分请求调度失效、调度生效慢等问题。 

比如:腾讯云利用在全国部署的302重定向服务器集群，能够为每一个请求实时决策最优的服务器资源， 精准解决小运营商的调度问题，提升用户访问质量, 能最快地把用户引导到最优的服务器节点上，避开性 能差或者异常的节点。

#### 1.3.5 CDN 分层缓存

提前对静态内容进行预缓存，避免大量的请求回源，导致主站网络带宽被打满而导致数据无法更新，另 外CDN可以将数据根据访问的热度不同而进行不同级别的缓存，例如:访问量最高的资源访问CDN 边缘节点的内存，其次的放在SSD或者SATA，再其次的放在云存储，这样兼顾了速度与成本。 

比如: 腾讯云CDN节点，根据用户的数据冷热不同，动态的进行识别，按照cache层次进行数据的存储， 在访问频率到40%-90%的数据，首先放在OC节点内存cache中，提供8G-64G的数据空间存储;在访问频率到30%-50%的数据，放在OC节点SSD/SATA硬盘cache中，提供1T-15T的数据空间存猪，其他的比较冷的数据，放在云存储中，采用回源拉取的方式进行处理。这样在成本和效率中计算出最优平衡点，为客户提供服务。

#### 1.3.6 CDN主要优势 

CDN 有效地解决了目前互联网业务中网络层面的以下问题： 

* 用户与业务服务器地域间物理距离较远，需要进行多次网络转发，传输延时较高且不稳定。 
* 用户使用运营商与业务服务器所在运营商不同，请求需要运营商之间进行互联转发。 
* 业务服务器网络带宽、处理能力有限，当接收到海量用户请求时，会导致响应速度降低、可用性降低。 
* 利用CDN防止和抵御DDos等攻击,实现安全保护

### 1.4 应用层缓存 

Nginx、PHP等web服务可以设置应用缓存以加速响应用户请求，另外有些解释性语言，比如： PHP/Python/Java不能直接运行，需要先编译成字节码，但字节码需要解释器解释为机器码之后才能执 行，因此字节码也是一种缓存，有时候还会出现程序代码上线后字节码没有更新的现象。所以一般上线新版前,需要先将应用缓存清理,再上线新版 

另外可以利用动态页面静态化技术,加速访问,比如:将访问数据库的数据的动态页面,提前用程序生成静态 页面文件html.电商网站的商品介绍,评论信息非实时数据等皆可利用此技术实现

### 1.5 数据层缓存

分布式缓存服务 

* Redis 
* Memcached

数据库 

* MySQL 查询缓存 
* innodb缓存、MYISAM缓存

### 1.6 硬件缓存

#### 1.6.1 CPU缓存 

CPU缓存(L1的数据缓存和L1的指令缓存)、二级缓存、三级缓存

#### 1.6.2 磁盘相关缓存

* 磁盘缓存:Disk Cache 
* 磁盘阵列缓存: Raid Cache,可使用电池防止断电丢失数据

## 2 redis 部署与使用

### 2.1 redis 基础 

#### 2.1.1 关系型数据库和NoSQL数据库

数据库主要分为两大类：关系型数据库与 NoSQL 数据库。 

关系型数据库，是建立在关系模型基础上的数据库，其借助于集合代数等数学概念和方法来处理数据库 中的数据。主流的 MySQL、Oracle、MS SQL Server 和 DB2 都属于这类传统数据库。 

NoSQL 数据库，全称为 Not Only SQL，意思就是适用关系型数据库的时候就使用关系型数据库，不适 用的时候也没有必要非使用关系型数据库不可，可以考虑使用更加合适的数据存储。主要分为临时性键 值存储（memcached、Redis）、永久性键值存储（ROMA、Redis）、面向文档的数据库 （MongoDB、CouchDB）、面向列的数据库（Cassandra、HBase），每种 NoSQL 都有其特有的使用 场景及优点。 

Oracle，mysql 等传统的关系数据库非常成熟并且已大规模商用，为什么还要用 NoSQL 数据库呢？ 

主要是由于随着互联网发展，数据量越来越大，对性能要求越来越高，传统数据库存在着先天性的缺陷，即单机（单库）性能瓶颈，并且扩展困难。这样既有单机单库瓶颈，却又扩展困难，自然无法满足日益增长的海量数据存储及其性能要求，所以才会出现了各种不同的 NoSQL 产品，NoSQL 根本性的优势在于在云计算时代，简单、易于大规模分布式扩展，并且读写性能非常高

**RDBMS和NOSQL的特点及优缺点：**

> |      | 关系型数据库                                                 |                         NoSQL数据库                          |
> | :--: | :----------------------------------------------------------- | :----------------------------------------------------------: |
> | 特点 | - 数据关系模型基于关系模型，结构化存储，完整性约束；                                                                       - 基于二维表及其之间的联系，需要连接并、交、差、除等数据操作；                            - 采用结构化的查询语言（sql）做数据读写；                                                           - 操作需要数据的一致性，需要事务甚至强调一致性 | - 非结构化的存储；          - 基于多维关系模型；       - 具有特殊的使用场景 |
> | 优点 | - 保持事务的一致性；- 可以进行join等复杂查询； - 通用化，技术成熟； | - 高并发，大数据下读写能力强；                                        - 基本支持分布式，易于扩展，可伸缩； - 简单，弱结构化存储 |
> | 缺点 | - 数据读写必须经过sql解析，大量数据、高并发下读写性能不足； - 对数据做读写，或修改数据结构时需要加锁，影响并发操作； - 无法适应非结构化存储； - 扩展困难； - 昂贵、复杂； | - join等复杂操作能力弱；-  事务支持较弱； - 通用性差； - 无完整约束复杂业务场景支持较差 |

#### 2.1.2 redis 简介

Redis (Remote Dictionary Server)在2009年发布，开发者是意大利的Salvatore Sanfilippo，他本想为 自己的公司开发一个用于替换MySQL的产品Redis，但是没有想到他把Redis开源后大受欢迎，短短几 年，Redis就有了很大的用户群体，目前国内外使用的公司众多,比如:阿里,百度,新浪微博,知乎 网,GitHub,Twitter 等

Redis是一个开源的、遵循BSD协议的、基于内存的而且目前比较流行的键值数据库(key-value  database)，是一个非关系型数据库，redis 提供将内存通过网络远程共享的一种服务，提供类似功能的 还有memcached，但相比memcached，redis还提供了易扩展、高性能、具备数据持久性等功能。

Redis 在高并发、低延迟环境要求比较高的环境使用量非常广泛

目前redis在DB-Engine月排行榜https://db-engines.com/en/ranking 中一直比较靠前，而且一直是键 值型存储类的首位

官网地址：https://redis.io/

#### 2.1.3 Redis 特性

* 速度快: 10W QPS,基于内存,C语言实现 
* 单线程 
* 持久化 
* 支持多种数据结构 
* 支持多种编程语言 
* 功能丰富: 支持Lua脚本,发布订阅,事务,pipeline等功能 
* 简单: 代码短小精悍(单机核心代码只有23000行左右),单线程开发容易,不依赖外部库,使用简单 
* 主从复制 
* 支持高可用和分布式

#### 2.1.4 单线程

Redis 6.0版本前一直是单线程方式处理用户的请求

单线程为何如此快? 

* 纯内存 
* 非阻塞 
* 避免线程切换和竞态消耗

注意事项: 

* 一次只运行一条命令 
* 拒绝长(慢)命令:keys, flushall, flushdb, slow lua script, mutil/exec, operate big value(collection) 
* 其实不是单线程: 早期版本是单进程单线程,3版本后实际还有其它的线程, 实现特定功能,如: fysnc  file descriptor,close file descriptor

#### 2.1.5 redis 对比 memcached

* 支持数据的持久化：可以将内存中的数据保持在磁盘中，重启redis服务或者服务器之后可以从备份文件中恢复数据到内存继续使用 
* 支持更多的数据类型：支持string(字符串)、hash(哈希数据)、list(列表)、set(集合)、zset(有序集 合) 
* 支持数据的备份：可以实现类似于数据的master-slave模式的数据备份，另外也支持使用快照 +AOF 
* 支持更大的value数据：memcache单个key value最大只支持1MB，而redis最大支持512MB(生产 不建议超过2M,性能受影响) 
* 在Redis6版本前,Redis 是单线程，而memcached是多线程，所以单机情况下没有memcached 并发高,性能更好,但redis 支持分布式集群以实现更高的并发，单Redis实例可以实现数万并发 
* 支持集群横向扩展：基于redis cluster的横向扩展，可以实现分布式集群，大幅提升性能和数据安全性 
* 都是基于 C 语言开发

#### 2.1.6 redis 典型应用场景

* Session 共享：常见于web集群中的Tomcat或者PHP中多web服务器session共享 
* 缓存：数据查询、电商网站商品信息、新闻内容 
* 计数器：访问排行榜、商品浏览数等和次数相关的数值统计场景 
* 微博/微信社交场合：共同好友,粉丝数,关注,点赞评论等 
* 消息队列：ELK的日志缓存、部分业务的订阅发布系统 
* 地理位置: 基于GEO(地理信息定位),实现摇一摇,附近的人,外卖等功能

#### 2.1.7 缓存的实现流程

### 2.2 Redis 安装及连接

官方下载地址：http://download.redis.io/releases/

#### 2.2.1 yum安装redis

在centos系统上需要安装epel源

##### 2.2.1.1 查看yum仓库redis版本

```bash
#CentOS 8 由系统源提供
[root@centos8 ~]#dnf info redis
             
#CentOS 7 由epel源提供
[root@centos7 ~]#yum info redis
```

##### 2.2.1.2 yum安装 redis

```bash
[root@centos8 ~]#dnf -y install redis
[root@centos8 ~]#systemctl enable --now redis
[root@centos8 ~]#ss -tnl
[root@centos8 ~]#pstree -p|grep redis
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> info 
# Server
redis_version:5.0.3
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:8c0bf22bfba82c8f
redis_mode:standalone
os:Linux 4.18.0-147.el8.x86_64 x86_64
```

#### 2.2.2 编译安装 redis

下载当前最新release版本redis 源码包：http://download.redis.io/releases/

##### 2.2.2.1 编译安装

官方的安装方法：https://redis.io/download

范例: 编译安装过程

```bash
#安装依赖包
[root@centos7 ~]#yum -y install gcc jemalloc-devel
#如果支持systemd需要安装下面包
[root@centos7 ~]#yum -y install gcc jemalloc-devel systemd-devel
[root@ubuntu1804 ~]#apt -y install make gcc libjemalloc-dev libsystemd-dev
#下载源码
[root@centos7 ~]#wget http://download.redis.io/releases/redis-5.0.7.tar.gz
[root@centos7 ~]#tar xvf redis-5.0.7.tar.gz
#编译安装
[root@centos7 ~]#cd redis-5.0.7/
[root@centos7 redis-5.0.7]#make PREFIX=/apps/redis install #指定redis安装目录
#如果支持systemd,需要执行下面
[root@centos7 redis-6.2.4]#make USE_SYSTEMD=yes PREFIX=/apps/redis install
#配置变量
[root@centos7 ~]#echo 'PATH=/apps/redis/bin:$PATH' > /etc/profile.d/redis.sh
[root@centos7 ~]#. /etc/profile.d/redis.sh
#目录结构
[root@centos7 ~]#tree /apps/redis/
/apps/redis/
└── bin
   ├── redis-benchmark
   ├── redis-check-aof
   ├── redis-check-rdb
   ├── redis-cli
   ├── redis-sentinel -> redis-server
   └── redis-server
1 directory, 6 files
#准备相关目录和配置文件
[root@centos7 ~]#mkdir /apps/redis/{etc,log,data,run} 			#创建配置文件、日志、数据等目录
[root@centos7 redis-5.0.7]#cp redis.conf /apps/redis/etc/
```

##### 2.2.2.2 前台启动 redis

redis-server 是 redis 服务器程序

```bash
[root@centos7 ~]#redis-server --help
Usage: ./redis-server [/path/to/redis.conf] [options]
       ./redis-server - (read config from stdin)
       ./redis-server -v or --version
       ./redis-server -h or --help
       ./redis-server --test-memory <megabytes>
Examples:
       ./redis-server (run the server with default conf)
       ./redis-server /etc/redis/6379.conf
       ./redis-server --port 7777
       ./redis-server --port 7777 --slaveof 127.0.0.1 8888
       ./redis-server /etc/myredis.conf --loglevel verbose
Sentinel mode:
       ./redis-server /etc/sentinel.conf --sentinel
```

前台启动 redis

```bash
[root@centos7 ~]#redis-server /apps/redis/etc/redis.conf 
[root@centos7 ~]#ss -ntl
```

范例: 开启 redis 多实例

```bash
[root@centos7 ~]#redis-server --port 6380
[root@centos7 ~]#ss -ntl
[root@centos7 ~]#ps -ef|grep redis
redis      4407      1  0 10:56 ?        00:00:01 /apps/redis/bin/redis-server 
0.0.0.0:6379
root       4451    963  0 11:05 pts/0    00:00:00 redis-server *:6380
root       4484   4455  0 11:09 pts/1    00:00:00 grep --color=auto redis
[root@centos7 ~]#redis-cli -p 6380
127.0.0.1:6380> 
```

##### 2.2.2.3 解决启动时的三个警告提示

###### 2.2.2.3.1 tcp-backlog

```bash
WARNING: The TCP backlog setting of 511 cannot be enforced because 
/proc/sys/net/core/somaxconn is set to the lower value of 128.
```

backlog参数控制的是三次握手的时候server端收到client ack确认号之后的队列值，即全连接队列

```bash
#vim /etc/sysctl.conf
net.core.somaxconn = 1024
#sysctl -p 
```

###### 2.2.2.3.2 vm.overcommit_memory

```bash
WARNING overcommit_memory is set to 0! Background save may fail under low memory 
condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf 
and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to 
take effect.
```

内核参数说明:

```bash
0、表示内核将检查是否有足够的可用内存供应用进程使用；如果有足够的可用内存，内存申请允许；否则，内
存申请失败，并把错误返回给应用进程。
1、表示内核允许分配所有的物理内存，而不管当前的内存状态如何
2、表示内核允许分配超过所有物理内存和交换空间总和的内存
```

```bash
# 范例
#vim /etc/sysctl.conf
vm.overcommit_memory = 1
#sysctl -p
```

###### 2.2.2.3.3 transparent hugepage

```bash
WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. 
This will create latency and memory usage issues with Redis. To fix this issue 
run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as 
root, and add it to your /etc/rc.local in order to retain the setting after a 
reboot. Redis must be restarted after THP is disabled.
警告：您在内核中启用了透明大页面（THP,不同于一般内存页的4k为2M）支持。 这将在Redis中造成延迟和
内存使用问题。 要解决此问题，请以root 用户身份运行命令“echo never> 
/sys/kernel/mm/transparent_hugepage/enabled”，并将其添加到您的/etc/rc.local中，以便在
重启后保留设置。禁用THP后，必须重新启动Redis。
```

```bash
# 范例
[root@centos7 ~]#echo 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' 
>> /etc/rc.d/rc.local 
[root@centos7 ~]#cat /etc/rc.d/rc.local
#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.
touch /var/lock/subsys/local
echo never > /sys/kernel/mm/transparent_hugepage/enabled
[root@centos7 ~]#chmod +x /etc/rc.d/rc.local
```

###### 2.2.2.3.4 再次启动 redis

将以上配置同步到其他redis 服务器

```bash
[root@centos7 ~]#redis-server /apps/redis/etc/redis.conf 
```

##### 2.2.2.4 创建 redis 用户和数据目录

```bash
[root@centos7 ~]#useradd -r -s /sbin/nologin redis
#设置目录权限
[root@centos7 ~]#chown -R redis.redis /apps/redis/
```

##### 2.2.2.5 编辑 redis 服务启动文件

```bash
#复制CentOS8安装生成的redis.service文件，进行修改
[root@centos7 ~]#scp 10.0.0.8:/lib/systemd/system/redis.service 
/lib/systemd/system/
[root@centos7 ~]#vim /usr/lib/systemd/system/redis.service
[root@centos7 ~]#cat /usr/lib/systemd/system/redis.service
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-server /apps/redis/etc/redis.conf --supervised
systemd
ExecStop=/bin/kill -s QUIT $MAINPID
#Type=notify 如果支持systemd可以启用此行
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target
```

##### 2.2.2.6 验证 redis 启动

```bash
[root@centos7 ~]#systemctl daemon-reload 
[root@centos7 ~]#systemctl start redis
[root@centos7 ~]#systemctl status redis
[root@centos7 ~]#ss -ntl
```

##### 2.2.2.7 使用客户端连接 redis

```bash
[root@centos7 ~]#/apps/redis/bin/redis-cli -h IP/HOSTNAME -p PORT -a PASSWORD
[root@centos7 ~]#redis-cli 
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> info
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:673d8c0ee1a8872
redis_mode:standalone
os:Linux 3.10.0-1062.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:4.8.5
process_id:1669
run_id:5e0420e92e35ad1d740e9431bc655bfd0044a5d1
tcp_port:6379
uptime_in_seconds:140
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:4807524
executable:/apps/redis/bin/redis-server
config_file:/apps/redis/etc/redis.conf
...

127.0.0.1:6379> exit
[root@centos7 ~]#
```

##### 2.2.2.8 创建命令软连接

```bash
[root@centos7 ~]#ln -sv /apps/redis/bin/redis-* /usr/bin/
‘/usr/bin/redis-benchmark’ -> ‘/apps/redis/bin/redis-benchmark’
‘/usr/bin/redis-check-aof’ -> ‘/apps/redis/bin/redis-check-aof’
‘/usr/bin/redis-check-rdb’ -> ‘/apps/redis/bin/redis-check-rdb’
‘/usr/bin/redis-cli’ -> ‘/apps/redis/bin/redis-cli’
‘/usr/bin/redis-sentinel’ -> ‘/apps/redis/bin/redis-sentinel’
‘/usr/bin/redis-server’ -> ‘/apps/redis/bin/redis-server’
```

##### 2.2.2.9 编译安装后的命令

```bash
[root@centos7 ~]#ll /apps/redis/bin/
total 32772
-rwxr-xr-x 1 root root 4366792 Feb 16 21:12 redis-benchmark #redis性能测试工具
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-check-aof #AOF文件检查工具
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-check-rdb #RDB文件检查工具
-rwxr-xr-x 1 root root 4807856 Feb 16 21:12 redis-cli       #客户端工具
lrwxrwxrwx 1 root root      12 Feb 16 21:12 redis-sentinel -> redis-server #哨兵，
软连
接到server
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-server #redis 服务启动命令
[root@centos7 ~]#ll /apps/redis/bin/
total 32772
-rwxr-xr-x 1 root root 4366792 Feb 16 21:12 redis-benchmark #redis性能测试工具
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-check-aof #AOF文件检查工具
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-check-rdb #RDB文件检查工具
-rwxr-xr-x 1 root root 4807856 Feb 16 21:12 redis-cli       #客户端工具
lrwxrwxrwx 1 root root      12 Feb 16 21:12 redis-sentinel -> redis-server #哨兵，软连接到server
-rwxr-xr-x 1 root root 8125184 Feb 16 21:12 redis-server #redis 服务启动命令
```

#### 2.2.3 安装Redis脚本

```bash
[root@redis-node1 ~]#cat install_redis_for_centos.sh
#!/bin/bash
#********************************************************************
#********************************************************************
VERSION=redis-4.0.14
PASSWORD=123456
INSTALL_DIR=/apps/redis
color () {
    RES_COL=60
    MOVE_TO_COL="echo -en \\033[${RES_COL}G"
    SETCOLOR_SUCCESS="echo -en \\033[1;32m"
    SETCOLOR_FAILURE="echo -en \\033[1;31m"
    SETCOLOR_WARNING="echo -en \\033[1;33m"
    SETCOLOR_NORMAL="echo -en \E[0m"
    echo -n "$1" && $MOVE_TO_COL
    echo -n "["
    if [ $2 = "success" -o $2 = "0" ] ;then
        ${SETCOLOR_SUCCESS}
        echo -n $" OK "    
    elif [ $2 = "failure" -o $2 = "1" ] ;then 
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
install() {
wget http://download.redis.io/releases/${VERSION}.tar.gz || { color "Redis 源码下载失败" 1 ; exit; }
yum  -y install gcc jemalloc-devel || { color "安装软件包失败，请检查网络配置" 1 ; exit; }
tar xf ${VERSION}.tar.gz
cd ${VERSION}
make -j 4 PREFIX=${INSTALL_DIR} install && color "Redis 编译安装完成" 0 || { color "Redis 编译安装失败" 1 ;exit ; }
ln -s ${INSTALL_DIR}/bin/redis-* /usr/bin/
mkdir -p ${INSTALL_DIR}/{etc,log,data,run}
cp redis.conf  ${INSTALL_DIR}/etc/
sed -i -e 's/bind 127.0.0.1/bind 0.0.0.0/'  -e "/# requirepass/a requirepass 
$PASSWORD"  -e "/^dir .*/c dir ${INSTALL_DIR}/data/"  -e "/logfile .*/c logfile 
${INSTALL_DIR}/log/redis-6379.log"  -e  "/^pidfile .*/c pidfile 
${INSTALL_DIR}/run/redis_6379.pid" ${INSTALL_DIR}/etc/redis.conf
if id redis &> /dev/null ;then 
   color "Redis 用户已存在" 1
else
   useradd -r -s /sbin/nologin redis
   color "Redis 用户创建成功" 0
fi
chown -R redis.redis ${INSTALL_DIR}

cat >> /etc/sysctl.conf <<EOF
net.core.somaxconn = 1024
vm.overcommit_memory = 1
EOF
sysctl -p
echo 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' >> 
/etc/rc.d/rc.local
chmod +x /etc/rc.d/rc.local
/etc/rc.d/rc.local
cat > /usr/lib/systemd/system/redis.service <<EOF
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=${INSTALL_DIR}/bin/redis-server ${INSTALL_DIR}/etc/redis.conf --
supervised systemd
ExecStop=/bin/kill -s QUIT \$MAINPID
#Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload 
systemctl enable --now redis &> /dev/null && color "Redis 服务启动成功,Redis信息如下:"  0 || { color "Redis 启动失败" 1 ;exit; } 
sleep 2
redis-cli -a $PASSWORD INFO Server 2> /dev/null
}

install
```

#### 2.2.4 连接到 Redis

主要分为客户端连接和程序的连接

##### 2.2.4.1 客户端连接 redis

###### 2.2.4.1.1 本机无密码连接

```http
redis-cli 
```

###### 2.2.4.1.2 本机密码连接

```bash
redis-cli -a <PASSWORD>
#或者
export REDISCLI_AUTH=<PASSWORD>
redis-cli 
```

###### 2.2.4.1.3 跨主机无密码连接

```bash
redis-cli -h HOSTNAME/IP -p PORT
```

###### 2.2.4.1.4 跨主机密码连接

```bash
redis-cli -h HOSTNAME/IP -p PORT -a PASSWORD
```

##### 2.2.4.2 程序连接 Redis

redis 支持多种开发语言访问：Java、PHP、Python、Ruby、Lua

```http
https://redis.io/clients
```

###### 2.2.4.2.1 shell 脚本写入数据到 Redis

```bash
[root@centos8 ~]#cat redis_test.sh
#!/bin/bash
NUM=100
PASS=123456
for i in `seq $NUM`;do
   redis-cli -h 127.0.0.1 -a "$PASS"  --no-auth-warning  set key${i} value${i}
    echo "key${i} value${i} 写入完成"
done
echo "$NUM个key写入到Redis完成" 
```

###### 2.2.4.2.2 python 连接方式

python 多种开发库,可以支持连接redis

```
https://redis.io/clients
```

使用redis-py 连接 redis  

官方github : https://github.com/andymccurdy/redis-py

```bash
# 范例
[root@centos8 ~]#yum info python3-redis
Last metadata expiration check: 1 day, 10:19:31 ago on Thu 15 Oct 2020 11:03:33 
PM CST.
Installed Packages
Name         : python3-redis
Version     : 3.3.8
Release     : 1.el8
Architecture : noarch
Size         : 523 k
Source       : python-redis-3.3.8-1.el8.src.rpm
Repository   : @System
From repo   : epel
Summary     : Python 3 interface to the Redis key-value store
URL         : https://github.com/andymccurdy/redis-py
License     : MIT
Description : This is a Python 3 interface to the Redis key-value store.

[root@centos8 ~]#yum -y install python3 python3-redis
#注意文件名不要为redis,会和redis模块名称冲突

[root@centos8 ~]#cat redis_test.py 
#!/bin/env python3 
import redis
#import time
pool =
redis.ConnectionPool(host="127.0.0.1",port=6379,password="",decode_responses=Tru
e)
r = redis.Redis(connection_pool=pool)
for i in range(100):
 r.set("k%d" % i,"v%d" % i)
# time.sleep(1)
 data=r.get("k%d" % i)
 print(data)
 
[root@centos8 ~]# python3 redis_test.py
......
'v94'
'v95'
'v96'
'v97'
'v98'
'v99'
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> get k10
"v10"
127.0.0.1:6379> 
```

##### 2.2.4.3 图形工具连接

有一此第三方开发的图形工具也可以连接redis ,比如: RedisDesktopManager

#### 2.2.5 redis 的多实例

测试环境中经常使用多实例,需要指定不同实例的相应的端口,配置文件,日志文件等相关配置

范例: 以编译安装为例实现 redis 多实例

```bash
#生成的文件列表
[root@centos7 ~]#ll /apps/redis/
total 0
drwxr-xr-x 2 redis redis 134 Oct 15 22:13 bin
drwxr-xr-x 2 redis redis  69 Oct 15 23:25 data
drwxr-xr-x 2 redis redis  75 Oct 15 22:42 etc
drwxr-xr-x 2 redis redis  72 Oct 15 23:25 log
drwxr-xr-x 2 redis redis  72 Oct 15 22:47 run
[root@centos7 ~]#tree /apps/redis/
/apps/redis/
├── bin
│   ├── redis-benchmark
│   ├── redis-check-aof
│   ├── redis-check-rdb
│   ├── redis-cli
│   ├── redis-sentinel -> redis-server
│   └── redis-server
├── data
│   ├── dump_6379.rdb
│   ├── dump_6380.rdb
│   └── dump_6381.rdb
├── etc
│   ├── redis_6379.conf
│   ├── redis_6380.conf
│   └── redis_6381.conf
├── log
│   ├── redis_6379.log
│   ├── redis_6380.log
│   └── redis_6381.log
└── run
   ├── redis_6379.pid
   ├── redis_6380.pid
   └── redis_6381.pid
5 directories, 18 files
[root@centos7 ~]#grep '^[^#]' /apps/redis/etc/redis_6379.conf 
bind 0.0.0.0
protected-mode yes
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize yes
supervised no
pidfile /apps/redis/run/redis_6379.pid
loglevel notice
logfile "/apps/redis/log/redis_6379.log"
databases 16
always-show-logo yes
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump_6379.rdb
dir /apps/redis/data/
replica-serve-stale-data yes
replica-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-disable-tcp-nodelay no
replica-priority 100
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
appendonly no
appendfilename "appendonly_6379.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble yes
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
stream-node-max-bytes 4096
stream-node-max-entries 100
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
dynamic-hz yes
aof-rewrite-incremental-fsync yes
rdb-save-incremental-fsync yes

[root@centos7 ~]#grep 6380 /apps/redis/etc/redis_6380.conf 
# Accept connections on the specified port, default is 6380 (IANA #815344).
port 6380
pidfile /apps/redis/run/redis_6380.pid
logfile "/apps/redis/log/redis_6380.log"
dbfilename dump_6380.rdb
appendfilename "appendonly_6380.aof"
# cluster-config-file nodes-6380.conf
# cluster-announce-port 6380
# cluster-announce-bus-port 6380

[root@centos7 ~]#grep 6381 /apps/redis/etc/redis_6381.conf 
# Accept connections on the specified port, default is 6381 (IANA #815344).
port 6381
pidfile /apps/redis/run/redis_6381.pid
logfile "/apps/redis/log/redis_6381.log"
dbfilename dump_6381.rdb
appendfilename "appendonly_6381.aof"
# cluster-config-file nodes-6381.conf
# cluster-announce-port 6381

[root@centos7 ~]#cat /lib/systemd/system/redis6379.service 
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-server /apps/redis/etc/redis_6379.conf --
supervised systemd
#ExecStop=/usr/libexec/redis-shutdown
ExecStop=/bin/kill -s QUIT $MAINPID
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target

[root@centos7 ~]#cat /lib/systemd/system/redis6380.service 
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-server /apps/redis/etc/redis_6380.conf --
supervised systemd
#ExecStop=/usr/libexec/redis-shutdown
ExecStop=/bin/kill -s QUIT $MAINPID
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target

[root@centos7 ~]#cat /lib/systemd/system/redis6381.service 
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-server /apps/redis/etc/redis_6381.conf --
supervised systemd
#ExecStop=/usr/libexec/redis-shutdown
ExecStop=/bin/kill -s QUIT $MAINPID
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target

[root@centos7 ~]#systemctl daemon-reload
[root@centos7 ~]#systemctl enable --now redis6379 redis6380 redis6381
[root@centos7 ~]#ss -ntl
State     Recv-Q Send-Q     Local Address:Port   Peer Address:Port             
LISTEN     0      128                     *:22                 *:*                 
LISTEN     0      100             127.0.0.1:25                 *:*                
LISTEN     0      511                     *:6379               *:*                 
LISTEN     0      511                     *:6380               *:*                
LISTEN     0      511                     *:6381               *:*                
LISTEN     0      128                 [::]:22             [::]:*                
LISTEN     0      100                 [::1]:25             [::]:* 
```

### 2.3 redis 配置和优化

#### 2.3.1 redis 主要配置项

```bash
bind 0.0.0.0 #监听地址，可以用空格隔开后多个监听IP
protected-mode yes #redis3.2之后加入的新特性，在没有设置bind IP和密码的时候,redis只允许访问127.0.0.1:6379，可以远程连接，但当访问将提示警告信息并拒绝远程访问
port 6379 #监听端口,默认6379/tcp
tcp-backlog 511 #三次握手的时候server端收到client ack确认号之后的队列值，即全连接队列长度
timeout 0 #客户端和Redis服务端的连接超时时间，默认是0，表示永不超时
tcp-keepalive 300 #tcp 会话保持时间300s
daemonize no #默认no,即直接运行redis-server程序时,不作为守护进程运行，而是以前台方式运行，如果想在后台运行需改成yes,当redis作为守护进程运行的时候，它会写一个 pid 到/var/run/redis.pid 文件
supervised no #和OS相关参数，可设置通过upstart和systemd管理Redis守护进程，centos7后都使用systemd
pidfile /var/run/redis_6379.pid #pid文件路径,可以修改为/apps/redis/run/redis_6379.pid
loglevel notice #日志级别
logfile "/path/redis.log" #日志路径,示例:logfile "/apps/redis/log/redis_6379.log"
databases 16 #设置数据库数量，默认：0-15，共16个库
always-show-logo yes #在启动redis 时是否显示或在日志中记录记录redis的logo
save 900 1 #在900秒内有1个key内容发生更改,就执行快照机制
save 300 10 #在300秒内有10个key内容发生更改,就执行快照机制
save 60 10000  #60秒内如果有10000个key以上的变化，就自动快照备份
stop-writes-on-bgsave-error yes #默认为yes时,可能会因空间满等原因快照无法保存出错时，会禁止redis写入操作，生产建议为no			#此项只针对配置文件中的自动save有效
rdbcompression yes #持久化到RDB文件时，是否压缩，"yes"为压缩，"no"则反之
rdbchecksum yes #是否对备份文件开启RC64校验，默认是开启
dbfilename dump.rdb #快照文件名
dir ./ #快照文件保存路径，示例：dir "/apps/redis/data"

#主从复制相关
# replicaof <masterip> <masterport> #指定复制的master主机地址和端口，5.0版之前的指令为slaveof 
# masterauth <master-password> #指定复制的master主机的密码
replica-serve-stale-data yes #当从库同主库失去连接或者复制正在进行，从机库有两种运行方式：
1、设置为yes(默认设置)，从库会继续响应客户端的读请求，此为建议值
2、设置为no，除去特定命令外的任何请求都会返回一个错误"SYNC with master in progress"。

replica-read-only yes #是否设置从库只读，建议值为yes,否则主库同步从库时可能会覆盖数据，造成数据丢失

repl-diskless-sync no #是否使用socket方式复制数据(无盘同步)，新slave第一次连接master时需要做数据的全量同步，redis server就要从内存dump出新的RDB文件，然后从master传到slave，有两种方式把RDB文件传输给客户端：
1、基于硬盘（disk-backed）：为no时，master创建一个新进程dump生成RDB磁盘文件，RDB完成之后由父进程（即主进程）将RDB文件发送给slaves，此为默认值
2、基于socket（diskless）：master创建一个新进程直接dump RDB至slave的网络socket，不经过主进程和硬盘
#推荐使用基于硬盘（为no），是因为RDB文件创建后，可以同时传输给更多的slave，但是基于socket(为yes)， 新slave连接到master之后得逐个同步数据。只有当磁盘I/O较慢且网络较快时，可用diskless(yes),否则一般建议使用磁盘(no)

repl-diskless-sync-delay 5 #diskless时复制的服务器等待的延迟时间，设置0为关闭，在延迟时间内到达的客户端，会一起通过diskless方式同步数据，但是一旦复制开始，master节点不会再接收新slave的复制请求，直到下一次同步开始才再接收新请求。即无法为延迟时间后到达的新副本提供服务，新副本将排队等待下一次RDB传输，因此服务器会等待一段时间才能让更多副本到达。推荐值：30-60

repl-ping-replica-period 10 #slave根据master指定的时间进行周期性的PING master,用于监测master状态,默认10s

repl-timeout 60 #复制连接的超时时间，需要大于repl-ping-slave-period，否则会经常报超时

repl-disable-tcp-nodelay no #是否在slave套接字发送SYNC之后禁用 TCP_NODELAY，如果择"yes"，Redis将合并多个报文为一个大的报文，从而使用更少数量的包向slaves发送数据，但是将使数据传输到slave上有延迟，Linux内核的默认配置会达到40毫秒，如果 "no" ，数据传输到slave的延迟将会减少，但要使用更多的带宽

repl-backlog-size 512mb #复制缓冲区内存大小，当slave断开连接一段时间后，该缓冲区会累积复制副本数据，因此当slave 重新连接时，通常不需要完全重新同步，只需传递在副本中的断开连接后没有同步的部分数据即可。只有在至少有一个slave连接之后才分配此内存空间,建议建立主从时此值要调大一些或在低峰期配置,否则会导致同步到slave失败

repl-backlog-ttl 3600 #多长时间内master没有slave连接，就清空backlog缓冲区

replica-priority 100 #当master不可用，哨兵Sentinel会根据slave的优先级选举一个master，此值最低的slave会优先当选master，而配置成0，永远不会被选举，一般多个slave都设为一样的值，让其自动选择

#min-replicas-to-write 3 #至少有3个可连接的slave，mater才接受写操作
#min-replicas-max-lag 10 #和上面至少3个slave的ping延迟不能超过10秒，否则master也将停止写操作

requirepass foobared #设置redis连接密码，之后需要AUTH pass,如果有特殊符号，用" "引起来,生产建议设置
rename-command #重命名一些高危命令，示例：rename-command FLUSHALL "" 禁用命令
   			  #示例: rename-command del magedu
   			  
maxclients 10000 #Redis最大连接客户端

maxmemory <bytes> #redis使用的最大内存，单位为bytes字节，0为不限制，建议设为物理内存一半，8G内存的计算方式8(G)*1024(MB)1024(KB)*1024(Kbyte)，需要注意的是缓冲区是不计算在maxmemor内,生产中如果不设置此项,可能会导致OOM

appendonly no #是否开启AOF日志记录，默认redis使用的是rdb方式持久化，这种方式在许多应用中已经足够用了，但是redis如果中途宕机，会导致可能有几分钟的数据丢失(取决于dump数据的间隔时间)，根据save来策略进行持久化，Append Only File是另一种持久化方式，可以提供更好的持久化特性，Redis会把每次写入的数据在接收后都写入 appendonly.aof 文件，每次启动时Redis都会先把这个文件的数据读入内存里，先忽略RDB文件。默认不启用此功能

appendfilename "appendonly.aof" #文本文件AOF的文件名，存放在dir指令指定的目录中

appendfsync everysec #aof持久化策略的配置
#no表示由操作系统保证数据同步到磁盘,Linux的默认fsync策略是30秒，最多会丢失30s的数据
#always表示每次写入都执行fsync，以保证数据同步到磁盘,安全性高,性能较差
#everysec表示每秒执行一次fsync，可能会导致丢失这1s数据,此为默认值,也生产建议值

#同时在执行bgrewriteaof操作和主进程写aof文件的操作，两者都会操作磁盘，而bgrewriteaof往往会涉及大量磁盘操作，这样就会造成主进程在写aof文件的时候出现阻塞的情形,以下参数实现控制
no-appendfsync-on-rewrite no #在aof rewrite期间,是否对aof新记录的append暂缓使用文件同步
策略,主要考虑磁盘IO开支和请求阻塞时间。
#默认为no,表示"不暂缓",新的aof记录仍然会被立即同步到磁盘，是最安全的方式，不会丢失数据，但是要忍受阻塞的问题
#为yes,相当于将appendfsync设置为no，这说明并没有执行磁盘操作，只是写入了缓冲区，因此这样并不会造成阻塞（因为没有竞争磁盘），但是如果这个时候redis挂掉，就会丢失数据。丢失多少数据呢？Linux的默认fsync策略是30秒，最多会丢失30s的数据,但由于yes性能较好而且会避免出现阻塞因此比较推荐

#rewrite 即对aof文件进行整理,将空闲空间回收,从而可以减少恢复数据时间

auto-aof-rewrite-percentage 100 #当Aof log增长超过指定百分比例时，重写AOF文件，设置为0表示不自动重写Aof日志，重写是为了使aof体积保持最小，但是还可以确保保存最完整的数据

auto-aof-rewrite-min-size 64mb #触发aof rewrite的最小文件大小

aof-load-truncated yes #是否加载由于某些原因导致的末尾异常的AOF文件(主进程被kill/断电等)，建议yes

aof-use-rdb-preamble no #redis4.0新增RDB-AOF混合持久化格式，在开启了这个功能之后，AOF重写产生的文件将同时包含RDB格式的内容和AOF格式的内容，其中RDB格式的内容用于记录已有的数据，而AOF格式的内容则用于记录最近发生了变化的数据，这样Redis就可以同时兼有RDB持久化和AOF持久化的优点（既能够快速地生成重写文件，也能够在出现问题时，快速地载入数据）,默认为no,即不启用此功能

lua-time-limit 5000 #lua脚本的最大执行时间，单位为毫秒
cluster-enabled yes #是否开启集群模式，默认不开启,即单机模式

cluster-config-file nodes-6379.conf #由node节点自动生成的集群配置文件名称

cluster-node-timeout 15000 #集群中node节点连接超时时间，单位ms,超过此时间，会踢出集群

cluster-replica-validity-factor 10 #单位为次,在执行故障转移的时候可能有些节点和master断开一段时间导致数据比较旧，这些节点就不适用于选举为master，超过这个时间的就不会被进行故障转移,不能当选master，计算公式：(node-timeout * replica-validity-factor) + repl-pingreplica-period 

cluster-migration-barrier 1 #集群迁移屏障，一个主节点至少拥有1个正常工作的从节点，即如果主节点的slave节点故障后会将多余的从节点分配到当前主节点成为其新的从节点。

cluster-require-full-coverage yes #集群请求槽位全部覆盖，如果一个主库宕机且没有备库就会出现集群槽位不全，那么yes时redis集群槽位验证不全,就不再对外提供服务(对key赋值时,会出现CLUSTERDOWN The cluster is down的提示,cluster_state:fail,但ping 仍PONG)，而no则可以继续使用,但是会出现查询数据查不到的情况(因为有数据丢失)。生产建议为no

cluster-replica-no-failover no #如果为yes,此选项阻止在主服务器发生故障时尝试对其主服务器进行故障转移。 但是，主服务器仍然可以执行手动强制故障转移，一般为no

#Slow log 是 Redis 用来记录超过指定执行时间的日志系统，执行时间不包括与客户端交谈，发送回复等I/O操作，而是实际执行命令所需的时间（在该阶段线程被阻塞并且不能同时为其它请求提供服务）,由于slow log 保存在内存里面，读写速度非常快，因此可放心地使用，不必担心因为开启 slow log 而影响Redis 的速度

slowlog-log-slower-than 10000 #以微秒为单位的慢日志记录，为负数会禁用慢日志，为0会记录每个命令操作。默认值为10ms,一般一条命令执行都在微秒级,生产建议设为1ms-10ms之间

slowlog-max-len 128 #最多记录多少条慢日志的保存队列长度，达到此长度后，记录新命令会将最旧的命令从命令队列中删除，以此滚动删除,即,先进先出,队列固定长度,默认128,值偏小,生产建议设为1000以上
```

#### 2.3.2 CONFIG 动态修改配置

config 命令用于查看当前redis配置、以及不重启redis服务实现动态更改redis配置等

**注意：不是所有配置都可以动态修改,且此方式无法持久保存**

```bash
CONFIG SET parameter value
时间复杂度：O(1)
CONFIG SET 命令可以动态地调整 Redis 服务器的配置(configuration)而无须重启。

可以使用它修改配置参数，或者改变 Redis 的持久化(Persistence)方式。
CONFIG SET 可以修改的配置参数可以使用命令 CONFIG GET * 来列出，所有被 CONFIG SET 修改的配置参数都会立即生效。

CONFIG GET parameter
时间复杂度： O(N)，其中 N 为命令返回的配置选项数量。
CONFIG GET 命令用于取得运行中的 Redis 服务器的配置参数(configuration parameters)，在Redis 2.4 版本中， 有部分参数没有办法用 CONFIG GET 访问，但是在最新的 Redis 2.6 版本中，所有配置参数都已经可以用 CONFIG GET 访问了。

CONFIG GET 接受单个参数 parameter 作为搜索关键字，查找所有匹配的配置参数，其中参数和值以“键-值对”(key-value pairs)的方式排列。
比如执行 CONFIG GET s* 命令，服务器就会返回所有以 s 开头的配置参数及参数的值：
```

##### 2.3.2.1 设置连接密码

```bash
#设置连接密码
127.0.0.1:6379> CONFIG SET requirepass 123456
OK
#查看连接密码
127.0.0.1:6379> CONFIG GET requirepass  
1) "requirepass"
2) "123456"
```

##### 2.3.2.2 获取当前配置

```bash
#奇数行为键，偶数行为值
127.0.0.1:6379> CONFIG GET *
  1) "dbfilename"
  2) "dump.rdb"
  3) "requirepass"
  4) ""
  5) "masterauth"
  6) ""
  7) "cluster-announce-ip"
  8) ""
  9) "unixsocket"
10) ""
11) "logfile"
12) "/var/log/redis/redis.log"
13) "pidfile"
14) "/var/run/redis_6379.pid"
15) "slave-announce-ip"
16) ""
17) "replica-announce-ip"
18) ""
19) "maxmemory"
20) "0"
......
#查看bind 
127.0.0.1:6379> CONFIG GET bind
1) "bind"
2) "0.0.0.0"
#有些设置无法修改
127.0.0.1:6379> CONFIG SET bind 127.0.0.1
(error) ERR Unsupported CONFIG parameter: bind
```

##### 2.3.2.3 更改最大内存

```bash
127.0.0.1:6379> CONFIG SET maxmemory 8589934592
OK
127.0.0.1:6379> CONFIG GET maxmemory
1) "maxmemory"
2) "8589934592"
```

#### 2.3.3 慢查询

范例: SLOW LOG

```bash
[root@centos8 ~]#vim /etc/redis.conf
slowlog-log-slower-than 1    #指定为超过1us即为慢的指令
slowlog-max-len 1024         #指定保存1024条慢记录
127.0.0.1:6379> SLOWLOG LEN  #查看慢日志的记录条数
(integer) 14
127.0.0.1:6379> SLOWLOG GET [n] #查看慢日志的n条记录
1) 1) (integer) 14
2) (integer) 1544690617
3) (integer) 4
4) 1) "slowlog"
127.0.0.1:6379> SLOWLOG GET 3
1) 1) (integer) 7
   2) (integer) 1602901545
   3) (integer) 26
   4) 1) "SLOWLOG"
      2) "get"
   5) "127.0.0.1:38258"
   6) ""
2) 1) (integer) 6
   2) (integer) 1602901540
   3) (integer) 22
   4) 1) "SLOWLOG"
      2) "get"
      3) "2"
   5) "127.0.0.1:38258"
   6) ""
3) 1) (integer) 5
   2) (integer) 1602901497
   3) (integer) 22
   4) 1) "SLOWLOG"
      2) "GET"
   5) "127.0.0.1:38258"
   6) ""
127.0.0.1:6379> SLOWLOG RESET #清空慢日志
OK
```

#### 2.3.4 redis 持久化

Redis 虽然是一个内存级别的缓存程序，也就是redis 是使用内存进行数据的缓存的，但是其可以将内存 的数据按照一定的策略保存到硬盘上，从而实现数据持久保存的目的 

目前redis支持两种不同方式的数据持久化保存机制，分别是RDB和AOF

##### 2.3.4.1 RDB 模式

###### 2.3.4.1.1 RDB 模式工作原理

RDB(Redis DataBase)：基于时间的快照，其默认只保留当前最新的一次快照，特点是执行速度比较快，缺点是可能会丢失从上次快照到当前时间点之间未做快照的数据

**RDB bgsave 实现快照的具体过程:**

Redis从master主进程先fork出一个子进程，使用写时复制机制，子进程将内存的数据保存为一个临时文件，比如:tmp-.rdb，当数据保存完成之后再将上一次保存的RDB文件替换掉，然后关闭子进程，这样可以保证每一次做RDB快照保存的数据都是完整的

因为直接替换RDB文件的时候,可能会出现突然断电等问题,而导致RDB文件还没有保存完整就因为突然关机停止保存,而导致数据丢失的情况.后续可以手动将每次生成的RDB文件进行备份，这样可以最大化保存历史数据

###### 2.3.4.2.2 RDB 相关配置

```bash
save 900 1         #900s内修改了1个key即触发保存RDB
save 300 10        #300s内修改了10个key即触发保存RDB
save 60 10000      #60s内修改了10000个key即触发保存RDB
dbfilename dump.rdb
dir ./         #编泽编译安装,默认RDB文件存放在启动redis的工作目录,建议明确指定存入目录
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
```

###### 2.3.4.2.3 实现RDB方式

* save: 同步,会阻赛其它命令,不推荐使用 
* bgsave: 异步后台执行,不影响其它命令的执行 
* 自动: 制定规则,自动执行

###### 2.3.4.2.4 RDB 模式的优缺点

优点：

* RDB快照保存了某个时间点的数据，可以通过脚本执行redis指令bgsave(非阻塞，后台执行)或者 save(会阻塞写操作,不推荐)命令自定义时间点备份，可以保留多个备份，当出现问题可以恢复到不 同时间点的版本,很适合备份,并且此文件格式也支持有不少第三方工具可以进行后续的数据分析 比如: 可以在最近的24小时内，每小时备份一次RDB文件，并且在每个月的每一天，也备份一个 RDB文件。这样的话，即使遇上问题，也可以随时将数据集还原到不同的版本。 
* RDB可以最大化Redis的性能，父进程在保存 RDB文件时唯一要做的就是fork出一个子进程，然后 这个子进程就会处理接下来的所有保存工作，父进程无须执行任何磁盘 I/O 操作。 
* RDB在大量数据,比如几个G的数据，恢复的速度比AOF的快

缺点：

* 不能实时保存数据，可能会丢失自上一次执行RDB备份到当前的内存数据 
* 如果你需要尽量避免在服务器故障时丢失数据，那么RDB并不适合。虽然Redis允许设置不同的保 存点（save point）来控制保存RDB文件的频率，但是，因为RDB文件需要保存整个数据集的状 态，所以它并不是一个轻松快速的操作。因此一般会超过5分钟以上才保存一次RDB文件。在这种情况下，一旦发生故障停机，你就可能会丢失好几分钟的数据。 
* 当数据量非常大的时候，从父进程fork子进程进行保存至RDB文件时需要一点时间，可能是毫秒或 者秒，取决于磁盘IO性能 
* 在数据集比较庞大时，fork()可能会非常耗时，造成服务器在一定时间内停止处理客户端﹔如果数 据集非常巨大，并且CPU时间非常紧张的话，那么这种停止时间甚至可能会长达整整一秒或更久。 虽然 AOF重写也需要进行fork()，但无论AOF重写的执行间隔有多长，数据的持久性都不会有任何 损失。

```bash
范例: 手动备份RDB文件的脚本
#配置文件
[root@centos7 ~]#vim /apps/redis/etc/redis.conf
save ""
dbfilename dump_6379.rdb
dir "/data/redis"
appendonly no 
#脚本
[root@centos8 ~]#cat redis_backup_rdb.sh 
#!/bin/bash
#********************************************************************
BACKUP=/backup/redis-rdb
DIR=/data/redis
FILE=dump_6379.rdb
PASS=123456
color () {
    RES_COL=60
    MOVE_TO_COL="echo -en \\033[${RES_COL}G"
    SETCOLOR_SUCCESS="echo -en \\033[1;32m"
    SETCOLOR_FAILURE="echo -en \\033[1;31m"
    SETCOLOR_WARNING="echo -en \\033[1;33m"
    SETCOLOR_NORMAL="echo -en \E[0m"
    echo -n "$1" && $MOVE_TO_COL
    echo -n "["
    if [ $2 = "success" -o $2 = "0" ] ;then
        ${SETCOLOR_SUCCESS}
        echo -n $" OK "    
    elif [ $2 = "failure" -o $2 = "1" ] ;then 
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
redis-cli -h 127.0.0.1 -a $PASS --no-auth-warning bgsave 
result=`redis-cli -a 123456 --no-auth-warning info Persistence |grep 
rdb_bgsave_in_progress| sed -rn 's/.*:([0-9]+).*/\1/p'`
until [ $result -eq 0 ] ;do
    sleep 1
    result=`redis-cli -a 123456 --no-auth-warning info Persistence |grep 
rdb_bgsave_in_progress| sed -rn 's/.*:([0-9]+).*/\1/p'`
done
DATE=`date +%F_%H-%M-%S`
[ -e $BACKUP ] || { mkdir -p $BACKUP ; chown -R redis.redis $BACKUP; }
cp -a $DIR/$FILE $BACKUP/dump_6379-${DATE}.rdb
color "Backup redis RDB" 0
#执行
[root@centos8 ~]#bash redis_backup_rdb.sh
Background saving started
Backup redis RDB                                           [ OK ]
[root@centos8 ~]#ll /backup/redis-rdb/ -h
total 143M
-rw-r--r-- 1 redis redis 143M Oct 21 11:08 dump_6379-2020-10-21_11-08-47.rdb
```

范例: 观察save 和 bgsave的执行过程

```bash
#阻塞
#生成临时文件
[root@centos7 ~]#(redis-cli -a 123456 save &) ; echo save is finished; redis-cli -a 123456 get class
```

范例: 自动保存

```bash
[root@centos7 ~]#vim /apps/redis/etc/redis.conf
save 60 3
#测试60s内修改3个key,验证是否生成RDB文件
```

##### 2.3.4.2 AOF 模式

###### 2.3.4.2.1 AOF 模式工作原理

AOF：AppendOnylFile，按照操作顺序依次将操作追加到指定的日志文件末尾 

AOF 和 RDB 一样使用了写时复制机制，AOF默认为每秒钟fsync一次，即将执行的命令保存到AOF文件当中，这样即使redis服务器发生故障最多只丢失1秒钟之内的数据，也可以设置不同的fsync策略 always，即设置每次执行命令的时候执行fsync，fsync会在后台执行线程，所以主线程可以继续处理用 户的正常请求而不受到写入AOF文件的I/O影响 

同时启用RDB和AOF,进行恢复时,默认AOF文件优先级高于RDB文件,即会使用AOF文件进行恢复 

**注意: AOF 模式默认是关闭的,第一次开启AOF后,并重启服务生效后,会因为AOF的优先级高于RDB,而 AOF默认没有数据文件存在,从而导致所有数据丢失**

范例: 正确启用AOF功能,访止数据丢失

```bash
[root@centos8 ~]#ll /var/lib/redis/
total 314392
-rw-r--r-- 1 redis redis 187779391 Oct 17 14:23 dump.rdb
[root@centos8 ~]#redis-cli
127.0.0.1:6379> config get appendonly 
1) "appendonly"
2) "no"
127.0.0.1:6379> config set appendonly  yes  #自动触发AOF重写
OK
[root@centos8 ~]#ll /var/lib/redis/
total 314392
-rw-r--r-- 1 redis redis 187779391 Oct 17 14:23 dump.rdb
-rw-r--r-- 1 redis redis  85196805 Oct 17 14:45 temp-rewriteaof-2146.aof
[root@centos8 ~]#ll /var/lib/redis/
total 366760
-rw-r--r-- 1 redis redis 187779391 Oct 17 14:45 appendonly.aof
-rw-r--r-- 1 redis redis 187779391 Oct 17 14:23 dump.rdb
[root@centos8 ~]#vim /etc/redis.conf
appendonly yes #改为yes 
#config set appendonly yes 后可以同时看到下面显示
```

###### 2.3.4.2.2 AOF 相关配置

```bash
appendonly no #是否开启AOF日志记录，默认redis使用的是rdb方式持久化，这种方式在许多应用中已经足够用了，但是redis如果中途宕机，会导致可能有几分钟的数据丢失(取决于dump数据的间隔时间)，根据save来策略进行持久化，Append Only File是另一种持久化方式，可以提供更好的持久化特性，Redis会把每次写入的数据在接收后都写入 appendonly.aof 文件，每次启动时Redis都会先把这个文件的数据读入内存里，先忽略RDB文件。默认不启用此功能

appendfilename "appendonly.aof" #文本文件AOF的文件名，存放在dir指令指定的目录中

appendfsync everysec #aof持久化策略的配置
#no表示由操作系统保证数据同步到磁盘,Linux的默认fsync策略是30秒，最多会丢失30s的数据
#always表示每次写入都执行fsync，以保证数据同步到磁盘,安全性高,性能较差
#everysec表示每秒执行一次fsync，可能会导致丢失这1s数据,此为默认值,也生产建议值
dir /path

#rewrite相关
no-appendfsync-on-rewrite yes
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
```

范例: 动态修改配置自动生成appendonly.aof文件

```bash
127.0.0.1:6379> CONFIG set appendonly yes
```

###### 2.3.4.2.3 AOF rewrite 重写

将一些重复的,可以合并的,过期的数据重新写入一个新的AOF文件,从而节约AOF备份占用的硬盘空间,也能加速恢复过程 

可以手动执行bgrewriteaof 触发AOF,第一次开启AOF功能,或定义自动rewrite 策略

* AOF rewrite 重写配置

```bash
#同时在执行bgrewriteaof操作和主进程写aof文件的操作，两者都会操作磁盘，而bgrewriteaof往往会涉及大量磁盘操作，这样就会造成主进程在写aof文件的时候出现阻塞的情形,以下参数实现控制
no-appendfsync-on-rewrite no #在aof rewrite期间,是否对aof新记录的append暂缓使用文件同步策略,主要考虑磁盘IO开支和请求阻塞时间。
#默认为no,表示"不暂缓",新的aof记录仍然会被立即同步到磁盘，是最安全的方式，不会丢失数据，但是要
忍受阻塞的问题
#为yes,相当于将appendfsync设置为no，这说明并没有执行磁盘操作，只是写入了缓冲区，因此这样并不会造成阻塞（因为没有竞争磁盘），但是如果这个时候redis挂掉，就会丢失数据。丢失多少数据呢？Linux的默认fsync策略是30秒，最多会丢失30s的数据,但由于yes性能较好而且会避免出现阻塞因此比较推荐

#rewrite 即对aof文件进行整理,将空闲空间回收,从而可以减少恢复数据时间
auto-aof-rewrite-percentage 100 #当Aof log增长超过指定百分比例时，重写AOF文件，设置为0表示不自动重写Aof日志，重写是为了使aof体积保持最小，但是还可以确保保存最完整的数据

auto-aof-rewrite-min-size 64mb #触发aof rewrite的最小文件大小

aof-load-truncated yes #是否加载由于某些原因导致的末尾异常的AOF文件(主进程被kill/断电等)，建议yes
```

###### 2.3.4.2.4 手动执行AOF重写 BGREWRITEAOF 命令

```bash
BGREWRITEAOF
时间复杂度： O(N)， N 为要追加到 AOF 文件中的数据数量。执行一个 AOF文件 重写操作。重写会创建一个当前 AOF 文件的体积优化版本。

即使 BGREWRITEAOF 执行失败，也不会有任何数据丢失，因为旧的 AOF 文件在 BGREWRITEAOF 成功之前不会被修改。

重写操作只会在没有其他持久化工作在后台执行时被触发，也就是说：
如果 Redis 的子进程正在执行快照的保存工作，那么 AOF 重写的操作会被预定(scheduled)，等到保存工作完成之后再执行 AOF 重写。在这种情况下， BGREWRITEAOF 的返回值仍然是 OK ，但还会加上一条额外的信息，说明 BGREWRITEAOF 要等到保存操作完成之后才能执行。在 Redis 2.6 或以上的版本，可以使用 INFO [section] 命令查看 BGREWRITEAOF 是否被预定。

如果已经有别的 AOF 文件重写在执行，那么 BGREWRITEAOF 返回一个错误，并且这个新的BGREWRITEAOF 请求也不会被预定到下次执行。

从 Redis 2.4 开始， AOF 重写由 Redis 自行触发， BGREWRITEAOF 仅仅用于手动触发重写操作。
```

范例: 手动bgrewriteaof

```bash
127.0.0.1:6379> BGREWRITEAOF
Background append only file rewriting started
#同时可以观察到下面显示
```

###### 2.3.4.2.5 AOF 模式优缺点

优点：

* 数据安全性相对较高，根据所使用的fsync策略(fsync是同步内存中redis所有已经修改的文件到存 储设备)，默认是appendfsync everysec，即每秒执行一次 fsync,在这种配置下，Redis 仍然可以保 持良好的性能，并且就算发生故障停机，也最多只会丢失一秒钟的数据( fsync会在后台线程执行， 所以主线程可以继续努力地处理命令请求) 
* 由于该机制对日志文件的写入操作采用的是append模式，因此在写入过程中不需要seek, 即使出现 宕机现象，也不会破坏日志文件中已经存在的内容。然而如果本次操作只是写入了一半数据就出现 了系统崩溃问题，不用担心，在Redis下一次启动之前，可以通过 redis-check-aof 工具来解决数据 一致性的问题 
* Redis可以在 AOF文件体积变得过大时，自动地在后台对AOF进行重写,重写后的新AOF文件包含了恢复当前数据集所需的最小命令集合。整个重写操作是绝对安全的，因为Redis在创建新 AOF文件 的过程中，append模式不断的将修改数据追加到现有的 AOF文件里面，即使重写过程中发生停 机，现有的 AOF文件也不会丢失。而一旦新AOF文件创建完毕，Redis就会从旧AOF文件切换到新 AOF文件，并开始对新AOF文件进行追加操作。 
* AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作。事实上，也可以通过该文 件完成数据的重建AOF文件有序地保存了对数据库执行的所有写入操作，这些写入操作以Redis协议的格式保存，因 此 AOF文件的内容非常容易被人读懂，对文件进行分析(parse)也很轻松。导出（export)AOF文件 也非常简单:举个例子，如果不小心执行了FLUSHALL.命令，但只要AOF文件未被重写，那么只要停 止服务器，移除 AOF文件末尾的FLUSHAL命令，并重启Redis ,就可以将数据集恢复到FLUSHALL执 行之前的状态。

缺点：

* 即使有些操作是重复的也会全部记录，AOF 的文件大小要大于 RDB 格式的文件 
* AOF 在恢复大数据集时的速度比 RDB 的恢复速度要慢 
* 根据fsync策略不同,AOF速度可能会慢于RDB 
* bug 出现的可能性更多

##### 2.3.4.3 RDB和AOF 的选择

如果主要充当缓存功能,或者可以承受数分钟数据的丢失, 通常生产环境一般只需启用RDB即可,此也是默认值 

如果数据需要持久保存,一点不能丢失,可以选择同时开启RDB和AOF 

一般不建议只开启AOF

### 2.4 Redis 常用命令

官方文档：https://redis.io/commands 

参考链接:  

http://redisdoc.com/ 

http://doc.redisfans.com/

#### 2.4.1 INFO 

显示当前节点redis运行状态信息

```bash
127.0.0.1:6379> INFO
# Server
redis_version:5.0.3
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:8c0bf22bfba82c8f
redis_mode:standalone
os:Linux 4.18.0-147.el8.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:8.2.1
process_id:725
run_id:8af0d3fba2b7c5520e0981b125cc49c3ce4d2a2f
tcp_port:6379
uptime_in_seconds:18552
......
```

#### 2.4.2 SELECT

切换数据库，相当于在MySQL的 USE DBNAME 指令

```bash
[root@centos8 ~]#redis-cli
127.0.0.1:6379> info cluster
# Cluster
cluster_enabled:0
127.0.0.1:6379[15]> SELECT 0
OK
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> SELECT 15
OK
127.0.0.1:6379[15]> SELECT 16
(error) ERR DB index is out of range
127.0.0.1:6379[15]>
```

注意: 在 redis cluster 模式下不支持多个数据库,会出现下面错误

```bash
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> info cluster
# Cluster
cluster_enabled:1
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> select 1
(error) ERR SELECT is not allowed in cluster mode
```

#### 2.4.3 KEYS

查看当前库下的所有key，此命令慎用！

```bash
127.0.0.1:6379[15]> SELECT 0
OK
127.0.0.1:6379> KEYS *
1) "9527"
2) "9526"
3) "paihangbang"
4) "list1"
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> KEYS *
(empty list or set)
127.0.0.1:6379[1]> 
redis>MSET one 1 two 2 three 3 four 4  # 一次设置 4 个 key
OK
redis> KEYS *o*
1) "four"
2) "two"
3) "one"
redis> KEYS t??
1) "two"
redis> KEYS t[w]*
1) "two"
redis> KEYS *  # 匹配数据库内所有 key
1) "four"
2) "three"
3) "two"
4) "one"
```

#### 2.4.4 BGSAVE

手动在后台执行RDB持久化操作

```bash
#交互式执行
127.0.0.1:6379[1]> BGSAVE
Background saving started
#非交互式执行
[root@centos8 ~]#ll /var/lib/redis/
total 4
-rw-r--r-- 1 redis redis 326 Feb 18 22:45 dump.rdb
[root@centos8 ~]#redis-cli -h 127.0.0.1 -a '123456' BGSAVE
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
Background saving started
[root@centos8 ~]#ll /var/lib/redis/
total 4
-rw-r--r-- 1 redis redis 92 Feb 18 22:54 dump.rdb
```

#### 2.4.5 DBSIZE

返回当前库下的所有key 数量

```bash
127.0.0.1:6379> DBSIZE
(integer) 4
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> DBSIZE
(integer) 0
```

#### 2.4.6 FLUSHDB

强制清空当前库中的所有key，此命令慎用！

```bash
127.0.0.1:6379[1]> SELECT 0
OK
127.0.0.1:6379> DBSIZE
(integer) 4
127.0.0.1:6379> FLUSHDB
OK
127.0.0.1:6379> DBSIZE
(integer) 0
127.0.0.1:6379>
```

#### 2.4.7 FLUSHALL

强制清空当前redis服务器所有数据库中的所有key，即删除所有数据，此命令慎用！

```bash
127.0.0.1:6379> FLUSHALL
OK
#生产建议修改配置 /etc/redis.conf
rename-command FLUSHALL ""
#rename-command 在6.2.4版淘汰了
```

#### 2.4.8 SHUTDOWN

```bash
可用版本： >= 1.0.0
时间复杂度： O(N)，其中 N 为关机时需要保存的数据库键数量。
SHUTDOWN 命令执行以下操作：
停止所有客户端
如果有至少一个保存点在等待，执行 SAVE 命令
如果 AOF 选项被打开，更新 AOF 文件
关闭 redis 服务器(server)
如果持久化被打开的话， SHUTDOWN 命令会保证服务器正常关闭而不丢失任何数据。

另一方面，假如只是单纯地执行 SAVE 命令，然后再执行 QUIT 命令，则没有这一保证 —— 因为在执行SAVE 之后、执行 QUIT 之前的这段时间中间，其他客户端可能正在和服务器进行通讯，这时如果执行 QUIT 就会造成数据丢失。
```

### 2.5 redis 数据类型

参考资料：http://www.redis.cn/topics/data-types.html 

相关命令参考: http://redisdoc.com/

#### 2.5.1 字符串 string 

字符串是所有编程语言中最常见的和最常用的数据类型，而且也是redis最基本的数据类型之一，而且 redis 中所有的 key 的类型都是字符串。常用于保存 Session 信息场景，此数据类型比较常用

| 命令          | 含义                         | 复杂度 |
| ------------- | ---------------------------- | ------ |
| set key value | 设置key-value                | o(1)   |
| get key       | 获取key-value                | o(1)   |
| del key       | 删除key-value                | o(1)   |
| setnx set**   | 根据key是否存在设置key-value | o(1)   |
| incr decr     | 计数                         | o(1)   |
| mget mset     | 批量操作key-value            | o(n)   |

##### 2.5.1.1 添加一个key

set 指令可以创建一个key 并赋值, 使用格式

```bash
SET key value [EX seconds] [PX milliseconds] [NX|XX]
时间复杂度： O(1)
将字符串值 value 关联到 key 。

如果 key 已经持有其他值， SET 就覆写旧值， 无视类型。
当 SET 命令对一个带有生存时间（TTL）的键进行设置之后， 该键原有的 TTL 将被清除。

从 Redis 2.6.12 版本开始， SET 命令的行为可以通过一系列参数来修改：
EX seconds ： 将键的过期时间设置为 seconds 秒。 执行 SET key value EX seconds 的效果等同于执行 SETEX key seconds value 。
PX milliseconds ： 将键的过期时间设置为 milliseconds 毫秒。 执行 SET key value PX milliseconds 的效果等同于执行 PSETEX key milliseconds value 。
NX ： 只在键不存在时， 才对键进行设置操作。 执行 SET key value NX 的效果等同于执行 SETNX key value 。
XX ： 只在键已经存在时， 才对键进行设置操作。
```

范例: 

```bash
#不论key是否存在.都设置
127.0.0.1:6379> set key1 value1
OK
127.0.0.1:6379> get key1
"value1"
127.0.0.1:6379> TYPE key1  #判断类型
string
127.0.0.1:6379> SET title ceo ex 3 #设置自动过期时间3s
OK
127.0.0.1:6379> set NAME wang
OK
127.0.0.1:6379> get NAME
"wang"
#大小写敏感
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> set name mage
OK
127.0.0.1:6379> get name
"mage"
127.0.0.1:6379> get NAME
"wang"

#key不存在,才设置,相当于add 
127.0.0.1:6379> get title
"ceo"
127.0.0.1:6379> setnx title coo  #set key value nx
(integer) 0
127.0.0.1:6379> get title
"ceo"

#key存在,才设置,相当于update
127.0.0.1:6379> get title
"ceo"
127.0.0.1:6379> set title coo xx
OK
127.0.0.1:6379> get title
"coo"
127.0.0.1:6379> get age
(nil)
127.0.0.1:6379> set age 20 xx
(nil)
127.0.0.1:6379> get age
(nil)
```

##### 2.5.1.2 获取一个key的内容

```bash
127.0.0.1:6379> get key1
"value1"
127.0.0.1:6379> get name age
(error) ERR wrong number of arguments for 'get' command
```

##### 2.5.1.3 删除一个和多个key

```bash
127.0.0.1:6379> DEL key1
(integer) 1
127.0.0.1:6379> DEL key1 key2
(integer) 2
```

##### 2.5.1.4 批量设置多个key

```bash
127.0.0.1:6379> MSET key1 value1 key2 value2 
OK
```

##### 2.5.1.5 批量获取多个key

```bash
127.0.0.1:6379> MGET key1 key2
1) "value1"
2) "value2"
127.0.0.1:6379> KEYS n*
1) "n1"
2) "name"
127.0.0.1:6379> KEYS *
1) "k2"
2) "k1"
3) "key1"
4) "key2"
5) "n1"
6) "name"
7) "k3"
8) "title"
```

##### 2.5.1.6 追加数据

```bash
127.0.0.1:6379> APPEND key1 " append new value"
(integer) 12              #添加数据后,key1总共9个字节
127.0.0.1:6379> get key1
"value1 append new value"
```

##### 2.5.1.7 设置新值并返回旧值

```bash
#set key newvalue并返回旧的value
127.0.0.1:6379> set name wang
OK
127.0.0.1:6379> getset name magedu
"wang"
127.0.0.1:6379> get name
"magedu"
```

##### 2.5.1.8 返回字符串 key 对应值的字节数

```bash
127.0.0.1:6379> SET name wang
OK
127.0.0.1:6379> STRLEN name
(integer) 4
127.0.0.1:6379> APPEND name " xiaochun"
(integer) 13
127.0.0.1:6379> GET name
"wang xiaochun"
127.0.0.1:6379> STRLEN name  #返回字节数
(integer) 13
127.0.0.1:6379> set name 马哥教育
OK
127.0.0.1:6379> get name
"\xe9\xa9\xac\xe5\x93\xa5\xe6\x95\x99\xe8\x82\xb2"
127.0.0.1:6379> strlen name
(integer) 12
127.0.0.1:6379> 
```

##### 2.5.1.9 判断 key 是否存在

```bash
127.0.0.1:6379> SET name wang ex 10
OK
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> EXISTS NAME #key的大小写敏感
(integer) 0
127.0.0.1:6379> EXISTS name age #返回值为1,表示存在2个key,0表示不存在
(integer) 2
127.0.0.1:6379> EXISTS name  #过几秒再看
(integer) 0
```

##### 2.5.1.10 查看 key 的过期时间

```bash
ttl key #查看key的剩余生存时间,如果key过期后,会自动删除
-1 #返回值表示永不过期，默认创建的key是永不过期，重新对key赋值，也会从有剩余生命周期变成永不过
期
-2 #返回值表示没有此key
num #key的剩余有效期
127.0.0.1:6379> TTL key1
(integer) -1
127.0.0.1:6379> SET name wang EX 100
OK
127.0.0.1:6379> TTL name
(integer) 96
127.0.0.1:6379> TTL name
(integer) 93
127.0.0.1:6379> SET name mage #重新设置，默认永不过期
OK
127.0.0.1:6379> TTL name
(integer) -1
127.0.0.1:6379> SET name wang EX 200
OK
127.0.0.1:6379> TTL name
(integer) 198
127.0.0.1:6379> GET name
"wang"
```

##### 2.5.1.11 重新设置key的过期时间

```bash
127.0.0.1:6379> TTL name
(integer) 148
127.0.0.1:6379> EXPIRE name 1000
(integer) 1
127.0.0.1:6379> TTL name
(integer) 999
127.0.0.1:6379>
```

##### 2.5.1.12 取消key的过期时间

即永不过期

```bash
127.0.0.1:6379> TTL name
(integer) 999
127.0.0.1:6379> PERSIST name
(integer) 1
127.0.0.1:6379> TTL name
(integer) -1
```

##### 2.5.1.13 数值递增

利用INCR命令簇（INCR, DECR, INCRBY,DECRBY)来把字符串当作原子计数器使用。

```bash
127.0.0.1:6379> set num 10 #设置初始值
OK
127.0.0.1:6379> INCR num
(integer) 11
127.0.0.1:6379> get num
"11"
```

##### 2.5.1.14 数值递减

```bash
127.0.0.1:6379> set num 10
OK
127.0.0.1:6379> DECR num
(integer) 9
127.0.0.1:6379> get num
"9"
```

##### 2.5.1.15 数值增加

将key对应的数字加decrement(可以是负数)。如果key不存在，操作之前，key就会被置为0。如果key的value类型错误或者是个不能表示成数字的字符串，就返回错误。这个操作最多支持64位有符号的正型数字。

```bash
redis> SET mykey 10
OK
redis> INCRBY mykey 5
(integer) 15
127.0.0.1:6379> get mykey
"15"
127.0.0.1:6379> INCRBY mykey -10
(integer) 5
127.0.0.1:6379> get mykey
"5"
127.0.0.1:6379> INCRBY nokey  5
(integer) 5
127.0.0.1:6379> get nokey
"5"
```

##### 2.5.1.16 数据减少

decrby 可以减小数值(也可以增加)

```bash
127.0.0.1:6379> SET mykey 10
OK
127.0.0.1:6379> DECRBY mykey 8
(integer) 2
127.0.0.1:6379> get mykey
"2"
127.0.0.1:6379> DECRBY mykey -20
(integer) 22
127.0.0.1:6379> get mykey
"22"
127.0.0.1:6379> DECRBY nokey 3
(integer) -3
127.0.0.1:6379> get nokey
"-3"
```

#### 2.5.2 列表 list

列表是一个双向可读写的管道，其头部是左侧，尾部是右侧，一个列表最多可以包含2^32- 1（4294967295）个元素，下标 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。 也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，元素值可以重复，常用于存入日志等场景，此数据类型比较常用

列表特点 

* 有序 
* 可重复 
* 左右都可以操作

##### 2.5.2.1 生成列表并插入数据

LPUSH和RPUSH都可以插入列表

```bash
LPUSH key value [value …]
时间复杂度： O(1)
将一个或多个值 value 插入到列表 key 的表头

如果有多个 value 值，那么各个 value 值按从左到右的顺序依次插入到表头： 比如说，对空列表mylist 执行命令 LPUSH mylist a b c ，列表的值将是 c b a ，这等同于原子性地执行 LPUSH mylist a 、 LPUSH mylist b 和 LPUSH mylist c 三个命令。

如果 key 不存在，一个空列表会被创建并执行 LPUSH 操作。
当 key 存在但不是列表类型时，返回一个错误。

RPUSH key value [value …]
时间复杂度： O(1)
将一个或多个值 value 插入到列表 key 的表尾(最右边)。

如果有多个 value 值，那么各个 value 值按从左到右的顺序依次插入到表尾：比如对一个空列表 mylist 执行 RPUSH mylist a b c ，得出的结果列表为 a b c ，等同于执行命令 RPUSH mylist a 、RPUSH mylist b 、 RPUSH mylist c 。

如果 key 不存在，一个空列表会被创建并执行 RPUSH 操作。
当 key 存在但不是列表类型时，返回一个错误。
```

范例: 

```bash
#从左边添加数据，已添加的需向右移
127.0.0.1:6379> LPUSH name mage wang zhang  #根据顺序逐个写入name，最后的zhang会在列表
的最左侧。
(integer) 3
127.0.0.1:6379> TYPE name
list

#从右边添加数据
127.0.0.1:6379> RPUSH course linux python go
(integer) 3
127.0.0.1:6379> type course
list
```

##### 2.5.2.2 向列表追加数据

```bash
127.0.0.1:6379> LPUSH list1 tom
(integer) 2
#从右边添加数据，已添加的向左移
127.0.0.1:6379> RPUSH list1 jack
(integer) 3
```

##### 2.5.2.3 获取列表长度(元素个数)

```bash
127.0.0.1:6379> LLEN list1
(integer) 3
```

##### 2.5.2.4 获取列表指定位置数据

```bash
127.0.0.1:6379> LPUSH list1 a b c d
(integer) 4
127.0.0.1:6379> LINDEX list1 0 #获取0编号的元素
"d"
127.0.0.1:6379> LINDEX list1 3 #获取3编号的元素
"a"
127.0.0.1:6379> LINDEX list1 -1 #获取最后一个的元素
"a"
#元素从0开始编号
127.0.0.1:6379> LPUSH list1 a b c d
(integer) 4
127.0.0.1:6379> LRANGE list1 1 2
1) "c"
2) "b"
127.0.0.1:6379> LRANGE list1 0 3  #所有元素
1) "d"
2) "c"
3) "b"
4) "a"
127.0.0.1:6379> LRANGE list1 0 -1  #所有元素
1) "d"
2) "c"
3) "b"
4) "a"
127.0.0.1:6379> RPUSH list2 zhang wang li zhao
(integer) 4
127.0.0.1:6379> LRANGE list2 1 2 #指定范围
1) "wang"
2) "li"
127.0.0.1:6379> LRANGE list2 2 2 #指定位置
1) "li"
127.0.0.1:6379> LRANGE list2 0 -1  #所有元素
1) "zhang"
2) "wang"
3) "li"
4) "zhao"
```

##### 2.5.2.5 修改列表指定索引值

```bash
127.0.0.1:6379> RPUSH listkey a b c d e f
(integer) 6
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
5) "e"
6) "f"
127.0.0.1:6379> lset listkey 2 java
OK
127.0.0.1:6379> lrange listkey 0 -1
1) "a"
2) "b"
3) "java"
4) "d"
5) "e"
6) "f"
127.0.0.1:6379>
```

##### 2.5.2.6 移除列表数据

```bash
127.0.0.1:6379> LPUSH list1 a b c d
(integer) 4
127.0.0.1:6379> LRANGE list1 0 3
1) "d"
2) "c"
3) "b"
4) "a"
127.0.0.1:6379> LPOP list1 #弹出左边第一个元素，即删除第一个
"d"
127.0.0.1:6379> LLEN list1
(integer) 3
127.0.0.1:6379> LRANGE list1 0 2
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> RPOP list1  #弹出右边第一个元素，即删除最后一个
"a"
127.0.0.1:6379> LLEN list1
(integer) 2
127.0.0.1:6379> LRANGE list1 0 1
1) "c"
2) "b"

#LTRIM 对一个列表进行修剪(trim)，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除
127.0.0.1:6379> LLEN list1
(integer) 4
127.0.0.1:6379> LRANGE list1 0 3
1) "d"
2) "c"
3) "b"
4) "a"
127.0.0.1:6379> LTRIM list1 1 2 #只保留1，2号元素
OK
127.0.0.1:6379> LLEN list1
(integer) 2
127.0.0.1:6379> LRANGE list1 0 1
1) "c"
2) "b"

#删除list
127.0.0.1:6379> DEL list1
(integer) 1
127.0.0.1:6379> EXISTS list1
(integer) 0
```

#### 2.5.3 集合 set

Set 是 String 类型的无序集合，集合中的成员是唯一的，这就意味着集合中不能出现重复的数据，可以在两个不同的集合中对数据进行对比并取值，常用于取值判断，统计，交集等场景,例如: 实现共同的朋友

集合特点 

* 无序 
* 无重复 
* 集合间操作

##### 2.5.3.1 生成集合key

```
127.0.0.1:6379> SADD set1 v1
(integer) 1
127.0.0.1:6379> SADD set2 v2 v4
(integer) 2
127.0.0.1:6379> TYPE set1
set
127.0.0.1:6379> TYPE set2
set
```

##### 2.5.3.2 追加数值

```bash
#追加时，只能追加不存在的数据，不能追加已经存在的数值
127.0.0.1:6379> SADD set1 v2 v3 v4
(integer) 3
127.0.0.1:6379> SADD set1 v2 #已存在的value,无法再次添加
(integer) 0
127.0.0.1:6379> TYPE set1
set
127.0.0.1:6379> TYPE set2
set
```

##### 2.5.3.3 查看集合的所有数据

```bash
127.0.0.1:6379> SMEMBERS set1
1) "v4"
2) "v1"
3) "v3"
4) "v2"
127.0.0.1:6379> SMEMBERS set2
1) "v4"
2) "v2"
```

##### 2.5.3.4 删除集合中的元素

```bash
127.0.0.1:6379> sadd goods mobile laptop car 
(integer) 3
127.0.0.1:6379> srem goods car
(integer) 1
127.0.0.1:6379> SMEMBERS goods
1) "mobile"
2) "laptop"
127.0.0.1:6379> 
```

集合间操作

##### 2.5.3.5 获取集合的交集

交集：已属于A且属于B的元素称为A与B的交（集）

```bash
127.0.0.1:6379> SINTER set1 set2
1) "v4"
2) "v2"
```

##### 2.5.3.6 获取集合的并集

并集：已属于A或属于B的元素为称为A与B的并（集）

```bash
127.0.0.1:6379> SUNION set1 set2
1) "v2"
2) "v4"
3) "v1"
4) "v3"
```

##### 2.5.2.7 获取集合的差集

差集：已属于A而不属于B的元素称为A与B的差（集）

```bash
127.0.0.1:6379> SDIFF set1 set2
1) "v1"
2) "v3"
```

#### 2.5.4 有序集合 sorted set

Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员，不同的是每个元素都会关 联一个double(双精度浮点型)类型的分数，redis正是通过该分数来为集合中的成员进行从小到大的排 序，有序集合的成员是唯一的,但分数(score)却可以重复，集合是通过哈希表实现的，所以添加，删除， 查找的复杂度都是O(1)， 集合中最大的成员数为 2^32 - 1 (4294967295, 每个集合可存储40多亿个成 员)，经常用于排行榜的场景

有序集合特点 

* 有序 
* 无重复元素 
* 每个元素是由score和value组成 
* score 可以重复 
* value 不可以重复

##### 2.5.4.1 生成有序集合

```bash
127.0.0.1:6379> ZADD zset1 1 v1  #分数为1
(integer) 1
127.0.0.1:6379> ZADD zset1 2 v2
(integer) 1
127.0.0.1:6379> ZADD zset1 2 v3  #分数可重复，元素值不可以重复
(integer) 1
127.0.0.1:6379> ZADD zset1 3 v4
(integer) 1
127.0.0.1:6379> TYPE zset1
zset
127.0.0.1:6379> TYPE zset2
zset
#一次生成多个数据：
127.0.0.1:6379> ZADD zset2 1 v1 2 v2 3 v3 4 v4 5 v5
(integer) 5
```

##### 2.5.4.2 有序集合实现排行榜

```bash
127.0.0.1:6379> ZADD paihangbang 90 nezha 199 zhanlang 60 zhuluoji 30 gangtiexia
(integer) 4
127.0.0.1:6379> ZRANGE paihangbang 0 -1  #正序排序后显示集合内所有的key,score从小到大显示
1) "gangtiexia"
2) "zhuluoji"
3) "nezha"
4) "zhanlang"
127.0.0.1:6379> ZREVRANGE paihangbang 0 -1 #倒序排序后显示集合内所有的key,score从大到小显示
1) "zhanlang"
2) "nezha"
3) "zhuluoji"
4) "gangtiexia"
127.0.0.1:6379> ZRANGE paihangbang 0 -1 WITHSCORES  #正序显示指定集合内所有key和得分情况
1) "gangtiexia"
2) "30"
3) "zhuluoji"
4) "60"
5) "nezha"
6) "90"
7) "zhanlang"
8) "199"
127.0.0.1:6379> ZREVRANGE paihangbang 0 -1 WITHSCORES  #倒序显示指定集合内所有key和得分情况
1) "zhanlang"
2) "199"
3) "nezha"
4) "90"
5) "zhuluoji"
6) "60"
7) "gangtiexia"
8) "30"
127.0.0.1:6379> 
```

##### 2.5.4.3 获取集合的个数

```bash
127.0.0.1:6379> ZCARD paihangbang
(integer) 4
127.0.0.1:6379> ZCARD zset1
(integer) 4
127.0.0.1:6379> ZCARD zset2
(integer) 4
```

##### 2.5.4.4 基于索引返回数值

```bash
127.0.0.1:6379> ZRANGE paihangbang 0 2
1) "gangtiexia"
2) "zhuluoji"
3) "nezha"
127.0.0.1:6379> ZRANGE paihangbang 0 10  #超出范围不报错
1) "gangtiexia"
2) "zhuluoji"
3) "nezha"
4) "zhanlang"
127.0.0.1:6379> ZRANGE zset1 1 3
1) "v2"
2) "v3"
3) "v4"
127.0.0.1:6379> ZRANGE zset1 0 2
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> ZRANGE zset1 2 2
1) "v3"
```

##### 2.5.4.5 返回某个数值的索引(排名)

```bash
127.0.0.1:6379> ZADD paihangbang 90 nezha 199 zhanlang 60 zhuluoji 30 gangtiexia
(integer) 4
127.0.0.1:6379> ZRANK paihangbang zhanlang
(integer) 3   #第4个
127.0.0.1:6379> ZRANK paihangbang zhuluoji
(integer) 1   #第2个
```

##### 2.5.4.6 获取分数

```bash
127.0.0.1:6379> zscore paihangbang gangtiexia
"30"
```

##### 2.5.4.7 删除元素

```bash
127.0.0.1:6379> ZADD paihangbang 90 nezha 199 zhanlang 60 zhuluoji 30 gangtiexia
(integer) 4
127.0.0.1:6379> ZRANGE paihangbang 0 -1
1) "gangtiexia"
2) "zhuluoji"
3) "nezha"
4) "zhanlang"
127.0.0.1:6379> ZREM paihangbang zhuluoji zhanlang
(integer) 2
127.0.0.1:6379> ZRANGE paihangbang 0 -1
1) "gangtiexia"
2) "nezha"
```

#### 2.5.5 哈希 hash

hash 即字典, 是一个string类型的字段(field)和值(value)的映射表，Redis 中每个 hash 可以存储 2^32 -1  键值对，类似于字典，存放了多个k/v 对，hash特别适合用于存储对象场景

##### 2.5.5.1 生成 hash key

格式:

```bash
HSET hash field value
时间复杂度： O(1)
将哈希表 hash 中域 field 的值设置为 value 。

如果给定的哈希表并不存在， 那么一个新的哈希表将被创建并执行 HSET 操作。
如果域 field 已经存在于哈希表中， 那么它的旧值将被新值 value 覆盖。
```

范例: 

```bash
127.0.0.1:6379> HSET 9527 name zhouxingxing age 20
(integer) 2
127.0.0.1:6379> TYPE 9527
hash
#查看所有字段的值
127.0.0.1:6379> hgetall 9527
1) "name"
2) "zhouxingxing"
3) "age"
4) "20"
#增加字段
127.0.0.1:6379> HSET 9527 gender male
(integer) 1
127.0.0.1:6379> hgetall 9527
1) "name"
2) "zhouxingxing"
3) "age"
4) "20"
5) "gender"
6) "male"
```

##### 2.5.5.2 获取hash key的对应字段的值

```bash
127.0.0.1:6379> HGET 9527 name
"zhouxingxing"
127.0.0.1:6379> HGET 9527 age
"20"

127.0.0.1:6379> HMGET 9527 name age  #获取多个值
1) "zhouxingxing"
2) "20"
127.0.0.1:6379> 
```

##### 2.5.5.3 删除一个hash key 的对应字段

```bash
127.0.0.1:6379> HDEL 9527 age
(integer) 1
127.0.0.1:6379> HGET 9527 age
(nil)
127.0.0.1:6379> hgetall 9527
1) "name"
2) "zhouxingxing"
127.0.0.1:6379> HGET 9527 name
"zhouxingxing"
```

##### 2.5.5.4 批量设置hash key的多个field和value

```bash
127.0.0.1:6379> HMSET 9527 name zhouxingxing age 50 city hongkong
OK
127.0.0.1:6379> HGETALL 9527
1) "name"
2) "zhouxingxing"
3) "age"
4) "50"
5) "city"
6) "hongkong"
```

##### 2.5.5.5 获取hash中指定字段的值

```bash
127.0.0.1:6379> HMSET 9527 name zhouxingxing age 50 city hongkong
OK
127.0.0.1:6379> HMGET 9527 name age 
1) "zhouxingxing"
2) "50"
127.0.0.1:6379> 
```

##### 2.5.5.6 获取hash中的所有字段名field

```bash
127.0.0.1:6379> HMSET 9527 name zhouxingxing age 50 city hongkong #重新设置
OK
127.0.0.1:6379> HKEYS 9527
1) "name"
2) "age"
3) "city"
```

##### 2.5.5.7 获取hash key对应所有field的value

```bash
127.0.0.1:6379> HMSET 9527 name zhouxingxing age 50 city hongkong
OK
127.0.0.1:6379> HVALS 9527
1) "zhouxingxing"
2) "50"
3) "hongkong"
```

##### 2.5.5.7 获取指定hash key 的所有field及value

```bash
127.0.0.1:6379> HGETALL 9527
1) "name"
2) "zhouxingxing"
3) "age"
4) "50"
5) "city"
6) "hongkong"
127.0.0.1:6379>
```

##### 2.5.5.8 删除 hash

```bash
127.0.0.1:6379> DEL 9527
(integer) 1
127.0.0.1:6379> HMGET 9527 name city
1) (nil)
2) (nil)
127.0.0.1:6379> EXISTS 9527
(integer) 0
```

### 2.6 消息队列

消息队列: 把要传输的数据放在队列中

功能: 可以实现多个系统之间的解耦,异步,削峰/限流等 

常用的消息队列应用: kafka,rabbitMQ,redis 

消息队列主要分为两种,这两种模式Redis都支持 

* 生产者/消费者模式 
* 发布者/订阅者模式

#### 2.6.1 生产者消费者模式

在生产者/消费者(Producer/Consumer)模式下，上层应用接收到的外部请求后开始处理其当前步骤的操 作，在执行完成后将已经完成的操作发送至指定的频道(channel,逻辑队列)当中，并由其下层的应用监听 该频道并继续下一步的操作，如果其处理完成后没有下一步的操作就直接返回数据给外部请求，如果还 有下一步的操作就再将任务发布到另外一个频道，由另外一个消费者继续监听和处理。此模式应用广泛

##### 2.6.1.1 模式介绍

生产者消费者模式下，多个消费者同时监听一个队列，但是一个消息只能被最先抢到消息的消费者消 费，即消息任务是一次性读取和处理，此模式在分布式业务架构中很常用，比较常用的消息队列软件还有RabbitMQ、Kafka、RocketMQ、ActiveMQ等。

##### 2.6.1.2 队列介绍

队列当中的消息由不同的生产者写入，也会有不同的消费者取出进行消费处理，但是一个消息一定是只能被取出一次也就是被消费一次。

##### 2.6.1.3 生产者发布消息

```bash
[root@redis ~]# redis-cli
127.0.0.1:6379> AUTH 123456
OK
127.0.0.1:6379> LPUSH channel1 msg1 #从管道的左侧写入
(integer) 1
127.0.0.1:6379> LPUSH channel1 msg2
(integer) 2
127.0.0.1:6379> LPUSH channel1 msg3
(integer) 3
127.0.0.1:6379> LPUSH channel1 msg4
(integer) 4
127.0.0.1:6379> LPUSH channel1 msg5
(integer) 5
```

##### 2.6.1.4 查看队列所有消息

```bash
127.0.0.1:6379> LRANGE channel1 0 -1
1) "msg5"
2) "msg4"
3) "msg3"
4) "msg2"
5) "msg1"
```

##### 2.6.1.5 消费者消费消息

```bash
127.0.0.1:6379> RPOP channel1 	#从管道的右侧消费，用于消息的先进先出
"msg1"
127.0.0.1:6379> RPOP channel1
"msg2"
127.0.0.1:6379> RPOP channel1
"msg3"
127.0.0.1:6379> RPOP channel1
"msg4"
127.0.0.1:6379> RPOP channel1
"msg5"
127.0.0.1:6379> RPOP channel1
(nil)
```

##### 2.6.1.6 再次验证队列消息

```bash
127.0.0.1:6379> LRANGE channel1 0 -1
(empty list or set) 			#队列中的消息已经被已全部消费完毕
```

#### 2.6.2 发布者订阅模式

##### 2.6.2.1 模式简介

在发布者订阅者模式下，发布者将消息发布到指定的channel里面，凡是监听该channel的消费者都会收 到同样的一份消息，这种模式类似于是收音机的广播模式，即凡是收听某个频道的听众都会收到主持人 发布的相同的消息内容。此模式常用语群聊天、群通知、群公告等场景

* Publisher：发布者
* Subscriber：订阅者 
* Channel：频道

##### 2.6.2.2 订阅者监听频道

```bash
[root@redis ~]# redis-cli
127.0.0.1:6379> AUTH 123456
OK
127.0.0.1:6379> SUBSCRIBE channel1 #订阅者事先订阅指定的频道，之后发布的消息才能收到
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "channel1"
3) (integer) 1
```

##### 2.6.2.3 发布者发布消息

```bash
127.0.0.1:6379> PUBLISH channel1 test1 	  #发布者发布消息
(integer) 2   							#订阅者个数
127.0.0.1:6379> PUBLISH channel1 test2
(integer) 2
```

##### 2.6.2.4 各个订阅者都能收到消息

```bash
127.0.0.1:6379> subscribe channel1
```

##### 2.6.2.5 订阅多个频道

```bash
#订阅指定的多个频道
127.0.0.1:6379> SUBSCRIBE channel1 channel2
```

##### 2.6.2.6 订阅所有频道

```bash
127.0.0.1:6379> PSUBSCRIBE *  #支持通配符*
```

##### 2.6.2.7 订阅匹配的频道

```bash
127.0.0.1:6379> PSUBSCRIBE chann* #匹配订阅多个频道
```

##### 2.6.2.8 取消订阅

```bash
127.0.0.1:6379> unsubscribe channel1
1) "unsubscribe"
2) "channel1"
3) (integer) 0
```

## 3 redis 集群与高可用

虽然Redis可以实现单机的数据持久化，但无论是RDB也好或者AOF也好，都解决不了单点宕机问题，即 一旦单台 redis服务器本身出现系统故障、硬件故障等问题后，就会直接造成数据的丢失 

此外,单机的性能也是有极限的,因此需要使用另外的技术来解决单点故障和性能扩展的问题。

### 3.1 redis 主从复制

#### 3.1.1 redis 主从复制架构

主从模式（master/slave），可以实现Redis数据的跨主机备份。 

程序端连接到高可用负载的VIP，然后连接到负载服务器设置的Redis后端real server，此模式不需要在 程序里面配 置Redis服务器的真实IP地址，当后期Redis服务器IP地址发生变更只需要更改redis 相应的 后端real server即可， 可避免更改程序中的IP地址设置。

主从复制特点 

* 一个master可以有多个slave 
* 一个slave只能有一个master 
* 数据流向是从master到slave单向的

#### 3.1.2 主从复制实现

Redis Slave 也要开启持久化并设置和master同样的连接密码，因为后期slave会有提升为master的可 能,Slave 端切换master同步后会丢失之前的所有数据,而通过持久化可以恢复数据

一旦某个Slave成为一个master的slave，Redis Slave服务会清空当前redis服务器上的所有数据并将 master的数据导入到自己的内存，但是如果只是断开同步关系后,则不会删除当前已经同步过的数据。

当配置Redis复制功能时，强烈建议打开主服务器的持久化功能。否则的话，由于延迟等问题，部署的主节点Redis服务应该要避免自动启动。

参考案例: 导致主从服务器数据全部丢失

```bash
1.假设节点A为主服务器，并且关闭了持久化。并且节点B和节点c从节点A复制数据
2.节点A崩溃，然后由自动拉起服务重启了节点A.由于节点A的持久化被关闭了，所以重启之后没有任何数据
3.节点B和节点c将从节点A复制数据，但是A的数据是空的，于是就把自身保存的数据副本删除。
```

在关闭主服务器上的持久化，并同时开启自动拉起进程的情况下，即便使用Sentinel来实现Redis的高可用性，也是非常危险的。因为主服务器可能拉起得非常快，以至于Sentinel在配置的心跳时间间隔内没 有检测到主服务器已被重启，然后还是会执行上面的数据丢失的流程。无论何时，数据安全都是极其重要的，所以应该禁止主服务器关闭持久化的同时自动启动。

##### 3.1.2.1 命令行配置

###### 3.1.2.1.1 启用主从同步

默认redis 状态为master，需要转换为slave角色并指向master服务器的IP+PORT+Password

在从节点执行 REPLICAOF MASTER_IP PORT 指令可以启用主从同步复制功能,早期版本使用 SLAVEOF  指令

```bash
127.0.0.1:6379> REPLICAOF MASTER_IP PORT
127.0.0.1:6379> CONFIG SET masterauth <masterpass>
```

```bash
#在mater上设置key1
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> AUTH 123456
OK
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:0
master_replid:a3504cab4d33e9723a7bc988ff8e022f6d9325bf
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> SET key1 v1-master
OK
127.0.0.1:6379> KEYS *
1) "key1"
127.0.0.1:6379> GET key1
"v1-master"
127.0.0.1:6379>

#以下都在slave上执行，登录
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> info 
NOAUTH Authentication required.
127.0.0.1:6379> AUTH 123456
OK
127.0.0.1:6379> INFO replication  #查看当前角色默认为master 
# Replication
role:master
connected_slaves:0
master_replid:a3504cab4d33e9723a7bc988ff8e022f6d9325bf
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> SET key1 v1-slave-18
OK
127.0.0.1:6379> KEYS *
1) "key1"
127.0.0.1:6379> GET key1
"v1-slave-18"
127.0.0.1:6379> 

#在第二个slave,也设置相同的key1,但值不同
127.0.0.1:6379> KEYS *
1) "key1"
127.0.0.1:6379> GET key1
"v1-slave-28"
127.0.0.1:6379>

127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:0
master_replid:a3504cab4d33e9723a7bc988ff8e022f6d9325bf
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379>

#在slave上设置master的IP和端口，4.0版之前的指令为slaveof 
127.0.0.1:6379> REPLICAOF 10.0.0.8 6379 #仍可使用SLAVEOF MasterIP Port
OK
#在slave上设置master的密码，才可以同步
127.0.0.1:6379> CONFIG SET masterauth 123456
OK
127.0.0.1:6379> INFO replication
# Replication   #角色变为slave
role:slave
master_host:10.0.0.8   #指向master
master_port:6379
master_link_status:up
master_last_io_seconds_ago:8
master_sync_in_progress:0
slave_repl_offset:42
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:b69908f23236fb20b810d198f7f4539f795e0ee5
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:42
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:42
#查看已经同步成功
127.0.0.1:6379> GET key1
"v1-master"

#在master上可以看到所有slave信息
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:2
slave0:ip=10.0.0.18,port=6379,state=online,offset=112,lag=1   #slave信息
slave1:ip=10.0.0.28,port=6379,state=online,offset=112,lag=1
master_replid:dc30f86c2d3c9029b6d07831ae3f27f8dbacac62
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:112
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:112
127.0.0.1:6379>
```

###### 3.1.2.1.2 删除主从同步

在从节点执行 REPLIATOF NO ONE 指令可以取消主从复制

```bash
#取消复制,在slave上执行REPLIATOF NO ONE,会断开和master的连接不再主从复制, 但不会清除slave上已有的数据
127.0.0.1:6379> REPLICAOF no one
```

##### 3.1.2.2 同步日志

###### 3.1.2.2.1 在 master 上观察日志

```bash
[root@centos8 ~]#tail /var/log/redis/redis.log 
24402:M 06 Oct 2020 09:09:16.448 * Replica 10.0.0.18:6379 asks forsynchronization
24402:M 06 Oct 2020 09:09:16.448 * Full resync requested by replica 10.0.0.18:6379
24402:M 06 Oct 2020 09:09:16.448 * Starting BGSAVE for SYNC with target: disk
24402:M 06 Oct 2020 09:09:16.453 * Background saving started by pid 24507
24507:C 06 Oct 2020 09:09:16.454 * DB saved on disk
24507:C 06 Oct 2020 09:09:16.455 * RDB: 2 MB of memory used by copy-on-write
24402:M 06 Oct 2020 09:09:16.489 * Background saving terminated with success
24402:M 06 Oct 2020 09:09:16.490 * Synchronization with replica 10.0.0.18:6379 succeeded
```

###### 3.1.2.2.2 在 slave 节点观察日志

```bash
[root@centos8 ~]#tail -f /var/log/redis/redis.log 
24395:S 06 Oct 2020 09:09:16.411 * Connecting to MASTER 10.0.0.8:6379
24395:S 06 Oct 2020 09:09:16.412 * MASTER <-> REPLICA sync started
24395:S 06 Oct 2020 09:09:16.412 * Non blocking connect for SYNC fired the event.
24395:S 06 Oct 2020 09:09:16.412 * Master replied to PING, replication can continue...
24395:S 06 Oct 2020 09:09:16.414 * Partial resynchronization not possible (no cached master)
24395:S 06 Oct 2020 09:09:16.419 * Full resync from master: 20ec2450b850782b6eeaed4a29a61a25b9a7f4da:0
24395:S 06 Oct 2020 09:09:16.456 * MASTER <-> REPLICA sync: receiving 196 bytes from master
24395:S 06 Oct 2020 09:09:16.456 * MASTER <-> REPLICA sync: Flushing old data
24395:S 06 Oct 2020 09:09:16.456 * MASTER <-> REPLICA sync: Loading DB in memory
24395:S 06 Oct 2020 09:09:16.457 * MASTER <-> REPLICA sync: Finished with success
```

##### 3.1.2.3 修改slave节点配置文件

```bash
# 范例
[root@centos8 ~]#vim /etc/redis.conf 
 .......
# replicaof <masterip> <masterport>
replicaof 10.0.0.8 6379 #指定master的IP和端口号
# If the master is password protected (using the "requirepass" configuration
# directive below) it is possible to tell the replica to authenticate before
# starting the replication synchronization process, otherwise the master will
# refuse the replica request.
# masterauth <master-password>
masterauth 123456     #如果密码需要设置
.......
[root@centos8 ~]#systemctl restart redis
```

##### 3.1.2.4 master和slave查看状态

```bash
#在master上查看状态
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.18,port=6379,state=online,offset=1104403,lag=0
master_replid:b2517cd6cb3ad1508c516a38caed5b9d2d9a3e73
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1104403
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:55828
repl_backlog_histlen:1048576
127.0.0.1:6379>

#在slave上查看状态
127.0.0.1:6379> get key1    #同步成功后,slave上的key信息丢失,从master复制过来新的值
"v1-master"
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:6
master_sync_in_progress:0
slave_repl_offset:1104431
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:b2517cd6cb3ad1508c516a38caed5b9d2d9a3e73
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1104431
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:55856
repl_backlog_histlen:1048576
127.0.0.1:6379>

#停止master的redis服务：systemctl stop redis,在slave上可以观察到以下现象
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:down  #显示down，表示无法连接master
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:1104529
master_link_down_since_seconds:4
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:b2517cd6cb3ad1508c516a38caed5b9d2d9a3e73
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1104529
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:55954
repl_backlog_histlen:1048576
127.0.0.1:6379>
```

##### 3.1.2.5 slave 日志

```bash
[root@centos8 ~]#tail -f /var/log/redis/redis.log 
24592:S 20 Feb 2020 12:03:58.792 * Connecting to MASTER 10.0.0.8:6379
24592:S 20 Feb 2020 12:03:58.792 * MASTER <-> REPLICA sync started
24592:S 20 Feb 2020 12:03:58.797 * Non blocking connect for SYNC fired the event.
24592:S 20 Feb 2020 12:03:58.797 * Master replied to PING, replication can continue...
24592:S 20 Feb 2020 12:03:58.798 * Partial resynchronization not possible (no cached master)
24592:S 20 Feb 2020 12:03:58.801 * Full resync from master: b69908f23236fb20b810d198f7f4539f795e0ee5:2440
24592:S 20 Feb 2020 12:03:58.863 * MASTER <-> REPLICA sync: receiving 213 bytes from master
24592:S 20 Feb 2020 12:03:58.863 * MASTER <-> REPLICA sync: Flushing old data
24592:S 20 Feb 2020 12:03:58.863 * MASTER <-> REPLICA sync: Loading DB in memory
24592:S 20 Feb 2020 12:03:58.863 * MASTER <-> REPLICA sync: Finished with success
```

##### 3.1.2.6 Master日志

```bash
[root@centos8 ~]#tail /var/log/redis/redis.log 
11846:M 20 Feb 2020 12:11:35.171 * DB loaded from disk: 0.000 seconds
11846:M 20 Feb 2020 12:11:35.171 * Ready to accept connections
11846:M 20 Feb 2020 12:11:36.086 * Replica 10.0.0.18:6379 asks forsynchronization
11846:M 20 Feb 2020 12:11:36.086 * Partial resynchronization not accepted: 
Replication ID mismatch (Replica asked for
'b69908f23236fb20b810d198f7f4539f795e0ee5', my replication IDs are 
'4bff970970c073c1f3d8e8ad20b1c1f126a5f31c' and 
'0000000000000000000000000000000000000000')
11846:M 20 Feb 2020 12:11:36.086 * Starting BGSAVE for SYNC with target: disk
11846:M 20 Feb 2020 12:11:36.095 * Background saving started by pid 11850
11850:C 20 Feb 2020 12:11:36.121 * DB saved on disk
11850:C 20 Feb 2020 12:11:36.121 * RDB: 4 MB of memory used by copy-on-write
11846:M 20 Feb 2020 12:11:36.180 * Background saving terminated with success
11846:M 20 Feb 2020 12:11:36.180 * Synchronization with replica 10.0.0.18:6379 succeeded
```

##### 3.1.2.7 slave 状态只读无法写入数据

```bash
127.0.0.1:6379> set key1 v1-slave
(error) READONLY You can't write against a read only replica.
```

#### 3.1.3 主从复制故障恢复

##### 3.1.3.1 主从复制故障恢复过程介绍

###### 3.1.3.1.1 slave 节点故障和恢复 

当 slave 节点故障时，将Redis Client指向另一个 slave 节点即可,并及时修复故障从节点

###### 3.1.3.1.2 master 节点故障和恢复

需要提升slave为新的master

master故障后，只能手动提升一个slave为新master，不支持自动切换。 

之后将其它的slave节点重新指定新的master为master节点 

Master的切换会导致master_replid发生变化，slave之前的master_replid就和当前master不一致从而 会引发所有 slave的全量同步。

##### 3.1.3.2 主从复制故障恢复实现

假设当前主节点10.0.0.8故障,提升10.0.0.18为新的master

```bash
#查看当前10.0.0.18节点的状态为slave,master指向10.0.0.8
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:3794
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:8e8279e461fdf0f1a3464ef768675149ad4b54a3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:3794
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:3781
repl_backlog_histlen:14
127.0.0.1:6379>
```

停止slave同步并提升为新的master

```bash
#将当前 slave 节点提升为 master 角色
127.0.0.1:6379> REPLICAOF NO ONE   #旧版使用SLAVEOF no one
OK
(5.04s)
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:0
master_replid:94901d6b8ff812ec4a4b3ac6bb33faa11e55c274
master_replid2:0083e5a9c96aa4f2196934e10b910937d82b4e19
master_repl_offset:3514
second_repl_offset:3515
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:3431
repl_backlog_histlen:84
127.0.0.1:6379>
```

测试能否写入数据：

```bash
127.0.0.1:6379> set keytest1 vtest1
OK
```

修改所有slave 指向新的master节点

```bash
#修改10.0.0.28节点指向新的master节点10.0.0.18
127.0.0.1:6379> SLAVEOF 10.0.0.8 6379
OK
127.0.0.1:6379> set key100 v100
(error) READONLY You can't write against a read only replica.

#查看日志
[root@centos8 ~]#tail -f /var/log/redis/redis.log 
1762:S 20 Feb 2020 13:28:21.943 # Connection with master lost.
1762:S 20 Feb 2020 13:28:21.943 * Caching the disconnected master state.
1762:S 20 Feb 2020 13:28:21.943 * REPLICAOF 10.0.0.18:6379 enabled (user request 
from 'id=5 addr=127.0.0.1:59668 fd=9 name= age=149 idle=0 flags=N db=0 sub=0 
psub=0 multi=-1 qbuf=41 qbuf-free=32727 obl=0 oll=0 omem=0 events=r 
cmd=slaveof')
1762:S 20 Feb 2020 13:28:21.966 * Connecting to MASTER 10.0.0.18:6379
1762:S 20 Feb 2020 13:28:21.966 * MASTER <-> REPLICA sync started
1762:S 20 Feb 2020 13:28:21.967 * Non blocking connect for SYNC fired the event.
1762:S 20 Feb 2020 13:28:21.968 * Master replied to PING, replication can continue...
1762:S 20 Feb 2020 13:28:21.968 * Trying a partial resynchronization (request 8e8279e461fdf0f1a3464ef768675149ad4b54a3:3991).
1762:S 20 Feb 2020 13:28:21.969 * Successful partial resynchronization with master.
1762:S 20 Feb 2020 13:28:21.969 * MASTER <-> REPLICA sync: Master accepted a Partial Resynchronization.
```

在新master可看到slave

```bash
#在新master节点10.0.0.18上查看状态
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.28,port=6379,state=online,offset=4606,lag=0
master_replid:8e8279e461fdf0f1a3464ef768675149ad4b54a3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:4606
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:4606
127.0.0.1:6379>
```

#### 3.1.4 实现 redis 的级联复制

即实现基于Slave节点的Slave

master和slave1节点无需修改,只需要修改slave2及slave3指向slave1做为mater即可

```bash
#在slave2和slave3上执行下面指令
127.0.0.1:6379> REPLICAOF 10.0.0.18 6379
OK
127.0.0.1:6379> CONFIG SET masterauth 123456
```

在 master 设置key,观察是否同步

```bash
#在master新建key
127.0.0.1:6379> set key2 v2
OK
127.0.0.1:6379> get key2
"v2"

#在slave1和slave2验证key
127.0.0.1:6379> get key2
"v2"

#在slave1和slave2都无法新建key
127.0.0.1:6379> set key3 v3
(error) READONLY You can't write against a read only replica.
```

在中间那个slave查看状态

```bash
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:8 #最近一次与master通信已经过去多少秒。
master_sync_in_progress:0  #是否正在与master通信。
slave_repl_offset:4312  #当前同步的偏移量
slave_priority:100   #slave优先级，master故障后值越小越优先同步。
slave_read_only:1
connected_slaves:1
slave0:ip=10.0.0.28,port=6379,state=online,offset=4312,lag=0 #slave的slave节点
master_replid:8e8279e461fdf0f1a3464ef768675149ad4b54a3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:4312
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:4312
```

#### 3.1.5 主从复制优化

Redis主从复制分为全量同步和增量同步

##### 3.1.5.1 全量复制过程

首次主从同步是全量同步，主从同步可以让从服务器从主服务器同步数据，而且从服务器还可再有其它 的从服务器，即另外一台redis服务器可以从一台从服务器进行数据同步，redis 的主从同步是非阻塞 的，master收到从服务器的psync(2.8版本之前是SYNC)命令,会fork一个子进程在后台执行bgsave命 令，并将新写入的数据写入到一个缓冲区中，bgsave执行完成之后,将生成的RDB文件发送给slave，然 后master再将缓冲区的内容以redis协议格式再全部发送给slave，slave 先删除旧数据,slave将收到后的 RDB文件载入自己的内存，再加载所有收到缓冲区的内容 从而这样一次完整的数据同步

Redis全量复制一般发生在Slave首次初始化阶段，这时Slave需要将Master上的所有数据都复制一份。

##### 3.1.5.2 增量复制过程

全量同步之后再次需要同步时,从服务器只要发送当前的offset位置(等同于MySQL的binlog的位置)给主 服务器，然后主服务器根据相应的位置将之后的数据(包括写在缓冲区的积压数据)发送给从服务器,再次 将其保存到从节点内存即可。

##### 3.1.5.3 主从同步完整过程

具体主从同步过程如下：

```bash
1）从服务器连接主服务器，发送PSYNC命令
2）主服务器接收到PSYNC命令后，开始执行BGSAVE命令生成RDB快照文件并使用缓冲区记录此后执行的所有写命令
3）主服务器BGSAVE执行完后，向所有从服务器发送RDB快照文件，并在发送期间继续记录被执行的写命令
4）从服务器收到快照文件后丢弃所有旧数据，载入收到的快照至内存
5）主服务器快照发送完毕后,开始向从服务器发送缓冲区中的写命令
6）从服务器完成对快照的载入，开始接收命令请求，并执行来自主服务器缓冲区的写命令
7）后期同步会先发送自己slave_repl_offset位置，只同步新增加的数据，不再全量同步
```

复制缓冲区(环形队列)配置参数：

```bash
#复制缓冲区大小,建议要设置足够大
repl-backlog-size 1mb 
#Redis同时也提供了当没有slave需要同步的时候，多久可以释放环形队列：
repl-backlog-ttl   3600
```

##### 3.1.5.4 避免全量复制

* 第一次全量复制不可避免,后续的全量复制可以利用小主节点(内存小),业务低峰时进行全量 
* 节点运行ID不匹配:主节点重启会导致RUNID变化,可能会触发全量复制,可以利用故障转移，例如哨兵或集群,而从节点重启动,不会导致全量复制 
* 复制积压缓冲区不足: 当主节点生成的新数据大于缓冲区大小,从节点恢复和主节点连接后,会导致全量复制.解决方法将repl-backlog-size 调大

##### 3.1.5.5 避免复制风暴

* 单主节点复制风暴 
  * 当主节点重启，多从节点复制 
  * 解决方法:更换复制拓扑

* 单机器多实例复制风暴 
  * 机器宕机后，大量全量复制 
  * 解决方法:主节点分散多机器

##### 3.1.5.6 主从同步优化配置

Redis在2.8版本之前没有提供增量部分复制的功能，当网络闪断或者slave Redis重启之后会导致主从之 间的全量同步，即从2.8版本开始增加了部分复制的功能。

**性能相关配置**

```bash
repl-diskless-sync no # 是否使用无盘同步RDB文件，默认为no，no为不使用无盘，需要将RDB文件保存到磁盘后再发送给slave，yes为支持无盘，支持无盘就是RDB文件不需要保存至本地磁盘，而且直接通过socket文件发送给slave
repl-diskless-sync-delay 5 #diskless时复制的服务器等待的延迟时间
repl-ping-slave-period 10 #slave端向server端发送ping的时间间隔，默认为10秒
repl-timeout 60 #设置主从ping连接超时时间,超过此值无法连接,master_link_status显示为down,并记录错误日志

repl-disable-tcp-nodelay no #是否启用TCP_NODELAY，如设置成yes，则redis会合并小的TCP包从而节省带宽， 但会增加同步延迟（40ms），造成master与slave数据不一致，假如设置成no，则redis master会立即发送同步数据，没有延迟，yes关注网络性能，no关注redis服务中的数据一致性

repl-backlog-size 1mb #master的写入数据缓冲区，用于记录自上一次同步后到下一次同步过程中间的写入命令，计算公式：repl-backlog-size = 允许从节点最大中断时长 * 主实例offset每秒写入量，比如master每秒最大写入64mb，最大允许60秒，那么就要设置为64mb*60秒=3840MB(3.8G),建议此值是设置的足够大

repl-backlog-ttl 3600 #如果一段时间后没有slave连接到master，则backlog size的内存将会被释放。如果值为0则 表示永远不释放这部份内存。

slave-priority 100 #slave端的优先级设置，值是一个整数，数字越小表示优先级越高。当master故障时将会按照优先级来选择slave端进行恢复，如果值设置为0，则表示该slave永远不会被选择。

min-replicas-to-write 1 #设置一个master的可用slave不能少于多少个，否则master无法执行写
min-slaves-max-lag 20 #设置至少有上面数量的slave延迟时间都大于多少秒时，master不接收写操作(拒绝写入)
```

#### 3.1.6 常见主从复制故障汇总

##### 3.1.6.1 master密码不对

即配置的master密码不对，导致验证不通过而无法建立主从同步关系。

```bash
[root@centos8 ~]#tail -f /var/log/redis/redis.log 
24930:S 20 Feb 2020 13:53:57.029 * Connecting to MASTER 10.0.0.8:6379
24930:S 20 Feb 2020 13:53:57.030 * MASTER <-> REPLICA sync started
24930:S 20 Feb 2020 13:53:57.030 * Non blocking connect for SYNC fired the event.
24930:S 20 Feb 2020 13:53:57.030 * Master replied to PING, replication can continue...
24930:S 20 Feb 2020 13:53:57.031 # Unable to AUTH to MASTER: -ERR invalid password
```

##### 3.1.6.2 Redis版本不一致

不同的redis大版本之间存在兼容性问题，比如：3和4，4和5之间，因此各master和slave之间必须保持版本一致

##### 3.1.6.3 无法远程连接

在开启了安全模式情况下，没有设置bind地址或者密码

```bash
[root@centos8 ~]#vim /etc/redis.conf 
#bind 127.0.0.1   #将此行注释
[root@centos8 ~]#systemctl restart redis
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q       Local Address:Port         Peer 
Address:Port     
LISTEN       0             128                 0.0.0.0:22        		0.0.0.0:*       
LISTEN       0             100               127.0.0.1:25               0.0.0.0:*       
LISTEN       0             128                 0.0.0.0:6379             0.0.0.0:*       
LISTEN       0             128                   [::]:22                   [::]:*       
LISTEN       0             100                   [::1]:25                   [::]:*       
LISTEN       0             128                   [::]:6379                 [::]:*       
[root@centos8 ~]#redis-cli -h 10.0.0.8
10.0.0.8:6379> KEYS *
(error) DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
10.0.0.38:6379> exit

#可以本机登录
[root@centos8 ~]#redis-cli 
127.0.0.1:6379> KEYS *
(empty list or set)
```

##### 3.1.6.4 配置不一致

主从节点的maxmemory不一致,主节点内存大于从节点内存,主从复制可能丢失数据

rename-command 命令不一致,如在主节点定义了fushall,flushdb,从节点没定义,结果执行flushdb,不同步

```bash
#master有一个rename-command flushdb "magedu",而slave没有这个配置,则同步时从节点可以看到以下同步错误
3181:S 21 Oct 2020 17:34:50.581 # == CRITICAL == This replica is sending an 
error to its master: 'unknown command `magedu`, with args beginning with: ' after processing the command '<unknown>'
```

### 3.2 redis 哨兵（Sentinel）

#### 3.2.1 redis 集群介绍

主从架构无法实现master和slave角色的自动切换，即当master出现redis服务异常、主机断电、磁盘损 坏等问题导致master无法使用，而redis主从复制无法实现自动的故障转移(将slave 自动提升为新 master)，需要手动修改环境配置,才能切换到slave redis服务器，另外当单台Redis服务器性能无法满足 业务写入需求的时候,也无法横向扩展Redis服务的并行写入性能

需要解决以上的两个核心问题：

* master和slave角色的无缝切换，让业务无感知从而不影响业务使用 
* 可横向动态扩展Redis服务器，从而实现多台服务器并行写入以实现更高并发的目的。

Redis 集群实现方式： 

* 客户端分片: 由应用决定将不同的KEY发送到不同的Redis服务器 
* 代理分片: 由代理决定将不同的KEY发送到不同的Redis服务器,代理程序如:codis,twemproxy等 
* Redis Cluster

#### 3.2.2 哨兵 (Sentinel) 工作原理

##### 3.2.2.1 sentinel 架构和故障转移

Sentinel 进程是用于监控redis集群中Master主服务器工作的状态，在Master主服务器发生故障的时候，可以实现Master和Slave服务器的切换，保证系统的高可用，此功能在redis2.6+的版本已引用， Redis的哨兵模式到了2.8版本之后就稳定了下来。一般在生产环境也建议使用Redis的2.8版本的以后版本 

哨兵(Sentinel) 是一个分布式系统，可以在一个架构中运行多个哨兵(sentinel) 进程，这些进程使用流言协议(gossip protocols)来接收关于Master主服务器是否下线的信息，并使用投票协议(Agreement  Protocols)来决定是否执行自动故障迁移,以及选择哪个Slave作为新的Master 

每个哨兵(Sentinel)进程会向其它哨兵(Sentinel)、Master、Slave定时发送消息，以确认对方是否”活” 着，如果发现对方在指定配置时间(此项可配置)内未得到回应，则暂时认为对方已离线，也就是所谓的” 主观认为宕机” (主观:是每个成员都具有的独自的而且可能相同也可能不同的意识)，英文名称： Subjective Down，简称SDOWN 

有主观宕机，对应的有客观宕机。当“哨兵群”中的多数Sentinel进程在对Master主服务器做出SDOWN  的判断，并且通过 SENTINEL is-master-down-by-addr 命令互相交流之后，得出的Master Server下线 判断，这种方式就是“客观宕机”(客观:是不依赖于某种意识而已经实际存在的一切事物)，英文名称是： Objectively Down， 简称 ODOWN 

通过一定的vote算法，从剩下的slave从服务器节点中，选一台提升为Master服务器节点，然后自动修改相关配置，并开启故障转移（failover） 

Sentinel 机制可以解决master和slave角色的自动切换问题，但单个 Master 的性能瓶颈问题无法解决, 类似于MySQL中的MHA功能 

Redis Sentinel中的Sentinel节点个数应该为大于等于3且最好为奇数 

客户端初始化时连接的是Sentinel节点集合，不再是具体的Redis节点，但Sentinel只是配置中心不是代理。 

Redis Sentinel 节点与普通redis 没有区别,要实现读写分离依赖于客户端程序 

redis 3.0 之前版本中,生产环境一般使用哨兵模式,3.0后推出redis cluster功能,可以支持更大规模的生产环境

##### 3.2.2.2 sentinel中的三个定时任务

* 每10秒每个sentinel对master和slave执行info 

  发现slave节点 

  确认主从关系 

* 每2秒每个sentinel通过master节点的channel交换信息(pub/sub) 

  通过sentinel__:hello频道交互 

  交互对节点的“看法”和自身信息 

* 每1秒每个sentinel对其他sentinel和redis执行ping

#### 3.2.3 实现哨兵

##### 3.2.3.1 哨兵的准备实现主从复制架构

哨兵的前提是已经实现了一个redis的主从复制的运行环境，从而实现一个一主两从基于哨兵的高可用 redis架构 

注意: master 的配置文件中masterauth 和slave 都必须相同

**所有主从节点的redis.conf中关健配置**

范例: 准备主从环境配置

```bash
#在所有主从节点执行
[root@centos8 ~]#dnf -y install redis
[root@centos8 ~]#vim /etc/redis.conf
bind 0.0.0.0
masterauth "123456"
requirepass "123456"

#或者非交互执行
[root@centos8 ~]#sed -i -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e 's/^# masterauth .*/masterauth 123456/' -e 's/^# requirepass .*/requirepass 123456/' /etc/redis.conf

#在所有从节点执行
[root@centos8 ~]#echo "replicaof 10.0.0.8 6379" >> /etc/redis.conf

#在所有主从节点执行
[root@centos8 ~]#systemctl enable --now redis
```

master服务器状态

```bash
[root@redis-master ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not 
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:2
slave0:ip=10.0.0.28,port=6379,state=online,offset=112,lag=1
slave1:ip=10.0.0.18,port=6379,state=online,offset=112,lag=0
master_replid:8fdca730a2ae48fb9c8b7e739dcd2efcc76794f3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:112
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:112
127.0.0.1:6379>
```

配置slave1

```bash
[root@redis-slave1 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> REPLICAOF 10.0.0.8 6379
OK
127.0.0.1:6379> CONFIG SET masterauth "123456"
OK
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:4
master_sync_in_progress:0
slave_repl_offset:140
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:8fdca730a2ae48fb9c8b7e739dcd2efcc76794f3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:140
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:99
repl_backlog_histlen:42
```

配置slave2

```bash
[root@redis-slave2 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> REPLICAOF 10.0.0.8 6379
OK
127.0.0.1:6379> CONFIG SET masterauth "123456"
OK
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:3
master_sync_in_progress:0
slave_repl_offset:182
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:8fdca730a2ae48fb9c8b7e739dcd2efcc76794f3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:182
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:15
repl_backlog_histlen:168
127.0.0.1:6379>
```

##### 3.2.3.2 编辑哨兵的配置文件

**sentinel配置** 

Sentinel实际上是一个特殊的redis服务器,有些redis指令支持,但很多指令并不支持.默认监听在 26379/tcp端口. 

哨兵可以不和Redis服务器部署在一起，但一般部署在一起以节约成本 

所有redis节点使用相同的以下示例的配置文件

```bash
#如果是编译安装，在源码目录有sentinel.conf，复制到安装目录即可，
如:/apps/redis/etc/sentinel.conf
[root@centos8 ~]#vim /etc/redis-sentinel.conf 
bind 0.0.0.0
port 26379
daemonize yes
pidfile "redis-sentinel.pid"
logfile "sentinel_26379.log"
dir "/tmp"  #工作目录

sentinel monitor mymaster 10.0.0.8 6379 2
#mymaster是集群的名称，此行指定当前mymaster集群中master服务器的地址和端口
#2为法定人数限制(quorum)，即有几个sentinel认为master down了就进行故障转移，一般此值是所有sentinel节点(一般总数是>=3的 奇数,如:3,5,7等)的一半以上的整数值，比如，总数是3，即3/2=1.5，取整为2,是master的ODOWN客观下线的依据

sentinel auth-pass mymaster 123456
#mymaster集群中master的密码，注意此行要在上面行的下面

sentinel down-after-milliseconds mymaster 30000
#(SDOWN)判断mymaster集群中所有节点的主观下线的时间，单位：毫秒，建议3000

sentinel parallel-syncs mymaster 1
#发生故障转移后，可以同时向新master同步数据的slave的数量，数字越小总同步时间越长，但可以减轻新master的负载压力

sentinel failover-timeout mymaster 180000
#所有slaves指向新的master所需的超时时间，单位：毫秒
sentinel deny-scripts-reconfig yes #禁止修改脚本

logfile /var/log/redis/sentinel.log
```

三个哨兵服务器的配置都如下

```bash
[root@redis-master ~]#grep -vE "^#|^$" /etc/redis-sentinel.conf 
port 26379
daemonize no
pidfile "/var/run/redis-sentinel.pid"
logfile "/var/log/redis/sentinel.log"
dir "/tmp"
sentinel monitor mymaster 10.0.0.8 6379 2   #修改此行
sentinel auth-pass mymaster 123456 #增加此行
sentinel down-after-milliseconds mymaster 3000   #修改此行
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
sentinel deny-scripts-reconfig yes

#以下内容自动生成，不需要修改
sentinel myid 50547f34ed71fd48c197924969937e738a39975b  
#此行自动生成必须唯一,修改此值需重启redis和sentinel服务
.....
# Generated by CONFIG REWRITE
protected-mode no
supervised systemd
sentinel leader-epoch mymaster 0
sentinel known-replica mymaster 10.0.0.28 6379
sentinel known-replica mymaster 10.0.0.18 6379
sentinel current-epoch 0

[root@redis-master ~]#scp /etc/redis-sentinel.conf redis-slave1:/etc/
[root@redis-master ~]#scp /etc/redis-sentinel.conf redis-slave2:/etc/
```

##### 3.2.3.3 启动哨兵

三台哨兵服务器都要启动

```bash
#确保每个哨兵主机myid不同
[root@redis-slave1 ~]#vim /etc/redis-sentinel.conf
sentinel myid 50547f34ed71fd48c197924969937e738a39975c
[root@redis-slave2 ~]#vim /etc/redis-sentinel.conf
sentinel myid 50547f34ed71fd48c197924969937e738a39975d 

[root@redis-master ~]#systemctl enable --now redis-sentinel.service
[root@redis-slave1 ~]#systemctl enable --now redis-sentinel.service
[root@redis-slave2 ~]#systemctl enable --now redis-sentinel.service

#如果是编译安装在所有节点生成新的service文件
[root@redis-master ~]#cat /lib/systemd/system/redis-sentinel.service
[Unit]
Description=Redis Sentinel
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-sentinel /apps/redis/etc/redis-sentinel.conf --
supervised systemd
ExecStop=/bin/kill -s QUIT $MAINPID
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target

#注意所有节点的目录权限,否则无法启动服务
[root@redis-master ~]#chown -R redis.redis /apps/redis/
```

如果是编译安装,在所有哨兵服务器执行下面操作启动哨兵

```bash
#vim /apps/redis/etc/sentinel.conf
bind 0.0.0.0
port 26379
daemonize yes
pidfile "redis-sentinel.pid"
Logfile "sentinel_26379.log"
dir "/apps/redis/data"
sentinel monitor mymaster 10.0.0.8 6379 2
sentinel auth-pass mymaster 123456
sentinel down-after-milliseconds mymaster 15000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
sentinel deny-scripts-reconfig yes

#/apps/redis/bin/redis-sentinel /apps/redis/etc/sentinel.conf
```

##### 3.2.3.4 验证哨兵端口

```bash
[root@redis-master ~]#ss -ntl
State       Recv-Q       Send-Q     Local Address:Port     Peer Address:Port    
LISTEN       0             128               0.0.0.0:22            0.0.0.0:*         
LISTEN       0             128               0.0.0.0:26379         0.0.0.0:*         
LISTEN       0             128               0.0.0.0:6379          0.0.0.0:*         
LISTEN       0             128                 [::]:22               [::]:*         
LISTEN       0             128                 [::]:26379           [::]:*       
LISTEN       0             128                 [::]:6379             [::]:* 
```

##### 3.2.3.5 查看哨兵日志

master的哨兵日志

```bash
[root@redis-master ~]#tail -f /var/log/redis/sentinel.log 
38028:X 20 Feb 2020 17:13:08.702 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
38028:X 20 Feb 2020 17:13:08.702 # Redis version=5.0.3, bits=64, 
commit=00000000, modified=0, pid=38028, just started
38028:X 20 Feb 2020 17:13:08.702 # Configuration loaded
38028:X 20 Feb 2020 17:13:08.702 * supervised by systemd, will signal readiness
38028:X 20 Feb 2020 17:13:08.703 * Running mode=sentinel, port=26379.
38028:X 20 Feb 2020 17:13:08.703 # WARNING: The TCP backlog setting of 511 
cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
38028:X 20 Feb 2020 17:13:08.704 # Sentinel ID is 50547f34ed71fd48c197924969937e738a39975b
38028:X 20 Feb 2020 17:13:08.704 # +monitor master mymaster 10.0.0.8 6379 quorum 2
38028:X 20 Feb 2020 17:13:08.709 * +slave slave 10.0.0.28:6379 10.0.0.28 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:13:08.709 * +slave slave 10.0.0.18:6379 10.0.0.18 6379 @ mymaster 10.0.0.8 6379
```

slave的哨兵日志

```bash
[root@redis-slave1 ~]#tail -f /var/log/redis/sentinel.log 
25509:X 20 Feb 2020 17:13:27.435 * Removing the pid file.
25509:X 20 Feb 2020 17:13:27.435 # Sentinel is now ready to exit, bye bye...
25572:X 20 Feb 2020 17:13:27.448 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
25572:X 20 Feb 2020 17:13:27.448 # Redis version=5.0.3, bits=64, 
commit=00000000, modified=0, pid=25572, just started
25572:X 20 Feb 2020 17:13:27.448 # Configuration loaded
25572:X 20 Feb 2020 17:13:27.448 * supervised by systemd, will signal readiness
25572:X 20 Feb 2020 17:13:27.449 * Running mode=sentinel, port=26379.
25572:X 20 Feb 2020 17:13:27.449 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
25572:X 20 Feb 2020 17:13:27.449 # Sentinel ID is 50547f34ed71fd48c197924969937e738a39975b
25572:X 20 Feb 2020 17:13:27.449 # +monitor master mymaster 10.0.0.8 6379 quorum 2
```

##### 3.2.3.6 当前sentinel状态

在sentinel状态中尤其是最后一行，涉及到masterIP是多少，有几个slave，有几个sentinels，必须是符合全部服务器数量

```bash
[root@redis-master ~]#redis-cli -p 26379
127.0.0.1:26379> INFO sentinel
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=10.0.0.8:6379,slaves=2,sentinels=3 #两个slave,三个sentinel服务器,如果sentinels值不符合,检查myid可能冲突
```

##### 3.2.3.7 停止Redis Master 节点测试故障转移

```bash
[root@redis-master ~]#killall redis-server
```

查看各节点上哨兵信息：

```bash
[root@redis-master ~]#redis-cli -a 123456 -p 26379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:26379> INFO sentinel
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=10.0.0.18:6379,slaves=2,sentinels=2
```

故障转移时sentinel的信息：

```bash
[root@redis-master ~]#tail -f /var/log/redis/sentinel.log 
38028:X 20 Feb 2020 17:42:27.362 # +sdown master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.418 # +odown master mymaster 10.0.0.8 6379 #quorum 2/2
38028:X 20 Feb 2020 17:42:27.418 # +new-epoch 1
38028:X 20 Feb 2020 17:42:27.418 # +try-failover master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.419 # +vote-for-leader 50547f34ed71fd48c197924969937e738a39975b 1
38028:X 20 Feb 2020 17:42:27.422 # 50547f34ed71fd48c197924969937e738a39975d voted for 50547f34ed71fd48c197924969937e738a39975b 1
38028:X 20 Feb 2020 17:42:27.475 # +elected-leader master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.475 # +failover-state-select-slave master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.529 # +selected-slave slave 10.0.0.18:6379 10.0.0.18 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.529 * +failover-state-send-slaveof-noone slave 10.0.0.18:6379 10.0.0.18 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:27.613 * +failover-state-wait-promotion slave 10.0.0.18:6379 10.0.0.18 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.506 # +promoted-slave slave 10.0.0.18:6379 10.0.0.18 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.506 # +failover-state-reconf-slaves master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.582 * +slave-reconf-sent slave 10.0.0.28:6379 10.0.0.28 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.736 * +slave-reconf-inprog slave 10.0.0.28:6379 10.0.0.28 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.736 * +slave-reconf-done slave 10.0.0.28:6379 10.0.0.28 6379 @ mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.799 # +failover-end master mymaster 10.0.0.8 6379
38028:X 20 Feb 2020 17:42:28.799 # +switch-master mymaster 10.0.0.8 6379 10.0.0.18 6379
38028:X 20 Feb 2020 17:42:28.799 * +slave slave 10.0.0.28:6379 10.0.0.28 6379 @ mymaster 10.0.0.18 6379
38028:X 20 Feb 2020 17:42:28.799 * +slave slave 10.0.0.8:6379 10.0.0.8 6379 @ mymaster 10.0.0.18 6379
38028:X 20 Feb 2020 17:42:31.809 # +sdown slave 10.0.0.8:6379 10.0.0.8 6379 @ mymaster 10.0.0.18 6379
```

##### 3.2.3.8 故障转移后的redis配置文件会被自动修改

故障转移后redis.conf中的replicaof行的master IP会被修改

```bash
[root@redis-slave2 ~]#grep ^replicaof /etc/redis.conf 
replicaof 10.0.0.18 6379
```

哨兵配置文件的sentinel monitor IP 同样也会被修改

```bash
[root@redis-slave1 ~]#grep "^[a-Z]" /etc/redis-sentinel.conf
port 26379
daemonize no
pidfile "/var/run/redis-sentinel.pid"
logfile "/var/log/redis/sentinel.log"
dir "/tmp"
sentinel myid 50547f34ed71fd48c197924969937e738a39975b
sentinel deny-scripts-reconfig yes
sentinel monitor mymaster 10.0.0.18 6379 2  #自动修改此行
sentinel down-after-milliseconds mymaster 3000
sentinel auth-pass mymaster 123456
sentinel config-epoch mymaster 1
protected-mode no
supervised systemd
sentinel leader-epoch mymaster 1
sentinel known-replica mymaster 10.0.0.8 6379
sentinel known-replica mymaster 10.0.0.28 6379
sentinel known-sentinel mymaster 10.0.0.28 26379
50547f34ed71fd48c197924969937e738a39975d
sentinel current-epoch 1

[root@redis-slave2 ~]#grep "^[a-Z]" /etc/redis-sentinel.conf
port 26379
daemonize no
pidfile "/var/run/redis-sentinel.pid"
logfile "/var/log/redis/sentinel.log"
dir "/tmp"
sentinel myid 50547f34ed71fd48c197924969937e738a39975d
sentinel deny-scripts-reconfig yes
sentinel monitor mymaster 10.0.0.18 6379 2  #自动修改此行
sentinel down-after-milliseconds mymaster 3000
sentinel auth-pass mymaster 123456
sentinel config-epoch mymaster 1
protected-mode no
supervised systemd
sentinel leader-epoch mymaster 1
sentinel known-replica mymaster 10.0.0.28 6379  
sentinel known-replica mymaster 10.0.0.8 6379
sentinel known-sentinel mymaster 10.0.0.8 26379
50547f34ed71fd48c197924969937e738a39975b
sentinel current-epoch 1
```

##### 3.2.3.9 当前 redis状态

新的master 状态

```bash
[root@redis-slave1 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> INFO replication
# Replication
role:master   #提升为master
connected_slaves:1
slave0:ip=10.0.0.28,port=6379,state=online,offset=56225,lag=1
master_replid:75e3f205082c5a10824fbe6580b6ad4437140b94
master_replid2:b2fb4653bdf498691e5f88519ded65b6c000e25c
master_repl_offset:56490
second_repl_offset:46451
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:287
repl_backlog_histlen:5620
```

另一个slave指向新的master

```bash
[root@redis-slave2 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.18  #指向新的master
master_port:6379
master_link_status:up
master_last_io_seconds_ago:0
master_sync_in_progress:0
slave_repl_offset:61029
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:75e3f205082c5a10824fbe6580b6ad4437140b94
master_replid2:b2fb4653bdf498691e5f88519ded65b6c000e25c
master_repl_offset:61029
second_repl_offset:46451
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:61029
```

##### 3.2.3.10 恢复故障的原master重新加入redis集群

```bash
[root@redis-master ~]#cat /etc/redis.conf 
#sentinel会自动修改下面行指向新的master
replicaof 10.0.0.18 6379   
```

在原 master上观察状态

```bash
[root@redis-master ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> INFO replication
# Replication
role:slave
master_host:10.0.0.18
master_port:6379
master_link_status:up
master_last_io_seconds_ago:0
master_sync_in_progress:0
slave_repl_offset:764754
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:75e3f205082c5a10824fbe6580b6ad4437140b94
master_replid2:b2fb4653bdf498691e5f88519ded65b6c000e25c
master_repl_offset:764754
second_repl_offset:46451
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:46451
repl_backlog_histlen:718304
[root@redis-master ~]#redis-cli -p 26379
127.0.0.1:26379> INFO sentinel
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=10.0.0.18:6379,slaves=2,sentinels=3
127.0.0.1:26379> 
```

观察新master上状态和日志

```bash
[root@redis-slave1 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
127.0.0.1:6379> INFO replication
# Replication
role:master
connected_slaves:2
slave0:ip=10.0.0.28,port=6379,state=online,offset=769027,lag=0
slave1:ip=10.0.0.8,port=6379,state=online,offset=769027,lag=0
master_replid:75e3f205082c5a10824fbe6580b6ad4437140b94
master_replid2:b2fb4653bdf498691e5f88519ded65b6c000e25c
master_repl_offset:769160
second_repl_offset:46451
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:287
repl_backlog_histlen:768874
127.0.0.1:6379> 

[root@redis-slave1 ~]#tail -f /var/log/redis/sentinel.log 
25717:X 20 Feb 2020 17:42:33.757 # +sdown slave 10.0.0.8:6379 10.0.0.8 6379 @ mymaster 10.0.0.18 6379
25717:X 20 Feb 2020 18:41:29.566 # -sdown slave 10.0.0.8:6379 10.0.0.8 6379 @ mymaster 10.0.0.18 6379
```

##### 3.2.3.11 sentinel 运维

手动让主节点下线

```http
sentinel failover <masterName>
```

范例: 手动故障转移

```bash
[root@centos8 ~]#vim /etc/redis.conf
replica-priority 10 #指定优先级,值越小sentinel会优先将之选为新的master,默为值为100
[root@centos8 ~]#redis-cli   -p 26379
127.0.0.1:26379> sentinel failover mymaster 
OK
```

##### 3.2.3.12 应用程序如何连接 redis

Redis 官方客户端：https://redis.io/clients

###### 3.2.3.12.1 客户端连接 sentinel 工作原理

1. 客户端获取sentinel节点集合,选举出一个sentinel
2. 由这个sentinel 通过masterName 获取master节点信息,客户端通过sentinel get-master-addr-byname master-name这个api来获取对应主节点信息
3. 客户端发送role指令确认mater的信息,验证当前获取的“主节点”是真正的主节点，这样的目的是为 了防止故障转移期间主节点的变化
4. 客户端保持和sentinel节点集合的联系，即订阅sentinel节点相关频道，时刻获取关于主节点的相 关信息,获取新的master 信息变化,并自动连接新的master

###### 3.2.2.12.2 java 连接Sentinel哨兵

java 客户端连接Redis：https://github.com/xetorthio/jedis/blob/master/pom.xml

```bash
#jedis/pom.xml 配置连接redis 
<properties>
 <redis-hosts>localhost:6379,localhost:6380,localhost:6381,localhost:6382,localhost:6383
,localhost:6384,localhost:6385</redis-hosts>
 <sentinel-hosts>localhost:26379,localhost:26380,localhost:26381</sentinel-hosts>
 <cluster-hosts>localhost:7379,localhost:7380,localhost:7381,localhost:7382,localhost:7383
,localhost:7384,localhost:7385</cluster-hosts>
   <github.global.server>github</github.global.server>
 </properties>
```

java客户端连接单机的redis是通过Jedis来实现的，java代码用的时候只要创建Jedis对象就可以建多个 Jedis连接池来连接redis，应用程序再直接调用连接池即可连接Redis。而Redis为了保障高可用,服务一 般都是Sentinel部署方式，当Redis服务中的主服务挂掉之后,会仲裁出另外一台Slaves服务充当 Master。这个时候,我们的应用即使使用了Jedis 连接池,如果Master服务挂了,应用将还是无法连接新的 Master服务，为了解决这个问题, Jedis也提供了相应的Sentinel实现,能够在Redis Sentinel主从切换时 候,通知应用,把应用连接到新的Master服务。

Redis Sentinel的使用也是十分简单的,只是在JedisPool中添加了Sentinel和MasterName参数，JRedis  Sentinel底层基于Redis订阅实现Redis主从服务的切换通知，当Reids发生主从切换时，Sentinel会发送 通知主动通知Jedis进行连接的切换，JedisSentinelPool在每次从连接池中获取链接对象的时候,都要对连 接对象进行检测,如果此链接和Sentinel的Master服务连接参数不一致,则会关闭此连接,重新获取新的 Jedis连接对象。

###### 3.2.2.12.3 python 连接Sentinel哨兵

```bash
[root@centos8 ~]#yum -y install python3 python3-redis
[root@centos8 ~]#cat sentinel_test.py
#!/usr/bin/python3
import redis
from redis.sentinel import Sentinel
#连接哨兵服务器(主机名也可以用域名)
sentinel = Sentinel([('10.0.0.8', 26379),
                     ('10.0.0.18', 26379),
                     ('10.0.0.28', 26379)],
                    socket_timeout=0.5)
redis_auth_pass = '123456'
#mymaster 是配置哨兵模式的redis集群名称，此为默认值,实际名称按照个人部署案例来填写
#获取主服务器地址
master = sentinel.discover_master('mymaster')
print(master)
#获取从服务器地址
slave = sentinel.discover_slaves('mymaster')
print(slave)
#获取主服务器进行写入
master = sentinel.master_for('mymaster', socket_timeout=0.5, 
password=redis_auth_pass, db=0)
w_ret = master.set('name', 'wang')
#输出：True
#获取从服务器进行读取（默认是round-roubin）
slave = sentinel.slave_for('mymaster', socket_timeout=0.5, 
password=redis_auth_pass, db=0)
r_ret = slave.get('name')
print(r_ret)
#输出：wang
[root@centos8 ~]#chmod +x sentinel_test.py
[root@centos8 ~]#./sentinel_test.py
('10.0.0.8', 6379)
[('10.0.0.18', 6379), ('10.0.0.28', 6379)]
b'wang'
```

### 3.3 Redis Cluster

#### 3.3.1 Redis Cluster 工作原理

在哨兵sentinel机制中，可以解决redis高可用问题，即当master故障后可以自动将slave提升为 master，从而可以保证redis服务的正常使用，但是无法解决redis单机写入的瓶颈问题，即单机redis写 入性能受限于单机的内存大小、并发数量、网卡速率等因素。

为了解决单机性能的瓶颈，提高Redis 性能，可以使用分布式集群的解决方案

早期Redis 分布式集群部署方案：

1. 客户端分区：由客户端程序决定key写分配和写入的redis node，但是需要客户端自己实现写入分 配、高可用管理和故障转移等 
2. 代理方案：基于三方软件实现redis proxy，客户端先连接之代理层，由代理层实现key的写入分 配，对客户端来说是有比较简单，但是对于集群管节点增减相对比较麻烦，而且代理本身也是单点 和性能瓶颈。

redis 3.0版本之后推出了无中心架构的redis cluster机制，在无中心的redis集群当中，其每个节点保存 当前节点数据和整个集群状态,每个节点都和其他所有节点连接

Redis Cluster特点如下：

1. 所有Redis节点使用(PING机制)互联 
2. 集群中某个节点的是否失效，是由整个集群中超过半数的节点监测都失效，才能算真正的失效 
3. 客户端不需要proxy即可直接连接redis，应用程序中需要配置有全部的redis服务器IP 
4. redis cluster把所有的redis node 平均映射到 0-16383个槽位(slot)上，读写需要到指定的redis  node上进行操作，因此有多少个redis node相当于redis 并发扩展了多少倍，每个redis node 承担 16384/N个槽位 
5. Redis cluster预先分配16384个(slot)槽位，当需要在redis集群中写入一个key -value的时候，会使 用CRC16(key) mod 16384之后的值，决定将key写入值哪一个槽位从而决定写入哪一个Redis节点 上，从而有效解决单机瓶颈。

#### 3.3.2 Redis cluster 架构

##### 3.3.2.1 Redis cluster 基本架构

假如三个主节点分别是：A, B, C 三个节点，采用哈希槽 (hash slot)的方式来分配16384个slot 的话 

它们三个节点分别承担的slot 区间可以是：

```bash
节点A覆盖 0－5460
节点B覆盖 5461－10922
节点C覆盖 10923－16383
```

##### 3.3.2.2 Redis cluster 主从架构

Redis cluster的架构虽然解决了并发的问题，但是又引入了一个新的问题，每个Redis master的高可用 如何解决？那就是对每个master 节点都实现主从复制,从而实现 redis 高可用性

##### 3.3.2.3 Redis Cluster 部署架构说明

环境A：3台服务器，每台服务器启动6379和6380两个redis 服务实例，适用于测试环境

环境B：6台服务器，分别是三组master/slave，适用于生产环境

```bash
#集群节点
10.0.0.8 
10.0.0.18
10.0.0.28
10.0.0.38
10.0.0.48 
10.0.0.58
#预留服务器扩展使用
10.0.0.68 
10.0.0.78
```

说明：Redis 5.X 和之前版本相比有很多变化，以下分别介绍两个版本5.X和4.X的配置

##### 3.3.2.4 部署方式介绍

redis cluster 有多种部署方法 

* 原生命令安装 

  理解Redis Cluster架构 

  生产环境不使用 

* 官方工具安装 

  高效、准确 

  生产环境可以使用 

* 自主研发 

  可以实现可视化的自动化部署

#### 3.3.3 原生命令手动部署

##### 3.3.3.1 原生命令手动部署过程

* 在所有节点安装redis,并配置开启cluster功能 
* 各个节点执行meet,实现所有节点的相互通信 
* 为各个master 节点指派槽位范围 
* 指定各个节点的主从关系

##### 3.3.3.2 实战案例: 利用原生命令手动部署redis cluster

###### 3.3.3.2.1 在所有节点安装redis并启动cluster功能

```bash
#在所有6个节点上都执行下面相同操作
[root@centos8 ~]#dnf -y install redis
#手动修改配置文件
[root@centos8 ~]vim /etc/redis.conf
bind 0.0.0.0
masterauth 123456   #建议配置，否则后期的master和slave主从复制无法成功，还需再配置
requirepass 123456
cluster-enabled yes #取消此行注释,必须开启集群，开启后redis 进程会有cluster标识
cluster-config-file nodes-6379.conf #取消此行注释,此为集群状态文件,记录主从关系及slot范围信息,由redis cluster 集群自动创建和维护
cluster-require-full-coverage no   #默认值为yes,设为no可以防止一个节点不可用导致整个cluster不可能
#或者批量修改配置文件
[root@centos8 ~]#sed -i.bak -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e '/masterauth/a masterauth 123456' -e '/# requirepass/a requirepass 123456' -e '/# cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file nodes-6379.conf/a cluster-config-file nodes-6379.conf' -e '/cluster-require-full-coverage yes/c cluster-require-full-coverage no' /etc/redis.conf
[root@centos8 ~]#systemctl enable --now redis
```

###### 3.3.3.2.2 执行meet 操作实现相互通信

```bash
#在任一节点上和其它所有节点进行meet通信
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster meet 10.0.0.18 6379
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster meet 10.0.0.28 6379
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster meet 10.0.0.38 6379
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster meet 10.0.0.48 6379
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster meet 10.0.0.58 6379
#可以看到所有节点之间可以相互连接通信
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602515365000 3 connected
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602515367093 1 connected
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 master - 01602515365057 0 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 master - 01602515365000 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 master - 01602515365000 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602515366074 2 connected

#由于没有槽位无法创建key
[root@centos8 ~]#redis-cli -a 123456 --no-auth-warning set name wang
(error) CLUSTERDOWN Hash slot not served

#查看当前状态
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster 
info
cluster_state:fail
cluster_slots_assigned:0   #无槽位分配置
cluster_slots_ok:0
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:0              #无集群成员
cluster_current_epoch:5
cluster_my_epoch:3
cluster_stats_messages_ping_sent:584
cluster_stats_messages_pong_sent:145
cluster_stats_messages_meet_sent:8
cluster_stats_messages_sent:737
cluster_stats_messages_ping_received:145
cluster_stats_messages_pong_received:151
cluster_stats_messages_received:296
```

###### 3.3.3.2.3 为各个master 节点指派槽位范围

```bash
#创建添加槽位的脚本
[root@centos8 ~]#cat addslot.sh
#!/bin/bash
#********************************************************************
host=$1
port=$2
start=$3
end=$4
pass=123456
for slot in `seq ${start} ${end}`;do
    echo slot:$slot
   	redis-cli -h ${host} -p $port -a ${pass} --no-auth-warning cluster addslots 
${slot}
done

#为三个master分配槽位,共16364/3=5,461.333333333333,平均每个master分配5,461个槽位
[root@centos8 ~]#bash addslot.sh 10.0.0.8 6379 0 5461
[root@centos8 ~]#bash addslot.sh 10.0.0.18 6379 5462 10922
[root@centos8 ~]#bash addslot.sh 10.0.0.28 6379 10923 16383

#当第一个master分配完槽位后,可以看到下面信息
[root@centos8 ~]#redis-cli -a 123456 --no-auth-warning cluster info
cluster_state:ok
cluster_slots_assigned:5462  #分配槽位数
cluster_slots_ok:5462
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:1     #加入集群
cluster_current_epoch:5
cluster_my_epoch:3
cluster_stats_messages_ping_sent:1234
cluster_stats_messages_pong_sent:782
cluster_stats_messages_meet_sent:8
cluster_stats_messages_sent:2024
cluster_stats_messages_ping_received:782
cluster_stats_messages_pong_received:801
cluster_stats_messages_received:1583

#当第一个master分配完槽位后,可以看到下面信息
[root@centos8 ~]#redis-cli -a 123456 --no-auth-warning cluster nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 0
1602516039000 3 connected 0-5461
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 0
1602516044606 1 connected
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 master - 0
1602516042000 0 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 master - 0
1602516041575 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 master - 0
1602516042585 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 0
1602516043595 2 connected

#当所有三个节点都分配槽位后可以创建key
[root@centos8 ~]#redis-cli -a 123456 --no-auth-warning set name wang 
(error) MOVED 5798 10.0.0.18:6379
[root@centos8 ~]#redis-cli -h 10.0.0.18 -a 123456 --no-auth-warning set name mage 
OK
[root@centos8 ~]#redis-cli -h 10.0.0.18 -a 123456 --no-auth-warning get name
"mage"

#当所有的三个master分配完槽位后,可以看到下面信息,所有节点都是master
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602516633000 3 connected 0-5461
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602516635862 1 connected 5462-10922
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 master - 01602516635000 0 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 master - 01602516635000 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 master - 01602516634852 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602516636872 2 connected 10923-16383

[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster 
nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602516633000 3 connected 0-5461
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602516635862 1 connected 5462-10922
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 master - 01602516635000 0 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 master - 01602516635000 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 master - 01602516634852 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602516636872 2 connected 10923-16383
```

###### 3.3.3.2.4 指定各个节点的主从关系

```bash
#通过上面cluster nodes 查看master的ID信息,执行下面操作,将对应的slave 指定相应的master节点,实现三对主从节点
#将10.0.0.38指定10.0.0.8的ID做为其从节点
[root@centos8 ~]#redis-cli -h 10.0.0.38 -a 123456 --no-auth-warning cluster replicate a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab
OK
#将10.0.0.48指定10.0.0.18的ID做为其从节点
[root@centos8 ~]#redis-cli -h 10.0.0.48 -a 123456 --no-auth-warning cluster replicate 97c5dcc3f33c2fc75c7fdded25d05d2930a312c0
OK
#将10.0.0.58指定10.0.0.28的ID做为其从节点
[root@centos8 ~]#redis-cli -h 10.0.0.58 -a 123456 --no-auth-warning cluster replicate 4f146b1ac51549469036a272c60ea97f065ef832
OK

#在第一组主从节点创建成功后,可以看到下面信息
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602517124000 3 connected 0-5461
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602517123000 1 connected 5462-10922
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 slave 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 0 1602517125709 3 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 master - 01602517124689 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 master - 01602517123676 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602517123000 2 connected 10923-16383

#在第一组主从节点创建成功后,可以看到下面信息
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning info replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.38,port=6379,state=online,offset=322,lag=1
master_replid:7af8303230e2939cc22943e991f06c6409356c6e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:322
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:322

[root@centos8 ~]#redis-cli -h 10.0.0.38 -a 123456 --no-auth-warning info replication
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:10
master_sync_in_progress:0
slave_repl_offset:336
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:7af8303230e2939cc22943e991f06c6409356c6e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:336
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:336

#所有三组主从节点创建成功后,可以看到最终结果
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster nodes
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602517611000 3 connected 0-5461
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602517614000 1 connected 5462-10922
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 slave 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 0 1602517615000 3 connected
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 slave 
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 0 1602517616011 4 connected
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 slave 
4f146b1ac51549469036a272c60ea97f065ef832 0 1602517613966 5 connected
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602517617034 2 connected 10923-16383

[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:5
cluster_my_epoch:3
cluster_stats_messages_ping_sent:2813
cluster_stats_messages_pong_sent:2346
cluster_stats_messages_meet_sent:8
cluster_stats_messages_sent:5167
cluster_stats_messages_ping_received:2346
cluster_stats_messages_pong_received:2380
cluster_stats_messages_received:4726

[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning info replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.38,port=6379,state=online,offset=1022,lag=1
master_replid:7af8303230e2939cc22943e991f06c6409356c6e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1022
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:1022

[root@centos8 ~]#redis-cli -h 10.0.0.18 -a 123456 --no-auth-warning info replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.48,port=6379,state=online,offset=182,lag=1
master_replid:e4a8394213bd865a800c9326224584f8cb52f169
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:182
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:182

[root@centos8 ~]#redis-cli -h 10.0.0.28 -a 123456 --no-auth-warning info replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.58,port=6379,state=online,offset=252,lag=0
master_replid:6d5e8f898e9023cfa0b7fe006ce42142175895e7
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:252
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:252

#查看主从节关系及槽位信息
[root@centos8 ~]#redis-cli -h 10.0.0.28 -a 123456 --no-auth-warning cluster slots
1) 1) (integer) 10923
   2) (integer) 16383
   3) 1) "10.0.0.28"
      2) (integer) 6379
      3) "4f146b1ac51549469036a272c60ea97f065ef832"
   4) 1) "10.0.0.58"
      2) (integer) 6379
      3) "07231a50043d010426c83f3b0788e6b92e62050f"
2) 1) (integer) 0
   2) (integer) 5461
   3) 1) "10.0.0.8"
      2) (integer) 6379
      3) "a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab"
   4) 1) "10.0.0.38"
      2) (integer) 6379
      3) "cb20d58870fe05de8462787cf9947239f4bc5629"
3) 1) (integer) 5462
   2) (integer) 10922
   3) 1) "10.0.0.18"
      2) (integer) 6379
      3) "97c5dcc3f33c2fc75c7fdded25d05d2930a312c0"
   4) 1) "10.0.0.48"
      2) (integer) 6379
      3) "779a24884dbe1ceb848a685c669ec5326e6c8944"
```

###### 3.3.3.2.5 验证 redis cluster 访问

```bash
#指定选项 -c 表示以集群方式连接
[root@centos8 ~]#redis-cli -c -h 10.0.0.8 -a 123456 --no-auth-warning set name wang
OK
[root@centos8 ~]#redis-cli -c -h 10.0.0.8 -a 123456 --no-auth-warning get name
"wang"

#非集群方式连接
[root@centos8 ~]#redis-cli   -h 10.0.0.8 -a 123456 --no-auth-warning get name
(error) MOVED 5798 10.0.0.18:6379
[root@centos8 ~]#redis-cli   -h 10.0.0.18 -a 123456 --no-auth-warning get name
"wang"
[root@centos8 ~]#redis-cli   -h 10.0.0.28 -a 123456 --no-auth-warning get name
(error) MOVED 5798 10.0.0.18:6379
```

#### 3.3.4 实战案例：基于Redis 5 的 redis cluster 部署

官方文档：https://redis.io/topics/cluster-tutorial

**redis cluster 相关命令** 

范例: 查看 --cluster 选项帮助

```bash
[root@centos8 ~]#redis-cli --cluster help
Cluster Manager Commands:
 create         host1:port1 ... hostN:portN
                 --cluster-replicas <arg>
 check         host:port
                 --cluster-search-multiple-owners
 info           host:port
 fix           host:port
                 --cluster-search-multiple-owners
 reshard       host:port
                 --cluster-from <arg>
                 --cluster-to <arg>
                 --cluster-slots <arg>
                 --cluster-yes
                 --cluster-timeout <arg>
                 --cluster-pipeline <arg>
                 --cluster-replace
 rebalance     host:port
                 --cluster-weight <node1=w1...nodeN=wN>
                 --cluster-use-empty-masters
                 --cluster-timeout <arg>
                 --cluster-simulate
                 --cluster-pipeline <arg>
                 --cluster-threshold <arg>
                 --cluster-replace
 add-node       new_host:new_port existing_host:existing_port
                 --cluster-slave
                 --cluster-master-id <arg>
 del-node       host:port node_id
 call           host:port command arg arg .. arg
 set-timeout   host:port milliseconds
 import         host:port
                 --cluster-from <arg>
                 --cluster-copy
                 --cluster-replace
 help           
For check, fix, reshard, del-node, set-timeout you can specify the host and port of any working node in the cluster.
```

范例: 查看CLUSTER 指令的帮助

```bash
[root@centos8 ~]#redis-cli CLUSTER HELP
1) CLUSTER <subcommand> arg arg ... arg. Subcommands are:
2) ADDSLOTS <slot> [slot ...] -- Assign slots to current node.
3) BUMPEPOCH -- Advance the cluster config epoch.
4) COUNT-failure-reports <node-id> -- Return number of failure reports for <node-id>.
5) COUNTKEYSINSLOT <slot> - Return the number of keys in <slot>.
6) DELSLOTS <slot> [slot ...] -- Delete slots information from current node.
7) FAILOVER [force|takeover] -- Promote current replica node to being a master.
8) FORGET <node-id> -- Remove a node from the cluster.
9) GETKEYSINSLOT <slot> <count> -- Return key names stored by current node in a slot.
10) FLUSHSLOTS -- Delete current node own slots information.
11) INFO - Return onformation about the cluster.
12) KEYSLOT <key> -- Return the hash slot for <key>.
13) MEET <ip> <port> [bus-port] -- Connect nodes into a working cluster.
14) MYID -- Return the node id.
15) NODES -- Return cluster configuration seen by node. Output format:
16)     <id> <ip:port> <flags> <master> <pings> <pongs> <epoch> <link> <slot> ... <slot>
17) REPLICATE <node-id> -- Configure current node as replica to <node-id>.
18) RESET [hard|soft] -- Reset current node (default: soft).
19) SET-config-epoch <epoch> - Set config epoch of current node.
20) SETSLOT <slot> (importing|migrating|stable|node <node-id>) -- Set slot state.
21) REPLICAS <node-id> -- Return <node-id> replicas.
22) SLOTS -- Return information about slots range mappings. Each range is made of:
23)     start, end, master and replicas IP addresses, ports and ids
```

##### 3.3.4.1 创建 redis cluster集群的环境准备

* 每个redis 节点采用相同的相同的redis版本、相同的密码、硬件配置 
* 所有redis服务器必须没有任何数据 
* 准备六台主机，地址如下：

```http
10.0.0.8
10.0.0.18
10.0.0.28
10.0.0.38
10.0.0.48
10.0.0.58
```

##### 3.3.4.2 启用 redis cluster 配置

所有6台主机都执行以下配置

```bash
[root@centos8 ~]#dnf -y install redis
```

* 每个节点修改redis配置，必须开启cluster功能的参数

```bash
#手动修改配置文件
[root@redis-node1 ~]vim /etc/redis.conf
bind 0.0.0.0
masterauth 123456   #建议配置，否则后期的master和slave主从复制无法成功，还需再配置
requirepass 123456
cluster-enabled yes #取消此行注释,必须开启集群，开启后redis 进程会有cluster标识
cluster-config-file nodes-6379.conf #取消此行注释,此为集群状态文件,记录主从关系及slot范围信息,由redis cluster 集群自动创建和维护
cluster-require-full-coverage no   #默认值为yes,设为no可以防止一个节点不可用导致整个cluster不可能

#或者执行下面命令,批量修改
[root@redis-node1 ~]#sed -i.bak -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e '/masterauth/a masterauth 123456' -e '/# requirepass/a requirepass 123456' -e '/# cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file 
nodes-6379.conf/a cluster-config-file nodes-6379.conf' -e '/cluster-require-full-coverage yes/c cluster-require-full-coverage no' /etc/redis.conf

[root@redis-node1 ~]#systemctl enable --now redis
```

* 验证当前Redis服务状态：

```bash
#开启了16379的cluster的端口,实际的端口=redis port + 10000
[root@centos8 ~]#ss -ntl
State       Recv-Q       Send-Q Local Address:Port     Peer Address:Port    
LISTEN       0             128           0.0.0.0:22            0.0.0.0:*     
LISTEN       0             100         127.0.0.1:25            0.0.0.0:*     
LISTEN       0             128           0.0.0.0:16379         0.0.0.0:*     
LISTEN       0             128           0.0.0.0:6379          0.0.0.0:*     
LISTEN       0             128             [::]:22               [::]:*     
LISTEN       0             100             [::1]:25               [::]:* 

#注意进程有[cluster]状态
[root@centos8 ~]#ps -ef|grep redis
redis   1939    1  0 10:54 ?    00:00:00 /usr/bin/redis-server 0.0.0.0:6379 
[cluster]
root    1955   1335  0 10:57 pts/0    00:00:00 grep --color=auto redis
```

##### 3.3.4.3 创建集群

```bash
#命令redis-cli的选项 --cluster-replicas 1 表示每个master对应一个slave节点
[root@redis-node1 ~]#redis-cli -a 123456 --cluster create 10.0.0.8:6379   
10.0.0.18:6379   10.0.0.28:6379   10.0.0.38:6379   10.0.0.48:6379   
10.0.0.58:6379 --cluster-replicas 1 
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 10.0.0.38:6379 to 10.0.0.8:6379
Adding replica 10.0.0.48:6379 to 10.0.0.18:6379
Adding replica 10.0.0.58:6379 to 10.0.0.28:6379
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379 #带M的为master
   slots:[0-5460] (5461 slots) master #当前master的槽位起始
和结束位
M: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots:[5461-10922] (5462 slots) master
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379  #带S的slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7      
S: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   replicates 99720241248ff0e4c6fa65c2385e92468b3b5993
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
Can I set the above configuration? (type 'yes' to accept): yes #输入yes自动创建集群
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
....
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master #已经分配的槽位
   1 additional replica(s)   #分配了一个slave
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave   #slave没有分配槽位
   replicates d34da8666a6f587283a1c2fca5d13691407f9462  #对应的master的10.0.0.28的
ID
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7 #对应的master的10.0.0.8的ID
S: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots: (0 slots) slave
   replicates 99720241248ff0e4c6fa65c2385e92468b3b5993 #对应的master的10.0.0.18的
ID
M: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.  #所有节点槽位分配完成
>>> Check for open slots... #检查打开的槽位
>>> Check slots coverage... #检查插槽覆盖范围
[OK] All 16384 slots covered. #所有槽位(16384个)分配完成

#观察以上结果，可以看到3组master/slave
master:10.0.0.8---slave:10.0.0.38
master:10.0.0.18---slave:10.0.0.48
master:10.0.0.28---slave:10.0.0.58
```

##### 3.3.4.4 查看主从状态

```bash
[root@redis-node1 ~]#redis-cli -a 123456 -c INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.38,port=6379,state=online,offset=896,lag=1
master_replid:3a388865080d779180ff240cb75766e7e57877da
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:896
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:896

[root@redis-node2 ~]#redis-cli -a 123456 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.48,port=6379,state=online,offset=980,lag=1
master_replid:b9066d3cbf0c5fecc7f4d1d5cb2433999783fa3f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:980
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:980

[root@redis-node2 ~]#redis-cli -a 123456 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.48,port=6379,state=online,offset=980,lag=1
master_replid:b9066d3cbf0c5fecc7f4d1d5cb2433999783fa3f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:980
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:980
repl_backlog_histlen:980

[root@redis-node4 ~]#redis-cli -a 123456 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:slave
master_host:10.0.0.8
master_port:6379
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:1036
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:3a388865080d779180ff240cb75766e7e57877da
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1036
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:1036

[root@redis-node5 ~]#redis-cli -a 123456 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:slave
master_host:10.0.0.18
master_port:6379
master_link_status:up
master_last_io_seconds_ago:2
master_sync_in_progress:0
slave_repl_offset:1064
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:b9066d3cbf0c5fecc7f4d1d5cb2433999783fa3f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1064
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:1064

[root@redis-node6 ~]#redis-cli -a 123456 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
# Replication
role:slave
master_host:10.0.0.28
master_port:6379
master_link_status:up
master_last_io_seconds_ago:7
master_sync_in_progress:0
slave_repl_offset:1078
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:53208e0ed9305d721e2fb4b3180f75c689217902
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:1078
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:1078
```

范例: 查看指定master节点的slave节点信息

```bash
[root@centos8 ~]#redis-cli cluster nodes 
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 01602571565772 12 connected 10923-16383
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 slave 
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 0 1602571565000 11 connected
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 01602571564000 11 connected 5462-10922
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 slave 
4f146b1ac51549469036a272c60ea97f065ef832 0 1602571565000 12 connected
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 01602571566000 10 connected 0-5461
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 slave 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 0 1602571566780 10 connected

#以下命令查看指定master节点的slave节点信息,其中
#a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 为master节点的ID
[root@centos8 ~]#redis-cli cluster slaves 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab
1) "cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 slave 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 0 1602571574844 10 connected"
```

##### 3.3.4.5 验证集群状态

```bash
[root@redis-node1 ~]#redis-cli -a 123456 CLUSTER INFO
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6  #节点数
cluster_size:3                        #三个集群
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:837
cluster_stats_messages_pong_sent:811
cluster_stats_messages_sent:1648
cluster_stats_messages_ping_received:806
cluster_stats_messages_pong_received:837
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:1648

#查看任意节点的集群状态
[root@redis-node1 ~]#redis-cli -a 123456 --cluster info 10.0.0.38:6379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
10.0.0.18:6379 (99720241...) -> 0 keys | 5462 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 0 keys | 5461 slots | 1 slaves.
10.0.0.8:6379 (cb028b83...) -> 0 keys | 5461 slots | 1 slaves.
[OK] 0 keys in 3 masters.
0.00 keys per slot on average.
```

##### 3.3.4.6 查看集群node对应关系

```bash
[root@redis-node1 ~]#redis-cli -a 123456 CLUSTER NODES
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379@16379 slave 
d34da8666a6f587283a1c2fca5d13691407f9462 0 1582344815790 6 connected
f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379@16379 slave 
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 0 1582344811000 4 connected
d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379@16379 slave 
99720241248ff0e4c6fa65c2385e92468b3b5993 0 1582344815000 5 connected
99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379@16379 master - 0 1582344813000 2 connected 5461-10922
d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379@16379 master - 0 1582344814780 3 connected 10923-16383
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379@16379 myself,master - 0 1582344813000 1 connected 0-5460

[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.38:6379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
10.0.0.18:6379 (99720241...) -> 0 keys | 5462 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 0 keys | 5461 slots | 1 slaves.
10.0.0.8:6379 (cb028b83...) -> 0 keys | 5461 slots | 1 slaves.
[OK] 0 keys in 3 masters.
0.00 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.38:6379)
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
S: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots: (0 slots) slave
   replicates 99720241248ff0e4c6fa65c2385e92468b3b5993
M: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

##### 3.3.4.7 验证集群写入key

###### 3.3.4.7.1 redis cluster 写入key

```bash
#经过算法计算，当前key的槽位需要写入指定的node
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.8 SET key1 values1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
(error) MOVED 9189 10.0.0.18:6379    #槽位不在当前node所以无法写入

#指定槽位对应node可写入
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 SET key1 values1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
OK
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 GET key1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
"values1"

#对应的slave节点可以KEYS *,但GET key1失败,可以到master上执行GET key1
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.48 KEYS "*"
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
1) "key1"
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.48 GET key1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
(error) MOVED 9189 10.0.0.18:6379
```

###### 3.3.4.7.2 redis cluster 计算key所属的slot

```bash
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster nodes
4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379@16379 master - 0 1602561649000 12 connected 10923-16383
779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379@16379 slave 
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 0 1602561648000 11 connected
97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379@16379 master - 0 1602561650000 11 connected 5462-10922
07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379@16379 slave 
4f146b1ac51549469036a272c60ea97f065ef832 0 1602561650229 12 connected
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379@16379 myself,master - 0 1602561650000 10 connected 0-5461
cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379@16379 slave 
a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 0 1602561651238 10 connected

#计算得到hello对应的slot
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster keyslot hello
(integer) 866
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning set hello magedu
OK
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning cluster keyslot name 
(integer) 5798
[root@centos8 ~]#redis-cli -h 10.0.0.8 -a 123456 --no-auth-warning set name wang
(error) MOVED 5798 10.0.0.18:6379
[root@centos8 ~]#redis-cli -h 10.0.0.18 -a 123456 --no-auth-warning set name wang
OK
[root@centos8 ~]#redis-cli -h 10.0.0.18 -a 123456 --no-auth-warning get name
"wang"

#使用选项-c 以集群模式连接
[root@centos8 ~]#redis-cli -c -h 10.0.0.8 -a 123456 --no-auth-warning 
10.0.0.8:6379> cluster keyslot linux
(integer) 12299
10.0.0.8:6379> set linux love
-> Redirected to slot [12299] located at 10.0.0.28:6379
OK
10.0.0.28:6379> get linux 
"love"
10.0.0.28:6379> exit
[root@centos8 ~]#redis-cli -h 10.0.0.28 -a 123456 --no-auth-warning get linux
"love"
```

##### 3.3.4.8 python脚本实现RedisCluster集群写入

官网:https://github.com/Grokzen/redis-py-cluster

范例: 

```bash
[root@redis-node1 ~]#yum -y install python3
[root@redis-node1 ~]#pip3 install redis-py-cluster
[root@redis-node1 ~]#vim redis_cluster_test.py
[root@redis-node1 ~]#cat ./redis_cluster_test.py 
#!/usr/bin/env python3
from rediscluster  import RedisCluster
startup_nodes = [
   {"host":"10.0.0.8", "port":6379},
   {"host":"10.0.0.18", "port":6379},
   {"host":"10.0.0.28", "port":6379},
   {"host":"10.0.0.38", "port":6379},
   {"host":"10.0.0.48", "port":6379},
   {"host":"10.0.0.58", "port":6379}
]
redis_conn= RedisCluster(startup_nodes=startup_nodes,password='123456', decode_responses=True)
for i in range(0, 10000):
    redis_conn.set('key'+str(i),'value'+str(i))
    print('key'+str(i)+':',redis_conn.get('key'+str(i)))

[root@redis-node1 ~]#chmod +x redis_cluster_test.py
[root@redis-node1 ~]#./redis_cluster_test.py
......
key9998: value9998
key9999: value9999

#验证数据
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.8
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379> DBSIZE
(integer) 3331
10.0.0.8:6379> GET key1
(error) MOVED 9189 10.0.0.18:6379
10.0.0.8:6379> GET key2
"value2"
10.0.0.8:6379> GET key3
"value3"
10.0.0.8:6379> KEYS *
......
3329) "key7832"
3330) "key2325"
3331) "key2880"
10.0.0.8:6379>
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 DBSIZE
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
(integer) 3340
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 GET key1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
"value1"
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.28 DBSIZE
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
(integer) 3329
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 GET key5
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
"value5"
[root@redis-node1 ~]#
```

##### 3.3.4.9 模拟master故障，对应的slave节点自动提升为新master

```bash
#模拟node2节点出故障,需要相应的数秒故障转移时间
[root@redis-node2 ~]#tail -f /var/log/redis/redis.log  
[root@redis-node2 ~]#redis-cli -a 123456
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> shutdown
not connected> exit
[root@redis-node2 ~]#ss -ntl
State       Recv-Q       Send-Q Local Address:Port Peer Address:Port        
LISTEN       0             128           0.0.0.0:22         0.0.0.0:*           
LISTEN       0             100         127.0.0.1:25         0.0.0.0:*           
LISTEN       0             128             [::]:22           [::]:*           
LISTEN       0             100             [::1]:25           [::]:*  

[root@redis-node2 ~]# redis-cli -a 123456 --cluster info 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
Could not connect to Redis at 10.0.0.18:6379: Connection refused
10.0.0.8:6379 (cb028b83...) -> 3331 keys | 5461 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 3340 keys | 5462 slots | 0 slaves. #10.0.0.48为新的master
10.0.0.28:6379 (d34da866...) -> 3329 keys | 5461 slots | 1 slaves.
[OK] 10000 keys in 3 masters.
0.61 keys per slot on average.

[root@redis-node2 ~]# redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
Could not connect to Redis at 10.0.0.18:6379: Connection refused
10.0.0.8:6379 (cb028b83...) -> 3331 keys | 5461 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 3340 keys | 5462 slots | 0 slaves.
10.0.0.28:6379 (d34da866...) -> 3329 keys | 5461 slots | 1 slaves.
[OK] 10000 keys in 3 masters.
0.61 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[5461-10922] (5462 slots) master
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@redis-node2 ~]#redis-cli -a 123456 -h 10.0.0.48
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.48:6379> INFO replication
# Replication
role:master
connected_slaves:0
master_replid:0000698bc2c6452d8bfba68246350662ae41d8fd
master_replid2:b9066d3cbf0c5fecc7f4d1d5cb2433999783fa3f
master_repl_offset:2912424
second_repl_offset:2912425
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1863849
repl_backlog_histlen:1048576
10.0.0.48:6379>

#恢复故障节点node2自动成为slave节点
[root@redis-node2 ~]#systemctl start redis

#查看自动生成的配置文件，可以查看node2自动成为slave节点
[root@redis-node2 ~]#cat /var/lib/redis/nodes-6379.conf
99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379@16379 myself,slave 
d04e524daec4d8e22bdada7f21a9487c2d3e1057 0 1582352081847 2 connected
f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379@16379 slave 
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 1582352081868 1582352081847 4 connected
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379@16379 master - 1582352081868 1582352081847 1 connected 0-5460
9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379@16379 slave 
d34da8666a6f587283a1c2fca5d13691407f9462 1582352081869 1582352081847 3 connected
d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379@16379 master - 1582352081869 1582352081847 7 connected 5461-10922
d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379@16379 master - 1582352081869 1582352081847 3 connected 10923-16383
vars currentEpoch 7 lastVoteEpoch 0

[root@redis-node2 ~]#redis-cli -a 123456 -h 10.0.0.48
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.48:6379> INFO replication
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.18,port=6379,state=online,offset=2912564,lag=1
master_replid:0000698bc2c6452d8bfba68246350662ae41d8fd
master_replid2:b9066d3cbf0c5fecc7f4d1d5cb2433999783fa3f
master_repl_offset:2912564
second_repl_offset:2912425
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1863989
repl_backlog_histlen:1048576
10.0.0.48:6379> 
```

#### 3.3.5 实战案例：基于Redis 4 的 redis cluster 部署

##### 3.3.5.1 准备redis Cluster 基本配置

1. 每个redis 节点采用相同的硬件配置、相同的密码、相同的redis版本 
2. 所有redis服务器必须没有任何数据 
3. 准备三台CentOS 7 主机，已编译安装好redis，各启动两个redis实例，分别使用6379和6380端 口，从而模拟实现6台redis实例

```bash
10.0.0.7:6379|6380
10.0.0.17:6379|6380
10.0.0.27:6379|6380
```

范例: 6个物理节点环境的基于脚本安装后批量修改配置

```bash
[root@centos7 ~]#sed -i -e '/^# masterauth/a masterauth 123456' -e '/# cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file nodes-6379.conf/a cluster-config-file nodes-6379.conf'   -e '/cluster-require-full-coverage yes/c cluster-require-full-coverage no' /apps/redis/etc/redis.conf

[root@centos7 ~]#grep '^[^#]' /apps/redis/etc/redis.conf
bind 0.0.0.0
protected-mode yes
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize no
supervised no
pidfile /apps/redis/run/redis_6379.pid
loglevel notice
logfile /apps/redis/log/redis-6379.log
databases 16
always-show-logo yes
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
dir /apps/redis/data/
masterauth 123456
slave-serve-stale-data yes
slave-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-disable-tcp-nodelay no
slave-priority 100
requirepass 123456
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
slave-lazy-flush no
appendonly no
appendfilename "appendonly.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble no
lua-time-limit 5000
cluster-enabled yes
cluster-config-file nodes-6379.conf
cluster-require-full-coverage no
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
aof-rewrite-incremental-fsync yes
```

准备6个实例：在三个主机上重复下面的操作

范例: 3个节点

```bash
#在三台主机上都编译安装redis
[root@redis-node1 ~]#yum -y install gcc jemalloc-devel 
[root@redis-node1 ~]#cd /usr/local/src
[root@redis-node1 src]#wget http://download.redis.io/releases/redis-4.0.14.tar.gz
[root@redis-node1 src]#tar xf redis-4.0.14.tar.gz
[root@redis-node1 src]#cd redis-4.0.14
[root@redis-node1 redis-4.0.14]#make PREFIX=/apps/redis install

#准备相关文件和目录
[root@redis-node1 redis-4.0.14]#ln -s /apps/redis/bin/redis-* /usr/bin/
[root@redis-node1 redis-4.0.14]#mkdir -p /apps/redis/{etc,log,data,run}
[root@redis-node1 redis-4.0.14]#cp redis.conf /apps/redis/etc/

#准备用户
[root@redis-node1 ~]#useradd -r -s /sbin/nologin redis

#配置权限和相关优化配置
[root@redis-node1 ~]#chown -R redis.redis /apps/redis
[root@redis-node1 ~]#cat >> /etc/sysctl.conf <<EOF
net.core.somaxconn = 1024
vm.overcommit_memory = 1
EOF

[root@redis-node1 ~]#sysctl -p 
[root@redis-node1 ~]#echo 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' >> /etc/rc.d/rc.local
[root@redis-node1 ~]#chmod +x /etc/rc.d/rc.local
[root@redis-node1 ~]#/etc/rc.d/rc.local

#准备service文件
[root@redis-node1 ~]#cat > /usr/lib/systemd/system/redis.service <<EOF
[Unit]
Description=Redis persistent key-value database
After=network.target
[Service]
ExecStart=/apps/redis/bin/redis-server /apps/redis/etc/redis.conf --supervised
systemd
ExecStop=/bin/kill -s QUIT \$MAINPID
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
[Install]
WantedBy=multi-user.target
EOF

[root@redis-node1 ~]#systemctl daemon-reload
[root@redis-node1 ~]#systemctl enable --now redis

#准备6379的实例配置文件
[root@redis-node1 ~]#systemctl stop redis
[root@redis-node1 ~]#cd /apps/redis/etc/
[root@redis-node1 etc]#sed -i -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e '/^# masterauth/a masterauth 123456' -e '/# requirepass/a requirepass 123456' -e '/# cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file nodes-6379.conf/a cluster-config-file nodes-6379.conf' -e 's/^dir .*/dir /apps/redis\/data/' -e '/appendonly no/c appendonly yes' -e '/logfile ""/c logfile "/apps/redis/log/redis-6379.log"' -e '/^pidfile .*/c pidfile 
/apps/redis/run/redis_6379.pid' /apps/redis/etc/redis.conf

#准备6380端口的实例的配置文件
[root@redis-node1 etc]#cp -p redis.conf redis6380.conf 
[root@redis-node1 etc]#sed -i -e 's/6379/6380/' -e 's/dbfilename dump.rdb/dbfilename dump6380.rdb/' -e 's/appendfilename "appendonly\.aof"/appendfilename "appendonly6380.aof"/' /apps/redis/etc/redis6380.conf

#准备服务文件
[root@redis-node1 ~]#cp /lib/systemd/system/redis.service /lib/systemd/system/redis6380.service
[root@redis-node1 ~]#sed -i 's/redis.conf/redis6380.conf/' /lib/systemd/system/redis6380.service

#启动服务，查看到端口都打开
[root@redis-node1 ~]#systemctl daemon-reload 
[root@redis-node1 ~]#systemctl enable --now redis redis6380
[root@redis-node1 ~]#ss -ntl
State       Recv-Q Send-Q   Local Address:Port     Peer Address:Port             
LISTEN      0      100          127.0.0.1:25                 *:*               
LISTEN      0      128                 *:16379               *:*               
LISTEN      0      128                 *:16380               *:*               
LISTEN      0      128                 *:6379               *:*               
LISTEN      0      128                 *:6380               *:*               
LISTEN      0      128                 *:22                 *:*               
LISTEN      0      100             [::1]:25               [::]:*               
LISTEN      0      128               [::]:22               [::]:* 

[root@redis-node1 ~]#ps -ef|grep redis
redis 71539   1  0 22:13 ?   00:00:00 /apps/redis/bin/redis-server 0.0.0.0:6379 [cluster]
redis 71543   1  0 22:13 ?   00:00:00 /apps/redis/bin/redis-server 0.0.0.0:6380 [cluster]
root  71553  31781  0 22:15 pts/0 00:00:00 grep --color=auto redis

[root@redis-node1 ~]#tree /apps/redis/
/apps/redis
├── bin
│   ├── redis-benchmark
│   ├── redis-check-aof
│   ├── redis-check-rdb
│   ├── redis-cli
│   ├── redis-sentinel -> redis-server
│   └── redis-server
├── data
│   ├── appendonly6380.aof
│   ├── appendonly.aof
│   ├── nodes-6379.conf
│   └── nodes-6380.conf
├── etc
│   ├── redis6380.conf
│   └── redis.conf
├── log
│   ├── redis-6379.log
│   └── redis-6380.log
└── run
   ├── redis_6379.pid
   └── redis_6380.pid
5 directories, 16 files
```

##### 3.3.5.2 准备redis-trib.rb工具

Redis 3和 4版本需要使用到集群管理工具redis-trib.rb，这个工具是redis官方推出的管理redis集群的工 具，集成在redis的源码src目录下，是基于redis提供的集群命令封装成简单、便捷、实用的操作工具， redis-trib.rb是redis作者用ruby开发完成的，需要安装ruby的redis模块,但是centos 7 系统yum安装的 ruby存在版本较低问题，如下：

```bash
[root@redis-node1 ~]#find / -name redis-trib.rb
/usr/local/src/redis-4.0.14/src/redis-trib.rb
[root@redis-node1 ~]#cp /usr/local/src/redis-4.0.14/src/redis-trib.rb /usr/bin/
[root@redis-node1 ~]#redis-trib.rb #缺少ruby环境无法运行rb脚本
/usr/bin/env: ruby: No such file or directory

#CentOS 7带的ruby版本过低,无法运行上面ruby脚本,需要安装2.3以上版本,安装rubygems依赖ruby自动安装
[root@redis-node1 ~]#yum install rubygems -y  
[root@redis-node1 ~]#gem install redis #gem相当于python里pip和linux的yum
Fetching: redis-4.1.3.gem (100%)
ERROR: Error installing redis:
 redis requires Ruby version >= 2.3.0.
```

解决ruby版本较低问题：

* 编译安装高版本的ruby ruby 

  官网: www.ruby-lang.org/zh_cn

```bash
[root@redis-node1 ~]#yum -y install gcc openssl-devel zlib-devel
[root@redis-node1 ~]#wget https://cache.ruby-lang.org/pub/ruby/2.5/ruby-2.5.5.tar.gz
[root@redis-node1 ~]#tar xf ruby-2.5.5.tar.gz
[root@redis-node1 ~]#cd ruby-2.5.5
[root@redis-node1 ruby-2.5.5]#./configure
[root@redis-node1 ruby-2.5.5]#make -j 2 && make install
[root@redis-node1 ruby-2.5.5]#which ruby
/usr/local/bin/ruby
[root@redis-node1 ruby-2.5.5]# ruby -v
ruby 2.5.5p157 (2019-03-15 revision 67260) [x86_64-linux]
[root@redis-node1 ruby-2.5.5]#exit   #注意需要重新登录
```

* 安装ruby中redis模块

只编译安装高版本的ruby后,运行redis-trib.rb 仍会出错

```bash
[root@redis-node1 ~]#redis-trib.rb -h
Traceback (most recent call last):
 2: from /usr/bin/redis-trib.rb:25:in `<main>'
 1: from /usr/local/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in 
`require'
/usr/local/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require': cannot load such file -- redis (LoadError)
```

解决上述错误：

```bash
[root@redis-node1 ~]#gem install redis -v 4.1.3 #注意需要重新登录再执行,否则无法识别到新ruby版本
Fetching: redis-4.1.3.gem (100%)
Successfully installed redis-4.1.3
Parsing documentation for redis-4.1.3
Installing ri documentation for redis-4.1.3
Done installing documentation for redis after 1 seconds
1 gem installed
#gem uninstall redis 可以卸载已安装好redis模块
```

如果无法在线安装，可以下载redis模块安装包离线安装

```bash
#https://rubygems.org/gems/redis #先下载redis模块安装包
[root@redis-node1 ~]#gem install -l redis-4.1.3.gem #离线安装redis模块
```

##### 3.3.5.3 redis-trib.rb 命令用法

```bash
[root@redis-node1 ~]#redis-trib.rb
Usage: redis-trib <command> <options> <arguments ...>
create host1:port1 ... hostN:portN #创建集群
 --replicas <arg> #指定每个master的副本数量,即对应slave数量,一般为1 
check host:port #检查集群信息
info host:port #查看集群主机信息
fix host:port #修复集群
 --timeout <arg>
reshard host:port #在线热迁移集群指定主机的slots数据
 --from <arg>
 --to <arg>
 --slots <arg>
            --yes
 --timeout <arg>
 --pipeline <arg>
rebalance host:port #平衡集群中各主机的slot数量
 --weight <arg>
 --auto-weights
 --use-empty-masters
 --timeout <arg>
 --simulate
 --pipeline <arg>
 --threshold <arg>
add-node new_host:new_port existing_host:existing_port #添加主机到集群
 --slave
 --master-id <arg>
del-node host:port node_id #删除主机
set-timeout host:port milliseconds #设置节点的超时时间
call host:port command arg arg .. arg #在集群上的所有节点上执行命令
import host:port #导入外部redis服务器的数据到当前集群
 --from <arg>
 --copy
 --replace
help (show this help)
```

##### 3.3.5.4 修改密码 redis 登录密码

```bash
#修改redis-trib.rb连接redis的密码
[root@redis ~]#vim /usr/local/lib/ruby/gems/2.5.0/gems/redis-4.1.3/lib/redis/client.rb 
```

##### 3.3.5.5 创建redis cluster集群

```bash
#确保三台主机6个实例都启动状态
[root@redis-node1 ~]#systemctl is-active redis redis6380
active
active
[root@redis-node2 ~]#systemctl is-active redis redis6380
active
active
[root@redis-node3 ~]#systemctl is-active redis redis6380
active
active

#在第一个主机上执行下面操作
#--replicas 1 表示每个 master 分配一个 slave 节点,前三个节点自动划分为master,后面都为slave节点
[root@redis-node1 ~]#redis-trib.rb create --replicas 1 10.0.0.7:6379 
10.0.0.17:6379 10.0.0.27:6379 10.0.0.7:6380 10.0.0.17:6380 10.0.0.27:6380 
>>> Creating cluster
>>> Performing hash slots allocation on 6 nodes...
Using 3 masters:
10.0.0.7:6379
10.0.0.17:6379
10.0.0.27:6379
Adding replica 10.0.0.17:6380 to 10.0.0.7:6379
Adding replica 10.0.0.27:6380 to 10.0.0.17:6379
Adding replica 10.0.0.7:6380 to 10.0.0.27:6379
M: 739cb4c9895592131de418b8bc65990f81b75f3a 10.0.0.7:6379
   slots:0-5460 (5461 slots) master
S: 0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380
   replicates a01fd3d81922d6752f7c960f1a75b6e8f28d911b
M: dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379
   slots:5461-10922 (5462 slots) master
S: 34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380
   replicates 739cb4c9895592131de418b8bc65990f81b75f3a
M: a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379
   slots:10923-16383 (5461 slots) master
S: aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380
   replicates dddabb4e19235ec02ae96ab2ce67e295ce0274d7
Can I set the above configuration? (type 'yes' to accept): yes  #输入yes 
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join....
>>> Performing Cluster Check (using node 10.0.0.7:6379)
M: 739cb4c9895592131de418b8bc65990f81b75f3a 10.0.0.7:6379
   slots:0-5460 (5461 slots) master
   1 additional replica(s)
S: 0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380
   slots: (0 slots) slave
   replicates a01fd3d81922d6752f7c960f1a75b6e8f28d911b
S: 34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380
   slots: (0 slots) slave
   replicates 739cb4c9895592131de418b8bc65990f81b75f3a
S: aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380
   slots: (0 slots) slave
   replicates dddabb4e19235ec02ae96ab2ce67e295ce0274d7
M: a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
M: dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

如果有之前的操作导致Redis集群创建报错，则执行清空数据和集群命令：

```bash
127.0.0.1:6379> FLUSHALL
OK
127.0.0.1:6379> cluster reset
OK
```

##### 3.3.5.6 查看 redis cluster 集群状态

自动生成配置文件记录master/slave对应关系

```bash
[root@redis-node1 ~]#cat /apps/redis/data/nodes-6379.conf 
0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380@16380 slave 
a01fd3d81922d6752f7c960f1a75b6e8f28d911b 0 1582383256000 5 connected
34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380@16380 slave 
739cb4c9895592131de418b8bc65990f81b75f3a 0 1582383256216 4 connected
aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380@16380 slave 
dddabb4e19235ec02ae96ab2ce67e295ce0274d7 0 1582383257000 6 connected
739cb4c9895592131de418b8bc65990f81b75f3a 10.0.0.7:6379@16379 myself,master - 0 1582383256000 1 connected 0-5460
a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379@16379 master - 0 1582383258230 5 connected 10923-16383
dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379@16379 master - 0 1582383257223 3 connected 5461-10922
vars currentEpoch 6 lastVoteEpoch 0
[root@redis-node1 ~]#
```

查看状态

```bash
[root@redis-node1 ~]#redis-trib.rb info 10.0.0.7:6379
10.0.0.7:6379 (739cb4c9...) -> 0 keys | 5461 slots | 1 slaves.
10.0.0.27:6379 (a01fd3d8...) -> 0 keys | 5461 slots | 1 slaves.
10.0.0.17:6379 (dddabb4e...) -> 0 keys | 5462 slots | 1 slaves.
[OK] 0 keys in 3 masters.
0.00 keys per slot on average.

[root@redis-node1 ~]#redis-trib.rb check 10.0.0.7:6379
>>> Performing Cluster Check (using node 10.0.0.7:6379)
M: 739cb4c9895592131de418b8bc65990f81b75f3a 10.0.0.7:6379
   slots:0-5460 (5461 slots) master
   1 additional replica(s)
S: 0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380
   slots: (0 slots) slave
   replicates a01fd3d81922d6752f7c960f1a75b6e8f28d911b
S: 34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380
   slots: (0 slots) slave
   replicates 739cb4c9895592131de418b8bc65990f81b75f3a
S: aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380
   slots: (0 slots) slave
   replicates dddabb4e19235ec02ae96ab2ce67e295ce0274d7
M: a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
M: dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@redis-node1 ~]#redis-cli -a 123456
Warning: Using a password with '-a' option on the command line interface may not be safe.
127.0.0.1:6379> CLUSTER INFO
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:252
cluster_stats_messages_pong_sent:277
cluster_stats_messages_sent:529
cluster_stats_messages_ping_received:272
cluster_stats_messages_pong_received:252
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:529
127.0.0.1:6379>

[root@redis-node1 ~]#redis-cli -a 123456 -p 6379 CLUSTER NODES
Warning: Using a password with '-a' option on the command line interface may not be safe.
29a83275db60f1c8f9f6d39b66cbc6c3d5cf20f1 10.0.0.7:6379@16379 myself,master - 0 1601985995000 1 connected 0-5460
3e607de412a8a240e8214c2d7a663cf1523412eb 10.0.0.17:6380@16380 slave 
29a83275db60f1c8f9f6d39b66cbc6c3d5cf20f1 0 1601985997092 4 connected
17d0b29d2f50ea9c89d4e6e0cf3ee3ee4f7c4179 10.0.0.7:6380@16380 slave 
90b206131d89b0812c626677343df9a11ff1d211 0 1601985995075 5 connected
90b206131d89b0812c626677343df9a11ff1d211 10.0.0.27:6379@16379 master - 0 1601985996084 5 connected 10923-16383
fb34c3a704aefb1e1ef2317b20598d6e1e51c010 10.0.0.17:6379@16379 master - 01601985995000 3 connected 5461-10922
c9ea6113a1992695fb86f5368fe6320349b0f8a6 10.0.0.27:6380@16380 slave 
fb34c3a704aefb1e1ef2317b20598d6e1e51c010 0 1601985996000 6 connected

[root@redis-node1 ~]#redis-cli -a 123456 -p 6379 INFO replication
Warning: Using a password with '-a' option on the command line interface may not be safe.
# Replication
role:master
connected_slaves:1
slave0:ip=10.0.0.17,port=6380,state=online,offset=196,lag=0
master_replid:4ee36f9374c796ca4c65a0f0cb2c39304bb2e9c9
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:196
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:196

[root@redis-node1 ~]#redis-cli -a 123456 -p 6380 INFO replication
Warning: Using a password with '-a' option on the command line interface may not be safe.
# Replication
role:slave
master_host:10.0.0.27
master_port:6379
master_link_status:up
master_last_io_seconds_ago:2
master_sync_in_progress:0
slave_repl_offset:224
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:dba41cb31c14de7569e597a3d8debc1f0f114c1e
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:224
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:224
```

##### 3.3.3.7 python脚本实现RedisCluster集群写入

```bash
[root@redis-node1 ~]#yum -y install python3
[root@redis-node1 ~]#pip3 install redis-py-cluster
[root@redis-node1 ~]#vim redis_cluster_test.py
[root@redis-node1 ~]#cat ./redis_cluster_test.py
#!/usr/bin/env python3
from rediscluster import RedisCluster
startup_nodes = [
   {"host":"10.0.0.7", "port":6379},
   {"host":"10.0.0.7", "port":6380},
   {"host":"10.0.0.17", "port":6379},
   {"host":"10.0.0.17", "port":6380},
   {"host":"10.0.0.27", "port":6379},
   {"host":"10.0.0.27", "port":6380}
]
redis_conn= RedisCluster(startup_nodes=startup_nodes,password='123456', 
decode_responses=True)
for i in range(0, 10000):
   redis_conn.set('key'+str(i),'value'+str(i))
   print('key'+str(i)+':',redis_conn.get('key'+str(i)))

[root@redis-node1 ~]#chmod +x redis_cluster_test.py
[root@redis-node1 ~]#./redis_cluster_test.py
......
key9998: value9998
key9999: value9999
```

验证脚本写入的状态

```
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.7 DBSIZE
Warning: Using a password with '-a' option on the command line interface may not be safe.
(integer) 3331
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.17 DBSIZE
Warning: Using a password with '-a' option on the command line interface may not be safe.
(integer) 3340
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.27 DBSIZE
Warning: Using a password with '-a' option on the command line interface may not be safe.
(integer) 3329
[root@redis-node1 ~]#redis-cli -a 123456 GET key1
Warning: Using a password with '-a' option on the command line interface may not be safe.
(error) MOVED 9189 10.0.0.17:6379
[root@redis-node1 ~]#redis-cli -a 123456 GET key2
Warning: Using a password with '-a' option on the command line interface may not be safe.
"value2"
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.17 GET key1
Warning: Using a password with '-a' option on the command line interface may not be safe.
"value1"

[root@redis-node1 ~]#redis-trib.rb info 10.0.0.7:6379
10.0.0.7:6379 (739cb4c9...) -> 3331 keys | 5461 slots | 1 slaves.
10.0.0.27:6379 (a01fd3d8...) -> 3329 keys | 5461 slots | 1 slaves.
10.0.0.17:6379 (dddabb4e...) -> 3340 keys | 5462 slots | 1 slaves.
[OK] 10000 keys in 3 masters.
0.61 keys per slot on average.
[root@redis-node1 ~]#
```

##### 3.3.3.8 模拟 master 故障，对应的slave节点自动提升为新master

```bash
[root@redis-node1 ~]#systemctl stop redis

#不会立即提升,需要稍等一会儿再观察下面结果
[root@redis-node1 ~]#redis-trib.rb check 10.0.0.27:6379
>>> Performing Cluster Check (using node 10.0.0.27:6379)
M: a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
S: aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380
   slots: (0 slots) slave
   replicates dddabb4e19235ec02ae96ab2ce67e295ce0274d7
S: 0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380
   slots: (0 slots) slave
   replicates a01fd3d81922d6752f7c960f1a75b6e8f28d911b
M: 34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380
   slots:0-5460 (5461 slots) master
   0 additional replica(s)
M: dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@redis-node1 ~]#tail /var/log/messages 
Feb 22 23:23:13 centos7 redis-server: 71887:M 22 Feb 23:23:13.656 * Saving the final RDB snapshot before exiting.
Feb 22 23:23:13 centos7 systemd: Stopped Redis persistent key-value database.
Feb 22 23:23:13 centos7 redis-server: 71887:M 22 Feb 23:23:13.660 * DB saved on disk
Feb 22 23:23:13 centos7 redis-server: 71887:M 22 Feb 23:23:13.660 * Removing the pid file.
Feb 22 23:23:13 centos7 redis-server: 71887:M 22 Feb 23:23:13.660 # Redis is now ready to exit, bye bye...
Feb 22 23:23:13 centos7 systemd: Unit redis.service entered failed state.
Feb 22 23:23:13 centos7 systemd: redis.service failed.
Feb 22 23:23:30 centos7 redis-server: 72046:S 22 Feb 23:23:30.077 * FAIL message received from dddabb4e19235ec02ae96ab2ce67e295ce0274d7 about 739cb4c9895592131de418b8bc65990f81b75f3a
Feb 22 23:23:30 centos7 redis-server: 72046:S 22 Feb 23:23:30.077 # Cluster state changed: fail
Feb 22 23:23:30 centos7 redis-server: 72046:S 22 Feb 23:23:30.701 # Cluster state changed: ok

[root@redis-node1 ~]#redis-trib.rb info 10.0.0.27:6379
10.0.0.27:6379 (a01fd3d8...) -> 3329 keys | 5461 slots | 1 slaves.
10.0.0.17:6380 (34708909...) -> 3331 keys | 5461 slots | 0 slaves.
10.0.0.17:6379 (dddabb4e...) -> 3340 keys | 5462 slots | 1 slaves.
[OK] 10000 keys in 3 masters.
0.61 keys per slot on average.
```

将故障的master恢复后，该节点自动加入集群成为新的slave

```bash
[root@redis-node1 ~]#systemctl start redis
[root@redis-node1 ~]#redis-trib.rb check 10.0.0.27:6379
>>> Performing Cluster Check (using node 10.0.0.27:6379)
M: a01fd3d81922d6752f7c960f1a75b6e8f28d911b 10.0.0.27:6379
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
S: aefc6203958859024b8383b2fdb87b9e09411ccd 10.0.0.27:6380
   slots: (0 slots) slave
   replicates dddabb4e19235ec02ae96ab2ce67e295ce0274d7
S: 739cb4c9895592131de418b8bc65990f81b75f3a 10.0.0.7:6379
   slots: (0 slots) slave
   replicates 34708909088ba562decbc1525a9606e088bdddf1
S: 0e0beba04cc98da02ebdb5225a11b84aa8062e10 10.0.0.7:6380
   slots: (0 slots) slave
   replicates a01fd3d81922d6752f7c960f1a75b6e8f28d911b
M: 34708909088ba562decbc1525a9606e088bdddf1 10.0.0.17:6380
   slots:0-5460 (5461 slots) master
   1 additional replica(s)
M: dddabb4e19235ec02ae96ab2ce67e295ce0274d7 10.0.0.17:6379
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

#### 3.3.6 Redis cluster集群节点维护

redis 集群运行之后，难免由于硬件故障、网络规划、业务增长等原因对已有集群进行相应的调整， 比如: 增加Redis node节点、减少节点、节点迁移、更换服务器等。增加节点和删除节点会涉及到已有的 槽位重新分配及数据迁移。

##### 3.3.6.1 集群维护之动态扩容

实战案例：

因公司业务发展迅猛，现有的三主三从的redis cluster架构可能无法满足现有业务的并发写入需求，因此公司紧急采购两台服务器10.0.0.68，10.0.0.78，需要将其动态添加到集群当中，但不能影响业务使用 和数据丢失。

注意: 生产环境一般建议master节点为奇数个,比如:3,5,7,以防止脑裂现象

###### 3.3.6.1.1 添加节点准备

增加Redis node节点，需要与之前的Redis node版本相同、配置一致，然后分别再启动两台Redis  node，应为一主一从。

```bash
#配置node7节点
[root@redis-node7 ~]#dnf -y install redis
[root@redis-node7 ~]#sed -i.bak -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e 
'/masterauth/a masterauth 123456' -e '/# requirepass/a requirepass 123456' -e '/# 
cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file nodes-6379.conf/a cluster-config-file nodes-6379.conf' -e '/cluster-require-full-coverage yes/c cluster-require-full-coverage no' /etc/redis.conf
[root@redis-node7 ~]#systemctl enable --now redis

#配置node8节点
[root@redis-node8 ~]#dnf -y install redis
[root@redis-node8 ~]#sed -i.bak -e 's/bind 127.0.0.1/bind 0.0.0.0/' -e 
'/masterauth/a masterauth 123456' -e '/# requirepass/a requirepass 123456' -e '/# 
cluster-enabled yes/a cluster-enabled yes' -e '/# cluster-config-file nodes-6379.conf/a cluster-config-file nodes-6379.conf' -e '/cluster-require-full-coverage yes/c cluster-require-full-coverage no' /etc/redis.conf
[root@redis-node8 ~]#systemctl enable --now redis
```

###### 3.3.6.1.2 添加新的master节点到集群

使用以下命令添加新节点，要添加的新redis节点IP和端口添加到的已有的集群中任意节点的IP:端口

```bash
add-node new_host:new_port existing_host:existing_port [--slave --master-id
<arg>]
#说明：
new_host:new_port #为新添加的主机的IP和端口
existing_host:existing_port #为已有的集群中任意节点的IP和端口
```

Redis 3/4 添加方式：

```bash
#把新的Redis 节点10.0.0.37添加到当前Redis集群当中。
[root@redis-node1 ~]#redis-trib.rb add-node 10.0.0.37:6379 10.0.0.7:6379
[root@redis-node1 ~]#redis-trib.rb info 10.0.0.7:6379
10.0.0.7:6379 (29a83275...) -> 3331 keys | 5461 slots | 1 slaves.
10.0.0.37:6379 (12ca273a...) -> 0 keys | 0 slots | 0 slaves.
10.0.0.27:6379 (90b20613...) -> 3329 keys | 5461 slots | 1 slaves.
10.0.0.17:6379 (fb34c3a7...) -> 3340 keys | 5462 slots | 1 slaves.
[OK] 10000 keys in 4 masters.
0.61 keys per slot on average.
```

Redis 5 添加方式：

```bash
#将一台新的主机10.0.0.68加入集群,以下示例中10.0.0.58可以是任意存在的集群节点
[root@redis-node1 ~]#redis-cli -a 123456 --cluster add-node 10.0.0.68:6379 <当前任意集群节点>:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Adding node 10.0.0.68:6379 to cluster 10.0.0.58:6379
>>> Performing Cluster Check (using node 10.0.0.58:6379)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
>>> Send CLUSTER MEET to node 10.0.0.68:6379 to make it join the cluster.
[OK] New node added correctly.

#观察到该节点已经加入成功，但此节点上没有slot位,也无从节点，而且新的节点是master
[root@redis-node1 ~]#redis-cli -a 123456 --cluster info 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379 (cb028b83...) -> 6672 keys | 5461 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 0 keys | 0 slots | 0 slaves.
10.0.0.48:6379 (d04e524d...) -> 6679 keys | 5462 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6649 keys | 5461 slots | 1 slaves.
[OK] 20000 keys in 5 masters.
1.22 keys per slot on average.

[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379 (cb028b83...) -> 6672 keys | 5461 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 0 keys | 0 slots | 0 slaves.
10.0.0.48:6379 (d04e524d...) -> 6679 keys | 5462 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6649 keys | 5461 slots | 1 slaves.
[OK] 20000 keys in 5 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots: (0 slots) master
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@redis-node1 ~]#cat /var/lib/redis/nodes-6379.conf 
d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379@16379 master - 0 1582356107260 8 connected
9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379@16379 slave 
d34da8666a6f587283a1c2fca5d13691407f9462 0 1582356110286 6 connected
f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379@16379 slave 
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 0 1582356108268 4 connected
d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379@16379 master - 0 1582356105000 7 connected 5461-10922
99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379@16379 slave 
d04e524daec4d8e22bdada7f21a9487c2d3e1057 0 1582356108000 7 connected
d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379@16379 master - 0 1582356107000 3 connected 10923-16383
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379@16379 myself,master - 0 1582356106000 1 connected 0-5460
vars currentEpoch 8 lastVoteEpoch 7

#和上面显示结果一样
[root@redis-node1 ~]#redis-cli -a 123456 CLUSTER NODES
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379@16379 master - 0
1582356313200 8 connected
9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379@16379 slave 
d34da8666a6f587283a1c2fca5d13691407f9462 0 1582356311000 6 connected
f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379@16379 slave 
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 0 1582356314208 4 connected
d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379@16379 master - 0
1582356311182 7 connected 5461-10922
99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379@16379 slave 
d04e524daec4d8e22bdada7f21a9487c2d3e1057 0 1582356312000 7 connected
d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379@16379 master - 0
1582356312190 3 connected 10923-16383
cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379@16379 myself,master - 0
1582356310000 1 connected 0-5460

#查看集群状态
[root@redis-node1 ~]#redis-cli -a 123456 CLUSTER INFO
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:7
cluster_size:3
cluster_current_epoch:8
cluster_my_epoch:1
cluster_stats_messages_ping_sent:17442
cluster_stats_messages_pong_sent:13318
cluster_stats_messages_fail_sent:4
cluster_stats_messages_auth-ack_sent:1
cluster_stats_messages_sent:30765
cluster_stats_messages_ping_received:13311
cluster_stats_messages_pong_received:13367
cluster_stats_messages_meet_received:7
cluster_stats_messages_fail_received:1
cluster_stats_messages_auth-req_received:1
cluster_stats_messages_received:26687
[root@redis-node1 ~]#
```

###### 3.3.6.1.3 在新的master上重新分配槽位

新的node节点加到集群之后,默认是master节点，但是没有slots，需要重新分配 添加主机之后需要对添加至集群种的新主机重新分片,否则其没有分片也就无法写入数据。

**注意: 重新分配槽位需要清空数据,所以需要先备份数据,扩展后再恢复数据**

Redis 3/4:

```bash
[root@redis-node1 ~]# redis-trib.rb check 10.0.0.67:6379 #当前状态
[root@redis-node1 ~]# redis-trib.rb reshard <任意节点>:6379 #重新分片
[root@redis-node1 ~]# redis-trib.rb fix 10.0.0.67:6379 #如果迁移失败使用此命令修复集群
```

Redis 5：

```bash
[root@redis-node1 ~]#redis-cli -a 123456 --cluster reshard <当前任意集群节点>:6379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
>>> Performing Cluster Check (using node 10.0.0.68:6379)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots: (0 slots) master
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: f67f1c02c742cd48d3f48d8c362f9f1b9aa31549 10.0.0.78:6379
   slots: (0 slots) master
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
How many slots do you want to move (from 1 to 16384)?4096 #新分配多少个槽位
=16384/master个数
What is the receiving node ID? d6e2eca6b338b717923f64866bd31d42e52edc98 #新的
master的ID
Please enter all the source node IDs.
 Type 'all' to use all the nodes as source nodes for the hash slots.
 Type 'done' once you entered all the source nodes IDs.
Source node #1: all #输入all,将哪些源主机的槽位分配给新的节点，all是自动在所有的redis 
node选择划分，如果是从redis cluster删除某个主机可以使用此方式将指定主机上的槽位全部移动到别的
redis主机
......
Do you want to proceed with the proposed reshard plan (yes/no)?  yes #确认分配
......
Moving slot 12280 from 10.0.0.28:6379 to 10.0.0.68:6379: .
Moving slot 12281 from 10.0.0.28:6379 to 10.0.0.68:6379: .
Moving slot 12282 from 10.0.0.28:6379 to 10.0.0.68:6379: 
Moving slot 12283 from 10.0.0.28:6379 to 10.0.0.68:6379: ..
Moving slot 12284 from 10.0.0.28:6379 to 10.0.0.68:6379: 
Moving slot 12285 from 10.0.0.28:6379 to 10.0.0.68:6379: .
Moving slot 12286 from 10.0.0.28:6379 to 10.0.0.68:6379: 
Moving slot 12287 from 10.0.0.28:6379 to 10.0.0.68:6379: ..
[root@redis-node1 ~]#
#确定slot分配成功
[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
10.0.0.8:6379 (cb028b83...) -> 5019 keys | 4096 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 4948 keys | 4096 slots | 0 slaves.
10.0.0.48:6379 (d04e524d...) -> 5033 keys | 4096 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 5000 keys | 4096 slots | 1 slaves.
[OK] 20000 keys in 5 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[1365-5460] (4096 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[5461-6826],[10923-12287] (4096 slots) master #可看到4096个slots
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[6827-10922] (4096 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[12288-16383] (4096 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

###### 3.3.6.1.4 为新的master添加新的slave节点

需要再向当前的Redis集群中添加一个Redis单机服务器10.0.0.78，用于解决当前10.0.0.68单机的潜在宕 机问题，即实现响应的高可用功能，有两种方式：

方法1：在新加节点到集群时，直接将之设置为slave

Redis 3/4 添加方式：

```bash
redis-trib.rb   add-node --slave --master-id 750cab050bc81f2655ed53900fd43d2e64423333 10.0.0.77:6379 <任意集群节点>:6379
```

Redis 5 添加方式：

```bash
redis-cli -a 123456 --cluster add-node 10.0.0.78:6379 <任意集群节点>:6379 --cluster-slave --cluster-master-id <master ID> 
```

范例:

```bash
#查看当前状态
[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface 
may not be safe.
10.0.0.8:6379 (cb028b83...) -> 5019 keys | 4096 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 4948 keys | 4096 slots | 0 slaves.
10.0.0.48:6379 (d04e524d...) -> 5033 keys | 4096 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 5000 keys | 4096 slots | 1 slaves.
[OK] 20000 keys in 4 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[1365-5460] (4096 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[5461-6826],[10923-12287] (4096 slots) master
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[6827-10922] (4096 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[12288-16383] (4096 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

#直接加为slave节点
[root@redis-node1 ~]#redis-cli -a 123456 --cluster add-node 10.0.0.78:6379 
10.0.0.8:6379 --cluster-slave --cluster-master-id 
d6e2eca6b338b717923f64866bd31d42e52edc98

#验证是否成功
[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379 (cb028b83...) -> 5019 keys | 4096 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 4948 keys | 4096 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 5033 keys | 4096 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 5000 keys | 4096 slots | 1 slaves.
[OK] 20000 keys in 4 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[1365-5460] (4096 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[5461-6826],[10923-12287] (4096 slots) master
   1 additional replica(s)
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[6827-10922] (4096 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[12288-16383] (4096 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@centos8 ~]#redis-cli -a 123456 -h 10.0.0.8 --no-auth-warning cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:8   #8个节点
cluster_size:4          #4组主从
cluster_current_epoch:11
cluster_my_epoch:10
cluster_stats_messages_ping_sent:1810
cluster_stats_messages_pong_sent:1423
cluster_stats_messages_auth-req_sent:5
cluster_stats_messages_update_sent:14
cluster_stats_messages_sent:3252
cluster_stats_messages_ping_received:1417
cluster_stats_messages_pong_received:1368
cluster_stats_messages_meet_received:2
cluster_stats_messages_fail_received:2
cluster_stats_messages_auth-ack_received:2
cluster_stats_messages_update_received:4
cluster_stats_messages_received:2795
```

方法2：先将新节点加入集群，再修改为slave

* 为新的master添加slave节点

Redis 3/4 版本：

```bash
[root@redis-node1 ~]#redis-trib.rb add-node 10.0.0.78:6379 10.0.0.8:6379
```

Redis 5 版本：

```bash
#把10.0.0.78:6379添加到集群中：
[root@redis-node1 ~]#redis-cli -a 123456 --cluster add-node 10.0.0.78:6379 10.0.0.8:6379
```

* 更改新节点更改状态为slave：

需要手动将其指定为某个master的slave，否则其默认角色为master。

```bash
[root@redis-node1 ~]#redis-cli -h 10.0.0.78 -p 6379 -a 123456 #登录到新添加节点
10.0.0.78:6380> CLUSTER NODES #查看当前集群节点，找到目标master 的ID
10.0.0.78:6380> CLUSTER REPLICATE 886338acd50c3015be68a760502b239f4509881c #将其设置slave，命令格式为cluster replicate MASTERID

10.0.0.78:6380> CLUSTER NODES #再次查看集群节点状态，验证节点是否已经更改为指定master 的slave
```

##### 3.3.6.2 集群维护之动态缩容

实战案例：

由于10.0.0.8服务器使用年限已经超过三年，已经超过厂商质保期而且硬盘出现异常报警，经运维部架 构师提交方案并与开发同事开会商议，决定将现有Redis集群的8台主服务器中的master 10.0.0.8和对应 的slave 10.0.0.38 临时下线，三台服务器的并发写入性能足够支出未来1-2年的业务需求

删除节点过程：

添加节点的时候是先添加node节点到集群，然后分配槽位，删除节点的操作与添加节点的操作正好相 反，是先将被要删除的Redis node上的槽位迁移到集群中的其他Redis node节点上，然后再将其删除， 如果一个Redis node节点上的槽位没有被完全迁移，删除该node的时候会提示有数据且无法删除。

###### 3.3.6.2.1 迁移master 的槽位之其他master

注意: 被迁移Redis master源服务器必须保证没有数据，否则迁移报错并会被强制中断。

Redis 3/4 版本

```bash
[root@redis-node1 ~]# redis-trib.rb reshard 10.0.0.8:6379
[root@redis-node1 ~]# redis-trib.rb fix 10.0.0.8:6379 #如果迁移失败使用此命令修复集群
```

Redis 5版本

```bash
#查看当前状态
[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379 (cb028b83...) -> 5019 keys | 4096 slots | 1 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 4948 keys | 4096 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 5033 keys | 4096 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 5000 keys | 4096 slots | 1 slaves.
[OK] 20000 keys in 4 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[1365-5460] (4096 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[5461-6826],[10923-12287] (4096 slots) master
   1 additional replica(s)
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[6827-10922] (4096 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[12288-16383] (4096 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

#连接到任意集群节点，#最后1365个slot从10.0.0.8移动到第一个master节点10.0.0.28上
[root@redis-node1 ~]#redis-cli -a 123456 --cluster reshard 10.0.0.18:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Performing Cluster Check (using node 10.0.0.18:6379)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates cb028b83f9dc463d732f6e76ca6bbcd469d948a7
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots:[1365-5460] (4096 slots) master
   1 additional replica(s)
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[5461-6826],[10923-12287] (4096 slots) master
   1 additional replica(s)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[6827-10922] (4096 slots) master
   1 additional replica(s)
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[12288-16383] (4096 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
How many slots do you want to move (from 1 to 16384)? 1356 #共4096/3分别给其它三个master节点
What is the receiving node ID? d34da8666a6f587283a1c2fca5d13691407f9462 #master 
10.0.0.28
Please enter all the source node IDs.
 Type 'all' to use all the nodes as source nodes for the hash slots.
 Type 'done' once you entered all the source nodes IDs.
Source node #1: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 #输入要删除节点10.0.0.8的ID
Source node #2: done

Ready to move 1356 slots.
 Source nodes:
   M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
       slots:[1365-5460] (4096 slots) master
       1 additional replica(s)
 Destination node:
   M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
       slots:[12288-16383] (4096 slots) master
       1 additional replica(s)
 Resharding plan:
   Moving slot 1365 from cb028b83f9dc463d732f6e76ca6bbcd469d948a7
......
 Moving slot 2719 from cb028b83f9dc463d732f6e76ca6bbcd469d948a7
   Moving slot 2720 from cb028b83f9dc463d732f6e76ca6bbcd469d948a7
Do you want to proceed with the proposed reshard plan (yes/no)? yes #确定
......
Moving slot 2718 from 10.0.0.8:6379 to 10.0.0.28:6379: ..
Moving slot 2719 from 10.0.0.8:6379 to 10.0.0.28:6379: .
Moving slot 2720 from 10.0.0.8:6379 to 10.0.0.28:6379: ..

#非交互式方式
#再将1365个slot从10.0.0.8移动到第二个master节点10.0.0.48上
[root@redis-node1 ~]#redis-cli -a 123456 --cluster reshard 10.0.0.18:6379 --cluster-slots 1365 --cluster-from cb028b83f9dc463d732f6e76ca6bbcd469d948a7 --cluster-to d04e524daec4d8e22bdada7f21a9487c2d3e1057 --cluster-yes

#最后的slot从10.0.0.8移动到第三个master节点10.0.0.68上
[root@redis-node1 ~]#redis-cli -a 123456 --cluster reshard 10.0.0.18:6379 --cluster-slots 1375 --cluster-from cb028b83f9dc463d732f6e76ca6bbcd469d948a7 --cluster-to d6e2eca6b338b717923f64866bd31d42e52edc98 --cluster-yes

#确认10.0.0.8的所有slot都移走了，上面的slave也自动删除，成为其它master的slave 
[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.8:6379
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.8:6379 (cb028b83...) -> 0 keys | 0 slots | 0 slaves.
10.0.0.68:6379 (d6e2eca6...) -> 6631 keys | 5471 slots | 2 slaves.
10.0.0.48:6379 (d04e524d...) -> 6694 keys | 5461 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6675 keys | 5452 slots | 1 slaves.
[OK] 20000 keys in 4 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: cb028b83f9dc463d732f6e76ca6bbcd469d948a7 10.0.0.8:6379
   slots: (0 slots) master
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[4086-6826],[10923-12287] (5471 slots) master
   2 additional replica(s)
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[2721-4085],[6827-10922] (5461 slots) master
   1 additional replica(s)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[1365-2720],[12288-16383] (5452 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

#原有的10.0.0.38自动成为10.0.0.68的slave
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.68 INFO replication
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
# Replication
role:master
connected_slaves:2
slave0:ip=10.0.0.78,port=6379,state=online,offset=129390,lag=0
slave1:ip=10.0.0.38,port=6379,state=online,offset=129390,lag=0
master_replid:43e3e107a0acb1fd5a97240fc4b2bd8fc85b113f
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:129404
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:129404

[root@centos8 ~]#redis-cli -a 123456 -h 10.0.0.8 --no-auth-warning cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:8  #集群中8个节点
cluster_size:3       #少了一个主从的slot
cluster_current_epoch:16
cluster_my_epoch:13
cluster_stats_messages_ping_sent:3165
cluster_stats_messages_pong_sent:2489
cluster_stats_messages_fail_sent:6
cluster_stats_messages_auth-req_sent:5
cluster_stats_messages_auth-ack_sent:1
cluster_stats_messages_update_sent:27
cluster_stats_messages_sent:5693
cluster_stats_messages_ping_received:2483
cluster_stats_messages_pong_received:2400
cluster_stats_messages_meet_received:2
cluster_stats_messages_fail_received:2
cluster_stats_messages_auth-req_received:1
cluster_stats_messages_auth-ack_received:2
cluster_stats_messages_update_received:4
cluster_stats_messages_received:4894
```

###### 3.3.6.2.2 从集群删除服务器

虽然槽位已经迁移完成，但是服务器IP信息还在集群当中，因此还需要将IP信息从集群删除 

注意: 删除服务器前,必须清除主机上面的槽位,否则会删除主机失败

Redis 3/4：

```bash
redis-trib.rb del-node <任意cluster节点的IP:port> <删除节点的ID>
```

范例: 

```
[root@s~]#redis-trib.rb del-node 10.0.0.8:6379 dfffc371085859f2858730e1f350e9167e287073
>>> Removing node dfffc371085859f2858730e1f350e9167e287073 from cluster 192.168.7.102:6379
>>> Sending CLUSTER FORGET messages to the cluster...
>>> SHUTDOWN the node.

```

Redis 5：

```http
redis-cli -a 123456 --cluster del-node <任意cluster节点的IP:port> <删除节点的ID>
```

范例:

```bash
[root@redis-node1 ~]#redis-cli -a 123456 --cluster del-node 10.0.0.8:6379 cb028b83f9dc463d732f6e76ca6bbcd469d948a7
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Removing node cb028b83f9dc463d732f6e76ca6bbcd469d948a7 from cluster 10.0.0.8:6379
>>> Sending CLUSTER FORGET messages to the cluster...
>>> SHUTDOWN the node.
#删除节点后,redis进程自动关闭
#删除节点信息
[root@redis-node1 ~]#rm -f /var/lib/redis/nodes-6379.conf
```

###### 3.3.6.2.3 删除多余的slave节点验证结果

```bash
#验证删除成功
[root@redis-node1 ~]#ss -ntl
State       Recv-Q       Send-Q   Local Address:Port     Peer Address:Port     
LISTEN       0             128            0.0.0.0:22             0.0.0.0:*       
LISTEN       0             128               [::]:22               [::]:*  

[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.18:6379 
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.68:6379 (d6e2eca6...) -> 6631 keys | 5471 slots | 2 slaves.
10.0.0.48:6379 (d04e524d...) -> 6694 keys | 5461 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6675 keys | 5452 slots | 1 slaves.
[OK] 20000 keys in 3 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.18:6379)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
S: f9adcfb8f5a037b257af35fa548a26ffbadc852d 10.0.0.38:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[4086-6826],[10923-12287] (5471 slots) master
   2 additional replica(s)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[2721-4085],[6827-10922] (5461 slots) master
   1 additional replica(s)
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[1365-2720],[12288-16383] (5452 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

#删除多余的slave从节点
[root@redis-node1 ~]#redis-cli -a 123456 --cluster del-node 10.0.0.18:6379 f9adcfb8f5a037b257af35fa548a26ffbadc852d
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
>>> Removing node f9adcfb8f5a037b257af35fa548a26ffbadc852d from cluster 10.0.0.18:6379
>>> Sending CLUSTER FORGET messages to the cluster...
>>> SHUTDOWN the node.

#删除集群文件
[root@redis-node4 ~]#rm -f /var/lib/redis/nodes-6379.conf 

[root@redis-node1 ~]#redis-cli -a 123456 --cluster check 10.0.0.18:6379 
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.68:6379 (d6e2eca6...) -> 6631 keys | 5471 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 6694 keys | 5461 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6675 keys | 5452 slots | 1 slaves.
[OK] 20000 keys in 3 masters.
1.22 keys per slot on average.
>>> Performing Cluster Check (using node 10.0.0.18:6379)
S: 99720241248ff0e4c6fa65c2385e92468b3b5993 10.0.0.18:6379
   slots: (0 slots) slave
   replicates d04e524daec4d8e22bdada7f21a9487c2d3e1057
S: 36840d7eea5835ba540d9b64ec018aa3f8de6747 10.0.0.78:6379
   slots: (0 slots) slave
   replicates d6e2eca6b338b717923f64866bd31d42e52edc98
M: d6e2eca6b338b717923f64866bd31d42e52edc98 10.0.0.68:6379
   slots:[0-1364],[4086-6826],[10923-12287] (5471 slots) master
   1 additional replica(s)
S: 9875b50925b4e4f29598e6072e5937f90df9fc71 10.0.0.58:6379
   slots: (0 slots) slave
   replicates d34da8666a6f587283a1c2fca5d13691407f9462
M: d04e524daec4d8e22bdada7f21a9487c2d3e1057 10.0.0.48:6379
   slots:[2721-4085],[6827-10922] (5461 slots) master
   1 additional replica(s)
M: d34da8666a6f587283a1c2fca5d13691407f9462 10.0.0.28:6379
   slots:[1365-2720],[12288-16383] (5452 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

[root@redis-node1 ~]#redis-cli -a 123456 --cluster info 10.0.0.18:6379 
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
10.0.0.68:6379 (d6e2eca6...) -> 6631 keys | 5471 slots | 1 slaves.
10.0.0.48:6379 (d04e524d...) -> 6694 keys | 5461 slots | 1 slaves.
10.0.0.28:6379 (d34da866...) -> 6675 keys | 5452 slots | 1 slaves.
[OK] 20000 keys in 3 masters.
1.22 keys per slot on average.

#查看集群信息
[root@redis-node1 ~]#redis-cli -a 123456 -h 10.0.0.18 CLUSTER INFO
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6  #只有6个节点
cluster_size:3
cluster_current_epoch:11
cluster_my_epoch:10
cluster_stats_messages_ping_sent:12147
cluster_stats_messages_pong_sent:12274
cluster_stats_messages_update_sent:14
cluster_stats_messages_sent:24435
cluster_stats_messages_ping_received:12271
cluster_stats_messages_pong_received:12147
cluster_stats_messages_meet_received:3
cluster_stats_messages_update_received:28
cluster_stats_messages_received:24449
```

##### 3.3.6.3 集群维护之导入现有Redis数据至集群

官方提供了离线迁移数据到集群的工具,有些公司开发了离线迁移工具 

* 官方工具: redis-cli --cluster import 
* 第三方在线迁移工具: 模拟slave 节点实现, 比如: 唯品会 redis-migrate-tool , 豌豆荚 redis-port

实战案例： 

公司将redis cluster部署完成之后，需要将之前的数据导入之Redis cluster集群，但是由于Redis cluster 使用的分片保存key的机制，因此使用传统的AOF文件或RDB快照无法满足需求，因此需要使用集群数据导入命令完成。

注意: 导入数据需要redis cluster不能与被导入的数据有重复的key名称，否则导入不成功或中断。

###### 3.3.6.3.1 基础环境准备

导入数据之前需要关闭各redis 服务器的密码，包括集群中的各node和源Redis server，避免认证带来 的环境不一致从而无法导入，可以加参数--cluster-replace 强制替换Redis cluster已有的key。

```bash
#在所有节点包括master和slave节点上关闭各Redis密码认证
[root@redis ~]# redis-cli -h 10.0.0.18 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
OK
```

###### 3.3.6.3.2 执行数据导入

将源Redis server的数据直接导入之 redis cluster,此方式慎用!

Redis 3/4：

```bash
[root@redis ~]# redis-trib.rb import --from <外部Redis node-IP:PORT> --replace <集群服务器IP:PORT>
```

Redis 5：

```bash
[root@redis ~]#redis-cli --cluster import <集群服务器IP:PORT> --cluster-from <外部Redis node-IP:PORT> --cluster-copy --cluster-replace

#只使用cluster-copy，则要导入集群中的key不能存在
#如果集群中已有同样的key，如果需要替换，可以cluster-copy和cluster-replace联用，这样集群中的key就会被替换为外部数据
```

范例：将非集群节点的数据导入redis cluster 

```bash
#在非集群节点10.0.0.78生成数据
[root@centos8 ~]#hostname -I
10.0.0.78 
[root@centos8 ~]#cat redis_test.sh 
#!/bin/bash
#********************************************************************
NUM=10
PASS=123456
for i in `seq $NUM`;do
   redis-cli -h 127.0.0.1 -a "$PASS"  --no-auth-warning  set testkey${i}
testvalue${i}
    echo "testkey${i} testvalue${i} 写入完成"
done
echo "$NUM个key写入到Redis完成"  
[root@centos8 ~]#bash redis_test.sh
OK
testkey1 testvalue1 写入完成
OK
testkey2 testvalue2 写入完成
OK
testkey3 testvalue3 写入完成
OK
testkey4 testvalue4 写入完成
OK
testkey5 testvalue5 写入完成
OK
testkey6 testvalue6 写入完成
OK
testkey7 testvalue7 写入完成
OK
testkey8 testvalue8 写入完成
OK
testkey9 testvalue9 写入完成
OK
testkey10 testvalue10 写入完成
10个key写入到Redis完成

#取消需要导入的主机的密码
[root@centos8 ~]#redis-cli -h 10.0.0.78 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""

#取消所有集群服务器的密码
[root@centos8 ~]#redis-cli -h 10.0.0.8 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
[root@centos8 ~]#redis-cli -h 10.0.0.18 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
[root@centos8 ~]#redis-cli -h 10.0.0.28 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
[root@centos8 ~]#redis-cli -h 10.0.0.38 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
[root@centos8 ~]#redis-cli -h 10.0.0.48 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""
[root@centos8 ~]#redis-cli -h 10.0.0.58 -p 6379 -a 123456 --no-auth-warning CONFIG SET requirepass ""

#导入数据至集群
[root@centos8 ~]#redis-cli --cluster import 10.0.0.8:6379 --cluster-from 10.0.0.78:6379 --cluster-copy --cluster-replace
>>> Importing data from 10.0.0.78:6379 to cluster 10.0.0.8:6379
>>> Performing Cluster Check (using node 10.0.0.8:6379)
M: a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab 10.0.0.8:6379
   slots:[0-5461] (5462 slots) master
   1 additional replica(s)
M: 4f146b1ac51549469036a272c60ea97f065ef832 10.0.0.28:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 779a24884dbe1ceb848a685c669ec5326e6c8944 10.0.0.48:6379
   slots: (0 slots) slave
   replicates 97c5dcc3f33c2fc75c7fdded25d05d2930a312c0
M: 97c5dcc3f33c2fc75c7fdded25d05d2930a312c0 10.0.0.18:6379
   slots:[5462-10922] (5461 slots) master
   1 additional replica(s)
S: 07231a50043d010426c83f3b0788e6b92e62050f 10.0.0.58:6379
   slots: (0 slots) slave
   replicates 4f146b1ac51549469036a272c60ea97f065ef832
S: cb20d58870fe05de8462787cf9947239f4bc5629 10.0.0.38:6379
   slots: (0 slots) slave
   replicates a177c5cbc2407ebb6230ea7e2a7de914bf8c2dab
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
*** Importing 10 keys from DB 0
Migrating testkey4 to 10.0.0.18:6379: OK
Migrating testkey8 to 10.0.0.18:6379: OK
Migrating testkey6 to 10.0.0.28:6379: OK
Migrating testkey1 to 10.0.0.8:6379: OK
Migrating testkey5 to 10.0.0.8:6379: OK
Migrating testkey10 to 10.0.0.28:6379: OK
Migrating testkey7 to 10.0.0.18:6379: OK
Migrating testkey9 to 10.0.0.8:6379: OK
Migrating testkey2 to 10.0.0.28:6379: OK
Migrating testkey3 to 10.0.0.18:6379: OK

#验证数据
[root@centos8 ~]#redis-cli -h 10.0.0.8 keys '*'
1) "testkey5"
2) "testkey1"
3) "testkey9"
[root@centos8 ~]#redis-cli -h 10.0.0.18 keys '*'
1) "testkey8"
2) "testkey4"
3) "testkey3"
4) "testkey7"
[root@centos8 ~]#redis-cli -h 10.0.0.28 keys '*'
1) "testkey6"
2) "testkey10"
3) "testkey2"
```

##### 3.3.6.4 集群偏斜

redis cluster 多个节点运行一段时间后,可能会出现倾斜现象,某个节点数据偏多,内存消耗更大,或者接受 用户请求访问更多

发生倾斜的原因可能如下:

* 节点和槽分配不均 
* 不同槽对应键值数量差异较大 
* 包含bigkey,建议少用 
* 内存相关配置不一致 
* 热点数据不均衡 : 一致性不高时,可以使用本地缓存和MQ

获取指定槽位中对应键key值的个数

```bash
#redis-cli cluster countkeysinslot {slot的值}
```

范例: 获取指定slot对应的key个数

```bash
[root@centos8 ~]#redis-cli -a 123456 cluster countkeysinslot 1
(integer) 0
[root@centos8 ~]#redis-cli -a 123456 cluster countkeysinslot 2
(integer) 0
[root@centos8 ~]#redis-cli -a 123456 cluster countkeysinslot 3
(integer) 1
```

执行自动的槽位重新平衡分布,但会影响客户端的访问,此方法慎用

```bash
#redis-cli --cluster rebalance <集群节点IP:PORT>
```

范例: 执行自动的槽位重新平衡分布

```bash
[root@centos8 ~]#redis-cli -a 123456 --cluster rebalance 10.0.0.8:6379
>>> Performing Cluster Check (using node 10.0.0.8:6379)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
*** No rebalancing needed! All nodes are within the 2.00% threshold.
```

获取bigkey ,建议在slave节点执行

```bash
#redis-cli --bigkeys
```

范例: 查找 bigkey

```bash
[root@centos8 ~]#redis-cli -a 123456 --bigkeys
# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type. You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).
[00.00%] Biggest string found so far 'key8811' with 9 bytes
[26.42%] Biggest string found so far 'testkey1' with 10 bytes
-------- summary -------
Sampled 3335 keys in the keyspace!
Total key length in bytes is 22979 (avg len 6.89)
Biggest string found 'testkey1' has 10 bytes
3335 strings with 29649 bytes (100.00% of keys, avg size 8.89)
0 lists with 0 items (00.00% of keys, avg size 0.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
0 hashs with 0 fields (00.00% of keys, avg size 0.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00)
0 streams with 0 entries (00.00% of keys, avg size 0.00)
```

#### 3.3.7 redis cluster 的局限性

* 大多数时客户端性能会”降低” 
* 命令无法跨节点使用:mget、keys、scan、flush、sinter等 
* 客户端维护更复杂:SDK和应用本身消耗(例如更多的连接池) 
* 不支持多个数据库︰集群模式下只有一个db 0 
* 复制只支持一层∶不支持树形复制结构,不支持级联复制 
* Key事务和Lua支持有限∶操作的key必须在一个节点,Lua和事务无法跨节点使用

范例: 跨slot的局限性

```bash
[root@centos8 ~]#redis-cli -a 123456 mget key1 key2 key3
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
(error) CROSSSLOT Keys in request don't hash to the same slot
```

### 3.4 redis 扩展集群方案

在Redis 官方自带的Redis cluster集群功能出来之前，有一些开源的集群解决方案可被参考使用

#### 3.4.1 codis

Codis 是一个豌豆荚开发基于go实现的分布式 Redis 代理解决方案, 对于上层的应用来说, 连接到 Codis  Proxy 和连接原生的 Redis Server 没有显著区别 (不支持的命令列表), 上层应用可以像使用单机的 Redis  一样使用, Codis 底层会处理请求的转发, 不停机的数据迁移等工作, 所有后边的一切事情, 对于前面的客 户端来说是透明的, 可以简单的认为后边连接的是一个内存无限大的 Redis 服务

codis proxy 对客户端来说相当于redis，即连接codis proxy和连接redis是没有任何区别的，codisproxy无状态，不负责记录是否在哪保存，数据在zookeeper记录，即codis proxy向zookeeper查询key 的记录位置，codis proxy 将请求转发到一个group组进行处理，一个group 里面有一个master和一个 或者多个slave组成

codis 默认有1024个槽位，而redis cluster 默认有16384个槽位，其把不同的槽位的内容放在不同的 group

Github 地址：https://github.com/CodisLabs/codis/

#### 3.4.2 twemproxy

Twemproxy是一种代理分片机制，由Twitter开源。Twemproxy作为代理，可接受来自多个程序的访 问，按照proxy配置的算法，转发给后台的各个Redis服务器，再原路返回。解决了单个Redis实例承载 能力的问题。Twemproxy本身也是单点，需要用Keepalived做高可用方案。通过Twemproxy可以使用 多台服务器来水平扩张redis服务，可以有效的避免单点故障问题。虽然使用Twemproxy需要更多的硬 件资源和在redis性能有一定的损失（twitter测试约20%），也不支持数据迁移。但是能够提高整个系统 的HA,另外twemproxy实现了memcached协议 

Github 地址：https://github.com/twitter/twemproxy.
