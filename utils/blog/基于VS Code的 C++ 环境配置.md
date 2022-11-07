### 基于VS Code的C++环境配置

#### 1. 安装MinGW-w64编译器套件

##### 1.1 下载

- [百度网盘](https://pan.baidu.com/s/1DqSmuxi1z_2VqU_3yB4bTQ ) 提取码：3dz1 
- [SourceForge托管地址](https://sourceforge.net/projects/mingw-w64/files/) 
- **64位Windows平台推荐：x_86_64-win32-seh**
- win32和posix代表线程模型，seh、sjlj、dwarf代表不同的异常处理模式，理论上都可以随便选。

##### 1.2 解压

- [下载解压软件](https://pan.baidu.com/s/1aMOVgvviFxUj50neO3jSPw )  提取码：21bu ，解压到同名目录
- 将目录文件剪切到路径：`C:\Program Files\`
- 例如我的是：`C:\Program Files\mingw-w64`

##### 1.3 加入环境变量

- 将路径 `C:\Program Files\mingw-w64\x86_64-8.1.0-release-win32-seh-rt_v6-rev0\mingw64\bin` 加入环境变量
- 不会的自行百度

##### 1.4 测试

- `Win+r` 输入 cmd，进入命令窗口
- 输入：`gcc -version` 出现版本号表示安装成功

#### 2. 安装VS Code文本编辑器

- 下载并安装 [官方地址](https://code.visualstudio.com/) 
- 应用商店安装：Chinese、C/C++ 
- 重启VS Code

#### 3. 配置C/C++环境

- 以文件夹的方式打开cpp文件，因为还需要保存.vscode 的配置文件。
- 打开命令面板`【Ctrl】+【Shift】+【P】` ，输入：`C/C++` ，选择**编辑配置（UI）**。
- 配置【编译器路径】和【IntelliSense模式】，**编写C程序请选择gcc.exe，C++则选择g++.exe**；模式选择**gcc-x64**。
- 可在 `.vscode\c_cpp_properties.json` 文件中查看是否配置好。

#### 4. 编译执行

- 创建 `hello.cpp` 文件，写入代码，保存。
- 第一次执行编译任务前，需要配置这个任务。选择菜单栏【终端】→【配置任务…】
- 在弹出的待选项中选择"`C/C++: gcc.exe build active file`"（如果是C++则应是"`g++.exe build active file`"）
- 这时一个`tasks.json`文件将被自动创建并保存在`.vscode`中
- 为了执行这个编译任务，**先打开要编译的代码文件**，然后选择菜单栏【终端】→【运行生成任务】（快捷键Ctrl+Shift+B）
- 在弹出的待选项中选择刚刚配置的任务名，如：`g++.exe build active file` 
- 执行：`.\hello.exe`

#### 5. 调试

- 选择菜单栏【运行】【添加配置…】
- 选择第一项"C++ (GDB/LLDB)"（Windows那个是给MSVC编译器用的，MingGW需要使用GDB）
- 接下来选择默认的"gcc.exe - 生成和调试活动文件"（或"g++.exe - 生成和调试活动文件"，具体取决于之前编译器路径的配置）
- F5

#### 6. 参考

[1] [基于 VS Code + MinGW-w64 的C语言/C++简单环境配置，专致小白](https://zhuanlan.zhihu.com/p/77074009)

[2] [VS Code之C/C++程序的调试(Debug)功能简介](https://zhuanlan.zhihu.com/p/85273055)