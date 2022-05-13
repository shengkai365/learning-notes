### 九、docker教程

#### 1. 将当前用户添加到`docker`用户组

为了避免每次使用`docker`命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的`docker`用户组(可以参考[官方文档](https://docs.docker.com/engine/install/linux-postinstall/))：

```shell
sudo usermod -aG docker $USER
```

#### 2. 镜像（images）

1. docker pull ubuntu:20.04：拉取一个镜像
2. docker images：列出本地所有镜像
3. docker image rm ubuntu:20.04 或 docker rmi ubuntu:20.04：删除镜像ubuntu:20.04
4. docker [container] commit CONTAINER IMAGE_NAME:TAG：创建某个container的镜像
5. docker save -o ubuntu_20_04.tar ubuntu:20.04：将镜像ubuntu:20.04导出到本地文件ubuntu_20_04.tar中
6. docker load -i ubuntu_20_04.tar：将镜像ubuntu:20.04从本地文件ubuntu_20_04.tar中加载出来

#### 3. 容器(container)

1. docker [container] create -it ubuntu:20.04：利用镜像ubuntu:20.04创建一个容器。
2. docker ps -a：查看本地的所有容器
3. docker [container] start CONTAINER：启动容器
4. docker [container] stop CONTAINER：停止容器
5. docker [container] restart CONTAINER：重启容器
6. docker [contaienr] run -itd ubuntu:20.04：创建并启动一个容器
7. docker [container] attach CONTAINER：进入容器
   - 先按Ctrl-p，再按Ctrl-q可以挂起容器
8. docker [container] exec CONTAINER COMMAND：在容器中执行命令
9. docker [container] rm CONTAINER：删除容器
10. docker container prune：删除所有已停止的容器
11. docker export -o xxx.tar CONTAINER：将容器CONTAINER导出到本地文件xxx.tar中
12. docker import xxx.tar image_name:tag：将本地文件xxx.tar导入成镜像，并将镜像命名为image_name:tag
13. docker export/import与docker save/load的区别：
    - export/import会丢弃历史记录和元数据信息，仅保存容器当时的快照状态
    - save/load会保存完整记录，体积更大
14. docker top CONTAINER：查看某个容器内的所有进程
15. docker stats：查看所有容器的统计信息，包括CPU、内存、存储、网络等信息
16. docker cp xxx CONTAINER:xxx 或 docker cp CONTAINER:xxx xxx：在本地和容器间复制文件
17. docker rename CONTAINER1 CONTAINER2：重命名容器
18. docker update CONTAINER --memory 500MB：修改容器限制

#### 4. 实战

进入AC Terminal，然后：

```shell
scp /var/lib/acwing/docker/images/docker_lesson_1_0.tar server_name:  # 将镜像上传到自己租的云端服务器
ssh server_name  # 登录自己的云端服务器

docker load -i docker_lesson_1_0.tar  # 将镜像加载到本地
docker run -p 20000:22 --name my_docker_server -itd docker_lesson:1.0  # 创建并运行docker_lesson:1.0镜像

docker attach my_docker_server  # 进入创建的docker容器
passwd  # 设置root密码
```

去云平台控制台中修改安全组配置，放行端口`20000`。

返回AC Terminal，即可通过`ssh`登录自己的`docker`容器：

```shell
ssh root@xxx.xxx.xxx.xxx -p 20000  # 将xxx.xxx.xxx.xxx替换成自己租的服务器的IP地址
```

然后，可以仿照上节课内容，创建工作账户`acs`。

最后，登录配置`docker`容器的别名和免密登录。
如果`apt-get`下载软件速度较慢，可以参考清华大学开源软件镜像站中的内容，修改软件源。





