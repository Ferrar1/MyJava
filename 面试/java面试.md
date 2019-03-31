- [计算机网络](#计算机网络)
- [数据库](#数据库)
   - [mysql索引](#mysql索引)
- [操作系统](#操作系统)
- [spring](#spring)
- [集合框架](#集合框架)
- [JVM](#jvm)
- [IO](#io)
- [Nginx](#nginx)
- [Redis](#redis) 

# 计算机网络
## 浏览器从输入网址到显示都发生了些什么？
1. 输入网址www.baidu.com 。 如果没有指明是https默认http协议。
2. 浏览器查找域名的IP地址。即应用层DNS解析获取IP地址
   -  浏览器缓存：浏览器会在一定时间内缓存DNS记录（2min-30min不等）
   -  系统缓存：操作系统也会缓存一定的DNS记录。浏览器缓存找不到的时候会查找系统里的缓存记录。一般在hosts文件
   -  本地域名服务器ISP DNS：查询被发送到本地域名服务器，如果依然解析不了，将作为代理，进行迭代或者递归查询：
       - 客户端查询根服务器，找到com域名解析服务器。
       - 客户端查询com顶级域名服务器，找到baidu.com权威域名服务器。
       - 客户端查询baidu.com权威域名服务器，获得www.baidu.com的IP地址。
3. 根据IP地址，向web服务器发送HTTP请求request。
4. 传输层首先需要建立TCP连接，采用三次握手确认建立一个连接。然后才能将应用层发送的报文分成段segment，交给网络层。
5. 网络层将segment封装到datagram。利用ARP协议将IP地址解析为MAC地址。然后路由器利用路由表中的目的网络的下一步路径信息进行转发，在网络中传递。
6. 链路层，将根据MAC地址将数据进行传输。这时，客户端发送请求的过程的阶段才结束。
7. 服务端接收数据
8. 服务端处理请求
9. 服务端返回一个HTTP响应请求
10. 浏览器开始渲染拿到的HTML文件

## 转发(Forward)和重定向(Redirect)的区别
1. 重定向的执行过程：Web服务器向浏览器发送一个http响应->浏览器接受此响应后再发送一个新的http请求到服务器->服务器根据此请求寻找资源并发送给浏览器。它可以重定向到任意URL，不能共享request范围内的数据。
2. 重定向是在客户端发挥作用.
3. 通过浏览器重新请求地址，在地址栏中可以显示转向后的地址。
4. 转发过程：Web服务器调用内部方法在容器内部完成请求和转发动作->将目标资源发送给浏览器，它只能在同一个Web应用中使用，可以共享request范围内的数据。
5. 转发是在服务器端发挥作用，通过forward()方法将提交信息在多个页面间进行传递.
6. 转发是在服务器内部控制权的转移，客户端浏览器的地址栏不会显示出转向后的地址

## HTTP请求,响应报文格式
- HTTP请求报文主要由请求行、请求头部、请求正文3部分组成
- HTTP响应报文主要由状态行、响应头部、响应正文3部分组成
- https://blog.csdn.net/a19881029/article/details/14002273


# 数据库
## mysql索引

### b加树简介
1. 什么是B+树：[B+大致认识](https://blog.csdn.net/qq_26222859/article/details/80631121)
    >节点的度：一个节点含有的子树的个数称为该节点的度；
    >树的度：一棵树中，最大的节点的度称为树的度
2. [B+树插入过程](http://www.cnblogs.com/yangecnu/p/Introduce-B-Tree-and-B-Plus-Tree.html)如下图1：
    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/1-B+树插入过程.gif" width=130% height=130% />
3. 一般在数据库系统或文件系统中使用的B+Tree结构都在经典B+Tree的基础上进行了优化，增加了顺序访问指针。做法：在B+Tree的每个叶子节点增加一个指向相邻叶子节点的指针，就形成了带有顺序访问指针的B+Tree。做这个优化的目的是为了提高区间访问的性能，例如如果要查询key为从18到49的所有数据记录，当找到18后，只需顺着节点和指针顺序遍历就可以一次性访问到所有数据节点，极大提到了区间查询效率。如下图2：

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/2-B+树优化后.png" width=60% height=60% />
### mysQL索引实现
[来源](http://blog.codinglabs.org/articles/theory-of-mysql-index.html)
#### MyISAM索引实现
1. 非聚集索引，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。如下图3，左边是索引文件，右边是数据文件，一行代表一个数据。MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。

    <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/3-MyISAM索引实现.png" width=55% height=55% />
2. 假设我们以Col1为主键，则上图是一个MyISAM表的主索引（Primary key）示意，因为通过主键建立的索引，这样主键对应的地址唯一。但如果想建立辅助索引，如在Col2建立索引，因为不是主键所以Col2可能重复。
#### InnoDB索引实现
1. 聚集索引，在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。如下图4。一行数据直接在叶子节点里面，因此InnoDB要求表必须有主键。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/4-InnoDB.png" width=55% height=55% />
2. 如果建立辅助索引，如下图5，Col3作索引，InnoDB的所有辅助索引都引用主键作为data域。辅助索引搜索需要检索两遍索引：首先检索辅助索引获得主键，然后用主键到主索引中检索获得记录。

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/5-InnoDB辅助索引.png" width=55% height=55% />

#### 回答
1. 索引是如何提高查询速度的？使用了B+树，根据索引的Key值建立B+树，将无序的数据变成相对有序的数据。
2. 例如如果要查询key为从18到49的所有数据记录，根据B+树的特性，当找到18后，只需顺着节点和指针顺序遍历就可以一次性访问到所有数据节点，极大提到了区间查询效率。
3. 使用一个与业务无关的自增字段作为主键。理由：如果表使用自增主键，那么每次插入新的记录，记录就会顺序添加到当前索引节点的后续位置，当一页写满，就会自动开辟一个新的页；如果使用非自增主键（如果身份证号或学号等），由于每次插入主键的值近似于随机，因此每次新纪录都要被插到现有索引页得中间某个位置。
4. 不建议使用过长的字段作为主键：因为所有辅助索引都引用主索引，过长的主索引会令辅助索引变得过大。自增字段作为主键则是一个很好的选择。
5. 覆盖索引：我们知道在InnoDB存储引擎中，如果不是主键索引，叶子节点存储的是主键+列值。最终还是要“回表”。覆盖索引就是select的数据列只用从索引中就能够取得，不必从数据表中读取。
6. MyISAM索引实现不支持事务，用于只读程序提高性能，而InnoDB：支持ACID事务、行级锁、并发。 

# 操作系统
## 进程与线程
1. 概念：进程是操作系统的概念，每当我们执行一个程序时，对于操作系统来讲就创建了一个进程,在这个过程中，伴随着资源的分配和释放。而线程是进程中的一部分，进程包含多个线程在运行。
2. [进程间的几种通信方式](https://zhuanlan.zhihu.com/p/37891272)说一下？
   - 管道（pipe）。半双工。只能用于具有亲缘关系的进程之间的通信。而命名管道（FIFO），允许无亲缘关系进程间的通信。若要数据流从父进程流向子进程，则关闭父进程的读端（fd[0]）与子进程的写端（fd[1]）；反之，则可以使数据流从子进程流向父进程。如下图6：
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/6-管道.png" width=72% height=72% />
   - 消息队列。在两个不相关进程间传递数据的一种方式，她独立于发送进程、接受进程而存在。由消息组成的链表，存放在内核中,并由消息队列标识符标识。有足够的权限的线程可以往队列中放置消息，有足够读权限的线程可以从队列中取走消息。每个消息都是一个记录，它由发送者赋予一个优先级。
      - 管道是文件，存放在磁盘上，访问速度慢
      - 匿名管道是跟随进程的，消息队列是跟随内核的，也就是说进程结束之后，匿名管道就死了，但是消息队列还会存在（除非显示调用函数销毁）
      - 管道是数据流式存取，消息队列是数据块式存取.
   - 共享内存。共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问，共享内存是最快的IPC方式。如信号量配合使用，来实现进程间的同步和通信
   - 信号量。信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它通常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源：
      - 系统首先要检测该资源的信号量
      - 若该资源的信号量值大于0，则进程可以使用该资源，此时，进程将该资源的信号量值减1；
      - 若该资源的信号量值为0，则进程进入休眠状态，直到信号量值大于0时进程被唤醒，访问该资源；
      - 当进程不再使用由一个信号量控制的共享资源时，该信号量值增加1，如果此时有进程处于休眠状态等待此信号量，则该进程会被唤醒。
   - 套接字。套接字通信更侧重于不同主机间的进程通信。
   - 信号。信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生

    >临界资源：只能被一个进程同时使用（不可以多个进程共享），要用到互斥。

3. 线程间的几种通信方式
   - 锁机制
   - 信号量机制
   - 信号机制
    > 线程间通信的主要目的是用于线程同步，所以线程没有象进程通信中用于数据交换的通信机制

# spring
## springmvc流程
1. 客户端发送请求，直接请求到DispatcherServlet。

2. DispatcherServlet 根据请求信息调用 HandlerMapping，解析请求对应的 Handler：这里首先通过请求获取一个处理链

		HandlerExecutionChain mappedHandler = getHandler(processedRequest); 
     
    这个mappedHandler就是对Handler的一个封装，里面包括Handler属性、以及拦截器。

3. 解析到对应的 Handler（也就是我们平常说的 Controller 控制器）后，开始由 HandlerAdapter 适配器处理：**处理器适配器的出现，是为了处理不同类型的Handler，这个类型指的是处理请求映射的方式**。有的是通过在类的上面加注解@Controller来进行映射处理。有的handler是通过实现Controller接口。（具体使用的时候就根据自己在配置文件中选择的适配器来选择配置相应的Handler）。源码中doDispatch方法：
   
    	HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
   getHandlerAdapter函数参数为handler，内部foreach对所有适配器进行遍历，每一个适配器都有support方法，输入handler是否是某一类型的实例，来判断是否支持某一类适配器。如SimpleControllerHandlerAdapter这个适配器return (handler instanceof Controller);说明适配器都是写好了的，需要新的bean加载方式就新建新的适配器。另外，HandlerAdapter这个接口有3个方法，主要是里面的handle方法。适配器的作用就是针对不同类型的handler，通过这个handle方法来执行控制器的具体逻辑。   
  例如：    
  
    - 对于这种AbstractHandlerMethodAdapter，handle重写的内容是   
   
    		return handleInternal(request, response, (HandlerMethod) handler);    
    - 而SimpleControllerHandlerAdapter，重写内容   

     		return ((Controller) handler).handleRequest(request, response);
 
   这就说明，适配器适配的是不同类型的handler。对应于不同处理请求映射的方式。
4. HandlerAdapter 会根据 Handler 来调用真正的处理器开处理请求，并处理相应的业务逻辑：具体逻辑就在handle方法中。
5. 处理器处理完业务后，会返回一个 ModelAndView 对象，Model 是返回的数据对象，View 是个逻辑上的 View。
6. ViewResolver 会根据逻辑 View 查找实际的 View。
7. DispaterServlet 把返回的 Model 传给 View（视图渲染）。   
   这个model就是一个map,将里面的值一个一个赋值给request。
8. 把 View 返回给请求者（浏览器）

>适配器模式在使用的时候，除了处理事件的handle方法，还需要一个support方法来，用以遍历循环，进行适配找到合适的适配器处理。

## spring的aop与ioc

## spring事务
1. [结论](http://blog.itpub.net/69900354/viewspace-2565243/)：
   - 如果是编译时异常不会自动回滚，如果是运行时异常，那会自动回滚！
   - 如果本类没有事务的方法调用有事务的方法，没有事务发生。下面实例经过测试（tmall_springboot项目中可以看到），在UserService里面有下面2个方法添加用户：
      
			@Transactional
			public void add(User user) {
				userDAO.save(user);
				int a = 10/0;//加事务后，模拟异常后回滚。
			}
			
			public void Add(User user){
				this.add(user);
			}
       在ForeRESTController的register方法调用`userService.Add(user);`,结果是虽然报错，但数据成功插入，也就是没有事务执行；但是调用`userService.add(user);`，结果是报错且数据没有插入，也就是出现事务。
    - 事务方法调用另一个事务方法，不会发生事务嵌套。[来源](https://www.jianshu.com/p/0da29e4f354a)
    - 上面2种情况解决方案：可以通过在方法内部获得代理对象的方式，通过代理对象调用同类的其他方法。如果配置@EnableAspectJAutoProxy(exposeProxy = true)，则将`this.add(user)`改为`((UserService) AopContext.currentProxy()).add(user);`另外，在tmall_springboot中SpringContextUtil这个工具类也可以直接使用，缓存与事务这种通过AOP来拿到代理对象的都会遇到这个问题。
			
2. Spring事务传播机制
   在当前含有事务方法内部调用其他的方法(无论该方法是否含有事务)，属于Spring事务传播机制的范畴.嵌套事务：嵌套是子事务套在父事务中执行，子事务是父事务的一部分，在进入子事务之前，父事务建立一个回滚点，叫save point，然后执行子事务，这个子事务的执行也算是父事务的一部分，然后子事务执行结束，父事务继续执行。重点就在于那个save point：
      - 如果子事务回滚，父事务会回滚到进入子事务前建立的save point，然后尝试其他的事务或者其他的业务逻辑，父事务之前的操作不会受到影响，更不会自动回滚。
      - 如果父事务回滚，父事务回滚，子事务也会跟着回滚！为什么呢，因为父事务结束之前，子事务是不会提交的
      - 提交：子事务先提交，父事务再提交。子事务是父事务的一部分
   
3. 一些概念   
	  - 基于接口代理(JDK代理):凡是类的方法非public修饰，或者用了static关键字修饰，那这些方法都不能被Spring AOP增强;
	  - 基于CGLib代理(子类代理):凡是类的方法使用了private、static、final修饰，那这些方法都不能被Spring AOP增强
	  - Spring IOC所管理的对象默认都是单例的，那么在使用的时候解决线程安全问题：ThreadLocal。
	  - 脏读：指一个事务读取了一个未提交事务的数据
	  - 不可重复读：在一个事务内读取表中的某一行数据,多次读取结果不同.一个事务读取到了另一个事务提交后的数据.
	  - 虚读(幻读)：在一个事务内读取了别的事务插入的数据，导致前后读取不一致(insert)
  
4. [为了避免上面出现几种情况在标准SQL规范中定义了4个事务隔离级别](https://www.cnblogs.com/huanongying/p/7021555.html)：
	  - 未授权读取Read Uncommitted，允许脏读取但不允许更新丢失，如果一个事务已经开始写数据则另外一个数据则不允许同时进行写操作但允许其他事务读此行数据。事务隔离的最低级别。
	  - 授权读取Read Committed，允许不可重复读取但不允许脏读取。SELECT 命令不会返回尚未提交（Committed） 的数据，也不能返回脏数据
	  - 可重复读取Repeatable Read，禁止 不可重复读取和脏读取。读取数据事务将会禁止写事务（但允许读事务），写事务则禁 止任何其他事务。在此隔离级下，用SELECT 命令读取的数据在整个命令执行过程中不会被更改。此选项会影响系统的效能，非必要情况最好不用此隔离级。
	  - 串行Serializable，提供严格的事务隔离，它要求事务序列化执行，事务只能一个接着一个地执行，但不能并发执行。事务隔离的最高级别，事务之间完全隔离

5. [事务传播机制](https://blog.csdn.net/tiandao321/article/details/75388959)
    - PROPAGATION_REQUIRED，加入当前正要执行的事务不在另外一个事务里，那么就起一个新的事务
    - PROPAGATION_SUPPORTS，如果当前在事务中，即以事务的形式运行，如果当前不再一个事务中，那么就以非事务的形式运行
    - PROPAGATION_MANDATORY，必须在一个事务中运行。也就是说，他只能被一个父事务调用。否则，他就要抛出异常
    - PROPAGATION_REQUIRES_NEW 执行当前新建事务完成以后，上下文事务恢复再执行
    - PROPAGATION_NOT_SUPPORTED当前不支持事务
    - PROPAGATION_NEVER不能在事务中运行
    - PROPAGATION_NESTED


# 集合框架
[来源](https://segmentfault.com/a/1190000014240704)。
## list
1. 特点：有序，存储顺序与取出顺序一致。

### ArrayList
1. 结构图7：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/7ArrayList.png" width=80% height=80% />
2. 实现了List接口，可以List基本操作；实现了RandomAccess接口，作为一个标识，表示可以快速随机访问元素。
1. 底层数组，线程不安全。将数据存在Object[] elementData中。
2. `add(E e)`方法：
   - 检查一下数组的容量是否足够：size+1即当前增加元素所需要的容量minCapacity，如果是增加的第一个元素，设置minCapacity为默认10。
   - 所需容量minCapacity与当前容量elementData.length比较，如果数组长度不够，扩容1.5倍。如果还小，扩充为minCapacity。返回`Arrays.copyOf(elementData, newCapacity);`而Arrays.copyOf底层是System.arraycopy。
3. `remove(int index)`方法，跟`add(int index,E element)`一样，底层元素移动都是System.arraycopy实现。删除元素时不会减少容量，若减少容量则调用trimToSize()

### LinkedList
1. 结构图8：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/8LinkedList.png" width=80% height=80% />
2. 实现了List接口，可以List基本操作；实现了Deque接口，可以操作队列。没有实现RandomAccess接口，不可快速随机访问元素。
3. 底层双向链表，线程不安全。注意：双向链表不是循环链表，只能说节点有前指针与后指针。
4. `add(E e)`方法，往链表最后添加元素。
5. `get(int index)`方法：如果下标小于长度的一半就从头遍历，否则从尾遍历。set方法类似。

### Vector
1. 底层数组，线程安全。扩容是直接扩一倍。

### 总结
1. 查询多用ArrayList，增删多用LinkedList
2. 实现了RandomAccess接口的list，优先选择普通for循环
3. 未实现RandomAccess接口的ist， 优先选择iterator遍历
4. [ArrayList与LinkedList的对比](https://github.com/Snailclimb/JavaGuide/blob/master/%E9%9D%A2%E8%AF%95%E5%BF%85%E5%A4%87/%E7%BE%8E%E5%9B%A2-%E8%BF%9B%E9%98%B6%E7%AF%87.md#%E4%B8%89-%E8%81%8A%E8%81%8A-java-%E4%B8%AD%E7%9A%84%E9%9B%86%E5%90%88%E5%90%A7%EF%BC%81)

## Set
1. 特点：元素不可重复。

### HashSet
1. 底层哈希表，即一个数组，其中元素为链表。

### TreeSet
1. 底层红黑树。

### LinkedHashSet
1. 底层哈希表与链表组成。



## Map
1. [红黑树](http://www.360doc.com/content/18/0904/19/25944647_783893127.shtml)特性：
	- 根节点总是黑色的；
	- 每个叶子节点都是黑色的空节点（NIL节点）；
	- 如果节点是红色的，则它的子节点必须是黑色的（反之不一定）；
	- 从根节点到叶节点或空子节点的每条路径，必须包含相同数目的黑色节点
### HashMap
1. 结构图

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/9Hashmap.png" width=80% height=80% />
2. 底层数组+链表(拉链法)，即散列表
3. `put(K key,V value)`方法。
   - 以key计算哈希值,这里面的key.hashCode()是Object方法，就是任何对象都有一个哈希码hashCode。接着计算`hashCode^hashCode>>>16`，才是最终的哈希值。下面解释这样计算的原因。
     
		    static final int hash(Object key) {
		        int h;
		        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
		    }
    - 接着，执行put操作。散列表容量n,而哈希值范围很广，为了将哈希值缩小范围正好对应在[0,n-1]上做数组下标，采用&运算，这样不管哈希值多大，高于n的二进制位全部为0，计算的结果就在[0,n-1]。现在解释`hashCode^hashCode>>>16`，如果很多哈希码高位差异大而低位相同，直接计算`(n - 1) & hash`的值相同的情况会增加，导致碰撞概率增大。而这样计算后，此时的低位实际上是高位与低位的结合，增加了随机性。
     
			final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
                    //table就是存放实体Node的数组，p就是实体即一对KV
			        Node<K,V>[] tab; Node<K,V> p; int n, i;
                    //散列表为空，初始化散列表,resize方法在初始化以及元素个数大于capacity*load factor时调用。
			        if ((tab = table) == null || (n = tab.length) == 0)
			            n = (tab = resize()).length;
                    //没有冲突，直接添加
			        if ((p = tab[i = (n - 1) & hash]) == null)
			            tab[i] = newNode(hash, key, value, null);
			        else {
                        //出现冲突，即要插入的元素，在散列表中的位置有值，记录
                        //如果插入的hash与key都相同，说明是覆盖
				//如果插入的元素是红黑树结构，调用树的插入
				//如果是链表结构(这里就是拉链法的应用)，在该节点处开始找该节点所在的链表，如果链表中存在相同的hash与key，就是覆盖，记录。如果不存在，在链表后加一个节点，同时，如果插入后发现临界值大于TREEIFY_THRESHOLD，将该链表转成红黑树。这里注意的是，是该哈希值下的链表元素个数如果大于8，才转树。
				//最后，如果有记录，进行覆盖操作。
                        
                    }
            }

## ConCurrentHashMap
1. 特性：
   - JDK1.8底层是散列表+红黑树
   - 支持高并发的访问和更新，线程安全
   - 检索操作不用加锁，get方法非阻塞
   - key和value都不允许为null
    >Hashtable是在每个方法上都加上了Synchronized完成同步，效率低下。ConcurrentHashMap通过在部分加锁和利用CAS算法来实现同步。
    
    
    
# jvm
## 内存区域
1. [运行时数据区](https://mp.weixin.qq.com/s?__biz=MzU4NDQ4MzU5OA==&mid=2247483910&idx=1&sn=246f39051a85fc312577499691fba89f&chksm=fd985467caefdd71f9a7c275952be34484b14f9e092723c19bd4ef557c324169ed084f868bdb#rd)如下图所示：

   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/1jvm.jpg" width=80% height=80% />
2. [HotSpot虚拟机加载对象](https://blog.csdn.net/yangshangwei/article/details/81252722)：
  - 虚拟机遇到一条new 指令时，首先将去检查这个指令的参数是否能在常量池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已被加载、解析和初始化过。如果没有，那必须先执行相应的类加载过程。
  - 在类加载检查通过后，接下来虚拟机将为新生对象分配内存。根据堆中内存是否绝对规整，选择空闲列表或者指针碰撞的方式来分配内存。而Java堆是否规整是由所采用的垃圾收集器是否带有压缩整理功能决定。
   	>Java堆内存绝对规整，指针向空闲空间那边挪动一段与对象大小相等的距离，这种分配方式称为“指针碰撞”（ Bump the Pointer）。例子：使用的垃圾收集器是Serial、ParNew。
   
   	>Java堆内存不规整，已使用的内存和空闲的内存相互交错，虚拟机就必须维护一个列表，记录哪些内存块是可用的，在分配的时候从列表中找到一块足够大的空间划分给对象实例，并更新列表上的记录，这种分配方式称为“空闲列表”（Free List）。例子：使用的垃圾收集器是CMS这类基于快速标记。
   
3. 对象的内存布局:
   - 对象头
     - 第一部分存储对象自身的运行时数据：如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID 、偏向时间戳等
     - 另一部分是类型指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。
   - 实例数据
   - 对齐填充，不是必须的，只起到地址对齐的作用

4. 对象的访问定位，即如何使用对象，通过栈上的reference数据来操作堆上的具体对象。由于reference类型在Java虚拟机规范中只规定了一个指向对象引用。而没有规定这个引用应该通过何种方式去定位、访问堆中的对象的具体位置，它取决于Java虚拟机实现。
   -  使用句柄：在Java堆中划分出一块内存来作为句柄池，reference中存储的就是对象的句柄地址，句柄中包含对象实例数据与类型各自具体地址信息，好处是当reference指向别的对象时，只需改变句柄中的实例数据指针。
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/2jvm句柄.png" width=72% height=72% />
   -  直接指针访问：HotSpot的reference中存储的直接就是对象地址。好处是速度更快，节省一次指针定位时间开销
      <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/3jvm直接指针.png" width=72% height=72% />


## 引用类型
1. [强引用](https://www.cnblogs.com/yw-ah/p/5830458.html)：垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。下面代码中，只有当obj这个引用被释放之后，对象才会被释放掉

		Object obj = new Object();
2. 软引用（SoftReference）：如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。sf是对obj的一个软引用，通过sf.get()方法可以取到这个对象。

		Object obj = new Object();
		SoftReference<Object> sf = new SoftReference<Object>(obj);
		obj = null;
		sf.get();//有时候会返回null
    >可用于实现内存敏感的高速缓存，在内存足够的情况下直接通过软引用取值，无需从繁忙的真实来源查询数据，提升速度；当内存不足时，自动删除这部分缓存数据，从真正的来源查询这些数据。

3. 弱引用（WeakReference）:在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存.

		Object obj = new Object();
		WeakReference<Object> wf = new WeakReference<Object>(obj);
		obj = null;
		wf.get();//有时候会返回null
		wf.isEnQueued();//返回是否被标记为即将回收的垃圾
    >弱引用可用于解决内存泄漏的问题。另外，弱引用主要用于监控对象是否已经被垃圾回收器标记为即将回收的垃圾，可以通过弱引用的isEnQueued方法返回对象是否被垃圾回收器标记。

4. 虚引用（PhantomReference）：垃圾回收时回收，无法通过引用取到对象值。虚引用是每次垃圾回收的时候都会被回收，通过虚引用的get方法永远获取到的数据为null，因此也被成为幽灵引用。

		Object obj = new Object();
		PhantomReference<Object> pf = new PhantomReference<Object>(obj);
		obj=null;
		pf.get();//永远返回null
		pf.isEnQueued();//返回是否从内存中已经删除
    >虚引用主要用于检测对象是否已经从内存中删除。

5. 注意：在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速JVM对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生


### 内存回收
1. 如何判断对象是否死亡
   - 引用计数法，给对象中添加一个引用计数器，每当有一个地方引用它，计数器就加1；当引用失效，计数器就减1；任何时候计数器为0的对象就是不可能再被使用的。缺点：很难解决对象之间相互循环引用的问题，下面的例子就是：
   
			class MyObject{
			    public Object object = null;
			}
		    public class Main {
			    public static void main(String[] args) {
			        MyObject object1 = new MyObject();
			        MyObject object2 = new MyObject();
			 
			        object1.object = object2;
			        object2.object = object1;
			 
			        object1 = null;//设为null就是想回收
			        object2 = null;//但是
			    }
			}
   - 可达性分析算法，通过一系列称为“GC Roots”的对象作为起始点，从这些节点开始向下搜索，搜索走过的路径称为“引用链”，当一个对象到GC Roots没有任何的引用链相连时(从GC Roots到这个对象不可达)时，证明此对象不可用。对于上面的两个对象，虽然彼此引用，但是GC Roots不可达，也会回收。具体GC Roots有：
     - 虚拟机栈中引用的对象，程序中new一个对象，堆上将开辟一块空间，同时将空间的地址作为引用保存到虚拟机栈中，如果对象生命周期结束了，那么引用就会从虚拟机栈中出栈，因此如果在虚拟机栈中有引用，就说明这个对象还是有用的
     - 方法区中静态属性引用的对象
     - 方法区中常量引用的对象
     - 本地方法栈中(Native方法)引用的对象

### 垃圾收集算法
1. 标记-清除算法Mark-Sweep。如下图。缺点是效率低以及标记清除后会产生大量的不连续内存碎片。
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/4jvm垃圾回收标记清除.png" width=50% height=50% />
2. 复制算法：将内存划分为大小相等的两块，每次只使用其中的一块。当这块内存用完了，就将还存活的对象复制到另一块内存上，然后把已使用过的内存空间一次清理掉。不会产生内存碎片，缺点是内存缩小一半。目前流行。
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/5jvm复制算法.png" width=50% height=50% />
3. 标记-整理算法Mark-Compact：让所有存活对象都向一端移动，然后直接清理掉端边界以外的所有内存
   <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/6jvm标记整理.png" width=50% height=50% />
4. 分代收集算法Generational Collection：根据对象的存活周期的不同将内存划分为几块，一般就分为新生代和老年代，根据各个年代的特点采用不同的收集算法。新生代（少量存活，容易产生碎片）用复制算法，老年代（对象存活率高）“标记-清理”算法。
    >HotSpot还多一个持久代：存放静态文件，例如Java类和方法，持久代对GC没有显著的影响。

### 垃圾收集器
[来源](http://www.cnblogs.com/ityouknow/p/5614961.html)

1. 串行收集器Serial，只使用一个线程去回收，即新生代、老年代均串行回收，工作的时候必须暂停其他所有的工作线程。如下图：
   - 参数控制：-XX:+UseSerialGC
     
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/7jvm串行.png" width=50% height=50% />

2. ParNew收集器，新生代并行，老年代串行。
   - 参数控制：-XX:+UseParNewGC  ParNew收集器
   - 参数控制：-XX:ParallelGCThreads 限制线程数量
   
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/8jvmParNew.png" width=50% height=50% />

3. Parallel Scavenge收集器，使用复制算法、并行。
   - 参数控制：-XX:+UseParallelGC  使用Parallel收集器+老年代串行
   - 老年版Parallel Old 收集器:-XX:+UseParallelOldGC 使用Parallel收集器+ 老年代并行
     >Parallel Scavenge收集器关注点是吞吐量（高效率的利用CPU）。CMS等垃圾收集器的关注点更多的是用户线程的停顿时间（提高用户体验）。所谓吞吐量就是CPU中用于运行用户代码的时间与CPU总消耗时间的比值

4. CMS收集器Concurrent Mark Sweep，获取最短回收停顿时间为目标的收集器。运行过程如下图：
   - 初始标记，暂停所有的其他线程，并记录下直接与GC Roots相连的对象，速度很快
   - 并发标记，通过GC Roots Tracing判断对象是否仍在使用中，也就是可达性分析。此时用户线程可能会不断的更新引用域，无法保证实时性。耗时
   - 重新标记，暂停所有的其他线程，为了修正并发标记期间，因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录。
   - 并发清除，开启用户线程，同时GC线程开始对为标记的区域做清扫。耗时
   
     <img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/9jvmCMS.png" width=60% height=60% />
     
    缺点是回收算法“标记-清除”产生大量空间碎片、并发阶段会降低吞吐量。另外参数控制：
   - -XX:+UseConcMarkSweepGC，使用CMS收集器
   - -XX:+UseCMSCompactAtFullCollection， Full GC后，进行一次碎片整理；整理过程是独占的，会引起停顿时间变长
   - -XX:+CMSFullGCsBeforeCompaction  设置进行几次Full GC后，进行一次碎片整理
   - -XX:ParallelCMSThreads  设定CMS的线程数量

以上都是针对下图，而G1不同。

<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/10jvm分区.png" width=70% height=70% />

5. G1收集器

    特点：
    - 空间整合，G1收集器采用标记整理算法，不会产生内存空间碎片。
    - 可预测停顿，G1除了追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定在一个长度为N毫秒的时间片段内，消耗在垃圾收集上的时间不得超过N毫秒。
    - 使用G1收集器时，Java堆的内存布局与其他收集器有很大差别，如下图。

    提供了3种模式回收垃圾
    - young GC，当所有eden region被耗尽无法申请内存时，就会触发一次young gc。执行完一次young gc，活跃对象会被拷贝到survivor region或者晋升到old region中，空闲的region会被放入空闲列表中，等待下次被使用
      - -XX:MaxGCPauseMillis	设置G1收集过程目标时间，默认值200ms
      - -XX:G1NewSizePercent	新生代最小值，默认值5%
      - -XX:G1MaxNewSizePercent	新生代最大值，默认值60%
    - mixed gc，除了回收整个young region，还会回收一部分的old region
      - -XX:InitiatingHeapOccupancyPercent，当老年代大小占整个堆大小百分比达到该阈值时，会触发一次mixed gc
    - full gc
    
    过程：
    - 初始标记，只是标记一下GC Roots能直接关联到的对象
    - 根分区扫描Root Region Scanning，在初始标记暂停结束后，年轻代收集就完成的对象复制到Survivor的工作。这时为了保证标记算法的正确性，所有新复制到Survivor分区的对象，都需要被扫描并标记成根
    - 并发标记Concurrent Marking，若发现区域对象中的所有对象都是垃圾，那个这个区域会被立即回收
    - 再标记，再标记阶段是用来收集并发标记阶段产生新的垃圾(并发阶段和应用程序一同运行)
    - 筛选回收，在筛选回收阶段首先对各个Region的回收价值和成本进行排序，根据用户所期望的GC停顿时间来制定回收计划

    参数配置
    - -XX:+UseG1GC -Xmx32g -XX:MaxGCPauseMillis=200，设置GC的最大暂停时间为200ms.

>-Xmx最大堆,
>
>-Xms：初始堆大小,
>
>-Xmn:年轻代大小,
>
>-XXSurvivorRatio年轻代中Eden区与Survivor区的大小比值,注意，Survivor有2个，这里是一个的比值。

### 类加载
1. 加载：
   - 通过类型的完全限定名，产生一个代表该类型的二进制数据流
   - 解析这个二进制数据流为方法区内的运行时数据结构
   - 创建一个表示该类型的java.lang.Class类的实例，作为方法区这个类的各种数据的访问入口。
   
2. 验证，为了确保Class文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。
   - 文件格式验证
   - 元数据验证
   - 字节码验证
   - 符号引用验证
   
3. 准备，正式为类变量分配内存并设置类变量初始值的阶段，这些变量所使用的内存都将在方法区中进行分配。
4. 解析，虚拟机将常量池内的符号引用替换为直接引用的过程
5. 初始化，真正开始执行类中定义的java程序代码。

### 类加载器
1. 启动类加载器（Bootstrap ClassLoader）将存放在\lib目录的类库加载到虚拟机内存中
2. 扩展类加载器（Extension ClassLoader）：它负责加载\lib\ext
3. 应用程序类加载器，负责加载用户类路径（ClassPath）上所指定的类库
4. 双亲委派模型的工作过程： 如果一个类加载器收到了类加载的请求，先把这个请求委派给父类加载器去完成（所以所有的加载请求最终都应该传送到顶层的启动类加载器中），只有当父加载器反馈自己无法完成加载请求时，子加载器才会尝试自己去加载。

## io

### NIO
1. Non-blocking，与IO的区别：
   - IO是面向流的，NIO是面向缓冲区的。
   - IO流是阻塞的，NIO流是不阻塞的。NIO中，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。而IO则是：当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了
   - Selectors（选择器）。选择器用于使用单个线程处理多个通道。线程之间的切换对于操作系统来说是昂贵的。 选择器是一个可以监视多个通道的对象，使用Selector的话，我们必须把Channel注册到Selector上，然后就可以调用Selector的select()方法。这个方法会进入阻塞，直到有一个channel的状态符合条件。当方法返回后，线程可以处理这些事件

#### Channel
[来源](https://mp.weixin.qq.com/s?__biz=MzU4NDQ4MzU5OA==&mid=2247483966&idx=1&sn=d5cf18c69f5f9ec2aff149270422731f&chksm=fd98545fcaefdd49296e2c78000ce5da277435b90ba3c03b92b7cf54c6ccc71d61d13efbce63#rd)

1. FileChannel： 用于文件的数据读写。使用：
   - 开启FileChannel。无法直接打开抽象类FileChannel，需要通过 InputStream，OutputStream或RandomAccessFile获取FileChannel。
   - 从FileChannel读取数据read()/写入数据write()
   - 关闭FileChannel
2. DatagramChannel： 用于UDP的数据读写
   - 获取DataGramChannel
   - 接收/发送消息
3. SocketChannel： 用于TCP的数据读写，一般是客户端实现
  - 通过SocketChannel连接到远程服务器
  - 创建读数据/写数据缓冲区对象来读取服务端数据或向服务端发送数据
  - 关闭SocketChannel
4. ServerSocketChannel: 允许我们监听TCP链接请求，每个请求会创建会一个SocketChannel，一般是服务器实现
   - 通过ServerSocketChannel 绑定ip地址和端口号
   - 通过ServerSocketChannelImpl的accept()方法创建一个SocketChannel对象用户从客户端读/写数据
   - 创建读数据/写数据缓冲区对象来读取客户端数据或向客户端发送数据
   - 关闭SocketChannel和ServerSocketChannel
5. Scatter / Gather
   - Scatter: 从一个Channel读取的信息分散到N个缓冲区中(Buufer)
      
		     ByteBuffer header = ByteBuffer.allocate(128);
			 ByteBuffer body = ByteBuffer.allocate(128);
			 ByteBuffer [] array = {header,body}
			 channel.read(array);
			 //read()方法内部会负责把数据按顺序写进传入的buffer数组内。一个buffer写满后，接着写到下一个buffer中。
			 //举个例子，假如通道中有200个字节数据，那么header会被写入128个字节数据，body会被写入72个字节数据；

   - Gather: 将N个Buffer里面内容按照顺序发送到一个Channel.
      >无论是scatter还是gather操作，都是按照buffer在数组中的顺序来依次读取或写入的

6. 通道之间的数据传输。以上都是通道与缓冲的传输，通道之间可以传输。在Java NIO中如果一个channel是FileChannel类型的，那么他可以直接把数据传输到另一个channel。


#### Selector（选择器）
[来源](https://mp.weixin.qq.com/s?__biz=MzU4NDQ4MzU5OA==&mid=2247483970&idx=1&sn=d5e2b133313b1d0f32872d54fbdf0aa7&chksm=fd985423caefdd354b587e57ce6cf5f5a7bec48b9ab7554f39a8d13af47660cae793956e0f46#rd)

1. 用于检查一个或多个NIO Channel（通道）的状态是否处于可读、可写。使用Selector的好处在于： 使用更少的线程来就可以来处理通道了， 相比使用多个线程，避免了线程上下文切换带来的开销。

2. SelectionKey表示了一个特定的通道对象和一个特定的选择器对象之间的注册关系.


### 问题：
- System.out.println()是什么？println是PrintStream的一个方法。out是一个静态PrintStream类型的成员变量，System是一个java.lang包中的类，用于和底层的操作系统进行交互。
- File类，它主要用于知道一个文件的属性，读写权限，大小等信息
- RandomAccessFile，它在java.io包中是一个特殊的类，既不是输入流也不是输出流，它两者都可以做到。


### AIO
1. 特点：
   - 读完后通知
   - 不会加快IO，只是读完后进行通知
   - 使用回调函数，进行业务处理。能够胜任那些重量级，读写过程长的任务
   
   
## Nginx
1. [nginx](https://www.songma.com/news/txtlist_i29104v.html)模块一般被分成三大类：handler、filter和upstream。其中upstream模块，将使nginx跨越单机的限制，完成网络数据的接收、处理和转发。
### 负载均衡涉及到的算法
1. 轮询Round Robin， 对所有的后端服务端backend轮训发送请求，默认的分配方式。
2. weight，跟踪backend当前的活跃连接数目，最少的连接数目说明这个backend负载最轻，将请求分配给他。这种方式会考虑到配置中给每个upstream分配的weight权重信息；
3. IP Hash(ip_hash)，每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题
4. Generic Hash(hash),以客户自己设置资源(比方URL)的方式计算hash值完成分配，其可选consistent关键字支持一致性hash特性；

### 会话一致性
1. 原始负载均衡的基本原理：当客户端连接请求过来时，负载均衡系统内部会专门有一张表来记录这些连接的状况:
   
   		[源IP：端口]、[目的IP：端口]、[服务器IP：端口]、空闲超时时间（Idle Timeout）
   这张表的大小一般称之为最大并发连接数，也就是系统同时能够容纳的连接数量。在该表中，设计了一个空闲超时时间的参数，即当该连接在一定时间内无流量通过时，负载均衡会自动删除该连接条目，释放系统资源。
2. 会话保持机制的意义就在于，确保将来自相同客户端的请求，转发至后端相同的服务器进行处理。如果在客户端和服务器之间部署了负载均衡设备，很有可能，这多个连接会被转发至不同的服务器进行处理。如果服务器之间没有会话信息的同步机制，会导致其他服务器无法识别用户身份。

### 后端服务端的动态配置
1. 出问题的backend要能被及时探测并剔除出分配群，而当业务增长的时候可以灵活的增加backend数目

### 基于DNS的负载均衡

### Nginx中的负载均衡
1. 会话一致性：在第一次分配之后，该会话的所有请求都分配到那个相同的backend上面。
   - Cookie Insertion。在backend第一次response之后，会在其头部增加一个session cookie，之后用户端接下来的请求都会带有这个cookie值，Nginx可以根据这个cookie判断需要转发给哪个backend了。
   - Sticky Routes。在backend第一次response之后，会产生一个route信息，route信息通常会从cookie/URI信息中提取。
   - Learn。Nginx会自动监测request和response中的session信息，而且通常需要回话一致性的请求、应答中都会带有session信息，这和第一种方式相比是不用添加cookie，而是动态学习已有的session。
2. backend健康监测
3. 通过DNS设置HTTP负载均衡
4. TCP/UDP流量的负载均衡


## redis
redis快，基于内存，单线程，基于resp协议。
可以将mysql中的数据，根据select语句做管道输入，将数据转入redis。

### redis做消息队列
1. 通过2个命令实现pub/sub模式的消息通知
   - `publish 主题名称 消息内容`,向指定主题发送一条消息
   - `subscribe 主题名称`,订阅某一主题。
2. key失效的时候会发送一些通知，可以通过订阅某一个主题，接受key失效的消息通知（此通知是redis内部事件处理机制）
   - 首先开启事件通知，修改redis启动的配置文件：安装目录下的redis.windows.conf，查找到`notify-keyspace-events Ex`，取消其注释，然后重启redis服务器。
   - 接着，需要订阅的主题名称：`__keyevent@dbindex__:expired`。例子：存储到的redis服务器是0号数据库，则订阅命令为` subscribe __keyevent@0__:expired`，然后测试`set keyName "xu"`,`expire keyName 5`设置其5秒后过期，`ttl keyName`查看其剩余时间。
    >不过很遗憾，我修改配置文件并没有生效，而是运行命令`config set notify-keyspace-events Ex`才可以



### redis为何那么快
1. redis是基于内存的，一般都是简单的存取操作，内存的读写速度非常快
2. redis是单线程的，省去了很多上下文切换线程的时间，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗
    >官方FAQ表示：因为Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存的大小或者网络带宽。既然单线程容易实现，而且CPU不会成为瓶颈，那就顺理成章地采用单线程的方案了
3. redis使用非阻塞I/O多路复用技术，可以处理并发的连接
   - 多路，指的是多个socket连接
   - 复用，指的是复用一个线程

    [Redis-client 在操作的时候](https://zhuanlan.zhihu.com/p/50392209)，会产生具有不同事件类型的 Socket。在服务端，有一段 I/O 多路复用程序，将其置入队列之中。然后，文件事件分派器，依次去队列中取，转发到不同的事件处理器中。
    >Redis服务器与客户端通过RESP（REdis Serialization Protocol）协议通信


### 过期数据的删除对 Redis 性能影响

1. [定时删除](https://zhuanlan.zhihu.com/p/57089960)：在设置键过期expire时间的同时，创建一个定时器 Timer，让定时器在键过期时间来临时立即执行对过期键的删除。
  - 对内存友好，对 CPU 不友好。如果过期删除的键比较多的时候，删除键这一行为会占用相当一部分 CPU 性能，会对 Redis 的吞吐量造成一定影响
2. 惰性删除：键过期后不管，每次读取该键时，判断该键是否过期，如果过期删除该键返回空。
  - 对 CPU 友好，内存不友好。如果很多键过期了，但在将来很长一段时间内没有很多客户端访问该键导致过期键不会被删除，占用大量内存空间。
3. 定期删除：每隔一段时间对数据库中的过期键进行一次检查。
  - Redis 会将每一个设置了 expire 的键存储在一个独立的字典中，以后会定时遍历这个字典来删除过期的 key。除了定时遍历外，它还会使用惰性删除策略来删除过期的 key。
  - Redis 默认每秒进行十次过期扫描，过期扫描不会扫描所有过期字典中的 key，而是采用了一种简单的贪心策略:从过期字典中随机选择 20 个 key；删除这 20 个 key 中已过期的 key；如果过期 key 比例超过 1/4，那就重复步骤一
  - 同时，为了保证在过期扫描期间不会出现过度循环，导致线程卡死，算法还增加了扫描时间上限，默认不会超过 25ms
4. 定期删除+惰性删除。如果定期删除没删除掉 Key。并且你也没及时去请求 Key，也就是说惰性删除也没生效。这样，Redis 的内存会越来越高。那么就应该采用内存淘汰机制。在redis.conf文件中，`# maxmemory-policy volatile-lru`用于配内存淘汰策略.其中比较推荐的一种是：allkeys-lru，当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 Key

### 如何应对缓存穿透和缓存雪崩问题
1. 缓存穿透，即黑客故意去请求缓存中不存在的数据，导致所有的请求都怼到数据库上，从而数据库连接异常。
  - 提供一个能迅速判断请求是否有效的拦截机制，比如，利用布隆过滤器，内部维护一系列合法有效的 Key。迅速判断出，请求所携带的 Key 是否合法有效。如果不合法，则直接返回。
2. 缓存雪崩，即缓存同一时间大面积的失效，这个时候又来了一波请求，结果请求都怼到数据库上，从而导致数据库连接异常。
  - 给缓存的失效时间，加上一个随机值，避免集体失效。
  - 双缓存。我们有两个缓存，缓存 A 和缓存 B。缓存 A 的失效时间为20分钟，缓存 B 不设失效时间。自己做缓存预热操作。例如：从缓存 A 读数据库，有则直接返回；A 没有数据，直接从 B 读数据，直接返回，并且异步启动一个更新线程，更新线程同时更新缓存 A 和缓存 B。




### 题：
1. [TCP协议-如何保证传输可靠性](https://blog.csdn.net/liuchenxia8/article/details/80428157)
2. [跳表](https://www.cnblogs.com/seniusen/p/9870398.html)
3. [非递归且不用额外空间（不用栈），如何遍历二叉树](https://blog.csdn.net/Darfie/article/details/53302104)
4. 操作系统的内存管理机制，待解决
5. [快排](https://www.cnblogs.com/MOBIN/p/4681369.html)
6. [OOM](https://www.jianshu.com/p/2fdee831ed03)：
   - java.lang.OutOfMemoryError:Java heap space：
     - 堆大小设置小了。解决办法：在IDEA的Run/Debug Configturations的VM中填写`-Xmx2014m`
     - 内存泄漏：由于GC无法识别一些已经不再使用的对象，而这些未使用的对象一直留在堆空间中，这种堆积最终会导致OOM。
   - java.lang.OutOfMemoryError:GC overhead limit exceeded。默认情况下，当应用程序花费超过98%的时间用来做GC并且回收了不到2%的堆内存时，会抛出该错误。具体的表现就是你的应用几乎耗尽所有可用内存，并且GC多次均未能清理干净。
   - java.lang.OutOfMemoryError:Permgen space。表明持久代所在区域的内存已被耗尽。不过JDK8已经完全移除持久代空间：
     - 如果只是空间小，`-XX:MaxPermSize=512m`
     - 如果是Redeploy时的OutOfMemoryError，
   - java.lang.OutOfMemoryError:Metaspace。太多的类或太大的类加载到元空间。
     - 如果空间不够，`-XX：MaxMetaspaceSize = 512m`
   - java.lang.OutOfMemoryError:Unable to create new native thread.意味着Java应用程序已达到其可以启动线程数量的极限了。
7. [堆](https://www.jianshu.com/p/6b526aa481b1)
8. 局部变量使用前需要显式地赋值，否则编译通过不了，为什么这么设计：
   - 成员变量赋值与取值的先后顺序具有不确定性，因此交给JVM去初始化
   - 局部变量，其赋值与取值的访问顺序是确定的。这样做为了尽最大可能减少使用者犯错。
   


