### Linux 解压和压缩命令

#### .zip

```bash
# 解压
unzip FileName.zip

# 将DirName本身压缩
zip FileName.zip DirName

# 将文件夹递归压缩
zip -r FileName.zip DirName
```



#### .tar（仅打包，并非压缩）

```bash
# 解包
tar -xvf FileName.tar

# 将DirName和其所有文件（夹）打包
tar -cvf FileName.tar DirName
```



#### .tar.gz（或.tgz）

```bash
# 解压
tar -zxvf FileName.tar.gz

# 解压到目标路径
tar -C DesDirName -zxvf FileName.tar.gz

# 将DirName和其下所有文件（夹）压缩
tar -zxvf FileName.tar.gz DirName
```



#### .gz

```bash
# 解压1
gunzip FileName.gz

# 解压2
gzip -d FileName.gz
```



#### .rar

```bash
# 解压
rar x FileName.rar

# 压缩
rar a FileName.rar DirName
```

