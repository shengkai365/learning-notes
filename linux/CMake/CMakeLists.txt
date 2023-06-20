cmake_minimum_required(VERSION 3.5)
project(sayhello)

# 设置编译模式，默认为 Release 模式
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

# 设置C++版本
set (CMAKE_CXX_STANDARD 14)
set (CMAKE_CXX_STANDARD_REQUIRED ON)
set (CMAKE_CXX_EXTENSIONS OFF)

# 设置库文件输出目录
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# 设置可执行文件输出目录
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

# 添加头文件搜索路径
include_directories(${PROJECT_SOURCE_DIR}/include)

# 添加动态库搜索路径
link_directories(${PROJECT_SOURCE_DIR}/lib)

# 编译 libhello.so 库
set(SRC_LIST src/hello.cpp)
add_library(hello SHARED ${SRC_LIST})

# 编译可执行文件
set(APP_SRC_LIST src/useHello.cpp)
add_executable(sayhello ${APP_SRC_LIST})
target_link_libraries(sayhello hello)

# 安装规则
install(FILES lib/libhello.so DESTINATION /usr/local/lib)
install(FILES include/hello.h DESTINATION /usr/local/include)
