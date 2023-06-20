## Ubuntu安装中文man手册

> [在线查阅命令](https://manpages.debian.org/unstable/manpages-zh/index.html)
>
> [release下载](https://src.fedoraproject.org/repo/pkgs/man-pages-zh-CN/)

```bash
# 1. 安装依赖
sudo apt-get install autoconf
sudo apt-get install automake
sudo apt-get install opencc

# 2. 查看release地址并下载最新压缩包
wget https://src.fedoraproject.org/repo/pkgs/man-pages-zh-CN/v1.6.3.6.tar.gz/sha512/dc9ecd461eba41fc30658e028f853e3664fc6ce27c5b48c3159c5c8a452ad6d71730e0e5f551efa7b4c358baf010ba27a855457ae69b21e9637af326044dcca8/v1.6.3.6.tar.gz

tar -zxvf v1.6.3.4.tar.gz


# 3. 配置指定安装目录 /usr/local/zhman
cd manpages-zh-1.6.3.6/
autoreconf --install --force
./configure --disable-zhtw  --prefix=/usr/local/zhman

# 4. 编译安装
sudo make
sudo make install

# 5. 配置别名
echo "alias cman='man -M /usr/local/zhman/share/man/zh_CN' " >> ~/.bashrc

# 6. 重载配置文件并运行cman
source ~/.bashrc
cman echo
```

