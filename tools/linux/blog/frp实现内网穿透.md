### frp实现内网穿透

#### 0. 下载

地址：https://github.com/fatedier/frp/releases

```
#选择对应系统版本下载
wget https://github.com/fatedier/frp/releases/download/v0.34.3/frp_0.34.3_linux_amd64.tar.gz
#解压
tar zxvf filename.tar.gz
```



#### 1. frps服务端配置

- 复制文件到特定目录（可省略）

```
cp frps /usr/local/bin/frps
mkdir /etc/frp
cp frps.ini /etc/frp/frps.ini
```

- 配置文件详解

```
[common]
bind_port        = 7000               # 服务监听端口
bind_addr        = 0.0.0.0             # 监听IP
token            = 123456              # 密钥
dashboard_port   = 7500               # web面板
dashboard_user   = admin               # 面板用户名
dashboard_pwd    = admin               # 面板密码
subdomain_host   = *.your_doming.com   # WEB访问域名绑定（绑定后只能绑定子域名访问）
vhost_http_port  = 10000               # web服务http端口
vhost_https_port = 10001               # web服务https端口
#注：以上配置根据需求设置，最简单的配置只需要前两行，既仅配置服务监听端口，其余按需配置。
```

- 我的配置

```
[common]
bind_port=7000
dashboard_port=7500
dashboard_user=shengkai
dashboard_pwd=123456
```



#### 2. frpc客户端配置

- 复制文件到特定目录（可省略）

```
# 需要先 cd frp 解压目录.
cp frpc /usr/local/bin/frpc
mkdir /etc/frp
cp frpc.ini /etc/frp/frpc.ini
```



- 配置文件详解

```
[common]
server_addr = free.frp.ioiox.com  # 服务器IP或者地址
server_port = 7007                # 服务器提供的端口号
token = www.ioiox.com             # 服务器提供的token

[web1]                            # 为避免错误,一定需更改为比较特殊的名称,不能和服务器端其他配置重名.
type = http                       # http协议
local_ip = 127.0.0.1              # 127.0.0.1指穿透本机,也可以填写内网IP.
local_port = 5000                 # 群晖内网HTTP端口,默认为5000.
custom_domains = nas.ioiox.com    # 填写你的域名

[web2]                            # 为避免错误,一定需更改为比较特殊的名称,不能和服务器端其他配置重名.
type = https                      # https协议
local_ip = 127.0.0.1              # 127.0.0.1指穿透本机,也可以填写内网IP.
local_port = 5001                 # 群晖内网HTTPS端口,默认为5001.
custom_domains = nas.ioiox.com    # 填写你的域名

[OpenVpn]
type = udp                        # 协议
local_ip = 192.168.123.142        # 127.0.0.1指穿透本机,也可以填写内网IP.
local_port = 1194                 # 内网端口
remote_port = 11194               # 远程连接端口vim
```

- 我的配置

```
[common]
server_addr = 121.5.70.227
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
#远程桌面服务
[rdp]
type=tcp
local_ip = 127.0.0.1
local_port=3389
remote_port = 3389          
```



#### 3. nohup后台启动

```
#以客户端为例，服务端同理
# 前台启动
./frpc -c ./frpc.ini

# 后台启动命令
nohup ./frpc -c ./frpc.ini &
```



#### 4. systemctl 开机自启

- 创建服务文件

```
sudo vim /lib/systemd/system/frps.service
```

- 服务内容

```
[Unit]
Description=fraps service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
#启动服务的命令（此处写你的frps的实际安装目录）
ExecStart=/your/path/frps -c /your/path/frps.ini

[Install]
WantedBy=multi-user.target
```

- 我的内容

```
[Unit]
Description=frpc
After=network.target syslog.target
Wants=network.target

[Service]
Type = simple
ExecStart=/usr/local/bin/frpc -c /etc/frp/frpc.ini
ExecStop=/bin/kill $MAINPID

RestartSec = 1min
Killmode = control-group

Restart=always
[Install]
WantedBy=multi-user.target
```

- 使用方法

```
# 启动frps
sudo systemctl start frps
# 自启动
sudo systemctl enable frps
# 重启应用
sudo systemctl restart frps
# 停止应用
sudo systemctl stop frps
# 查看应用的日志
sudo systemctl status frps
```

- 以服务端为例，客户端同理