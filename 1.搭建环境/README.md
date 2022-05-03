# 搭建环境说明

> 哈工大李治军老师的操作系统实验课程是一门不可多得的好课程，该课程基于linux0.11让学生们编写代码实现：系统启动、系统调用、进程切换、内核级线程等操作系统的基本功能。这个课程在[蓝桥云课](https://www.lanqiao.cn/courses/115)上有对应的在线虚拟机系统。不过，该系统单次只能运行60分钟，如果要连续长时间使用，需要不断延时，错过延时系统就会关闭，之前做的进度无法保存，所以很多小伙伴会选择在自己的电脑上搭载相应的实验环境，方便随时随地进行实验。



## 第一步：在自己电脑上通过VMware安装Ubuntu系统

首先，下载VMware虚拟机软件；

然后，去Ubuntu官网下载一个系统映像iso

最后，通过VMware创建一个Ubuntu系统

具体过程我这里就不展开讲了，这不是本项目的重点，CSDN上有很多这样的文章。

## 第二步：下载哈工大实验课所需的压缩包文件

- **hit-oslab-linux-20110823.tar.gz**: linux-0.11源码、bochs

- **gcc-3.4.tar.gz**:GCC3.4编译器（只能在低版本编译器上编译Linux0.11）

这两个文件已经上传至本目录

## 第三步：准备编译环境

### 1.安装GCC

解压下载下来的GCC3.4，命令如下：

 ``` c 
 tar -zxvf gcc-3.4.tar.gz
 ```


然后进入解压以后的目录，命令如下：

``` c
cd gcc-3.4
```


然后使用ls命令可以看到有amd64和i386两个目录，其中amd64目录下存放的是64位操作系统安装gcc3.4的包，i386目录存放的是32位操作系统安装gcc3.4的包。我的Ubuntu是64位的（具体是Ubuntu Kylin 20.04版本），因此选择amd64目录下的包进行安装，使用如下命令：

```c
cd amd64                #进入该目录
sudo dpkg -i *.deb      #安装所有包
```


安装完成以后，可以输入如下的命令，查看是否安装成功。

```c
gcc-3.4 -v
```

在32位操作系统上安装gcc3.4的命令也是一样的。

### 2.安装编译环境
bootsect.S 和 setup.S 是实模式下运行的 16位代码程序，采用近似于 Intel 的汇编语言语法，并且需要使用 8086 汇编编译器和连接器 as86 和 ld86。而 head.s 则使用一种 AT&T 的汇编语法格式，并且运行在保护模式下，需要用 GNU 的 as（gas）汇编器进行编译。所以，我们需要安装as86、ld86。

搜索as86和ld86，命令如下：

```c
apt-cache search as86 ld86
```

然后，安装bin86，命令如下：

```c
sudo apt install bin86
```

由于是64位系统，还需要安装32位系统的兼容库，命令如下：

```c
sudo apt install libc6-dev-i386
```

### 3.编译linux0.11源码

首先解压下载下来的hit-oslab-linux-20110823.tar.gz，命令如下：

```c
tar -zxvf hit-oslab-linux-20110823.tar.gz
```

解压之后，得到如下的文件:

![image-20220503075845488](https://cdn.jsdelivr.net/gh/ChaochengZhong/pic_respository@main/img/image-20220503075845488.png)

进入linux-0.11目录，编译源代码。使用如下命令

```c
make all
```

> 如果系统提示找不到`make`命令怎么办呢？首先是执行`sudo apt-get install make`，如果执行完这条命令还是找不到`make`命令，那就继续执行`sudo apt-get update`以及`sudo apt-get upgrade`，最后再执行`sudo apt-get install make`应该就能成功了。



最后一行出现`sync`即代表编译成功，结果如下图所示：

![image-20220502232645194](https://cdn.jsdelivr.net/gh/ChaochengZhong/pic_respository@main/img/image-20220502232645225.png)



之后，在oslab目录下运行run会出错，这是因为我们缺少一些东西，因此，安装它们。命令如下：

```c
sudo apt install libsm6:i386
sudo apt install libx11-6:i386
sudo apt install libxpm4:i386
```

安装完上面的这些依赖库之后，输入命令`./run`，就会看到bochs加载Linux0.11成功，界面如下：

![image-20220503080108115](https://cdn.jsdelivr.net/gh/ChaochengZhong/pic_respository@main/img/image-20220503080108115.png)

至此，我们就完成了实验的环境搭建，后面的实验都可以在这上面完成。



## 参考资料

[1] [中国大学MOOC《操作系统》李治军 哈尔滨工业大学](https://www.icourse163.org/course/HIT-1002531008?tid=1003635010)
[2] 《Linux内核完全注释》赵炯
[3] [源码及实验环境下载地址](https://www.icourse163.org/learn/HIT-1002531008?tid=1003635010#/learn/custom?id=1004066001)

https://hoverwinter.gitbooks.io/hit-oslab-manual/content/environment.html