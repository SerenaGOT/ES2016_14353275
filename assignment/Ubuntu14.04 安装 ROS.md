

# Ubuntu14.04 安装 ROS

本次安装完全按照[官网的教程](http://wiki.ros.org/jade/Installation/Ubuntu)实现。



## 1.确认Ubuntu的配置

Ubuntu的基本配置都不需要改，直接进入安装步骤



## 2.设置好sources.list

1) 在命令行输入以下指令

```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```



2) 设置好密钥

```sh
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```



## 3.进入安装

1）首先更新指令

```sh
sudo apt-get update
```

2) 选择安装全部的包的指令

```sh
sudo apt-get install ros-jade-desktop-full
```



## 4.初始化rosdep

````sh
sudo rosdep init
rosdep update
````



## 5.设置环境

```sh
echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc


// If you just want to change the environment of your current shell, you can type: 
source /opt/ros/jade/setup.bash
```



## 6.安装 rosinstall

```sh
sudo apt-get install python-rosinstall
```



## 7.获取source的源码

官网上的指令执行会报错

```sh
apt-get source ros-jade-laser-pipeline
```

如果获取不了可以直接去github下载。点击[此处](https://github.com/ros-perception/laser_pipeline)获取。



## 8.验证ros的安装

首先可以查找ros的软件包相关信息（软件包都放在/opt/ros/）

```sh
rospack find stereo_msgs
```

 显示可以找到文件包。

![](http://www.serena.pub/wp-content/uploads/2016/11/1.png)

然后我们可以查看环境变量

```sh
export | grep ROS
```

可以看到

![](http://www.serena.pub/wp-content/uploads/2016/11/2.png)

我们还可以检查ros系统情况

```sh
roswtf
```

可以看到

![](http://www.serena.pub/wp-content/uploads/2016/11/3.png)



**至此ROS的验证就基本完成了。**

