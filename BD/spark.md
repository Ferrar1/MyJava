[来源厦大大数据,强烈推荐](http://dblab.xmu.edu.cn/blog/)
## scala的部分知识点
1. 闭包
   1. 闭包是一个函数，一种比较特殊的函数。一个普通的函数，在函数中只会应用函数中定义的变量，比如x就是函数的传入参数，不会引用函数外部的变量。而闭包则不同，闭包会引用函数外部的变量。当外部变量被绑定值后，不再是“自由变量”，或者说“被关闭”了，这也是为什么我们说这样一个函数叫做“闭包”，它反映了一个从开放到封闭的过程。而且，闭包（也是一个函数）对这个绑定的变量作出的改变在闭包之外也是可见的，函数体中对该值做了修改，在函数外部也可以看到这种变化。

                //在上面的MyTest中， step是一个自由变量，它的值只有在运行的时候才能确定，num的类型是确定的，num的值只有在调用的时候才被赋值。
                //这样的函数，被称为“闭包”，它反映了一个从开放到封闭的过程。
                object MyTest{
                    def main(args: Array[String]): Unit={    
                        def plusStep(step: Int) = (num: Int) => num + step
                        //给step赋值
                        val myFunc = plusStep(3)
                        //调用myFunc函数
                        println(myFunc(10))     
                    }
                }
        
2. Option类型
   1. Option类包含一个子类Some，当存在可以被引用的值的时候，就可以使用Some来包含这个值，例如Some(“Hadoop”)。Option类型还提供了getOrElse方法，这个方法在这个Option是Some的实例时返回对应的值，而在是None的实例时返回传入的参数。Option[T]实际上是一个容器，我们可以对get出的值进行map、filter等操作

           //首先我们创建一个映射
           scala> val books=Map("hadoop"->5,"spark"->10,"hbase"->7)
           books: scala.collection.immutable.Map[String,Int] = Map(hadoop -> 5, spark -> 10, hbase -> 7)

           //下面我们从映射中取出键为"hadoop"对应的值，这个键是存在的，可以取到值，并且取到的值会被包含在Some中返回
           scala> books.get("hadoop")
           res0: Option[Int] = Some(5)

           //下面我们从映射中取出键为"hive"对应的值，这个键是不存在的，所以取到的值是None对象
           scala> books.get("hive")
           res1: Option[Int] = None
           
           Seq(1,2,3).headOption
           //res2: Option[Int] = Some(1)
3. object
   1. 采用object关键字实现单例对象.
   2. 伴生对象: 在Java中，我们经常需要用到同时包含实例方法和静态方法的类，在Scala中可以通过伴生对象来实现.2者必须存在于同一个文件中。
      > Scala源代码编译后都会变成JVM字节码，实际上，在编译上面的源代码文件以后，在Scala里面的class和object在Java层面都会被合二为一，class里面的成员成了实例成员，object成员成了static成员。
4. reduce
   1. 使用reduce二元操作对集合中的元素进行归约。
                
                //默认reduceLeft
                scala> val list = List(1,2,3,4,5)
                list: List[Int] = List(1, 2, 3, 4, 5)
                scala> list.reduceLeft(_ - _)
                res25: Int = -13
                scala> list.reduceRight(_ - _)
                res26: Int = 3
5. fold
   1. fold操作需要从一个初始的“种子”值开始，并以该值作为上下文，处理集合中的每个元素。
   
            //fold函数需要两个参数，一个参数是初始种子值，这里是10，另一个参数是用于计算结果的累计函数，这里是累乘。
            scala> val list = List(1,2,3,4,5)
            list: List[Int] = List(1, 2, 3, 4, 5)

            scala> list.fold(10)(_*_)
            res0: Int = 1200
            
## spark2.0
1. [RDD的设计与运行原理](http://dblab.xmu.edu.cn/blog/985-2/)
   1. 例子。map(line => line.split(" ").size)，对lines中的每个元素执行匿名函数后，map操作返回每个元素的size组成的集合`RDD[Int]`。reduce：首先取出1和2，把a赋值为1，把b赋值为2，然后，执行大小判断，保留2。下一次，让保留下来的2赋值给a，再从`RDD[Int]`中取出下一个元素3，把3赋值给b，然后，对a和b执行大小判断，保留较大者3.依此类推。最终，reduce()操作会得到最大值是5。
   
            scala> val lines = sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
            scala> lines.map(line => line.split(" ").size).reduce((a,b) => if (a>b) a else b)
   2. [创建RDD](http://dblab.xmu.edu.cn/blog/990-2/)
   3. 常见操作
      - reduceByKey(func)，使用func函数合并具有相同键的值。比如，reduceByKey((a,b) => a+b)，有四个键值对(“spark”,1)、(“spark”,2)、(“hadoop”,3)和(“hadoop”,5)，对具有相同key的键值对进行合并后的结果就是：(“spark”,3)、(“hadoop”,8)。可以看出，(a,b) => a+b这个Lamda表达式中，a和b都是指value，比如，对于两个具有相同key的键值对(“spark”,1)、(“spark”,2)，a就是1，b就是2。
      - groupByKey()，对具有相同键的值进行分组。比如，对四个键值对(“spark”,1)、(“spark”,2)、(“hadoop”,3)和(“hadoop”,5)，采用groupByKey()后得到的结果是：(“spark”,(1,2))和(“hadoop”,(3,5))。
      - keys，只会把键值对RDD中的key返回形成一个新的RDD。
      - values只会把键值对RDD中的value返回形成一个新的RDD。
      - mapValues(func)，对键值对RDD中的每个value都应用一个函数，但是，key不会发生变化。
      - join，内连接，对于给定的两个输入数据集(K,V1)和(K,V2)，只有在两个数据集中都存在的key才会被输出，最终得到一个(K,(V1,V2))类型的数据集。
      
            scala> pairRDD1.join(pairRDD2).foreach(println)
            (spark,(1,fast))
            (spark,(2,fast))
