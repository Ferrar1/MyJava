# 计算机网络基础

## 一、计算机网络概述

### 1.1、协议三要素

- 语法：数据与控制信息的结构或格式，包括数据的组织方式、编码方式、信号电平的表示方式等。（用于解读）
- 语义：即需要发出何种控制信息，完成何种动作，以及做出何种应答，以实现数据交换的协调和差错处理。（用来说明通信双方应当怎么做）
- 时序：即事件实现顺序的详细说明，以实现速率匹配和排序。（定义了何时进行通信，先讲什么，后讲什么，讲话的速度等）


### 1.2、计算机网络结构

#### 1.2.1、 网络边缘
即主机（端系统），运行网络应用程序。普通网络用户通过使用各种网络应用实现网络资源的共享，通常这些网络资源位于网络边缘。通信方式：

   - 客户/服务器应用模型；
   - P2P应用模型，没有专用服务器，通信再对等实体之间进行通信（如QQ）


#### 1.2.2、 接入网络
解决的问题是如何将网络边缘接入核心网（边缘路由器）。分类：

   - 住宅接入网络
      - 数字用户线路（DSL），独占。利用已有的电话线连接中心局的DSLAM。
      - 电缆网络。共享。在不同频带（载波）上传输不同频道
   - 机构（学校、企业）接入网络。局域网。以太。 
   - 移动接入网络。通过共享的无线接入网络连接端系统与路由器。通过基站或接入点。如WiFi。

#### 1.2.3、 网络核心
1)、关键功能：路由、转发。解决的基本问题：如何实现数据从源主机通过网络核心送达目的主机。手段就是**数据交换**.

>为何进行数据交换：n台主机相连需要产生（N-1)N/2条链路，数量庞大不可行；因此引入交换设备，实现在主机之间实现动态数据转换，但如果主机数量多要求交换设备的端口需要大，另外如果网络距离大很难连接在一个交换设备上；最终解决方案：将交换设备互联在一起形成交换网络，让主机跟交换网络中的某一个交换设备连接。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-为何进行数据交换.PNG" width=50% height=50% />
 
2)、交换： 实现动态转接（在所有交换设备端口之间建立物理逻辑的连接，动态的将一个端口的数据转到另一个端口）

  - 电路交换。如果多台主机相连产生多条链路，会有共享的路段称中继线，这些如何共享：**多路复用**。<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-电路交换.PNG" width=66% height=66% />
  - 报文交换。一个完整的信息。存储转发。从一个路由转发到另一个路由需要完整的信息才能转发（串行）。而分组转发时每完成一个数据包就能转发一个数据包（并行）。<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-报文交换.PNG" width=66% height=66% />
  - 分组交换。将报文拆分出来一系列小的数据包，再加一个头部信息。需要报文的拆分重组。不用事先分配好。分组传输时延=每组L(bits)的数据包/R(链路传输速率)，注意是bit为单位进行的计算。而分组交换的报文交换时间：`T=M/R+n*L/R`.其中n为路由器数。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-分组交换.PNG" width=50% height=50% />

过程：在分组交换网络中，分组到达路由器，路由器会按照存储转发的方式先收下来，收下来先暂存，确定了在哪一个链路转发后，在这个链路进行转发，如果这个链路正在转发别的分组，那就要排队等着。路由中供排队的缓存都是有限的，如果缓存满了，就会将这个分组丢弃，即数据包的丢失。中间的四种分组延迟：

  - 结点处理延迟：分组到达路由器，路由器要对这个分组进行处理
  - 排队延迟：当分组确定从某一链路传出，但这一链路刚好在传输其他的分组，就要在结点排队等待
  - 传输延迟：当某一链路开始传输分组时，从分组的第一个bit开始到最后一个bit发完，需要的时间称为传输延迟。
  - 传播延迟：当分组从某一结点发出后，到达下一个结点所花的时间。
