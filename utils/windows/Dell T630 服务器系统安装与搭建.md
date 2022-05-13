### Windows server 2019 服务器搭建步骤

#### 1. windows server 2019 安装

1. 先在百度云盘下载 iso 文件，[链接](https://pan.baidu.com/s/1hXJhoQsUXy1_xlB-UJNWBQ)； 提取码: ecyn 

2. 找一张空U盘，格式化（NTFS）

3. 安装刻录软件UltraISO，制作U盘[启动盘](http://www.upantool.com/jiaocheng/boot/4221.html)。**先“打开文件”，然后“启动”-“写入硬盘映像”**

   ![img](https://img2018.cnblogs.com/i-beta/476776/202001/476776-20200116165111354-1702819976.png)

   

   ![img](https://img2018.cnblogs.com/i-beta/476776/202001/476776-20200116172548616-1357155797.png)

4. 制作好的U盘插入服务器，开机自检时，根据屏幕右上角提示按  F11进入Boot Manager设置，选择 Bios Boot Menu，选择U盘，按 F5 将它移到第一位，保存并退出重启即可进入安装

5. 安装windows [参考](https://www.jb51.net/article/148806.htm) 

#### 2. 服务器配置

1. 先启用远程功能 

​     右键点击【此电脑】--【属性】，进入【控制面板\系统和安全\系统】，点击【允许远程访问】。

2. 在【远程】下方，点击【允许远程连接到此计算机】，还有去掉下方【仅允许运行使用网络级别身份验证的远程桌面的计算机连接】

3. 在运行（快捷键win+r）中运行 gpedit.msc 命令，调出【本地组策略编辑器】

   依次点击展开【计算机配置】>>>【管理模板】>>>【Windows 组件】>>>【远程桌面服务】>>>【远程桌面会话主机】>>>【连接】”

4. 在连接项右侧找到并双击打开【将远程桌面服务的用户限制到单独的远程桌面会话】

   当前状态为未配置，选中【已禁用】，点击确定 

5. 在连接项右侧找到限制连接的数量。并启用，设置最大用户数目

   

现在可以测试客户端是否可以远程登陆此服务器。但是你会发现，只能单用户登陆

备注：如果无法访问，关闭防火墙再尝试。

[图文参考](https://blog.csdn.net/weixin_40816738/article/details/104712602)

#### 3. 客户端连接

1. 打开【远程桌面连接】>>>选项卡【体验】>>>选择【高速宽带（2Mbps-10Mbps）】
2. 选项卡【显示】>>>【显示配置】划到最大
3. 选项卡【常规】>>> 【计算机】填服务器 IP 地址 >>>【用户名】服务器设备名称+用户，如：shengkai\Administrator
4. 点击【连接】>>>【输入你的凭据】填些服务器用户登录密码

#### 4. 解决多用户登陆问题

参考：

https://www.cnblogs.com/laosan007/p/11734283.html

https://blog.csdn.net/weixin_40816738/article/details/104712602

https://www.jb51.net/article/180453.htm

#### 5. Windows server 2019密钥激活

1. 以管理员身份运行 `Windows PowerShell`

2. 输入以下代码
   - `slmgr /upk` 卸载计算机密钥
   - `slmgr /ipk WMDGN-G9PQG-XVVXX-R3X43-63DFG` 安装计算机密钥
   - `slmgr /skms kms.03k.org` 密钥管理服务计算机名称设置为kms.03k.org
   - `slmgr /ato` 激活指令
   - `slmgr /xpr` 激活查看指令

3. Windows Server 2019操作系统版本 KMS 客户端安装密钥
   - Windows Server 2019 Datacenter WMDGN-G9PQG-XVVXX-R3X43-63DFG
   - Windows Server 2019 Standard N69G4-B89J2-4G8F4-WWYCC-J464C
   - Windows Server 2019 Essentials WVDHN-86M7X-466 P 6-VHXV7-YY726