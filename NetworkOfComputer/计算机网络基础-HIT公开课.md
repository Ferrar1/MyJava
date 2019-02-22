- [计算机网络概述](#计算机网络概述)
  - [协议三要素](#协议三要素)
  - [计算机网络结构](#计算机网络结构)
    - [网络边缘](#网络边缘)
    - [接入网络](#接入网络)
    - [网络核心](#网络核心)
  - [计算机体系结构](#计算机体系结构)
    - [分层体系结构基本概念](#分层体系结构基本概念)
    - [OSI参考模型](#osi参考模型)
    - [TCP/IP分层模型图](#tcpip分层模型图)
- [TCP/IP分层模型](#tcpip分层模型)
  - [应用层](#应用层)
    - [Web应用](#web应用)
    - [Email应用](#email应用)
    - [DNS服务](#dns服务)
    - [P2P应用](#p2p应用)
    - [网络编程](#网络编程)
  - [传输层](#传输层)
    - [多路复用/分用](#多路复用分用)
    - [无连接传输协议](#无连接传输协议)
    - [可靠数据传输机制](#可靠数据传输机制)
    - [滑动窗口协议](#滑动窗口协议)
    - [TCP面向连接的传输协议](#tcp面向连接的传输协议)
    - [拥塞控制原理](#拥塞控制原理)
    - [拥塞控制算法](#拥塞控制算法)
  - [网络层](#网络层)
    - [核心功能：转发与路由。](#核心功能转发与路由)
    - [网络层服务模型](#网络层服务模型)
      - [虚电路网络](#虚电路网络)
      - [数据报网络](#数据报网络)
    - [internet的网络层](#internet的网络层)
      - [ipv4协议](#ipv4协议)
      - [dhcp协议](#dhcp协议)
      - [nat网络地址转换](#nat网络地址转换)
      - [icmp协议](#icmp协议)
      - [ipv6](#ipv6)
    - [路由算法](#路由算法)
      - [链路状态路由算法](#链路状态路由算法)
      - [距离向量路由算法](#距离向量路由算法)
      - [层次路由](#层次路由)
      - [icmp协议](#icmp协议)
    - [internet路由](#internet路由)
  - [数据链路层](#数据链路层)
    - [差错检测](#差错检测)
    - [链路接入之多路访问控制mac协议](#链路接入之多路访问控制mac协议)
  - [局域网](#局域网)
    - [arp协议](#arp协议)
      - [mac地址](#mac地址)
    - [以太网ethernet](#以太网ethernet)
      - [交换机](#交换机)
      - [虚拟局域网vlan](#虚拟局域网vlan)
    - [ppp协议](#ppp协议)
    - [wifi即802.11无线局域网](#wifi即80211无线局域网)

## 计算机网络概述

### 协议三要素

- 语法：数据与控制信息的结构或格式，包括数据的组织方式、编码方式、信号电平的表示方式等。（用于解读）
- 语义：即需要发出何种控制信息，完成何种动作，以及做出何种应答，以实现数据交换的协调和差错处理。（用来说明通信双方应当怎么做）
- 时序：即事件实现顺序的详细说明，以实现速率匹配和排序。（定义了何时进行通信，先讲什么，后讲什么，讲话的速度等）


### 计算机网络结构

#### 网络边缘
即主机（端系统），运行网络应用程序。普通网络用户通过使用各种网络应用实现网络资源的共享，通常这些网络资源位于网络边缘。通信方式：

   - 客户/服务器应用模型；
   - P2P应用模型，没有专用服务器，通信再对等实体之间进行通信（如QQ）


#### 接入网络
解决的问题是如何将网络边缘接入核心网（边缘路由器）。分类：

   - 住宅接入网络
      - 数字用户线路（DSL），独占。利用已有的电话线连接中心局的DSLAM。
      - 电缆网络。共享。在不同频带（载波）上传输不同频道
   - 机构（学校、企业）接入网络。局域网。以太。 
   - 移动接入网络。通过共享的无线接入网络连接端系统与路由器。通过基站或接入点。如WiFi。

#### 网络核心
1. 关键功能：路由、转发。解决的基本问题：如何实现数据从源主机通过网络核心送达目的主机。手段就是**数据交换**.

>为何进行数据交换：n台主机相连需要产生（N-1)N/2条链路，数量庞大不可行；因此引入交换设备，实现在主机之间实现动态数据转换，但如果主机数量多要求交换设备的端口需要大，另外如果网络距离大很难连接在一个交换设备上；最终解决方案：将交换设备互联在一起形成交换网络，让主机跟交换网络中的某一个交换设备连接。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-为何进行数据交换.PNG" width=50% height=50% />
 
2. 交换： 实现动态转接（在所有交换设备端口之间建立物理逻辑的连接，动态的将一个端口的数据转到另一个端口）

    - 电路交换。如果多台主机相连产生多条链路，会有共享的路段称中继线，这些如何共享：**多路复用**。<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-电路交换.PNG" width=66% height=66% />
    - 报文交换。一个完整的信息。存储转发。从一个路由转发到另一个路由需要完整的信息才能转发（串行）。而分组转发时每完成一个数据包就能转发一个数据包（并行）。<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-报文交换.PNG" width=66% height=66% />
    - 分组交换。将报文拆分出来一系列小的数据包，再加一个头部信息。需要报文的拆分重组。不用事先分配好。分组传输时延=每组L(bits)的数据包/R(链路传输速率)，注意是bit为单位进行的计算。而分组交换的报文交换时间：`T=M/R+n*L/R`.其中n为路由器数。
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-分组交换.PNG" width=50% height=50% />

   过程：在分组交换网络中，分组到达路由器，路由器会按照存储转发的方式先收下来，收下来先暂存，确定了在哪一个链路转发后，在这个链路进行转发，如果这个链路正在转发别的分组，那就要排队等着。路由中供排队的缓存都是有限的，如果缓存满了，就会将这个分组丢弃，即数据包的丢失。中间的四种分组延迟：

    - 结点处理延迟：分组到达路由器，路由器要对这个分组进行处理
    - 排队延迟：当分组确定从某一链路传出，但这一链路刚好在传输其他的分组，就要在结点排队等待
    - 传输延迟：当某一链路开始传输分组时，从分组的第一个bit开始到最后一个bit发完，需要的时间称为传输延迟。也叫发送时延
    - 传播延迟：当分组从某一结点发出后，到达下一个结点所花的时间。
>扩展：   
>1、端系统通过接入ISP连接到Internet。(Internet Service Provider)，互联网服务提供商。   
>2、链路的时延带宽积：以比特为单位的链路长度。计算：传播时延X带宽(R)。   
>3、吞吐量：在发送端与接收端之间传输数据速率。取决于端到端的路径上的瓶颈链路（R小的端）。


### 计算机体系结构
#### 分层体系结构基本概念：

- 实体，表示任何可发送或接受信息的硬件或软件进程。
- 协议，控制2个对等实体进行通信的规则的集合，协议是水平的，包括三要素。
- 任一层实体需要使用下层服务，遵循本层协议，实现本层功能，向上层提供服务，服务是垂直的。
- 下层协议的实现对上层的服务用户是不可见的。
- 同系统中的相邻层实体之间通过接口进行交互，通过服务访问点SAP交换原语（请求、响应、指定、确认等），指定请求的特定服务。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构概念.PNG" width=66% height=66% />

#### osi参考模型

1. OSI解释的通信过程，如下图所示：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构通信过程.PNG" width=65% height=65% />
   
   **数据封装**与通信过程，如下图所示：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构通信过程数据封装.PNG" width=70% height=70% />

2. 物理层：概括的说是实现bit流在物理介质上的传输。 
   - 定义、规范接口特性：主机跟发送设备之间的接口、发送设备跟物理介质之间的接口。
   - 比特的编码：用信号的什么特征来表示比特。
   - 数据率。
   - 比特同步：解决时钟同步问题。
   - 传输模式：按什么方式传输：单工（单向），半双工（可以双向，只能交替进行）、全双工
   
3. 数据链路层：解决物理链路直接相连的2个相邻节点之间的数据传输，以帧为数据单元传输。
   - 负责节点与节点的数据传输
   - 组帧：目的在于让接受一端从物理层接受到一系列比特流的时候，能成功切分出来不同的帧。 
   - 物理寻址：在帧头增加发送端或接受端的物理地址标识数据帧的发送端/接收端。
   - 流量控制：匹配发送端与接收端的发送速度与接受速度。
   - 差错控制：检测并重传损坏或丢失帧，并避免重复帧。
   - 访问控制：在任一给定时刻决定哪个设备拥有链路控制权。
   
4. 网络层：负责源主机到目的主机数据分组交付。
   - 可能跨越多个网络，因此链路层的物理寻址就不管用了，因此需要逻辑寻址——全局唯一逻辑地址，确保数据分组被送达目的主机，如IP地址。
   - 路由：帮助数据分组选路径。
   
5. 传输层：负责源-目的（端到端）进程间的完整报文传输。即，将会话层给的完整报文，分割成数据段，交给网络层进行传输。
   - 报文的分段与重组。
   - SAP寻址。既然是进程到进程之间，因此需要确保将完整报文提交给正确进程，如端口号。
   - 连接控制。端到端的连接建立。
   - 流量控制
   - 差错控制
   
6. 会话层：接受表示层，构造会话层的数据单元。注意：不会进行分割。
   - 对话控制。
   - 同步。在数据流中插入同步点。
   
7. 表现层：处理2个系统之间交换信息的语法语义问题。 
   - 数据表示转化。将数据转换为独立编码，与主机无关。
   - 加密解密
   - 压缩解压缩。
   
8. 应用层：运行的应用。

>数据封装：增加控制信息，包括地址（标识发送端/接收端）、差错检测编码、以及协议控制（实现协议功能的附加信息，如优先级、服务质量、安全控制）
   

#### tcp/ip分层模型图
通信过程如下图所示：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系TCP.PNG" width=70% height=70% />

## tcp/ip分层模型

### 应用层
1. 网络体系结构

  - CS
  - P2P，适用于文件传输等
    - 没有永远在线的服务器
    - 任意端系统/节点之间可以直接通讯
    - 节点间歇性接入网络
    - 节点可能改变IP地址
  - 混合结构
    - Napster:文件传输使用P2P，文件搜索使用CS结构 

2. 网络应用进程通信   

  - 同一主机运行的进程之间的通信：进程间通信机制，由操作系统提供。
  - 不同主机运行的进程通信：
    - 消息（报文）交换，包括客户机进程（发起通信的进程），服务机进程（等待通信请求的进程）

>进程之间通信利用SOCKET发送/接收消息。不同主机上的进程通信，每个进程需要唯一标识符。一台主机会有多个进程，因此需要IP＋端口来作为标识符。具体消息交换的方式依靠应用层协议。

3. 网络应用对传输服务的需求

  - 数据丢失/可靠性：有的能容忍一定的数据丢失（网络电话），有的要求100%可靠（文件传输）
  - 时间/延迟：有的应用在延迟足够低才有效，网络游戏。
  - 带宽：有的应用只有在带宽达到最低要求才有效（网络视频）有的适应任何带宽（email）

#### web应用

1. 内容：

  - 万维网：网页，网页互相链接
  - 网页包含多个对象：HTML文件，图片等。
  - 对象寻址：URL，统一资源定位符。

2. HTTP应用层协议使用的传输层协议是TCP,其过程：   

  - 服务器在80端口等待客户请求
  - 浏览器发起服务器的TCP连接（创建套接字Socket）
  - 服务器接受来自浏览器的TCP连接
  - 浏览器跟服务器交换HTTP消息
  - 关闭TCP连接。

>http属于无状态协议。就是不记录之前的客户群请求的信息。因为如果突然关闭后重启产生状态不一致。维护代价高。

3. Http连接的2种类型：

  - 非持久性连接：当用户在浏览器输入网址www.example.com/index.html 时过程如下图，缺点是慢，为每个TCP连接开销资源。 <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用tcp.png" width=100% height=100% />
  - 持久性连接：发送响应后服务器保持TCP连接的打开，后续HTTP消息可以通过这个连接发送，带流水机制的持久性连接是客户端只要遇到一个引用对象就尽快发出请求（所有引用对象一共耗时1个RTT），无流机制的持久性连接是只有前一个收到响应完成请求才发出新的请求（每个对象耗时1个RTT）。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用tcp时间.PNG" width=35% height=35% />

>RTT(round trip time)从客户端发送一个很小的数据包到服务器并返回，所经历的时间。

4. HTTP协议的2类消息

请求消息：ASCII人工可读。其中Host为请求的服务器地址(用于缓存与服务器代理)。User-agent为浏览器版本。Connection发完之后的连接状态。

响应消息：Date是web服务器生成响应消息的时间。Last-modified网页上次修改的时间，不同。

5. cookie技术

   由于HTTP协议是无状态协议，因此用cookie记录客户端的信息。cookie组件：

    - 保存在客户端主机上的cookie文件，由浏览器管理。
    - HTTP响应消息的cookie头部行
    - HTTP请求详细的cookie头部行
    - web服务器端的后台数据库   
    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用cookie.PNG" width=63% height=63% />

6. 缓存/代理服务器技术

   功能：在不访问服务器的前提下满足客户端的HTTP请求。目的：缩短客户请求的响应时间，减少机构组织的流量，在大范围内实现有效的内容分发（例如CDN）

   过程：用户设定浏览器通过缓存进行web访问，作用是浏览器不再向目的服务器发送HTTP请求，而是向缓存/代理服务器发送HTTP请求，如果所请求对象在缓存中，就直接返回，否则缓存服务器向目的服务器发送HTTP请求获取对象，然后返回客户端并保存。
>现在有另外的问题，代理服务器上的缓存是否为最新，解决为**条件性GET方法**:在HTTP请求消息中声明所持有的版本日期if-modified-since。

#### email应用
1. 构成组件

  - 邮件客户端（user agent）
  - 邮件服务器，核心。当计算机不在线时就发送到邮件服务器，否则没法建立连接进行发送。
    - 消息队列存储等待发送的Email。
    - 邮箱，存储发给该用户的Email。
  - Smtp协议，使用TCP，端口25.传输三阶段：握手，消息传输，关闭。采用命令/响应模式（与HTTP的请求响应不同）。使用持久性连接，消息必须为7位ASCII构成。
    - 头部行：To、From、Subject
    - 消息体：只能是ASCII字符。
    - MIME：为了支持多媒体二进制文件，在邮件头部增加额外的行以声明MIME的内容.下图为一个消息结构：

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4Email.PNG" width=65% height=65% />


>与HTTP的对比：HTTP为pull,smtp为push.

>邮件传输协议。发送方通过STMP发送给邮寄服务器，邮件服务器通过STMP发送给接收方所在的邮件服务器，而之后再发送到接收方是通过邮件传输协议。常见的有POP协议（无状态协议）、IMAP协议（支持跨会话）

### dns服务
1. 解决互联网上主机/路由器的识别问题。

2. 域名解析系统DNS将域名解析为IP地址。DNS采用分布式层次数据库，如下图:

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS数据库.PNG" width=65% height=65% />

   **当客户端查询www.amazon.com时**

    - 客户端查询根服务器，找到com域名解析服务器。
    - 顶级域名服务器负责com、cn等：客户端查询com域名解析服务器，找到Amazon.com域名解析服务器。该服务器一般被缓存在本地，因此根服务器不经常访问。
    - 权威域名服务器：客户端查询Amazon.com域名解析服务器，获得www.amazon.com的IP地址。
    - 本地域名服务器：不在上述的层次结构中，当主机进行DNS查询时，查询被发送到本地域名服务器。有２种查询方式：迭代与递归查询.

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS查询之迭代.PNG" width=35% height=35% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS查询之递归.PNG" width=35% height=35% />

>DNS做负载均衡：对于一个大的门户网站，在DNS服务器中为同一个主机名配置多个IP地址，当进行域名向IP地址的翻译时，将以DNS文件中主机记录的IP地址按顺序返回不同的解析结果，将客户端的访问引导到不同的机器上去

3. DNS记录

   资源记录格式（name,value,type,ttl）

    - Type=A时，Name：主机域名，Value：IP地址
    - Type=NS，Name：域（edu.cn），Value：该域权威域名解析器的主机域名。
    - Type=Cname,Name:某一真实域名的别名，value：真实域名。
    - Type=MX,Value是与name相对应的邮件服务器。

4. DNS协议与消息

   DNS协议：查询和回复协议。

   消息头部：

    - identification：16位查询编号，回复的时候使用相同的编号。
    - flags

5. 注册域名

    - 向域名管理机构提供你的权威域名解析服务器的名字和IP地址
    - 域名管理机构向com顶级域名解析服务器中插入2条记录
      - （example.com,dns1.example.com,NS）
      - (dns1.example.com,212.212.212.1,A)
    - 在权威域名解析器中为www.example.com 加入Type=A记录，让自己的主页被外面访问。为example.com加入Type=MX记录可以使用邮件系统。



#### p2p应用
1. peer-to-peer,没有服务器，热议端系统之间直接通信，节点阶段性接入Internet，节点可能更换IP地址。

2. 文件分发：BitTorrent协议。torrent是指交换同一个文件的文件块的节点组（所有参与的节点形成了一个组）。tracker是跟踪参与torrent的节点（跟踪有哪些节点参与到了torrent）。如下。
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/5P2P之BT.PNG" width=66% height=66% />

   过程就是：

    - 将目的文件划分为256KB的chunk
    - 节点加入torrent时，如果节点没有chunk，将逐渐积累，同时向tracker注册以获取节点清单，然后可以与邻近节点建立连接。获取chunk时，
      - 任意时刻，不同节点持有文件不同的chunk集合
      - 节点定期查询每个邻居节点所持有的chunk列表
      - 节点发送请求，请求获取缺失的chunk
    - 下载的同时，节点需要向其他节点上传chunk，发送chunk时，
      - 节点向4个邻居发送chunk（正在给我发且速度最快的4个相邻节点）
      - 每30s随机选择一个其他节点，向其发送chunk
    - 节点可能加入或者离开，动态。一旦节点获取完整文件，可以选择留下或者离开。

3. 索引技术

   在P2P应用中需要进行搜索信息（如何找到节点地址），因此建立P2P系统的索引：信息到节点（IP+端口）的映射。

   集中式索引，如下图：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/5P2P之集中式索引.PNG" width=66% height=66% />


   洪泛式索引，Query Flooding，完全分布式架构，每个节点对他共享的文件进行索引，且只对他共享的文件进行索引。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/5P2P之泛洪.PNG" width=35% height=35% />

   过程：

      - 查询消息通过已有TCP连接发送
      - 任何收到消息的节点进行转发查询消息。
      - 如果查询命中，则利用反向路径发回查询节点
>覆盖网络，overlay network。节点之间如果有TCP连接，则这两个节点构成一个边；所有的活动节点和边构成覆盖网络；节点邻居数少于10个，这里边是虚拟链路。

层次式覆盖网络，介于集中式与洪泛式。每个节点是普通或者超级节点，节点与超级节点之间维持TCP连接(集中式)，某些超级节点对之间维持TCP连接（洪泛式）。而超级节点负责跟踪子节点的内容。

#### 网络编程

1. 如下图，表面上是应用进程之间通过应用层协议在控制，其实还是通过操作系统在控制数据交换，因此应用层需要将数据传输到下一层，故需要接口来完成应用进程与底层交互。 该接口称为应用编程接口API。常见的接口有：socket、WinSock、TLI。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket.PNG" width=50% height=50% />
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket类型.PNG" width=50% height=50% />

2. 如何标识socket

    - 标识通信端点（对外）：IP地址+端口号。
    - 在操作系统/进程层面上（对内）：当应用程序创建socket时，操作系统分配一个数据结构存储该socket的相关信息，并返回一个socket descripter。即一个网络应用进程利用socket descripter(套接字描述符，小整数)引用其创建的一个Socket。如下图：

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket数据结构.PNG" width=50% height=50% />

>使用TCP/IP协议簇的网络应用程序声明**端点地址**变量时专门定义了数据结构sockaddr_in

3. winSock独有的函数

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket过程.PNG" width=45% height=45% />

    - WSAStartUp,初始化winSock的API。进行网络编程的应用程序在使用socket之前必须首先调用该函数。参数：
      - 第一个参数指明程序请求使用的winSock的版本
      - 第二个参数返回实际的winSock版本信息
    - WSACleanUp,释放所使用的winSock的DLL。进行网络编程的应用程序在使用完Socket库后最后要调用的函数。

4. 各平台基本适用的函数，非WinSock独有

    - socket，创建套接字，成功后操作系统返回套接字描述符。参数：
      - 第一个参数为协议族`protofamily=PF_INET`,表示TCP/IP
      - 第二个参数为套接字类型。TCP/IP有三种类型：SOCK_STREAM,SOCK_DGRAM,SOCK_RAW.
      - 第三个参数为协议号，用于说明创建的面向protofamily的协议族的type类型的套接字将要访问的是哪种协议。默认为0.
    - CloseSocket,关闭某一特定描述符的socket，如果多个进程共享一个socket，调用该函数后将socket的引用计数减1，直到0才关闭。
    - bind，一般服务器端。绑定socket的本地端点地址。客户程序一般不需调用，操作系统会做，服务器端绑定默定熟悉的端口号，以及使用地址通配符INADDR_ANY来绑定IP地址（存在多个IP地址的情况）参数：
      - socket描述符，可以通过指针指向对应的Socket数据结构
      - 端点地址，sockaddr_in类型，里面包括IP+端口。
      - 地址长度 
    - listen,只服务器端，仅面向连接的流套接字。设置连接请求队列大小。
    - connect，只用于客户端。客户程序调用connect来使客户socket与特定计算机的特定端口的socket进行连接。注意：TCP客户端建立TCP连接，而UDP客户端调用时只是指定了服务器端点地址。
    - accept，只服务器端。服务器调用时从处于监听状态的流socket的客户请求队列中取出最前面的一个客户请求，并创建新的socket来与客户socket创建连接通道（仅TCP的socket）。
    - send、sendto。
    - recv、recvfrom。
    - setsocket设置socket的选项参数
    - getsocket获取socket的选项当前值。

5. 网络字节顺序

    在OSI中表示层处理2个系统之间交换信息的语法语义问题，而TCP环境里面，TCP/IP定义了标准的用于协议头中的二进制整数：网络字节顺序，独立于操作系统的。因此一些socket的API函数的参数需要存储为网络字节参数。因此需要一些可以实现本地字节顺序与网络字节顺序间转化的函数：

    - htons:本地字节顺序->网络字节顺序，16bits
    - ntohs:网络字节顺序->本地字节顺序，16bits
    - htonl:本地字节顺序->网络字节顺序，32bits
    - ntohl:网络字节顺序->本地字节顺序，32bits

6. 客户端软件的设计。

   需要解决

    - 解析服务器的IP地址。客户端可能使用域名或IP地址标识服务器，而IP协议需要使用32位二进制IP地址，因此需要将域名或IP地址转化32位IP地址。
      - inet_addr()实现点分十进制IP地址到32位IP地址转换
      - gethostbyname()实现域名到32位IP地址转换
    - 解析服务器端口号。客户端可能使用服务名（HTTP）标识服务器端口，需要将服务名转换端口。
      - getservbyname()实现转换。
    - 解析协议号。客户端可能使用协议名指定协议，如TCP，需要将协议名转为协议号，如6.
      - getprotobyname()实现转换。

7. TCP客户端软件基本流程

    - 确定服务器IP与端口
    - 创建socket
    - 分配本地端口地址（系统完成）。
    - 连接服务器socket
    - 遵循应用层协议进行通信。
    - 关闭/释放连接

8. UDP客户端软件流程

    - 确定服务器IP与端口
    - 创建socket
    - 分配本地端口地址（系统完成）
    - 指定服务器端点地址，构造UDP数据报
    - 遵循应用层协议进行通信
    - 关闭socket

9. 服务器软件设计

   有4种基本类型：

    - 循环无连接：一次只处理一个客户的请求，结束后处理下一个。一般为UDP连接。流程：
      - 创建socket。
      - 绑定端点地址。作为服务器绑定IP地址时不要绑定具体IP地址，万一接入多个网络有多个IP地址呢，因此采用INADDR_ANY+端口
      - 反复接收来自客户端的请求。
      - 遵循应用层协议，构造响应报文并返回。这里发送数据使用sendto()。接收数据时recvfrom()将自动获取客户端点地址，用于后续服务器给客户端发送数据时使用。
    - 循环面向连接
      - 创建（主）socket。
      - 设置（主）socket为被动监听模式，用于服务器
      - 调用accept()接收下一个连接请求（通过主socket），创建新socket用于与客户建立连接。
      - 遵循应用层协议，反复接收客户请求，构造并发送响应。
      - 完成特定客户后关闭连接返回第三步。
    - 并发无连接
      - 主线程1：创建socket并绑定端口
      - 主线程2：反复调用recvfrom()，接收下一个客户请求并创建新线程处理该客户的响应。
      - 子线程1：接收一个特定请求，并依据应用层协议构造响应报文，并调用sendto()发送，结束后退出。
    - 并发面向连接
      - 主线程1：创建socket并绑定端口
      - 主线程2：设置（主）socket为被动监听模式，用于服务器
      - 主线程3：反复调用accept()接收下一个连接请求，并创建一个新的子线程处理该客户响应。
      - 子线程1：接收一个特定请求（新socket），并依据应用层协议构造响应报文，并调用sendto()发送，结束后线程终止。

  
  
### 传输层
传输层协议为运行在不同主机上的进程提供逻辑通信机制。与网络层负责提供主机之间的逻辑通信机制不同。

- 发送方：将应用层递交的消息分成segment段，传给网络层
- 接收方：将接收到的segment组装成消息，上交应用层。

#### 多路复用分用
如果某层的一个协议对应直接上层的多个协议/实体，则需要复用/分用。如图，host1与host3的socket发送的数据在host2的相同层中进行复用（都要经过这些层），在传输层进行分用（要将segment交给正确的socket）。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路复用.PNG" width=50% height=50% />

1. 发送端进行多路复用：从多个socket接收数据，为每块数据封装头部信息，生成segment，交给网络层。

2. 接收端进行多路分用：传输层依据头部信息将收到的segment交给正确的socket，即不同的进程。工作流程：主机收到IP数据报datagram(携带源IP与端口、目的IP与端口、segment、)；然后主机根据datagram中的信息，利用传输层协议提取IP与端口信息，将segment导向正确socket。

    - 无连接分用：UDP的socket用`（目的IP，目的端口）`标识。故若目的一致，不同源IP+port的IP数据包导向同一socket
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路分用UDP.PNG" width=50% height=50% />
    - 面向连接的分用：TCP的socket用`(源IP，源端口，目的Ip，目的端口)`标识。
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路分用TCP.PNG" width=50% height=50% />


#### 无连接传输协议

- 基于IP协议实现复用/分用，简单的错误校验。
- 因为IP层本身是提供"Best effort"，UDP没有进一步扩展，因此也是"Best effort"。
- 无连接。UDP发送方与接收方不需要握手，因此减少延迟。

1. 存在价值:

    - 无需连接
    - 实现简单，无需维护状态
    - 头部开销小
    - 没用拥塞控制，应用可更好的控制发送时间与速率。TCP内部会调整发送速率，不完全由应用控制。

2. 校验和：具体实现不讲，用一种方式表现出发送前的正确报文段，表示的结果就是校验和。然后发送到接收端后，接收端将现在的报文段计算校验和，2者相等，一般就无错。

#### 可靠数据传输机制
1. 可靠数据传输协议RDT，如下图，可以发现`rdt_send()`、`deliver_data()`是单向，发送方上层应用只负责将数据交给RDT，接收方RDT只负责将完整后的数据发送给上层应用（注意，传输层与应用层交互的还是完整的数据，没有分段）。这一切如何实现？靠RDT的版本升级。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之可靠数据RDT.PNG" width=60% height=60% />

2. rdt1.0是假设使用最可靠的通道情况。主要有传输端与接收端两个部分，资料传输方式很单纯，传输端等待上层传资料进来，收到上面的资料以后装成封包送出去。接收端收到封包以后，将封包解开，把讯息往上送。

3. RDT2.0,假设只产生位错误的信道，即底层信道可能翻转分组中的位bit。利用校验和检测位错误，恢复过程：

    - 确认机制ACK(acknowledgements),接收方显式的告诉发送方分组已正确接收。
    - NAK，接收方显式告诉发送方分组有错误。
    - 发送方收到NAK后重传分组。 

    相比于1.0，RDT2.0引入：

      - 差错检测。
      - 接收方反馈控制消息ACK/NAK
      - 重传。基于重传的RDT称为ARQ（automatic repeat request）

4. RDT2.1。如果ACK/NAK消息发生错误/被破坏。如果坏掉，发送方直接重传，但不能简单重传，容易产生重复分组。解决：发送方给每个分组增加序列号，这样接收方就可以丢弃重复分组。与2.0的变化：

    - 发送方
      - 为每个分组增加序列号。
      - 两个序列号(0,1)就够用，因为使用停等协议。如果不能理解，查看RDT3.0的过程。
      - 需校验ACK/NAK消息是否错误
      - 状态数量翻倍，状态必须记住当前的分组序列号
    - 接收方
      - 需判断分组是否重复，当前所处状态提供了期望收到分组的序列号
      - 另外，接收方无法知道ACK/NAK发送是否被正确接收。有一种情况是：发送方正确发送0，接收方也正确接收0但返回的ACK坏了，于是发送方由于没ACK只能继续发送0，而导致这时接收方期望1却收到0，但接收方应该返回ACK，这样发送方才会继续发送1.

>停等协议：发送方发送一个包，等待接收方回应。

5. RDT2.2:无NAK协议。接收方通过ACk告知最后一个被正确接收分组，在ACk中显示加入被确认分组的序列号。

6. RDT3.0:信道可能发生错误，可能丢失分组。加定时器。3.0的处理如下图：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之RDT3.PNG" width=60% height=60% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之RDT31.PNG" width=60% height=60% />


#### 滑动窗口协议
由于RDT3.0性能太低，基于停等协议的原因，现在采用流水线机制。

1. 滑动窗口协议概念：GBN，SR。窗口，即允许使用的序列号范围，窗口尺寸N表示最多N个等待确认的消息。滑动窗口：随着协议的运行，窗口在序列号空间内向前滑动。如下图，send_base下标黄色表示已经发送但还未确认的消息，nextseqnum下标蓝色表示还未使用的，当被完全使用后必须等所有确认了才能发送下一组N（其实只要收到ACK就进行滑动，但是虽然下一个N可以发送但由于当前N还有未确认的，因此依然拒收，只有当前N全部确认才处理下一个N）。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动窗口概念.PNG" width=45% height=45% />

2. GBN协议，Go-Back-N

    - 发送方
      - 分组头部包含k-bit序列号，窗口尺寸为N
      - ACK(n),确认到序列号n（包含n）的分组均已正确接收。为空中的分组设计计时器。
      - 超时事件：如果第n个分组超时，重传序列号>=n且未收到ACK的所有分组。因此可能造成资源浪费。
      - 发送过程：初始状态`send_base=1`,`nextseqNum=1`。
         - 如果`nextseqNum<send_base+N`,说明窗口还没有用光，可继续发分组。发送分组后判断如果`nextseqNum=send_base`，启动定时器，也就是说在一个新的窗口从该窗口第一个分组发出的时候启动定时器。
         - 如果窗口序列号已经用光，拒绝发送数据。当发生timeout时重启定时器，将未确认的重新发一遍。当收到确认消息的时候，提取确认消息的序列号，更新send_base进行滑动。当`nextseqNum=send_base`时分组全部发送确认成功，停止计时器，否则重启计时器。
         - 问题：这里收到分组确认消息一定是按序列号顺序发过来的吗
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动GBN.PNG" width=55% height=55% />
    - 接收方
      - ACK机制：发送拥有最高序列号的，已被正确接收的分组的ACK.
      - 乱序到达的分组：例如等待5号但7号缺先到了，直接丢弃。并重新确认序列号最大的，按序到达的分组。因此这里其实是确认4，也就是1-4确认收到。
      - 接收过程：维护expectedSeqnum,如果正确接收，更新expectedSeqnum的值。这样就解决了上面发送方的问题。
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动GBN接收.PNG" width=63% height=63% />
    - GBN示例，N=4：
      - 发送方：发01，2丢失，发3，45等待。当01确定了，45可以发（滑动了），但接收方丢弃（因为期望是2）。直到超时，重发2345（将发送但未确认的重发）.
      - 接收方：初始expectedSeqnum=0。期待0，收到0后expectedSeqnum更新为1，收到1后expectedSeqnum更新为2。收到3，因此丢弃并发ACK1，表明01确认，记住，敢发就敢表明之前的已确定。直到收到2，expectedSeqnum更新为3.开始接收3.
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动窗口GBN示例.PNG" width=42% height=42% />

3. SR协议，selective　Repeat。接收方对每个分组单独进行确认（设置缓存机制，缓存乱序到达的分组），发送方只重传那些没收到ACK的分组（为每个分组设置定时器）。过程如下：

    - 发送方：发送玩0123后等待，其中2丢失。收到ACK01后滑动，可以发送45.2的超时发生，重发2.已经收到了ACK345，当收到ACK2后就可以滑动。
    - 接收方：收到01后发送ACK01，滑动，2丢失因此没收到，收到3，缓存并发送ACK3，收到45，缓存发ACK45。收到2后发ACK，且满了就一起交付上层，窗口可以滑动。
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之SR过程.PNG" width=75% height=75% />
    - 困境：如下图。若采用n比特对帧编号，为了保证接收方向向前移动窗口后，新窗口序号与旧窗口序号没有重叠部分，需要满足条件：Nr+Ns<=2^n。单个最大就是2^(n-1)。
    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动SR.PNG" width=50% height=50% />

>如果序列号是2位比特表示，就是00，01，10，11，就是有0123这四种序列号可用。





>流水线协议：允许发送方在收到ACK之前连续发送多个分组。需要更大的序列号范围，发送方或接收方需要更大的存储空间缓存分组。





#### tcp面向连接的传输协议
1. 点对点，流水线机制，发送/接收方缓存。

2. TCP段结构：

    - 序列号。segment中第一个字节的编号，而不是segment的编号（比如1k的数据分成2个segment，则该序列号一般是500）。建立TCP连接时双方随机选择序列号，这里序列号只用于建立连接3次握手时使用，传输数据可不是这样随机的。
    - ACKs是希望接收到的下一个字节的序列号。也采用累计确认的方式：该序列号**之前**的所有字节均正确接收到。对乱序到达的segment，这里不做处理，由实现者做出决策。
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP段结构.PNG" width=58% height=58% />
    - 过程：由telnet来模拟，A输入"C"后B收到后会回传。首先A发出序列号42的段，期望收到的段是79，这两个数字是随机
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP段结构过程.PNG" width=35% height=35% />

3. TCP可靠数据传输

    - 发送方。创建segment，开启计时器，设置超时时间。处理情况：
      - 超时，重传引起超时的segment，重启定时器
      - 收到ACK。如果确认收到此前未确认的segment，更新sendBase（累计确认因此靠谱），如果此时窗口还有未确认的分组，重启定时器。
    - 接收方，主要是ACK的生成。
      - 顺序到达的段，延迟等待500ms。在等待时间内，如果没next的段，将发送ACK；如果有段来，立刻发送累积段的序列号。
      - 乱序到达的段，肯定有间隙，重复发送某ACK表示缺该段。
    - 过程，主要是第三个例子注意。发送端发送了一个92序列号8字节的段后下一个发送的肯定是序列号为100的段，因此虽然没收到ACK100，但是发送100序列号后收到了ACK120，更新sendBase=120。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP发送端流程.PNG" width=80% height=80% />
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP段结构过程3.PNG" width=38% height=38% />

>快速重传机制：在定时器超时之前进行重传。TCP的实现中，如果超时，超时时间间隔将重新设置，将超时时间间隔加倍，导致变大，重发丢失分组之前要等待很久。现在通过重复ACK检测分组丢失：如果发送方收到同一数据的3个ACK，则该数据之后的段已经丢失。

>TCP不同处在于使用单一重传定时器。关于定时器的超时时间设置。测量多个SampleRTT并求平均值形成RTT估计值。当然具体还需要加安全边界才是具体设置的值。

>TCP流量控制，接收方通过在segment的头部字段将RcvWindow告诉发送方，发送方限制自己已经发送但还未收到ACK的数据不超过RcvWindow尺寸。

4. TCP连接管理https://blog.csdn.net/qzcsu/article/details/72861891

   3次握手：

      - client要发起建立连接的请求，发送TCP的SYN段到server，包含初始化的序列号，无数据。
      - server收到SYN后，分配缓存作为建立该TCP连接所占用的资源，响应SYNACK段(SYNACK=client发送的序列号+1，累计确认，注意是**之前**)，包含初始化的序列号表示期望的序列号段。
      - client收到SYNACK，发送ACK段(累计确认)，可能包含数据。

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP的3次握手.PNG" width=49% height=49% />


    >TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。

    >为什么TCP客户端最后还要发送一次确认呢:主要防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误。例子：如果客户端发送了请求A超时但没丢失，又发请求B，服务器跟请求B建立连接后关闭。现在A到达服务器，两次握手的机制将会让客户端和服务器再次建立连接。如果是3次握手，服务端接受到了那条失效报文并且回复了确认报文，但客户端不会回应，因此无效。

    4次挥手：

      - client向server发送FIN(跟SYN一样，在TCP段结构图中标识关闭连接)。
      - server收到FIN后发送ACK表示已收到关闭请求。接着发送FIN到client。
      - client收到FIN后，发送ACK并进入等待状态：如果收到FIN会重新发送ACK以确保服务器收到ACK。
      - server收到ACK，关闭连接。

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP的4次挥手.PNG" width=40% height=40% />


    client与server的状态图：

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP的客户机状态转化.PNG" width=60% height=60% />

> TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。




#### 拥塞控制原理
1. 拥塞，发送方太多发送了太多数据或发送速度太快导致**网络**无法处理（不是接收方处理不了）。表现为：分组丢失（路由器缓存溢出），分组延迟过大（分组延迟因素前面说的4个因素，这里主要是路由器缓存中排队）

2. 场景一，vin为原始发送速率，vout为接收方接收速率，带宽为C，有2个发送方，因此每个发送速率vin最大为C/2。由于路由器的速度限制，即使无限缓存，到达的最大速度也无法超过路由器的速度，所以时延在拥塞发生的时候会无限增大。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景1.PNG" width=50% height=50% />

3. 场景二，有丢失。vin'表示原始发送数据+重传的数据。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景2.PNG" width=50% height=50% />

4. 场景三，A与C之间2个TCP连接，A发C，C发A。路由器有限缓存。有4个路由器。假如A传C的红线上A到第一个路由器已成功转发，但在第二个路由器发生了丢失，造成浪费：当分组被丢失时，任何用于该分组的“上游”传输能力全都被浪费掉，相当于白传了，浪费了资源和传输能力

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景3.PNG" width=50% height=50% />

#### 拥塞控制算法
1. 方法一，端到端拥塞控制。端系统通过观察丢失、延迟等网络行为判断是否发生拥塞。TCP采用这种。

   动态设置CongWin，最后发送的字节序列号-最后确认的字节序列号<=CongWin。这样限定了已发送但未确认的量，因此rate大致CongWin/RTT。如何合理调整rate，使用了2种机制。

    - AIMD，加性增-乘性减。逐渐增加发送速率直到发生loss， 
    - SS，慢启动，初始速率很小，采用指数性增长。

    TCP拥塞控制算法如下图。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法图解.PNG" width=50% height=50% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法.PNG" width=50% height=50% />


2. 方法二，网络辅助拥塞控制。指示发送方应该采取何种速率。ATM采用这种。

ABR，available bit rate。如果发送方发现整个网络负载比较低，使用可用带宽，如果发现拥塞，使用最低保障速率。

RM，resource management，交换机设置RM cell的位：NI bit,速率不许增长；CI bit告诉发送方现在已经拥塞。RM cell接收方返回给发送方。 
  




## 网络层
完成功能，即从发送主机向接收主机传送数据段segment。

- 发送主机：将segment封装到datagram
- 接收主机：向传输层交付segment

### 核心功能转发与路由

- 转发：forwarding,将分组从路由器的输入端口转移到合适的输出端口。每个路由器都需要维护一个转发表。
- 路由：routing，确定分组从源到目的经过的路径。每个路由器上运行路由算法，这些算法所确定的路由信息将记录在转发表中，当路由器收到具体的datagram时，根据datagram中的地址信息检索转发表，完成转发。下图为转发表。
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层转发表1.PNG" width=50% height=50% />
- 连接建立：某些网络才需要这个功能ATM、X.25、帧中继。数据分组传输之前2端主机需要首先建立逻辑连接，然后该连接路径上进行传输。与传输层的连接不同：
  - 网络层连接，2个主机之间。路径上的路由器等网络设备参与其中。
  - 传输层连接，2个应用进程之间，中间网络设备透明。

### 网络层服务模型

- 无连接服务。不事先为系列分组的传输确定传输路径。每个分组独立确定传输路径，故不同分组可能路径不同。典型：数据报网络如Internet。
- 连接服务。首先建立连接，为分组的传输确定从源到目的的路径。然后沿着该路径传输分组。典型：虚电路网络。

#### 虚电路网络
1. 虚电路：一条从源主机到目的主机的类似于电路的路径（逻辑连接）。与电路交换不同在于分组交换以及每个分组的传输利用链路的全部带宽。每条虚电路包括：

    - 从源主机到目的主机的一条路径
    - 虚电路号VCID，沿路每段链路一个编号。
    - 沿路每个网络层设备（如路由器），利用转发表记录经过的每条虚电路。

    同一条VC，在每段链路上的VCID通常不同，因此路由器转发分组时依据转发表改写VCID。下图虚电路转发表的建立：
    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层虚电路转发表2.PNG" width=60% height=60% />
2. 通信过程：

    - 呼叫建立(call setup)
    - 数据传输，沿某条虚电路传输的分组，携带对应虚电路的标识(VCID),而不是目的主机地址。
    - 拆除呼叫

3. 虚电路信令协议，用于支持虚电路VC的建立维护拆除。需要路由来选择路径。下图为建立虚电路过程。
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层虚电路建立3.PNG" width=60% height=60% />

#### 数据报网络
1. 属于无连接服务。如果传输中更新了路由器的转发表，会使得分组路径不同。然而目的主机的IP地址范围太大，无法维护这么大的转发表，因此选择将分组表的目的地址改为地址范围，相当于聚合转发表入口。下图是转发表结构：
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层数据报网络转发表5.PNG" width=50% height=50% />
   
   下图聚合方式为最长前缀匹配优先：
   
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层数据报网络4.PNG" width=60% height=60% />

2. 这两种网络的应用对比：

    - Internet：跨越不同网络时链路类型众多，统一服务困难。没有严格时间需求。智能端。简化网络复杂边缘。
    - ATM：电话网络演化而来，核心功能是实时对话，有严格的保障服务。非智能。简化边缘复杂网络。


### internet的网络层
下图为internet网络层主要功能，路由表供IP协议在转发处理分组的时候使用。

#### ipv4协议
1. IP数据报格式，下图所示：

    - 版本号ipv4就是占4位，ipv6就是6位。
    - 首部长度占4位，表示IP分组的首部长度。协议规定，以4字节为单位。例如首部长度值为5，对应的首部长度字节数为5*4=20
    - 服务类型TOS字段占8位。指示期望获得哪种类型的服务。但一般情况不使用该字段，通常值为00H（16进制）
    - 总长度，IP分组的总字节数（首部+数据）。单位是1字节.
    - TTL，存活时间。IP分组在网络中可以通过的路由器数（跳步数），路由器转发一次分组TTL-1，如果TTL=0则丢弃分组同时向源主机发送ICMP报文。
    - 协议，指示IP分组封装的是哪个协议的数据包。6为TCP，17为UDP
    - 校验和，逐跳计算，逐跳校验。
    - 标识ID，标识一个IP分组。利用计数器，每产生一个IP分组＋１。
    - 标志位，占3位。第一位保留，第二位DF，don't fragment,DF=1禁止分片，DF=0允许分片；第三位MF,more fragment，MF=1，非最后一片，MF=0最后一片。
    - 片偏移，一个IP分组分片封装IP分组数据的相对偏移量。ipv4以8字节为单位。

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP协议IP数据报结构6.PNG" width=65% height=65% />

2. IP分片

    IP分片过程:如果IP分组总长度L，MTU为M。如果L>M,DF=0,则可以分片。分片时每个分片的标识复制原IP分组的标识。由于片偏移单位为8字节，因此除最后一个分片，其他分片都是8的倍数字节(MTU允许的最大分片),故：

      - 一个最大分片可以封装的数据为`d=[(M-20字节的IP头)/8]*8`,注：[]向下取整。
      - 需要的总片数为`n=[(L-20)/d]`.注：[]向上取整。
      - 每片的片偏移字段取值`Fi=d/8*(i-1),i=[1,n]`。
      - 每片的总长度字段为`Li=d+20,i=[1,n);Li=L-(n-1)d,i=n`。这里最后一片一定要自己计算。
      - 每片MF标志位为：`MF=1,i=[1,n);MF=0,i=n`

  可以计算下图中的IP分片数据：

  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP分片示例7.PNG" width=60% height=60% />
  
>MTU，最大传输单元，网络链路存在MTU，即链路层数据帧可封装数据的上限。当大IP分组向较小MTU链路转发时，可以被分片，只有IP分片到达目的主机后才进行重组。

3. IP编址

   接口：主机/路由器与物理链路的连接。如果这些接口实现了网络层功能，才进行编址。

   IP地址：32比特(ipv4),编号标识主机路由器的接口。从网络层的角度来看，通过这些IP地址唯一标识这些接口。将IP地址分为网络号NetID（高比特位）和主机号HostID（低比特位）。

   IP子网：IP地址具有相同NetID的设备接口。这些子网中的设备不跨越路由器(第三及以上网络设备)就可以彼此物理联通。

4. 有类编址

    - A类地址第一字节的最高位固定为0
    - B类地址第一字节的高两位为10，故1000000以128开头
    - C类地址的第一字节的高三位固定为110，故11000000以192开头
    - D类地址第一字节的高四位固定为1110。组播地址
    - E类地址的一字节的最高四位固定为1111
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP有类编址8.PNG" width=80% height=80% />

5. 子网划分：如何区分一个IP子网更小范围网络。现在将IP地址分为NetID，SubID子网号，HostID。

    - 方式：借用主机中的某些比特来进行划分。
    - 如何确定是否划分子网。利用子网掩码。
    - 利用多少位划分子网。


>子网掩码的值：NetID、SubID位全部取1，HostID全取0。因此A默认子网掩码255.0.0.0依次类推。

   例如：子网201.2.3.0，255.255.255.0。将其划分为4个等长子网。如下图：
   
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP子网掩码9.PNG" width=50% height=50% />

   子网掩码的应用：将IP分组的目的IP地址与子网掩码**按位与**运算，提取子网地址。示例如下：
   
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP子网掩码的应用10.PNG" width=50% height=50% />

>可分配地址范围：去掉主机全为0与全为1的。

6. CIDR

   有类编址存在分配不均等问题，提出新的技术CIDR，classless interDomian Routing，无类域间路由。特点是：

      - 消除ABC地址界限：NetID+SubID->Network Prefis，其中前缀可以任意长度。
      - 融合子网地址与子网掩码：无类地址格式：a.b.c.d/x,x为前缀长度。例如：`子网201.2.3.64，255.255.255.192->201.2.3.64/26`
      - 提高路由效率，将多个子网聚合为一个大子网，路由聚集。 注意：在聚合时判断聚合子网是否缺少某一子网，如果缺少，在同一路由器里面有2个入口，通过他的前缀长度进行区分。
      
        <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层IP路由聚合11.PNG" width=50% height=50% />

#### dhcp协议
DHCP，Dynamic Host Configuration Protocol，动态主机配置协议。特点：

- 从服务器动态获取：IP地址，子网掩码，默认网关地址(同一路由器接口)，DNS服务器名称与IP地址。
- 即插即用
- 允许地址重用，续租
- 支持移动用户加入网络。

过程如下图：

- 主机广播DHCP discover（发现报文）其中255.255.255.255.67是受限广播地址。67是服务器应用进程端口，68是客户端应用进程端口，yiaddr(your ip address)
- DHCP服务器利用DHCP offer（提供报文）进行响应
- 主机请求IP地址DHCP　request（请求报文）
- DHCP服务器分配IP地址DHCP ack（确认报文）

  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层DHCP过程12.PNG" width=66% height=66% />

#### nat网络地址转换
目的：

- 一个网络只能/只需从ISP申请一个IP地址
- 本地网络设备IP地址的变更，无需通告外界网络
- 变更ISP接入网络时无需修改内部网络设备IP地址
- 内部网络设备对外界网络不可见，即不可直接寻址。

NAT实现：

- 替换：利用`(NAT IP，新port)`替换每个外出IP数据报的`(源Ip，源port)`
- 记录：将每对`(NAT IP,新port)`与`(源Ip，源port)`的替换信息存储到NAT转换表中。
- 替换：根据NAT转换表，利用`(源Ip，源port)`替换每个进入内网IP数据报的`(目的IP，目的port)`也就是`(NAT IP，新port)`

NAT穿透问题：当外部客户端期望连接内网地址为10.0.0.1的服务器，客户是不能直接利用地址10.0.0.1进行访问的，对外唯一可见的地址是NAT地址138.76.29.7。    
解决方案：中继(如Skype)

- NAT内部客户与中继服务器建立连接
- 外部客户与中继服务器建立连接
- 中继服务器桥接2个连接的分组


#### icmp协议
互联网控制报文协议ICMP，Internet Control Message Protocol，支持主机或者路由器。

两类ICMP报文：

- 差错报告报文
  - 目的不可达。当数据报到达某地址后无法到达目的主机被丢弃时同时发送ICMP报文给源主机
  - 源抑制。当一个路由器发现缓存已满导致IP数据报被丢弃，构造该ICMP向源主机发送，降低其发送速度。
  - 超时/超期。TTL=0时丢弃向源主机发送ICMP。
  - 参数问题。如果路由器发现Ip数据报头部某些数据有问题可以丢弃。
  - 重定向。路由器收到IP数据报，该IP数据报所要到达的目的网络，在该路由器看来不应该由自己发送，于是向源主机发送ICMP。
- 网络探询
  - 回声Echo请求与应答报文Reply。例如Ping，ping的话，由于不涉及数据传输，因此不涉及TCP或者UDP。
  - 时间戳请求与应答。 
  
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层ICMP13.PNG" width=50% height=50% />

不发送ICMP的情况：

- 对ICMP差错报文不再发送ICMP
- 除了第一个IP数据报分片外，对所有后续分片均不发送ICMP差错报文
- 对所有多播IP数据报均不发送
- 对特殊地址的IP数据报不发送

ICMP路径探测应用：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层ICMP应用14.PNG" width=60% height=60% />



#### ipv6
动机：32位IPv4地址空间分配殆尽。改进首部格式。128位。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层ipv6结构.PNG" width=50% height=50% />
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层ipv6结构2后14.PNG" width=50% height=50% />
IPv6数据格式特点：

- 固定长度的40字节基本首部。
- 不允许分片。
- 校验和移除，减少每跳处理时间
- ICMPv6，新版ICMP，附加报文类型

IPv6地址表示形式：

- 一般形式：`1080:0:FF:0:8:800:200C:417A`
- 压缩形式：`FF01:0:0:0:0:0:0:43`压缩后：`FF01::43`。::表示多个0且只能使用一次
- IPv4嵌入形式，将前80比特为0，接下来16比特为1，剩下32就是IPV4的。最终为`0:0:0:0:0:FFFF:13.1.68.3`。或者`::FFFF:13.1.68.3`
- 地址前缀：`2002:43c:476b::/48`。IPv6不再使用掩码。
- URLs:`http://[ipv6]:8000`

ipv6基本地址类型：

- 单播地址：一对一通信
- 多播：一对多通信 
- 任意播

隧道：Ipv6数据报作为IPv4数据报的载荷进行封装，穿越IPv4网络。

### 路由算法
整个网络就是一个图，图中的边有权重信息。路由算法关键：确定源到目的的最小权重的路径。路由算法分类：

- 静态路由：手工配置，路由更新慢，优先级高。
- 动态路由：路由更新快，定期更新，及时响应链路权重。
- 全局信息：所有路由器掌握完整网络拓扑与链路权重。例如链路状态LS路由算法。
- 分散信息：路由器只掌握物理相连的邻居以及链路权重。通过邻居路由信息交换，然后找到最佳路径。例如距离向量DV路由算法。

#### 链路状态路由算法
Dijkstra算法

- 通过链路状态广播，所有节点路由器掌握网络拓扑和链路权重。
- 然后计算从一个节点到所有其他节点的最短路径。
- k次迭代后得到到达k个目的节点的最短路径。

过程如下图所示：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层路由算法DJ20.PNG" width=50% height=50% />

跟普通的DJ算法不同，这里求的是u到每一个节点的最短路径，用于获取u的最终转发表：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层路由算法DJ转发21.PNG" width=45% height=45% />

>`c(x,y)`表示节点xy的链路权重，不直接相连就是无穷大。

>`D(v)`从源u到目的v的当前路径权重值。

>`p(v)`沿源到v的当前路径，v的前一个节点。

>`N'`已经找到最小权重的节点集合。

#### 距离向量路由算法
Bellman-Ford动态规划

- 每个节点处于等待状态，等待本地局部链路权重变化或者受到邻居DV更新
- 然后重新计算DV
- 如果CV中到达任一目的距离发生改变，通告所有邻居。
>`dx(y)=min{c(x,v)+dv(y)}`是x到y的最短路径权重。其中v是x的邻居。

#### 层次路由
网络规模太大，路由表无法存储。因此引出聚合路由器为一个区域：自治系统AS。同一AS内的路由器运行相同的路由协议。

自治系统间Inter-AS路由任务。假设AS1内某路由器受到一个目的地址在AS1之外的数据报，路由器应该将数据报转发哪个网关路由器？因此AS1必须：知道哪些目的网络可以通过AS2到达，哪些可以通过AS3到达。将这些网络可达性信息传播给AS1内部路由器。


### internet路由
采用层次路由。AS内部路由协议也称内部网络协议IGP，interior gateway protocol.最常见的AS内部路由协议：RIP路由信息协议，OSPF开放最短路径优先，IGRP内部网关路由协议。

1. AS内部路由协议
  - RIP协议，routing information protocol。利用距离向量路由算法。RIP路由表利用route-d的应用层进程进行管理，通告报文周期性通告UDP数据报发送。
    > 每隔30s，邻居之间交换一次DV，称为通告。

  - OSPF协议，open shortest path first。开放：公众可用。采用链路状态路由算法。OSPF通告在整个AS范围洪泛。类似IS-IS路由协议。与RIP相比：
	   - 安全，所有OSPF报文可以被认证。
	   - 允许使用多条相同权重的路径。分散，负载均衡。
	   - 最重要的是支持对大规模AS分层。链路状态通告只限于区内，每个路由器掌握所在区的详细拓扑。

2. 自治系统AS间的路由协议
   - BGP协议，Border Gateway Protocol。标准域间路由协议。BGP为每个AS提供了一种手段：eBGP从邻居AS获取子网可达性信息。iBGP向所有AS内部路由器传播子网可达性信息。

  
  
 
 
  
## 数据链路层
术语：

- 节点nodes：主机路由器统称。
- 链路links：连接相邻节点的通信信道。包括有线、无线链路、局域网
- 帧frame：链路层数据分组，封装网络层数据报

链路层服务：

- 组帧：
  - 封装数据报构成数据帧，加首部尾部
  - 帧同步：帧在物理层传输时是一系列的比特，物理层如何从比特流识别出一个一个数据帧。因此组帧时需要解决这个帧同步问题。一般首尾加特殊比特串。
- 链路接入：
  - 如果是共享介质（该链路有很多节点，都要通过这个链路发送），需要解决信道接入
  - 帧首部中MAC地址，用于标识帧的源和目的。
- 相邻节点间可靠交付：
  - 一些误码率低的链路，如光纤，双绞线，很少采用。
- 流量控制：协调相邻发送节点和接收
- 差错检测
- 差错纠正，有的可以有
- 全双工、半双工。

### 差错检测

#### 差错编码
1. 基本原理：在数据基础上增加冗余信息，即D->DR,其中R为差错检测与冗余比特，R=f(D)。
2. 差错编码的检错能力：
   - 检错码，如果编码集的汉明距离ds=r+1,则该差错编码可以检错r位的差错。
   - 纠错码,如果编码集的汉明距离ds=2r+1,则该差错编码可以纠正r位的差错。

    >在信息论中，两个等长字符串之间的汉明距离是两个字符串对应位置的不同字符的个数。
3. 常见差错编码：
	  - 奇偶校验码：1比特校验位，检测奇数位差错。编码效率高。
	  - Internet校验和：[计算过程，感兴趣可以自己查](https://www.cnblogs.com/ZCplayground/p/8440352.html)
	  - 循环冗余校验码CRC：检错能力强。

### 链路接入之多路访问控制mac协议
多路访问控制MAC协议，采用分布式算法决定节点如何共享信道，即决策节点何时可以传输数据。必须基于信道本身来协调。理想MAC协议：给定速率为R bps的广播信道，期望当只有一个节点希望传输数据时可以以R发送，当有M节点发送数据，每个节点平均发送数据速率为R/M.
    
MAC协议分类：

- 信道划分MAC协议，多路复用技术：TDMA时间、FDMA频带、CDMA码片、WDMA
  - 网络负载重，共享信道效率高，公平
  - 网络负载轻，共享信道效率低
- 随机访问MAC协议：信道不划分，允许冲突。网络负载轻时，共享信道效率高。网络负载重时，产生冲突开销。采用冲突恢复机制。典型：
  - 时隙ALOHA协议：假定所有帧大小相同，时间划分为等长时隙，节点只能在时隙开始时刻发生帧。如下图，节点123都通过该某一链路，同一时隙发送冲突C，都不发送空闲E。最大效率37%
    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/10链路层时隙ALOHA1.PNG" width=50% height=50% />
  - ALOHA：无需同步，有新帧生成时，立即发送。导致冲突也增大。最大效率18%
  - CSMA协议，载波监听多路访问协议。发送帧之前，监听信道载波，信道空闲时发送完整帧，信道忙推迟发送。
  - CSMA/CD:带有冲突检测的CSMA协议，有线局域网易于实现，无线则很难。冲突：边发边听，不发不听。两个计算公式：
      - `Lmin/R=2dmax/V`
      - `Lmin/R=RTTmax`,其中网络带宽R bps，数据帧最小长度Lmin(bits)信号传播速度V（m/s）。注意：1Gbps=10^9bps,与bit对应。1B则是8bit。
- 轮转MAC协议：节点轮流使用信道。综合上诉2种的优点。
  - 轮询polling。主节点轮流邀请从属节点发送数据。
  - 令牌传递token passing，控制令牌依次从节点传递到下一个节点，令牌就是特殊帧。


## 局域网

### arp协议

#### mac地址
32位ip地址作为网络层地址的接口，而MAC地址（也称LAN地址，物理地址，以太网地址）。

   - 作用：用于局域网内标识一个帧从哪个接口发出，到达哪个物理相连的其他接口。
   - 48位MAC地址固化在网卡的ROM中。现在某些软件也可以修改。每个数字为16进制。
   - MAC地址由IEEE统一管理分配。

#### arp协议
1. ARP地址解析协议。在同一个LAN内，如何在已知目的接口的IP地址前提下确定其MAC地址？通过ARP表。
2. ARP表：LAN中每个IP节点（主机和路由器）维护一个表：存储某些LAN节点的IP/MAC地址映射关系<IP,MAC,TTL>,经过TTL后该映射关系将被丢弃。ARP即插即用协议，节点自主创建ARP表，无需干预。
3. A想给B发送数据报。
   - 如果在LAN中的，B的MAC地址不在A的ARP表中：
	   - A广播ARP查询分组，其中包含：B的ip地址；目的MAC地址FF-FF-FF-FF-FF-FF。LAN所有节点都接受ARP查询。
	   - B接受ARP查询分组，IP地址匹配成功，利用单播帧向A应答B的MAC地址。 
	   - A在其ARP表中缓存B的IP-MAC地址对，直至超时。
   - 如果AB不在同一LAN中，通信过程：A通过路由器R向B发送数据报。A已知B的IP地址，且A的默认网关就是路由器R左侧的IP地址，于是A也就已知了R的左侧MAC地址。
       - 首先A构造IP数据报：srcIP=A，desIP=B,A构造链路层帧：srcMAC=A的MAC，desMAC=R左侧MAC
       - R接收A的数据报后，转发：srcIP、desIP不变，R构造链路层帧：srcMAC=R右侧MAC，desMAC=B的MAC。
         <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网ARP1.PNG" width=75% height=75% />

### 以太网ethernet
1. 以太简述：便宜，满足网络速率10Mbps-10Gbps。早期物理拓扑是总线，所有节点在同一冲突域。目前流行的是网络拓扑结构星型，每个节点一个单独冲突域。以太网在数据链路层提供不可靠无连接服务：
  - 无连接：发送帧的网卡与接收帧的网卡间没有握手过程。
  - 不可靠：接收网卡不向发送网卡进行确认。差错帧直接丢弃，丢弃帧如果想恢复要靠高层的TCP协议进行重发，否则数据丢失。

2. 采用MAC协议：二进制指数退避算法的CSMA/CD。 

3. 以太网帧结构
   发送端网卡将ip数据报封装到以太网帧中，如下图：
   - 前导码preamble，8B，前7个字节10101010，第8个字节10101011。用于发送网卡与接收网卡的时钟同步。
   - 目的MAC、源MAC，各6B。如果网卡的MAC与收到帧的目的MAC匹配，或者目的MAC为广播地址，则网卡接收该帧。否则网卡丢弃该帧。
   - Type类型，2B。指示帧中封装的是哪种高层协议的分组。ip数据报还是AppleTalk数据报啥的。相当于实现以太网的复用分用。
   - Data数据，46-1500B，上层协议载荷。
   - CRC，4B。循环冗余校验码，丢弃差错帧。
   
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网以太网结构2.PNG" width=60% height=60% />

   >802.3以太网标准，不同以太网标准中，具有相同的MAC协议和帧格式，不同速率以及不同物理介质（光纤、线缆）。物理链路为铜缆（双绞线）：100BASE-TX、100BASESE-T2、100BASESE-T4；而物理链路为光纤时：100BASE-SX、100BASE-FX、100BASE-BX。

#### 交换机
1. 以太网交换机，链路层设备，负责：
   - 存储/转发以太网帧
   - 检验到达的帧的目的MAC，选择性向一个或多个输出链路转发帧。
   - 利用CSMA/CD访问链路，发送帧。

    >透明交换机：主机感知不到交换机的存在。即插即用，自学习，不需配置。

2. 多端口间同时传输，如下图：
   - 主机利用独享链路直接连接交换机
   - 交换机缓存帧
   - 交换机在每段链路利用CSMA/CD收发帧，但无冲突，且可以全双工。 
   - 交换：AA'与BB'的传输可以同时进行没有冲突。至于MAC地址A传MAC地址A'时，是如何知道通过交换机时经过接口4到达的？交换机转发表。
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网以太网交换机3.PNG" width=45% height=45% />

3. 交换表。每个交换机有一个交换表，每一个入口entry存储主机的MAC地址，到达主机的接口，时间戳（时间到达后清除记录）。与路由器不同在于，交换机自学习获取到达主机的接口信息：
	- 当收到帧时，交换机学习到发送帧的主机位于收到该帧的接口所连接的LAN网段。
	- 将发送主机MAC与接口信息记录到交换表。将来如果要将数据发给A的话，通过1这个接口就可以了。
	- 利用目的MAC地址检索交换表：
	  - 如果检索到,选择性转发：if目的主机位于收到帧的网段，then丢弃帧，因为这样目的主机可以直接收；else，不在同一网段，将帧转发到该entry接口
	  - 如果没有检索到,泛洪：向除收到该帧的接口之外的所有接口转发。

#### 虚拟局域网vlan
1. 支持VLAN划分的交换机，可以在一个物理LAN架构上配置定义多个VLAN：
   - 基于端口的VLAN。
     - 将物理交换机划分为多个VLAN，针对某一组端口的广播只在该VLAN中进行广播。
     - 流量隔离：经历某端口1-8的帧只到达端口1-8.
     - 动态成员：端口可以动态分配给不同VLAN。
   - 基于MAC地址定义VLAN。
   
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网以太网交换机VLAN跨越4.PNG" width=65% height=65% />

2. 跨域多交换机的VLAN。
   中继端口trunk port：在跨越多个物理交换机定义的VLAN承载帧。需要携带VLAN ID信息。下图是802.1Q VLAN帧格式。
   
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网以太网交换机VLAN跨越8021q5.PNG" width=65% height=65% />

### ppp协议
1. 点对点数据链路控制协议。一个发送端，一个接收端，一条链路，无需介质访问控制MAC，无需明确MAC地址。例如拨号链路，ISDN链路。常见协议：HDLC，PPP。
2. PPP设计需求：
   - 组帧，可以承载任何网络协议分组，不止IP数据报
   - 比特透明传输：数据域必须支持承载任何比特模式。问题：如何判断一个标志比特流是数据还是标志，为此，采用字节填充技术，就是加缓冲字节。
   - 差错检测，无纠正。
   - 连接活性检测：检测、并向网络层通知链路失效。
   - 网络层地址协商，端节点可以学习/配置彼此网络地址。

3. PPP数据帧
   - 标志Flag：定界符，数据帧的开始或结束
   - 地址Address，无效，仅仅是一个选项
   - 控制Control：无效，未来的、
   - 协议：上层协议，例如IP、PPP-LCP等
   - 信息：上层协议分组数据
   - 校验：CRC校验，用于差错检测
   
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/11局域网PPP协议8.PNG" width=65% height=65% />

4. 在交换网络层数据之前，PPP数据链路两端必须：
   - 配置PPP链路：最大帧长度、身份认证
   - 学习/配置网络层信息：例如IP协议，通过交换IPCP(IP control protocol)报文(IP分组首部的上层协议字段=8021)，完成IP地址等相关信息配置。

### wifi即80211无线局域网
1. 在链路层均采用CSMA/CA多路访问控制协议，均支持两种网络模式：基站和自组网。
  - 早期802.11b版本，2.4-2.5GHz免费频段，最高速率11Mbps,物理层采用直接序列扩频技术DSSS：通过给每个用户分配唯一的码片(信道划分MAC协议)序列，每个用户在发送数据的时候利用码片序列对数据进行编码，在保证每个用户使用的码片序列正交的前提下，所有用户彼此不干扰。
    > 基站=访问点AP，主机与某AP关联：被动扫描与主动扫描。
  - 802.11a，5-6GHz频段，最高速率54Mbps
  - 802.11g, 2.4-2.5GHz，最高速率54Mbps
  - 802.11n，2.4-2.5GHz，最高速率699Mbps,多天线MIMO

2. 多路访问控制问题。CSMA/CA。



## 计算机网络安全
## 网络安全
1. 基本属性   
   - 机密性：只有发送方与预定接收方能理解报文内容。
   - 身份认证:发送方与接收方希望确认彼此真实身份。
   - 信息完整性：发送方与接收方希望确保信息未篡改，发生篡改一定会被检测到。
   - 可访问性与可用性：网络服务必须对被授权用户可访问与可用。

2. 基本特征
   - 相对性：只有相对安全
   - 时效性：新的漏洞与攻击方法不断发现
   - 相关性：新配置、新系统组件可能引入新的安全问题
   - 不确定性：攻击时间、目标、地点都不确定
   - 复杂性：需要技术与非技术手段
   - 重要性：

3. 手段：
   - 窃听
   - 插入：主动在连接中插入信息
   - 假冒impersonation：伪造(spoof)分组中的源地址或其他字段
   - 劫持hiJacking：通过移除取代，接管连接
   - 拒绝服务DOS（denial of Service）：阻止服务器为其他用户提供服务，例如通过过载资源

4. 常见威胁：
   - 映射：发起攻击前，case the joint探路，找到网络上运行什么服务。简单做法是：
	   - ping确定网络主机的地址
	   - 接着端口扫描，依次尝试与每个端口建立TCP连接
	   - 对策：记录到达的网络流量，分析识别可疑活动
   - 分组嗅探sniffing:需要接入广播介质(共享式以太网，无线网络)，如果一个主机将其设置为混杂模式网络接口，可以接受所有分组/帧，可以读到所有未加密数据。对策：组织中所有主机都运行软件，用以周期性检测网络接口是否工作在混杂模式。
   - Ip欺骗：直接由应用程序生成原始IP分组，可以设置分组的源IP地址字段为任意值。例如C向A发数据，然而将源IP改成了B的IP，让A以为分组来自B。对策：入口过滤ingress filtering。路由器不转发源IP地址无效的IP分组，例如B的IP地址不属于A在的连接网络。但是不能强制所有网络都执行入口过滤。
   - 分布式拒绝服务攻击DDOS：
     - 选择目标
     - 入侵网络中的主机，植入僵尸程序，构建僵尸网络
     - 控制僵尸主机向目标发送分组
   - 反射式DDOS攻击：
     - 选择目标
     - 入侵网络中的主机，植入僵尸程序，构建僵尸网络
     - 选择反射服务器，借助其他服务器(称反射服务器)，将服务响应反射到被攻击目标，造成被攻击目标的瘫痪。接着僵尸主机就可以向反射服务器发送请求，进行伪装，认为其来自被攻击的目标。
   - SYN攻击：大量发送第一次握手请求，服务器接受后为每个请求分配资源，导致资源不够。对策：不分配资源，只发送第二次握手。收到客户端的第三次握手后才分配资源。服务器收到第三次握手时如何知道已经进行了2次握手呢？服务器生成SYN cookie序列号在第二次握手发过去。如下图：
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/12网络安全SYN攻击.PNG" width=65% height=65% />

### 密码学基础
从这一节开始就没有看了。

## 网络安全协议
### 专用网PN
基于专属的网络设备、链路或协议等建设的专门服务于特定组织机构的网络。

1. 虚拟专用网络VPN，建立在公共网络上的安全通道，实现远程用户、分支机构等与机构总部网络的安全连接，从而构建针对特定组织机构的专用网络。
    >虚拟，安全通道不实际独占公共网络的资源，是一条逻辑的穿过公共网络的安全稳定的隧道。

2. VPN功能：
   - 数据机密性保护
   - 数据完整性认证
   - 数据源身份认证
   - 防重放攻击
   - 访问控制
3. VPN关键技术
   - 隧道技术：通过Internet提供安全的点到点的数据传输安全通道。实质是一种封装，过程：
     - 数据包进入隧道时，由VPN封装成IP数据报，数据安全穿越公共网络。
     - 通过加密和认证以确保安全。
     - 离开隧道后进行解封，数据不再被保护。  
   - 数据加密
   - 身份认证
   - 密钥管理
   - 访问控制
   - 网络管理
    >隧道协议内包含三种协议：乘客协议（封装哪个协议的数据报）、封装协议（遵循什么来进行封装）、承载协议（封装的结果通过什么传输，Internet上就是IP协议）。


4. VPN实现技术之IPsec









## 例题：
### 例题1：
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-例题.PNG" width=65% height=65% />

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-解答.PNG" width=65% height=65% />

### 例题2：传输层之滑动GBN例题
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动GBN例题.PNG" width=70% height=70% />

### 例题3：传输层拥塞总结算法例题
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法例题.PNG" width=70% height=70% />

### 例题4：网络层
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层例题1.PNG" width=70% height=70% />

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层例题2.PNG" width=70% height=70% />

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/9网络层例题3.PNG" width=70% height=70% />



### 例题5：链路层
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/10链路层CSMACD例题.PNG" width=50% height=50% />




