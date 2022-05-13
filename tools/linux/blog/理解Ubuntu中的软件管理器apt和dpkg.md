## 理解Ubuntu中的软件管理器apt和dpkg

大家都知道在ubuntu下，安装软件经常会用到一个命令就是“apt-get install”，这里的apt命令，其实是linux系统下一个通用的软件包管理器，使用该命令可以很方便的用来安装和卸载软件。然而，很多同学应该也知道，ubuntu下，还有另外一个软件包管理器，叫做dpkg，它也可以实现软件的安装和卸载。那么，它们二者究竟各自负责什么，又有什么区别呢？

### 1、apt命令
Advanced Packaging Tool（apt）是Linux下的一款安装包管理工具，是一个客户/服务器系统。在服务器上先复制所有DEB包（DEB是Debian软件包格式的文件扩展名），然后用APT的分析工具（genbasedir）根据每个DEB 包的包头（Header）信息对所有的DEB包进行分析，并将该分析结果记录在一个文件中，这个文件称为DEB 索引清单，APT服务器的DEB索引清单置于base文件夹内。一旦APT 服务器内的DEB有所变动，一定要使用genbasedir产生新的DEB索引清单。客户端在进行安装或升级时先要查询DEB索引清单，从而可以获知所有具有依赖关系的软件包，并一同下载到客户端以便安装。

当客户端需要安装、升级或删除某个软件包时，客户端计算机取得DEB索引清单压缩文件后，会将其解压置放于/var/state/apt/lists/，而客户端使用apt-get install或apt-get upgrade命令的时候，就会将这个文件夹内的数据和客户端计算机内的DEB数据库比对，知道哪些DEB已安装、未安装或是可以升级的。

**apt命令的几个缺省路径：**

- 下载的软件存放位置：/var/cache/apt/archives
- 安装后软件默认位置：/usr/share
- 可执行文件位置：/usr/bin
- 配置文件位置：/etc
- 库文件位置：/usr/lib


**常用的apt命令集：**
```
sudo apt-get install 			    # package 安装包
sudo apt-get reinstall 			    # package - - reinstall 重新安装包
sudo apt-get remove 			    # package 删除包(保留配置文件)
sudo apt-get remove --purge 		# package 删除包(不保留配置文件）
sudo apt-get autoremove --purge 	# package 删除现在不需要的依赖包
sudo apt-get update 			    # 更新源
sudo apt-get upgrade 			    # 更新已安装的包
sudo apt-get dist-upgrade 		    # 升级系统
```

### 2、dpkg命令
Ubuntu是基于Debian的Linux系统，而Debian系统的软件是使用APT和dpkg进行管理。dpkg是"Debian Packager"的简写，是一个底层的软件包管理工具。

可以输入`dpkg -l`来查看软件的状态，输入`dpkg -P`来卸载软件。因为`dpkg --remove`只是删除安装的文件，但不删除配置文件。而`dpkg --purge`则安装文件和配置文件都删除。

**常用的dpkg命令：**
```
dpkg -i package.deb		# 安装一个 Debian 软件包，如手动下载的文件。
dpkg -c package.deb		# 列出 package.deb 的内容。
dpkg -I package.deb		# 从 package.deb 中提取包信息。
dpkg -r package			# 移除一个已安装的包。
dpkg -P package			# 完全清除一个已安装的包。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。
dpkg -L package			# 列出 package 安装的所有文件清单。
dpkg -s package			# 显示已安装包的信息。
dpkg -reconfigure package	# 重新配制一个已经安装的包，如果它使用的是 debconf (debconf 为包安装提供了一个统一的配置界面)。
dpkg -S package			# 查看软件在哪个包里；
```

### 3、区别
apt是会解决和安装模块的依赖问题，并会咨询软件仓库，是在线安装。
dpkg只能安装本地的deb文件，不会关心Ubuntu的软件仓库内的软件，不会解决模块的依赖关系。
两者的区别是dpkg绕过apt包管理数据库对软件包进行操作，所以你用dpkg安装过的软件包用apt可以再安装一遍，系统不知道之前安装过了，将会覆盖之前dpkg的安装。
