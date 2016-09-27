# 基于Ubuntu 14.04  32位的 DOL开发环境配置



## 一.DOL框架描述

DOL全称为**Distributed Operation Layer**, 根据[TIK]( http://www.tik.ee.ethz.ch/~shapes/dol.html) 的定义：

- > The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform.

DOL 有三个基本的功能：

- > **DOL Application Programming Interface:** The DOL defines a set of computation and communication routines that enable the programming of distributed, parallel applications for the SHAPES platform. Using these routines, application programmers can write programs without having detailed knowledge about the underlying architecture. In fact, these routines are subject to further refinement in the hardware dependent software (HdS) layer.

- > **DOL Functional Simulation:** To provide programmers a possibility to test their applications, a functional simulation framework has been developed. Besides functional verification of applications, this framework is used to obtain performance parameters at the application level.

- > **DOL Mapping Optimization:** The goal of the DOL mapping optimization is to compute a set of optimal mappings of an application onto the SHAPES architecture platform. In a first step, XML based specification formats have been defined that allow to describe the application and the architecture at an abstract level. Still, all the information necessary to obtain accurate performance estimates is contained.



## 二.DOL开发环境配置流程

- 本次实验在Mac OSX 上安装VMware Fusion虚拟机，并在虚拟机上用Ubuntu14.04 32位上进行配置

1. 进行DOL配置之前需要先安装一些必要的环境，在Ubuntu打开终端，在命令行输入：

   ```shell
   sudo apt-get update
   sudo apt-get install ant
   sudo apt-get install openjdk-7-jdk
   sudo apt-get install unzip
   ```

   下面简要介绍一下上述安装的工具：（参考于[Ant工具介绍](http://blog.163.com/qiangyongbin2000@126/blog/static/77517819201151653423687/)）

   - __Ant__：一种基于Java的build工具，用Java的类进行扩展，是一个流程脚本引擎，用于自动化调用程序完成项目的编译，打包，测试等。Ant具有很多优点：
     - 跨平台性，基于纯Java语言编写
     - 操作简单，Ant由一个内置任务和可选任务组成，运行时需要xml文件（构建文件）
     - 易于维护、书写，结构简单，可集成到开发环境
   - __Openjdk-7-jdk__ ：在Java平台构建的JAVA开发环境（JDK）的开源版本

2. 开始安装前需要先准备好安装文件，包括systemc-2.3.1.tgz 和dol_ethz.zip，可以先行下载再放入ubuntu的home文件夹中，或者直接在命令行输入下面的代码：

   ```sh
   sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
   sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
   ```
   ​

3. 解压文件

   确认处于home文件目录下，首先创建dol文件夹：

   ```sh
   mkdir dol
   ```

   然后输入下面的指令把dolethz.zip解压到dol文件夹中：

   ```sh
   unzip dol_ethz.zip -d dol
   ```

   接着解压systemc

   ```sh
   tar -zxvf systemc-2.3.1.tgz
   ```

   ​

4. 编译systemc

   解压后进入systems-2.3.1的目录下

   ```sh
   cd systemc-2.3.1
   ```

   新建一个临时文件夹objdir(mkdir 即生成新文件夹指令)

   ```
   mkdir objdir
   ```


   用下面的命令运行configure（使其根据系统环境设置参数，用于编译）

   ```
   ../configure CXX=g++ --disable-async-updates
   ```

   接着我们可以进行编译

   ```sh
   sudo make install
   ```

   编译之后退到上一层目录

   ```sh
   cd ..
   ls
   ```

   文件目录如下（此为32位系统）

   ![](http://p1.bqimg.com/567571/e32c5a14aef40004.png)

   通过下面的指令输出当前路劲，并记下来

   ```
   pwd
   ```

   我的路径是  /home/serena/systemc-2.3.1

   接着进入dol文件夹

   ```sh
   cd ../dol
   ```

   修改build_zip.xml文件，可以使用gedit或者vim，要注意用sudo才有权限修改

   ```sh
   sudo gedit build_zip.xml
   //或者
   sudo vim build_zip.xml 
   ```

   在文件中找到以下两句话，把YYY部分的路径改为之前的pwd保存的结果(32位系统)，对于64位的系统，lib-linux要改成lib-linux64

   ```c
   <property name="systemc.inc" value="YYY/include"/>
   <property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
   ```

5. 编译dol

   继续在当前目录下输入指令

   ```sh
   ant -f build_zip.xml all
   ```

   成功会显示build successful

   然后可以试着运行第一个例子，进入build/bin/mian，运行下面的指令

   ```shell
   cd build/bin/mian
   ant -f runexample.xml -Dnumber=1
   ```

   成功结果如图

   ![](http://p1.bqimg.com/567571/a7262500de344880.png)

   ​

   至此全部配置完成。
