### Ubuntu下程序的后台运行与开机自启

#### 1. 后台运行：nohup和&

- **nohup** 

  不挂断的运行程序（即退出终端，程序也不会终止）。

- **&**

  在后台运行程序。

- **nohup 和 & 的区别** 

  nohup ： nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响运行，注意了nohup没有后台运行的意思；&是指在后台运行，但当用户推出(挂起)的时候，命令自动也跟着退出。

- **nohup command &**

  这样就能使命令永久的在后台执行

- **nohup command > myout.file 2>&1 &** 

  在这个例子中，0 – stdin (standard input)，1 – stdout (standard output)，2 – stderr (standard error) ；2>&1是将标准错误（2）重定向到标准输出（&1），标准输出（&1）再被重定向输入到myout.file文件中。

  

#### 2. 开机自启：systemctl

##### 2.1 创建服务文件

```
sudo vim /lib/systemd/system/myapp.service

#如果/etc/systemd/system/myapp.service有过修改，需要进行更新，执行下面这个命令
systemctl daemon-reload

#查看服务是否被正确识别
systemctl list-unit-files|grep myapp
```

##### 2.2 服务内容: myapp.service

```
[Unit]
#服务描述，写有意义的内容，便于识别
Description=myapp service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
#启动服务的命令（此处写你的frps的实际安装目录）
WorkingDirectory=/myapp_path
ExecStart=/myapp_path/startMyApp.sh
#ExecStart=/your/path/frps -c /your/path/frps.ini

[Install]
WantedBy=multi-user.target
```

##### 2.3 将服务设置成开机自启动

```
#查看系统服务文件是否被识别
# systemctl list-unit-files|grep myapp
--------------------------
myapp.service                      disabled

#自启动
# systemctl enable myapp.service

# systemctl list-unit-files|grep myapp
--------------------------
myapp.service                      enabled

```



##### 2.4 使用方法

```
# 启动myapp服务
sudo systemctl start myapp
# 自启动
sudo systemctl enable myapp
# 重启应用
sudo systemctl restart myapp
# 停止应用
sudo systemctl stop myapp
# 查看应用的日志
sudo systemctl status myapp
```



参考：

[Ubuntu中如何使得程序在后台运行](https://blog.csdn.net/weixin_30919919/article/details/98106625)

[Linux使用systemctl设置程序开机自启动](https://blog.csdn.net/sayyy/article/details/79276575)