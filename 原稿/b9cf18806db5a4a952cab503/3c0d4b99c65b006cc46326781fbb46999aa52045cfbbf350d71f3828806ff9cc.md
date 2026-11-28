`Vscode`负责自动补全，`CMake`负责实际的编译。

`Vscode`依赖MS自己开发的插件进行自动补全，所以没啥逻辑可循，直接按部就班即可：

1. 按下`F1`，选择`C/C++:编辑配置`，vscode会生成一个`.vscode`文件，内部会有一个`c_cpp_properties.json`文件
2. 根据你的操作系统和使用的C++编译器编辑，下面是一个案例：

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [	// include路径，后续include的自动补全都根据这个列表的路径去搜索
                "${workspaceFolder}/**",
                "/usr/local/lib/libtorch/**",
                "/usr/local/include/opencv4/**"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",				// 编译器路径
            "cStandard": "gnu11",						// C标准
            "cppStandard": "gnu++14",					// C++标准
            "intelliSenseMode": "linux-gcc-x64"			// 自动补全模型
        }
    ],
    "version": 4
}
```

如此一来，你就可以在工作目录下创建cpp源文件，并且享受到`include`时的自动补全和后续外部库的自动补全。



`CMake`还是那样，对于应用级别的项目，简单的设置如下：

```cmake
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(sr_cpp_inference)

# include的路径 有的库在cmake.config中帮你引入了，所以不需要写
include_directories("/usr/local/include/opencv4")

# 项目用到的包，这些包都是按照cmake的规范来写的
find_package(
    Torch REQUIRED
    NO_MODULE
    PATHS "/usr/local/lib/libtorch"
    NO_DEFAULT_PATH
)

find_package(
    OpenCV REQUIRED
    PATHS "/usr/local/lib/opencv4"
)

# 编译目标
add_executable(inference inference.cpp)

# 连接静态库，cmake貌似会帮你选择子集？
target_link_libraries(inference ${TORCH_LIBRARIES} ${OpenCV_LIBS})

# 编译选项，libtorch强制需要C++14标准
set_property(TARGET inference PROPERTY CXX_STANDARD 14)
```

编译指令，一般为了美观，习惯性这么做：

```bash
$mkdir build && cd build
$cmake ..
$make -j 8 # 8核
```