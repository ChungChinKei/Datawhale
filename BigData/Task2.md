
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
关闭防火墙
```
[root@DW1 ~]# systemctl stop firewalld.service
[root@DW1 ~]# systemctl disable firewalld.service
```

关闭SELINUX
```
# 修改为SELINUX=disabled
[root@DW1 ~]# vi etc/selinux/config

# 重启完成设置
[root@DW1 ~]# reboot
```
安装JDK（已完成）

### 2.Hadoop安装包下载  
创建一个hadoop文件夹存放安装包
```
[root@DW1 ~]# cd /usr
[root@DW1 ~]# mkdir hadoop
[root@DW1 ~]# cd hadoop
```

通过命令行在虚拟机上下载：

```
[root@DW1 hadoop]# wget http://apache.claz.org/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz
```
或者通过[下载链接](http://archive.apache.org/dist/hadoop/core/)  手动下载到主机，再通过xshell及lrzsz等工具上传到虚拟机：
```
[root@DW1 hadoop]# yum install -y lrzsz

#通过rz命令进行上传
[root@DW1 hadoop]# rz
```
解压安装包：
```
[root@DW1 hadoop]# tar -xzvf hadoop-3.2.0.tar.gz
```
修改环境变量：
```
[root@DW1 ~]# vi /etc/profile

# 添加以下内容
export HADOOP_HOME=/home/heitao/soft/hadoop-3.2.0
exprot PATH=$PATH:$ HADOOP_HOME/bin

[root@DW1 ~]# source /etc/profile
```
检验是否安装成功：
```
[root@DW1 hadoop]# hadoop -version
```
