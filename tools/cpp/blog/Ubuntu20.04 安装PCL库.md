# Ubuntu20.04 安装PCL库

### 1. 安装依赖

```bash
sudo apt-get update
sudo apt-get install git build-essential linux-libc-dev
sudo apt-get install cmake cmake-gui
sudo apt-get install libusb-1.0-0-dev libusb-dev libudev-dev
sudo apt-get install mpi-default-dev openmpi-bin openmpi-common
sudo apt-get install libflann1.9 libflann-dev
sudo apt-get install libeigen3-dev
sudo apt-get install libboost-all-dev
sudo apt-get install libqhull* libgtest-dev  
sudo apt-get install freeglut3-dev pkg-config  
sudo apt-get install libxmu-dev libxi-dev   
sudo apt-get install mono-complete   
sudo apt-get install libopenni-dev   
sudo apt-get install libopenni2-dev 
sudo apt-get install libvtk7-dev libvtk6-dev
sudo apt-get install qt5-default
```

显示无法定位包问题时查阅 [Ubuntu Packages](https://packages.ubuntu.com/search?suite=focal&searchon=names&keywords=libvtk)

### 2. 安装PCL

```bash
sudo apt-get install libpcl-dev
```



### 3. 测试

```bash
mkdir test_pcl && cd test_pcl
mkdir build
touch test.cpp
touch CMakeLists.txt
# 写入 test.cpp 和 CMakeLists.txt
cd build
cmake ..
make
./TEST
```

```C++
// test.cpp
#include <iostream>
#include <pcl/common/common_headers.h>
#include <pcl/io/pcd_io.h>
#include <pcl/visualization/pcl_visualizer.h>
#include <pcl/visualization/cloud_viewer.h>
#include <pcl/console/parse.h>
 
 
int main(int argc, char **argv) {
    std::cout << "Test PCL !!!" << std::endl;
    
    pcl::PointCloud<pcl::PointXYZRGB>::Ptr point_cloud_ptr (new pcl::PointCloud<pcl::PointXYZRGB>);
    uint8_t r(255), g(15), b(15);
    for (float z(-1.0); z <= 1.0; z += 0.05)
    {
      for (float angle(0.0); angle <= 360.0; angle += 5.0)
      {
    pcl::PointXYZRGB point;
    point.x = 0.5 * cosf (pcl::deg2rad(angle));
    point.y = sinf (pcl::deg2rad(angle));
    point.z = z;
    uint32_t rgb = (static_cast<uint32_t>(r) << 16 |
        static_cast<uint32_t>(g) << 8 | static_cast<uint32_t>(b));
    point.rgb = *reinterpret_cast<float*>(&rgb);
    point_cloud_ptr->points.push_back (point);
      }
      if (z < 0.0)
      {
    r -= 12;
    g += 12;
      }
      else
      {
    g -= 12;
    b += 12;
      }
    }
    point_cloud_ptr->width = (int) point_cloud_ptr->points.size ();
    point_cloud_ptr->height = 1;
    
    pcl::visualization::CloudViewer viewer ("test");
    viewer.showCloud(point_cloud_ptr);
    while (!viewer.wasStopped()){ };
    return 0;
}
```

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 2.6)
project(TEST)

find_package(PCL 1.2 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable(TEST test.cpp)

target_link_libraries (TEST ${PCL_LIBRARIES})

install(TARGETS TEST RUNTIME DESTINATION bin)
```