>扩展：   
>1、端系统通过接入ISP连接到Internet。(Internet Service Provider)，互联网服务提供商。   
>2、链路的时延带宽积：以比特为单位的链路长度。计算：传播时延X带宽(R)。   
>3、吞吐量：在发送端与接收端之间传输数据速率。取决于端到端的路径上的瓶颈链路（R小的端）。


### 1.3、计算机体系结构
#### 1.3.1、分层体系结构基本概念：

- 实体，表示任何可发送或接受信息的硬件或软件进程。
- 协议，控制2个对等实体进行通信的规则的集合，协议是水平的，包括三要素。
- 任一层实体需要使用下层服务，遵循本层协议，实现本层功能，向上层提供服务，服务是垂直的。
- 下层协议的实现对上层的服务用户是不可见的。
- 同系统中的相邻层实体之间通过接口进行交互，通过服务访问点SAP交换原语（请求、响应、指定、确认等），指定请求的特定服务。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构概念.PNG" width=66% height=66% />

#### 1.3.2、OSI参考模型

1)OSI解释的通信过程，如下图所示：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构通信过程.PNG" width=65% height=65% />
- **数据封装**与通信过程，如下图所示：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系结构通信过程数据封装.PNG" width=70% height=70% />

2)物理层：概括的说是实现bit流在物理介质上的传输。 
   - 定义、规范接口特性：主机跟发送设备之间的接口、发送设备跟物理介质之间的接口。
   - 比特的编码：用信号的什么特征来表示比特。
   - 数据率。
   - 比特同步：解决时钟同步问题。
   - 传输模式：按什么方式传输：单工（单向），半双工（可以双向，只能交替进行）、全双工
   
3)数据链路层：解决物理链路直接相连的2个相邻节点之间的数据传输，以帧为数据单元传输。
   - 负责节点与节点的数据传输
   - 组帧：目的在于让接受一端从物理层接受到一系列比特流的时候，能成功切分出来不同的帧。 
   - 物理寻址：在帧头增加发送端或接受端的物理地址标识数据帧的发送端/接收端。
   - 流量控制：匹配发送端与接收端的发送速度与接受速度。
   - 差错控制：检测并重传损坏或丢失帧，并避免重复帧。
   - 访问控制：在任一给定时刻决定哪个设备拥有链路控制权。
   
4)网络层：负责源主机到目的主机数据分组交付。
   - 可能跨越多个网络，因此链路层的物理寻址就不管用了，因此需要逻辑寻址——全局唯一逻辑地址，确保数据分组被送达目的主机，如IP地址。
   - 路由：帮助数据分组选路径。
   
5)传输层：负责源-目的（端到端）进程间的完整报文传输。即，将会话层给的完整报文，分割成数据段，交给网络层进行传输。
   - 报文的分段与重组。
   - SAP寻址。既然是进程到进程之间，因此需要确保将完整报文提交给正确进程，如端口号。
   - 连接控制。端到端的连接建立。
   - 流量控制
   - 差错控制
   
6)会话层：接受表示层，构造会话层的数据单元。注意：不会进行分割。
   - 对话控制。
   - 同步。在数据流中插入同步点。
   
7)表现层：处理2个系统之间交换信息的语法语义问题。 
   - 数据表示转化。将数据转换为独立编码，与主机无关。
   - 加密解密
   - 压缩解压缩。
   
8)应用层：运行的应用。

>数据封装：增加控制信息，包括地址（标识发送端/接收端）、差错检测编码、以及协议控制（实现协议功能的附加信息，如优先级、服务质量、安全控制）
   

#### 1.3.3、TCP/IP分层模型
通信过程如下图所示：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/2分层体系TCP.PNG" width=70% height=70% />

## 二、TCP/IP分层模型

### 2.1、应用层：网络应用
1)、网络体系结构

- CS
- P2P，适用于文件传输等
  - 没有永远在线的服务器
  - 任意端系统/节点之间可以直接通讯
  - 节点间歇性接入网络
  - 节点可能改变IP地址
- 混合结构
  - Napster:文件传输使用P2P，文件搜索使用CS结构 

2)、网络应用进程通信   

