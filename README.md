# VINS_Mono
笔者运行VINS_Mono的笔记

VINS_Mono运行
环境:ros-noetic ceres-solver1.14.0 opencv3.4.16
主要是环境的问题，编译的话没什么大问题，装ceres-solver1.14.0才能跑VINS_Mono，因为这个报错,需要卸载自己原有的ceres-solver2.0.2，安装ceres-solver1.14.0需要依赖opencv3，所以把opencv4卸载重新安装opencv3，再安装ceres-solver1.14.0，最后编译显示的关于ceres的报错就没了。
<1>卸载现有的ceres与opencv
因为本人当前使用的版本是ceres-solver2.0.0与opencv4.7.0，要换成ceres-solver1.14.0才能运行VINS-Mono，而安装ceres-solver1.14.0要依赖opencv3，所以opencv4也要卸载重装为opencv3版本，如下步骤
（1）卸载ceres-solver2.0.0
分两次输入以下两行语句
sudo rm -r /usr/local/lib/cmake/Ceres
sudo rm -r /usr/local/include/ceres /usr/local/lib/libceres.a
至此ceres-solver2.0.0卸载完成
（2）卸载opencv4.7.0
find . -name "*opencv*" 
sudo rm -rf
sudo apt-get remove libopencv-*
（3）重装opencv3.4.16
添加依赖
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev
sudo apt-get install libgtk2.0-dev
sudo apt-get install pkg-config
其中的我都有，所以显示的是安装0，升级0
下载.zip
网址https://opencv.org/releases/
找到3.4.16→Sources下载.zip
解压缩之后
mkdir build
cd build
sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
sudo make -j12
sudo make install
至此，安装成功。
（4）安装ceres-solver1.14.0
下载压缩包网址https://codeload.github.com/ceres-solver/ceres-solver/tar.gz/refs/tags/1.14.0
添加依赖项
sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3.1.2 libgflags-dev libgoogle-glog-dev libgtest-dev
倘若出现无法定位包libcxsoarse3.1.2的问题的解决方法
//第一步，打开sources.list
sudo gedit /etc/apt/sources.list
//第二步，将下面的源粘贴到最上方sources.list
deb http://cz.archive.ubuntu.com/ubuntu trusty main universe 
//第三步，更新源
sudo apt-get update
//第四步，重新输入依赖项安装命令安装依赖项
sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3.1.2 libgflags-dev libgoogle-glog-dev libgtest-dev
我没有出现这个问题，因为打开要改的.list文件里面要改的语句与文中的一致
安装步骤
解压文件夹
tar zxf ceres-solver1.14.0.tar.gz（或者在文件夹下直接解压到此处也可以)
mkdir ceres-bin
cd ceres-bin
cmake  ceres-solver1.14.0
make -j12
make test
make install
至此，ceres-solver1.14.0下载完成。
编译中出现的有关ceres的报错不复存在，其实ceres报错只有一句，解决这个我花了4个小时……
后面的是关于.cc代码中的关于CV语句的报错,这个错误是最多的，其实经常遇到，是opencv版本迭代导致底层变量名字变更了，需要修改变量名或者添加头文件来解决
修改的方法是如下两个
1.往.cc中添加头文件
比如CV_GRAY2RGBCV_GRAY2RGB等报错
添加#include <opencv2/imgproc/types_c.h>
又比如CV_AA报错
添加
#include <opencv2/imgproc/imgproc_c.h>
2.修改变量名
比如CV_GRAY2RGB其实也可以用这个方法来修改
CV_GRAY2RGB→cv::COLOR_GRAY2RGB
又比如CV_FONT_HERSHEY_SIMPLEX
CV_FONT_HERSHEY_SIMPLEX→cv::FONT_HERSHEY_SIMPLEX

一些指令
查看opencv版本 opencv_version
查看eigen版本 pkg-config --modversion eigen3
查看ceres版本 去下载文件夹那里看package.xml文件
