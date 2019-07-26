# 【Task1】创建虚拟机+熟悉linux(2day)

1. [创建三台虚拟机](https://mp.weixin.qq.com/s/WkjX8qz7nYvuX4k9vaCdZQ)
2. 在本机使用Xshell连接虚拟机
3. [CentOS7配置阿里云yum源和EPEL源](https://www.cnblogs.com/jimboi/p/8437788.html)
4. 安装jdk
5. 熟悉linux 常用命令
6. 熟悉，shell 变量/循环/条件判断/函数等

shell小练习1：
编写函数，实现将1-100追加到output.txt中，其中若模10等于0，则再追加输出一次。即10，20...100在这个文件中会出现两次。

注意：
* 电脑系统需要64位(4g+)
* 三台虚拟机的运行内存不能超过电脑的运行内存
* 三台虚拟机ip不能一样，否则会有冲突、



参考资料：
 1. [安装ifconfig](https://jingyan.baidu.com/article/363872ec26bd0f6e4aa16f59.html)
 2. [bash: wget: command not found的两种解决方法](https://www.cnblogs.com/areyouready/p/8909665.html)
 3. linux系统下载ssh服务
 4. [关闭windows的防火墙！如果不关闭防火墙的话，可能和虚拟机直接无法ping通！](https://www.linuxidc.com/Linux/2017-11/148427.htm)
 5. 大数据软件 ：[链接](https://pan.baidu.com/s/17fEq3IPVoeE29cWCrSpO8Q) 提取码：finf
 
---
## 创建三台虚拟机

1. Ubuntu16.04下安装virtualbox
 
```
sudo apt-get update
sudo apt-get install virtualbox
```
2. 命令行打开virtualbox
```
virtualbox
```
3. 进入GUI界面，新建三台虚拟机，每台分配1G内存，20G虚拟硬盘  
注意:三台虚拟机内存总和不能大于实体机  
![](https://github.com/ChungChinKei/Datawhale/blob/master/BigData/picture/virtualbox.png)

4. 分配光驱并加载镜像文件
5. 设置网络（桥接网卡）
6. 启动虚拟机
7. 设置网络
8. 分区
## 在本机使用Xshell连接虚拟机
1. Xshell6下载
下载链接：https://www.netsarang.com/en/xshell/
2. 设置好虚拟机ip
3. 进行连接

## CentOS7配置阿里云yum源和EPEL源
1. 备份系统的yum源
```
[root@DW1 ~]# cd /etc/yum.repos.d/
[root@DW1 yum.repos.d]# makdir repo_bak
[root@DW1 yum.repos.d]# yum -y install wget
[root@DW1 yum.repos.d]# mv *.repo repo_bak/
[root@DW1 yum.repos.d]# ls
repo_bak
```
2. 下载新的CentOS-Base.repo 到/etc/yum.repos.d/
```
[root@DW1 yum.repos.d]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
[root@DW1 yum.repos.d]# ls
CentOS-Base.repo  repo_bak
```
3. 清除缓存
```
[root@DW1 yum.repos.d]# yum clean all
```
4. 生成新的缓存
```
[root@DW1 yum.repos.d]# yum makecache
```
5. 安装EPEL源
```
[root@DW1 yum.repos.d]# yum list | grep epel-release
[root@DW1 yum.repos.d]# yum install -y prep epel-release
[root@DW1 yum.repos.d]# ls
CentOS-Base.repo  epel.repo  epel-testing.repo  repo_bak
```
6. 再次清除和生成缓存
```
[root@DW1 yum.repos.d]# yum clean all
[root@DW1 yum.repos.d]# yum makecache
```
7. 查看所有的yum源
```
[root@DW1 yum.repos.d]# yum repolist enabled
[root@DW1 yum.repos.d]# yum repolist all
```
## 安装jdk
1. 查看已有的jdk版本
```
[root@DW1 yum.repos.d]# yum search java|grep jdk
```
2. 进行安装
```
[root@DW1 yum.repos.d]# yum install java-1.8.0-openjdk
```
3. 设置环境变量
```
[root@DW1 yum.repos.d]# vi /etc/profile
```
在profile文件夹中添加以下内容
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10.e17_6.x86_64 
JRE_HOME=$JAVA_HOME/jre 
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib 
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin 
export JAVA_HOME JRE_HOME CLASS_PATH PATH 
```
4. 进行验证
```
[root@DW1 yum.repos.d]# java -version
```

## linux常用命令
```
cd #切换路径
ls #查看文件与目录
mv #移动文件或改名
rm #删除文件或目录
mkdir #创建文件夹
...
```
## shell小练习
```
# 写入脚本
[root@DW1 test]# vi test.sh

# 脚本为以下内容
#! /bin/bash

for((i=1;i<=100;i++));
do
echo $i >> output.txt
res=$(( $i % 10 ))
if [ $res = 0 ];then
echo $i >> output.txt
fi
done

# 执行脚本
[root@DW1 test]# base test.sh

# 查看输出
[root@DW1 test]# vi output.txt
```
