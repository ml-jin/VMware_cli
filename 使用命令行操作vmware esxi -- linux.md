# 使用命令行操作vmware esxi -- linux

[TOC]

为实现自动化，发现了两种命令行工具，一是开启vmware esxi后用xshell等连接工具去连接esxi后台；二是安装powercli连接。本文将介绍一些常用的命令去操作vmware esxi

------

　　本文使用xshell连接esxi后台，可以看到是一个典型的linux操作系统。

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220182733794-1460322141.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220182733794-1460322141.png)

## 1、查看esxi版本

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183052068-582860129.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183052068-582860129.png)

## 2、修改vmware esxi 的root用户的密码

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183135479-160464111.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183135479-160464111.png)

## 3、获取所有的虚机

　　其中vmid是虚机的序号，通过以下信息，我们可以看到虚机序号、虚机的名称、虚机磁盘所在位置、虚机操作系统、创建虚机时使用的vmware版本以及描述信息。

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183254856-1001862657.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183254856-1001862657.png)

## 4、开启虚机

　　　　1表示的是虚机的序号

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183617277-153494928.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183617277-153494928.png)

## 5、关闭虚机

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183729682-821835723.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183729682-821835723.png)

## 6、获取虚机的状态

 [![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183823094-1199899189.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220183823094-1199899189.png)

## 7、查看vim-cmd vmsvc 支持的命令集

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190221091746231-69497284.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190221091746231-69497284.png)

## 8、判断虚机是否真正在运行

　　虚机其实是运行在linux操作系统上的一个进程，若看到该进程在运行，那么可以判断虚机是运行状态

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184036885-595605838.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184036885-595605838.png)

## 9、列出当前运行的虚机

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184211045-1463676613.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184211045-1463676613.png)

## 10、根据world id 关闭虚机

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184454891-1198448663.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184454891-1198448663.png)

```
`esxcli vm process ``kill` `--``type``=[soft,hard,force] --world-``id``=WorldNumber<br>``注意：有三种关闭虚拟机的方法，Soft 程度最低，hard 为立即执行，如果依然不能关闭，则可以使用force 模式`
```

## 11、克隆虚机

　　首先需要关机

　　磁盘存放的目录一般位于cd /vmfs/volumes/磁盘名称/ 下，进入需要克隆的虚机目录下

[![img](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184839629-1685460914.png)](https://img2018.cnblogs.com/blog/1489604/201902/1489604-20190220184839629-1685460914.png)

　　首先创建一个存放文件的目录，将文件克隆到指定的位置。

　　-i 指定的是原虚机vmdk文件的位置，低版本肯不存在vmdk文件

　　-d thin　　指定克隆出来的虚机磁盘格式为thin

```
`vmkfstools -i win``/win``.vmdk c``/winclone``.vmdk -d thin`
```

## 12、获取虚机vmid和虚机ip

``` 
获取虚机的vmid    ``vim-cmd vmsvc``/getallvms` `| ``grep` `xa-lsr-billubuntu | ``awk` `'{print $1}'``        ` `获取虚机的ip``    ``vim-cmd vmsvc``/get``.summary 27 | ``grep` `ipAddress | ``awk` `-F ``'[\t",]'`  `'{print $2}'`
```

[ref:](https://www.cnblogs.com/zqj-blog/p/10408465.html) 