- 同一主机运行的进程之间的通信：进程间通信机制，由操作系统提供。
- 不同主机运行的进程通信：
  - 消息（报文）交换，包括客户机进程（发起通信的进程），服务机进程（等待通信请求的进程）

>进程之间通信利用SOCKET发送/接收消息。不同主机上的进程通信，每个进程需要唯一标识符。一台主机会有多个进程，因此需要IP＋端口来作为标识符。具体消息交换的方式依靠应用层协议。

３)、网络应用对传输服务的需求

- 数据丢失/可靠性：有的能容忍一定的数据丢失（网络电话），有的要求100%可靠（文件传输）
- 时间/延迟：有的应用在延迟足够低才有效，网络游戏。
- 带宽：有的应用只有在带宽达到最低要求才有效（网络视频）有的适应任何带宽（email）

#### 2.1.1、web应用

1)内容：

- 万维网：网页，网页互相链接
- 网页包含多个对象：HTML文件，图片等。
- 对象寻址：URL，统一资源定位符。

2)HTTP应用层协议使用的传输层协议是TCP,其过程：   

- 服务器在80端口等待客户请求
- 浏览器发起服务器的TCP连接（创建套接字Socket）
- 服务器接受来自浏览器的TCP连接
- 浏览器跟服务器交换HTTP消息
- 关闭TCP连接。

>http属于无状态协议。就是不记录之前的客户群请求的信息。因为如果突然关闭后重启产生状态不一致。维护代价高。

3)Http连接的2种类型：

- 非持久性连接：当用户在浏览器输入网址www.example.com/index.html 时过程如下图，缺点是慢，为每个TCP连接开销资源。 <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用tcp.png" width=100% height=100% />
- 持久性连接：发送响应后服务器保持TCP连接的打开，后续HTTP消息可以通过这个连接发送，带流水机制的持久性连接是客户端只要遇到一个引用对象就尽快发出请求（所有引用对象一共耗时1个RTT），无流机制的持久性连接是只有前一个收到响应完成请求才发出新的请求（每个对象耗时1个RTT）。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用tcp时间.PNG" width=35% height=35% />

>RTT(round trip time)从客户端发送一个很小的数据包到服务器并返回，所经历的时间。

4)HTTP协议的2类消息

请求消息：ASCII人工可读。其中Host为请求的服务器地址(用于缓存与服务器代理)。User-agent为浏览器版本。Connection发完之后的连接状态。

响应消息：Date是web服务器生成响应消息的时间。Last-modified网页上次修改的时间，不同。

5)cookie技术

由于HTTP协议是无状态协议，因此用cookie记录客户端的信息。cookie组件：

- 保存在客户端主机上的cookie文件，由浏览器管理。
- HTTP响应消息的cookie头部行
- HTTP请求详细的cookie头部行
- web服务器端的后台数据库   
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4web应用cookie.PNG" width=63% height=63% />

6)缓存/代理服务器技术

功能：在不访问服务器的前提下满足客户端的HTTP请求。目的：缩短客户请求的响应时间，减少机构组织的流量，在大范围内实现有效的内容分发（例如CDN）

过程：用户设定浏览器通过缓存进行web访问，作用是浏览器不再向目的服务器发送HTTP请求，而是向缓存/代理服务器发送HTTP请求，如果所请求对象在缓存中，就直接返回，否则缓存服务器向目的服务器发送HTTP请求获取对象，然后返回客户端并保存。
>现在有另外的问题，代理服务器上的缓存是否为最新，解决为**条件性GET方法**:在HTTP请求消息中声明所持有的版本日期if-modified-since。

#### 2.1.2 Email应用
1、构成组件

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

### 2.1.3 DNS服务
1、解决互联网上主机/路由器的识别问题。

2、域名解析系统DNS将域名解析为IP地址。DNS采用分布式层次数据库，如下图:

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS数据库.PNG" width=65% height=65% />

**当客户端查询www.amazon.com时**

