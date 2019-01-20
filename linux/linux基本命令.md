完整信息查看doc文件，这里只是记录基本信息
## 对终端中光标的操作：
- Ctrl+a 光标移动到开始位置
- Ctrl+e 光标移动到最末尾
- Ctrl+k 删除此处至末尾的所有内容
- Ctrl+u 删除此处至开始的所有内容
- Ctrl+l 相当于clear，即清屏
- Ctrl+d 关闭终端

## 常识
1、绝对路径从/开始，不需要参照物，在哪都行：ls /root。相对路径：./就是当前路径，../上一级目录下。

2、linux文件目录
![image text](https://github.com/xuzhuang1996/MyJava/blob/master/img/linux/linux1.png)
![image text](https://github.com/xuzhuang1996/MyJava/blob/master/img/linux/Linux2.png)

## 基础指令
1、ls

- `ls`——list列出当前路径下的所有文件以及文件夹名称
- `ls 路径`——指定路径下的所有文件文件夹名称。
- `ls -l 路径`——即-l表示list，以详细列表形式进行展示.
- `ls -la 路径`——显示所有文件文件夹，包括隐藏文件（隐藏文件以.开头）
- `ls -lh 路径`——以列表的形式且显示文件大小的时候以可读性较高的形式显示（所有目录都显示4k，但不代表他就是4k）

2、pwd

- `pwd`——print working directory打印当前工作目录

3、mkdir

- `mkdir -p 路径` ——`-p`没有路径就创建一个

4、touch 

- `touch 路径`——创建文件

5、cp

- `cp 被复制的文档路径 目的路径`——copy，
- `cp -r 被复制的文档路径 目的路径`——如果进行文件夹的复制，需要加-r(递归)

6、 mv

- `mv 需要移动的文档路径 目的路径`——move剪切,跟重命名用法一样

7、rm

- `rm 选项 需要移除的文档路径`——remove删除
- `rm -f 路径`——强制删除force，不需要y确认
- `rm -rf 路径`——强制删除目录。路径中lin*(使用通配符来删除，以lin开头)

8、输出重定向：将命令的输出结果保存到文件中。

- `>`覆盖输出
- `>>`追加输出

9、cat

- `cat 文件路径`——直接打开文件，不编辑，直接退出了。与vim不同。
- `cat 待合并文件1 待合并文件n  >  合并后路径`——可以用来合并文件。

## 进阶
1、df

- `df -h` ——查看磁盘空间。-h可读性

2、free

- `free -m`—— 查看内存使用情况(m是兆，-g也可以)

3、head

- `head -n 路径`——查看文件的前n行。默认10行。n为数字

4、tail

- `tail -n 路径`——显示后n行
- `tail -f 路径`——查看一个文件的动态变化（必须系统自己加的而不是人为加的）

5、less

- `less 路径`——查看文件，以较少的内容输出，按下辅助功能键输出更多：输入20：从20显示

6、wc

- `wc -lwc 路径`——l(line)行数w(word)单词数c(byte)字节数，统计文件信息

7、管道`|`(记住不能是中文的|)

- `ls /|grep y`——查询目录下包含y字母的文档（|前面的输出就是后面的输入，grep过滤

## 高级
1、hostname

- `hostname -f`——输出主机名FQDN，全限定域名,直接输出localhost这种
- `hostname`——输出完整主机名

2、id

- `id` ——默认显示当前用户信息

3、whoami

- `whoami`——显示当前用户名（一般用于shell脚本，方便记录日志）

4、ps

- `ps  -ef`——查看当前服务器的进程信息。-e(=-A)列出全部进程。-f显示全字段（属性）

5、top

- `top` ——（动态显示进程信息，退出q，一般3秒刷新一次）

6、du

- `du -sh 路径`——查看目录的真实大小。-s只显示汇总大小，-h高可读性显示

7、find

- `find  路径范围 选项-name 选项的值`——查找文件，支持模糊搜索
- `find  路径范围 选项-type 选项的值`——查找文件（-文件，这里需要用f替换，d文件夹）

8、kill

- `kill 进程PID`——根据pid杀死进程，配合ps使用。查到其PID。
- `killall 进程名称`

9、ifconfig

- `ifconfig`——获取网卡信息。2个网卡。Lo(loop)本地回环网卡,Inet 地址就是我们所说的ip地址

10、uptime

- `uptime`——输出计算机持续开机的时间

11、uname

- `uname`——获取计算机操作系统信息
- `uname  -a`——获取全部信息

12、netstat

- `netstat -tnlp`——查看网络的连接状态。   
   - -t只列出TCP协议的连接，
   - -n将地址从字母组合转化为ip地址，将协议转化为端口号来显示（否则端口显示- ipp,smtp）
   - -l只显示state状态列中值为listen（正在监听）的连接
   - -p显示发起连接的进程的PID与名称

## 答题：
- 如何在命令行中快速删除光标前后的内容？前：CTRL+U。后：CTRL+K
- 如何删除/tmp文件夹下所有A开头的文件？# rm  -f  /tmp/A*
- 统计系统有多少用户？# wc  -l  /etc/passwd(用户信息)
- 查看/etc的磁盘大小？#du  -sh  /etc
- fg：将后台中的命令调至前台继续运行。与配套：ctrl+z：可以将一个正在前台执行的命令放到后台，并且暂停。bg是将进程放到后台并唤醒
- awk -F ' ' '{print $3}' 指定空格是分隔符进行分割，取第三个。（不指定默认分隔符也是空格）
- uniq -c（uniq命令可以去除排序过的文件中的重复行，因此uniq经常和sort合用。也就是说，为了使uniq起作用，所有的重复行必须是相邻的。参数 - c ：进行计数）
缺省的Linux系统中，从后台启动进程，应在命令的结尾加上&符号。应用：redis集群时一次性开启6个服务不用开6个终端。

## vim
![image text](https://github.com/xuzhuang1996/MyJava/blob/master/img/linux/vimtmp.png)
