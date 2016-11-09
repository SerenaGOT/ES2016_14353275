# 在ROS的基础上安装cartographer

## 1.安装依赖项

```sh
sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev
```



## 2.安装ceres solver，选择1.11版本

```sh
git clone https://github.com/hitcm/ceres-solver-1.11.0.git
cd ceres-solver-1.11.0
mkdir build
cd build
cmake ..
make –j
sudo make install
```

结果如下：

![](http://www.serena.pub/wp-content/uploads/2016/11/cart1.png)



## 3.安装cartographer

安装路径随意

```sh
git clone https://github.com/hitcm/cartographer.git
cd cartographer/build
cmake .. -G Ninja
ninja
ninja test
sudo ninja install
```

运行test的结果如下

![](http://www.serena.pub/wp-content/uploads/2016/11/cart3.png)

运行完install之后如下：

![](http://www.serena.pub/wp-content/uploads/2016/11/cart4.png)



 ## **3.安装cartographer_ros。**

退出之前安装的文件，回到安装文件根目录之外

找到catkin_ws

```sh
cd catkin_ws/src
```

把原来的cartographer_ros删掉，重新下载修改的文件

```sh
git clone https://github.com/hitcm/cartographer_ros.git
```

然后退出src文件夹编译

```sh
cd ..
catkin_make
```

到这里所有安装步骤都完成了。



## 4.数据下载

选择了2D的数据（470M）放入Downloads文件夹下。

```sh
wget https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag
```

下载完之后运行launch文件即可运行(路径根据实际设置)

```sh
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
```

运行结果大致如图下的效果 ![cartRes](http://www.serena.pub/wp-content/uploads/2016/11/cartRes.jpg)