- 客户端查询根服务器，找到com域名解析服务器。
- 顶级域名服务器负责com、cn等：客户端查询com域名解析服务器，找到Amazon.com域名解析服务器。该服务器一般被缓存在本地，因此根服务器不经常访问。
- 权威域名服务器：客户端查询Amazon.com域名解析服务器，获得www.amazon.com的IP地址。
- 本地域名服务器：不在上述的层次结构中，当主机进行DNS查询时，查询被发送到本地域名服务器。有２种查询方式：迭代与递归查询.

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS查询之迭代.PNG" width=35% height=35% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/4DNS查询之递归.PNG" width=35% height=35% />

>DNS做负载均衡：对于一个大的门户网站，在DNS服务器中为同一个主机名配置多个IP地址，当进行域名向IP地址的翻译时，将以DNS文件中主机记录的IP地址按顺序返回不同的解析结果，将客户端的访问引导到不同的机器上去

３、DNS记录

资源记录格式（name,value,type,ttl）

- Type=A时，Name：主机域名，Value：IP地址
- Type=NS，Name：域（edu.cn），Value：该域权威域名解析器的主机域名。
- Type=Cname,Name:某一真实域名的别名，value：真实域名。
- Type=MX,Value是与name相对应的邮件服务器。

4、DNS协议与消息

DNS协议：查询和回复协议。

消息头部：

- identification：16位查询编号，回复的时候使用相同的编号。
- flags

5、注册域名

- 向域名管理机构提供你的权威域名解析服务器的名字和IP地址
- 域名管理机构向com顶级域名解析服务器中插入2条记录
  - （example.com,dns1.example.com,NS）
  - (dns1.example.com,212.212.212.1,A)
- 在权威域名解析器中为www.example.com 加入Type=A记录，让自己的主页被外面访问。为example.com加入Type=MX记录可以使用邮件系统。



#### 2.1.4 P2P应用
1、peer-to-peer,没有服务器，热议端系统之间直接通信，节点阶段性接入Internet，节点可能更换IP地址。

2、文件分发：BitTorrent协议。torrent是指交换同一个文件的文件块的节点组（所有参与的节点形成了一个组）。tracker是跟踪参与torrent的节点（跟踪有哪些节点参与到了torrent）。如下。
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

3、索引技术

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

#### 2.1.5 网络编程

1、如下图，表面上是应用进程之间通过应用层协议在控制，其实还是通过操作系统在控制数据交换，因此应用层需要将数据传输到下一层，故需要接口来完成应用进程与底层交互。 该接口称为应用编程接口API。常见的接口有：socket、WinSock、TLI。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket.PNG" width=50% height=50% />
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket类型.PNG" width=50% height=50% />

2、如何标识socket

- 标识通信端点（对外）：IP地址+端口号。
- 在操作系统/进程层面上（对内）：当应用程序创建socket时，操作系统分配一个数据结构存储该socket的相关信息，并返回一个socket descripter。即一个网络应用进程利用socket descripter(套接字描述符，小整数)引用其创建的一个Socket。如下图：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket数据结构.PNG" width=50% height=50% />

>使用TCP/IP协议簇的网络应用程序声明**端点地址**变量时专门定义了数据结构sockaddr_in

3、winSock独有的函数

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/6socket过程.PNG" width=45% height=45% />

- WSAStartUp,初始化winSock的API。进行网络编程的应用程序在使用socket之前必须首先调用该函数。参数：
  - 第一个参数指明程序请求使用的winSock的版本
  - 第二个参数返回实际的winSock版本信息
- WSACleanUp,释放所使用的winSock的DLL。进行网络编程的应用程序在使用完Socket库后最后要调用的函数。

4、各平台基本适用的函数，非WinSock独有

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

5、网络字节顺序

在OSI中表示层处理2个系统之间交换信息的语法语义问题，而TCP环境里面，TCP/IP定义了标准的用于协议头中的二进制整数：网络字节顺序，独立于操作系统的。因此一些socket的API函数的参数需要存储为网络字节参数。因此需要一些可以实现本地字节顺序与网络字节顺序间转化的函数：

