### Ubuntu下用户的创建与删除

#### 一、查看用户信息和密码

##### 1、查看用户信息

```ruby
mcdx@ubuntu:~$ cat /etc/passwd  # 用户信息在此文件中
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
... ...
mcdx:x:1000:1000:ubuntu,,,:/home/mcdx:/bin/bash
mysql:x:121:129:MySQL Server,,,:/nonexistent:/bin/false
mongodb:x:122:65534::/home/mongodb:/bin/false
redis:x:123:131::/var/lib/redis:/bin/false
```

文件中每行记录用冒号 `:` 分隔为 7 个字段，从左到右具体含义是：

> 用户名：密码占位符（x 表示用户需要密码登录）：用户标识号（`UID`）：
> 组标识号（`GID`）：注释性描述：主目录：登录的 `shell`

##### 2、查看密码文件，注意密码文件是 shadow 用户文件是 passwd

```ruby
mcdx@ubuntu:~$ sudo cat /etc/shadow  # 需要加 sudo
root:!:17686:0:99999:7:::
daemon:*:17379:0:99999:7:::
bin:*:17379:0:99999:7:::
... ...
```

文件中每行记录用冒号 `:` 分隔为 9 个字段，从左到右具体含义是：

> 用户名：加密口令：最后一次修改时间：最短有效天数：最长有效天数：
> 过期前的警告时间：不活动时间：用户失效时间：暂时保留未使用

##### 3. 相关文件

```
/etc/passwd - 使 用 者 帐 号 资 讯，可以查看用户信息
/etc/shadow - 使 用 者 帐 号 资 讯 加 密
/etc/group - 群 组 资 讯
/etc/default/useradd - 定 义 资 讯
/etc/login.defs - 系 统 广 义 设 定
/etc/skel - 内 含 定 义 档 的 目 录
```



#### 二、创建用户

- 创建用户有两条命令：adduser 和 useradd，对应着两条删除用户的命令：deluser 和 userdel。
  这两种命令之间的区别：
  **1. adduser：**会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。
  **2. useradd：**需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定shell版本。



##### 1. adduser（推荐）

```
#命令
sudo adduser dat
#根据下面提示输入信息即可
#1.(Retype) New password: 输入并确认用户密码
#2.Enter the new value, or press ENTER for the default：可以一路回车，第一个是登陆界面显示的用户名，可以自行更改。
#3.Is the information correct? [Y/n]： Y
```

![image-20210317094226134](C:\Users\shengkai\AppData\Roaming\Typora\typora-user-images\image-20210317094226134.png)



##### 2. useradd

- **注意：** 在使用useradd命令创建新用户时，不会为用户创建主目录，不会为用户指定shell版本，不会为用户创建密码。

- 创建

  ```
  #不使用任何参数选项创建用户：
  sudo useradd dat
  
  #为用户指定(修改)登录密码：
  sudo passwd dat
  
  #为用户指定命令解释程序(通常为/bin/bash)：
  sudo usermod -s /bin/bash dat
  
  #为用户指定用户主目录：
  sudo usermod -d /home/dat dat
  
  ```

  

- 指定参数的 **useradd**

  常用命令行选项：

  -d： 指定用户的主目录
  -m： 如果存在不再创建，但是此目录并不属于新创建用户；如果主目录不存在，则强制创建； -m和-d  一块使用。

  -s： 指定用户登录时的shell版本

  -M： 不创建主目录

  ```
  sudo useradd -d "/home/dat" -m -s "/bin/bash" dat
  #解释：
  #-d “/home/dat” ：就是指定/home/dat为主目录
  #-m 就是如果/home/dat不存在就强制创建
  #-s 就是指定shell版本
  ```

  

#### 三、删除用户

##### 1. deluser

```
#只删除用户:
sudo deluser dat

#连同用户的主目录和邮箱一起删除:
sudo deluser --remove-home dat

#连同用户拥有的所有文件删除
sudo deluser --remove-all-files dat
```



##### 2. userdel（推荐）

```
#只删除用户：
sudo userdel dat

#连同用户主目录一起删除：
sudo deluser -r dat

#强制删除
sudo deluser -f dat
```



参考：

[Ubuntu下创建新用户](https://blog.csdn.net/taolusi/article/details/81304057)