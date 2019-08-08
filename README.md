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
   4. [流stream的原理](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/)
      - 操作包括：
        - Intermediate：map (mapToInt, flatMap 等)、 filter、 distinct、 sorted、 peek、 limit、 skip、 parallel、 sequential、 unordered
        - Terminal：forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、 anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator
        - Short-circuiting：anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 limit
      - 一次性使用。terminal操作之后无法再次进行terminal操作。
      - 可以并行计算。缺点是结果打乱之前顺序
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
8. `git stash`将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。这也就是说，stash中的内容不仅仅可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。
   1. 之前我都是开发到一部分就提交一次。这样其实每次提交版本都需要确定，所以很麻烦，现在只是保存了，后面还可以恢复。
   2. 当正在dev分支上开发某个项目，这时项目中出现一个bug，需要紧急修复，但是正在开发的内容只是完成一半，还不想提交，这时可以用git stash命令将修改的内容保存至堆栈区，然后顺利切换到hotfix分支进行bug修复，修复完成后，再次切回到dev分支，从堆栈中恢复刚刚保存的内容。
   3. 由于疏忽，本应该在dev分支开发的内容，却在master上进行了开发，需要重新切回到dev分支上进行开发，可以用git stash将内容保存至堆栈中，切回到dev分支后，再次恢复内容即可。
