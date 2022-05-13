### 一. conda 相关命令

#### 1.镜像源

conda下载包是通过一些chanel来访问下载的，原本内置的有一些chanel，另外一些包需要自己添加下载所需的chanel。

**查看chanel：**

```
conda config --show
```

**添加chanel：**

```
#清华源
1. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
2. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
3. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
4. conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/fastai/
5. conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
6. conda config --append channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/


```

**删除chanel：**

```
conda config –remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```



#### 2.包管理

**conda搜索某个包及可安装的版本信息（以scikit-learn包为例）：**

```
conda list                    #查看目前已安装的包
conda list -n shengkai@py38   #查看指定环境已安装的包


conda search scikit-learn     #查看scikit-learn是否可安装
```

**conda安装指定版本的包：**

```
conda install scikit-learn=0.23.1
conda install cudatoolkit=10.1 #(安装英伟达的SDK10.1版本)
conda install cudnn=7.6 #(安装英伟达深度学习软件包7.6版本)

#如果不用-n指定环境名称，则被安装在当前的活跃状态：
conda install -n shengkai@py38  scikit-learn  
```

**conda卸载包：**

```
conda uninstall scikit-learn
conda remove scikit-learn
```

**conda升级包：**

```
conda update scikit-learn
conda update conda
conda update python
conda update anaconda
```



#### 3.conda缓存清理：

```
#conda报错segment fault的时候就是需要清理缓存哦
conda clean -p      //删除没有用的包



conda clean -t      //删除tar包



conda clean -y --all //删除所有的安装包及cache
```



#### 4.环境管理

```
conda create -n myenv python=3.5    #创建python3.5名字叫myenv虚拟环境



conda activate myenv                #开启myenv环境



conda deactivate                    #关闭环境


conda info --envs
conda env list                      #显示所有的虚拟环境



conda remove --name myenv --all     #删除创建的myenv环境
```

#### 5.官方指导

[conda官网](https://conda.io/projects/conda/en/latest/user-guide/concepts/packages.html)





