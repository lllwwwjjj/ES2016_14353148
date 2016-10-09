## DOL开发环境配置
***
* Description
	* DOL 框架描述
* How to install 
	* DOL 安装笔记
* Experimental experience
	* 实验感想、实验心得
***
### Description
##### DOL框架描述
分布式操作层*DOL(Distributed Operation Layer)*是一个用于管理并行程序的软件框架。DOL允许基于卡恩流程网络计算模型指定应用并基于*SystemC*语言设计出一个仿真引擎。不仅如此，DOL还提供了基于XML的规范格式来描述在多处理器系统上并行程序的实现，包括绑定和索引。

![](http://i1.piimg.com/567571/a4ab3c3796f64dc5.png =600x300)
### How to install
##### DOL安装笔记
本次实验环境在linux下进行，建议使用虚拟机安装Ubuntu

**下载安装Ubuntu虚拟机**

推荐虚拟机管理软件 VMware 和 VirtualBox 等。

VMware： [安装教程](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html/) VirtualBox教程：[安装教程](http://example.net/)

推荐安装 Ubuntu 14.04 [下载地址](http://www.ubuntu.com/download/desktop/)

**安装一些必要的环境**

更新下载源
>sudo apt-get update

安装Ant工具，Ant是一种基于Java的build工具，用Java的类来扩展。Ant本身就是这样一个流程脚本引擎，用于自动化调用程序完成项目的编译，打包，测试等。Ant优点在于它的跨平台性，操作简单，容易维护和书写，结构清晰，可以集成到开发环境中。

>sudo apt-get install ant

安装Java用来编译和执行Java代码
>sudo apt-get install openjdk-7-jdk

**下载文件**

下载文件SystemC和DOL
>	sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
>	
>	sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
**解压文件**

新建dol的文件夹
>mkdir dol

将dolethz.zip解压到 dol文件夹中

>unzip dol_ethz.zip -d dol

解压systemc
>tar -zxvf systemc-2.3.1.tgz

**编译systemc**

解压后进入systemc-2.3.1的目录下
>	cd systemc-2.3.1
新建一个临时文件夹objdir
>	mkdir objdir
进入该文件夹objdir
>	cd objdir
运行configure(能根据系统的环境设置一下参数，用于编译)
>	../configure CXX=g++ --disable-async-updates

编译
>	sudo make install
编译完后文件目录如下($ cd ..        $ ls)，能看到include, lib-linux64(对于32位系统，这里是lib-linux)

![](http://p1.bqimg.com/567571/9f53644ccbffd8a2.png)

记录当前的工作路径(会输出当前所在路径，记下来，待会有用)
>	pwd

![](http://p1.bqimg.com/567571/9c4564d7c3734478.png)
这里表示我当前的工作路径为 /home/jun/systemc-2.3.1

**编译dol**
进入刚刚dol的文件夹
>	cd ../dol
修改build_zip.xml文件
找到下面这段话，就是说上面编译的systemc位置在哪里，
	<property name="systemc.inc" value="YYY/include"/>	
	<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
把YYY改成上页pwd的结果（注意，对于64位系统的机器，lib-linux要改成lib-linux64）

然后是编译
>	ant -f build_zip.xml all
若成功会显示build successful

![](http://p1.bpimg.com/567571/bf08811d11edcd90.png)

接着可以试试运行第一个例子
进入build/bin/mian路径下
>	cd build/bin/main
然后运行第一个例子
>	sudo ant -f runexample.xml -Dnumber=1

成功结果如图

![](http://p1.bpimg.com/567571/23a9b2a8d58ba660.png)

### Experimental experience
##### 实验心得
DOL的配置过程比较简单，第一次运行第一个例子没有成功因为与中文系统有一点不兼容。在dol/build/bin/main 下 的 runexample.xml 的 215-217 行需要注释或者删掉。
    
    <touch datetime="${touch.time}">
      <fileset dir="example${number}"/>
    </touch>
