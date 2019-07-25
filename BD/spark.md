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
