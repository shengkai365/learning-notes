### Linux无线网卡芯片RTL8188EUS驱动安装

#### 1. 获得芯片类型

无线网卡最重要的就是芯片,外观什么的并不影响系统对其识别,因此在购买网卡时一定要查询其芯片类型.例如对于我购买的comfast CF-WU810N 无线网卡,介绍页面显示其芯片类型为 RTL8188EUS.对于介绍页面没有的,可以咨询客服或者到[wikidevi](https://wikidevi.com/wiki/Main_Page) 网站查询,需要注意的是,网站上网卡信息并不全.

#### 2. 查询内核支持情况

获得芯片类型后,到[passys](http://linux-wless.passys.nl/) 网站查询linux内核对芯片支持情况.对于我的网卡RTL8188EUS,其内核支持情况为Some are supported(页面显示为黄色),说明内核对其有一定的支持.在ubuntu18.04上插上之后,系统可以直接识别,可以接收到wifi信号并连接,但是,无法创建AP,功能收到限制.

#### 3. 寻找更好的驱动

在github上搜索了一下,获得了一些第三方的驱动[lwfinger](https://github.com/lwfinger/rtl8188eu.git) 和 [quickreflex](https://github.com/quickreflex/rtl8188eus.git),对两种驱动都进行了安装,测试发现,lwfinger的驱动仍然无法创建ap, 而quickreflex的驱动则可以完美创建ap(使用[create_ap](https://github.com/oblique/create_ap) ), 问题解决. 另外,也有一些其它的驱动,例如realtek官方提供的驱动(2013年的),其版本太老,无法编译(内核版本不支持).

#### 4. [lwfinger](https://github.com/lwfinger/rtl8188eu) 驱动安装

##### 4.1 依赖

要编译该驱动，你需要安装make和编译器。此外，你还必须[安装内核头文件](https://www.tecmint.com/install-kernel-headers-in-ubuntu-and-debian/)。如果你不明白这意味着什么，请咨询你的发行版。

##### 4.2 下载

```bash
git clone https://github.com/lwfinger/rtl8188eu.git
cd rtl8188eu
```

##### 4.3 编译 & 安装

```bash
make all
make install

# 或需要
insmod 8188eu.ko
ifconfig wlan0 up
```

##### 4.4 重启设备

```bash
reboot
```

