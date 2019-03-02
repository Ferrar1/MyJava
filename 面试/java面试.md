- [计算机网络](#计算机网络)
- [数据库](#数据库)
   - [mysql索引](#mysql索引)
- [操作系统](#操作系统)
- [spring](#spring)
- [集合框架](#集合框架)

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
5. 网络层将segment封装到datagram。利用ARP协议将IP地址解析为MAC地址。
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
  
4. 为了避免上面出现几种情况在标准SQL规范中定义了4个事务隔离级别：
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

### TreeMap
