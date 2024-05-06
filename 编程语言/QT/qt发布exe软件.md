

- QT Creat 调整成release模式，清除构建，重新构建，编译一次
- 将当前编译器的bin目录添加到环境变量中，这样后面使用windeployqt.exe才方便
- D:\Qt6\6.2.4\mingw_64\bin                这个是使用的编译器的目录，在QT安装目录上
- 如果有安装了其他版本的QT，请从环境变量中删除其他的QT版本
- 到你的工程目录（D:\code\qt\build-tools_cusid-Desktop_Qt_6_2_4_MinGW_64_bit-Release）执行 windeployqt.exe xxx.exe xxx是你的项目
- 在使用windeployqt.exe工具的时候，[命令行](https://so.csdn.net/so/search?q=命令行&spm=1001.2101.3001.7020)环境需要注意，不能直接使用cmd环境。需要使用qt的cmd环境：
- 得到的是一个 带有dll 的文件夹，在这文件夹下才能运行 对应exe文件
- 将外用的库，复制到这个文件夹下
- 打包工具（Enigma Virtual Box），将dll文件和exe文件打包成一个exe文件
- Process Explorer软件来看缺少了那些dll

```
D:\Qt6\6.2.4\mingw_64\bin\windeployqt.exe --release --no-quick-import --no-translations --no-virtualkeyboard --no-compiler-runtime tools_cusid.exe
```

windeployqt参数：

| --release                     | 发布Release版本的二进制文件                  |
| ----------------------------- | -------------------------------------------- |
| --no-quick-import             | 忽略Qt Quick 的相关库                        |
| --no-translations <languages> | 需要发布的语言列表，有逗号分隔，如（en，fr） |
| --no-virtualkeyboard          | 忽略虚拟键盘的相关文件                       |
| --no-compiler-runtime         | 忽略编译器的运行时文件                       |
| --no-opengl-sw                | 忽略OpenGL软件的渲染                         |
| --no-system-d3d-compiler      | 忽略D3D编译器                                |
|                               |                                              |
|                               |                                              |

