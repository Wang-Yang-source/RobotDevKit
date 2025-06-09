# RobotDevKit
一个集成了机器人开发过程中常用的通信协议、控制算法的机器人开发工具包，主要面向Linux平台同时也支持stm32f1与stm32f4平台。目前该工具包包含PID增量式与位置式算法的实现、维特智能私有协议（惯性导航）、达妙电机与大疆电机的控制协议、常见编码电机的控制算法、USB串口类、USB转CAN总线功能、简单消息传输协议（传输字符串）、简单二进制传输协议（传输自定义结构体）、可靠二进制传输协议（带丢包超时重传功能）、底盘运动解算（两轮差速、麦克纳姆轮、全向轮）、VOFA可视化服务器。

# 目录说明
cmake 配置文件

docs 文档

modules 模块

platforms 在其他平台上的实现

samples 例程

# 安装方法
1，目标平台为arm32
```bash
cd build
sudo apt-get install libboost-dev:arm
sudo apt-get install gcc-arm-linux-gnueabihf g++-arm-linux-gnueabihf
sudo apt-get install ninja-build
cmake -G Ninja -D BUILD_ARM32=ON -D CMAKE_INSTALL_PREFIX=/usr/arm-linux-gnueabihf -D CMAKE_BUILD_TYPE=RELEASE ..
ninja
ninja install
```
2，目标平台为arm64
```bash
cd build
sudo apt-get install gcc-aarch64-linux-gnu g++-aarch64-linux-gnu
sudo apt-get install libboost-dev:arm64
sudo apt-get install ninja-build
cmake -G Ninja -D BUILD_ARM64=ON -D CMAKE_INSTALL_PREFIX=/usr/aarch64-linux-gnu -D CMAKE_BUILD_TYPE=RELEASE ..
ninja
ninja install
```
3，目标平台为x86
```bash
cd build
sudo apt-get install libboost-dev
sudo apt-get install ninja-build
cmake -G Ninja -D CMAKE_BUILD_TYPE=RELEASE ..
ninja
ninja install
```

# 使用方法
1，交叉编译arm32程序
```cmake
cmake_minimum_required(VERSION 3.10)

#必须在project()前面
set(CMAKE_TOOLCHAIN_FILE "cmake/arm-gnueabi.toolchain.cmake")
set(CMAKE_CXX_STANDARD 20)

project(Demo VERSION 1.0)

find_package(RobotDevKit REQUIRED)
include_directories(${RobotDevKit_INCLUDE_DIRS})

add_executable(main src/main.cpp)
target_link_libraries(main ${RobotDevKit_LIBRARIES})
```

2，交叉编译arm64程序
```cmake
cmake_minimum_required(VERSION 3.10)

#必须在project()前面
set(CMAKE_TOOLCHAIN_FILE "cmake/aarch64-gnu.toolchain.cmake")
set(CMAKE_CXX_STANDARD 20)

project(Demo VERSION 1.0)

find_package(RobotDevKit REQUIRED)
include_directories(${RobotDevKit_INCLUDE_DIRS})

add_executable(main src/main.cpp)
target_link_libraries(main ${RobotDevKit_LIBRARIES})
```

3，直接在本地平台编译程序
```cmake
cmake_minimum_required(VERSION 3.10)

set(CMAKE_CXX_STANDARD 20)

project(Demo VERSION 1.0)

find_package(RobotDevKit REQUIRED)
include_directories(${RobotDevKit_INCLUDE_DIRS})

add_executable(main src/main.cpp)
target_link_libraries(main ${RobotDevKit_LIBRARIES})
```

# 使用 Meson 构建和安装

1. 本地编译（x86 平台）：
```bash
sudo apt-get install meson ninja-build libboost-dev libopencv-dev
meson setup builddir
meson compile -C builddir
sudo meson install -C builddir
```

2. 交叉编译（arm32/arm64）：
- 需准备对应的 cross file（如 `cross_arm32.txt` 或 `cross_arm64.txt`），内容指定交叉编译器和 sysroot。
- 以 arm32 为例：
```bash
meson setup builddir --cross-file cross_arm32.txt
meson compile -C builddir
sudo meson install -C builddir
```
- 以 arm64 为例：
```bash
meson setup builddir --cross-file cross_arm64.txt
meson compile -C builddir
sudo meson install -C builddir
```

3. cross file 示例（以 arm32 为例）：
```ini
[binaries]
c = '/usr/bin/arm-linux-gnueabihf-gcc'
cpp = '/usr/bin/arm-linux-gnueabihf-g++'
ar = '/usr/bin/arm-linux-gnueabihf-ar'
strip = '/usr/bin/arm-linux-gnueabihf-strip'
pkgconfig = '/usr/bin/pkg-config'

[host_machine]
system = 'linux'
cpu_family = 'arm'
cpu = 'armv7'
endian = 'little'
```

4. 说明：
- Meson 依赖通过 pkg-config 查找，需确保交叉环境下依赖库可用。
- 更多 Meson 用法详见 https://mesonbuild.com/