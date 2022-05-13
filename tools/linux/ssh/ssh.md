# SSH 速查

[toc]

## 1. 简介

### 1.1 SSH架构
SSH 的软件架构是服务器-客户端模式（Server - Client）。在这个架构中，SSH 软件分成两个部分：向服务器发出请求的部分，称为客户端（client），OpenSSH 的实现为 ssh；接收客户端发出的请求的部分，称为服务器（server），OpenSSH 的实现为 sshd。

### 1.2 加密传输

SSH会自动加密和解密所有SSH客户端与服务端之间的网络数据，还能够将其他TCP端口的网络数据通过SSH连接进行转发，并且自动提供了相应的加密及解密服务，这一过程被叫做“SSH隧道” (tunneling)。SSH隧道加密传输，两大优势：
- 加密SSH Client 端至SSH Server 端之间的通讯数据
- 突破防火墙的限制完成一些之前无法建立的TCP 连接


## 2. 客户端
### 2.1 安装 ssh
```bash
sudo apt-get install openssh-client

# 查看版本
ssh -V
```




### 2.2 ssh 登陆

#### 2.3.1 基本用法

```bash
# user: 用户名
# hostname: ip地址或域名
# -p 6000: 登陆6000端口, 默认22

ssh -p 6000 user@hostname
```

第一次登陆会有提示信息，需要输入`yes`，之后该服务器的信息会保存在`~/.ssh/known_hosts`文件中。



####  2.3.2 别名登陆

**配置文件**

创建文件`~/.ssh/config`, 输入:

```bash
Host myserver1
	HostName IP地址或域名
	User 用户名
	Port 端口号
	
	# 设置 ssh socks5 代理
	# 使用 nc 命令, 其参数 -X: 指定代理协议; -x 指定代理服务器[ip:port]
	ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p
```

后续可以直接以`ssh myserver1`命令登陆



#### 2.3.3 免密登陆

- ssh-keygen
- ssh-copy-id user@hostname

```bash
# 生成密钥，-t：指定dsa加密算法，默认rsa
# 公钥：~/.ssh/id_rsa.pub; 私钥：~/.ssh/id_rsa
ssh-keygen -t dsa

# 为保证安全，可修改权限
chmod 600 ~/.ssh/id_rsa
chmod 600 ~/.ssh/id_rsa.pub

# 手动上传公钥
# 用户公钥需保存在服务端登陆用户主目录的~/.ssh/authorized_keys文件中
# authorized_keys文件的权限要设为644
cat ~/.ssh/id_rsa.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# 自动上传公钥
ssh-copy-id myserver
```



#### 2.3.4 配置文件

SSH 客户端的全局配置文件是`etc/ssh/ssh_config`，用户个人的配置文件在`~/.ssh/config`，优先级高于全局配置文件。

除了配置文件，`~/.ssh`目录还有一些用户个人的密钥文件和其他文件。下面是其中一些常见的文件。

- `~/.ssh/id_ecdsa`：用户的 ECDSA 私钥。
- `~/.ssh/id_ecdsa.pub`：用户的 ECDSA 公钥。
- `~/.ssh/id_rsa`：用于 SSH 协议版本2 的 RSA 私钥。
- `~/.ssh/id_rsa.pub`：用于SSH 协议版本2 的 RSA 公钥。
- `~/.ssh/known_hosts`：包含 SSH 服务器的公钥指纹。

### 2.3 参数详解

| 参数                                             | 作用                                             |
| ------------------------------------------------ | ------------------------------------------------ |
| -C                                               | 压缩数据，加快传输速度                           |
| -p \<port\>                                      | 指定远程ssh服务端口port，默认22                  |
| -L \<listen_port\>:<target_host>:\<port\>        | 将本地的 listen_port 转发到远程机器地址上的 port |
| -R <remote_listen_port>:\<target_host\>:\<port\> | 将远程机器上的 listen_port 转发到本地上的 port   |
| -D \<port\>                                      | 指定一个本地机器 “动态的’’ 应用程序端口转发.     |
| -X                                               | 打开X11窗口转发                                  |
| -4,-6                                            | 指定协议，默认为IPv4                             |
| -v,-vv,-vvv                                      | 显示详细信息，v越多越详细                        |
| -N                                               | 只进行端口转发，不执行远程命令                   |
| -f                                               | 后台执行ssh指令                                  |
| -c                                               | 选择加密算法，默认3des                           |
| -o                                               | 制定配置选项                                     |

