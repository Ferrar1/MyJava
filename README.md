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
   - default接口
   - `::`，[双冒号](https://www.cnblogs.com/tietazhan/p/7486937.html)
   - lambda。都在Function包下。以及[流](https://blog.csdn.net/lidai352710967/article/details/82496783).注意：所有 Stream 的操作必须以 lambda 表达式为参数
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


`ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDaWA00alP3ACTMQ/5+NEJt7CmXVnnpGkG4Z3oK43Qr2XudAijduHP8V9BsI0nZBWQ0jgt6XtpB9wLT1ZtE/efWGGDh8hcMjNPn8qp/xLajIoUGLOUreUOkZYQnVdirCwnR+eo68rQ0V/X6o5CjHTz+0sHGO7wvjF0HeVww0N08Ea0FLW2WoeAXmLFFGzQjeKWTheAwZKUqsKuHaEiltzMWBYLRIt8WfIro7j78Dkur/01C7jsIExvsCYgU5MXtThWDoHCzxdZxzFMEL4nZBIQ0NasGeyqQ3qDH5mWjhTUYK6/vqvdY5rXd0ClZYN1Cv2v+9Jy50qtZU77+41F0eh3j xuzhuang`
