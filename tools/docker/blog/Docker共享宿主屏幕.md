### Docker共享宿主屏幕

原理：挂载方式是在使用image创建docker容器时，通过-v参数设置docker内外路径挂载，使显示xserver设备的socket文件在docker内也可以访问。并通过-e参数设置docker内的DISPLAY参数和宿主机一致。

```bash
# 安装xserver
sudo apt install x11-xserver-utils

# 许可所有用户都可以访问xserver
xhost +

# 创建docker容器
# -e: 设置docker内的DISPLAY参数和宿主机一致
# --net: 设置网络模式
docker run -itd \
	--net=host
	-e DISPLAY=$DISPLAY \
	-v /tmp/.X11-unix/:/tmp/.X11-unix/ \
	ubuntu:20.04
	
# 在docker中检验
sudo apt-get update
sudo apt-get install xarclock
xarclock

```

[参考一](https://cloud.tencent.com/developer/article/1541718)

[参考二](https://blog.csdn.net/Frank_Abagnale/article/details/80243939)



### 安装Ros仿真包

```bash
# 安装 turtlesim
sudo apt-get install ros-$(rosversion -d)-turtlesim

# 安装 rqt_graph
sudo apt-get install ros-$(rosversion -d)-rqt
sudo apt-get install ros-$(rosversion -d)-rqt-common-plugins
```

[参考三](https://www.jianshu.com/p/5b8893479874)





### ROS包更新

```bash
# 输入 sudo rosdep init后，提示找不到命令
sudo apt-get install python3-rosdep

# 初始化
sudo rosdep init

# 更新
sudo rosdep update
```

#### 1. rosdep init 出错

```
ERROR: connot download default sources list from:
https://row.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d/10-default.list
```

再`/etc/hosts`文件中加入一行：

```bash
# 二选一，各试一下
199.232.28.133 raw.githubusercontent.com

151.101.84.133  raw.githubusercontent.com
```



#### 2. rosdep update 更新出错

**原因：** 网站DNS污染

**原理：**

http://raw.githubusercontent.com实际上就是github的用户数据服务器

rosdep程序下载的就是从github.com/ros/rosdistro这个repo里的yaml文件

虽然http://raw.githubusercontent.com服务器无法访问，但是yaml文件可以直接从git clone的repo中获得



**方法：注意如果你手动把URL改为file路径，需要删除默认URL路径中的/master/，而不是简单的把https://raw.githubusercontent.com/ros 替换为 file:///home/yourname/**



**step1：clone 到  `/home/yourname`** 

```shell
git clone https://github.com/ros/rosdistro.git
```



**step2：修改的文件路径**

```bash
# file1: /home/yourname/rosdistro/rosdep/sources.list.d
# file2: /usr/lib/python3/dist-packages/rosdep2/gbpdistro_support.py
# file3: /usr/lib/python3/dist-packages/rosdep2/rep3.py
# file4: /usr/lib/python3/dist-packages/rosdistro/__init__.py
# file5: /etc/ros/rosdep/sources.list.d/20-default.list

# 修改形式，eg:
https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/sources.list.d
# --->
file:///home/yourname/rosdistro/rosdep/sources.list.d


rosdep update
```



[参考四](https://zhuanlan.zhihu.com/p/365183222)
