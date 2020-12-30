
# 指定 CMake 最低版本
cmake_minimum_required(VERSION 3.10.2)

# 声明项目并为项目命名.
project("cmake")

# 创建函数库并为函数库命名.
# 函数库类型 : 设置该函数库的类型 ① 静态库 STATIC ② 动态库 SHARED.
# 相对路径 : 指定源码路径, 注意是相对路径.
# 函数库个数据 : 可以定义多个函数库, CMake 会负责构建这些函数库.
# 动态库打包 : Gradle 会自动将动态库打包到 APK 安装包中.

add_library( # 参数 1 : 设置生成的动态库名称.
             native-lib

             # 参数 2 : 设置生成的函数库类型 : ① 静态库 STATIC ② 动态库 SHARED.
             SHARED

             # 参数 3 : 配置要编译的源文件.
             native-lib.cpp )

# 搜索存储 : 搜索指定的预编译库, 并存储该预编译库的路径到变量中, 这里存储到了 log-lib 变量中.
# 指定库名称 : CMake 的搜索路径默认包含了系统库, 只需要指定想添加的公共 NDK 库的名称即可, 这里指定 log 即可.
#            不需要指定 log 库的完整路径 ndk-bundle\platforms\android-29\arch-arm\usr\lib\liblog.so.
# 验证存在性 : 在完成编译之前, CMake 会验证该函数库是否存在.

# 到预设的目录查找 log 库 , 将找到的路径赋值给 log-lib
#   这个路径是 NDK 的 ndk-bundle\platforms\android-29\arch-arm\usr\lib\liblog.so
#   不同的 Android 版本号 和 CPU 架构 需要到对应的目录中查找 , 此处是 29 版本 32 位 ARM 架构的日志库

find_library( # 设置保存函数库路径的变量名称.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              # 指定 CMake 想要定位的 NDK 库名称
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

# 链接函数库
#       参数 1 : 本构建脚本要生成的动态库目 标
#       参数 2 ~ ... : 后面是之前预编译的动态库或静态库 , 或引入的动态库

target_link_libraries( # 指定目标函数库.
                       native-lib

                       # 链接目标库到 NDK 中包含的日志库.
                       ${log-lib} )