- htons:本地字节顺序->网络字节顺序，16bits
- ntohs:网络字节顺序->本地字节顺序，16bits
- htonl:本地字节顺序->网络字节顺序，32bits
- ntohl:网络字节顺序->本地字节顺序，32bits

6、客户端软件的设计。

需要解决

- 解析服务器的IP地址。客户端可能使用域名或IP地址标识服务器，而IP协议需要使用32位二进制IP地址，因此需要将域名或IP地址转化32位IP地址。
  - inet_addr()实现点分十进制IP地址到32位IP地址转换
  - gethostbyname()实现域名到32位IP地址转换
- 解析服务器端口号。客户端可能使用服务名（HTTP）标识服务器端口，需要将服务名转换端口。
  - getservbyname()实现转换。
- 解析协议号。客户端可能使用协议名指定协议，如TCP，需要将协议名转为协议号，如6.
  - getprotobyname()实现转换。

7、TCP客户端软件基本流程

- 确定服务器IP与端口
- 创建socket
- 分配本地端口地址（系统完成）。
- 连接服务器socket
- 遵循应用层协议进行通信。
- 关闭/释放连接

8、UDP客户端软件流程

- 确定服务器IP与端口
- 创建socket
- 分配本地端口地址（系统完成）
- 指定服务器端点地址，构造UDP数据报
- 遵循应用层协议进行通信
- 关闭socket

9、服务器软件设计

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
  
  
  
### 2.2 传输层
传输层协议为运行在不同主机上的进程提供逻辑通信机制。与网络层负责提供主机之间的逻辑通信机制不同。

- 发送方：将应用层递交的消息分成segment段，传给网络层
- 接收方：将接收到的segment组装成消息，上交应用层。

#### 2.2.1、多路复用/分用
如果某层的一个协议对应直接上层的多个协议/实体，则需要复用/分用。如图，host1与host3的socket发送的数据在host2的相同层中进行复用（都要经过这些层），在传输层进行分用（要将segment交给正确的socket）。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路复用.PNG" width=50% height=50% />

1)发送端进行多路复用：从多个socket接收数据，为每块数据封装头部信息，生成segment，交给网络层。

2)接收端进行多路分用：传输层依据头部信息将收到的segment交给正确的socket，即不同的进程。工作流程：主机收到IP数据报datagram(携带源IP与端口、目的IP与端口、segment、)；然后主机根据datagram中的信息，利用传输层协议提取IP与端口信息，将segment导向正确socket。

- 无连接分用：UDP的socket用`（目的IP，目的端口）`标识。故若目的一致，不同源IP+port的IP数据包导向同一socket
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路分用UDP.PNG" width=50% height=50% />
- 面向连接的分用：TCP的socket用`(源IP，源端口，目的Ip，目的端口)`标识。
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之多路分用TCP.PNG" width=50% height=50% />


#### 2.2.2、无连接传输协议

- 基于IP协议实现复用/分用，简单的错误校验。
- 因为IP层本身是提供"Best effort"，UDP没有进一步扩展，因此也是"Best effort"。
- 无连接。UDP发送方与接收方不需要握手，因此减少延迟。

1)存在价值:

- 无需连接
- 实现简单，无需维护状态
- 头部开销小
- 没用拥塞控制，应用可更好的控制发送时间与速率。TCP内部会调整发送速率，不完全由应用控制。

2)校验和：具体实现不讲，用一种方式表现出发送前的正确报文段，表示的结果就是校验和。然后发送到接收端后，接收端将现在的报文段计算校验和，2者相等，一般就无错。

#### 2.2.3、可靠数据传输机制
1)可靠数据传输协议RDT，如下图，可以发现`rdt_send()`、`deliver_data()`是单向，发送方上层应用只负责将数据交给RDT，接收方RDT只负责将完整后的数据发送给上层应用（注意，传输层与应用层交互的还是完整的数据，没有分段）。这一切如何实现？靠RDT的版本升级。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之可靠数据RDT.PNG" width=60% height=60% />

