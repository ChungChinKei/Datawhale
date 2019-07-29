
# 【Task 2】搭建Hadoop集群(3day)

1. 搭建HA的Hadoop集群并验证，3节点(1主2从)，理解HA/Federation,并截图记录搭建过程
2. 阅读Google三大论文，并总结
3. Hadoop的作用（解决了什么问题）/运行模式/基础组件及架构
4. 学会阅读HDFS源码，并自己阅读一段HDFS的源码(推荐HDFS上传/下载过程)
5. Hadoop中各个组件的通信方式，RPC/Http等
6. 学会写WordCount（Java/Python-Hadoop Streaming），理解分布式/单机运行模式的区别
7. 理解MapReduce的执行过程
8. Yarn在Hadoop中的作用

参考资料：
[Google三大论文](https://blog.csdn.net/w1573007/article/details/52966742)


集群规划1-省机器：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019042014023351.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hlaXRhbzUyMDA=,size_16,color_FFFFFF,t_70)


集群规划2-清晰：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190420140245947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hlaXRhbzUyMDA=,size_16,color_FFFFFF,t_70)

---
## 搭建HA的Hadoop集群并验证  

### 1.安装Hadoop前的准备工作 
关闭SELINUX
```
# 修改为SELINUX=disabled
vi etc/selinux/config
```

### 2.Hadoop安装包下载  
[下载hadoop安装包](http://archive.apache.org/dist/hadoop/core/)  

通过命令行在虚拟机上下载：

```
[root@DW1 ~]# wget http://apache.claz.org/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz
```
或者通过上面的链接手动下载到主机，再通过xshell及lrzsz等工具上传到虚拟机：
```
[root@DW1 ~]# yum install -y lrzsz

#通过rz命令进行上传
[root@DW1 ~]# rz
```