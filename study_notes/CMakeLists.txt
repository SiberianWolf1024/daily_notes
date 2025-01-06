# 设置最低 CMake 版本要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(MyProject)

# 设置默认的编译方式为 Debug
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the build type" FORCE)
endif()

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 指定头文件搜索路径（包含目录）
include_directories(
    ${PROJECT_SOURCE_DIR}/include        # 项目中 'include' 目录下的头文件
    #/path/to/other/include               # 其它第三方库的头文件目录（例如 OpenSSL、Boost 等）
)

# 指定库文件搜索路径（库目录）
link_directories(
    ${PROJECT_SOURCE_DIR}/lib           # 项目中的 'lib' 目录下的库文件
    #/path/to/other/lib                  # 其它第三方库的目录
)

# 设置可执行文件的输出目录 
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE})

# 查找 src 目录下的所有 .cpp 文件，将这些文件路径存储到变量 SOURCES 中
#file(GLOB SOURCES "src/*.cpp")

# 添加可执行文件的源代码文件
add_executable(demo
               #${SOURCES}
               main.cpp)

# 链接库文件（假设我们需要链接 'libexample.so' 或 'example.lib'）
target_link_libraries(demo
    #example    # 这是库文件的名字，不需要加文件后缀
)