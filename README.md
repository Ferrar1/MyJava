# MyJava
学习路上的一些知识点

  
## idea:

1. 键入psvm, 也就是public static void main的首字母。然后enter。    
2. 键入fori，enter即可。   
3. 如何像写一个System.out.println();就是sout   
4. view——>Tool windows——>structure或者alt+7。查看当前类的方法。
5. 选中方法，ctrl+F7。跳转到调用该方法的位置。
6. ctrl+shift+N，查文件名


## markdown：

1. 图片添加：
    - `<img src="https..." width = 30% height = 30% />`
    - `<div align=center>![这里写图片描述](http:...)`
    - `<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/1-B+树插入过程.gif" width=50% height=50% />`


## java
1. 正则表达式过滤出字母、数字和中文
   - 过滤出字母: `[^(A-Za-z)]`
   - 过滤出数字: `[^(0-9)]`
   - 过滤出中文: `[^(\\u4e00-\\u9fa5)]`,不过直接复制这个到idea会自动补/，因此记住这里只有2个/
   - 过滤字母数字：`s.replaceAll("[^A-Za-z0-9]", ""); `
   
2. 新特性：
   1. default接口
   2. `::`，[双冒号](https://www.cnblogs.com/tietazhan/p/7486937.html)
   3. lambda。都在Function包下。以及[流](https://blog.csdn.net/lidai352710967/article/details/82496783).注意：所有 Stream 的操作必须以 lambda 表达式为参数
   4. [流stream的原理](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/)。这篇文章学习流必须看。
      1. 操作包括：
        - Intermediate：map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered。Intermediate 操作永远是惰性化的。
        - Terminal：forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator
        - Short-circuiting：anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 limit
      2. 一次性使用。terminal操作之后无法再次进行terminal操作。
      3.  可以并行计算。缺点是结果打乱之前顺序
      4. 注意：reduce(),如果没有设置初始种子，返回的是 Optional，也就是可能没有值进行reduce操作。而如果指定了初始值，就返回具体的对象 
   4. Function接口：
      - [Function<T, R>，T—函数的输入类型，R-函数的输出类型。](https://www.orchome.com/935)lambda的写法就是Function内部写一个方法T为输入，使用的时候调用apply或者compose等就是相当于重写了该方法，获取返回。
      - 当然，对于需要重复使用的Function，可以事前定义好，像写函数一样，在具体使用的时候直接赋值。然后调用时直接用apply方法传参数就好了。
## git
1. `git remote update origin --prune`   # 更新远程主机origin 整理分支
2. `git branch -r`查看远程分支
3. `git push -f origin master `git push 强制提交
4. `git fetch -all`从远程仓库下载最新版本。`git reset --hard origin/master`将本地设为刚获取的最新的内容
5. `.gitignore`在已经push后，想重新添加忽略文件：

        git rm -r --cached .
        git add .
        git commit -m 'update .gitignore'
6. 只合并customize_new分支的部分文件到当前分支：`git checkout  customize_new /home/mi/miui-bi/miui-bi-web/src/main/java/com/MiuiAppPaiEntity.java`。
7. git提交后出现nano界面:Ctrl + X然后输入y再然后回车，就可以退出了
8. `git stash`将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。这也就是说，stash中的内容不仅仅可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。[来源](https://blog.csdn.net/stone_yw/article/details/80795669)
   - `git stash save ""`，作用等同于git stash，区别是可以加一些注释
   - `git stash list`,查看当前stash中的内容
   - `git stash pop`,将当前stash中的内容弹出，并应用到当前分支对应的工作目录上。
   - `git stash apply`,将堆栈中的内容应用到当前目录，不同于git stash pop，该命令不会将内容从堆栈中删除，也就说该命令能够将堆栈的内容多次应用到工作目录中，适应于多个分支的情况。
   
## mysql
1. Ubuntu下sudo能无密码访问，但是普通用户无法登录。这样就可以密码登录
   - update mysql.user set authentication_string=PASSWORD('12345678'), plugin='mysql_native_password' where user='root';
   - flush privileges;
