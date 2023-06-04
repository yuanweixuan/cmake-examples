# 目录
## 01-basic
    A:直接将cpp编译成可执行文件
    B:指定.h所在的路径，将cpp编译成可执行文件
    C：编译静态库，并且链接该静态库
    D：编译动态库并重命名，链接该动态库
    E：安装文件
    F：设置编译类型是debug还是release，默认是debug。debug更安全，release体积和性能更优
    G：添加编译时选项参数，编译二进制时添加宏定义变量(target_compile_definitions)
    H：find_package调用第三方库，<package>_FOUND 判断是否找到库。然后就可以link其中的库
    I：添加编译器选项，比如使用clang   cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6
# 工程中常出现的变量
    ${PROJECT_SOURCE_DIR}：当前cmake工程的根cmakelist所在目录
    ${CMAKE_INSTALL_PREFIX}：
        install时安装路径的前缀，默认是/usr/local，可以在ccmake中指定或者`cmake .. -DCMAKE_INSTALL_PREFIX=/install/location`
    cmake .. -DCMAKE_C_COMPILER=clang-3.6 -DCMAKE_CXX_COMPILER=clang++-3.6



# 常用命令 
install
    删除安装的文件 `sudo xargs rm < install_manifest.txt`
    **删除前仔细检查 install_manifest.txt 中的文件**
    
    install(TARGETS my_exe RUNTIME DESTINATION /usr/bin )

*   `TARGETS`: 指定要安装的目标文件，可以是可执行文件、静态库、共享库等。

*   `RUNTIME DESTINATION`: 指定可执行文件的安装目录。

*   `LIBRARY DESTINATION`: 指定共享库的安装目录。

*   `ARCHIVE DESTINATION`: 指定静态库的安装目录。

*   `INCLUDES DESTINATION`: 指定头文件的安装目录。

    ```
    install(DIRECTORY include/ DESTINATION /usr/include)

    # 将指定目录中所有文件，安装到目标目录中

target_compile_definitions
    为指定的目标（如可执行文件、动态库、静态库）添加编译时宏定义


    target_compile_definitions(target
                           [PRIVATE | PUBLIC | INTERFACE] 
                           [definition1 [definition2 ...]])

`target`表示要添加编译宏定义的目标名称；`PRIVATE`、`PUBLIC`和`INTERFACE`是可选参数，表示宏定义的可见性，缺省值为`PUBLIC`；`definition1 [definition2 ...]`表示需要添加的宏名称和对应的值（可选）
*   如果使用`PRIVATE`，那么宏定义只在相同目标的其他源文件中可见，而不会影响到有依赖它的其他目标；

*   如果使用`PUBLIC`，则该宏定义会在目标本身和依赖它的目标中可见；

*   如果使用`INTERFACE`，则宏定义只在依赖它的目标中可见，自身目标不会包含该宏定义。
以下代码将在目标`myexe`中添加一个名为`VERSION_NUM`的宏定义，其值为`123`：

```
add_executable(myexe main.cpp)
target_compile_definitions(myexe PRIVATE VERSION_NUM=123)

```

这样，在编译`myexe`过程中，编译器会将`VERSION_NUM`宏定义的值替换为`123`，从而影响程序的编译结果。通常，宏定义用于实现条件编译、调试信息输出等相关操作。
