# MyJava
学习路上的一些知识点

  
## idea:

1. 键入psvm, 也就是public static void main的首字母。然后enter。    
2. 键入fori，enter即可。   
3. 如何像写一个System.out.println();就是sout   
4. view——>Tool windows——>structure或者alt+7。查看当前类的方法。


## markdown：

1. 图片添加：
    - `<img src="https..." width = 30% height = 30% />`
    - `<div align=center>![这里写图片描述](http:...)`
    - `<img src="<img src="https://github.com/xuzhuang1996/MyJava/blob/master/img/面试/1-B+树插入过程.gif" width=50% height=50% />" width = 50% height = 50% div align=right />`


## java
1. 正则表达式过滤出字母、数字和中文
   - 过滤出字母: `[^(A-Za-z)]`
   - 过滤出数字: `[^(0-9)]`
   - 过滤出中文: `[^(\\u4e00-\\u9fa5)]`,不过直接复制这个到idea会自动补/，因此记住这里只有2个/
   - 过滤字母数字：`s.replaceAll("[^A-Za-z0-9]", ""); `
