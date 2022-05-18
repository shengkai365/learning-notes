### CMake 教程

**CMake** 是一个跨平台的、开源的构建工具。`cmake` 是 `makefile` 的上层工具，它们的目的正是为了产生可移植的makefile，并简化自己动手写makefile时的巨大工作量.

#### 1. Linux下使用cmake的流程

- 编写配置文件 CMakeLists.txt
- 执行`cmake PATH`或`ccmake PATH`生成Makefile
- `make` 编译

#### 2. CMake 项目文件目录

```
├── CMakeLists.txt
├── include
│        └── Hello.h
├── src
│        ├── Hello.cpp
│        └── main.cpp
└── build
```

#### 3. CMake Sample

```cmake
# 指明对cmake的最低(高)版本的要求
cmake_minimum_required(VERSION 2.6) 

# 创建项目
project (ProjectName)

# 查找依赖包，如果找到PACK库就把头文件路径和库文件路径赋值给下面
# 两个语句中的 ${PACK_INCLUDE_DIRS}、${PACK_LIBRARIES}。
find_package(PACK REQUIRED)
include_directorise(${PACK_INCLUDE_DIRS})
link_directorise(${PACK_LIBARAY_DIRS})
add_definition(${PACK_definition})

# 创建源文件变量SOURCES，并在可执行程序中添加源文件
set(SOURCES src/Hello.cpp src/main.cpp) 
add_executable(ProjectName ${SOURCES})

# 设置要包含的头文件的目录、设置要链接的库
target_include_directories(ProjectName PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libaries(ProjectName ${PACK_LIBARIES})

# 指定在安装时运行的规则
install(TARGET ProjectName RUNTIME DESTINATION bin)
```

#### 4. 生成可执行文件

```bash
cd build         #外部编译
cmake ..         #生成Makefile文件
make             #生成可执行文件
./ProjectName    #执行可执行文件
```

