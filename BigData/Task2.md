
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
[root@DW1 ~]# firewall-cmd --state
not running
```

关闭SELINUX
```
# 修改为SELINUX=disabled
[root@DW1 ~]# vi etc/selinux/config

# 重启完成设置
[root@DW1 ~]# reboot
```
安装JDK（已完成）  
  
建立ip与主机名的联系
```
vi /etc/hosts

# 添加内容
192.168.xx.xxx DW1
192.168.xx.xxx DW2
192.168.xx.xxx DW3

#添加完成后可以以DWX代替ip地址，如：
[root@DW1 ~]# ping DW2
```
配置SSH
```
# 通过ssh工具获取公匙密匙
[root@DW1 ~]$ ssh-keygen -t rsa

# 进行ssh搭建
[root@DW1 ~]$ cd .ssh
[root@DW1 ~]$ cp id_rsa.pub authorized_keys
[root@DW1 ~]$ ssh-copy-id dw2
[root@DW1 ~]$ ssh-copy-id dw3

# 这时dw2和dw3已可以对dw1免密码登录
[root@DW1 ~]$ ssh dw2
[root@DW1 ~]$ ssh dw3

#对DW2,DW3也进行相同地操作，保证3台设备可以免密互联
```

### 2.Hadoop安装包下载  
创建一个hadoop文件夹存放安装包
```
[root@DW1 ~]# cd /usr/local
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
export HADOOP_HOME=/usr/local/hadoop/hadoop-3.2.0
exprot PATH=$PATH:$HADOOP_HOME/bin

[root@DW1 ~]# source /etc/profile
```
检验是否安装成功：
```
[root@DW1 hadoop]# hadoop -version
```
### 3.修改hadoop的配置  
配置core-site.xml(主要就是hdfs的地址)
```
[root@DW1 hadoop-3.2.0]$ cd etc/hadoop
[root@DW1 hadoop]# vi core-site.xml

#在最后添加以下内容
<configuration>
<property>
    <name>fs.default.name</name>
    <value>hdfs://DW1:9000</value>
</property>
</configuration>
```
配置hdfs-site.xml  
DW1为主节点，DW2、3为从节点，因此设置两个副本
```
[root@DW1 hadoop]# vi hdfs-site.xml

#在最后添加以下内容
<configuration>
<property>
  <name>dfs.name.dir</name>
  <value>/usr/local/data/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/usr/local/data/datanode</value>
</property>
<property>
  <name>dfs.tmp.dir</name>
  <value>/usr/local/data/tmp</value>
</property>
<property>
  <name>dfs.replication</name>
  <value>2</value>
</property>
</configuration>
```
配置mapred-site.xml 
```
<configuration>
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
</configuration>
```
配置yarn-site.xml
```
<configuration>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>DW1</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
</configuration>
```
配置slaves文件
```
[root@DW1 hadoop-3.2.0]$ vi slaves

# 添加从节点DW2,DW3(如果有默认的localhost，删掉即可)
DW2
DW3
```
配置hadoop-env.sh
```
添加JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10-0.el7_6.x86_64
```
o配置yarn-env.sh
```
同样添加添加JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10-0.el7_6.x86_64
```
### 4.完成对从节点的配置
创建data文件夹(DW2,DW3也要创建)
```
# /usr/local 下创建data文件夹
[root@DW1 local]# mkdir data
```
将hadoop拷贝到DW2,DW3
```
[root@DW1 local]# scp -r hadoop DW2:/usr/local
[root@DW1 local]# scp -r hadoop DW3:/usr/local
```
同时DW2,DW3也要配置换环境等
```
#进行检验
[root@DW2 local]# java -version
[root@DW2 local]# hadoop -version
```
### 5.集群启动
在DW1上节点格式化
```
[root@DW1 ~]# hdfs namenode -format
```
启动集群
```
[root@DW1 ~]# start-dfs.sh
```
结果遇到以下问题：
```
ERROR: Attempting to operate on hdfs namenode as root
ERROR: but there is no HDFS_NAMENODE_USER defined. Aborting operation.
Starting datanodes
ERROR: Attempting to operate on hdfs datanode as root
ERROR: but there is no HDFS_DATANODE_USER defined. Aborting operation.
Starting secondary namenodes [DW1]
ERROR: Attempting to operate on hdfs secondarynamenode as root
ERROR: but there is no HDFS_SECONDARYNAMENODE_USER defined. Aborting operation.
```
在/usr/local/hadoop/hadoop-3.2.0/sbin下，
将start-dfs.sh，stop-dfs.sh两个文件顶部添加以下参数：
```
#!/usr/bin/env bash
HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```
start-yarn.sh，stop-yarn.sh的顶部添加以下参数：
```
#!/usr/bin/env bash
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
```
再启动就OK啦

