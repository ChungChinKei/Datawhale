
# 【Task 3】HDFS常用命令/API+上传下载过程
1. 认识HDFS
- HDFS是用来解决什么问题的
- HDFS设计与架构
2. 熟悉hdfs常用命令
3. Python操作HDFS的其他API
4. 观察上传后的文件，上传大于128M的文件与小于128M的文件有何区别？
5. 启动HDFS后，会分别启动NameNode/DataNode/SecondaryNameNode，这些进程的的作用分别是什么？
6. NameNode是如何组织文件中的元信息的，edits log与fsImage的区别？使用hdfs oiv命令观察HDFS上的文件的metadata
7. HDFS文件上传下载过程，源码阅读与整理。


参考: [https://segmentfault.com/a/1190000002672666](https://segmentfault.com/a/1190000002672666)

参考资料：[Python3调用Hadoop的API](https://www.cnblogs.com/sss4/p/10443497.html)

---
## HDFS的概念相关
写在了本人的CSDN博客：https://blog.csdn.net/qq_39315740/article/details/97955575

## HDFS常用命令
hadoop hdfs的命令与linux常用的命令非常相似，如：
```
hadoop fs -mkdir #创建HDFS目录 
hadoop fs -ls    #列出DFS目录 
hadoop fs -put   #复制本地文件到HDFS
hadoop fs -get   #复制HDFS文件到本地
hadoop fs -cp    #复制HDJS文件
hadoop fs -rm    #删除HDFS文件
```
## HDFS文件上传和下载过程
### 上传文件
```
# 创建input文件夹
[root@DW1 ~]# hadoop fs -mkdir /input 

# 查看hdfs的文件，可以看到刚才创建的文件夹
[root@DW1 ~]# hadoop fs -ls /
Found 1 items
drwxr-xr-x   - root supergroup          0 2019-07-29 07:13 /input

# 预先上传一个大于128M的文件test.flv到local,然后上传到HDFS
[root@DW1 ~]# cd /usr/local/
[root@DW1 local]# hadoop fs -put test.flv /input

# ls命令可以看到我们刚才上传的文件
[root@DW1 local]# hadoop fs -ls /input
Found 1 items
-rw-r--r--   2 root supergroup  185701130 2019-07-29 07:46 /input/test.flv

# 在DW2上下载test.flv
[root@DW2 ~]# cd /usr/local/
[root@DW2 local]# hadoop fs -get /input/test.flv
[root@DW2 local]# ls
bin  data  etc  games  hadoop  include  lib  lib64  libexec  sbin  share  src  test.flv

```