### 2.4 scp传文件

- `scp source destination`
- `scp source1 source2 destination`
- `scp -r source_dir destination`

```bash
# 复制source到服务器user@hostname家目录
scp source user@hostname:

# 使用scp配置其他服务器的 vim 和 tmux
scp ~/.vimrc ~/.tmux.conf user@hostname:
```

提一下Xshell 中传文件的命令: rz, sz

## 3. 服务端

### 3.1 安装 sshd
```bash
# install
sudo apt-get install openssh-server

# 启动
sudo systemctl start sshd.service
# 重启
sudo systemctl restart sshd.service
# 开机自启
sudo systemctl enable sshd.service
```

### 3.2 配置文件

sshd 的配置文件在`/etc/ssh`目录，主配置文件是`sshd_config`，此外还有一些安装时生成的密钥。

- `/etc/ssh/sshd_config`：配置文件
- `/etc/ssh/ssh_host_ecdsa_key`：ECDSA 私钥。
- `/etc/ssh/ssh_host_ecdsa_key.pub`：ECDSA 公钥。
- `/etc/ssh/ssh_host_rsa_key`：用于 SSH 2 协议版本的 RSA 私钥。
- `/etc/ssh/ssh_host_rsa_key.pub`：用于 SSH 2 协议版本的 RSA 公钥。
- `/etc/pam.d/sshd`：PAM 配置文件。

注意，如果重装 sshd，上面这些密钥都会重新生成，导致客户端重新连接 ssh 服务器时，会跳出警告，拒绝连接。为了避免这种情况，可以在重装 sshd 时，先备份`/etc/ssh`目录，重装后再恢复这个目录。



## 4. 端口转发

端口转发（port forwarding），又称 SSH 隧道（tunnel）。它有两个主要作用：

（1）将不加密的数据放在 SSH 安全连接里面传输，使得原本不安全的网络服务增加了安全性，比如通过端口转发访问 Telnet、FTP 等明文服务，数据传输就都会加密。

（2）作为数据通信的加密跳板，绕过网络防火墙。

### 4.1 动态转发

- 指定一个本地机器 “动态的’’ 应用程序端口转发
- socks5协议

`ssh -D <local_port> tunnel_host `



### 4.2 本地转发

- 将本地的 listen_port 转发到远程机器地址上的 port

`ssh -L <listen_port>:<target_host>:<port> user@hostname`



### 4.3 远程转发

- 将远程机器上的 remote_listen_port 转发到本地上的 port

`ssh -R <remote_listen_port>:<target_host>:<port> user@hostname`



### 4.4 autossh

autossh是一个用来启动 ssh 并进行监控的程序，可在需要时重启 ssh，如果程序问题或者是网络问题。
- 基本命令: `autossh -M <port> user@hostname`
```bash
# 安装
sudo apt-get install autossh

# 本地转发,断开重连(-M <port>)
autossh -M 4444:www.example.com:4444 user:hostname
```



## 5. 双重加密

利用”ssh隧道+rc4双重加密”去连接目标内网下指定机器上的meterpreter，让payload变的更加难以追踪。
[tencent_cloud_blog](https://cloud.tencent.com/developer/article/1584744)



## 参考
[ssh_tutorial](https://wangdoc.com/ssh/)

[ssh_tips](https://plantegg.github.io/2019/06/02/%E5%8F%B2%E4%B8%8A%E6%9C%80%E5%85%A8%20SSH%20%E6%9A%97%E9%BB%91%E6%8A%80%E5%B7%A7%E8%AF%A6%E8%A7%A3--%E6%94%B6%E8%97%8F%E4%BF%9D%E5%B9%B3%E5%AE%89/)