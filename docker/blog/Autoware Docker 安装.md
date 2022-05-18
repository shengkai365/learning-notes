## Autoware Docker 安装

### 1. Ubuntu20.04 Docker 官方教程安装

#### 1.1 完全卸载docker

[Uninstall Docker Engine](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

```bash
# 查看需要卸载的相关package
dpkg -l | grep docker

# 卸载上面列出的package
sudo apt-get purge docker-ce docker-ce-cli containerd.io [...]

# 删除docker相关镜像、容器、volumes
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

# 确认docker卸载完毕
docker --version
```



#### 1.2 安装docker（使用repository）

[Docker官方安装教程](https://docs.docker.com/engine/install/ubuntu/)

```bash
# 安装相关依赖包
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
# 添加docker官方GPG密钥
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
 
# 使用以下命令设置stable存储库（stable/nightly/test)
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
#  安装Docker引擎
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# 验证是否正确安装了Docker
sudo docker --version
```



#### 1.3 将当前用户添加到`docker`用户组

[Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

```bash
# 创建 docker 组
sudo groupadd docker

# 将你的用户添加到docker组
sudo usermod -aG docker $USER

# 注销并重新登录，以便重新评估您的组成员资格
newgrp docker

# 验证
docker --version
```



### 2. 安装 nvidia-container-runtime

[Access an NVIDIA GPU](https://docs.docker.com/engine/reference/commandline/run/#access-an-nvidia-gpu)

[官方参考](https://nvidia.github.io/nvidia-container-runtime/)

```bash
# 设置stable仓库和GPG密钥
curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list

# 更新package后安装 nvidia-docker2
sudo apt-get update
sudo apt-get install nvidia-container-runtime

# 重启Docker守护进程
sudo systemctl restart docker

# 测试, 出现显卡信息（nvidia-smi）界面表示成功
sudo docker run --rm --gpus all ubuntu:20.04 nvidia-smi
```



### 3.  安装Autoware

#### 3.1 安装预先构建的源代码的AutoWare Docker容器

[Autoware参考](https://github.com/Autoware-AI/autoware.ai/wiki/Generic-x86-Docker)

```bash
# 克隆docker仓库并进入generic文件夹
git clone https://github.com/Autoware-AI/docker.git
cd docker/generic

# 使用 run.sh 脚本创建并进入Docker容器
./run.sh

```