2)rdt1.0是假设使用最可靠的通道情况。主要有传输端与接收端两个部分，资料传输方式很单纯，传输端等待上层传资料进来，收到上面的资料以后装成封包送出去。接收端收到封包以后，将封包解开，把讯息往上送。

3)RDT2.0,假设只产生位错误的信道，即底层信道可能翻转分组中的位bit。利用校验和检测位错误，恢复过程：

- 确认机制ACK(acknowledgements),接收方显式的告诉发送方分组已正确接收。
- NAK，接收方显式告诉发送方分组有错误。
- 发送方收到NAK后重传分组。 

相比于1.0，RDT2.0引入：

- 差错检测。
- 接收方反馈控制消息ACK/NAK
- 重传。基于重传的RDT称为ARQ（automatic repeat request）

4)RDT2.1。如果ACK/NAK消息发生错误/被破坏。如果坏掉，发送方直接重传，但不能简单重传，容易产生重复分组。解决：发送方给每个分组增加序列号，这样接收方就可以丢弃重复分组。与2.0的变化：

- 发送方
  - 为每个分组增加序列号。
  - 两个序列号(0,1)就够用，因为使用停等协议。如果不能理解，查看RDT3.0的过程。
  - 需校验ACK/NAK消息是否错误
  - 状态数量翻倍，状态必须记住当前的分组序列号
- 接收方
  - 需判断分组是否重复，当前所处状态提供了期望收到分组的序列号
  - 另外，接收方无法知道ACK/NAK发送是否被正确接收。有一种情况是：发送方正确发送0，接收方也正确接收0但返回的ACK坏了，于是发送方由于没ACK只能继续发送0，而导致这时接收方期望1却收到0，但接收方应该返回ACK，这样发送方才会继续发送1.

>停等协议：发送方发送一个包，等待接收方回应。

5)RDT2.2:无NAK协议。接收方通过ACk告知最后一个被正确接收分组，在ACk中显示加入被确认分组的序列号。

6)RDT3.0:信道可能发生错误，可能丢失分组。加定时器。3.0的处理如下图：

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之RDT3.PNG" width=60% height=60% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之RDT31.PNG" width=60% height=60% />


#### 2.2.4、滑动窗口协议
由于RDT3.0性能太低，基于停等协议的原因，现在采用流水线机制。

1)滑动窗口协议概念：GBN，SR。窗口，即允许使用的序列号范围，窗口尺寸N表示最多N个等待确认的消息。滑动窗口：随着协议的运行，窗口在序列号空间内向前滑动。如下图，send_base下标黄色表示已经发送但还未确认的消息，nextseqnum下标蓝色表示还未使用的，当被完全使用后必须等所有确认了才能发送下一组N（其实只要收到ACK就进行滑动，但是虽然下一个N可以发送但由于当前N还有未确认的，因此依然拒收，只有当前N全部确认才处理下一个N）。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动窗口概念.PNG" width=45% height=45% />

2)GBN协议，Go-Back-N

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

3)SR协议，selective　Repeat。接收方对每个分组单独进行确认（设置缓存机制，缓存乱序到达的分组），发送方只重传那些没收到ACK的分组（为每个分组设置定时器）。过程如下：

- 发送方：发送玩0123后等待，其中2丢失。收到ACK01后滑动，可以发送45.2的超时发生，重发2.已经收到了ACK345，当收到ACK2后就可以滑动。
- 接收方：收到01后发送ACK01，滑动，2丢失因此没收到，收到3，缓存并发送ACK3，收到45，缓存发ACK45。收到2后发ACK，且满了就一起交付上层，窗口可以滑动。
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之SR过程.PNG" width=75% height=75% />
- 困境：如下图。若采用n比特对帧编号，为了保证接收方向向前移动窗口后，新窗口序号与旧窗口序号没有重叠部分，需要满足条件：Nr+Ns<=2^n。单个最大就是2^(n-1)。
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动SR.PNG" width=50% height=50% />

