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
`三台虚拟机内存总和不能大于实体机` 

4. 分配光驱并加载镜像文件
5. 设置网络（桥接网卡）
6. 启动虚拟机  
7. 设置网络   
手动分配时偶尔会出现问题：`在输入ip的页面，不能进行任何操作`  
`因此干脆不手动分配ip，等系统会自动分配，如果分配不合理（我没遇到），可以再vi进入设置文件修改`  

8. 分区  
直接自动分配即可

## 在本机使用Xshell连接虚拟机
1. Xshell6下载  
下载链接：https://www.netsarang.com/en/xshell/  

2. 设置好虚拟机ip  
`因为我们前面没有手动分配ip，因此需要先查看当前虚拟机的ip`  
ip不要设置错误，端口用默认的，不需要改动  

```
[root@DW1 ~]# ip addr
```    

3. 进行连接

## CentOS7配置阿里云yum源和EPEL源
1. 备份系统的yum源
```
[root@DW1 ~]# cd /etc/yum.repos.d/
[root@DW1 yum.repos.d]# mkdir repo_bak

#要先安装wget再备份系统yum源，原生CentOS没有wget，后续无法下载阿里云源
[root@DW1 yum.repos.d]# yum -y install wget 
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: ftp.twaren.net
 * extras: ftp.twaren.net
 * updates: ftp.twaren.net
base                                                                                | 3.6 kB  00:00:00     
extras                                                                              | 3.4 kB  00:00:00     
updates                                                                             | 3.4 kB  00:00:00     
(1/4): base/7/x86_64/group_gz                                                       | 166 kB  00:00:00     
(2/4): extras/7/x86_64/primary_db                                                   | 205 kB  00:00:00     
(3/4): updates/7/x86_64/primary_db                                                  | 6.5 MB  00:00:01     
(4/4): base/7/x86_64/primary_db                                                     | 6.0 MB  00:00:04     
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-18.el7_6.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================
 Package              Arch                   Version                         Repository               Size
===========================================================================================================
Installing:
 wget                 x86_64                 1.14-18.el7_6.1                 updates                 547 k

Transaction Summary
===========================================================================================================
Install  1 Package

Total download size: 547 k
Installed size: 2.0 M
Downloading packages:
warning: /var/cache/yum/x86_64/7/updates/packages/wget-1.14-18.el7_6.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Public key for wget-1.14-18.el7_6.1.x86_64.rpm is not installed
wget-1.14-18.el7_6.1.x86_64.rpm                                                     | 547 kB  00:00:00     
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-6.1810.2.el7.centos.x86_64 (@anaconda)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-18.el7_6.1.x86_64                                                             1/1 
  Verifying  : wget-1.14-18.el7_6.1.x86_64                                                             1/1 

Installed:
  wget.x86_64 0:1.14-18.el7_6.1                                                                            

Complete!
[root@DW1 yum.repos.d]# mv *.repo repo_bak/
[root@DW1 yum.repos.d]# ls
repo_bak
```
2. 下载新的CentOS-Base.repo 到/etc/yum.repos.d/
```
[root@DW1 yum.repos.d]# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
--2019-07-26 04:47:02--  http://mirrors.aliyun.com/repo/Centos-7.repo
Resolving mirrors.aliyun.com (mirrors.aliyun.com)... 47.246.5.226, 47.246.5.229, 47.246.5.227, ...
Connecting to mirrors.aliyun.com (mirrors.aliyun.com)|47.246.5.226|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2523 (2.5K) [application/octet-stream]
Saving to: ‘/etc/yum.repos.d/CentOS-Base.repo’

100%[=================================================================>] 2,523       --.-K/s   in 0s      

2019-07-26 04:47:03 (393 MB/s) - ‘/etc/yum.repos.d/CentOS-Base.repo’ saved [2523/2523]

[root@DW1 yum.repos.d]# ls
CentOS-Base.repo  repo_bak
```
3. 清除缓存
```
[root@DW1 yum.repos.d]# yum clean all
Loaded plugins: fastestmirror
Cleaning repos: base extras updates
Cleaning up list of fastest mirrors

```
4. 生成新的缓存
```
[root@DW1 yum.repos.d]# yum makecache
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
base                                                                                | 3.6 kB  00:00:00     
extras                                                                              | 3.4 kB  00:00:00     
updates                                                                             | 3.4 kB  00:00:00     
base/7/x86_64/primary_db       FAILED                                          
http://mirrors.cloud.aliyuncs.com/centos/7/os/x86_64/repodata/6614b3605d961a4aaec45d74ac4e5e713e517debb3ee454a1c91097955780697-primary.sqlite.bz2: [Errno 14] curl#6 - "Could not resolve host: mirrors.cloud.aliyuncs.com; Unknown error"
Trying other mirror.
(1/12): base/7/x86_64/group_gz                                                      | 166 kB  00:00:01     
(2/12): extras/7/x86_64/filelists_db                                                | 246 kB  00:00:01     
(3/12): extras/7/x86_64/primary_db                                                  | 205 kB  00:00:00     
(4/12): base/7/x86_64/other_db                                                      | 2.6 MB  00:00:02     
(5/12): extras/7/x86_64/other_db                                                    | 127 kB  00:00:00     
(6/12): updates/7/x86_64/filelists_db                                               | 4.9 MB  00:00:01     
(7/12): updates/7/x86_64/prestodelta                                                | 857 kB  00:00:02     
(8/12): updates/7/x86_64/other_db                                                   | 672 kB  00:00:00     
(9/12): updates/7/x86_64/primary_db                                                 | 6.5 MB  00:00:02     
(10/12): base/7/x86_64/primary_db                                                   | 6.0 MB  00:00:02     
base/7/x86_64/filelists_db     FAILED                                          
http://mirrors.aliyuncs.com/centos/7/os/x86_64/repodata/a0ec5a4708a1026db100d4799c404c9ed48a9371a4bab234a1355f86628a244a-filelists.sqlite.bz2: [Errno 12] Timeout on http://mirrors.aliyuncs.com/centos/7/os/x86_64/repodata/a0ec5a4708a1026db100d4799c404c9ed48a9371a4bab234a1355f86628a244a-filelists.sqlite.bz2: (28, 'Connection timed out after 30001 milliseconds')
Trying other mirror.
extras/7/x86_64/prestodelta    FAILED                                          
http://mirrors.aliyuncs.com/centos/7/extras/x86_64/repodata/5bfd3d5f07606011226e556e87d978ca1dfe51a63e18d793182900d5bbc702b5-prestodelta.xml.gz: [Errno 12] Timeout on http://mirrors.aliyuncs.com/centos/7/extras/x86_64/repodata/5bfd3d5f07606011226e556e87d978ca1dfe51a63e18d793182900d5bbc702b5-prestodelta.xml.gz: (28, 'Connection timed out after 30001 milliseconds')
Trying other mirror.
(11/12): extras/7/x86_64/prestodelta                                                |  65 kB  00:00:00     
(12/12): base/7/x86_64/filelists_db                                                 | 7.1 MB  00:00:02     
Metadata Cache Created

```
5. 安装EPEL源
```
[root@DW1 yum.repos.d]# yum list | grep epel-release
epel-release.noarch                         7-11                       extras 

[root@DW1 yum.repos.d]# yum install -y prep epel-release
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
No package prep available.
Resolving Dependencies
--> Running transaction check
---> Package epel-release.noarch 0:7-11 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================
 Package                       Arch                    Version               Repository               Size
===========================================================================================================
Installing:
 epel-release                  noarch                  7-11                  extras                   15 k

Transaction Summary
===========================================================================================================
Install  1 Package

Total download size: 15 k
Installed size: 24 k
Downloading packages:
epel-release-7-11.noarch.rpm                                                        |  15 kB  00:00:01     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : epel-release-7-11.noarch                                                                1/1 
  Verifying  : epel-release-7-11.noarch                                                                1/1 

Installed:
  epel-release.noarch 0:7-11                                                                               

Complete!

[root@DW1 yum.repos.d]# ls
CentOS-Base.repo  epel.repo  epel-testing.repo  repo_bak
```
6. 再次清除和生成缓存
```
[root@DW1 yum.repos.d]# yum clean all
Loaded plugins: fastestmirror
Cleaning repos: base epel extras updates
Cleaning up list of fastest mirrors

[root@DW1 yum.repos.d]# yum makecache
Loaded plugins: fastestmirror
Cleaning repos: base epel extras updates
Cleaning up list of fastest mirrors
[root@DW3 yum.repos.d]# yum makecache
Loaded plugins: fastestmirror
Determining fastest mirrors
epel/x86_64/metalink                                                                | 5.7 kB  00:00:00     
 * base: mirrors.aliyun.com
 * epel: mirror01.idc.hinet.net
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
base                                                                                | 3.6 kB  00:00:00     
epel                                                                                | 5.3 kB  00:00:00     
extras                                                                              | 3.4 kB  00:00:00     
updates                                                                             | 3.4 kB  00:00:00     
(1/19): base/7/x86_64/group_gz                                                      | 166 kB  00:00:00     
(2/19): base/7/x86_64/filelists_db                                                  | 7.1 MB  00:00:01     
(3/19): base/7/x86_64/other_db                                                      | 2.6 MB  00:00:00     
(4/19): epel/x86_64/group_gz                                                        |  88 kB  00:00:00     
(5/19): epel/x86_64/prestodelta                                                     |  618 B  00:00:00     
(6/19): epel/x86_64/updateinfo                                                      | 993 kB  00:00:01     
(7/19): epel/x86_64/filelists_db                                                    |  12 MB  00:00:03     
(8/19): extras/7/x86_64/filelists_db                                                | 246 kB  00:00:00     
(9/19): extras/7/x86_64/prestodelta                                                 |  65 kB  00:00:00     
(10/19): extras/7/x86_64/primary_db                                                 | 205 kB  00:00:00     
(11/19): extras/7/x86_64/other_db                                                   | 127 kB  00:00:00     
(12/19): updates/7/x86_64/filelists_db                                              | 4.9 MB  00:00:00     
(13/19): epel/x86_64/updateinfo_zck                                                 | 1.5 MB  00:00:02     
(14/19): updates/7/x86_64/prestodelta                                               | 857 kB  00:00:00     
(15/19): updates/7/x86_64/other_db                                                  | 672 kB  00:00:00     
(16/19): updates/7/x86_64/primary_db                                                | 6.5 MB  00:00:01     
(17/19): base/7/x86_64/primary_db                                                   | 6.0 MB  00:00:07     
(18/19): epel/x86_64/other_db                                                       | 3.3 MB  00:00:07     
(19/19): epel/x86_64/primary_db                                                     | 6.8 MB  00:00:08     
Metadata Cache Created
```
7. 查看所有的yum源
```
[root@DW1 yum.repos.d]# yum repolist enabled
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * epel: mirror01.idc.hinet.net
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
repo id                            repo name                                                         status
base/7/x86_64                      CentOS-7 - Base - mirrors.aliyun.com                              10,019
epel/x86_64                        Extra Packages for Enterprise Linux 7 - x86_64                    13,327
extras/7/x86_64                    CentOS-7 - Extras - mirrors.aliyun.com                               419
updates/7/x86_64                   CentOS-7 - Updates - mirrors.aliyun.com                            2,303
repolist: 26,068

[root@DW1 yum.repos.d]# yum repolist all
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * epel: mirror01.idc.hinet.net
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
repo id                       repo name                                                     status
base/7/x86_64                 CentOS-7 - Base - mirrors.aliyun.com                          enabled: 10,019
centosplus/7/x86_64           CentOS-7 - Plus - mirrors.aliyun.com                          disabled
contrib/7/x86_64              CentOS-7 - Contrib - mirrors.aliyun.com                       disabled
epel/x86_64                   Extra Packages for Enterprise Linux 7 - x86_64                enabled: 13,327
epel-debuginfo/x86_64         Extra Packages for Enterprise Linux 7 - x86_64 - Debug        disabled
epel-source/x86_64            Extra Packages for Enterprise Linux 7 - x86_64 - Source       disabled
epel-testing/x86_64           Extra Packages for Enterprise Linux 7 - Testing - x86_64      disabled
epel-testing-debuginfo/x86_64 Extra Packages for Enterprise Linux 7 - Testing - x86_64 - De disabled
epel-testing-source/x86_64    Extra Packages for Enterprise Linux 7 - Testing - x86_64 - So disabled
extras/7/x86_64               CentOS-7 - Extras - mirrors.aliyun.com                        enabled:    419
updates/7/x86_64              CentOS-7 - Updates - mirrors.aliyun.com                       enabled:  2,303
repolist: 26,068

```
## 安装jdk
1. 查看已有的jdk版本
```
[root@DW1 yum.repos.d]# yum search java|grep jdk
```
2. 进行安装
```
# 这里有个宇宙深坑
# 版本要选带devel的，如java-1.8.0-openjdk-devel.x86_64，不能简单输入java-1.8.0-openjdk
# 否则装完只有jre,没有bin,lib等等
[root@DW1 yum.repos.d]# yum install java-1.8.0-openjdk-devel.x86_64

```
3. 设置环境变量
```
[root@DW1 yum.repos.d]# vi /etc/profile
```
在profile文件夹中添加以下内容
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10-0.el7_6.x86_64  
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
PATH=$PATH:$JAVA_HOME/bin 
export JAVA_HOME CLASS_PATH PATH 
```
4. 进行验证
```
[root@DW1 yum.repos.d]# java -version
openjdk version "1.8.0_222"
OpenJDK Runtime Environment (build 1.8.0_222-b10)
OpenJDK 64-Bit Server VM (build 25.222-b10, mixed mode)
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
```
```
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
```
```
# 执行脚本
[root@DW1 test]# base test.sh

# 查看输出
[root@DW1 test]# vi output.txt
```
