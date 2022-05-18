### Ubuntu 安装显卡驱动

1.方法一：添加；软件源

```bash
# 添加软件源、更新
sudo add-apt-repository ppa:graphics-drivers/ppa  
sudo apt-get update

# 查看可以使用的驱动版本
ubuntu-drivers devices

# 安装推荐版本驱动
sudo ubuntu-drivers autoinstall

# 安装指定版本驱动
sudo apt install nvidia-driver-495 - distro non-free
```

2.方法二：附加驱动安装
软件和更新 -> 附加驱动 -> 选择版本 -> 应用更改

![](https://img-blog.csdnimg.cn/2020051418093848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RoYW5sb24=,size_16,color_FFFFFF,t_70)



![](https://img-blog.csdnimg.cn/20200514181700919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RoYW5sb24=,size_16,color_FFFFFF,t_70)

**有可能会失败，需要禁用nouveau**

```bash
# 编辑
sudo vim /etc/modprobe.d/blacklist.conf

# 文件末尾加入
blacklist nouveau
options nouveau modeset=0

# 更新系统
sudo update-initramfs -u

# 重启系统
reboot

# 验证是否禁用, 无信息表示已禁用
lsmod | grep nouveau
```


3.重启

```bash
reboot    
```


4.检查

```
nvidia-smi
```