>如果序列号是2位比特表示，就是00，01，10，11，就是有0123这四种序列号可用。





>流水线协议：允许发送方在收到ACK之前连续发送多个分组。需要更大的序列号范围，发送方或接收方需要更大的存储空间缓存分组。





#### 2.2.5、TCP面向连接的传输协议
1)点对点，流水线机制，发送/接收方缓存。

2)TCP段结构：

- 序列号。segment中第一个字节的编号，而不是segment的编号（比如1k的数据分成2个segment，则该序列号一般是500）。建立TCP连接时双方随机选择序列号，这里序列号只用于建立连接3次握手时使用，传输数据可不是这样随机的。
- ACKs是希望接收到的下一个字节的序列号。也采用累计确认的方式：该序列号**之前**的所有字节均正确接收到。对乱序到达的segment，这里不做处理，由实现者做出决策。
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP段结构.PNG" width=58% height=58% />
- 过程：由telnet来模拟，A输入"C"后B收到后会回传。首先A发出序列号42的段，期望收到的段是79，这两个数字是随机
  <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层TCP段结构过程.PNG" width=35% height=35% />

3)TCP可靠数据传输

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

4)TCP连接管理https://blog.csdn.net/qzcsu/article/details/72861891

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




#### 2.2.6、拥塞控制原理
1)拥塞，发送方太多发送了太多数据或发送速度太快导致**网络**无法处理（不是接收方处理不了）。表现为：分组丢失（路由器缓存溢出），分组延迟过大（分组延迟因素前面说的4个因素，这里主要是路由器缓存中排队）

2)场景一，vin为原始发送速率，vout为接收方接收速率，带宽为C，有2个发送方，因此每个发送速率vin最大为C/2。由于路由器的速度限制，即使无限缓存，到达的最大速度也无法超过路由器的速度，所以时延在拥塞发生的时候会无限增大。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景1.PNG" width=50% height=50% />

3)场景二，有丢失。vin'表示原始发送数据+重传的数据。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景2.PNG" width=50% height=50% />

4)场景三，A与C之间2个TCP连接，A发C，C发A。路由器有限缓存。有4个路由器。假如A传C的红线上A到第一个路由器已成功转发，但在第二个路由器发生了丢失，造成浪费：当分组被丢失时，任何用于该分组的“上游”传输能力全都被浪费掉，相当于白传了，浪费了资源和传输能力

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞场景3.PNG" width=50% height=50% />

#### 2.2.7、拥塞控制算法
1)方法一，端到端拥塞控制。端系统通过观察丢失、延迟等网络行为判断是否发生拥塞。TCP采用这种。

动态设置CongWin，最后发送的字节序列号-最后确认的字节序列号<=CongWin。这样限定了已发送但未确认的量，因此rate大致CongWin/RTT。如何合理调整rate，使用了2种机制。

- AIMD，加性增-乘性减。逐渐增加发送速率直到发生loss， 
- SS，慢启动，初始速率很小，采用指数性增长。

TCP拥塞控制算法如下图。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法图解.PNG" width=50% height=50% /><img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法.PNG" width=50% height=50% />


2)方法二，网络辅助拥塞控制。指示发送方应该采取何种速率。ATM采用这种。

ABR，available bit rate。如果发送方发现整个网络负载比较低，使用可用带宽，如果发现拥塞，使用最低保障速率。

RM，resource management，交换机设置RM cell的位：NI bit,速率不许增长；CI bit告诉发送方现在已经拥塞。RM cell接收方返回给发送方。 
  
  

  
  
  
  
  
  








## 例题：
### 例题1：
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-例题.PNG" width=70% height=70% />

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/1网络核心-解答.PNG" width=70% height=70% />

### 例题2：传输层之滑动GBN例题
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/7传输层之滑动GBN例题.PNG" width=70% height=70% />

### 例题3：传输层拥塞总结算法例题
<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/networkOfComputer/8传输层拥塞总结算法例题.PNG" width=70% height=70% />






